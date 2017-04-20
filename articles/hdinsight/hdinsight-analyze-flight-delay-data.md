<properties
    pageTitle="Vlucht vertraging gegevens analyseren met Hadoop in HDInsight | Microsoft Azure"
    description="Informatie over het gebruik van een Windows PowerShell-script te maken van een HDInsight-cluster, voert de taak van een component een Sqoop-up en het cluster te verwijderen."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Vlucht vertraging gegevens analyseren met behulp van de component in HDInsight

Component biedt een manier van Hadoop MapReduce taken uitvoert via een SQL-achtige scripttaal genoemd * [HiveQL][hadoop-hiveql]*, die kan worden toegepast voor het samenvatten en analyseren van grote hoeveelheden gegevens opvragen.

> [AZURE.NOTE] De stappen in dit document moet een cluster van Windows-gebaseerde HDInsight. Die met een Linux-gebaseerde cluster werken Zie [analyseren vlucht vertraging gegevens met behulp van de component in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Een van de belangrijkste voordelen van Azure HDInsight is de scheiding van de opslag van gegevens en berekenen. Azure Blob-opslag HDInsight gebruikt voor gegevensopslag. Een typische taak bestaat uit drie delen:

1. **Gegevens opslaan in Azure Blob-opslag.**  Weerbericht bijvoorbeeld gegevens, sensorgegevens, weblogs, en in dit geval vlucht vertraging gegevens worden opgeslagen in Azure Blob-opslag.
2. **Taken uitvoeren.** Wanneer is het tijd om de gegevens te verwerken, loopt u een Windows PowerShell-script (of een clienttoepassing) een HDInsight-cluster maken en verwijderen van het cluster taken uitvoeren. De taken opslaan uitvoergegevens naar Azure Blob-opslag. De uitvoergegevens blijft behouden zelfs nadat het cluster is verwijderd. Op deze manier, u betaalt alleen wat u hebt verbruikt.
3. **De uitvoer van Azure Blob-opslag te halen**, of in deze zelfstudie kunt u de gegevens exporteren naar een Azure SQL-database.

Het volgende diagram wordt het scenario en de structuur van deze zelfstudie:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Opmerking**: de getallen in het diagram die overeenkomen met de titels van de artikelen. **M** staat voor het hoofdproces. **A** staat voor de inhoud van de bijlage.

Het belangrijkste gedeelte van de zelfstudie wordt beschreven hoe u een Windows PowerShell-script gebruiken om de volgende taken uitvoeren:

- Maak een cluster HDInsight.
- Een component taak uitvoeren op het cluster voor het berekenen van de gemiddelde vertragingen op luchthavens. De vertraging vluchtgegevens wordt opgeslagen in een account Azure Blob-opslag.
- Een taak van de Sqoop om de uitvoer van de taak component exporteren naar een Azure SQL-database uitvoeren.
- De HDInsight cluster verwijderen.

In de bijlagen vindt u de instructies voor het uploaden vertraging vluchtgegevens, een queryreeks component maken/uploaden en de Azure SQL-database voorbereiden voor de taak Sqoop.

> [AZURE.NOTE] De stappen in dit document zijn specifiek voor de HDInsight op basis van Windows-clusters. Die met een Linux-gebaseerde cluster werken Zie [analyseren vertraging vluchtgegevens met behulp van de component in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u de volgende items:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Bestanden die worden gebruikt in deze zelfstudie**

In deze zelfstudie wordt de prestaties op het moment van vluchtgegevens luchtvaartmaatschappij uit [onderzoek en beheer van innovatieve technologie, Bureau Transportation Statistics of RITA] [rita-website]. Een kopie van de gegevens is geüpload naar een Azure Blob storage container met de openbare Blob-machtiging. Een deel van de PowerShell-script kopieert de gegevens uit de openbare blob container aan de standaardcontainer blob van het cluster. Het script HiveQL wordt ook gekopieerd naar dezelfde container Blob. Als u leren hoe get wilt/uploaden van de gegevens op uw eigen account voor de opslag en het maken/HiveQL-scriptbestand uploaden, Zie [bijlage A](#appendix-a) en [Bijlage B](#appendix-b).

De volgende tabel worden de bestanden die in deze zelfstudie wordt gebruikt:

<table border="1">
<tr><th>Bestanden</th><th>Beschrijving</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Het HiveQL script dat wordt gebruikt door de taak van de component. Dit script is geüpload naar een Azure Blob storage rekening met de toegang van het publiek. <a href="#appendix-b">Bijlage B</a> bevat instructies voor het voorbereiden en dit bestand te uploaden naar uw eigen account Azure Blob-opslag.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Gegevens die zijn ingevoerd voor de taak van de component. De gegevens is geüpload naar een Azure Blob storage rekening met de toegang van het publiek. <a href="#appendix-a">Aanhangsel A</a> bevat instructies voor het ophalen van de gegevens en de gegevens uploaden naar uw eigen account Azure Blob-opslag.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Het uitvoerpad voor de taak van de component. De standaardcontainer wordt gebruikt voor het opslaan van de gegevens over de output.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>De component project status-map op de standaardcontainer.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Cluster maken en uitvoeren van taken voor component/Sqoop

Hadoop MapReduce is batch-verwerking. De meest rendabele manier voor het uitvoeren van een taak onderdeel is voor het maken van een cluster voor de taak en taak verwijderen nadat de taak is voltooid. Het volgende script heeft betrekking op het gehele proces. Zie voor meer informatie over het maken van een cluster HDInsight en de uitvoering van taken voor component [maken Hadoop clusters in HDInsight] [ hdinsight-provision] en [Gebruik de component met HDInsight][hdinsight-use-hive].

**De component query's worden uitgevoerd door Azure PowerShell**

1. Azure SQL-database en de tabel voor de uitvoer van de taak Sqoop maken met behulp van de aanwijzingen in [Bijlage C](#appendix-c).
3. Windows PowerShell ISE openen en voer het volgende script:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
        # Create the HDInsight cluster
        $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
        $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
        New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -Location $location `
            -ClusterType Hadoop `
            -OSType Windows `
            -ClusterSizeInNodes 2 `
            -HttpCredential $httpCredential `
            -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Verbinding maken met de SQL-database en vertragingen op de plaats in de tabel AvgDelays gemiddelde vlucht naar:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Bijlage A - Upload vluchtgegevens vertraging naar Azure Blob-opslag
Uploaden van het bestand en de scriptbestanden van HiveQL (Zie [Aanhangsel B](#appendix-b)), moet een planning. Het idee is voor het opslaan van de bestanden en het bestand HiveQL voordat u een cluster HDInsight maken en uitvoeren van de taak van de component. U hebt twee opties:

- **Gebruik dezelfde rekening Azure opslag die wordt gebruikt door het cluster HDInsight als het standaardbestandssysteem.** Omdat het cluster HDInsight de toegangstoets opslag account, hoeft u niet extra wijzigingen aanbrengt.
- **Gebruik een andere account Azure opslag uit het standaardbestandssysteem cluster HDInsight.** Als dit het geval is, wijzigt u de maken deel uit van de Windows PowerShell-script dat is gevonden in de [cluster HDInsight maken en uitvoeren component/Sqoop taken voor](#runjob) de opslag-account als een extra opslag-account te koppelen. Zie voor instructies [maken Hadoop clusters in HDInsight][hdinsight-provision]. De HDInsight cluster kent dan de toegangstoets voor de opslag.

>[AZURE.NOTE] Het Blob storage-pad voor het bestand is hard gecodeerd in het script HiveQL bestand. U moet deze dienovereenkomstig bijwerken.

**De vluchtgegevens downloaden**

1. Ga naar [onderzoek en innovatieve technologie voor beheer, Bureau of Transportation Statistics][rita-website].
2. Selecteer op de pagina de volgende waarden:

    <table border="1">
    <tr><th>Naam</th><th>Waarde</th></tr>
    <tr><td>Jaar filter</td><td>2013 </td></tr>
    <tr><td>Periode filteren</td><td>Januari</td></tr>
    <tr><td>Velden</td><td>*Jaar*, *FlightDate*, *UniqueCarrier*, *vervoerders*, *FlightNum*, *OriginAirportID*, *oorsprong*, *OriginCityName*, *OriginState*, *DestAirportID*, *doel*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (alle andere velden wissen)</td></tr>
    </table>

3. Klik op **downloaden**.
4. Pak het bestand naar de map **C:\Tutorials\FlightDelay\2013Data** . Elk bestand een CSV-bestand is, en ongeveer 60GB groot is.
5.  Op de naam van de maand waarin deze gegevens voor de naam van het bestand. Bijvoorbeeld het bestand met de gegevens van januari de naam *January.csv*.
6. Herhaal stap 2 en 5 voor het downloaden van een bestand voor elk van de twaalf maanden in 2013. U moet minimaal één bestand uit te voeren van de zelfstudie.  

**De vertraging vluchtgegevens uploaden naar Azure Blob-opslag**

1. Voorbereiding van de parameters:

    <table border="1">
    <tr><th>Naam van variabele</th><th>Notities</th></tr>
    <tr><td>$storageAccountName</td><td>De opslag van Azure rekening waar u de gegevens te uploaden.</td></tr>
    <tr><td>$blobContainerName</td><td>De Blob-container waar u de gegevens te uploaden.</td></tr>
    </table>
2. Azure PowerShell ISE openen.
3. Plak het volgende script in het script-veld:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Druk op **F5** om het script uitvoert.

Als u een andere methode gebruiken voor het uploaden van bestanden, Controleer of dat het pad naar het bestand is flightdelay-zelfstudies/gegevens. De syntaxis voor de toegang tot de bestanden is:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Het pad zelfstudies/flightdelay/data is de virtuele map die u gemaakt wanneer u de bestanden hebt geüpload. Controleer of er 12-bestanden, één voor elke maand.

>[AZURE.NOTE] U kunt de component query lezen vanaf de nieuwe locatie moet bijwerken.

> U moet ofwel de container-machtiging openbaar of de account opslag binden aan het cluster HDInsight configureren. Anders de queryreeks component niet mogelijk toegang te krijgen tot de bestanden.

---
##<a id="appendix-b"></a>Bijlage B - maken en een HiveQL script uploaden

Azure PowerShell gebruikt, kunt u meerdere HiveQL instructies één tegelijk uitvoeren of pakket van de HiveQL-instructie in een scriptbestand. In deze sectie wordt beschreven hoe u een script HiveQL maken en het script uploaden naar Azure Blob-opslag met Azure PowerShell. Component vereist de HiveQL scripts in Azure Blob-opslag moeten worden opgeslagen.

Het script HiveQL wordt als volgt te werk:

1. **De tabel delays_raw**, bestaat in geval de tabel al.
2. **Delays_raw externe component-tabel maken** de Blob-opslaglocatie aan te wijzen met de vlucht vertraging bestanden. Deze query geeft aan dat de velden worden gescheiden door "," en dat de regels worden beëindigd door "\n". Dit vormt een probleem wanneer de waarden in het veld komma's bevatten, omdat de component kan niet differentiëren tussen een veldscheidingsteken is een komma en een die deel uitmaakt van de waarde van een veld (dat is het geval in veldwaarden voor oorsprong\_STAD\_naam en het doel\_STAD\_naam). Om dit op te lossen, wordt de query tijdelijk kolommen gegevens bevatten die niet goed in kolommen wordt opgesplitst.  
3. **De tabel vertraging**, bestaat in geval de tabel al.
4. **De tabel vertraging maken**. Het is handig voor het opschonen van de gegevens voor verdere verwerking. Deze query wordt een nieuwe tabel, *vertragingen*, uit de tabel delays_raw. Opmerking de kolommen tijdelijk (zoals eerder vermeld) worden niet gekopieerd en dat de functie **subtekenreeks** aanhalingstekens verwijderen van de gegevens wordt gebruikt.
5. **Het gemiddelde weer vertraging en groepen de resultaten berekenen met de naam van de stad.** Het zal ook de resultaten uitvoeren naar Blob-opslag. Overigens de query, apostrofs wordt verwijderd uit de gegevens en sluit de rijen waarvan de waarde voor **weather_delay** null is. Dit is nodig omdat de Sqoop, die later in deze zelfstudie wordt gebruikt niet netjes die waarden verwerkt standaard.

Zie [Component (Data Definition Language)]voor een volledige lijst van de opdrachten HiveQL[hadoop-hiveql]. Elke opdracht HiveQL moet worden beëindigd met een puntkomma.

**Voor het maken van een scriptbestand HiveQL**

1. Voorbereiding van de parameters:

    <table border="1">
    <tr><th>Naam van variabele</th><th>Notities</th></tr>
    <tr><td>$storageAccountName</td><td>De opslag van Azure rekening waar u het script HiveQL te uploaden.</td></tr>
    <tr><td>$blobContainerName</td><td>De Blob-container waar u het script HiveQL te uploaden.</td></tr>
    </table>
2. Azure PowerShell ISE openen.

3. Kopieer en plak het volgende script in het script-veld:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Hier worden de variabelen in het script worden gebruikt:

    - **$hqlLocalFileName** - slaat het script het scriptbestand HiveQL lokaal voordat u deze uploadt naar een Blob-opslag. Dit is de naam van het bestand. De standaardwaarde is <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** - dit is het HiveQL script blob bestandsnaam wordt gebruikt in de Azure Blob-opslag. De standaardwaarde is tutorials/flightdelay/flightdelays.hql. Omdat het bestand rechtstreeks naar Azure Blob-opslag geschreven, is er een "/" aan het begin van de naam van de blob. Als u toegang wilt tot het bestand uit de Blob-opslag, moet u een "/" aan het begin van de bestandsnaam toevoegen.
    - **$srcDataFolder** en **$dstDataFolder** - "flightdelay-zelfstudies/data" = "flightdelay-zelfstudies/output" =


---
##<a id="appendix-c"></a>Bijlage C - Azure SQL-database voorbereiden voor de uitvoer van de taak Sqoop
**De SQL-database voorbereiden (samenvoegen dit met het script Sqoop)**

1. Voorbereiding van de parameters:

    <table border="1">
    <tr><th>Naam van variabele</th><th>Notities</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>De naam van de databaseserver van SQL Azure. Geef niets om een nieuwe server te maken.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>De aanmeldingsnaam voor de databaseserver van SQL Azure. Als $sqlDatabaseServerName een bestaande server is, worden de aanmeldings-id en aanmeldingswachtwoord gebruikt voor verificatie bij de server. Anders worden ze gebruikt voor het maken van een nieuwe server.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Het aanmeldingswachtwoord voor de databaseserver van SQL Azure.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Deze waarde wordt alleen gebruikt als u een nieuwe Azure databaseserver maakt.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>De SQL-database gebruikt voor het maken van de AvgDelays-tabel voor het project Sqoop. Leeg laat, wordt een database met de naam HDISqoop gemaakt. De naam van de tabel voor de uitvoer van de taak Sqoop is AvgDelays. </td></tr>
    </table>
2. Azure PowerShell ISE openen.
3. Kopieer en plak het volgende script in het script-veld:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Het script wordt een representational state transfer (REST)-service, http://bot.whatismyipaddress.com, gebruikt voor het ophalen van uw externe IP-adres. Het IP-adres wordt gebruikt voor het maken van een firewallregel voor SQL server-database.  

    Hier zijn enkele variabelen die worden gebruikt in het script:

    - **$ipAddressRestService** - de standaardwaarde is http://bot.whatismyipaddress.com. Het is een openbaar IP-adres REST service voor het ophalen van uw externe IP-adres. U kunt andere services gebruiken als u wilt. Het externe IP-adres opgehaald via de service wordt gebruikt voor het maken van een firewallregel voor uw Azure SQL databaseserver, zodat u toegang hebt tot de database vanaf uw werkstation (met behulp van een Windows PowerShell-script).
    - **$fireWallRuleName** - dit is de naam van de firewallregel voor de databaseserver van SQL Azure. De standaardnaam is <u>FlightDelay</u>. U kunt de naam wijzigen als u wilt.
    - **$sqlDatabaseMaxSizeGB** - deze waarde wordt alleen gebruikt als u bij het maken van een nieuwe Azure SQL-databaseserver. De standaardwaarde is 10GB. 10GB is voldoende voor deze zelfstudie.
    - **$sqlDatabaseName** - deze waarde wordt alleen gebruikt als u bij het maken van een nieuwe Azure SQL-database. De standaardwaarde is HDISqoop. Als u de naam wijzigt, moet u het Sqoop Windows PowerShell-script dienovereenkomstig bijwerken.

4. Druk op **F5** om het script uitvoert.
5. De uitvoer van het script valideren. Zorg ervoor dat het script met succes is uitgevoerd.

##<a id="nextsteps"></a>Volgende stappen
U weet nu hoe een bestand te uploaden naar Azure Blob opslag, het vullen van een component-tabel met de gegevens uit de Azure Blob-opslag onderdeel query's uitvoeren en Sqoop gebruiken om gegevens te exporteren uit HDFS naar Azure SQL-databases. Voor meer informatie, Zie de volgende artikelen:

* [Aan de slag met HDInsight][hdinsight-get-started]
* [Gebruik component met HDInsight][hdinsight-use-hive]
* [Gebruik Oozie met HDInsight][hdinsight-use-oozie]
* [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop]
* [Varken met HDInsight gebruiken][hdinsight-use-pig]
* [MapReduce Java-programma's ontwikkelen voor HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
