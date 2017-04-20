<properties
   pageTitle="Herstellen van een datawarehouse Azure SQL (PowerShell) | Microsoft Azure"
   description="PowerShell-taken voor het terugzetten van een Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Een datawarehouse Azure SQL (PowerShell) herstellen

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In dit artikel leert u het herstellen van een Azure SQL Data Warehouse met PowerShell.

## <a name="before-you-begin"></a>Voordat u begint

**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijv. myserver.database.windows.net) met een standaardquotum DTU.  Voordat u een SQL-datawarehouse terugzetten kunt, Controleer de SQL server heeft weinig DTU resterende voor de database wordt hersteld. Meer informatie over het berekenen van DTU die nodig zijn of dat er meer DTU, Zie [een quotum DTU wijziging aanvragen][].

### <a name="install-powershell"></a>PowerShell installeren

U kunt Azure PowerShell met SQL Data Warehouse gebruiken, moet u Azure PowerShell versie 1.0 of hoger installeren.  U kunt uw versie controleren door het uitvoeren van **Get Module - ListAvailable-naam AzureRM**.  De meest recente versie kan worden geïnstalleerd vanuit [Microsoft Web Platform Installer][].  Zie voor meer informatie over het installeren van de nieuwste versie [installeren en configureren van Azure PowerShell][].

## <a name="restore-an-active-or-paused-database"></a>Een actief of onderbroken database terugzetten

Een database uit een momentopname gebruik de PowerShell-cmdlet [Terugzetten AzureRmSqlDatabase][] om te zetten.

1. Open Windows PowerShell.
2. Verbinding maken met uw account Azure en overzicht van alle abonnementen die aan uw account gekoppeld.
3. Selecteer het abonnement met de database te herstellen.
4. Een overzicht van de herstelpunten voor de database.
5. Kies het gewenste herstelpunt met behulp van de RestorePointCreationDate.
6. De database herstellen naar het gewenste herstelpunt.
7. Controleer of de teruggezette database on line is.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Nadat het terugzetten is voltooid, kunt u de herstelde database met de volgende [configuratie van de database na herstel][].


## <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Om een verwijderde database terugzetten, gebruikt u de cmdlet [Terugzetten AzureRmSqlDatabase][] .

1. Open Windows PowerShell.
2. Verbinding maken met uw account Azure en overzicht van alle abonnementen die aan uw account gekoppeld.
3. Selecteer het abonnement met de verwijderde database terugzetten.
4. De specifieke verwijderde database ophalen.
5. De verwijderde database terugzetten.
6. Controleer of de teruggezette database on line is.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Nadat het terugzetten is voltooid, kunt u de herstelde database met de volgende [configuratie van de database na herstel][].


## <a name="restore-from-an-azure-geographical-region"></a>Terugzetten vanaf een Azure geografische regio

U kunt een database herstellen door de cmdlet [Terugzetten AzureRmSqlDatabase][] te gebruiken.

1. Open Windows PowerShell.
2. Verbinding maken met uw account Azure en overzicht van alle abonnementen die aan uw account gekoppeld.
3. Selecteer het abonnement met de database te herstellen.
4. Zorgen dat de database die u wilt herstellen.
5. De aanvraag voor het herstel van de database maken.
6. Controleer of de status van de database geo hersteld.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Zie de database configureren nadat het terugzetten is voltooid, [de database na herstel configureren][]. 


De herstelde database zijn TDE ingeschakeld als de brondatabase TDE ingeschakeld is.


## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over de functies van de business continuity van edities Azure SQL-Database, de [business continuity overzicht van Azure SQL-Database][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Database business continuity-overzicht]: sql-database-business-continuity.md
[Een wijziging van de quota DTU aanvragen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[De database configureren na herstel]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Het installeren en configureren van Azure PowerShell]: powershell-install-configure.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[De database configureren na herstel]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Herstel AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
