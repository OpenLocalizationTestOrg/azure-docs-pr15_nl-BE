<properties
    pageTitle="SQL-Database prestaties & opties: lagen Service | Microsoft Azure"
    description="SQL-Database prestaties en zakelijke functies voor continuïteit van de niveaus van de service voor een evenwicht tussen kosten en mogelijkheden als u schalen met elkaar vergelijken."
    keywords="databaseopties voor databaseprestaties"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-Database-opties en prestaties: begrijpen wat beschikbaar is in elke servicelaag

[Azure SQL-Database](sql-database-technical-overview.md) biedt drie Servicelagen met verschillende prestatieniveaus voor verschillende werkbelastingen. Elk prestatieniveau biedt een groeiende verzameling van hulpmiddelen die zijn ontworpen voor het leveren van steeds hogere doorvoer. U kunt elke database in een eigen [servicelaag](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) met een eigen prestatieniveau beheren. U kunt ook meerdere databases in een [groep met elastische](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) met een gedeelde set bronnen beheren. De bronnen beschikbaar voor zelfstandige databases worden uitgedrukt in termen van Database-transactie-eenheden (DTUs) en voor groepen van elastische elastische DTUs of eDTUs. Zie [Wat is een DTU](sql-database-what-is-a-dtu.md)voor meer informatie over DTUs en eDTUs. 

In beide gevallen zijn de service niveaus **Basic**, **Standard**en **Premium**. Database-opties in deze niveaus zijn vergelijkbaar voor zelfstandige databases en elastische pools, maar er zijn aanvullende overwegingen voor elastisch van toepassingen. Dit artikel bevat details van service niveaus voor zelfstandige databases en elastische pools.

## <a name="service-tiers-and-database-options"></a>Service niveaus en database-opties
Basic, Standard en Premium service niveaus hebben alle een uptime SLA van 99,99% en voorspelbare prestaties, flexibele business continuity opties, beveiligingsfuncties en facturering per uur. In de volgende tabel bevat voorbeelden van de beste niveaus die geschikt zijn voor verschillende werklasten.

| Service-laag | Werkbelasting van doel |
|---|---|
| **Basic** | Geschikt voor een kleine database ondersteunen meestal één actieve bewerking op een bepaald moment. Voorbeelden zijn databases die worden gebruikt voor de ontwikkeling of het uittesten of kleinschalige niet frequent gebruikte toepassingen. |
| **Standaard** | De optie Ga naar voor de meeste cloud-toepassingen, ondersteuning van meerdere gelijktijdige query's. Voorbeelden zijn de werkgroep- of web-toepassingen. |
| **Premium** | Ontworpen voor hoge transactionele volume veel gelijktijdige gebruikers te ondersteunen en dat het hoogste niveau van de mogelijkheden van business continuity. Voorbeelden zijn databases die bedrijfskritieke toepassingen ondersteunen. |

