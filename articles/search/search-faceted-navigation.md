<properties 
    pageTitle="Beperkt navigatie implementeren in Azure zoeken | Microsoft Azure | navigatie rubrieken zoeken" 
    description="Beperkt navigatie toevoegen aan toepassingen die kunnen worden geïntegreerd met Azure Search een zoekservice cloud gehost op Microsoft Azure." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Beperkt navigatie implementeren in Azure zoeken

Beperkt navigatie is een filtermechanisme waarmee gericht drilldown navigatie in toepassingen. De term 'beperkt navigatie' wellicht niet bekend, maar het is bijna een gezien het feit dat u hebt gebruikt vóór. Zoals in het volgende voorbeeld wordt getoond, wordt beperkt navigatie is niets meer dan de categorieën gebruikt om resultaten te filteren.

## <a name="what-it-looks-like"></a>Hoe ziet

 ![][1]
  
Facetten kunt u vinden wat u zoekt, terwijl ervoor te zorgen dat u won't nul resultaten krijgt. Als ontwikkelaar kunnen facetten u de nuttigste zoekcriteria voor het navigeren in het corpus van uw zoekopdracht worden blootgesteld. Navigatie beperkt is in online retail-toepassingen vaak opgebouwd via merken, afdelingen (Kids schoenen), grootte, prijs, populariteit en beoordelingen. 

Beperkt navigatie niet in alle technologieën zoeken en zeer complex kunnen zijn. In Azure Search, beperkt navigatie ingebouwd bij query toegeschreven velden eerder in het schema is opgegeven. In de query's die de toepassing maakt, moet een query *facet queryparameters* verzenden ontvangen de filterwaarden facet beschikbaar voor de resultaatset van dat document. Voor het bijsnijden van het resultaat van het document daadwerkelijk ingesteld, moet de toepassing van toepassing een `$filter` expressie.

Voor de ontwikkeling van toepassingen vormt schrijven van code die wordt gemaakt van query's het grootste deel van het werk. Veel van het gedrag van toepassingen die u in de navigatie beperkt wilt wordt geleverd door de service, inclusief ingebouwde ondersteuning voor het instellen van bereiken en telt voor facet resultaten ophalen. De service omvat tevens sensible standaardinstellingen waarmee u onpraktische navigatiestructuur voorkomen. 

Dit artikel bevat de volgende secties:

