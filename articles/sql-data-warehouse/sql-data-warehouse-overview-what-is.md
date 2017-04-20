<properties
   pageTitle="Wat is Azure SQL Data Warehouse? | Microsoft Azure"
   description="Bedrijfsniveau gedistribueerde database kunnen petabyte volumes van relationele en niet-relationele gegevens verwerken. Is de bedrijfstak van de eerste cloud datawarehouse met vergroten, verkleinen en te onderbreken in seconden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Wat is Azure SQL Data Warehouse?

Azure SQL Data Warehouse is een cloud-gebaseerde, schalen database kunnen grote hoeveelheden relationele en niet-relationele gegevens verwerken. Gebaseerd op onze architectuur massively parallel processing (MPP) en kan SQL Data Warehouse uw onderneming werkbelasting verwerken.

SQL datawarehouse:

- De relationele database SQL Server combineert met mogelijkheden voor schaalvergroting Azure cloud. Vergroten, verkleinen, onderbreken of hervatten compute in seconden. U besparen kosten door geschaalde uitbreiding van de CPU wanneer u deze nodig hebt, en weer gebruik tijdens niet-piekuren knippen.
- Borduurt voort op de Azure-platform. Het is eenvoudig te implementeren, naadloos onderhouden en volledig fault tolerant door automatische back-ups.
- Vormt een aanvulling op het ecosysteem van SQL Server. U kunt vertrouwde SQL Server Transact-SQL-(T-SQL) en hulpmiddelen ontwikkelen.

Dit artikel beschrijft de belangrijkste functies van SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Massively parallel processing architecture

SQL Data Warehouse is dat een massively parallel processing (MPP) gedistribueerd databasesysteem. Gegevens delen en verwerken van mogelijkheden op verschillende knooppunten, biedt SQL Data Warehouse enorme schaalbaarheid - tot ver voorbij een enkel systeem.  Achter de schermen wordt gespreid SQL Data Warehouse uw gegevens over veel gedeelde opslag en verwerkingseenheden. De gegevens worden opgeslagen in een lokaal redundante opslag Premium en gekoppeld aan knooppunten voor de uitvoering van de query berekent. Met deze architectuur wordt SQL Data Warehouse "Verdeel en heers"-benadering geladen en complexe query's uitgevoerd. Aanvragen worden ontvangen door het knooppunt controle, geoptimaliseerd en vervolgens doorgegeven aan de Compute nodes kunnen werken in parallel.

Door een combinatie van MPP architectuur en Azure opslagmogelijkheden kunt SQL Data Warehouse:

- Groter of kleiner onafhankelijk van compute opslag.
- Groter of kleiner berekenen zonder de gegevens te verplaatsen.
- Computercapaciteit onderbreken terwijl gegevens intact blijft.
- Cv berekenen capaciteit op elk gewenst moment.

Het volgende diagram ziet de architectuur in meer detail.

![SQL-gegevens magazijn architectuur][1]


**Controleknooppunt:** Het knooppunt beheer beheert en query's worden geoptimaliseerd. Het is de front-endtoepassing die met alle toepassingen en verbindingen samenwerkt. In SQL Data Warehouse, het knooppunt controle wordt aangedreven door een SQL-Database en verbinding te maken met het uiterlijk en dezelfde lijkt. Onder het oppervlak coördineert het knooppunt controle alle verplaatsing van gegevens en berekenen van de parallelle query's uitvoeren op uw gedistribueerde gegevens vereist. Wanneer u een T-SQL-query naar SQL Data Warehouse indient, het knooppunt controle deze worden omgezet in afzonderlijke query's die worden uitgevoerd op elk computerknooppunt parallel.

**Compute nodes:** De Compute nodes fungeren als de kracht achter SQL Data Warehouse. SQL-Databases die uw gegevens op te slaan en de query verwerkt zijn. Als u gegevens toevoegt, distribueert SQL Data Warehouse de rijen aan de Compute nodes. De Compute nodes zijn de werknemers die de parallelle query's op uw gegevens uitvoeren. Na verwerking geeft zij de resultaten naar het knooppunt controle. Als u klaar bent met de query, het knooppunt controle aggregaten van de resultaten en het uiteindelijke resultaat.

**Opslag:** Uw gegevens worden opgeslagen in Azure Blob-opslag. Wanneer Compute nodes met uw gegevens, werken ze rechtstreeks van en naar een blob-opslag lezen en schrijven. Aangezien opslag Azure transparant wordt uitgebreid en sterk, SQL Data Warehouse hetzelfde kunt doen. Aangezien compute en opslag onafhankelijk zijn, kan SQL Data Warehouse opslag los van de schaal berekenen en vice versa automatisch schalen. Azure Blob-opslag is ook volledig fouttolerante en vergemakkelijkt het maken en terugzetten van back-up.

