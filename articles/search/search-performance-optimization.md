<properties 
    pageTitle="Azure prestaties en optimalisering van zoekacties | Microsoft Azure" 
    description="Azure Search prestaties afstemmen en optimale schaal configureren" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure prestaties en optimalisering van zoekacties

Een fantastische zoekfunctie is een sleutel tot succes voor veel mobiele telefoons en webtoepassingen. Van onroerend goed, auto marktplaatsen aan online catalogi gebruikt snel zoeken en relevante resultaten heeft invloed op de beleving van de klant. Dit document is bedoeld als kunt u aanbevolen procedures voor het optimaal Azure Search, met name voor geavanceerde scenario's met geavanceerde eisen voor schaalbaarheid, meertalige ondersteuning ontdekken of aangepaste volgorde.  Ook dit document geeft een overzicht van interne werking en benaderingen die effectief in real-world klant apps werken omvat.

## <a name="performance-and-scale-tuning-for-search-services"></a>Schaal afstemming voor diensten en prestaties

We zijn gebruikt voor zoekmachines zoals Bing en Google en de hoge prestaties bieden.  Wanneer klanten uw website zoeken is ingeschakeld of mobiele toepassing, verwachten zij bijgevolg vergelijkbare prestatie-eigenschappen.  Bij het optimaliseren voor prestaties, wordt een van de beste manieren te concentreren op de wachttijd is de tijd dat een query doet te voltooien en het resultaat.  Het is belangrijk dat bij het optimaliseren van latentie zoeken:

1. Pick een doel latentie (of een maximale hoeveelheid tijd) die een normale zoekopdracht aanvragen worden moet voltooid.

2. Maken en testen van een reële werklast ten opzichte van de search-service met een realistische dataset aan deze vertraging te meten.

3. Beginnen met een laag aantal query's per seconde (QPS) en het nummer in de test wordt uitgevoerd totdat de latentie van query zakt tot onder de opgegeven vertraging worden steeds.  Dit is een belangrijke benchmark te plannen voor schaal groeien met uw toepassing in gebruik.

4. Waar mogelijk, opnieuw gebruikt HTTP-verbindingen.  Als u de .NET SDK Azure Search gebruikt, betekent dit opnieuw te gebruiken of een instantie van [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) en als u de REST API gebruikt, moet u een enkel HttpClient hergebruiken.
 
Tijdens het maken van deze test werkbelasting, er zijn enkele kenmerken van Azure zoeken rekening moet houden:

1. Is het mogelijk om push zo veel zoeken in één keer een query dat de beschikbare bronnen in uw Azure Search-service zal worden overweldigd.  In dat geval ziet u HTTP 503 responscodes.  Om deze reden is het beste beginnen met verschillende bereiken van search-aanvragen voor een overzicht van de verschillen in tarieven latentie toevoegen van meer search-aanvragen.

2. Uploaden van inhoud die Azure Search is van invloed op de algehele prestaties en de latentie van de zoekservice Azure.  Als u verwacht om gegevens te verzenden terwijl gebruikers zoekopdrachten uitvoert, is het belangrijk dat u de werklast in aanmerking nemen bij de tests.

3. Niet elke zoekopdracht wordt uitgevoerd op het niveau van dezelfde prestaties.  Zo zal een document opzoeken of zoek suggestie meestal sneller dan een query met een groot aantal facetten en filters uitvoeren.  Het is raadzaam te nemen van de verschillende query's die u verwacht te zien in aanmerking bij het bouwen van uw tests.  

4. Variatie van de search-aanvragen is belangrijk omdat het in cache opslaan van gegevens als u voortdurend de aanvragen met dezelfde zoekopdracht uitvoert, begint met het zien er beter uit dan het mogelijk met een query meer ongelijksoortige prestaties.

> [AZURE.NOTE] [Visual Studio-Load testen](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) is een goede manier om uw benchmark-tests uitvoeren omdat u HTTP-verzoeken uitvoeren als u zou nodig hebt voor het uitvoeren van query's op Azure zoeken en kunnen parallelization van de aanvragen.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Schalen Azure zoeken naar query hoge tarieven en de snelheid van de aanvragen

Wanneer u te veel beperkt aanvragen ontvangen of groter zijn dan de tarieven van de latentie doel van een betere query laden, kunt u als u wilt verlagen tarieven latentie op twee manieren zoeken:

1. **Vergroten replica's:**  Een replica is vergelijkbaar met een kopie van uw gegevens zodat Azure Search laden tegen meerdere exemplaren aanvragen.  Alle taakverdeling en replicatie van gegevens tussen replica's wordt beheerd door Azure zoeken en u kunt het aantal replica's die zijn toegewezen voor uw service op elk gewenst moment wijzigen.  U kunt maximaal 12 replica's in een standaard zoekservice en 3 replica's in een standaardzoekopdracht service toewijzen.  Replica's kunnen worden aangepast via de [Azure Portal](search-create-service-portal.md) of met behulp van de [API voor beheer Azure Search](search-get-started-management-api.md).

