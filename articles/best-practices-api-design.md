<properties
   pageTitle="Ontwerpondersteuning API | Microsoft Azure"
   description="Hulp bij het maken van een goed ontworpen API."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="api-design-guidance"></a>Ontwerpondersteuning API

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Overzicht

Veel moderne weboplossingen maken het gebruik van webservices, die zijn ondergebracht op webservers bieden functionaliteit voor externe client toepassingen. De bewerkingen die een webservice beschikbaar worden gesteld, vormen een web API. Een goed ontworpen web API gericht dient te ondersteunen:

- **Platform-onafhankelijkheid**. Toepassingen die moeten gebruikmaken van de API waarmee de webservice zonder dat de wijze waarop gegevens of bewerkingen die API maakt fysiek zijn geïmplementeerd worden. Hiervoor is vereist dat de API houdt zich aan de gemeenschappelijke normen die een client toepassing en service afspreken welke gegevensindelingen die u wilt gebruiken en de structuur van de gegevens die worden uitgewisseld tussen client-toepassingen en de web-service in te schakelen.

- **Evolutie van de Service**. De webservice moeten kunnen ontwikkelen en toevoegen (of verwijderen) functionaliteit onafhankelijk van clienttoepassingen. Bestaande client-toepassingen moet kunnen blijven functioneren niet gewijzigd als de functies van de web-service wijzigen. Alle functies ook moet worden gedetecteerd, zodat u volledig door clienttoepassingen kunnen gebruiken.

Het doel van deze richtsnoeren is worden de problemen beschreven waarmee u rekening houden moet bij het ontwerpen van een web-API.

## <a name="introduction-to-representational-state-transfer-rest"></a>Inleiding tot Representational State Transfer (REST)

Roy Fielding voorgesteld zijn dissertation in 2000, een alternatieve architecturale benadering voor het structureren van de bewerkingen die door de webservices; REST. REST is een architectonische stijl voor het bouwen van gedistribueerde systemen op basis van hypermedia op basis. Het belangrijkste voordeel van het model voor de REST is dat is gebaseerd op open standaarden en de uitvoering van het model of de clienttoepassingen die een specifieke implementatie tot niet wordt gebonden. Bijvoorbeeld, een REST-webservice kan worden geïmplementeerd met behulp van de API Microsoft ASP.NET Web en client-toepassingen kunnen worden ontwikkeld met behulp van taal- en toolset die kan HTTP-verzoeken te genereren en HTTP-antwoorden worden geparseerd.

> [AZURE.NOTE] REST werkelijk onafhankelijk is van een onderliggend protocol en niet noodzakelijkerwijs is gekoppeld aan een HTTP. Meest voorkomende implementaties van systemen die zijn gebaseerd op de REST gebruiken echter HTTP application protocol voor het verzenden en ontvangen van aanvragen. Dit document ligt de nadruk op REST beginselen toe te wijzen aan systemen die zijn ontworpen om te rijden met behulp van HTTP.

De REST model gebruikt een navigatie schema voor het weergeven van objecten en services via een netwerk (hierna aangeduid als _bronnen_). Veel systemen REST meestal implementeren gebruik van het HTTP-protocol voor het verzenden van verzoeken om toegang tot deze bronnen. In deze systemen dient een aanvraag in de vorm van een URI die een resource wordt geïdentificeerd en een HTTP-methode (de meest voorkomende wordt u, POST, PUT of verwijderen) wordt aangegeven welke bewerking moet worden uitgevoerd op de bron in een clienttoepassing.  Het hoofdgedeelte van de HTTP-aanvraag bevat de gegevens die vereist zijn voor het uitvoeren van de bewerking. Het belangrijkste om te weten is dat REST een stateless aanvraag model definieert. HTTP-aanvragen moeten onafhankelijk en treedt op in willekeurige volgorde, wilt behouden, tijdelijke statusgegevens tussen aanvragen en is dus niet haalbaar is.  De enige plaats waar gegevens worden opgeslagen in de resources zelf, en elke aanvraag moet een atomaire bewerking. Effectief, implementeert een model voor de REST een eindige toestandsmachine waarbij een aanvraag voor een resource uit een welomschreven tijdelijke staat overgangen.

> [AZURE.NOTE] De staatloze aard van afzonderlijke aanvragen in het model voor de REST kunt een samengesteld aan de hand van deze beginselen zijn uiterst schaalbaar systeem. Er is niet nodig te behouden de affiniteit tussen een clienttoepassing maken van een reeks van aanvragen en de specifieke webservers die aanvragen afhandelen.

Een ander belangrijk punt bij de uitvoering van een effectief model voor de REST is het begrijpen van de relaties tussen de verschillende bronnen voor het model toegang verschaft. Deze bronnen worden gewoonlijk geordend in verzamelingen en relaties. Stel bijvoorbeeld dat een snelle analyse van een e-commerce systeem blijkt dat er twee verzamelingen waarin toepassingen zijn waarschijnlijk geïnteresseerd zijn: orders en klanten. Elke order en de klant moet een eigen unieke sleutel voor identificatiedoeleinden. De URI voor toegang tot het verzamelen van orders kan iets eenvoudigs als _/orders_en ook de URI voor het ophalen van alle klanten _/customers_kan zijn. Afgifte van een HTTP GET-verzoek naar de _/orders_ URI moet een lijst van alle orders in de collectie gecodeerd als een HTTP-antwoord retourneren:

```HTTP
GET http://adventure-works.com/orders HTTP/1.1
...
```

