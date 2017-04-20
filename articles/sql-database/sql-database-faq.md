<properties 
   pageTitle="Veelgestelde vragen over Azure SQL-Database" 
   description="Antwoorden op algemene vragen klanten vragen over cloud databases en Azure SQL-Database, Microsoft relationeel databasebeheersysteem (RDBMS) en de database als een service in de cloud." 
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
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>Veelgestelde vragen over SQL-Database

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Hoe is het gebruik van SQL-Database te zien op mijn factuur? 
Rekeningen van de SQL-Database op een voorspelbare uurtarief op basis van de servicelaag + prestatieniveau voor enkele databases of eDTUs per elastische database-toepassingen. Werkelijke verbruik wordt berekend en de pro rato berekend per uur, zodat u uw factuur kan fracties van een uur. Als een database voor 12 uur in een maand, ziet uw factuur gebruik van 0,5 dagen. Service tiers + prestatieniveau en eDTUs per groep van toepassingen bovendien zijn opgesplitst in de stuklijst gemakkelijker te zien van het aantal dagen van database die u voor elk in één maand gebruikt.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Wat gebeurt er als één enkele database voor minder dan een uur actief is of gebruikmaakt van een hoger niveau van service voor minder dan een uur?
Afschrijving voor elk uur bestaat in een database met behulp van het hoogste niveau van service + prestatieniveau die tijdens dat uur, ongeacht het gebruik of de of de database actief is voor minder dan een uur worden toegepast. Als u één database maakt en verwijdert u deze vijf minuten later weerspiegelt uw factuur een toeslag voor de database voor één uur. 

Voorbeelden
    
- Als u een eenvoudige database maken en vervolgens direct naar standaard S1 upgraden, u in rekening worden gebracht tegen het tarief van de standaard S1 voor het eerste uur.

- Als u een database van Basic naar Premium om 10:00 uur bijwerken en de upgrade is voltooid om 1:35 uur de volgende dag u in rekening worden gebracht op de premie vanaf 1:00 uur 