2. **Vergroten Tier zoeken:**  Azure Search wordt geleverd in een [aantal lagen](https://azure.microsoft.com/pricing/details/search/) en elk van deze niveaus biedt verschillende niveaus van prestaties.  In sommige gevallen wellicht zo veel query's dat de laag op voldoende lage latentie tarieven, kan niet bieden, zelfs wanneer replica's zijn maximum is overschreden.  In dit geval wilt u overwegen gebruik te maken van een van de hogere niveaus zoeken zoals de Azure Search S3-laag die is geschikt voor de scenario's met grote aantallen documenten en zeer hoge query werkbelasting.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Schalen Azure langzaam afzonderlijke query's zoeken

Een andere reden waarom latentie tarieven langzaam verlopen te is afkomstig van één query duurt te lang voordat voltooien.  In dit geval verbeterd toe te voegen replica's niet latentie tarieven.  Voor deze aanvraag zijn twee opties beschikbaar:

1. **Partities te vergroten** Een partitie is een mechanisme voor het opsplitsen van de gegevens over extra bronnen.  Om deze reden, wanneer u een tweede partitie toevoegen opgehaald uw gegevens in tweeën splitsen.  De index een derde partitie gesplitst in drie, enz.  Dit heeft ook het effect dat in sommige gevallen trage query's worden sneller door de parallelization van de berekening.  Er zijn een paar voorbeelden van waar wij hebben dit parallelization zeer goed werkt met query's met lage selectiviteit query's.  Dit bestaat uit de query's die overeenkomen met veel documenten of wanneer faceting moet worden geteld bieden via een groot aantal documenten.  Aangezien een groot aantal berekeningen die nodig zijn voor het verkrijgen van de relevantie van de documenten of voor het tellen van het aantal documenten, kunt toevoegen van extra partities bieden extra berekening.  

   Kunnen er maximaal 12 partities in de standaard zoekmachine en 1 partitie in de basic search-service.  Partities kunnen worden aangepast via de [Azure Portal](search-create-service-portal.md) of met behulp van de [API voor beheer Azure Search](search-get-started-management-api.md).

2. **Hoge kardinaliteit velden beperken:** Een hoge kardinaliteit veld bestaat uit een facetable of Filterbaar veld dat is een groot aantal unieke waarden, en hierdoor wordt veel bronnen voor het berekenen van resultaten op.   Bijvoorbeeld zou als u een veld Product-ID of beschrijving als facetable/Filterbaar maken voor hoge kardinaliteit omdat het merendeel van de waarden van document naar document uniek zijn. Waar mogelijk, het aantal hoge kardinaliteit velden beperken.

3. **Vergroten Tier zoeken:**  Maximaal verplaatsen is een hogere laag van Azure zoeken een andere manier om de prestaties van trage query's te verbeteren.  Elke hogere laag biedt ook snellere processor en meer geheugen die een positieve invloed op de prestaties van query's hebben kan.

## <a name="scaling-for-availability"></a>Schalen voor beschikbaarheid

Replica's niet alleen helpen verminderen van de latentie van query maar kunnen ook staan voor hoge beschikbaarheid.  Met een enkele replica verwachten u periodieke uitvaltijd vanwege de server opnieuw wordt opgestart na een software-updates of voor andere evenementen onderhoud dat zich zal voordoen.  Daarom is het belangrijk rekening te houden als uw toepassing hoge beschikbaarheid van zoekopdrachten (query's) en weggeschreven (indexing gebeurtenissen) vereist.  Azure Search biedt SLA opties op de betaalde zoekresultaten aanbiedingen met de volgende kenmerken:

- 2 replica's voor hoge beschikbaarheid van de werklast alleen-lezen (query's)
- 3 of meer replica's voor hoge beschikbaarheid van werkbelasting lezen-schrijven (query's en indexering)