De onderstaande reactie codeert de orders als de structuur van een JSON lijst:

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
[{"orderId":1,"orderValue":99.90,"productId":1,"quantity":1},{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2},{"orderId":3,"orderValue":16.60,"productId":2,"quantity":4},{"orderId":4,"orderValue":25.90,"productId":3,"quantity":1},{"orderId":5,"orderValue":99.90,"productId":1,"quantity":1}]
```
Moeten de id voor de order van de bron van de _orders_ , zoals _/orders/2_op te geven om een afzonderlijke order ophalen:

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
```

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2}
```

> [AZURE.NOTE] Voor eenvoud ziet deze voorbeelden u de informatie in antwoorden worden geretourneerd als JSON tekstgegevens. Er is echter geen reden waarom bronnen moeten geen andere typen die worden ondersteund door HTTP, zoals binaire of gecodeerde informatie; het content-type in de HTTP-antwoord moet geven het type. Ook een model voor de REST mogelijk om terug te keren dezelfde gegevens in verschillende indelingen, zoals XML of JSON. In dit geval moeten de webservice kunnen uitvoeren van inhoud onderhandelingen met de client die de aanvraag doet. De aanvraag kan bestaan uit een _Accept_ -header die de beste indeling die de client wilt ontvangen en de web-service moet proberen om deze indeling indien mogelijk.

U ziet dat de reactie van een aanvraag voor de REST wordt gebruik gemaakt van de standaard-HTTP-statuscodes. Aanvraag voor een geldige gegevens retourneert moet bijvoorbeeld de HTTP-reactiecode 200 (OK), terwijl een aanvraag die niet kunnen vinden of verwijderen van een opgegeven resource moet het resultaat de HTTP-statuscode 404 (niet gevonden bevat).

## <a name="design-and-structure-of-a-restful-web-api"></a>Ontwerp en de structuur van een RESTful web API

De toetsen op het ontwerpen van een succesvolle web API zijn eenvoud en consistentie. Een Web-API die deze twee factoren vertoont gemakkelijker te maken die nodig zijn voor het gebruik van de API-clienttoepassingen.

Een RESTful web API is gericht op de blootstelling van een set gekoppelde bronnen en bieden de kernactiviteiten waarmee een toepassing te manipuleren van deze bronnen en eenvoudig te navigeren tussen hen. Daarom moet de URI's die vormen een typische RESTful web API worden gericht op de gegevens die worden getoond en de faciliteiten die HTTP gebruiken om te rijden op deze gegevens. Deze aanpak vereist een andere denkt dat doorgaans wordt gebruikt bij het ontwerpen van een set klassen in een object-georiënteerde API doorgaans meer worden gemotiveerd door het gedrag van objecten en klassen. Bovendien moet een RESTful web API stateless en niet afhankelijk zijn van bewerkingen in een bepaalde volgorde worden ingeroepen. In de volgende secties worden samengevat de punten die u overwegen moet bij het ontwerpen van een RESTful web API.

### <a name="organizing-the-web-api-around-resources"></a>Het web API om resources indelen

> [AZURE.TIP] De URI's REST-webservice toegankelijk moet zijn gebaseerd op zelfstandige naamwoorden (de gegevens waarop het web API toegang biedt) en geen werkwoorden (wat een toepassing kunt doen met de gegevens).

Focus op de business entiteiten die het web API beschikbaar worden gesteld. Bijvoorbeeld in een web API ter ondersteuning van de e-commerce systeem die eerder zijn beschreven, zijn de primaire diensten klanten en orders. Processen, zoals de handeling van het plaatsen van een order kunnen worden bereikt door middel van een HTTP POST-bewerking die ervoor zorgt dat de gegevens toegevoegd aan de lijst met orders voor de klant. Intern, uitvoeren deze POST-bewerking taken, zoals controleren voorraadniveaus en de klant wordt gefactureerd. Het HTTP-antwoord kunt geven aan of de order is geplaatst is of niet. Vergeet niet dat een resource niet hoeft te worden gebaseerd op een enkele fysieke gegevensitem. Als u bijvoorbeeld mogelijk een order bron intern worden geïmplementeerd met behulp van gegevens uit meerdere rijen verdeeld over meerdere tabellen in een relationele database, maar aangeboden aan de client als een eenheid worden samengevoegd.

> [AZURE.TIP] Vermijd het ontwerpen van een interface voor de REST komt overeen met of afhankelijk is van de interne structuur van de gegevens die worden getoond. REST is over de implementatie meer dan eenvoudige CRUD (maken, ophalen, Update, Delete)-bewerkingen via afzonderlijke tabellen in een relationele database. Het doel van de REST is business entiteiten en de bewerkingen die door een toepassing van deze entiteiten om de fysieke implementatie van deze entiteiten te kan uitvoeren, maar een client niet worden blootgesteld aan deze fysieke gegevens toewijzen.

Afzonderlijke bedrijfsentiteiten bestaan zelden in isolatie (Hoewel sommige objecten singleton bestaat), maar in plaats daarvan worden gegroepeerd in verzamelingen. Zijn bronnen in de overige bepalingen, elke entiteit en elke collectie. Elke collectie heeft een eigen URI in de web-service in een RESTful web API en een lijst met items in de collectie die een HTTP GET-aanvraag uitvoeren via een URI voor een collectie opgehaald. Elk afzonderlijk artikel heeft ook een eigen URI en een toepassing een andere HTTP GET-aanvraag met de URI die de details van dat item ophalen kunt indienen. U moet de URI's voor verzamelingen en artikelen hiërarchisch ordenen. De URI- _/customers_ geeft de collectie van de klant in het e-commerce-systeem en _/customers/5_ de details voor de klant één met de 5-ID uit deze collectie opgehaald. Deze aanpak voorkomt dat het web API intuïtief.

> [AZURE.TIP] Vaststellen van een consistente naamgeving in URI's; in het algemeen helpt het meervoud zelfstandige naamwoorden gebruiken voor URI's die verwijzing collecties.

Ook moet u rekening houden met de relaties tussen de verschillende soorten bronnen en hoe deze koppelingen kan worden blootgesteld. Klanten kunnen bijvoorbeeld nul of meer orders te plaatsen. Een natuurlijke manier om aan te geven van deze relatie is via een URI zoals _/customers/5/orders_ alle orders voor klant 5. U kunt ook resultaat van de koppeling van een order terug naar een specifieke klant via een URI zoals _/orders/99/customer_ om de klant voor de order 99, maar belastend is voor de uitvoering van dit model te ver uitbreiden kan worden. Een betere oplossing is gekoppeld bevinden zich koppelingen naar bronnen als de klant, in het hoofdgedeelte van de HTTP response-bericht geretourneerd wanneer de order wordt opgevraagd. Dit mechanisme is meer gedetailleerd beschreven in de sectie met de aanpak van HATEOAS navigatie inschakelen op Verwante bronnen verderop in deze handleiding.

In complexere systemen kunnen er veel meer soorten entiteit kan, en verleidelijk om URI's waarmee een clienttoepassing om te navigeren door de verschillende niveaus van relaties, zoals _/customers/1/orders/99/products_ voor de lijst van producten in bestelling geplaatst door 1 klant 99. Echter, dit niveau van complexiteit kan moeilijk te onderhouden en niet als u de relaties tussen bronnen wijzigen in de toekomst kan worden. In plaats daarvan moet u contact houden URI's relatief eenvoudig. Houd er rekening mee dat wanneer een toepassing een verwijzing naar een bron heeft, moet deze verwijzing gebruiken om te zoeken naar items die verband houden met die resource. De voorgaande query kan worden vervangen door de URI- _/customers/1/orders_ te zoeken naar alle orders voor klant 1 en vervolgens de URI _/orders/99/products_ query om de producten in deze volgorde (ervan uitgaande order 99 is geplaatst door 1 klant).

> [AZURE.TIP] Dat bron-URI's die complexer zijn dan de _item-collection-collectie_.

Een ander punt te overwegen is dat alle webverzoeken opleggen van een belasting op de webserver, en hoe groter het aantal aanvragen voor de groter de belasting. U moet proberen te definiëren uw resources om te voorkomen dat "chatty" web API's die een groot aantal kleine bronnen worden blootgesteld. Een dergelijke API mogelijk een clienttoepassing naar meerdere aanvragen indienen om te zoeken naar alle gegevens die vereist zijn. Kan het nuttig zijn te denormalize gegevens en verwante gegevens samenvoegen tot een grotere bronnen die kunnen worden opgehaald via de uitgifte van een enkele aanvraag combineren. U moet echter deze aanpak ten opzichte van de overhead van het ophalen van gegevens die niet vaak nodig is door de client in evenwicht. Ophalen van grote objecten kunt verhogen de latentie van een aanvraag en kosten extra bandbreedte voor weinig voordeel als de extra gegevens niet vaak worden gebruikt.

Voorkomen dat de introductie van afhankelijkheden tussen de web API om de structuur, het type of de locatie van de onderliggende gegevensbronnen. Bijvoorbeeld, als uw gegevens zich in een relationele database, hoeft de web-API niet elke tabel weergegeven als een verzameling bronnen. Het web API beschouwen als een abstractie van de database en zo nodig een laag toewijzing tussen de database en het web API introduceren. Op deze manier als het ontwerp of de implementatie van de database wordt gewijzigd (bijvoorbeeld u verplaatsen uit een relationele database met een collectie van de genormaliseerde tabellen aan een systeem gedenormaliseerde NoSQL opslag, zoals een database van document) client-toepassingen zijn geïsoleerd uit deze wijzigingen.
> [AZURE.TIP] De bron van de gegevens die dient ter ondersteuning van web-API is niet noodzakelijk een gegevensarchief; is het mogelijk een andere service of toepassing van bedrijfs of zelfs een oudere toepassing die wordt uitgevoerd op-ruimten binnen een organisatie.

Ten slotte, het is niet mogelijk elke bewerking die wordt uitgevoerd door een web API voor een bepaalde resource toewijzen. U kunt dergelijke _niet-resource_ scenario's via HTTP GET-aanvragen die een stukje functionaliteit aanroepen en het resultaat als een HTTP-response-bericht verwerken. Een web-API die een eenvoudige rekenmachine-achtige bewerkingen zoals implementeert toevoegen en aftrekken kan URI's die deze bewerkingen als pseudo-bronnen weergeven en gebruiken om op te geven van de vereiste parameters in de query-tekenreeks bevatten. Bijvoorbeeld een GET-verzoek aan de URI _/add?operand1 = 99 & operand2 = 1_ kan geen responsbericht retourneren aan de instantie die de waarde 100 en GET-verzoek aan de URI _/subtract?operand1 = 50 & operand2 = 20_ kan geen responsbericht retourneren aan de instantie die de waarde 30. Alleen maar deze vormen van URI's spaarzaam gebruiken.

### <a name="defining-operations-in-terms-of-http-methods"></a>Bewerkingen van HTTP-methoden definiëren

Het HTTP-protocol definieert een aantal methoden die semantische betekenis aan een aanvraag toewijzen. Veelgebruikte HTTP-methoden door de meest RESTful web API's zijn:

- **Ophalen**, een kopie van de resource op de opgegeven URI te halen. De hoofdtekst van het antwoordbericht bevat de details van de aangevraagde bron.

- **Boeken**, een nieuwe bron maken in de opgegeven URI. De hoofdtekst van het bericht bevat de details van de nieuwe resource. Opmerking boeken kan ook worden gebruikt voor het starten van bewerkingen die geen middelen daadwerkelijk maakt.

- **Plaatsen**, vervangen of bijwerken van de resource op de opgegeven URI. De hoofdtekst van het bericht bevat de bron moet worden gewijzigd en de waarden moeten worden toegepast.

- **Verwijderen**, de resource op de opgegeven URI te verwijderen.

> [AZURE.NOTE] Het HTTP-protocol definieert ook andere minder gebruikte methoden, zoals PATCH die wordt gebruikt voor selectieve bijwerken tot een bron, HEAD die wordt gebruikt voor het aanvragen van een beschrijving van een bron, opties waarmee een clientinformatie verkrijgen van informatie over de communicatie-instellingen worden ondersteund door de server, en waarmee een client informatie opvragen die kan worden gebruikt voor test-en diagnostische gegevens traceren.

Het effect van een specifiek verzoek dient afhankelijk of de resource die wordt toegepast een verzameling of een afzonderlijk artikel is. In de volgende tabel bevat een overzicht van de algemene conventies aangenomen door meest RESTful met behulp van het voorbeeld van e-commerce-implementaties. Houd er rekening mee dat niet alle van deze aanvragen kunnen worden uitgevoerd; Dit is afhankelijk van het specifieke scenario.

| **Resource** | **Verzenden** | **Toevoegen** | **PLAATSEN** | **VERWIJDEREN** |
|--------------|----------|---------|---------|------------|
| /Customers | Een nieuwe klant | Alle klanten ophalen | Bulk-update van klanten (_indien uitgevoerd_) | Verwijder alle klanten |
| klanten/1 | Fout | De details voor klant 1 ophalen | De details van de klant 1 indien aanwezig, anders retourneren een fout bij bijwerken | 1 klant verwijderen |
| /Customers/1/orders | Maak een nieuwe order voor klant 1 | Alle bestellingen voor klant 1 ophalen | Bulk-update van orders voor klant 1 (_indien uitgevoerd_) | Verwijder alle orders voor klant 1 (_indien uitgevoerd_) |

Het doel van de GET- en DELETE-verzoeken zijn betrekkelijk eenvoudig, maar er is ruimte voor verwarring over het doel en de gevolgen van de POST en PUT-aanvragen.

Een POST-aanvraag moet u een nieuwe bron maken met gegevens in het hoofdgedeelte van de aanvraag. In het model REST toepassen u vaak POST-aanvragen voor bronnen die verzamelingen zijn; de nieuwe resource wordt toegevoegd aan de collectie.

> [AZURE.NOTE] Ook kunt u POST-aanvragen die bepaalde functionaliteit activeren (en die niet noodzakelijkerwijs gegevens), en een dergelijke aanvraag collecties kunnen worden toegepast. U kunt bijvoorbeeld een POST-aanvraag voor een rooster doorgeven aan een service payroll verwerking en de berekende btw terugkrijgen als reactie.

Een PUT-verzoek is bedoeld voor het wijzigen van een bestaande bron. Als de opgegeven resource niet bestaat, de aanvraag plaatsen kan retourneert een fout (in sommige gevallen kan het mogelijk daadwerkelijk de bron maakt). PUT-aanvragen worden meest toegepast op resources die afzonderlijke items (zoals een bepaalde klant of order), hoewel zij kunnen worden toegepast op de collecties, hoewel dit minder vaak wordt geïmplementeerd. Merk op dat de PUT-zijn idempotency is ingeschakeld aanvragen terwijl POST-aanvragen niet worden; Als u een aanvraag indient dezelfde moeten aanvraag plaatsen meerdere keren de resultaten altijd hetzelfde (dezelfde bron worden gewijzigd met dezelfde waarden), maar als een toepassing de dezelfde POST-aanvraag herhaalt het resultaat is het maken van meerdere bronnen.

> [AZURE.NOTE] PLAATST van de HTTP-aanvraag wordt strikt genomen een bestaande bron vervangen door de resource die is opgegeven in het hoofdgedeelte van de aanvraag. Als de bedoeling is te wijzigen van een selectie van eigenschappen in een bron, maar andere eigenschappen ongewijzigd laat, moet vervolgens dit worden geïmplementeerd met behulp van een PATCH van de HTTP-aanvraag. Echter, veel RESTful implementaties ontspannen deze regel en opslag gebruiken voor beide situaties.

### <a name="processing-http-requests"></a>Verwerking van HTTP-aanvragen
De gegevens die door een toepassing in een groot aantal HTTP-aanvragen en de bijbehorende antwoordberichten van de webserver kan worden gepresenteerd in verschillende indelingen (of typen). De gegevens waarmee de gegevens voor een klant of bestelling kan bijvoorbeeld worden opgegeven als XML, JSON, of een andere gecodeerde en gecomprimeerde indeling. Een RESTful web API moet ondersteuning voor verschillende mediatypen zoals aangevraagd door de clienttoepassing die een aanvraag indient.

Wanneer een clienttoepassing een aanvraag waarvoor gegevens worden geretourneerd in de hoofdtekst van een bericht verzendt, kan deze de mediatypen die kunnen worden verwerkt in de Accept-header van de aanvraag opgeven. De volgende code illustreert een HTTP GET-aanvraag die wordt opgehaald van de gegevens van de klant 1 en vraagt het resultaat moet worden gegeven als JSON (de client moet nog steeds het mediatype van de gegevens in het antwoord om te controleren of de notatie van de geretourneerde gegevens onderzoeken):

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
Accept: application/json
...
```