- Als u een database van Premium Basic om 11:00 uur downgraden en om 14:15 uur is voltooid, wordt de database in rekening wordt gebracht op de premie tot 3:00 uur, waarna wordt belast tegen de standaard tarieven.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Hoe elastische database toepassingen gebruik weergegeven op mijn rekening en wat gebeurt er als ik de eDTUs per groep?
Elastische database groep toeslagen weergegeven op uw rekening als elastische DTUs (eDTUs) in de stappen onder de eDTUs per groep van toepassingen op [de pagina prijzen](https://azure.microsoft.com/pricing/details/sql-database/). Er zijn geen kosten per database voor elastische database-toepassingen. Afschrijving voor elk uur die een groep bestaat op de hoogste eDTU, ongeacht het gebruik of de of de groep actief is voor minder dan een uur. 

Voorbeelden

- Als u een groep standaard elastische database met 200 eDTUs om 11:18 uur maakt, vijf databases toe te voegen aan de groep van toepassingen, u in rekening worden gebracht voor 200 eDTUs voor het hele uur vanaf 11: 00 tot en met de rest van de dag.
- Op dag 2, 5:05 uur 1 Database begint 50 eDTUs verbruikt en bezit gestaag tot en met de dag. 2-5 databases schommelen tussen 0 en 80 eDTUs. Gedurende de dag, voeg toe vijf andere databases, die verschillende eDTUs gedurende de dag in beslag nemen. Dag 2 is een volledige dag op 200 eDTU in rekening gebracht. 
- Op dag 3, 5 uur u toevoegen een andere 15 databases. Database-gebruik neemt toe gedurende de dag naar het punt waar u wilt eDTUs voor de toepassingen van 200 tot 400 verhogen om 8:05 uur Toeslagen op het niveau van 200 eDTU van kracht waren tot 20 h 00 en loopt op tot 400 eDTUs voor de resterende vier uur. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Hoe is het gebruik van actieve Geo-replicatie in een database met elastische toepassingen te zien op mijn factuur?
In tegenstelling tot enkele databases heeft [Active Geo-replicatie](sql-database-geo-replication-overview.md) gebruiken met elastische databases geen directe gevolgen voor facturering.  U worden alleen berekend voor de eDTUs ingericht voor elk van de toepassingen (toepassingen primaire en secundaire toepassingen)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Hoe is het gebruik van de controlefunctie van invloed op mijn factuur? 
Controle is ingebouwd in de SQL-Database service zonder extra kosten en is beschikbaar voor databases Basic, Standard en Premium. Echter voor het opslaan van de controlelogboeken de controle functie gebruikt die een account Azure opslag en tarieven voor tabellen en wachtrijen in Azure opslag van toepassing zijn op basis van de grootte van het controlelogboek.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Hoe vind ik het juiste niveau en de prestaties serviceniveau voor enkele databases en elastische database toepassingen 
Er zijn enkele hulpmiddelen beschikbaar zijn. 

- Gebruik [DTU sizing advisor](http://dtucalculator.azurewebsites.net/) aan te bevelen de databases en DTUs vereist voor databases voor gebouwen, en meerdere databases voor elastische database van toepassingen te beoordelen.
- Als u één database zou komen in een groep, raadt intelligente motor van Azure een elastische database toepassingen als er een historische patroon dat gerechtvaardigd. Zie [controleren en beheren van een elastische database toepassingen met Azure portal](sql-database-elastic-pool-manage-portal.md). Zie voor meer informatie over hoe u kunt doen de wiskunde zelf [prijs- prestatieverhouding overwegingen voor een elastische database toepassingen](sql-database-elastic-pool-guidance.md)
- Zie [richtlijnen voor prestaties voor enkele databases](sql-database-performance-guidance.md)of moet u één database kiezen omhoog of omlaag.

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Hoe vaak kan ik het serviceniveau voor laag of de prestaties van een enkele database wijzigen? 
V12-databases, kunt u de servicelaag (tussen Basic, Standard en Premium) of het prestatieniveau in een servicelaag (bijvoorbeeld S1 aan S2) zo vaak als u wilt. Voor databases uit eerdere versies, kunt u de laag of het serviceniveau in totaal vier keer in een periode van 24 uur.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Hoe vaak kan ik de eDTUs per groep van toepassingen aanpassen? 
Zo vaak als u wilt.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Hoe lang duurt het voordat het serviceniveau voor laag of de prestaties van een enkele database wijzigen of verplaatsen van een database en een elastische database-toepassingen? 
De servicelaag van een database wijzigen en verplaatsen naar een groep moet de database worden gekopieerd op het platform als achtergrond. Wijzigen van de opgegeven service kan enkele minuten duren tot enkele uren, afhankelijk van de grootte van de databases. In beide gevallen wordt de databases on line blijven en beschikbaar tijdens het verplaatsen. Zie voor meer informatie over het wijzigen van afzonderlijke databases [wijzigen de servicelaag van een database](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Wanneer gebruikt u één database versus elastische databases? 
In het algemeen Elastisch database toepassingen zijn ontworpen voor een typisch [patroon van software als een service (SaaS)-toepassing](sql-database-design-patterns-multi-tenancy-saas-applications.md), waarbij één database per klant of huurder. Inkoop van afzonderlijke databases en overprovisioning om te voldoen aan de variabele en pieksnelheden vraag voor elke database is vaak niet kosten efficiënt. Beheren van de collectieve prestaties van de groep van toepassingen, en de databases schalen automatisch omhoog en omlaag. 

Intelligente motor van Azure raadt u aan een groep van toepassingen voor databases wanneer een gebruikspatroon gerechtvaardigd. Zie de [SQL-Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md)voor meer informatie. Zie voor gedetailleerde richtlijnen over het kiezen tussen één en elastische [Overwegingen voor prijs- prestatieverhouding voor elastische database-toepassingen](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Wat betekent het hebben van 200% van de maximale ingerichte database opslagruimte voor back-up opslaan? 
Back-up opslaan is de opslaglocatie die is gekoppeld aan uw database voor geautomatiseerde back-ups die worden gebruikt voor [Point-In-Time-Restore](sql-database-recovery-using-backups.md#-point-in-time-restore) en [Geo terugzetten](sql-database-recovery-using-backups.md#geo-restore). SQL-Database van Microsoft Azure biedt tot 200% van de maximale ingerichte database-opslag van back-up opslaan zonder extra kosten. Bijvoorbeeld als u een standaard DB exemplaar met een ingerichte DB grootte van 250 GB, worden u voorzien van 500 GB van de back-up opslaan zonder extra kosten. Als uw database groter is dan de opgegeven back-up opslaan, kunt u de bewaarperiode verminderen door contact opnemen met ondersteuning van Azure of betalen voor de extra back-up opslaan op standaardtarief leestoegang tot geografisch redundante opslag (RA-GRS) in rekening gebracht. Zie voor meer informatie over facturering RA GRS opslag prijzen Details.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Ik ga verhuizen van Web/Business in de nieuwe service niveaus, wat moet ik weten?
Azure SQL Web en bedrijfsdatabases nu teruggetrokken. Voor de niveaus Basic, Standard, Premium en elastische vervangen door de retiring Web- en databases. We hebben meer veelgestelde vragen die u in deze overgangsperiode helpen kunnen. [Web- en Business Edition zonsondergang Veelgestelde vragen](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Wat is een replicatie verwachte vertraging bij het repliceren van geo een database tussen de twee regio's binnen de dezelfde Azure Geografie?  
We zijn momenteel ondersteunt een vrijgegeven Productieorder van vijf seconden en de vertragingstijd voor replicatie is kleiner dan dat wanneer de geo-secundaire wordt gehost in de Azure gepaarde regio aanbevolen en op hetzelfde niveau van de service.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Wat is een replicatie verwachte vertraging als geo-secundair wordt gemaakt in dezelfde regio als de primaire database?  
Op basis van empirische gegevens, is er niet te veel verschil tussen intra-regio en de vertragingstijd voor replicatie tussen de regio als de Azure aanbevolen gepaarde regio wordt gebruikt. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Als er een netwerkstoring tussen de twee regio's, hoe de logica opnieuw werkt als Geo-replicatie is ingesteld?  
Als er een verbinding verbreken, proberen we elke 10 seconden om verbindingen opnieuw tot stand te brengen.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Wat kan ik doen om ervoor te zorgen dat een belangrijke wijziging in de primaire database is gerepliceerd?
De geo-secundaire is een asynchrone replica en we niet proberen om het volledige synchroniseren met de primaire. Maar we bieden een methode om de synchronisatie te waarborgen van de replicatie van belangrijke wijzigingen (bijvoorbeeld wachtwoord updates) afdwingen. Geforceerde synchronisatie van invloed op prestaties omdat de thread worden geblokkeerd totdat alle doorgevoerde transacties worden gerepliceerd. Zie [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx)voor meer informatie. 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Welke hulpmiddelen zijn beschikbaar voor het controleren van de vertragingstijd voor replicatie tussen de primaire database en geo-secundair?
We blootstellen de vertraging realtime replicatie tussen de primaire database en geo-secundair via een DMV. Zie [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx)voor meer informatie.
