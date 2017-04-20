<properties
    pageTitle="Herstel verwijderde Azure SQL-database (Azure portal) | Microsoft Azure"
    description="Herstel verwijderde Azure SQL-database (Azure portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Een verwijderde Azure SQL-database via de Portal Azure terugzetten

> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md)
- [**Verwijderde herstellen DB: Portal**](sql-database-restore-deleted-database-portal.md)
- [Verwijderde herstellen DB: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Selecteer de database terugzetten 

Een verwijderde database in Azure portal terugzetten:

1.  Klik op **meer services**in [Azure portal](https://portal.azure.com) > **SQL-servers**.
3.  Selecteer de server met de database die u wilt terugzetten.
4.  Ga naar de sectie **bewerkingen** van uw server-blade en **Verwijderde databases**selecteren: ![Azure SQL-database terugzetten](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Selecteer de database die u wilt terugzetten.
6.  Geef een databasenaam en klik op **OK**:

    ![Azure SQL-database terugzetten](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie informatie over automatische back-ups gebruikt voor herstel, [een database van de service gestart back-ups terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over herstelopties voor sneller [Actief-Geo-replicatie](sql-database-geo-replication-overview.md)  
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor het archiveren van [kopie](sql-database-copy.md)
