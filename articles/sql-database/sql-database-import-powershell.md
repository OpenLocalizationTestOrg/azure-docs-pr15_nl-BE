<properties
    pageTitle="Importeren van een bestand Bacpac-een Azure SQL-database maken met behulp van PowerShell | Microsoft Azure"
    description="Een Azure SQL-database maken met behulp van PowerShell Bacpac-bestanden importeren"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Een Azure SQL-database maken met behulp van PowerShell Bacpac-bestanden importeren

**Enkele database**

> [AZURE.SELECTOR]
- [Azure portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Dit artikel bevat instructies voor het maken van een Azure SQL-database te importeren van een bestand [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) met PowerShell.

De database wordt gemaakt van een Bacpac-bestand (.bacpac) ingevoerd uit een container opslag Azure blob. Als er geen een Bacpac-bestand in Azure, opslag, Zie [archief Azure SQL-database naar een bestand Bacpac-met PowerShell](sql-database-export-powershell.md). Als u al een Bacpac-bestand niet in Azure, opslag, [Gebruik AzCopy om het heel eenvoudig uploaden naar uw rekening Azure opslag](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Azure SQL-Database wordt automatisch gemaakt en onderhouden van back-ups voor elke gebruikersdatabase die u kunt terugzetten. Zie voor details, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).


Als u wilt importeren in een SQL-database, moet u het volgende:

- Een abonnement op Azure. Als u een abonnement op Azure gewoon **Gratis** boven aan deze pagina op en teruggaan naar het voltooien van dit artikel.
- Een Bacpac-bestand van de database die u wilt importeren. De BACPAC moet in een container van de blob [Storage Azure-account](../storage/storage-create-storage-account.md) .



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>De variabelen voor uw omgeving instellen

Er zijn een paar variabelen waar u moet de voorbeeldwaarden vervangen door de specifieke waarden voor de database en uw account voor opslag.

De naam van de server moet een server die al in het abonnement in de vorige stap hebt geselecteerd bestaat. De server waarmee u te maken de database moeten worden. Een database importeren rechtstreeks in een elastische toepassingen wordt niet ondersteund. Maar eerst importeren in één database en vervolgens de database verplaatst naar een groep.

Naam van de database is de naam die u wilt gebruiken voor de nieuwe database.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


De volgende variabelen zijn van de account opslag waar uw BACPAC zich bevindt. Ga naar uw account opslag om deze waarden in de [portal Azure](https://portal.azure.com). De primaire toegangstoets kunt u vinden door te klikken op **alle instellingen** en vervolgens de **toetsen** van uw opslag-account blade.

De blob is de naam van een bestaand Bacpac-bestand dat u wilt maken van de database uit. U moet de extensie .bacpac.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Uitvoeren van de [Get-referentie] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) cmdlet opent u een venster waarin u wordt gevraagd uw gebruikersnaam en wachtwoord. Voer de admin login en het wachtwoord voor de SQL-databaseserver ($ServerName van boven), en niet de gebruikersnaam en wachtwoord voor uw account Azure.

    $credential = Get-Credential


## <a name="import-the-database"></a>De database importeren

Met deze opdracht verzendt een databaseverzoek importeren naar de service. De importbewerking kan enige tijd duren, afhankelijk van de grootte van de database.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>De voortgang van de bewerking

Na het uitvoeren van [New AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), u kunt de status controleren van de aanvraag door het uitvoeren van de [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>Script voor PowerShell voor SQL-Database importeren


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over verbinding maken met en de query een SQL-Database geïmporteerde [verbinding maken met een SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uit te voeren](sql-database-connect-query-ssms.md)