**Service voor verplaatsing van gegevens:** Data verkeer Service (DMS) verplaatst gegevens tussen de knooppunten. DMS geeft de Compute knooppunten-toegang tot gegevens die ze nodig hebben om joins en samenvoegingen. DMS is geen dienst van Azure. Het is een Windows-service die wordt uitgevoerd samen met SQL-Database op alle knooppunten. Aangezien DMS uitgevoerd op de achtergrond, won't u ermee rechtstreeks. Echter wanneer u queryplannen bekijkt, zult u dat ze bevatten sommige bewerkingen DMS aangezien verplaatsing van gegevens moet u elke query parallel uitvoeren.


## <a name="optimized-for-data-warehouse-workloads"></a>Geoptimaliseerd voor gegevens magazijn werkbelasting

De MPP-aanpak is door een aantal specifieke prestatie-optimalisatie, met inbegrip van gegevensopslag aided:

- Een gedistribueerde query optimizer en een complexe statistische gegevens voor alle gegevens. De service is met informatie op de gegevensgrootte en de distributie, kunnen query's worden geoptimaliseerd door de beoordeling van de kosten van de specifieke gedistribueerde query.

- Geavanceerde algoritmen en technieken geïntegreerd in het proces van het verkeer gegevens efficiënt om gegevens te verplaatsen tussen computerbronnen als u nodig hebt voor het uitvoeren van de query. Deze gegevens beweging bewerkingen zijn ingebouwd en alle optimalisaties die de Service voor het verkeer van gegevens automatisch worden uitgevoerd.

- Geclusterde indexen **columnstore** standaard. SQL Data Warehouse haalt met behulp van opslag op basis van een kolom, gemiddeld 5 x compressie winsten via traditionele rij gebaseerde opslag en tot 10 x of meer prestaties verbeteren van query. Analytics query's die u nodig hebt voor het scannen van een groot aantal rijen werken prima op columnstore-indexen.


## <a name="predictable-and-scalable-performance"></a>Voorspelbare en schaalbare prestaties

SQL Data Warehouse gescheiden opslag en compute waarmee elk afzonderlijk geschaald. SQL Data Warehouse is snel en eenvoudig schaalbaar, zodat meer compute resources op elk gewenst moment toevoegen. Deze aanvulling is het gebruik van Azure Blob-opslag. BLOB's bieden niet alleen stabiel, gerepliceerde opslag, maar ook de infrastructuur voor moeiteloze uitbreiding tegen lage kosten. SQL Data Warehouse kunt met deze combinatie van opslag van de wolk schaal en Azure berekenen, u betalen voor prestaties van query's en de opslag, wanneer u deze nodig hebt. De hoeveelheid berekenen is net zo eenvoudig als een schuifregelaar naar links of rechts in de Azure portal of deze ook kan worden gepland met behulp van T-SQL en PowerShell.

Samen met de mogelijkheid om de hoeveelheid compute onafhankelijk van opslag volledig te beheren, kunt SQL Data Warehouse u volledig uw datawarehouse, wat betekent dat u niet betaalt voor berekenen wanneer u deze niet hoeft te onderbreken. Terwijl de opslag in plaats is alle compute uitgebracht in de belangrijkste toepassingen van Azure, u geld besparen. Wanneer nodig, gewoon hervatten de compute en uw gegevens hebben en beschikbaar zijn voor uw werkbelasting berekenen.

## <a name="data-warehouse-units"></a>Magazijn eenheden

Toewijzing van middelen naar het datawarehouse SQL wordt gemeten in eenheden magazijn (DWUs). DWUs zijn een aanduiding van de onderliggende bronnen, zoals CPU, geheugen, IOP's worden toegewezen aan uw SQL Data Warehouse. Vergroten van het aantal DWUs wordt verhoogd, middelen en prestaties. Met name ervoor DWUs te zorgen dat:

- U zijn kunt uw datawarehouse eenvoudig schalen zonder dat de onderliggende hardware of software.

- U kunt prestaties verbeteren voor een niveau van DWU voorspellen voordat u de grootte van uw gegevensmagazijn wijzigen.

- De onderliggende hardware en software van uw exemplaar wijzigen of verplaatsen zonder de prestaties van de werkbelasting.

- Microsoft kan pas aan de onderliggende architectuur van de service zonder gevolgen voor de prestaties van uw werkbelasting.

- Microsoft kan snel prestaties in SQL Data Warehouse, op een manier die schaalbaar is en gelijkmatig invloed op het systeem.

