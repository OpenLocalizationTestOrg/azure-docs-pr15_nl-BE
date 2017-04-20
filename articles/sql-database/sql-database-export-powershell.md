<properties
    pageTitle="Azure SQL-database naar een bestand Bacpac-archiveren met PowerShell"
    description="Azure SQL-database naar een bestand Bacpac-archiveren met PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Azure SQL-database naar een bestand Bacpac-archiveren met PowerShell

> [AZURE.SELECTOR]
- [Azure portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


Dit artikel bevat instructies voor het archiveren van uw Azure SQL-database naar een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand (opgeslagen in Azure Blob-opslag) met PowerShell.

Wanneer u een Azure SQL-database te archiveren, kunt u het databaseschema en de gegevens exporteren naar een bestand Bacpac. Een Bacpac-bestand is gewoon een ZIP-bestand met de extensie .bacpac. Een Bacpac-bestand kan later worden opgeslagen in Azure Blob-opslag of lokale opslag op een locatie op gebouwen. Kunnen ook worden geïmporteerd in Azure SQL-Database of in een SQL Server-installatie in gebouwen.

**Overwegingen met betrekking tot**

- Een archief transactioneel consistent is, moet u ervoor zorgen dat er activiteit plaatsvindt tijdens het exporteren of exporteren van een [Transactioneel consistente kopie](sql-database-copy.md) van uw Azure SQL-database.
- De maximale grootte van een Bacpac-bestand opgeslagen in Azure Blob-opslag is 200 GB. Als u wilt archiveren een groter Bacpac-bestand naar de lokale opslag, gebruik het opdrachtregelprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server. U kunt ook [download](https://msdn.microsoft.com/library/mt204009.aspx) de nieuwste versie van SQL Server Data Tools om met dit hulpprogramma.
- Archiveren naar Azure premium opslag met behulp van een Bacpac-bestand wordt niet ondersteund.
- Als de exportbewerking 20 uur overschrijdt, kan deze worden geannuleerd. U verhoogt de prestaties tijdens het exporteren, kunt u het volgende doen:
 - Tijdelijk verhogen uw serviceniveau.
 - Niet meer alle lees- en schrijfbewerkingen tijdens het exporteren.
 - Een [gegroepeerde index](https://msdn.microsoft.com/library/ms190457.aspx) gebruiken met niet-nulwaarden voor alle grote tabellen. Zonder gegroepeerde indexen worden exporteren kan mislukken als het duurt langer dan 6 tot 12 uur. Dit komt omdat de export service scannen van de tabel om te proberen moet om de hele tabel exporteren te voltooien. Er is een goede manier om te bepalen als uw tabellen zijn geoptimaliseerd voor uitvoer **DBCC SHOW_STATISTICS** uitvoeren en zorg ervoor dat de *RANGE_HI_KEY* niet null is en de waarde ervan is een goede verdeling. Zie [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx)voor details.

> [AZURE.NOTE] BACPACs zijn niet bedoeld om te worden gebruikt voor back-up- en terugzetbewerkingen. Azure SQL-Database automatisch back-ups voor elke gebruiker-database gemaakt. Zie voor details, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).

Als u dit artikel wilt nodig u het volgende:

- Een abonnement op Azure.
- Azure SQL-database.
- Een [account Azure standaardopslag](../storage/storage-create-storage-account.md), met een blob-container voor het opslaan van de BACPAC in standaard opslag.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Uw database exporteren

De [nieuw AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)-cmdlet dient een databaseverzoek exporteren naar de service. De exportbewerking kan enige tijd duren, afhankelijk van de grootte van de database.

> [AZURE.IMPORTANT] Om te kunnen garanderen een transactioneel consistent Bacpac-bestand, moet eerst [een kopie van de database maken](sql-database-copy-powershell.md)en vervolgens de databasekopie te exporteren.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>De voortgang van de exportbewerking

Na het uitvoeren van [New AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), u kunt de status controleren van de aanvraag door te voeren [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Met dit onmiddellijk na de aanvraag meestal geeft **Status: InProgress**. Wanneer er **Status: geslaagd** het exporteren is voltooid.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Voorbeeld van SQL-database exporteren

In het volgende voorbeeld wordt een bestaande SQL-database geëxporteerd naar een BACPAC en wordt vervolgens de status van de exportbewerking.

U kunt het voorbeeld uitvoeren zijn er een paar variabelen die u wilt vervangen door de specifieke waarden voor uw account en de opslag. Ga naar uw account opslag opslag accountnaam, blob containernaam en sleutelwaarde in [Azure portal](https://portal.azure.com). De sleutel kunt u vinden door te klikken op de **toegangstoetsen** op uw account opslag blade.

Vervangen door de volgende `VARIABLE-VALUES` met de waarden voor uw specifieke resources Azure. De naam van de database, wordt de bestaande database die u wilt exporteren.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Volgende stappen

- Als u wilt weten hoe een Azure SQL-database importeert via Powershell, Zie [importeren een BACPAC met PowerShell](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Aanvullende bronnen

- [Nieuwe AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)