<properties 
    pageTitle="Het initiëren van failover of niet gepland voor Azure SQL-Database met PowerShell | Microsoft Azure" 
    description="Het initiëren van failover of niet gepland voor Azure PowerShell met SQL-Database" 
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
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Het initiëren van failover of niet gepland voor Azure SQL-Database met PowerShell



> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In dit artikel wordt beschreven hoe u een failover of niet gepland voor SQL-Database met PowerShell starten. Zie Geo-replicatie configureren [Geo-replicatie configureren voor Azure SQL-Database](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Een geplande failover-

Gebruik de cmdlet **Set-AzureRmSqlDatabaseSecondary** met de parameter **- Failover** ter bevordering van een secundaire database tot de nieuwe primaire database degradatie van de bestaande primaire tot een secundair. Deze functionaliteit is ontworpen voor een geplande failover, zoals tijdens de oefeningen disaster recovery, en vereist dat de primaire database beschikbaar zijn.

De opdracht wordt uitgevoerd de volgende workflow:

1. Replicatie tijdelijk overschakelen naar de synchrone modus. Hierdoor worden alle openstaande transacties moeten worden afgeboekt op de secundaire.

2. Omwisselen van de twee databases in het partnerschap voor de Geo-replicatie.  

Deze reeks zorgt ervoor dat de twee databases worden gesynchroniseerd voordat de rollen gaan en daarom geen gegevens verloren gaan zullen. Er is een korte periode waarin beide databases niet beschikbaar (volgorde van 0 tot en met 25 seconden zijn) tijdens de rollen worden getransporteerd. De hele bewerking moet minder dan een minuut in beslag onder normale omstandigheden duren. Zie [Set AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx)voor meer informatie.




Deze cmdlet retourneert wanneer het proces voor het activeren van de secundaire database primaire is voltooid.

Met de volgende opdracht schakelt u de functies van de database met de naam 'mijndb' op de server "srv2" onder de resource-groep "rg2" primaire. De oorspronkelijke primaire "db2" was verbonden aan wordt overgeschakeld naar de secundaire nadat de twee databases volledig gesynchroniseerd.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] In zeldzame gevallen is het mogelijk dat de bewerking kan niet worden voltooid en niet meer reageert lijkt. In dit geval de gebruiker de werking failover-opdracht (niet-geplande failover) aanroepen en verlies van gegevens te accepteren.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Een niet-geplande failover van de primaire database op de secundaire database starten


Kunt u de cmdlet **Set-AzureRmSqlDatabaseSecondary** met **Failover-** en **AllowDataLoss -** parameters ter bevordering van een secundaire database tot de nieuwe primaire database op een niet-geplande wijze, dat de degradatie van de bestaande primaire een secundair worden op een tijdstip waarop de primaire database niet meer beschikbaar is.

Deze functionaliteit is ontworpen voor noodherstel, wanneer de beschikbaarheid van de database terugzetten is essentieel en er gegevens verloren gaan, is aanvaardbaar. Wanneer gedwongen failover wordt aangeroepen, de opgegeven secundaire database onmiddellijk de primaire database en begint schrijftransacties accepteren. Zodra de oorspronkelijke primaire database, kan met deze nieuwe primaire database herstellen na de gedwongen failover-bewerking, een incrementele back-up wordt genomen op de oorspronkelijke primaire database en de oude primaire database wordt gemaakt in een secundaire database voor de nieuwe primaire database. het is slechts een replica van de nieuwe primaire.

Maar omdat het punt In tijd herstellen wordt niet ondersteund op secundaire databases, als u wilt herstellen gegevens vastgelegd in de oude primaire database die was niet zijn gerepliceerd naar de nieuwe primaire database, u CSS naar een database terugzet op de bekende back-up moet worden benaderd.

> [AZURE.NOTE] Als de opdracht wordt gegeven wanneer de primaire en secundaire zijn online de oude primaire komen de nieuwe secundaire onmiddellijk zonder te synchroniseren. Transacties doorgevoerd wanneer de opdracht wordt gegeven tot gegevensverlies kunnen optreden als de primaire is.


Als de primaire database meerdere secundaire servers voor die de opdracht slaagt gedeeltelijk heeft. De secundaire waarop de opdracht is uitgevoerd, worden primaire. De oude primaire echter blijven primaire, dat wil zeggen de twee primaire kleuren uiteindelijk in een inconsistente toestand en met elkaar verbonden door een replicatiekoppeling onderbroken. De gebruiker moet deze configuratie met behulp van een API 'secundaire verwijderen' op elk van beide databases primaire handmatig herstellen.


De volgende opdracht schakelt u de functies van de database met de naam 'mijndb' primaire wanneer de primaire niet beschikbaar is. De oorspronkelijke primaire 'mijndb' was verbonden aan wordt overgeschakeld naar de secundaire nadat deze weer on line is. Op dat moment de synchronisatie kan leiden tot verlies van gegevens.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Volgende stappen   

- Controleer dat de vereisten voor de server en de database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging voor SQL-Database na noodherstel](sql-database-geo-replication-security-config.md)voor meer informatie.
- Zie voor meer herstellen na een ramp met actieve Geo-replicatie, inclusief pre en boeken van de procedure en het uitvoeren van een detailanalyse disaster recovery, [Herstel na storing herstel oefeningen](sql-database-disaster-recovery.md)
- Zie voor een blogbericht Sasha Nosov over actieve Geo-replicatie [spotlicht op de nieuwe mogelijkheden van Geo-replicatie](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Zie voor meer informatie over het ontwerpen van cloud-toepassingen kunnen gebruikmaken van actieve Geo-replicatie [cloud-toepassingen ontwerpen voor bedrijfscontinuïteit Geo - replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Zie voor meer informatie over het gebruik van actieve Geo-replicatie met elastische database toepassingen [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zie voor een overzicht van continurity business, [Business Continuity-overzicht](sql-database-business-continuity.md)
