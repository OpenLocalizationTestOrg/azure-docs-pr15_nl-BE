<properties
    pageTitle="Gebruik van Hadoop Sqoop in HDInsight | Microsoft Azure"
    description="Informatie over het Azure PowerShell gebruiken vanaf een werkstation uitvoeren Sqoop importeren en exporteren tussen een cluster Hadoop en Azure SQL-database."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Sqoop gebruiken in combinatie met Hadoop in HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Sqoop in HDInsight importeren en exporteren tussen de cluster HDInsight en Azure SQL-database of SQL Server-database.

Hadoop is een natuurlijke keuze voor het verwerken van ongestructureerde en semistructured gegevens, zoals Logboeken en bestanden, kunnen ook er nodig voor het verwerken van gestructureerde gegevens die zijn opgeslagen in relationele databases.

[Sqoop] [ sqoop-user-guide-1.4.4] is een hulpmiddel dat is ontworpen voor het overbrengen van gegevens tussen clusters Hadoop en relationele databases. U kunt het importeren van gegevens uit een relationele database management system (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in het Hadoop, MapReduce of component transformeren en vervolgens de gegevens exporteren naar een RDBMS. In deze zelfstudie gebruikt u een SQL Server-database voor de relationele database.

Zie voor versies die worden ondersteund op clusters HDInsight Sqoop [Nieuw in de cluster-versies die door HDInsight?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Het scenario begrijpen

HDInsight cluster bevat voorbeeldgegevens. Gebruikt u de volgende twee voorbeelden:

- Een log4j logboekbestand zich in */example/data/sample.log bevindt*. De volgende logboekbestanden worden opgehaald uit het bestand:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Een component-tabel met de naam *hivesampletable*, die verwijst naar het bestand te vinden op */hive/warehouse/hivesampletable*. De tabel bevat enkele gegevens op mobiele apparaten. 

  	| Veld | Gegevenstype |
  	| ----- | --------- |
  	| clientid | tekenreeks |
  	| querytime | tekenreeks |
  	| markt | tekenreeks |
  	| deviceplatform | tekenreeks |
  	| devicemake | tekenreeks |
  	| devicemodel | tekenreeks |
  	| staat | tekenreeks |
  	| land | tekenreeks |
  	| querydwelltime | dubbel |
  	| sessie-id | bigint |
  	| sessionpagevieworder | bigint |

U exporteert u eerst *sample.log* en *hivesampletable* naar de Azure SQL-database of SQL Server en vervolgens importeren in de tabel met de gegevens van het mobiele apparaat terug naar de HDInsight door het volgende pad:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Cluster- en SQL-database maken

In deze sectie wordt beschreven hoe u een cluster en de SQL-database schema's voor het uitvoeren van de zelfstudie met behulp van de portal Azure en een bronnenbeheerder Azure-sjabloon te maken.  Als u liever Azure PowerShell, Zie [bijlage A](#appendix-a---a-powershell-sample).

1. Klik op de onderstaande afbeelding een resource manager als sjabloon wilt openen in de Portal Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De resource manager-sjabloon bevindt zich in een openbare blob container *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*. 
    
    De sjabloon resource manager roept een Bacpac-pakket voor de implementatie van het tabelschema SQL-database.  Het Bacpac-pakket bevindt zich ook in een openbare blob container https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Als u een persoonlijke container voor de Bacpac-bestanden gebruiken wilt, gebruikt u de volgende waarden in de sjabloon:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Voer de volgende gegevens van de bladeserver Parameters:

    - **Clusternaam**: Voer een naam voor het Hadoop-cluster dat u wilt maken.
    - **Cluster-aanmeldingsnaam en wachtwoord**: beheerder de aanmeldingsnaam van de standaard is.
    - **SSH-gebruikersnaam en wachtwoord**.
    - **SQL database server-aanmeldingsnaam en wachtwoord**.

    De volgende waarden zijn hardcoded in de sectie variabelen:
    
  	|Standaard opslag-accountnaam|<CluterName>winkel|
  	|----------------------------|-----------------|
  	|Azure SQL server-databasenaam|<ClusterName>dbserver|
  	|Azure SQL databasenaam|<ClusterName>DB|
    
    Schrijf deze waarden.  U moet ze later in de zelfstudie.
    
3. Klik op **OK** om op te slaan van de parameters.

4. uit de **aangepaste implementatie** blade, klikt u op de vervolgkeuzelijst **resourcegroep** en klik vervolgens op **Nieuw** om een nieuwe resourcegroep maken. De resourcegroep is een container waarin de account afhankelijk opslag en andere gekoppelde bron van het cluster worden gegroepeerd.

5. op **juridische voorwaarden**en klik vervolgens op **maken**.

6. Klik op **maken**. Ziet u een nieuwe tegel met de titel Submitting implementatie voor de implementatie van de sjabloon. Duurt ongeveer 20 minuten over het maken van het cluster en de SQL-database.

Als u kiest voor bestaande Azure SQL-database of Microsoft SQL Server

- **Azure SQL-database**: moet u een firewallregel voor de databaseserver van SQL Azure toegang toegestaan vanaf uw werkstation. Voor instructies over het maken van een Azure SQL database- en [aan de slag met Azure SQL-database]configureren van de firewall, Zie[sqldatabase-get-started]. 

    > [AZURE.NOTE] Standaard Azure SQL-database verbindingen mogelijk vanaf Azure services, zoals Azure HDInsight. Als deze firewallinstelling is uitgeschakeld, moet u deze vanaf de portal Azure ingeschakeld. Zie voor instructies over het maken van een Azure SQL-database en configureert firewallregels [maken en configureren van SQL-Database][sqldatabase-create-configue].

- **SQL Server**: als uw cluster HDInsight op hetzelfde virtuele netwerk in Azure als SQL Server, kunt u de stappen in dit artikel voor het importeren en exporteren van gegevens naar een SQL Server-database.

    > [AZURE.NOTE] HDInsight ondersteunt virtuele netwerken alleen op basis van locatie en deze werkt momenteel niet met virtuele netwerken op basis van affiniteit groep.

    * Zie als u wilt maken en configureren van een virtueel netwerk, [maken een virtueel netwerk met behulp van de portal Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Wanneer u SQL Server in uw datacenter gebruikt, moet u het virtuele netwerk configureren als *website* of *punt-naar-site*.

            > [AZURE.NOTE] Voor **punt-naar-site** virtuele netwerken, moet SQL Server worden uitgevoerd de VPN-client toepassing voor databaseconfiguratie, die beschikbaar via het **Dashboard** van uw Azure virtual netwerkconfiguratie is.

        * Wanneer u SQL Server op een Azure virtuele machine gebruikt, kan de configuratie van een virtueel netwerk als de virtuele machine waarop SQL-Server lid van hetzelfde virtuele netwerk als HDInsight is worden gebruikt.

    * Zie een cluster HDInsight maken op een virtueel netwerk [maken Hadoop clusters in HDInsight met aangepaste opties](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] SQL Server moet ook verificatie toestaan. Een SQL Server-aanmelding moet u de stappen in dit artikel.


## <a name="run-sqoop-jobs"></a>Sqoop taken uitvoeren

HDInsight Sqoop taken kan worden uitgevoerd met behulp van verschillende methoden. In de volgende tabel om te beslissen welke methode u gebruikt en volg de link voor een overzicht.

| **Gebruik deze optie** als u wilt...                                   | .. .an **interactieve** shell | ... **batch** -verwerking | .. .door dit **cluster besturingssysteem** | .. .from dit **besturingssysteem van client** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [.NET SDK voor Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux of Windows                          | Windows (voor nu)                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |

##<a name="limitations"></a>Beperkingen

* Bulk export - met Linux-gebaseerde HDInsight, de Sqoop-connector gebruikt om gegevens te exporteren naar Microsoft SQL Server of SQL-Database Azure biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchen - met Linux-gebaseerde HDInsight bij het gebruik van de `-batch` Ga bij het uitvoeren van toevoegingen, Sqoop meerdere ingevoegd in plaats van de batchverwerking de insert-bewerkingen worden uitgevoerd.

##<a name="next-steps"></a>Volgende stappen

U hebt nu geleerd dat het gebruik van Sqoop. Voor meer informatie, Zie:

- [Gebruik component met HDInsight](hdinsight-use-hive.md)
- [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
- [Gebruik van Oozie met HDInsight][hdinsight-use-oozie]: gebruik Sqoop actie in de workflow van een Oozie.
- [Gegevens te analyseren vlucht vertraging met HDInsight][hdinsight-analyze-flight-data]: component gebruiken voor het analyseren van flight uitstellen van gegevens en gebruik vervolgens Sqoop om gegevens te exporteren naar een database SQL Azure.
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]: zoeken naar andere methoden voor het uploaden van gegevens naar HDInsight/Azure Blob-opslag.


## <a name="appendix-a---a-powershell-sample"></a>Bijlage A - een monster PowerShell

De PowerShell-voorbeeld worden de volgende stappen uitgevoerd:

1. Verbinding maken met Azure.
2. Maak een brongroep Azure. Zie voor meer informatie, [Met behulp van Azure PowerShell Azure Resource Manager](../powershell-azure-resource-manager.md)
3. Maak een Azure SQL-databaseserver, een Azure SQL-database en twee tabellen. 

    Als u SQL Server in plaats daarvan, gebruikt u de volgende instructies om de tabellen te maken:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    De eenvoudigste manier om de onderzoeken van de database en tabellen is het gebruik van Visual Studio. De databaseserver en de database kunnen worden onderzocht met behulp van de portal Azure.

4. Maak een cluster HDInsight.

    Onderzoeken van het cluster, kunt u de Azure portal of Azure PowerShell.

5. Het brongegevensbestand vooraf worden verwerkt.

    In deze zelfstudie wordt u een logboekbestand log4j (een bestand met scheidingstekens) en een component-tabel exporteren naar een Azure SQL-database. Het bestand met scheidingstekens wordt */example/data/sample.log*genoemd. Eerder in de zelfstudie, hebt u enkele voorbeelden van logs log4j gezien. In het logboekbestand zijn er enkele lege regels en regels zoals de volgende:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    Dit is fijn voor andere voorbeelden die gebruikmaken van deze gegevens, maar deze uitzonderingen moet worden verwijderd voordat we de Azure SQL-database of SQL Server te importeren. Sqoop export zal mislukken als er een lege tekenreeks of een lijn met een kleiner aantal elementen dan het aantal velden in de tabel Azure SQL-database zijn gedefinieerd. De tabel log4jlogs bevat 7 velden van het type string.

    Deze procedure maakt u een nieuw bestand op het cluster: tutorials/usesqoop/data/sample.log. Om het gewijzigde bestand bekijkt, kunt u de Azure portal, een hulpprogramma voor opslag van Azure explorer of Azure PowerShell. [Aan de slag met HDInsight] [ hdinsight-get-started] is een voorbeeld van code voor Azure PowerShell gebruiken om inhoud van het bestand te downloaden van een bestand.

6. Een bestand exporteren naar de Azure SQL-database.

    Het bestand is tutorials/usesqoop/data/sample.log. De tabel waarin de gegevens worden geëxporteerd naar heet log4jlogs.
    
    > [AZURE.NOTE] Dan verbindingsinformatie werken de stappen in deze sectie moeten voor Azure SQL-database of SQL Server. Deze stappen zijn getest met de volgende configuratie:
    >
    > * **Configuratie van Azure virtueel netwerk punt-naar-site**: een virtueel netwerk de cluster HDInsight verbonden met een SQL-Server in een particulier datacenter. Zie [een Point-to-Site VPN in de Portal voor beheer configureren](../vpn-gateway/vpn-gateway-point-to-site-create.md) voor meer informatie.
    > * **Azure HDInsight 3.1**: Zie [clusters maken Hadoop in HDInsight met aangepaste opties](hdinsight-provision-clusters.md) voor informatie over het maken van een cluster op een virtueel netwerk.
    > * **SQL Server 2014**: geconfigureerd om verificatie en de VPN-client waarop configuratiepakket voor een veilige verbinding met het virtuele netwerk.

7. Een component-tabel exporteren naar de Azure SQL-database.

8. De tabel mobiledata in het cluster HDInsight importeren.

    Om het gewijzigde bestand bekijkt, kunt u de Azure portal, een hulpprogramma voor opslag van Azure explorer of Azure PowerShell.  [Aan de slag met HDInsight] [ hdinsight-get-started] is een voorbeeld van code over Azure PowerShell gebruiken om inhoud van het bestand te downloaden van een bestand.


### <a name="the-powershell-sample"></a>De PowerShell-voorbeeld

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
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
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
