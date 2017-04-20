<properties
    pageTitle="PowerShell gebruiken om een back-up en herstel App Service apps"
    description="Informatie over het gebruik van PowerShell back-up maken en terugzetten van een app in Azure App-Service"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>PowerShell gebruiken om een back-up en herstel App Service apps

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST-API](../app-service-web/websites-csm-backup.md)

Informatie over het Azure PowerShell gebruiken om een back-up maken en terugzetten van [apps in App-Service](https://azure.microsoft.com/services/app-service/web/). Zie voor meer informatie over web app back-ups, vereisten en beperkingen, met inbegrip van de [Back-up van een web app in Azure App-Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Vereisten
PowerShell voor het beheren van uw app back-ups gebruikt, moet u het volgende:

- **Een SAS URL** waarmee u lees- en schrijftoegang tot een Azure opslag container. Zie [informatie over het model SAS](../storage/storage-dotnet-shared-access-signature-part-1.md) voor een uitleg van SAS-URL's. [Met behulp van Azure PowerShell met Azure opslag](../storage/storage-powershell-guide-full.md) Zie voor voorbeelden van het beheer van opslagruimte op Azure PowerShell gebruiken.
- **A de verbindingsreeks** als u wilt back-up van een database, samen met uw web app.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Het genereren van een SAS-URL te gebruiken met de web app backup-cmdlets
Een SAS-URL kan worden gegenereerd met PowerShell. Hier volgt een voorbeeld van hoe te genereren die kan worden gebruikt met de cmdlets die in dit artikel besproken.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Azure PowerShell 1.3.2 installeren of hoger

Zie [Azure PowerShell Azure Resource Manager gebruiken](../powershell-install-configure.md) voor instructies voor het installeren en gebruiken van Azure PowerShell.

## <a name="create-a-backup"></a>Een back-up maken

Gebruik de cmdlet New-AzureRmWebAppBackup voor het maken van een back-up van een web app.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Hiermee maakt u een back-up met een automatisch gegenereerde naam. Als u een naam voor de back-up wilt, gebruikt u de optionele parameter naam_back-up.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Als u wilt een database opnemen in de back-up, maakt eerst een back-up database-instelling met de cmdlet New-AzureRmWebAppDatabaseBackupSetting en die instelling in de Databases-parameter van de cmdlet New-AzureRmWebAppBackup te leveren. De Databases parameter accepteert een matrix met database-instellingen, zodat u kunt een back-up van meer dan één database.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Ophalen van back-ups

De cmdlet Get-AzureRmWebAppBackupList geeft als resultaat een matrix van alle back-ups voor een web app. U moet de naam van de web app en de resourcegroep opgeven.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Als u een specifieke back-up, gebruikt u de cmdlet Get-AzureRmWebAppBackup.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

U kunt ook een web app-object in een van de back-up beheer-cmdlets voor het gemak pipe.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Automatische back-ups plannen

U kunt back-ups automatisch gebeuren met een bepaald interval plannen. U kunt een back-upschema configureren met de cmdlet bewerken AzureRmWebAppBackupConfiguration. Deze cmdlet heeft verschillende parameters:

- **Name** - de naam van de web app.
- **ResourceGroupName** - de naam van de bronnengroep met de web app.
- - **Sleuf** - optioneel. De naam van de web app-sleuf.
- **StorageAccountUrl** - de SAS-URL voor de opslag van Azure-container gebruikt voor het opslaan van de back-ups.
- **FrequencyInterval** - numerieke waarde voor hoe vaak de back-ups moeten worden gemaakt. Moet een positief geheel getal zijn.
- **FrequencyUnit** - eenheid voor hoe vaak de back-ups moeten worden gemaakt. Opties zijn uur en dag.
- **RetentionPeriodInDays** - hoeveel dagen de automatische back-ups moeten worden opgeslagen voordat ze automatisch worden verwijderd.
- **StartTime** - optioneel. Het tijdstip waarop de automatische back-ups moeten beginnen. Back-ups worden meteen beginnen als deze leeg is. Moet u een datum/tijd.
- **Databases** - optioneel. Een matrix van DatabaseBackupSettings voor back-up van de databases.
- **KeepAtLeastOneBackup** - optionele parameter gewijzigd. Geef dit als een back-up moet altijd worden bewaard in de opslag account, ongeacht hoe oud het is.

Hier volgt een voorbeeld van het gebruik van deze cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Als u de huidige back-upschema, gebruikt u de cmdlet Get-AzureRmWebAppBackupConfiguration. Dit is handig voor het wijzigen van een schema dat al is geconfigureerd.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Een web app vanaf een back-up terugzetten

Als u een web app herstellen vanaf een back-up, gebruikt u de cmdlet terugzetten AzureRmWebAppBackup. De eenvoudigste manier om deze cmdlet gebruikt wordt om pijp in een back-up-object opgehaald uit de cmdlet Get-AzureRmWebAppBackup of de cmdlet Get-AzureRmWebAppBackupList.

Als u een back-up-object hebt, kunt u deze naar de cmdlet terugzetten AzureRmWebAppBackup pipe. De schakeloptie overschrijven om aan te geven dat u van plan bent om het te overschrijven van uw web app met de inhoud van de back-up opgeven. Als de back-up-databases bevat, worden deze databases ook hersteld.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Hier volgt een voorbeeld van het gebruik van de herstel-AzureRmWebAppBackup door te geven van alle parameters.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Een back-up verwijderen

Als u wilt een back-up verwijderen, gebruikt u de cmdlet verwijderen AzureRmWebAppBackup. Hiermee verwijdert u de back-up van uw account voor opslag. Geef de naam van uw app, de resourcegroep en de ID van de back-up die u wilt verwijderen.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

U kunt ook een back-up-object pipe naar de cmdlet verwijderen AzureRmWebAppBackup te verwijderen.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
