<properties
    pageTitle="Azure SQL-Database terugzetten vanaf een geo-redundante back-up (PowerShell) | Microsoft Azure"
    description="Azure SQL-Database naar een nieuwe server van een geo-redundante back-up terugzetten"
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
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Azure SQL-Database van een geo-redundante back-up terugzetten met PowerShell


> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md)
- [Geo-terugzetten: Azure Portal](sql-database-geo-restore-portal.md)

In dit artikel wordt beschreven hoe u de database naar een nieuwe server herstellen met behulp van geo terugzetten. Dit kan worden gedaan via PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Geo-terugzetten van uw database in een zelfstandige database

1. Ophalen van de geo-redundante back-up van de database die u herstellen wilt met behulp van de [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx)-cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Het herstellen van de geo-redundante back-up starten via de [herstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx)-cmdlet.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Geo-terugzetten van uw database in een elastische database toepassingen

1. Ophalen van de geo-redundante back-up van de database die u herstellen wilt met behulp van de [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx)-cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Het herstellen van de geo-redundante back-up starten via de [herstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx)-cmdlet. Geef de naam van de toepassingen die u wilt terugzetten in de-database.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Volgende stappen

- Zie [Business continuity-overzicht](sql-database-business-continuity.md)voor een overzicht van business continuity en scenario's.
- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
- Zie informatie over het gebruik van geautomatiseerde back-ups voor herstel [herstellen van een database van back-ups service gestart](sql-database-recovery-using-backups.md).
- Zie meer informatie over herstelopties voor sneller, [Active-Geo-replicatie](sql-database-geo-replication-overview.md).  
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor archivering, [databasekopie](sql-database-copy.md).