Als de webserver dit mediatype ondersteunt, kan het antwoord met een respons die de header Content-Type waarmee de indeling van de gegevens in de hoofdtekst van het bericht bevat:

> [AZURE.NOTE] De media typen waarnaar wordt verwezen in de kopteksten voor accepteren en Content-Type moeten worden herkend voor maximale interoperabiliteit, MIME-typen in plaats van sommige aangepaste mediatype.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":2,"productID":4,"quantity":2,"orderValue":10.00}
```

Als de server het aangevraagde medium-type niet ondersteunt, kan de gegevens in een andere indeling verzenden. IN alle gevallen moet deze het mediumtype (zoals _application/json_) in de header Content-Type opgeven. Het is de verantwoordelijkheid van de clienttoepassing het antwoordbericht geparseerd en de resultaten van het bericht op de juiste manier worden geïnterpreteerd.

Houd er rekening mee dat in dit voorbeeld wordt de webserver is de gevraagde gegevens worden opgehaald en geeft een geslaagde bewerking door weer een statuscode van 200 in de kopregel van het antwoord. Geen overeenkomende gegevens zijn gevonden, er moet in plaats daarvan de statuscode 404 (niet gevonden) geretourneerd als de hoofdtekst van het antwoordbericht kan extra informatie bevatten. De indeling van deze gegevens wordt bepaald door de header Content-Type, zoals in het volgende voorbeeld wordt getoond:

```HTTP
GET http://adventure-works.com/orders/222 HTTP/1.1
...
Accept: application/json
...
```

Order 222 bestaat niet, zodat het antwoordbericht zien er zo uit:

```HTTP
HTTP/1.1 404 Not Found
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"message":"No such order"}
```

Wanneer een toepassing een PUT HTTP-aanvraag voor het bijwerken van een resource verzendt, geeft de URI van de bron en de gegevens worden gewijzigd in de hoofdtekst van het bericht. Het moet ook de indeling van deze gegevens met behulp van de header Content-Type opgeven. Een algemene bestandsindeling die wordt gebruikt voor tekst-gebaseerde informatie uit is _application/x-1-800-www-Dell-form-urlencoded_, die bestaat uit een set van naam/waarde-paren, gescheiden door het & teken. In het volgende voorbeeld ziet u een PUT HTTP-verzoek dat Hiermee wijzigt u de gegevens in de volgorde 1:

```HTTP
PUT http://adventure-works.com/orders/1 HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
ProductID=3&Quantity=5&OrderValue=250
```

Als de wijziging voltooid is, moet het in het ideale geval reageren met een statuscode HTTP 204, die aangeeft dat het proces met succes zijn verwerkt, maar dat de hoofdtekst van het antwoord bevat geen verdere informatie. De kop van de locatie in het antwoord bevat de URI van de bijgewerkte bron:

```HTTP
HTTP/1.1 204 No Content
...
Location: http://adventure-works.com/orders/1
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

