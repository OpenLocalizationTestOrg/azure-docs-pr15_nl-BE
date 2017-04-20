<properties
   pageTitle="Schaalbare webtoepassing | Azure referentiearchitectuur | Microsoft Azure"
   description="Verbeteren van de schaalbaarheid in een webtoepassing die wordt uitgevoerd in Microsoft Azure."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Verbeteren van de schaalbaarheid in een webtoepassing 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel wordt een aanbevolen architectuur voor het verbeteren van de schaalbaarheid en prestaties in een webtoepassing die wordt uitgevoerd op Microsoft Azure. De architectuur is gebaseerd op [Azure referentiearchitectuur: eenvoudige webtoepassing][basic-web-app]. Van toepassing op deze architectuur ook de aanbevelingen en overwegingen van dat artikel.

>[AZURE.NOTE] Azure heeft twee verschillende modellen: Resource Manager en klassiek. In dit artikel wordt een resourcemanager, wordt aangeraden voor nieuwe implementaties.

## <a name="architecture-diagram"></a>Architectuurdiagram

![[0]][0]

De architectuur, heeft de volgende onderdelen:

- **Resourcegroep**. Een [resourcegroep] [ resource-group] is een logische container voor Azure bronnen. 

- ** [Web app] [ app-service-web-app] ** en ** [API app][app-service-api-app]**. Een typische moderne toepassing kan zowel een website en een of meer RESTful web API's bevatten. Een web API kan worden verbruikt door browserclients via AJAX, native client-toepassingen of servertoepassingen. Zie [richtlijnen API]voor overwegingen op ontwerpen web API's[api-guidance].    

- **WebJob**. [Azure-WebJobs] gebruik[ webjobs] langdurige taken uitvoeren op de achtergrond. WebJobs kan worden uitgevoerd volgens een schema, continu, of in antwoord op een trigger, zoals een bericht in een wachtrij plaatsen. Een achtergrondproces in het kader van een app App-Service wordt uitgevoerd met een WebJob. 

- **Wachtrij**. In de architectuur die hier worden weergegeven, de toepassingswachtrijen achtergrond taken door het plaatsen van een bericht naar een [Azure Queue Storage] [ queue-storage] wachtrij. Het bericht wordt een functie in de WebJob geactiveerd. 

    U kunt ook wachtrijen Service Bus. Zie voor een vergelijking, [Azure wachtrijen en wachtrijen met Service Bus - vergeleken en tegenstelling tot][queues-compared].

- **Cache**. Semi-statische gegevens opslaan in [Cache van Azure bestand Vgx.][azure-redis].  

- **CDN**. [Azure inhoud levering] netwerk[ azure-cdn] (CDN) naar openbaar toegankelijke inhoud van de cache, voor lagere latentie en snellere levering van inhoud.

- **Opslag van gegevens.** [Azure SQL-Database] gebruik[ sql-db] voor relationele gegevens. Voor niet-relationele gegevens, kunt u overwegen een NoSQL-archief, zoals Azure tabelopslag of [DocumentDB][documentdb].

- **Azure zoeken**. [Azure] zoeken[ azure-search] -zoekfunctie, waaronder zoeksuggesties, fuzzy zoeken en taal-specifieke zoekactie toevoegen. Azure Search wordt meestal gebruikt in combinatie met een ander gegevensarchief, vooral als de primaire gegevensopslag strikte consistentie vereist. In deze benadering zou de bindende gegevens in de gegevensopslag en de zoekindex in Azure Search plaatsen. Azure Search kan ook worden gebruikt voor de consolidatie van een enkele zoekactie index uit meerdere gegevensopslagplaatsen.  

- **E-mail/SMS**. Als uw toepassing verzenden per e-mail of SMS-berichten moet, gebruikt u een service van derden zoals SendGrid of Twilio in plaats van deze functie rechtstreeks in de toepassing te bouwen.

## <a name="recommendations"></a>Aanbevelingen

### <a name="app-service-apps"></a>De Service App apps 

Wij raden u aan de webtoepassing en de web-API als afzonderlijke App Service apps maken. Dit ontwerp kunt u ze in afzonderlijke App Service plannen uit te voeren die op zijn beurt kunt u ze afzonderlijk geschaald. Als u geen niveau van schaalbaarheid op eerst, kunt u de apps implementeren in hetzelfde plan en ze in afzonderlijke plannen later verplaatst, indien nodig. (Voor de Basic-, Standard- en Premium-plannen u worden gefactureerd voor de VM exemplaren in het plan, niet per app. Zie [serviceprijzen App][app-service-pricing])

Als u van plan bent de *Eenvoudige tabellen* of *Eenvoudige API* -functies van mobiele Apps in App-Service wilt gebruiken, maakt u een afzonderlijke App Service app voor dit doel.  Deze functies zijn gebaseerd op een specifieke toepassing framework te kunnen.

### <a name="webjobs"></a>WebJobs

Als de WebJob veel bronnen is, overwegen het te installeren op een lege App Service app in een apart App Service plan te voorzien in specifieke gevallen de WebJob. [Achtergrond taken richtlijnen]Zie[webjobs-guidance].  

### <a name="cache"></a>Cache

U kunt de prestaties en schaalbaarheid verbeteren met behulp van [Azure bestand Vgx. Cache] [ azure-redis] om bepaalde gegevens in de cache. Overweeg het gebruik van de Cache voor bestand Vgx.:

- Semi-statische gegevens.

- Status van de sessie.

- HTML-uitvoer. Dit is nuttig in toepassingen die complexe HTML-uitvoer te renderen. 

Voor meer instructies gedetailleerde voor het ontwerpen van een strategie voor caching, Zie [richtlijnen voor Caching][caching-guidance].

### <a name="cdn"></a>CDN 