>[AZURE.NOTE] Web- en edities teruggetrokken. Lees de [Veelgestelde vragen over zonsondergang](https://azure.microsoft.com/pricing/details/sql-database/web-business/) als u van plan bent om door te gaan met behulp van Web- en edities.

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Zelfstandige database service niveaus en prestaties
Voor zelfstandige databases zijn er meerdere prestaties in elke servicelaag. Hebt u de flexibiliteit om het kiezen die het beste voldoet aan de eisen van uw werkbelasting. Als u omhoog of omlaag te schalen, kunt u de lagen van uw database gemakkelijk wijzigen. Zie [lagen van Database-Service wijzigen en de prestaties](sql-database-scale-up.md) voor meer informatie.

Hier vermelde prestatiekenmerken van toepassing op databases die zijn gemaakt met behulp van [SQL Database V12](sql-database-v12-whats-new.md). Ongeacht het aantal databases staan uw database haalt een gegarandeerde set bronnen en de kenmerken van de verwachte prestaties van uw database worden niet beïnvloed.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Zie voor een gedetailleerde uitleg van alle rijen in deze tabel service niveaus [laag mogelijkheden en beperkingen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Elastische pool service niveaus en prestaties in eDTUs
Behalve maken en het schalen van een zelfstandige database, moet u ook de optie voor het beheer van meerdere databases binnen een [groep met elastische](sql-database-elastic-pool.md)hebben. Alle databases in een elastische toepassingen delen een gemeenschappelijke set bronnen. De prestatiekenmerken worden gemeten door de *Elastische Database transactie-eenheden* (eDTUs). Met standalone databases, toepassingen in de drie Servicelagen komen: **Basic**, **Standard**en **Premium**. Voor groepen van toepassingen kunnen definiëren deze drie Servicelagen nog steeds de grenzen van de algehele prestaties en verschillende functies.

Deze groepen kunt u databases delen en DTU bronnen verbruiken zonder dat een specifieke prestatieniveau toewijzen aan elk van deze databases in de groep. Bijvoorbeeld gaat een afzonderlijke database in een standaard toepassingen van 0 eDTUs om de maximale eDTU die u ingesteld wanneer u de groep configureren via. Deze groepen kunt u meerdere databases met verschillende werklasten efficiënt eDTU bronnen die beschikbaar zijn met de hele groep. Zie [Overwegingen bij het prijs- prestatieverhouding voor een elastische toepassingen](sql-database-elastic-pool-guidance.md) voor meer informatie.

De volgende tabel beschrijft de kenmerken van groep service niveaus.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Elke database binnen een groep voldoet ook aan de zelfstandige database-eigenschappen voor die laag. Bijvoorbeeld, de eenvoudige toepassingen heeft een limiet voor het maximum aantal sessies per groep 4800-28800 maar een afzonderlijke database binnen een groep Basic heeft een database limiet van 300 sessies.

## <a name="choosing-a-service-tier"></a>Een servicelaag kiezen

Start om na te gaan op een servicelaag, door te bepalen of de database moet een zelfstandige database of als deel van een elastische toepassingen. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Een servicelaag voor een zelfstandige database kiezen

Start om na te gaan op een servicelaag voor een zelfstandige database, door bepaling van de databasefuncties die u nodig hebt om te kiezen uw editie van SQL-Database:

- Databasegrootte (maximaal 2 GB voor Basic voor standaard maximum van 250 GB en 500 GB tot 1 TB maximum voor Premium - afhankelijk van het prestatieniveau van de)
- Database back-periode (7 dagen voor Basic, 35 dagen voor Standard en Premium 35 dagen)

Als u de SQL-Database edition hebt vastgesteld, bent u klaar om te bepalen van het prestatieniveau van de van de database (het aantal DTUs). U kunt achterhalen en vervolgens [schalen naar boven of beneden dynamisch](sql-database-scale-up.md) op basis van werkelijke ervaring. U kunt ook de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken tot de onderlinge aanpassing van het aantal DTUs dat nodig is. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Een servicelaag voor een elastische database toepassingen kiezen.

Start om te beslissen over het niveau van de service voor een elastische database-toepassingen, databasefuncties die u moet kiezen, het niveau van de service voor uw groep te bepalen.

- Grootte van de database (2 GB voor Basic, 250 GB voor standaard en 500 GB voor Premium)
- Database back-periode (7 dagen voor Basic, 35 dagen voor Standard en Premium 35 dagen)
- Aantal databases per groep van toepassingen (400 voor Basic, 400 voor norm en 50 voor Premium)
- Maximale opslag per groep van toepassingen (117 GB voor 1200 voor standaard, Basic en 750 voor Premium)

Als u het niveau van de service hebt vastgesteld voor uw groep, bent u klaar om te bepalen van het prestatieniveau van de van de groep (eDTUs). U kunt inschatten en vervolgens [vergroten of verkleinen dynamisch](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) gebaseerd op de werkelijke ervaring. U kunt de [Rekenmachine DTU](http://dtucalculator.azurewebsites.net/) bij benadering het aantal DTUs dat nodig is voor elke database die in een groep met de bovengrens van de toepassingen bepalen.

## <a name="next-steps"></a>Volgende stappen
- Meer weten over de prijzen voor deze lagen op de [Prijzen van de SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Informatie over de details van de [elastische pools](sql-database-elastic-pool-guidance.md) en [Overwegingen voor prijs- prestatieverhouding voor elastisch van toepassingen](sql-database-elastic-pool-guidance.md).
- Meer informatie over hoe [Monitor, beheren, en het formaat van elastisch van toepassingen](sql-database-elastic-pool-manage-portal.md) en [de prestaties van zelfstandige databases Monitor](sql-database-single-database-monitor.md).
- Als u weet over de niveaus van de SQL-Database, uitproberen met een [gratis account](https://azure.microsoft.com/pricing/free-trial/) en informatie over [het maken van uw eerste SQL-database](sql-database-get-started.md).

## <a name="additional-resources"></a>Aanvullende bronnen

- [Patronen voor SaaS-toepassingen met meerdere huurder met Azure SQL-Database ontwerpen](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy video cursus Elastisch database mogelijkheden in Azure SQL-Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