- [Het bouwen van deze](#howtobuildit)
- [De presentatie laag maken](#presentationlayer)
- [De index bouwen](#buildindex)
- [Controleren op de kwaliteit van de gegevens](#checkdata)
- [De query bouwen](#buildquery)
- [Tips over het beheren van beperkt navigatie](#tips)
- [Beperkt navigatie op basis van de waarden](#rangefacets)
- [Beperkt navigatie op basis van GeoPoints](#geofacets)
- [Probeer het zelf](#tryitout)

##<a name="why-use-it"></a>Waarom gebruikt
De meest effectieve toepassingen hebben meerdere modellen van interactie naast een zoekvak. Beperkt navigatie is een alternatieve ingangspunt zoeken, biedt een alternatief voor het zoeken van complexe expressies met de hand te typen.

##<a name="know-the-fundamentals"></a>Kent u de basisprincipes

Als u nieuwe ontwikkeling zoeken, is de beste manier om na te denken van beperkt navigatie wordt van de mogelijkheden om gericht te zoeken. Het is een soort drill-down zoekervaring te bieden, op basis van vooraf gedefinieerde filters, die wordt gebruikt voor het snel omlaag zoekresultaten via point-and-click acties toespitsen. 

**Interactie-Model**

De zoekfunctie voor navigatie beperkt is iteratief, dus laten we beginnen door inzicht in een reeks van query's die zich in reactie op acties van gebruikers ontvouwt.

Het uitgangspunt is een pagina die beperkt navigatie worden, meestal geplaatst op de omtrek. Beperkt navigatie is het vaak een boomstructuur met de selectievakjes uit voor elke waarde of tekst waarop kan worden geklikt. 

1.  Een query verzonden naar Azure Search Hiermee geeft u de navigatiestructuur voor beperkt via een of meer facet query-parameters. Bijvoorbeeld de query kan ook `facet=Rating`, wellicht met een `:values` of `:sort` optie om de presentatie verder te verfijnen.
2.  De presentatie laag wordt een zoekpagina waarmee beperkt navigatie, met behulp van de facetten in de aanvraag vermeld.
3.  Een beperkt navigatiestructuur met Rating wordt gegeven, de gebruiker klikt op "4" om aan te geven dat alleen producten met een score van 4 of hoger moeten worden weergegeven. 
4.  In reactie verzendt de toepassing een query met`$filter=Rating ge 4` 
5.  De presentatie laag werkt de pagina, met een verminderde resultaatset met alleen die items die voldoen aan de nieuwe criteria (in dit geval producten beoordeeld 4 en hoger).

Een facet is een queryparameter, maar niet verwarren met de invoer voor de query. Het wordt nooit gebruikt als selectiecriteria in een query. In plaats daarvan zien queryparameters facet als invoer voor de navigatiestructuur die in het antwoord terugkomt. Voor elk facet queryparameter die u bieden, evalueren Azure Search hoeveel documenten zijn in de gedeeltelijke resultaten voor elke facetwaarde.

Aankondiging van de `$filter` in stap 4. Dit is een belangrijk aspect van navigatie beperkt. Hoewel facetten en filters onafhankelijk van elkaar in de API zijn, moet u zowel de ervaring die u van plan bent. 

**Ontwerppatronen**

Het patroon is facet query-parameters gebruiken om terug te keren de beperkt navigatiestructuur met facet resultaten, plus een $filter-expressie die de gebeurtenis afhandelt in de code van toepassing. Van de `$filter` expressie als de code achter de werkelijke bijsnijden van de zoekresultaten worden geretourneerd aan de presentatie laag. Uitgaande van een facet kleuren, klikt u op de kleur rood wordt geïmplementeerd via een `$filter` expressie die alleen de items die een kleur rood geselecteerd. 

**Basisbeginselen van query in Azure zoeken**

In Azure-zoeken is via een of meer query-parameters (Zie [Documenten zoeken](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor een beschrijving van elke) een aanvraag opgegeven. Geen van de queryparameters vereist zijn moet, maar u ten minste één om een query om geldig te zijn.

Precision, algemeen wordt opgevat als de mogelijkheid om het uitfilteren van niet-relevante treffers wordt bereikt door middel van een of beide van deze expressies:

- **Search =**<br/>
De waarde van deze parameter vormt de zoekformule. Mogelijk is een stukje tekst of een complexe zoekopdracht met meerdere voorwaarden en operatoren. Op de server, wordt een zoekformule gebruikt voor volledige-tekstzoekacties doorzoekbare velden in de index naar overeenkomende termen, resultaten retourneren in volgorde van rang opvragen. Als u `search` op null, query kan worden uitgevoerd via de volledige index is (dat wil zeggen, `search=*`). In dit geval, de andere elementen van de query, zoals een `$filter` of profiel scoren, wordt de primaire factoren beïnvloeden welke documenten worden geretourneerd `($filter`) en in welke volgorde (`scoringProfile` of `$orderb`y).

- **$filter =**<br/>
Een filter is een krachtig mechanisme voor het beperken van de grootte van de zoekresultaten op basis van de waarden van documentkenmerken voor specifieke. A `$filter` wordt als eerste geëvalueerd, gevolgd door faceting logica die de beschikbare waarden en bijbehorende aantallen voor elke waarde genereert

Complexe zoekformules zullen verminderen de prestaties van de query. Waar mogelijk, gebruik van goed gebouwd filterexpressies precisie verhogen en verbeteren de prestaties van query's.

Vergelijk beter wilt begrijpen hoe een filter wordt toegevoegd met meer precisie, een complexe zoekopdracht-expressie die een expressie bevat:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Hoewel beide query's geldig zijn, is de tweede superieure u bent op zoek naar niet-motels met parkeerplaatsen in Seattle. De eerste query is gebaseerd op deze specifieke woorden worden genoemd of niet wordt genoemd in de reeks velden, zoals naam, beschrijving en andere velden met gegevens die kan worden doorzocht. De tweede query gezocht naar exacte overeenkomsten van gestructureerde gegevens en waarschijnlijk veel nauwkeuriger.

In toepassingen die beperkt navigatie zijn, wilt u om ervoor te zorgen dat elke actie van de gebruiker via een navigatiestructuur beperkt vergezeld gaat van een vernauwing van de zoekresultaten bereikt door middel van een expressie.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Het bouwen van deze

Beperkt navigatie in Azure Search is geïmplementeerd in uw toepassingscode die de aanvraag is gebaseerd, maar is afhankelijk van de vooraf gedefinieerde elementen in het schema.

Vooraf gedefinieerd in de zoekopdracht de index is de `Facetable [true|false]` indexkenmerk instellen voor geselecteerde velden of het gebruik ervan in de navigatiestructuur van een beperkt uit te schakelen. Zonder `"Facetable" = true`, een veld kan niet worden gebruikt in facet navigatie.

Bij query uw toepassingscode maakt u een aanvraag met `facet=[string]`, een aanvraag-parameter waarmee het veld facet door. Een query kan hebben meerdere facetten, zoals `&facet=color&facet=category&facet=rating`, een gescheiden door een ampersand (&) tekens.

Toepassingscode moet ook opgeven, een `$filter` expressie in de navigatie beperkt de klikgebeurtenissen afhandelen. A `$filter` vermindert de zoekresultaten met de facetwaarde als filtercriteria.

Azure zoekactie resulteert in de zoekresultaten per de items opgegeven door de gebruiker, en de updates van de navigatiestructuur beperkt. In Azure-zoekopdracht beperkt navigatie is één niveau gebouwd met facet waarden, en telt hoeveel resultaten voor elke gevonden.

De presentatie laag in uw code geeft de gebruikerservaring. De samenstellende delen van de beperkt navigatie, zoals het label, waarden, selectievakjes en het aantal moet worden vermeld. De Azure Search REST API platform agnostic, dus ongeacht welke taal en platform dat u wilt gebruiken. Het belangrijkste is dat UI elementen opnemen die ondersteuning voor incrementele vernieuwen met bijgewerkte gebruikersinterface staat elke extra facet is geselecteerd. 

In de volgende secties zult we Kijk eens hoe elk deel, beginnen met de presentatie laag.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>De presentatie laag maken

Werken van de presentatie laag kunt u eisen dat anders zou worden overgeslagen en begrijpen welke mogelijkheden zijn van essentieel belang om de zoekervaring te schuiven.

Invoer van de gebruiker op de pagina detecteert en de gewijzigde elementen invoegt in beperkt navigatie, uw web- of pagina bevat de navigatiestructuur beperkt. 

Voor webtoepassingen, wordt AJAX vaak gebruikt in de presentatie laag omdat u incrementele wijzigingen te vernieuwen. U kunt ook ASP.NET MVC of elk ander platform visualisatie die via HTTP verbinding met een Azure Search-service maken kan. De voorbeeldtoepassing waarnaar wordt verwezen in dit artikel-- **AdventureWorks catalogus** – gebeurt er een aanvraag voor ASP.NET MVC.

Het volgende voorbeeld wordt opgehaald uit het bestand **index.cshtml** van de voorbeeldtoepassing bouwt een dynamische HTML-structuur voor het weergeven van beperkt navigatie op de pagina met zoekresultaten. In het voorbeeld wordt beperkt navigatie is ingebouwd in de pagina met zoekresultaten en wordt weergegeven nadat de gebruiker een zoekopdracht heeft ingediend.

Mededeling dat elk facet een label (kleuren, categorieën, prijzen), een binding met een beperkt veld (kleur, categorienaam, listPrice) en een heeft `.count` parameter als resultaat het aantal items gevonden voor dat resultaat facet gebruikt.

  ![][2]
 

> [AZURE.TIP] Bij het ontwerpen van de pagina met zoekresultaten, moet u een mechanisme voor het leegmaken van facetten toevoegen. Als u selectievakjes, kunnen gebruikers gemakkelijk de filters wissen intuit. Voor andere indelingen moet u mogelijk een ' breadcrumb '-patroon of een andere creatieve benadering. Bijvoorbeeld in de voorbeeldtoepassing AdventureWorks catalogus kunt u de titel, de AdventureWorks-catalogus, de zoekpagina opnieuw instellen.

<a name="buildindex"></a>
##<a name="build-the-index"></a>De index bouwen

Faceting is ingeschakeld op basis van veld veld in de index via deze indexkenmerk: `"Facetable": true`.  
Alle veldtypen die eventueel kan worden gebruikt in de navigatie beperkt `Facetable` standaard. Deze veldtypen zijn `Edm.String`, `Edm.DateTimeOffset`, en alle numerieke veldtypen (in feite alle veldtypen zijn facetable, met uitzondering van `Edm.GeographyPoint`, die in de navigatie beperkt kan niet worden gebruikt). 

Als u een index maakt, is een best practice voor navigatie beperkt expliciet faceting om uit te schakelen voor de velden die u moeten nooit worden gebruikt als een facet.  Tekenreeksvelden voor singleton-waarden, zoals de naam van een product-ID of de in het bijzonder moeten worden ingesteld op `"Facetable": false` om te voorkomen dat het gebruik ervan per ongeluk (en inefficiënte) in een beperkt navigatie.

Dit is het schema voor de AdventureWorks-catalogus monster app (ontdaan van bepaalde kenmerken te verminderen grootte):

 ![][3]
 
Houd er rekening mee dat `Facetable` is uitgeschakeld voor de reeks velden die niet moeten worden gebruikt als facetten, zoals een ID of naam. Faceting uitschakelen wanneer u deze niet nodig te schakelen zorgt u ervoor dat de grootte van de index kleine en verbetert de prestaties in het algemeen.

> [AZURE.TIP] Beste, omvatten de volledige set van indexkenmerken voor elk veld. Hoewel `Facetable` is standaard ingeschakeld voor bijna alle velden, opzettelijk stellen elk kenmerk kunt u de gevolgen van elke beslissing van het schema analyseren. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Controleren op de kwaliteit van de gegevens 

Gegevensgerichte toepassingen ontwikkelen, is voorbereiden van de gegevens vaak een groter deel van de taak. Toepassingen zijn geen uitzondering. De kwaliteit van uw gegevens heeft een directe invloed op de vraag of de navigatiestructuur beperkt gebeurtenis zich voordoet als u verwacht, en de doeltreffendheid ervan bij het samenstellen van filters die het resultaat minder instellen.

In Azure zoeken, wordt het corpus van de zoekopdracht gevormd door documenten waarmee een index wordt gevuld. Documenten bevatten de waarden die worden gebruikt voor het berekenen van facetten. Als u onderdeel door een merk of prijs wilt, moet elk document waarden bevatten voor *BrandName* en *ProductPrice* zijn geldig, consistent en productief als een filter.

Hieronder staan enkele herinneringen van wat te corrigeren voor:

- Voor elk veld dat u wilt facet door, moet u zich afvragen of deze waarden bevat die geschikt zijn als filters in gericht zoeken zijn. De waarden moeten worden korte, beschrijvende en voldoende onderscheidend zijn om een duidelijke keuze tussen concurrerende opties bieden.
- Spelfouten en bijna overeenkomende waarden. Als facet van kleur en waarden in het veld zijn oranje en Ornage (spelfout), een facet op basis van het veld kleur zou oppakken beide.
- Gemengde tekst van de aanvraag kan ook verwoesting in navigatie in beperkt, met oranje en oranje worden weergegeven als twee verschillende waarden aanrichten. 
- Enkel- en meervoud versies van dezelfde waarde kunnen resulteren in een afzonderlijk onderdeel voor elk.

Zoals u zich voorstellen kunt, is de voortvarendheid bij het voorbereiden van de gegevens een wezenlijk aspect van effectieve beperkt navigatie.

<a name="buildquery"></a>
##<a name="build-the-query"></a>De query bouwen

De code die u schrijft voor het maken van query's moet alle onderdelen van een geldige query, inclusief zoekformules, facetten, filters, scoren profielen: iets gebruikt voor het formuleren van een aanvraag opgeven. In deze sectie wordt besproken waar facetten past in een query en hoe filters met facetten worden gebruikt voor het leveren van een verminderde resultaatset.

Een voorbeeld is het vaak een goede plaats om te beginnen. Het volgende voorbeeld wordt opgehaald uit het bestand **CatalogSearch.cs** maakt een aanvraag die wordt gemaakt op basis van kleur, categorie en prijs facet-navigatie. 

U ziet dat facetten integraal in deze voorbeeldtoepassing. De zoekfunctie in de catalogus AdventureWorks is ontworpen rond beperkt navigatie en filters. Dit wordt duidelijk in de prominente positie van beperkt navigatie op de pagina. De voorbeeldtoepassing bevat de URI-parameters voor facetten (kleur, categorie, prijzen) als eigenschappen van de methode Search (zoals deze is gebouwd in de voorbeeldtoepassing).

  ![][4]
 
Een queryparameter facet is ingesteld op een veld en afhankelijk van het gegevenstype kan worden verder met parameters door komma's gescheiden lijst met `count:<integer>`, `sort:<>`, `intervals:<integer>`, en `values:<list>`. Een waardenlijst wordt ondersteund voor numerieke gegevens bij het instellen van bereiken. Zie [Documenten zoeken (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor details van gebruik.

De aanvraag geformuleerd door de toepassing met facetten, moet ook filters om de set kandidaat documenten op basis van een selectie facet waarde vast te stellen maken. Voor een fiets winkel, biedt beperkt navigatie aanwijzingen te vragen, zoals 'welke kleuren, fabrikanten en soorten fietsen beschikbaar zijn", terwijl filteren antwoorden vragen als"welke exacte bikes zijn rood, mountainbikes, in deze prijs bereik".

Wanneer een gebruiker op 'Rood' om aan te geven dat alleen rode producten moeten worden weergegeven, de volgende query de toepassing verzendt zou zijn `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Aanbevolen procedures voor beperkt navigatie

De volgende lijst bevat een overzicht van enkele aanbevolen procedures.

- **Precisie**<br/>
Filters gebruiken. Als u met alleen zoekformules alleen afleiding kan ervoor zorgen dat een document werkt moet worden geretourneerd die de facetwaarde nauwkeurig geen in van de velden. 

- **Doelvelden**<br/>
In een beperkt inzoomen, meestal wilt u alleen documenten met facetwaarde in een bepaald veld (gefacetteerd) niet overal over alle doorzoekbare velden opnemen. Een filter toevoegen versterkt het doelveld door de service om te zoeken in het veld beperkt voor een overeenkomende waarde.

- **Efficiëntie van de index**<br/>
Als uw toepassing beperkt navigatie uitsluitend gebruikt (dat wil zeggen, geen zoekvak) kunt u het veld als markeren `searchable=false`, `facetable=true` voor de productie van een compactere index. Bovendien indexing doet zich alleen op hele facet waarden, geen woordafbreking of het indexeren van de onderdelen van een waarde van meerdere woorden.

- **Prestaties**<br/>
Filters beperken de set documenten kandidaat voor zoeken en rangorde uitsluiten. Als u een grote reeks documenten, krijgt met behulp van een detailanalyse zeer voorzichtig facet omlaag vaak u aanzienlijk betere prestaties.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Tips over het beheren van beperkt navigatie

Hieronder vindt u een blad met richtsnoeren voor specifieke problemen.

**Labels voor elk veld in facet navigatie toevoegen**

Labels worden meestal gedefinieerd in de HTML-code of het formulier (**index.cshtml** in de voorbeeldtoepassing). Er is geen API in Azure facet navigatielabels zoeken of een ander type van metagegevens.

**Bepalen welke velden worden gebruikt als facet**

Let erop dat het schema van de index wordt bepaald welke velden beschikbaar zijn voor gebruik als een facet. Als dat een veld is facetable, de query geeft aan welke velden u wilt facet door. Het veld waarop u faceting bent bevat de waarden die worden weergegeven onder het label. 

De waarden die worden weergegeven onder elk etiket worden opgehaald uit de index. Bijvoorbeeld als het veld facet *kleur*, zijn de beschikbare waarden voor aanvullende filters de waarden voor dat veld (rood, zwart, enzovoort).

Voor numerieke en datum/tijd-waarden alleen, u kunt expliciet instellen in het veld facet (bijvoorbeeld: `facet=Rating,values:1|2|3|4|5`). Een waardenlijst is toegestaan voor deze typen voor het vereenvoudigen van de scheiding van de facet in aaneengesloten bereiken (beide bereiken op basis van numerieke waarden of perioden). 

**Trim facet resultaten**

Facet resultaten worden gevonden in de zoekresultaten weergegeven die overeenkomen met een facet term documenten. In het volgende voorbeeld wordt hebben in de zoekresultaten voor *cloud computing*, 254 artikelen ook *interne specificatie* als een inhoudstype. Objecten elkaar niet noodzakelijkerwijs uitsluiten. Als een item voldoet aan de criteria voor beide filters, is het in elk geteld. Dit is mogelijk wanneer faceting op `Collection(Edm.String)` velden die vaak worden gebruikt voor het implementeren van document codering.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

In het algemeen, als u vindt dat facet resultaten blijvend te groot zijn, raden we toevoegen u meer filters, zoals wordt beschreven in eerdere secties, om uw toepassingsgebruikers meer opties voor het verfijnen van de zoekactie.

**Beperk aantal items in de navigatie facet**

Er is een standaardlimiet van 10 waarden voor elk veld beperkt in de navigatiestructuur. Deze standaard is zinvol voor de navigatiestructuur omdat het zorgt ervoor dat de waardelijst met tot een beheersbaar formaat. U kunt de standaardinstelling negeren door het toewijzen van een waarde voor het tellen van.

- `&facet=city,count:5`Hiermee geeft u aan dat alleen de eerste 5 steden gevonden in de bovenste resultaten gerangschikt worden geretourneerd als resultaat facet. Gegeven de query bevat een zoekterm "luchthaven" en komt overeen met 32, `&facet=city,count:5`, alleen de eerste vijf unieke steden met de meeste documenten in de zoekresultaten worden opgenomen in de resultaten facet.

Aankondiging van het onderscheid tussen facet resultaten en zoekresultaten. Zoekresultaten zijn alle documenten die overeenkomen met de query. Facet resultaten zijn de resultaten voor elke facetwaarde. In het voorbeeld vindt zoekresultaten plaatsnamen die niet in de lijst met facet classificatie (5 in ons voorbeeld). De resultaten worden gefilterd via beperkt navigatie zichtbaar voor de gebruiker wanneer hij of zij facetten wist of andere facetten naast plaats kiest. 

> [AZURE.NOTE] Bespreking van `count` wanneer er meer dan één type kan verwarrend zijn. De volgende tabel biedt een korte samenvatting van hoe de term wordt gebruikt in Azure Zoek-API, voorbeeldcode en documentatie. 

- `@colorFacet.count`<br/>
In de presentatie programmacode ziet u een parameter aantal op de facet, gebruikt om het aantal facet resultaten weer te geven. In facet resultaten geeft count het aantal documenten die overeenkomen met de term facet of het bereik.

- `&facet=City,count:12`<br/>
U kunt count in een query facet instellen op een waarde.  De standaardwaarde is 10, maar u kunt instellen dat deze hoger of lager. Het instellen van `count:12` haalt de top 12 overeenkomt met facet resultaten door het aantal documenten.

- "`@odata.count`"<br/>
In antwoord op de query, geeft deze waarde aan het aantal overeenkomende items in de lijst met zoekresultaten. Gemiddeld het groter is dan de som van alle facet resultaten gecombineerd, vanwege de aanwezigheid van objecten die overeenkomen met de zoekterm, maar hebben geen waarde facet overeenkomsten.


**Niveaus in beperkt navigatie** 

Zoals opgemerkt, is er geen directe ondersteuning voor nesten facetten in een hiërarchie. Beperkt navigatie ondersteunt slechts één niveau van filters uit het vak. Echter bestaan oplossingen. U kunt coderen een facet hiërarchische structuur in een `Collection(Edm.String)` punt met één vermelding per hiërarchie. Deze oplossing implementeren valt buiten het bestek van dit artikel, maar u kunt lezen over collecties in [OData door voorbeeld](http://msdn.microsoft.com/library/ff478141.aspx). 

**Velden valideren**

Als u de lijst met facetten dynamisch op basis van de invoer van niet-vertrouwde gebruiker bouwt, moet u ofwel valideren de namen van de velden beperkt geldig zijn, of druk op ESC om de namen bij het maken van URL's met behulp van `Uri.EscapeDataString()` in .NET, of het equivalent in uw platform van keuze.

**Telt in facet resultaten**

Wanneer een filter toevoegen aan een query beperkt, wilt u mogelijk de instructie facet behouden (bijvoorbeeld: `facet=Rating&$filter=Rating ge 4`). Technisch, facet = score is niet nodig, maar deze te houden geeft als resultaat de tellingen van waarden voor beoordelingen facet 4 en hoger. Bijvoorbeeld als een gebruiker klikt op "4" en de query een filter voor een groter of gelijk aan '4 bevat', worden tellingen geretourneerd voor elke beoordeling is 4 en hoger.  

**Sharding implicaties op facet tellingen**

Onder bepaalde omstandigheden kan het gebeuren facet aantallen niet overeenkomen met de resultaatsets (Zie [beperkt navigatie in Azure Search (forum post)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Facet telt kunnen zijn vanwege de architectuur sharding onnauwkeurig. Elke zoekindex heeft meerdere shards en elkaar de top N facetten rapporten door het aantal documenten, die vervolgens worden gecombineerd tot een enkel resultaat. Als sommige shards een groot hebben aantal overeenkomende waarden, terwijl andere een kleiner, kan het gebeuren dat sommige facet waarden ontbreken of onder-geteld in de resultaten.

Maar dit probleem kan te allen tijde worden gewijzigd als u dit probleem nu optreedt, u kunt het omzeilen door het aantal kunstmatig verversen:<number> voor een erg groot getal af te dwingen met volledige rapportage vanuit elke shard. Als de waarde van count: groter is dan of gelijk is aan het aantal unieke waarden in het veld, wordt gegarandeerd nauwkeurige resultaten. Wanneer echter document telt zijn echt hoog is, is er een op de prestaties, dus gebruikt deze optie overgangseffecten.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Facet navigatie op basis van de bereikwaarden in een

Faceting via bereiken is een algemene vereiste voor search toepassing. Bereiken worden voor numerieke gegevens en datum-/ tijdwaarden ondersteund. U kunt meer lezen over elke benadering in [Documenten zoeken (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Constructie bereik vereenvoudigt Azure Search door middel van twee methoden voor het berekenen van een bereik. Voor beide benaderingen maakt Azure Search u de juiste bereiken gezien de invoer die u hebt opgegeven. Bijvoorbeeld, als u de waarden van 10 | 20 | 30, zal het automatisch bereiken van 0-10, 10-20, 20-30 maken. De voorbeeldtoepassing verwijdert de tijdstippen die leeg zijn. 

**Methode 1: Gebruik de parameter interval**<br/>
Prijs facetten in stappen van $10 stelt opgeven:`&facet=price,interval:10`


**Methode 2: Gebruik een waardenlijst met**<br/>
Voor numerieke gegevens kunt u een waardenlijst.  Houd rekening met het facet bereik voor listPrice, als volgt weergegeven:

  ![][5]

Het bereik is opgegeven in het **CatalogSearch.cs** -bestand met een waardenlijst:

    facet=listPrice,values:10|25|100|500|1000|2500

Elk bereik is gebouwd met 0 als uitgangspunt, een waarde uit de lijst als eindpunt, en wordt vervolgens gesneden van het vorige bereik voor het maken van aparte intervallen. Azure Search gebeurt dit als onderdeel van de navigatie beperkt. U hoeft geen code schrijven voor het structureren van elk interval.

### <a name="build-a-filter-for-facet-ranges"></a>Een filter voor facet bereiken maken ###

Als u wilt filteren op basis van een door de gebruiker geselecteerde bereik, kunt u de `"ge"` en `"lt"` operators in een tweedelige expressie waarin de eindpunten van het bereik filteren. Bijvoorbeeld als de gebruiker het bereik 10-25, het filter is `$filter=listPrice ge 10 and listPrice lt 25`.

In de voorbeeldtoepassing gebruikt de filterexpressie **priceFrom** en **priceTo** parameters in te stellen van de eindpunten. De methode **BuildFilter** in **CatalogSearch.cs** bevat de filterexpressie waarmee u de documenten binnen een bereik.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Gefilterde navigatie op basis van GeoPoints

Gemeenschappelijke zien filters waarmee u een winkel, restaurant of op basis van de nabijheid van uw huidige locatie. Dit type filter beperkt navigatie lijkt, is maar het eigenlijk gewoon een filter. Hier vermelden we voor mensen die specifiek op zoek bent naar advies voor het ontwerpprobleem van dat specifieke uitvoering.

Er zijn twee georuimtelijke functies in Azure Search, **geo.distance** en **geo.intersects**.

- De functie **geo.distance** retourneert de afstand in kilometers tussen twee punten, één die een veld en één voor een constante doorgegeven als onderdeel van het filter. 

- De functie **geo.intersects** retourneert true als een bepaald punt binnen een bepaalde veelhoek, waarbij het punt een veld en de veelhoek als een constante lijst met coördinaten die worden doorgegeven als onderdeel van het filter is opgegeven.

Filter voorbeelden kunt u vinden in de [syntaxis van de expressie OData (Azure Search)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Probeer het zelf

Azure Search Adventure Works Demo op Codeplex bevat de voorbeelden in dit artikel wordt verwezen. Als u met zoekresultaten werkt, bekijk de URL voor wijzigingen in de constructie van de query. Deze toepassing gebeurt met facetten als u elk toevoegen aan de URI.

1.  Configureer de voorbeeldtoepassing uw URL en een api-sleutel te gebruiken. 

    U ziet het schema dat is gedefinieerd in het bestand Program.cs van het project CatalogIndexer. Het bevat velden voor kleur, listPrice, grootte, gewicht, categorienaam en modelName facetable.  Slechts enkele van deze (kleur, listPrice, categorienaam) worden daadwerkelijk geïmplementeerd in de navigatie beperkt.

3.  De toepassing wordt uitgevoerd. 

    Op het eerste wordt alleen in het vak Zoeken weergegeven. U kunt zoeken klikt u meteen alle om resultaten te krijgen of typ een zoekterm.

    ![][7]
 
4.  Geef een zoekterm zoals fiets, en klik op **Zoeken**. Snel de query wordt uitgevoerd.
 
    Een navigatiestructuur beperkt ook met de zoekresultaten geretourneerd.  In de URL, zijn facetten voor kleuren, categorieën en prijzen, zijn null. In de zoekresultaten bevat de navigatiestructuur beperkt telt voor elk resultaat facet.

     ![][8]
 
5.  Klik op een kleur, categorie en prijsklasse. Facetten op een oorspronkelijke zoekopdracht null zijn, maar als ze op de waarden, de zoekresultaten worden ontdaan van items die niet langer voldoen aan. U ziet dat de URI de wijzigingen in de query wordt opgehaald.

    ![][9]
 
6.  Schakel de query beperkt, zodat u het gedrag van andere query proberen kunt, klikt u op de **AdventureWorks-catalogus** op de bovenkant van de pagina.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Volgende stap

Als u wilt uw kennis testen, kunt u een veld facet voor *modelName*toevoegen. De index is al ingesteld voor dit facet, zodat er geen wijzigingen aan de index vereist zijn. Maar moet u de HTML-code voor modellen een nieuw facet, en voeg het veld facet aan de constructor van de query wijzigen.

Voor meer informatie over ontwerpprincipes voor navigatie beperkt wordt aangeraden de volgende koppelingen:

- [Ontwerpen voor beperkt zoeken](http://www.uie.com/articles/faceted_search/)
- [Ontwerppatronen: Beperkt navigatie](http://alistapart.com/article/design-patterns-faceted-navigation)

U kunt ook controleren of [Azure Search diep Kennismaking](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). 45:25 is er een demo over het implementeren van facetten.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 