[Azure CDN] gebruiken[ azure-cdn] cache statische inhoud. Het belangrijkste voordeel van een CDN is minder latentie voor gebruikers, omdat inhoud is opgeslagen op een *edge-server* die geografisch dicht bij de gebruiker. CDN belasting van de toepassing, kan ook worden verminderd omdat dat verkeer niet wordt verwerkt door de toepassing.

- Als uw app voornamelijk uit statische pagina's bestaat, kunt u overwegen CDN cache de hele app. [Azure CDN in Azure App-Service]Zie[cdn-app-service].

- Anders bestanden statische inhoud, zoals afbeeldingen, CSS en HTML-, in Azure opslag en CDN gebruiken om deze bestanden in cache. Zie [een Account opslag met CDN integreren][cdn-storage-account].

> [AZURE.NOTE] Azure CDN kan niet voldoen aan inhoud waarvoor verificatie vereist is.

Voor meer informatie, Zie [richtlijnen voor Content Delivery Network (CDN)][cdn-guidance]. 

### <a name="storage"></a>Opslag

Moderne toepassingen worden vaak grote hoeveelheden gegevens verwerken. Om het schalen van de wolk, is het belangrijk de juiste opslag kiezen. Hier volgen enkele aanbevelingen voor de basislijn.  [Beoordeling van de Data Store mogelijkheden voor Polyglot persistentie oplossingen]vindt u meer gedetailleerde leidraden,[polyglot-storage].

U wilt opslaan | Voorbeeld | Aanbevolen opslag
--- | --- | ---
Bestanden | Afbeeldingen, documenten, PDF 's | Azure Blob-opslag
Sleutel/waarde-paren | Gebruikersprofielgegevens worden opgezocht door gebruikers-ID | Azure tabelopslag
Korte berichten die bestemd zijn voor verdere verwerking starten | Serviceaanvragen | Azure Queue Storage Service Bus wachtrij of Service Bus onderwerp
Niet-relationele gegevens, met een flexibele schema, vereisen basic opvragen | Productcatalogus | Document database, zoals Azure, DocumentDB, MongoDB of Apache CouchDB
Relationele gegevens, waarvoor ondersteuning voor uitgebreidere query, strikte schema en/of sterke samenhang | Productvoorraad | Azure SQL-Database

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

### <a name="app-service-app"></a>De Service App app

Als uw oplossing verschillende apps in App-Service bevat, kunt u ze afzonderlijke App serviceplannen. Deze benadering kunt u ze onafhankelijk van elkaar, schalen, omdat ze worden uitgevoerd op afzonderlijke instanties. Zie voor meer informatie over het schalen, de [schaalbaarheid, overwegingen met betrekking tot] [ basic-web-app-scalability] sectie van de [Basic web application architecture][basic-web-app].

Op dezelfde manier te plaatsen van een WebJob in een eigen indeling, zodat achtergrondtaken niet uitvoeren op dezelfde instanties waarmee HTTP-aanvragen verwerkt.  

### <a name="sql-database"></a>SQL-Database

Schaalbaarheid van een SQL-database verhogen door *sharding* de database &mdash; , horizontaal partitioneren van de database. Sharding kunt u horizontaal schalen uit de database met behulp van [Hulpmiddelen voor databases elastische][sql-elastic]. Potentiële voordelen van sharding zijn:

- Hogere doorvoersnelheid van de transactie.

- Query's sneller worden uitgevoerd via een subset van de gegevens. 

### <a name="azure-search"></a>Azure zoeken

Azure Search de overhead van complexe zoekacties uitvoeren vanuit de primaire gegevensopslag worden verwijderd en het is schaalbaar, zodat het laden te verwerken. Zie [resource schaalniveaus voor query's en indexering werklasten in Azure Search][azure-search-scaling].

## <a name="security-considerations"></a>Beveiligingsoverwegingen

### <a name="cross-origin-resource-sharing-cors"></a>Cross-oorsprong Resource Sharing (CORS)

Als u een website en web API als aparte apps maakt, kan de website client-side AJAX aanroepen naar de API kan doen tenzij u CORS inschakelen. 

> [AZURE.NOTE] Browserbeveiliging voorkomt dat een webpagina maken van AJAX-aanvragen naar een ander domein. Deze beperking wordt het beleid van dezelfde oorsprong genoemd en wordt voorkomen dat schadelijke upnaam gegevens lezen van een andere site. CORS is een W3C-standaard waarmee een server te versoepelen van het beleid van dezelfde oorsprong en bepaalde aanvragen van verschillende oorsprong bij het afwijzen van anderen.

App Services beschikt over ingebouwde ondersteuning voor CORS, zonder toepassingscode te schrijven. Zie [verbruiken een API app uit JavaScript aan met CORS][cors]. De website toevoegen aan de lijst met toegestane oorsprong voor de API. 

### <a name="sql-database-encryption"></a>Codering voor SQL-Database

[Transparante gegevenscodering] gebruiken[ sql-encryption] als u nodig hebt om gegevens in rust in de database te coderen. Deze functie voert real-time codering en decodering van een volledige database (inclusief back-ups en transactielogboekbestanden), zonder dat wijzigingen in de toepassing. Codering houdt sommige latentie, er is een goede gewoonte om te scheiden van de gegevens die moeten worden beveiligd in de eigen database en codering alleen voor die database inschakelen.  

## <a name="next-steps"></a>Volgende stappen

- Voor een hogere mate van beschikbaarheid, de toepassing in meer dan één gebied implementeren en gebruiken van [Azure verkeer Manager] [ tm] voor failover. Zie voor meer informatie [Azure referentiearchitectuur: webtoepassing met hoge beschikbaarheid][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Webtoepassing in Azure met verbeterde schaalbaarheid"