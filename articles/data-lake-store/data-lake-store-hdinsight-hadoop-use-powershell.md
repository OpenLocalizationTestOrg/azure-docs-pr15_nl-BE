<properties
   pageTitle="HDInsight clusters maken met Azure Lake gegevensarchief met PowerShell | Azure"
   description="Azure PowerShell kunt maken en gebruiken van clusters HDInsight met Azure gegevens Lake"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Maak een cluster HDInsight met Lake gegevensarchief met Azure PowerShell

> [AZURE.SELECTOR]
- [Met behulp van Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Met behulp van PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Met behulp van bronbeheer](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Informatie over het Azure PowerShell gebruiken voor het configureren van een cluster HDInsight (Hadoop, HBase of Storm) met toegang tot de gegevensopslag Lake Azure. Enkele belangrijke overwegingen voor deze release:

* **Motoren voor clusters (Linux), en clusters van Hadoop/Storm (Windows en Linux)**, het gegevensarchief Lake kan alleen worden gebruikt als een account extra opslagruimte. De standaardaccount voor de opslag van dergelijke clusters steeds Azure opslag BLOB's (WASB) nog.

* **HBase voor clusters (Windows en Linux)**, het gegevensarchief Lake kan worden gebruikt als een standaard opslag of extra opslagruimte.

> [AZURE.NOTE] Enkele belangrijke punten om te onthouden. 
> 
> * Optie voor het maken van clusters van HDInsight met toegang tot de gegevensopslag Lake is alleen beschikbaar voor HDInsight versie 3.2 en 3.4 (voor clusters op zowel Windows als Linux Hadoop, HBase en Storm). Deze optie is alleen beschikbaar op HDInsight 3.4 clusters voor clusters Spark op Linux.
>
> * Zoals hierboven vermeld, is als standaard opslag voor bepaalde clustertypen (HBase) en extra opslagruimte voor andere clustertypen (Hadoop, Spark, Storm) Lake gegevensopslag beschikbaar. Met behulp van Lake gegevensarchief als een account extra opslagruimte heeft geen gevolgen voor prestaties of de mogelijkheid om te lezen/schrijven voor de opslag van het cluster. In een scenario waar Lake gegevensarchief wordt gebruikt als extra opslagruimte, cluster-bestanden (zoals Logboeken, enz.) om de standaard opslag Azure BLOB's (), geschreven terwijl de gegevens die u wilt verwerken in een gegevensarchief Lake account kunnen worden opgeslagen.


In dit artikel richten we een cluster Hadoop met Lake gegevensarchief als extra opslagruimte.

HDInsight werken met Lake gegevensarchief configureren omvat met PowerShell de volgende stappen:

* Maak een Azure-gegevensarchief Lake
* Voor op rollen gebaseerde toegang tot de gegevensopslag Lake-verificatie instellen
* HDInsight cluster met verificatie in Lake gegevensarchief maken
* Een testtaak uitvoeren op het cluster

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 of hoger**. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

- **Windows SDK**. Vanaf [hier](https://dev.windows.com/en-us/downloads)kunt u deze installeren. Hiermee u maakt een beveiligingscertificaat.

- **Azure Active Directory Service Principal**. Stappen in deze zelfstudie geven instructies voor het maken van een service principal in AD Azure. U moet wel een beheerder AD Azure mogelijk te maken van een service principal. Als u een Azure AD-beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.
    
    **Als u niet een Azure AD-beheerder bent**, is het niet mogelijk voor het uitvoeren van de stappen die nodig zijn voor het maken van een service principal. In dat geval moet uw beheerder Azure AD eerst een service principal maken voordat u een cluster HDInsight met Lake gegevensarchief kunt maken. Ook moet de service principal worden gemaakt met behulp van een certificaat, zoals beschreven bij het [maken van een service principal met certificaat](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Maak een Azure-gegevensarchief Lake

Volg deze stappen uit om een gegevensarchief Lake.

1. Op het bureaublad opent een nieuw venster met Azure PowerShell en voer het volgende fragment. Wanneer u wordt gevraagd aan te melden, moet dat u zich aanmelden als een van de admininistrators abonnement/eigenaar:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Als er een vergelijkbaar met foutbericht `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` bij het registreren van de provider Lake gegevensarchief resource, is het mogelijk dat de subsrcription niet whitelisted voor Azure Lake gegevensarchief. Zorg ervoor dat u uw abonnement Azure voor openbare preview Lake gegevensarchief inschakelen door deze [instructies](data-lake-store-get-started-portal.md#signup)te volgen.

3. Een gegevensarchief voor Lake Azure-account is gekoppeld aan een resourcegroep Azure. Begin met het maken van een Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Maak een brongroep Azure] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Maak een brongroep Azure")

2. Maak een account Azure Lake gegevensarchief. De naam van de account die u opgeeft moet alleen kleine letters en cijfers bevatten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Een gegevens-Lake Azure-account maken] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Een gegevens-Lake Azure-account maken")

3. Controleer of de account is gemaakt.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    De uitvoer van dit moet **waar**zijn.

4. Voorbeeldgegevens naar Azure gegevens Lake uploaden. Gebruikt dit later in dit artikel om te controleren of de gegevens toegankelijk zijn vanuit een cluster HDInsight. Als u voorbeeldgegevens wilt uploaden zoekt, krijgt u de map **Data ambulances** uit de [Azure gegevens Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Voor op rollen gebaseerde toegang tot de gegevensopslag Lake-verificatie instellen

Elke Azure abonnement is gekoppeld aan een Azure Active Directory. Gebruikers en services die toegang hebben tot de bronnen van het abonnement met de klassieke Portal Azure of Azure Resource Manager API moeten eerst worden geverifieerd met die Azure Active Directory. Toegang wordt verleend tot Azure abonnementen en diensten door het toewijzen van de juiste rol op een Azure-bron.  Een service principal geeft voor services de service in de Azure Active Directory (AAD). In dit gedeelte ziet u hoe u een toepassingsservice, zoals HDInsight, toegang tot een Azure bron (de account Azure Lake gegevensarchief u eerder hebt gemaakt) verlenen door het maken van een service principal voor de toepassing en rollen toewijzen aan die via Azure PowerShell.

Als u verificatie van Active Directory voor Azure gegevens meer instelt, moet u de volgende taken uitvoeren.

* Een zelfondertekend certificaat maken
* Een toepassing maken in Azure Active Directory en een Service Principal

### <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

Zorg ervoor dat u beschikt over [Windows SDK](https://dev.windows.com/en-us/downloads) is geïnstalleerd voordat u verdergaat met de stappen in deze sectie. U moet een map, zoals **C:\mycertdir**, waar het certificaat wordt gemaakt ook hebt gemaakt.

1. Navigeer naar de locatie waar u de SDK van Windows geïnstalleerd vanuit het venster PowerShell (meestal `C:\Program Files (x86)\Windows Kits\10\bin\x86` en gebruik de [MakeCert] [ makecert] hulpprogramma voor het maken van een zelfondertekend certificaat en een persoonlijke sleutel. Gebruik de volgende opdrachten.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    U wordt gevraagd het wachtwoord voor persoonlijke sleutel opgeven. Nadat de opdracht met succes wordt uitgevoerd, ziet u een **CertFile.cer** en **mykey.pvk** in de opgegeven map voor het certificaat.

4. Gebruik de [Pvk2Pfx] [ pvk2pfx] hulpprogramma de MakeCert gemaakt PVK en .cer-bestanden converteren naar een pfx-bestand. Voer de volgende opdracht.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Wanneer u wordt gevraagd de eerder opgegeven wachtwoord voor persoonlijke sleutel opgeven. De waarde die u voor de parameter **-IO opgeeft** is het wachtwoord dat is gekoppeld aan het .pfx-bestand. Nadat de opdracht met succes is voltooid, ziet u ook een CertFile.pfx in de opgegeven map voor het certificaat.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Maak een Azure Active Directory en een service principal

In dit gedeelte voert u de stappen voor het maken van een service principal voor Azure Active Directory-toepassingen, een rol toewijzen aan de service principal en door middel van een certificaat worden geverifieerd als de service principal. Voer de volgende opdrachten voor het maken van een toepassing in Azure Active Directory.

1. Plak de volgende cmdlets in het consolevenster van PowerShell. Zorg ervoor dat de waarde die u voor de eigenschap **DisplayName opgeeft-** uniek is. Ook de waarden voor **Startpagina-** en **IdentiferUris -** tijdelijke waarden en niet worden geverifieerd.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Maken van een service principal met behulp van de toepassings-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. De service principal toegang verlenen tot het gegevensarchief Lake bestand of de map die u toegang uit het cluster HDInsight tot. Het onderstaande stukje biedt toegang tot de hoofdmap van het gegevensarchief Lake-account.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Typ achter de prompt **Y** om te bevestigen.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Een HDInsight-cluster maken met verificatie in het gegevensarchief Lake

In deze sectie maken we een cluster HDInsight Hadoop. Voor deze release, de cluster HDInsight en het gegevensarchief Lake moeten op dezelfde locatie (Oost-VS 2).

1. Begin met het ophalen van de abonnement-id huurder. Moet u die later.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Voor deze release geldt kan voor een cluster Hadoop Lake gegevensopslag alleen worden gebruikt als een extra opslagruimte voor het cluster. De standaard opslag nog steeds de opslag Azure BLOB's (WASB). Dus eerst maken we de opslag account en Opslagcontainers nodig zijn voor het cluster.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. De HDInsight cluster maken. Gebruik de volgende cmdlets.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nadat de cmdlet met succes is voltooid, ziet u een output als volgt:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Testtaken uitvoeren op de cluster HDInsight gebruiken het gegevensarchief Lake

Nadat u een HDInsight-cluster hebt geconfigureerd, kunt u testtaken uitvoeren op het cluster om te testen of het cluster HDInsight Lake gegevensarchief toegang. We voeren een taak monster component maakt een tabel met behulp van de voorbeeldgegevens die u eerder hebt geüpload naar de gegevensopslag Lake hiervoor.

### <a name="for-a-linux-cluster"></a>Voor een Linux-cluster

In deze sectie wordt u SSH in de cluster en run de een voorbeeldquery component. Windows beschikt niet over een ingebouwde SSH-client. Beste **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Wanneer een verbinding, start u de CLI component met de volgende opdracht:

        hive

2. Met behulp van de CLI, voert u de volgende instructies om een nieuwe tabel met de naam **voertuigen** de voorbeeldgegevens in de gegevensopslag Lake maken:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Een uitvoer van de volgende strekking worden weergegeven:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Voor een cluster met Windows

Gebruik de volgende cmdlets voor het uitvoeren van de query voor de component. In deze query een tabel maken van de gegevens in het gegevensarchief Lake we en vervolgens een query uitvoeren op de gemaakte tabel.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Dit heeft de volgende uitvoer. **ExitValue** van 0 in de uitvoer wordt voorgesteld dat de taak is voltooid.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

De uitvoer van de taak met behulp van de volgende cmdlet ophalen:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

De uitvoer van de taak de volgende strekking weergegeven:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Access Lake gegevensarchief HDFS opdrachten

Nadat u het cluster HDInsight Lake gegevensopslag gebruikt hebt geconfigureerd, kunt u de HDFS shell-opdrachten voor toegang tot de winkel.

### <a name="for-a-linux-cluster"></a>Voor een Linux-cluster

In dit gedeelte u SSH zal in het cluster en de HDFS opdrachten uitvoeren. Windows beschikt niet over een ingebouwde SSH-client. Beste **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Wanneer een verbinding, u het volgende HDFS filesystem om de bestanden in het gegevensarchief Lake.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Dit moet het bestand dat u eerder hebt geüpload naar de gegevensopslag Lake aanbieden.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

U kunt ook de `hdfs dfs -put` opdracht sommige bestanden uploaden naar de gegevensopslag Lake en vervolgens met `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.


### <a name="for-a-windows-cluster"></a>Voor een cluster met Windows

1. Aanmelden voor de nieuwe [Portal Azure](https://portal.azure.com).

2. Klik op **Bladeren** **HDInsight clusters**en klik vervolgens op het cluster HDInsight die u hebt gemaakt.

3. Klik op **Extern bureaublad**in de blade cluster en klik op **verbinding maken**in de **Extern bureaublad** -blade.

    ![Externe HDI cluster] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Maak een brongroep Azure")

    Voer desgevraagd de referenties die u hebt opgegeven voor de extern bureaublad-gebruiker.

4. In de externe sessie start Windows PowerShell en de HDFS filesystem-opdrachten gebruiken om de bestanden in de Azure Lake gegevensopslag.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    Dit moet het bestand dat u eerder hebt geüpload naar de gegevensopslag Lake aanbieden.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    U kunt ook de `hdfs dfs -put` opdracht sommige bestanden uploaden naar de gegevensopslag Lake en vervolgens met `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.

## <a name="see-also"></a>Zie ook

* [Portal: Een cluster van HDInsight voor het gebruik van Lake gegevensarchief maken](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