> [AZURE.TIP] Als de gegevens in een PUT http-request-bericht informatie over datum en tijd bevat, zorg ervoor dat uw webservice datums accepteert en tijden opgemaakt volgens de ISO 8601 norm.

Als de resource moet worden bijgewerkt niet bestaat nog, wordt de webserver kunt reageren met een antwoord niet gevonden zoals eerder beschreven. Ook als de server het object zelf maakt kan deze retourneert de status codes HTTP-200 (OK) of HTTP-201 (gemaakt) en de reactie-instantie kunnen de gegevens bevatten die voor de nieuwe bron. Als de header Content-Type van de aanvraag in een indeling die de webserver niet kan verwerken, moet deze reageren met de HTTP-statuscode 415 (niet-ondersteund mediatype).

> [AZURE.TIP] Overweeg de implementatie van HTTP PUT bulkbewerkingen die u kunnen batch-updates van meerdere bronnen in een collectie. PUT-verzoek moet de URI van de collectie opgeven en het hoofdgedeelte van de aanvraag moet geeft de details van de middelen moet worden gewijzigd. Deze benadering kunt u chattiness beperken en de prestaties verbeteren.

De indeling van een HTTP POST-aanvragen dat nieuwe resources maken vergelijkbaar zijn met die van de PUT-aanvragen; de berichttekst bevat de details van de nieuwe bron wilt toevoegen. De URI bevat echter meestal de collectie waaraan u de resource worden toegevoegd. In het volgende voorbeeld wordt een nieuwe order gemaakt en toegevoegd aan de collectie orders:

```HTTP
POST http://adventure-works.com/orders HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
productID=5&quantity=15&orderValue=400
```

Als de aanvraag voltooid is, moet de server reageren met een bericht met de HTTP-statuscode 201 (gemaakt). De locatie-header bevat de URI van de zojuist gemaakte bron en de hoofdtekst van het antwoord bevat een kopie van het nieuwe middel; de header Content-Type geeft de indeling van deze gegevens:

```HTTP
HTTP/1.1 201 Created
...
Content-Type: application/json; charset=utf-8
Location: http://adventure-works.com/orders/99
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":99,"productID":5,"quantity":15,"orderValue":400}
```

> [AZURE.TIP] Als de gegevens van een PUT- of POST-aanvraag ongeldig is, moet de server reageren met een bericht met de HTTP-statuscode 400 (onjuiste aanvraag). De tekst van dit bericht als u meer informatie over het probleem met het verzoek en de verwachte opmaak kan bevatten of een koppeling naar een URL die meer details kan bevatten.

Als u een resource verwijdert, biedt verwijderen van de HTTP-aanvraag simpelweg de URI van de resource moet worden verwijderd. In het volgende voorbeeld wordt geprobeerd te verwijderen 99:

```HTTP
DELETE http://adventure-works.com/orders/99 HTTP/1.1
...
```

Als de bewerking voltooid is, moet de server reageren met de HTTP-statuscode 204, die aangeeft dat het proces met succes zijn verwerkt, maar dat de hoofdtekst van het antwoord bevat geen verdere informatie (dit is hetzelfde antwoord geretourneerd door een PUT-bewerking geslaagd, maar zonder header locatie als de resource niet langer bestaat.) Het is ook mogelijk dat een verzoek tot verwijdering naar de HTTP-statuscode 200 (OK) of 202 (geaccepteerde) als resultaat als de verwijdering wordt asynchroon uitgevoerd.

