<properties
    pageTitle="Problemen met back-up en terugzetten met Azure SQL-Database"
    description="Informatie over het herstellen van een database wolk van fouten en storingen met behulp van back-ups en replica's in Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Een database herstellen naar een vorig punt in de tijd, een verwijderde database terugzetten of herstellen na een stroomstoring gegevens-center

SQL-Database, zodat u na storingen en gebruikersfouten herstellen kunt replica's van de database blijft. Beschikbare opties zijn afhankelijk van de databaselaag van de service en de opties die u kiest. Zie [Business Continuity-overzicht](sql-database-business-continuity.md) voor meer informatie en overwegingen bij het ontwerpen.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Een database terugzetten naar een vorig punt in de tijd
1.  Klik in de [Portal Azure](https://azure.microsoft.com/) **SQL-databases**.
2.  Selecteer de database in de lijst en klik op **herstellen**.
3.  Typ een nieuwe naam voor de database, kies de datum en tijd wilt terugzetten en klik op **maken.**
4.  App pas zo nodig te verwijzen naar de nieuwe database. Zie [herstellen van een database naar een punt in de tijd](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Een per ongeluk verwijderde database terugzetten
1.  Klik in de [Portal Azure](https://azure.microsoft.com/) **SQL-servers**.
2.  De server die als host van de database in de lijst selecteren.
3.  De blade Server Schuif naar beneden en klik op **Verwijderde databases**.
4.  Selecteer de database die u wilt terugzetten en klik op **maken**.
5.  App pas zo nodig te verwijzen naar de nieuwe database. Zie [een verwijderde database terugzetten](sql-database-recovery-using-backups.md#deleted-database-restore).

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Herstellen na een storing in regionale datacenter
Standard- en Premium-databases, als u secundaire servers gerepliceerd geo kunt u herstellen met behulp van deze secundaire servers. Hiermee kunt u een database met een minder kans op verlies van gegevens te herstellen. Zie [een Azure SQL-database met behulp van geautomatiseerde back-ups terugzetten](sql-database-disaster-recovery.md) voor meer informatie.

Azure biedt ook een back-up van elke database in een andere regio (een geo-redundante back-up). Vanuit deze back-ups, die wordt aangeroepen Geo terugzetten, kunt u een nieuwe database maken maar het is waarschijnlijk dat u zult er gegevens verloren gaan als u erop vertrouwen dat deze methode alleen.

**Een database met behulp van geo terugzetten herstellen:**

- In de [Portal Azure](https://azure.microsoft.com/), klikt u op **Nieuw**, klikt u op **gegevens- en**klikt u op **SQL-Database**en selecteer **back-up** als de databasebron. Zie [een Azure SQL-database uit een storing herstellen](sql-database-disaster-recovery.md) voor meer informatie.