<properties 
   pageTitle="Aan de slag met Azure gegevens Lake Analytics met Azure PowerShell | Azure" 
   description="Informatie over het gebruik van de Azure PowerShell een gegevensarchief Lake-account maken, geen gegevens meer Analytics taak gebruikt U SQL maken en indienen van de taak. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Zelfstudie: aan de slag met Azure gegevens Lake Analytics met Azure PowerShell

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Leren werken met de Azure PowerShell Azure gegevens Lake Analytics accounts maken en verzenden taken naar de gegevens meer analytische gegevens Lake Analytics taken definiëren in [U SQL](data-lake-analytics-u-sql-get-started.md). Zie voor meer informatie over gegevens Lake Analytics [Azure gegevens Lake Analytics overzicht](data-lake-analytics-overview.md).

In deze zelfstudie wordt u een taak die u leest een tab gescheiden waarden (TSV) en zet deze in een bestand met door komma's gescheiden waarden (CSV) ontwikkelen. Om na te gaan via dezelfde zelfstudie met behulp van andere ondersteunde hulpprogramma's, klikt u op de tabs boven aan deze sectie.

##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- **Een workstation met Azure PowerShell**. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Gegevens Lake Analytics-account maken

Voordat u alle taken kunt uitvoeren, moet u een account gegevens Lake Analytics hebben. Om een gegevens Lake Analytics-account maken, moet u het volgende opgeven:

- **Resourcegroep Azure**: A Data Lake Analytics account moet worden gemaakt binnen een groep Azure. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u werken met de resources in uw toepassing als een groep. U kunt implementeren, bijwerken of verwijderen alle bronnen voor uw toepassing in een enkele, gecoördineerde werking.  

    Inventariseren van de resourcegroepen in uw abonnement:
    
        Get-AzureRmResourceGroup
    
    Een nieuwe resourcegroep maken:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Naam Analytics gegevens Lake**
- **Locatie**: een van de Azure datacenters die gegevens Lake Analytics ondersteunt.
- **Standaard gegevens Lake account**: elke gegevens Lake Analytics-account heeft een standaardaccount voor meer gegevens.

    Een nieuwe account te maken meer gegevens:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] De naam van de gegevens meer mag alleen kleine letters en cijfers bevatten.



**Een Data Lake Analytics-account maken**

1. PowerShell ISE openen vanuit een Windows-werkstation.
2. Voer het volgende script:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Gegevens uploaden naar Lake gegevens

In deze zelfstudie verwerkt u sommige zoeken in Logboeken.  Het logboek zoeken kan worden opgeslagen in de winkel gegevens Lake of Azure Blob-opslag. 

Een voorbeeldlogboekbestand voor het zoeken is naar een openbare Azure Blob-container gekopieerd. Download het bestand naar uw werkstation met de volgende PowerShell script en vervolgens het bestand uploaden naar de standaardaccount Lake gegevensarchief van uw account gegevens Lake Analytics.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

De volgende PowerShell script laat zien hoe u de naam van het gegevensarchief Lake standaard voor een gegevens Lake Analytics-account:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] De Portal Azure biedt een gebruikersinterface Kopieer de voorbeeldbestanden naar de standaardaccount gegevensarchief Lake. Zie voor instructies [Aan de slag met Azure gegevens Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Gegevens Lake Analytics ook toegang tot Azure Blob-opslag.  Zie voor het uploaden van gegevens naar Azure Blob-opslag, [Azure PowerShell gebruiken met Azure opslag](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Indienen van gegevens Lake Analytics taken

De gegevens Lake Analytics taken zijn geschreven in de taal die U SQL. Zie voor meer informatie over U SQL, [aan de slag met U SQL-taal](data-lake-analytics-u-sql-get-started.md) en [U SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348).

**Een taak gegevens Lake Analytics script maken**

- Maak een tekstbestand met de volgende U SQL-script en sla het tekstbestand op uw werkstation:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Dit U SQL-script leest het brongegevensbestand met **Extractors.Tsv()**en maakt vervolgens een CSV-bestand met behulp van **Outputters.Csv()**. 
    
    De twee paden niet worden gewijzigd, tenzij u het bronbestand naar een andere locatie kopiëren.  Gegevens Lake Analytics maakt de uitvoermap als het niet bestaat.
    
    Is het eenvoudiger de relatieve paden voor de bestanden opgeslagen in standaard gegevens meer accounts gebruikt. Ook kunt u absolute paden.  Bijvoorbeeld 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Absolute paden hebt u toegang tot bestanden in de gekoppelde accounts voor opslag.  De syntaxis voor bestanden die zijn opgeslagen in de gekoppelde Azure opslag account is:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob container met openbare BLOB's of machtigingen voor openbare containers worden momenteel niet ondersteund.    
    
    
**De taak verzenden**

1. PowerShell ISE openen vanuit een Windows-werkstation.
2. Voer het volgende script:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    In het script, wordt het bestand U SQL-script opgeslagen op c:\tutorials\data-lake-analytics\copyFile.usql. Het pad naar het bestand dienovereenkomstig bijwerken.
 
Nadat de taak is voltooid, kunt u de volgende cmdlets gebruiken om het bestand en het bestand downloaden:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Zie ook

- Klik op het tabblad kiezers boven aan de pagina overzicht dezelfde zelfstudie met behulp van andere hulpprogramma's.
- Een complexe query's, Zie [Logboeken van Website analyseren met behulp van Azure gegevens Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Zie [ontwikkelt U SQL - scripts met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen U SQL-toepassingen ontwikkelen.
- Zie informatie over U SQL, [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md).
- Zie voor beheertaken [Beheren Azure gegevens Lake Analytics met Azure Portal](data-lake-analytics-manage-use-portal.md).
- Zie voor een overzicht van gegevens Lake Analytics, [Azure gegevens Lake Analytics overzicht](data-lake-analytics-overview.md).

