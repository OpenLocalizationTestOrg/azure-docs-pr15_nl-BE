<properties 
    pageTitle="Initiëren van failover of niet gepland voor SQL-Database met Transact-SQL Azure | Microsoft Azure" 
    description="Het initiëren van failover of niet gepland voor Azure SQL-Database met behulp van Transact-SQL" 
    services="sql-database" 
    documentationCenter="" 
    authors="CarlRabeler" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management"
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Het initiëren van failover of niet gepland voor Azure SQL-Database met Transact-SQL


> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In dit artikel wordt beschreven hoe u failover naar een secundaire SQL-Database starten met behulp van Transact-SQL. Zie Geo-replicatie configureren [Geo-replicatie configureren voor Azure SQL-Database](sql-database-geo-replication-transact-sql.md).



Om te beginnen de failover, moet u het volgende:

- Een aanmelding die in de primaire, DBManager hebben db_ownership van de lokale database dat u geo-repliceren zult en DBManager op de partner (s) waarop u Geo-replicatie configureren.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Een geplande failover-bevordering van een secundaire database tot de nieuwe primaire

U kunt de instructie **ALTER DATABASE** een secundaire database tot de nieuwe primaire database in een geplande manier bevorderen degradatie van de bestaande primaire tot een secundair. Deze instructie is uitgevoerd op de master-database op de Azure SQL Database logische server waar de geo gerepliceerd secundaire database is gepromoveerd woont. Deze functionaliteit is ontworpen voor geplande failover, zoals tijdens de oefeningen DR en vereist dat de primaire database beschikbaar zijn.

De opdracht wordt uitgevoerd de volgende workflow:

1. Replicatie wordt tijdelijk overgeschakeld op de synchrone modus, waardoor alle openstaande transacties moeten worden afgeboekt op het secundaire en het blokkeren van alle nieuwe transacties;

2. Schakelopties voor de rollen van de twee databases in het partnerschap voor de Geo-replicatie.  

Deze reeks zorgt ervoor dat de twee databases worden gesynchroniseerd voordat de rollen gaan en daarom geen gegevens verloren gaan zullen. Er is een korte periode waarin beide databases niet beschikbaar (volgorde van 0 tot en met 25 seconden zijn) tijdens de rollen worden getransporteerd. Als de primaire database meerdere secundaire databases heeft, wordt de opdracht automatisch de andere secundaire verbinding maken met de nieuwe primaire servers configureren.  De hele bewerking moet minder dan een minuut in beslag onder normale omstandigheden duren. Voor meer informatie Zie [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) en [Service Tiers](sql-database-service-tiers.md).


Gebruik de volgende stappen uit om een geplande failover.

1. Verbinding maken met de logische Azure SQL-Database-server waarin een secundaire geo gerepliceerd-database zich bevindt in Management Studio.

2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Met de volgende instructie **ALTER DATABASE** kunt u de secundaire database aan de primaire rol.

        ALTER DATABASE <MyDB> FAILOVER;

4. Klik op **uitvoeren** als de query wilt uitvoeren.

>[AZURE.NOTE] In zeldzame gevallen is het mogelijk dat de bewerking kan niet worden voltooid en het alsof zitten lijkt. In dit geval de gebruiker de opdracht force failover uitgevoerd en verlies van gegevens te accepteren.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Een niet-geplande failover van de primaire database op de secundaire database starten

U kunt de instructie **ALTER DATABASE** een secundaire database tot de nieuwe primaire database op een niet-geplande wijze bevorderen dat de degradatie van de bestaande primaire een secundair worden op een tijdstip waarop de primaire database niet meer beschikbaar is. Deze instructie is uitgevoerd op de master-database op de Azure SQL Database logische server waar de geo gerepliceerd secundaire database is gepromoveerd woont.

Deze functionaliteit is ontworpen voor noodherstel, wanneer de beschikbaarheid van de database terugzetten is essentieel en er gegevens verloren gaan, is aanvaardbaar. Wanneer gedwongen failover wordt aangeroepen, de opgegeven secundaire database onmiddellijk de primaire database en begint schrijftransacties accepteren. Zodra de oorspronkelijke primaire database meer is tot stand brengen met deze nieuwe primaire database, een incrementele back-up wordt genomen op de oorspronkelijke primaire database en de oude primaire database wordt gemaakt in een secundaire database voor de nieuwe primaire database. het is dan ook alleen een synchronisatie replica van de nieuwe primaire.

Echter omdat het punt In tijd herstellen niet wordt ondersteund op de secundaire databases als de gebruiker wenst te herstellen van gegevens die zijn vastgelegd in de oude primaire database die was niet zijn gerepliceerd naar de nieuwe primaire database voordat de geforceerde plaatsgevonden, moet de gebruiker verrichten ondersteuning dit verloren gegevens te herstellen.

Als de primaire database meerdere secundaire databases heeft, wordt de opdracht automatisch de andere secundaire verbinding maken met de nieuwe primaire servers configureren.

De volgende stappen gebruiken om een niet-geplande failover.

1. Verbinding maken met de logische Azure SQL-Database-server waarin een secundaire geo gerepliceerd-database zich bevindt in Management Studio.

2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Met de volgende instructie **ALTER DATABASE** kunt u de secundaire database aan de primaire rol.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Klik op **uitvoeren** als de query wilt uitvoeren.

>[AZURE.NOTE] Als de opdracht wordt gegeven wanneer de primaire en secundaire zijn online de oude primaire komen de nieuwe secundaire onmiddellijk zonder te synchroniseren. Transacties doorgevoerd wanneer de opdracht wordt gegeven tot gegevensverlies kunnen optreden als de primaire is.



## <a name="next-steps"></a>Volgende stappen   

- Controleer dat de vereisten voor de server en de database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging voor SQL-Database na noodherstel](sql-database-geo-replication-security-config.md)voor meer informatie.
- Zie [Noodherstel](sql-database-disaster-recovery.md) voor meer herstellen na een ramp met actieve Geo-replicatie, inclusief pre en boeken van de procedure en het uitvoeren van een detailanalyse disaster recovery,
- Zie voor een blogbericht Sasha Nosov over actieve Geo-replicatie [spotlicht op de nieuwe mogelijkheden van Geo-replicatie](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Zie voor meer informatie over het ontwerpen van cloud-toepassingen kunnen gebruikmaken van actieve Geo-replicatie [cloud-toepassingen ontwerpen voor bedrijfscontinuïteit Geo - replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Zie voor meer informatie over het gebruik van actieve Geo-replicatie met elastische database toepassingen [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zie voor een overzicht van continurity business, [Business Continuity-overzicht](sql-database-business-continuity.md)
