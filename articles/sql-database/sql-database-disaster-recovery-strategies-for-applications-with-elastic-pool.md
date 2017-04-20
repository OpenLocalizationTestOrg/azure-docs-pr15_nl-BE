<properties 
   pageTitle="Cloud-oplossingen voor Disaster Recovery SQL Database Geo-replicatie met ontwerpen | Microsoft Azure"
   description="Informatie over het kiezen van de juiste failover-patroon voor het ontwerpen van uw cloud oplossing voor noodherstel."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Disaster recovery strategieën voor toepassingen met behulp van SQL Database elastische Pool 

In de jaren die hebt u geleerd cloud-services zijn niet waterdicht en ernstige incidenten kan en zal gebeuren. SQL-Database biedt een aantal mogelijkheden te bieden voor de bedrijfscontinuïteit van uw toepassing, wanneer deze incidenten zich voordoen. [Elastische pools](sql-database-elastic-pool.md) en standalone databases ondersteunen dezelfde soort disaster recovery mogelijkheden. Dit artikel beschrijft verschillende DR strategieën voor elastische groepen gebruikmaken van deze functies van SQL-Database business continuity.

Voor de toepassing van dit artikel gebruiken we de canonieke patroon van SaaS ISV-toepassing:

<i>Een moderne wolk gebaseerd web application bepalingen een SQL-database voor elke gebruiker. De ISV heeft een groot aantal klanten en daarom gebruikt veel databases, databases huurder genoemd. Omdat de huurder databases meestal activiteit onvoorspelbare patronen hebben, de ISV een elastische toepassingen gebruikt de database te maken kosten zeer voorspelbare gedurende langere tijd. De elastische pool vereenvoudigt ook het prestatiebeheer wanneer de gebruikersactiviteit bereikt. Naast de huurder databases gebruikt de toepassing ook verschillende databases beheren van gebruikersprofielen, beveiliging, verzamelen gebruikspatronen enz. Beschikbaarheid van de individuele huurder heeft geen gevolgen voor de beschikbaarheid van de toepassing als geheel. Echter, de beschikbaarheid en prestaties van het beheer van databases is van essentieel belang voor de functie van de toepassing en als het beheer van databases off line zijn de gehele toepassing off line is.</i>  

In de rest van het papier bespreken we de DR strategieën die betrekking hebben op een reeks scenario's uit de kosten gevoelige opstarttoepassingen die met beschikbaarheid van strenge eisen.  

## <a name="scenario-1-cost-sensitive-startup"></a>Scenario 1. Kosten gevoelige opstarten

<i>Ik ben een bedrijf opstarten en ben zeer gevoelige kosten.  Ik wil eenvoudiger implementatie en beheer van de toepassing en ik wil een beperkte SERVICEOVEREENKOMST voor afzonderlijke klanten hebt. Maar ik wil voor de toepassing als geheel nooit off line is.</i>

Om te voldoen aan de eis van de eenvoud, alle databases van huurder implementeren in één elastisch van toepassingen in de regio van uw keuze met Azure en u de database (s) beheer als zelfstandige geo-gerepliceerde database (s) te implementeren. Gebruik voor het herstel van huurders geo-restore, die wordt geleverd zonder extra kosten. Om de beschikbaarheid van het beheer van databases, moet zij geo gerepliceerd naar een andere regio (stap 1). De lopende kosten van de configuratie van disaster recovery in dit scenario is gelijk aan de totale kosten van de secundaire database (s). Deze configuratie wordt geïllustreerd in het volgende diagram.

