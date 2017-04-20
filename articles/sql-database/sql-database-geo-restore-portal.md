<properties
    pageTitle="Azure SQL-database terugzetten vanaf een automatische back-up (Azure portal) | Microsoft Azure"
    description="Azure SQL-database terugzetten vanaf een automatische back-up (Azure portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Azure SQL-database terugzetten vanaf een automatische back-up met behulp van de portal Azure


> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md#geo-restore)
- [Geo-terugzetten: PowerShell](sql-database-geo-restore-powershell.md)

In dit artikel wordt beschreven hoe u de database te herstellen van een [Automatische back-up](sql-database-automated-backups.md) naar een nieuwe server met [Geo terugzetten](sql-database-recovery-using-backups/.md#geo-restore) met behulp van de portal Azure.

## <a name="select-a-database-to-restore"></a>Selecteer een database terugzetten

Als u een database in de portal Azure herstellen, moet u de volgende stappen uitvoeren:

1.  Ga naar de [portal Azure](https://portal.azure.com).
2.  Selecteer aan de linkerkant van het scherm **+ nieuwe** > **Databases** > **SQL-Database**:

    ![Azure SQL-database terugzetten](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Selecteer **back-up** als bron, en selecteer vervolgens de back-up die u wilt terugzetten. Geef de naam van een database, een server die u wilt terugzetten van de database in en klik op **maken**:
  
    ![Azure SQL-database terugzetten](./media/sql-database-geo-restore-portal/geo-restore.png)

Controleer de status van de bewerking voor terugzetten door te klikken op het pictogram in de rechterbovenhoek van de pagina. 


## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie informatie over automatische back-ups gebruikt voor herstel, [een database van de service gestart back-ups terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over herstelopties voor sneller [Actief-Geo-replicatie](sql-database-geo-replication-overview.md)  
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor het archiveren van [kopie](sql-database-copy.md)
