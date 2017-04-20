<properties 
    pageTitle="Een failover of niet gepland voor Azure SQL-Database tot stand brengen met de Azure portal | Microsoft Azure" 
    description="Het initiëren van failover of niet gepland voor Azure SQL-Database via de portal Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Een failover of niet gepland voor Azure SQL-Database tot stand brengen met de Azure portal


> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In dit artikel wordt beschreven hoe u failover naar een secundaire SQL-Database met [Azure portal](http://portal.azure.com)te starten. Zie Geo-replicatie configureren [Geo-replicatie configureren voor Azure SQL-Database](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Een failover-

De secundaire database kan worden ontstoken tot de primaire.  

1. Blader in het [Azure portal](http://portal.azure.com) naar de primaire database in het partnerschap Geo-replicatie.
2. Selecteer **alle instellingen**op de SQL-Database-blade > **Geo-replicatie**.
3. Selecteer in de lijst met **secundaire servers voor** de database die u wilt de nieuwe primaire en **Failover**op.

    ![failover][2]

4. Klik op **Ja** om te beginnen met de overname.

De opdracht andere secundaire database onmiddellijk aan de primaire rol. 

Er is een korte periode waarin beide databases niet beschikbaar (volgorde van 0 tot en met 25 seconden zijn) tijdens de rollen worden getransporteerd. Als de primaire database meerdere secundaire databases heeft, wordt de opdracht automatisch de andere secundaire verbinding maken met de nieuwe primaire servers configureren. De hele bewerking moet minder dan een minuut in beslag onder normale omstandigheden duren. 

>[AZURE.NOTE] Als de primaire on line is en doorvoeren van transacties wanneer de opdracht wordt gegeven tot gegevensverlies kan optreden.


## <a name="next-steps"></a>Volgende stappen   

- Controleer dat de vereisten voor de server en de database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging voor SQL-Database na noodherstel](sql-database-geo-replication-security-config.md)voor meer informatie.
- Zie voor meer herstellen na een ramp met actieve Geo-replicatie, inclusief pre en boeken van de procedure en het uitvoeren van een detailanalyse disaster recovery, [Herstel na storing herstel oefeningen](sql-database-disaster-recovery.md)
- Zie voor een blogbericht Sasha Nosov over actieve Geo-replicatie [spotlicht op de nieuwe mogelijkheden van Geo-replicatie](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Zie voor meer informatie over het ontwerpen van cloud-toepassingen kunnen gebruikmaken van actieve Geo-replicatie [cloud-toepassingen ontwerpen voor bedrijfscontinuïteit Geo - replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Zie voor meer informatie over het gebruik van actieve Geo-replicatie met elastische database toepassingen [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zie voor een overzicht van continurity business, [Business Continuity-overzicht](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