![Figuur 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

In het geval van een storing in de primaire regio, worden de stappen aan uw toepassing online brengen door in het volgende diagram geïllustreerd.

- Het beheer van failover databases direct (2) op de DR regio. 
- Wijziging de verbindingsreeks van de toepassing om te verwijzen naar de DR regio. Alle nieuwe accounts en huurder databases gemaakt in de DR regio. De bestaande klanten zien hun gegevens tijdelijk niet beschikbaar.
- De elastische toepassingen maken met dezelfde configuratie als de oorspronkelijke groep (3). 
- Geo-terugzetten gebruiken om kopieën te maken van de huurder (4) van databases. U kunt rekening houden met de individuele herstelt die door de eindgebruiker verbindingen of sommige andere toepassing specifieke prioriteit schema.

Op dit moment uw toepassing in de DR regio weer on line is, maar sommige klanten krijgen vertraging bij de toegang tot hun gegevens.

![Figuur 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Als de onderbreking is een tijdelijke, is het mogelijk dat de primaire regio zullen worden goedgemaakt door Azure voordat alle herstelbewerkingen uitgevoerd in de DR regio zijn. In dit geval moet u goedkeuringen door de toepassing terug naar de primaire regio verplaatsen. Het proces duurt de stappen in het volgende diagram geïllustreerd.
 
- Aanvragen voor alle uitstaande geo-terugzetten annuleren.   
- Failover de database (s) beheer van het primaire gebied (5). Opmerking: Na het herstel van het gebied de oude primaire kleuren zijn automatisch geworden secundaire servers. Nu gaat ze opnieuw rollen. 
- Wijziging de verbindingsreeks van de toepassing om te verwijzen naar de primaire regio. Nu worden alle nieuwe accounts en databases van de huurder gemaakt in de primaire regio. Sommige klanten zien hun gegevens tijdelijk niet beschikbaar.   
- Stel alle databases in de groep DR op alleen-lezen zodat ze in de DR regio (6) kunnen niet worden gewijzigd. 
- Voor elke database in de DR-toepassingen die zijn gewijzigd sinds het herstel, hernoemen of verwijderen van de bijbehorende databases in de primaire groep (7). 
- De bijgewerkte databases uit de DR groep kopiëren naar de primaire groep (8). 
- Verwijder de DR-toepassingen (9)

Op dit moment wordt uw toepassing in de primaire regio met alle huurder databases beschikbaar in de primaire groep on line zijn.

![Figuur 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Het belangrijkste **voordeel** van deze strategie is lage lopende kosten voor gegevensredundantie laag. Back-ups worden automatisch opgehaald van de SQL-Database-service met geen toepassing herschrijven en zonder extra kosten.  De kosten worden alleen weergegeven wanneer de elastische databases zijn teruggezet. Het **compromis** is het volledige herstel van de huurder-databases gaat heel wat tijd. Dit is afhankelijk van het totaal aantal herstelt u in de regio van Dr. Watson wordt gestart en de totale grootte van de huurder-databases. Zelfs als u herstelt sommige huurders boven andere prioriteit, zal u concurreren met alle andere terugzetten die zijn geïnitieerd in dezelfde regio als de service zal bemiddelen en beperken om te minimaliseren van de algemene impact op de databases van de bestaande klanten. Bovendien kan het herstel van de databases van de huurder niet starten totdat de nieuwe elastische toepassingen in de DR regio wordt gemaakt.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scenario 2. Rijpe toepassing met gelaagde service 

<i>Ik ben een rijpe SaaS-applicatie met gelaagde service aanbiedingen en andere Sla's voor klanten proefversies en betalende klanten. Ik heb voor de evaluatieversie klanten om de kosten zoveel mogelijk te beperken. Evaluatieversie klanten uitvaltijd kunnen nemen, maar ik wil de kans vrij klein. Elke uitvaltijd is voor de betalende klanten een vlucht risico. Dus ik wil om ervoor te zorgen dat betalende zijn klanten altijd toegang krijgen tot hun gegevens.</i> 

Ter ondersteuning van dit scenario, moet u de evaluatieversie huurders van betaalde huurders scheiden door ze in afzonderlijke groepen van elastisch te plaatsen. De evaluatieversie klanten er lagere eDTU per huurder en lagere SLA met een langere hersteltijd. De betalende klanten worden in een groep met hogere eDTU per huurder en een hogere SLA. Om er zeker van de laagste hersteltijd, moet de betalende klanten huurder databases geo gerepliceerd. Deze configuratie wordt geïllustreerd in het volgende diagram. 

![Figuur 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Als in het eerste scenario, het beheer van databases heel actief zijn zodat u een zelfstandige geo-gerepliceerde database voor (1 gebruiken). Hierdoor worden de voorspelbare prestaties voor nieuwe klant abonnementen, profielupdates en andere beheertaken uit te voeren. Het gebied waarin de primaire kleuren van het management-databases zich bevinden wordt het primaire regio en de regio waarin de secundaire servers van de beheer-databases zich bevinden worden de DR regio.

De betalende klanten huurder databases hebben in de "betaalde" groep ingericht in de regio voor primaire actieve databases. U moet een tweede groep met dezelfde naam in de regio DR inrichten. Elke huurder worden geo gerepliceerd naar de tweede groep (2). Hiermee schakelt u een snel herstel van de huurder-databases met behulp van failover. 

Als een storing in de primaire regio optreedt, worden de stappen aan uw toepassing online brengen in het volgende diagram geïllustreerd:

![Figuur 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Onmiddellijk een failover de management database (s) op de DR regio (3).
- De verbindingsreeks van de toepassing om te verwijzen naar de DR regio wijzigen. Nu worden alle nieuwe accounts en databases van de huurder gemaakt in de DR regio. De evaluatieversie bestaande klanten zien hun gegevens tijdelijk niet beschikbaar.
- Failover-databases naar de groep in het gebied DR onmiddellijk herstel van hun beschikbaarheid (4) de huurder betaalde. Nadat de failover een snelle metagegevens wijzigingen die kunt u overwegen een optimalisatie waar de afzonderlijke failover op verzoek worden geactiveerd door de eindgebruiker verbindingen is. 
- Als uw toepassingen secundaire eDTU kleiner dan de primaire is omdat de secundaire databases alleen de capaciteit voor het verwerken van de logboekbestanden wijzigen terwijl ze secundaire servers zijn vereist, moet u onmiddellijk de capaciteit van toepassingen nu aangepast aan de volledige werklast van alle huurders (5) verhogen. 
- De nieuwe elastische toepassingen maken met dezelfde naam en dezelfde configuratie in de DR regio voor de evaluatieversie klanten databases (6). 
- Als de evaluatieversie klanten van toepassingen is gemaakt, gebruiken geo herstellen de huurder voor afzonderlijke evaluatie-databases herstellen naar de nieuwe groep (7). U kunt rekening houden met de individuele herstelt die door de eindgebruiker verbindingen of sommige andere toepassing specifieke prioriteit schema.

De toepassing is nu weer online in de DR regio. Alle betalende klanten hebben toegang tot hun gegevens tijdens de proefperiode klanten vertraging krijgen bij de toegang tot hun gegevens.

Wanneer de primaire regio wordt hersteld door Azure *nadat* u de toepassing in de DR regio die kunt u doorgaan met de toepassing in die regio hebt teruggezet of u kunt de primaire regio niet. Als de primaire regio hersteld *voordat u* is de failover-voltooid, moet u rekening houden bij gebreke terug meteen. De failback-bewerking gaat de stappen in het volgende diagram wordt geïllustreerd: 
 
![Figuur 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Aanvragen voor alle uitstaande geo-terugzetten annuleren.   
- Failover-beheer van de database (s) (8). De oude primaire na het herstel van het gebied waren automatisch de secundaire geworden. Nu wordt de primaire opnieuw.  
- De huurder betaalde failover-databases (9). Ook na het herstel van het gebied de oude primaire kleuren automatisch worden de secundaire servers. Nu worden ze opnieuw de primaire kleuren. 
- Stel de teruggezette evaluatieversie databases die in de DR regio hebt gewijzigd in alleen-lezen (10).
- Voor elke database in de evaluatieversie klanten DR toepassingen die gewijzigd sinds het herstel, hernoemen of verwijderen van de bijbehorende database in de primaire groep van klanten proefversies (11). 
- De bijgewerkte databases uit de DR groep kopiëren naar de primaire groep (12). 
- Verwijder de DR-toepassingen (13) 

> [AZURE.NOTE] De failover-bewerking is asynchroon. De hersteltijd is het belangrijk dat u de huurder-databases failover-opdracht in batches van ten minste 20 database uitvoeren minimaliseren. 

Het belangrijkste **voordeel** van deze strategie is dat u de hoogste SLA voor de betalende klanten. Ook zorgt ervoor dat de nieuwe proeven opgeheven wordt zodra de proefperiode DR van toepassingen is gemaakt. Het **compromis** is dat deze instelling de totale kosten van de huurder databases door de kosten van de secundaire DR van toepassingen voor betaalde klanten wordt verhoogd. Bovendien als de secundaire groep een ander formaat heeft, betalende klanten zullen de prestaties lager na een failover totdat de upgrade van toepassingen in de DR regio is voltooid. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scenario 3. Geografisch gedistribueerde toepassing met gelaagde service

<i>Ik heb een volwassen SaaS-toepassing met gelaagde service biedt. Ik wil een zeer agressieve SLA te bieden aan mijn klanten betaald en verkleint het risico van effecten wanneer storingen optreden omdat zelfs korte onderbreking leiden ontevredenheid van de klant tot kan. Het is essentieel dat de betalende klanten altijd toegang hebben tot hun gegevens. De proeven zijn gratis en een SLA wordt niet aangeboden tijdens de proefperiode.</i> 

Ter ondersteuning van dit scenario moet u drie afzonderlijke groepen van elastische hebben. Twee groepen van gelijke grootte met een hoog eDTUs per database moeten worden ingericht in twee verschillende regio's aan de betaalde klanten huurder databases bevatten. De derde groep met de evaluatieversie huurders hebben een lagere eDTUs per database en worden ingericht in een van de twee regio.

Om er zeker van de laagste hersteltijd tijdens storingen de betalende klanten huurder moet databases geo gerepliceerd met 50% van de primaire databases in elk van de twee regio's. Elke regio moet ook 50% van de secundaire databases. Op deze manier als een off line is slechts 50% van de betaalde klanten databases negatieve gevolgen zou hebben en moet failover. De andere databases blijft intact. Deze configuratie wordt geïllustreerd in het volgende diagram:

![Figuur 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Als in het vorige scenario's, het beheer van databases heel actief zijn zodat u deze als zelfstandige geo-gerepliceerde database (s) (1 configureren moet). Hierdoor worden de voorspelbare prestaties van de nieuwe klant abonnementen, profielupdates en andere beheertaken uit te voeren. Regio A zou de primaire regio voor het beheer van databases en de regio B wordt gebruikt voor het herstel van de beheer-database (s).

De betalende klanten huurder databases wordt ook geo gerepliceerd, maar met primaire en secundaire servers voor de splitsing tussen regio A en B (2) de regio. Op deze manier de huurder primaire databases door de storing getroffen kan failover naar de andere regio en beschikbaar komen. De andere helft van de databases van de huurder worden helemaal niet beïnvloed. 

Het volgende diagram ziet u de stappen te nemen als er een storing optreedt in gebied A.

![Figuur 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Onmiddellijk een failover de management-databases naar regio B (3).
- Pas de verbindingstekenreeks van de toepassing om te verwijzen naar de database (s) beheer in regio B. het beheer database (s) om te controleren of de nieuwe accounts en de databases van de huurder in regio B wordt gemaakt en de bestaande databases voor de huurder te vinden er ook wijzigen. De evaluatieversie bestaande klanten zien hun gegevens tijdelijk niet beschikbaar.
- Failover-databases naar groep 2 in B gebied onmiddellijk herstel van hun beschikbaarheid (4) de huurder betaalde. Nadat de failover een snelle metagegevens wijzigingen die kunt u overwegen een optimalisatie waar de afzonderlijke failover op verzoek worden geactiveerd door de eindgebruiker verbindingen is. 
- Groep 2 bevat sinds nu alleen primaire databases de totale werkbelasting in de groep verhoogt zodat u onmiddellijk de grootte van eDTU (5) moet verhogen. 
- De nieuwe elastische toepassingen maken met dezelfde naam en dezelfde configuratie in de regio B voor de evaluatieversie klanten databases (6). 
- Als de groep is gemaakt gebruik geo herstellen de huurder voor afzonderlijke evaluatie-database terugzetten in de groep (7). U kunt rekening houden met de individuele herstelt die door de eindgebruiker verbindingen of sommige andere toepassing specifieke prioriteit schema.


> [AZURE.NOTE] De failover-bewerking is asynchroon. De hersteltijd is het belangrijk dat u de huurder-databases failover-opdracht in batches van ten minste 20 database uitvoeren minimaliseren. 

Op dit moment is uw toepassing in gebied B. weer on line Alle betalende klanten hebben toegang tot hun gegevens tijdens de proefperiode klanten vertraging krijgen bij de toegang tot hun gegevens.

Wanneer een regio wordt hersteld moet u beslissen of u regio B gebruiken voor klanten proefversies of failback wilt voor het gebruik van de proefperiode klanten van toepassingen in gebied A. Een criteria zijn de % van de evaluatieversie huurder databases die zijn gewijzigd sinds het herstel. Ongeacht dat besluit moet u opnieuw de huurder betaalde tussen twee groepen te verdelen. het volgende diagram illustreert het proces wanneer de proefperiode huurder databases failback naar regio A.  
 
![Figuur 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Aanvragen voor alle uitstaande geo-terugzetten naar evaluatieversie DR groep annuleren.   
- Failover de database management (8). Na het herstel van het gebied, was de oude primaire automatisch de secundaire werd. Nu wordt de primaire opnieuw.  
- Selecteer welke betaalde huurder databases terug naar groep 1 en start een failover naar hun secundaire servers (9) mislukt. Na het herstel van het gebied werd van alle databases in de groep 1 automatisch secundaire servers. Nu 50% van hen worden primaire kleuren opnieuw. 
- Verkleinen van groep 2 naar de oorspronkelijke eDTU (10).
- Alle hersteld evaluatieversie databases in de regio B naar alleen-lezen (11).
- Voor elke database in de evaluatieversie DR-toepassingen die zijn gewijzigd sinds het herstel hernoemen of verwijderen van de bijbehorende database in de evaluatie van de primaire groep (12). 
- De bijgewerkte databases uit de DR groep kopiëren naar de primaire groep (13). 
- Verwijder de DR-groep (14) 

De belangrijkste **voordelen** van deze strategie zijn:

- Biedt ondersteuning voor de meest agressieve SLA voor de betalende klanten omdat deze ervoor zorgt dat een stroomstoring meer dan 50% van de databases van de huurder niet kan beïnvloeden. 
- Het zorgt ervoor dat de nieuwe proeven opgeheven wordt zodra de trail DR van toepassingen wordt gemaakt tijdens het herstel. 
- Efficiënter gebruik van de capaciteit van toepassingen kan worden gegarandeerd dat 50% van de secundaire databases in groep 1 en groep 2 minder actief zijn dan de primaire databases.

De belangrijkste **afweging** zijn:

- De CRUD-bewerkingen op de beheer-database (s) heeft lagere latentie voor de eindgebruikers verbonden met de regio A dan voor de regio B verbonden als zij wordt uitgevoerd tegen de primaire database (s) het beheer van de eindgebruikers.
- Complex ontwerp van de database management is vereist. Bijvoorbeeld zou elke record huurder moeten hebben een locatie code die moet worden gewijzigd tijdens failover en failback.  
- Betalende klanten prestaties lager dan normaal totdat de upgrade van toepassingen in regio B is voltooid. 

## <a name="summary"></a>Samenvatting

Dit artikel is gericht op de disaster recovery strategieën voor de databaselaag door een huurder met meerdere SaaS ISV-toepassing gebruikt. De strategie die u kiest moet worden gebaseerd op de behoeften van de toepassing, zoals het business model, de SLA u wilt bieden aan uw klanten, budget beperking enz... Elke strategie beschreven geeft een overzicht van de voordelen en de wisselwerking zodat u een weloverwogen beslissing kan nemen. De specifieke toepassing zal waarschijnlijk ook andere onderdelen Azure. Dus controleer hun business continuity richtsnoeren en goedkeuringen door het herstel van de databaselaag met hen. Raadpleeg voor meer informatie over het beheren van terugwinning van databasetoepassingen in Azure, [cloud-oplossingen ontwerpen voor noodherstel](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Volgende stappen

- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
- Zie informatie over automatische back-ups gebruikt voor herstel, [een database van de service gestart back-ups terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over herstelopties voor sneller [Actief-Geo-replicatie](sql-database-geo-replication-overview.md)  
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor het archiveren van [kopie](sql-database-copy.md)
