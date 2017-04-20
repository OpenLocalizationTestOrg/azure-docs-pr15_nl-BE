<properties
    pageTitle="Azure SQL Database Resource limieten"
    description="Deze pagina beschrijft enkele algemene resource limieten voor Azure SQL-Database."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Azure SQL-Database resource limieten

## <a name="overview"></a>Overzicht

Azure SQL-Database beheert de bronnen die beschikbaar zijn in een database met behulp van twee verschillende mechanismen: **Resources Governance** en **Handhaving van limieten**. Dit onderwerp wordt uitgelegd dat deze twee hoofdgebieden van bronbeheer.

## <a name="resource-governance"></a>Resource governance
Een van de doelstellingen van het ontwerp van de Basic-, Standard- en Premium-service de onderlaag is voor Azure SQL-Database als de database wordt uitgevoerd op de eigen computer, volledig geïsoleerd van andere databases werkt. Resource governance wordt geëmuleerd voor dit probleem. Als het geaggregeerde Resourcegebruik bereikt de maximaal beschikbare CPU, geheugen, I/O logboek en i/o-gegevens-bronnen toegewezen aan de database, wordt de resource governance query's wordt uitgevoerd in de wachtrij en resources toewijzen aan de query's die in de wachtrij als ze vrij te maken.

Als op een specifieke computer gebruikmaken van alle beschikbare middelen, resulteert in een langere uitvoering van query's leiden time-outs voor de opdracht op de client tot kunnen dat momenteel wordt uitgevoerd. Met agressieve opnieuw logica en toepassingen die het uitvoeren van query's op de database met een hoge frequentie, kunnen er foutberichten tijdens het uitvoeren van nieuwe query's wanneer het maximum aantal gelijktijdige aanvragen is bereikt.

### <a name="recommendations"></a>Aanbevelingen:
Het brongebruik, evenals de gemiddelde responstijden van query's controleren wanneer bijna de maximale capaciteit van een database. Wanneer er hogere query vertragingstijden in het algemeen hebt u drie opties:

1.  Minder van binnenkomende aanvragen in de database-out en het stapeltje van aanvragen te voorkomen.

2.  Een hoger prestatieniveau aan de database toewijzen.

3.  Query's om het gebruik van bronnen van elke query wordt geoptimaliseerd. Zie het gedeelte Query afstemmen/Hinting in de Azure SQL Database prestaties artikel voor meer informatie.

## <a name="enforcement-of-limits"></a>Handhaving van limieten
Bronnen buiten de CPU, geheugen, I/O logboek en i/o-gegevens worden afgedwongen door de nieuwe aanvragen te weigeren wanneer de limiet wordt bereikt. Clients ontvangen een [foutbericht wordt weergegeven](sql-database-develop-error-messages.md) , afhankelijk van de limiet is bereikt.

Het aantal verbindingen met een SQL-database en het aantal gelijktijdige aanvragen dat kan worden verwerkt, worden zo beperkt. SQL-Database kunt het aantal verbindingen met de database groter is dan het aantal gelijktijdige aanvragen ter ondersteuning van groepsgewijze verbindingen. Het bedrag van de verbindingen die beschikbaar zijn, kan gemakkelijk worden beheerd door de toepassing, is het bedrag van de parallelle aanvragen vaak keer moeilijker in te schatten en te bepalen. Met name tijdens piekbelastingen wanneer de toepassing ofwel te veel aanvragen of de database bereikt de resource verzendt beperkt en begint opstapelen worker-threads meer query's uitgevoerd als gevolg van kunnen fouten worden opgetreden.

## <a name="service-tiers-and-performance-levels"></a>Service niveaus en prestaties

Er zijn service niveaus en prestatieniveaus voor beide groepen van zelfstandige database en elastisch.

### <a name="standalone-databases"></a>Standalone databases

Voor een zelfstandige database, worden de grenzen van een database gedefinieerd door de database laag en prestaties serviceniveau. De volgende tabel beschrijft de kenmerken van Basic, Standard en Premium databases op verschillende prestatieniveaus.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Elastische pools

[Elastische toepassingen](sql-database-elastic-pool.md) delen bronnen in verschillende databases in de groep. De volgende tabel beschrijft de kenmerken van Basic, Standard en Premium elastische database-toepassingen.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Zie voor een uitgebreide omschrijving van elke resource die in de vorige tabel, de beschrijvingen in de [laag mogelijkheden en beperkingen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Zie voor een overzicht van service niveaus [Azure SQL Database Service Tiers en prestaties](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>De grenzen van andere SQL-Database

| Gebied | Limiet | Beschrijving |
|---|---|---|
| Databases die gebruikmaken van automatische export per abonnement | 10 | Geautomatiseerde exporteren kunt u een aangepast schema voor back-ups van SQL-databases maken. Zie voor meer informatie [SQL-Databases: ondersteuning voor de uitvoer van SQL-Database automatisch](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Database per server | Maximaal 5000 | Tot 5000 databases zijn toegestaan per server op V12-servers. |  
| DTUs per server | 45000 | 45000 DTUs zijn per server op V12 servers voor elastisch van toepassingen en datawarehouses van provisioning databases beschikbaar. |



## <a name="resources"></a>Bronnen

[Azure abonnement en grenzen van de Service, quota's en beperkingen](../azure-subscription-service-limits.md)

[Azure SQL Database Service Tiers en prestaties](sql-database-service-tiers.md)

[Foutberichten voor SQL-Database-clientprogramma 's](sql-database-develop-error-messages.md)