```HTTP
HTTP/1.1 204 No Content
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

Als de bron niet wordt gevonden, moet de server in plaats daarvan een 404 (niet gevonden) bericht retourneren.

> [AZURE.TIP] Als alle bronnen in een collectie worden verwijderd moeten, kunt u een verwijderen van de HTTP-aanvraag worden opgegeven voor de URI van de collectie in plaats van dat elke resource weer te verwijderen uit de collectie van een toepassing.

### <a name="filtering-and-paginating-data"></a>Filteren en het pagineren van gegevens

U moet trachten te houden van de URI's eenvoudig en intuïtief. Een verzameling bronnen via een enkele blootstelling URI helpt in dit opzicht, maar dat kan leiden tot toepassingen die grote hoeveelheden gegevens worden opgehaald wanneer u slechts een subset van de gegevens is vereist. Genereren van een grote hoeveelheid verkeer heeft gevolgen voor niet alleen de prestaties en de schaalbaarheid van de webserver, maar ook negatieve invloed hebben op de reactietijd van toepassingen die de gegevens opvraagt.

Bijvoorbeeld als orders de prijs die betaald is voor de order bevatten, een clienttoepassing die nodig zijn voor het ophalen van alle orders waarvoor de kosten over een bepaalde waarde moet mogelijk alle orders ophalen uit de _/orders_ URI en filtert vervolgens lokaal deze orders. Duidelijk is dit proces zeer inefficiënt; het afval netwerk bandbreedte en verwerking power op de server die als host fungeert voor het web API.

Een oplossing kan zijn om een URI-schema zoals _/orders/ordervalue_greater_than_n_ , waarbij _n_ staat voor de prijs, maar voor alle maar een beperkt aantal van de prijzen van een dergelijke aanpak is niet praktisch. Bovendien, als u moet de query orders op basis van andere criteria, loopt u geconfronteerd worden met voorzien van een lange lijst van URI's mogelijk niet-intuïtieve namen.

Een betere strategie voor het filteren van gegevens is het leveren van de filtercriteria in de queryreeks die wordt doorgegeven aan het web API, zoals _/orders?ordervaluethreshold n =_. In dit voorbeeld wordt de overeenkomstige handeling in het web API verantwoordelijk is voor het parseren en de verwerking van de `ordervaluethreshold` -parameter in de querytekenreeks en de gefilterde resultaten in de HTTP-reactie retourneren.

Sommige eenvoudige HTTP GET-aanvragen via de collectie resources kunnen mogelijk een groot aantal artikelen retourneren. Ter bestrijding van de mogelijkheid van deze die u moeten ontwerpen is het web API te beperken van de hoeveelheid gegevens die wordt geretourneerd door een enkele aanvraag. Dit kunt u bereiken door de ondersteuning van de query-tekenreeks waarmee de gebruiker geeft het maximum aantal items worden opgehaald (dit kan zichzelf met een maximum bovengrens Denial of Service-aanvallen voorkomen), en een verschuiving van het begin in de collectie. De querytekenreeks van de URI _/orders?limit = 25 & offset = 50_ 25 orders vanaf de 50e order gevonden in de collectie orders moeten worden opgehaald. Met het filteren van gegevens, de bewerking die het GET-verzoek in het web API implementeert is verantwoordelijk voor het parseren en verwerking van de `limit` en `offset` parameters in de query-tekenreeks. Voor clienttoepassingen krijgen aanvragen die terug gepagineerde gegevens behoren ook een vorm van metagegevens die aangeven van het totale aantal bronnen die beschikbaar zijn in de collectie. U kunt ook andere intelligente paginering strategieën; Zie voor meer informatie [API ontwerpnotities: Smart pagineren](http://bizcoder.com/api-design-notes-smart-paging)

U kunt volgen een soortgelijke strategie voor het sorteren van gegevens zoals deze is opgehaald; Zo kunt u opgeven dat een sort-parameter die de naam van een veld de waarde, zoals _/orders?sort = ProductID_. Bedenk wel dat deze aanpak kan schadelijke gevolgen hebben voor caching (query-parameters string deel uitmaken van de resource-id die door veel implementaties van de cache als de sleutel tot de gegevens in de cache).

U kunt deze benadering te beperken (project) uitbreiden de velden die worden geretourneerd als een afzonderlijke resource-item een grote hoeveelheid gegevens bevat. Zo kunt u een query string parameter een door komma's gescheiden lijst met velden, zoals accepteert _/orders?fields = ProductID, aantal_.

> [AZURE.TIP] Geef alle optionele parameters in de query-tekenreeks zinvolle standaardwaarden. Zo instellen dat de `limit` parameter tot en met 10 en de `offset` parameter op 0 als u paginering implementeert, stelt u de parameter sorteren op de sleutel van de resource als u implementeert bestellen en stel de `fields` parameter op alle velden in de bron als u ondersteuning voor projecties.

### <a name="handling-large-binary-resources"></a>Verwerken van grote binaire resources

Een enkele bron kan grote binaire velden, zoals bestanden of afbeeldingen bevatten. Te overwinnen de transmissie problemen veroorzaakt door onbetrouwbare en intermitterende verbindingen en voor een betere responstijden, overwegen bewerkingen die kunnen dergelijke bronnen in stukken worden opgehaald door de clienttoepassing. Hiervoor moet het web API ondersteunen de kop akkoord bereiken voor GET-aanvragen voor grote bronnen en HTTP HEAD-aanvragen voor deze bronnen in het ideale geval implementeren. De kop akkoord bereiken geeft aan dat de bewerking ophalen uitgevoerd gedeeltelijke resultaten ondersteunt, dat een clienttoepassing GET-aanvragen die een subset van een resource die is opgegeven als een reeks bytes terug kunt indienen. Een HEAD-verzoek is vergelijkbaar met een GET-aanvraag met dien verstande dat retourneert alleen een header waarin de bron en een lege berichttekst worden beschreven. Een HEAD-verzoek om te bepalen of een bron ophalen via de gedeeltelijke GET-aanvragen kan worden verleend door een clienttoepassing. In het volgende voorbeeld ziet u een HEAD-verzoek dat verkrijgt informatie over de productafbeelding van een:

```HTTP
HEAD http://adventure-works.com/products/10?fields=productImage HTTP/1.1
...
```

Het antwoordbericht bevat een kop met de grootte van de bron (4580 bytes) en de kop van het akkoord bereiken dat de bijbehorende GET-bewerking ondersteunt gedeeltelijke resultaten:

```HTTP
HTTP/1.1 200 OK
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 4580
...
```

De clienttoepassing kunt u deze informatie gebruiken om een reeks bewerkingen ophalen voor het ophalen van de afbeelding in kleinere stukken samen te stellen. De eerste aanvraag worden de eerste 2500 bytes opgehaald met behulp van de kop van het bereik:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=0-2499
...
```

Het antwoordbericht geeft aan dat dit een gedeeltelijk antwoord door HTTP-statuscode 206 terug te keren. De header Content-Length geeft het feitelijke aantal bytes dat is geretourneerd in de hoofdtekst van het bericht (niet de grootte van de bron) en de header Content-Range geeft aan welk deel van de resource dit (0-2499 bytes van 4580):

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2500
Content-Range: bytes 0-2499/4580
...
_{binary data not shown}_
```

Een volgende aanvraag van de clienttoepassing kunt ophalen van de rest van de resource met behulp van een geschikt bereik header:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=2500-
...
```