Magazijn eenheden bieden een maatregel drie precieze parameters die zeer aan elkaar zijn gekoppeld met de prestaties van de werkbelasting voor gegevensopslag. Het doel is dat de volgende sleutel werkbelasting metrics zal recht evenredig met de DWUs die u hebt gekozen voor uw datawarehouse.

**Scan/aggregatie:** Deze werkbelasting ingestelde metric is een query die een groot aantal rijen wordt gescand en voert een complexe aggregatie voor standaard gegevensopslag. Dit is een intensieve I/O en het CPU-bewerking.

**Laden:** Deze meetmethode is de mogelijkheid om op te nemen gegevens in de service. Laden zijn met PolyBase laden van een representatieve verzameling van Azure Blob-opslag voltooid. Deze metriek is ontworpen om stress-netwerk en CPU-aspecten van de service.

**Tabel maken als selecteren (CTAS):** CTAS meet de mogelijkheid om een tabel te kopiëren. Dit omvat het lezen van gegevens uit de opslag, distributie op de knooppunten van het toestel en opnieuw naar de opslag te schrijven. Het is een intensief gebruik van CPU, IO en netwerk.

## <a name="pause-and-scale-on-demand"></a>Onderbreken en de schaal op de vraag

Wanneer u een snellere resultaten nodig, uw DWUs te verhogen en betalen voor betere prestaties. Wanneer u moet minder rekenkracht, uw DWUs verlagen en betaalt alleen voor wat u nodig hebt. U denkt over het wijzigen van de DWUs in deze scenario's:

- U hoeft niet bij query's uitvoeren, bijvoorbeeld in het 's avonds of in het weekend, stilleggen uw query's. Vervolgens onderbreken compute resources om te voorkomen dat het betalen van DWUs wanneer u deze niet nodig hebt.

- Wanneer het systeem weinig vraag heeft, kunt u overwegen DWU op een klein formaat. U kunt nog steeds toegang tot de gegevens, maar op een aanzienlijke kostenbesparingen.

- Wanneer u een zware gegevens laden of transformatie-bewerking uitvoert, is het raadzaam om uit te breiden zodat de gegevens sneller beschikbaar is.

Om te begrijpen wat uw ideale DWU waarde is, probeer het omhoog en omlaag schalen en enkele query's uitvoeren na het laden van uw gegevens. Aangezien schalen snel is, kunt u proberen een aantal verschillende niveaus van prestaties in een uur of minder.  Houd er rekening mee dat SQL Data Warehouse is ontworpen voor het verwerken van grote hoeveelheden gegevens en voor een overzicht van de werkelijke capaciteit voor schalen, met name op de grotere schalen we bieden, u zult willen gebruiken een grote gegevensset die bijna 1 TB.


## <a name="built-on-sql-server"></a>Gebouwd op SQL Server

SQL Data Warehouse is gebaseerd op de relationele database-engine van SQL Server en bevat veel functies die u van een enterprise datawarehouse verwacht. Als u T-SQL al weet, is het gemakkelijk uw kennis overbrengen naar SQL Data Warehouse. Of u geavanceerde zijn of gewoon aan de slag, de voorbeelden in de documentatie kunt beginnen. Over het algemeen kunt u denkt over de manier waarop we de taalelementen van SQL Data Warehouse als volgt hebt samengesteld:

- SQL Data Warehouse gebruikt T-SQL-syntaxis voor veel bewerkingen. Ondersteunt ook een heel scala aan traditionele SQL-constructies zoals opgeslagen procedures, door de gebruiker gedefinieerde functies tabel partitioneren, indexen en sorteringen.

- SQL Data Warehouse bevat ook een aantal nieuwe functies van SQL Server, waaronder: geclusterd **columnstore** indexen, integratie van de PolyBase en controle (compleet met de beoordeling van de bedreiging) gegevens.

- Bepaalde elementen van de T-SQL taal die minder gebruikelijk zijn voor data warehousing workloads of nieuwer naar SQL Server, zijn mogelijk niet beschikbaar. Zie voor meer informatie de [documentatie bij de migratie][].

Met de functie voor Transact-SQL en gemeenschappelijkheid tussen SQL Server, SQL Data Warehouse, SQL-Database en Analytics Platform systeem, kunt u een oplossing die past bij de gegevensbehoeften van uw te ontwikkelen. U kunt bepalen waar u uw gegevens, op basis van prestaties, beveiliging en vereisten van de schaal, en brengt u de gegevens zo nodig tussen de verschillende systemen.

## <a name="data-protection"></a>Bescherming van gegevens

