<properties
    pageTitle="Azure SQL-database herstellen naar een vorig punt in tijd (Azure portal) | Microsoft Azure"
    description="Azure SQL-database herstellen naar een vorig punt in de tijd."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Azure SQL-database herstellen naar een vorig punt in de tijd met Azure portal


> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md)
- [Restore Point-In-Time: PowerShell](sql-database-point-in-time-restore-powershell.md)

In dit artikel wordt beschreven hoe u de database te herstellen naar een eerder punt in de tijd van [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md) via de portal Azure.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Een SQL-database herstellen naar een vorig punt in de tijd

Selecteer een database om te zetten in Azure portal:

1.  Open de [Azure portal](https://portal.azure.com).
2.  Aan de linkerkant van het scherm, selecteer **meer services** > **SQL-databases**.
3.  Klik op de database die u wilt terugzetten.
4.  Selecteer **terugzetten**aan de bovenkant van de pagina van uw database:

    ![Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Selecteer de datum en tijd (UTC-tijd) de database wilt terugzetten op de pagina **herstellen** en klik op **OK**:

    ![Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Controleren de bewerking voor terugzetten

1. Wanneer u op **OK** in de vorige stap, klikt u op het pictogram in de rechterbovenhoek van de pagina en klik op de melding van het **terugzetten van SQL-database** voor meer informatie.

    ![Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. De pagina herstellen SQL-database wordt geopend met informatie over de status van de herstelbewerking. U kunt het-artikel voor meer informatie:

    ![Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie informatie over automatische back-ups gebruikt voor herstel, [een database van de service gestart back-ups terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over herstelopties voor sneller [Actief-Geo-replicatie](sql-database-geo-replication-overview.md)  
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor het archiveren van [kopie](sql-database-copy.md)