Het overeenkomstige resultaat bericht ziet er zo uit:

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2080
Content-Range: bytes 2500-4580/4580
...
```

## <a name="using-the-hateoas-approach-to-enable-navigation-to-related-resources"></a>Met behulp van de methode HATEOAS om te navigeren naar gerelateerde bronnen

Een van de primaire motieven achter de REST is moet kunnen navigeren de volledige set bronnen zonder voorafgaande kennis van de URI-schema. Elk HTTP GET-verzoek moet retourneren de informatie nodig om te zoeken naar resources die rechtstreeks verband houden met het gevraagde object via hyperlinks opgenomen in het antwoord, en moet ook worden verstrekt met informatie over de beschikbare bewerkingen voor elk van deze bronnen. Dit principe heet HATEOAS of Hypertext als de motor van de toepassing staat. Het systeem is in feite een eindige toestandsmachine en het antwoord op elke aanvraag bevat de informatie die nodig is voor het verplaatsen van de ene toestand naar de andere; geen andere informatie moet nodig zijn.

> [AZURE.NOTE] Er zijn momenteel geen normen of specificaties die definieert hoe het beginsel van het HATEOAS model. De voorbeelden in deze sectie illustreren één mogelijke oplossing.

Als u bijvoorbeeld voor het verwerken van de relatie tussen klanten en orders, bevatten de gegevens voor een specifieke antwoord URI's in de vorm van een hyperlink die identificatie van de klant die de order heeft geplaatst en de bewerkingen die kunnen worden uitgevoerd voor die klant.

```HTTP
GET http://adventure-works.com/orders/3 HTTP/1.1
Accept: application/json
...
```

De hoofdtekst van het antwoordbericht bevat een `links` (gemarkeerd in het voorbeeld) array die de aard van de relatie (_klant_), de URI van de klant (_http://adventure-works.com/customers/3_) geeft aan hoe u de details van deze klant (_GET_) en de MIME-typen die door de webserver worden ondersteund voor het ophalen van deze gegevens (_tekst/xml_ en _application/json_) ophalen. Dit is de informatie die een clienttoepassing kunnen moet worden opgehaald van de gegevens van de klant. De matrix van koppelingen bevat ook koppelingen voor de bewerkingen die kunnen worden uitgevoerd, zoals (voor het wijzigen van de klant, samen met de indeling die de server de client om verwacht) plaatsen en verwijderen.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[(some links omitted){"rel":"customer","href":" http://adventure-works.com/customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":"
customer","href":" http://adventure-works.com /customers/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"customer","href":" http://adventure-works.com /customers/3","action":"DELETE","types":[]}]}
```