SQL Data Warehouse alle gegevens worden opgeslagen in een lokaal redundante opslag Azure Premium. Synchrone kopieën van de gegevens worden bijgehouden in het midden van de lokale gegevens te garanderen van transparant gegevensbescherming gelokaliseerde storingen. Bovendien SQL Data Warehouse automatisch back-ups van uw actieve databases (voortgezet) met regelmatige tussenpozen met Azure opslag momentopnamen. Zie voor meer informatie over hoe u back-up en terugzetten gaat, het [maken en terugzetten van back-up-overzicht][].

## <a name="integrated-with-microsoft-tools"></a>Geïntegreerd met Microsoft-hulpprogramma 's

SQL Data Warehouse ook geïntegreerd veel van de hulpmiddelen die gebruikers bekend met wellicht SQL-Server. Deze omvatten:

**Traditionele SQL Server's:** SQL Data Warehouse is volledig geïntegreerd met SQL Server Analysis Services, Integration Services en Reporting Services.

**Cloud-gebaseerde hulpprogramma's:** SQL Data Warehouse kunt gebruiken samen met een aantal nieuwe functies in Azure, met inbegrip van Data Factory, Stream Analytics Machine Learning en Power BI. Zie [overzicht van geïntegreerde hulpmiddelen][]voor een volledige lijst.

**Hulpprogramma's van derden:** Een groot aantal hulpprogramma derden providers hebben de integratie van de hulpprogramma's met SQL Data Warehouse gecertificeerd. Zie voor een volledige lijst [van SQL Data Warehouse solution partners][].

## <a name="hybrid-data-sources-scenarios"></a>Scenario's hybride gegevensbronnen

SQL Data Warehouse met PolyBase kunnen gebruikers een ongekende verplaatsen van gegevens tussen hun ecosysteem, de mogelijkheid voor het instellen van geavanceerde hybride scenario's met niet-relationele ontgrendelen en gegevensbronnen ruimten.

Polybase kunt u uw gegevens uit verschillende bronnen met behulp van bekende T-SQL-opdrachten. Polybase kunt u query niet-relationele gegevens in Azure Blob-opslag al dit een normale tabel is. Polybase niet-relationele query uitvoeren op gegevens of niet-relationele gegevens importeren in SQL Data Warehouse gebruiken.

- PolyBase maakt gebruik van externe tabellen niet-relationele gegevens. De definities zijn opgeslagen in SQL Data Warehouse, en kunt u ze openen met behulp van SQL en hulpprogramma's als u wilt toegang hebben tot normale relationele gegevens.

- Polybase is een agnostische in de integratie. Het heeft dezelfde functies en functionaliteit aan de bronnen die dit ondersteunt. De gegevens gelezen door de Polybase worden in verschillende indelingen, inclusief bestanden met scheidingstekens of ORC-bestanden.

- PolyBase kan worden gebruikt voor toegang tot de blob-opslag ook wordt gebruikt als opslag voor een cluster HDInsight. Hiermee hebt u toegang tot dezelfde gegevens met tools voor relationele en niet-relationele.

## <a name="sla"></a>SLA

SQL Data Warehouse biedt een product niveau service level agreement (SLA) als onderdeel van de SERVICEOVEREENKOMST van Microsoft Online Services. Ga naar de [SLA voor SQL Data Warehouse][]voor meer informatie. SLA alle andere producten u kunt vindt u informatie over de [Service Level Agreements] Azure pagina of deze downloaden op de pagina met [Volume Licensing][] . 

## <a name="next-steps"></a>Volgende stappen

Als u iets weet over SQL Data Warehouse, informatie over het snel [maken van een SQL-datawarehouse][] en [voorbeeldgegevens worden geladen][]. Bent u nieuw in Azure, is wellicht de [Azure woordenlijst][] handig zijn als er nieuwe terminologie. Of Bekijk enkele van deze andere SQL gegevens magazijn bronnen.  

- [Succesverhalen van klanten]
- [Blogs]
- [Functie-aanvragen]
- [Video 's]
- [Klant Advisory Team blogs]
- [Support ticket maken]
- [MSDN-forum]
- [Stack Overflow-forum]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Support ticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[voorbeeldgegevens worden geladen]: ./sql-data-warehouse-load-sample-databases.md
[maken van een SQL-datawarehouse]: ./sql-data-warehouse-get-started-provision.md
[Migratie van documentatie]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Geïntegreerde hulpmiddelen-overzicht]: ./sql-data-warehouse-overview-integrate.md
[Overzicht van back-up en terugzetten]: ./sql-data-warehouse-restore-database-overview.md
[Azure-verklarende woordenlijst]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Succesverhalen van klanten]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Klant Advisory Team blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functie-aanvragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video 's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA voor SQL datawarehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/en-us/support/legal/sla/