Ga naar de [Azure Search Service Level Agreement](https://azure.microsoft.com/support/legal/sla/search/v1_0/)voor meer details over dit.

Omdat replica's kopieën van uw gegevens zijn, kunt u met meerdere replica's Azure Search te van de computer opnieuw wordt opgestart en onderhoud tegen een replica tegelijkertijd terwijl de query's worden uitgevoerd op de andere replica's blijven.  Daarom moet u ook overwegen hoe de query's die moeten worden uitgevoerd op een kopie van de gegevens minder mogelijk van invloed op deze downtime.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Werklast verdeeld geo schalen en redundantie geo

Voor een standaardwerkbelasting geo verdeeld vindt u dat gebruikers die zich ver van het datacenter waar uw Azure Search-service wordt gehost hogere tarieven van latentie hebben.  Om deze reden is het vaak belangrijk om meerdere diensten in gebieden die dichter bij de gebruikers.  Azure Search biedt momenteel geen een geautomatiseerde methode van geo repliceren Azure zoekindexen tussen de regio's, maar er zijn sommige technieken die kunnen worden gebruikt om dit proces eenvoudig te implementeren en te beheren. Deze worden in de volgende secties beschreven.

Twee of meer indexen die beschikbaar zijn in twee of meer regio's waar een gebruiker worden doorgestuurd naar de Azure Search-service met de laagste latentie zoals in dit voorbeeld is de doelstelling van een set geo distributed zoekservices:

   ![Cross-tabblad Services per regio][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Gegevens synchroon houden over meerdere Azure Search services

Er zijn twee opties voor het bijhouden van de gedistribueerde zoekservices gesynchroniseerd die bestaan uit hetzij met behulp van de [Azure Search Indexer](search-indexer-overview.md) of de Push API (ook wel genoemd de [Azure Search REST API](https://msdn.microsoft.com/library/dn798935.aspx)).  

### <a name="azure-search-indexers"></a>Azure Search indexeerfuncties

Als u de Azure Search Indexer, importeert u al wijzigingen in gegevens uit een centraal gegevensarchief zoals Azure SQL DB of DocumentDB. Wanneer u een nieuwe zoekopdracht Service, maakt u gewoon ook een nieuwe Azure Search Indexer voor die service die naar deze dezelfde gegevensarchief verwijst maken. Wanneer u nieuwe wijzigingen in het gegevensarchief komen zal ze vervolgens worden geïndexeerd door de verschillende indexeerfuncties.  

Hier volgt een voorbeeld van wat die architectuur eruit zou zien.

   ![Één gegevensbron met gedistribueerde indexeerfunctie en combinaties van service][2]


### <a name="push-api"></a>Push API 
Als u de Azure Search Push API voor het [bijwerken van inhoud in de zoekindex Azure](https://msdn.microsoft.com/library/dn798930.aspx), kunt u de verschillende zoekservices synchroon houden door wijzigingen aan alle diensten pushen wanneer er een update is vereist.  Hierbij is het belangrijk om te handelen wanneer een update voor één zoekservice mislukt en slagen voor een of meer updates.

## <a name="leveraging-azure-traffic-manager"></a>Gebruik van Azure verkeer Manager

[Azure verkeer Manager](../traffic-manager/traffic-manager-overview.md) kunt u op route verzoeken aan meerdere geo vinden websites die vervolgens worden ondersteund door meerdere Azure zoekservices.  Een voordeel van het beheer van netwerkverkeer is dat het zoeken om te zorgen dat deze beschikbaar is en gebruikers doorsturen naar andere diensten in geval van downtime Azure kunt zoeken.  Bovendien, als u routering search-aanvragen via Azure websites, Azure verkeer Manager kunt u saldo gevallen waarin de Website van laden, maar geen Azure zoeken.  Hier volgt een voorbeeld van wat de architectuur die maakt gebruik van beheer van netwerkverkeer.

   ![Cross-tabblad Services per regio, met centrale verkeer Manager][3]

## <a name="monitoring-performance"></a>Prestaties controleren

Azure Search biedt de mogelijkheid om te analyseren en controleren van de prestaties van uw service via [Search verkeer Analytics (STA)](search-traffic-analytics.md). Via STA, kunt u desgewenst de afzonderlijke bewerkingen als geaggregeerde statistieken op een rekening Azure opslag die vervolgens kan worden verwerkt voor analyse of weergegeven in Power BI registreren.  STA parameters kunt u prestatiestatistieken zoals gemiddeld aantal query's of responstijden query bekijken.  Ook kunt de logboekregistratie van bewerkingen u inzoomen op de details van de specifieke zoekcriteria in.

STA is een waardevol hulpmiddel te begrijpen tarieven vanuit dat perspectief zoeken Azure latentie.  Omdat de query prestatiegegevens vastgelegd zijn gebaseerd op de duur voor een query worden volledig verwerkt in Azure zoekopdracht (vanaf het moment dat daarom wordt gevraagd wanneer deze is verzonden), kan u dit gebruiken om te bepalen of latentieproblemen van de kant van Azure Search service of andere services, zoals van netwerkvertraging zijn.  

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de prijzen lagen en services grenzen voor elkaar, [limieten in Azure Search Service](search-limits-quotas-capacity.md).

Ga naar de [Capaciteitsplanning](search-capacity-planning.md) voor meer informatie over de partitie en replica combinaties.

Bekijk de volgende video voor meer drilldown op prestaties en voor bepaalde demonstraties van het implementeren van de optimalisaties die in dit artikel besproken:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png