Voor de volledigheid behoren de matrix koppelingen ook naar zichzelf verwijzende informatie met betrekking tot de bron die is opgehaald. Deze koppelingen zijn weggelaten uit het vorige voorbeeld, maar in de volgende code worden gemarkeerd. U ziet dat in deze koppelingen, de relatie _zelf_ is gebruikt om aan te geven dat dit een verwijzing naar de bron die wordt geretourneerd door de bewerking:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[{"rel":"self","href":" http://adventure-works.com/orders/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" self","href":" http://adventure-works.com /orders/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"self","href":" http://adventure-works.com /orders/3", "action":"DELETE","types":[]},{"rel":"customer",
"href":" http://adventure-works.com /customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" customer" (customer links omitted)}]}
```

Voor deze benadering werkt moeten clienttoepassingen op te halen en deze aanvullende informatie parseren worden bereid.

## <a name="versioning-a-restful-web-api"></a>Een RESTful web API versiebeheer

Het is zeer onwaarschijnlijk dat in alle maar de eenvoudigste situaties dat een web API statisch blijven. Bedrijfsbehoeften wijzigen nieuwe collecties van resources kunnen worden toegevoegd, de relaties tussen bronnen kunnen worden gewijzigd en de structuur van de gegevens in resources kan worden gewijzigd. Tijdens het bijwerken van een web-API voor het verwerken van nieuwe of verschillende behoeften is relatief eenvoudig, moet u rekening houden met de gevolgen die deze wijzigingen op het web API die toepassingen. Het probleem is dat de ontwikkelaar van de ontwerpen en implementeren van een web API heeft volledige controle over deze API, de ontwikkelaar hoeft niet dezelfde mate van controle over de toepassingen die door organisaties van derden die op afstand kan worden opgebouwd. De primaire imperatieve is het inschakelen van bestaande toepassingen blijft ongewijzigd terwijl u nieuwe client toepassingen kunnen profiteren van nieuwe functies en bronnen werken.

Met versiebeheer kunt een web API om aan te geven de functies en bronnen die worden getoond en een clienttoepassing aanvragen die zijn gericht aan een specifieke versie van een functie of een resource kan worden verzonden. De volgende secties worden verschillende manieren, die elk eigen voordelen en e-mailverkeer zijn.

### <a name="no-versioning"></a>Geen versiebeheer

Dit is de eenvoudigste oplossing en is aanvaardbaar voor bepaalde interne API's. Grote veranderingen kunnen worden weergegeven als nieuwe resources of nieuwe koppelingen.  Inhoud toevoegen aan bestaande bronnen, mogelijk een recente wijziging niet aanwezig als clienttoepassingen die niet worden verwacht te zien dat deze inhoud wordt gewoon genegeerd.

Een verzoek aan de URI- _http://adventure-works.com/customers/3_ weer, bijvoorbeeld de details van één klant met `id`, `name`, en `address` velden door de clienttoepassing wordt verwacht:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

> [AZURE.NOTE] Voor de toepassing van de eenvoud en duidelijkheid bevatten de antwoorden van de voorbeeld weergegeven in deze sectie geen HATEOAS koppelingen.

Als de `DateCreated` veld toegevoegd aan het schema van de bron van de klant en vervolgens het antwoord eruit als volgt:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":"1 Microsoft Way Redmond WA 98053"}
```

Bestaande toepassingen kunnen blijven goed als ze kunnen negeren niet-herkende velden, terwijl de nieuwe client-toepassingen kunnen worden ontworpen voor het verwerken van dit nieuwe veld. Echter als meer radicale wijzigingen in het schema van de middelen plaatsvinden (zoals het verwijderen of hernoemen van velden) of het wijzigen van de relaties tussen bronnen inhouden vervolgens deze recente wijzigingen om te voorkomen dat bestaande toepassingen goed functioneert. In deze gevallen kunt u overwegen een van de volgende methoden.

### <a name="uri-versioning"></a>URI-versies

Telkens wanneer u het web API of wijzigen van het schema van bronnen, voegt u een versienummer aan de URI voor elke resource. De bestaande URI's moet blijven functioneren als voorheen resources die voldoen aan hun oorspronkelijke schema te retourneren.

Uitbreiding van het vorige voorbeeld, als de `address` veld in de onderliggende velden met elk bestanddeel van het adres wordt geherstructureerd (zoals `streetAddress`, `city`, `state`, en `zipCode`), kan deze versie van de resource die toegankelijk zijn via een URI met een versienummer zoals http://adventure-works.com/v2/customers/3:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Dit mechanisme versiebeheer is erg eenvoudig, maar is afhankelijk van de server de aanvraag naar het eindpunt van de juiste routering. Echter, deze kan onhandig worden als het web dat API ouder via verschillende iteraties en de server heeft een aantal verschillende versies ondersteunen. Ook vanuit het oogpunt van een purist, in alle gevallen de clienttoepassingen zijn ophalen van dezelfde gegevens (klant 3), dus de URI mag niet echt verschillen, afhankelijk van de versie. Implementatie van HATEOAS ingewikkelder dit schema ook als alle koppelingen moet het versienummer in de URI's opnemen.

### <a name="query-string-versioning"></a>Query string versiebeheer

In plaats van meerdere URI's bieden, kunt u de versie van de resource met behulp van een parameter in de querytekenreeks toegevoegd aan de HTTP-aanvraag, zoals _http://adventure-works.com/customers/3?version=2_. De parameter versie standaard moet worden ingesteld op een betekenisvolle waarde bijvoorbeeld 1 als deze wordt weggelaten door oudere toepassingen.

Deze aanpak heeft het semantische voordeel dat dezelfde resource altijd uit dezelfde URI wordt opgehaald, maar deze afhankelijk is van de code die het verzoek aan de query-tekenreeks te parseren en de juiste HTTP-antwoord teruggezonden worden verwerkt. Deze aanpak heeft ook de dezelfde complicaties voor de uitvoering van de HATEOAS als de URI versioning mechanisme.

> [AZURE.NOTE] Sommige oudere webbrowsers en webproxy cache niet antwoorden op aanvragen die in de URL een queryreeks. Dit kan nadelige gevolgen hebben voor de prestaties voor webtoepassingen die een web-API gebruiken en die uitvoeren vanuit een webbrowser.

### <a name="header-versioning"></a>Koptekst versiebeheer

In plaats van het versienummer als queryreeksparameter toe te voegen, kunt u een aangepaste header die de versie van de resource aangeeft implementeren. Deze benadering vergt de clienttoepassing wordt de juiste header toegevoegd aan alle verzoeken, hoewel de code voor het verwerken van de aanvraag van de client een standaardwaarde (versie 1) gebruiken kan als de koptekst wordt weggelaten. De volgende voorbeelden maken gebruik van een aangepaste header met de naam _Custom-Header_. De waarde van deze header geeft de versie van web API.

Versie 1:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=1
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Versie 2:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=2
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Houd er rekening mee dat net als bij de vorige twee methoden ter uitvoering van de HATEOAS vereist de juiste aangepaste header met inbegrip van eventuele koppelingen.

### <a name="media-type-versioning"></a>Mediatype versiebeheer

Wanneer een clienttoepassing een HTTP GET-verzoek naar een webserver verzendt moet deze bepalen de indeling van de inhoud die verwerken kan met behulp van een Accept-header, zoals eerder in deze handleiding worden beschreven. Het doel van de _Accept_ -header is vaak dat de clienttoepassing opgeven of de tekst van het antwoord moet XML, JSON, of een andere algemene indeling die de client kan parseren. Het is echter mogelijk voor het definiëren van aangepaste mediatypen met informatie voor de client mogelijk maakt om aan te geven welke versie van een resource wordt verwacht. In het volgende voorbeeld ziet u een aanvraag die een _Accept_ -header met de waarde _application/vnd.adventure-works.v1+json geeft_. Het element _vnd.adventure works.v1_ geeft de webserver dat versie 1 van de resource retourneren moet als de _json_ -element aangeeft dat de indeling van het hoofdgedeelte van de response JSON moet worden:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Accept: application/vnd.adventure-works.v1+json
...
```

De afhandeling van het verzoek is verantwoordelijk voor het verwerken van de _Accept_ -header en het behouden blijft zo veel mogelijk (de clienttoepassing kan meerdere indelingen in de _Accept_ -header, in welke geval de webserver de meest geschikte indeling voor het hoofdgedeelte van de response kunt opgeven). De webserver wordt de indeling van de gegevens in het hoofdgedeelte van de response bevestigd met behulp van de header Content-Type:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/vnd.adventure-works.v1+json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Als de Accept-header niet alle bekende mediatypen geeft, kan de webserver genereert een antwoordbericht HTTP 406 (niet geaccepteerd) of een bericht met een standaard-mediatype.

Deze aanpak is, hoewel de purest van de mechanismen voor versiebeheer en natuurlijk geschikt voor HATEOAS die het MIME-type van gerelateerde gegevens in de bron links opnemen kunt.

> [AZURE.NOTE] Wanneer u een strategie voor versiebeheer selecteert, moet u ook overwegen de gevolgen voor de prestaties, vooral in cache opslaan op de webserver. De URI versiebeheer en Query-tekenreeks versioning regelingen zijn cache-vriendelijke zover dezelfde combinatie van URI-query string naar dezelfde gegevens elke keer verwijst.

> De koptekst versiebeheer en mediumtype versioning mechanismen moeten meestal extra logica voor het onderzoeken van de waarden in de aangepaste kop- of de Accept-header. In een grootschalige omgeving, veel clients met verschillende versies van een web-API kunnen leiden tot een aanzienlijke hoeveelheid dubbele gegevens in een server-side cache. Dit probleem kan resulteren in acute als een clienttoepassing met een webserver via een proxy die wordt geïmplementeerd in de cache opslaan en die alleen een verzoek doorstuurt naar de webserver communiceert als deze niet op dit moment een kopie van de gevraagde gegevens in de cache bevat.

## <a name="more-information"></a>Meer informatie

- Het [RESTful kookboek](http://restcookbook.com/) bevat een inleiding tot het bouwen van RESTful API's.
- De Web- [API controlelijst](https://mathieu.fenniak.net/the-api-checklist/) bevat een handig overzicht van rekening mee moet houden bij het ontwerpen en implementeren van een Web-API.
