<properties
   pageTitle="Azure Service REST API versie 2015-02-28-voorbeeld Search | Microsoft Azure | Voorbeeld van Azure Search API"
   description="Azure Search Service REST API versie 2015-02-28-voorbeeld bevat experimentele functies zoals zoekopdrachten in natuurlijke taal Analyzers en moreLikeThis."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure Search Service REST API: Versie 2015-02-28-voorbeeld

In dit artikel wordt de documentatie bij `api-version=2015-02-28-Preview`. In dit voorbeeld wordt de huidige versie van de algemeen beschikbare uitgebreid [api versie 2015-02-28 =](https://msdn.microsoft.com/library/dn798935.aspx), door middel van de volgende experimentele functies:

- `moreLikeThis`queryparameter in [Documenten zoeken](#SearchDocs) API. Hiermee zoekt u andere documenten die relevant voor een ander specifiek document zijn.

Een paar extra onderdelen van de `2015-02-28-Preview` REST API zijn afzonderlijk gedocumenteerd. Deze omvatten:

- [Score van profielen](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexeerfuncties](search-api-indexers-2015-02-28-preview.md)

Azure Search-service is beschikbaar in meerdere versies. Raadpleeg [Search Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie.

## <a name="apis-in-this-document"></a>API's in dit document

Azure Search service API ondersteunt twee URL-syntaxis voor bewerkingen API: eenvoudig en OData (Zie [ondersteuning voor OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) voor meer informatie). De volgende lijst ziet u de eenvoudige syntaxis.

[Index maken](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Index bijwerken](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Index opvragen](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Indexen weergeven](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Indexstatistieken opvragen](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Test Analyzer](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Een Index verwijderen](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Toevoegen, verwijderen en bijwerken van gegevens in een Index](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Documenten zoeken](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Document zoeken](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Aantal documenten](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Suggesties](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Indexen

U kunt maken en beheren van indexen in Azure Search-service via een eenvoudige HTTP-aanvragen (POST, GET, PUT, DELETE) tegen een bron voor de opgegeven index. Om een index te maken, moet u eerst een JSON-document met het schema index boeken. De velden van de index, de gegevenstypen en hoe ze kunnen worden gebruikt (bijvoorbeeld in de volledige-tekstzoekacties, filters, sorteren of faceting) in het schema gedefinieerd. Definieert ook score profielen, suggesters en andere kenmerken configureren van het gedrag van de index.

In het volgende voorbeeld ziet u een schema voor het zoeken van gegevens van hotel met het veld Omschrijving is gedefinieerd in twee talen. U ziet hoe kenmerken bepalen hoe het veld wordt gebruikt. Bijvoorbeeld de `hotelId` wordt gebruikt als de documentsleutel (`"key": true`) en wordt uitgesloten van volledige-tekstzoekacties (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Nadat de index is gemaakt, kunt u documenten die de index vullen gaat uploaden. Zie [toevoegen of bijwerken, documenten](#AddOrUpdateDocuments) voor deze stap.

Zie voor een video-Inleiding tot het indexeren in Azure Search, de [aflevering Channel 9 Cloud Cover van Azure zoeken](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Index maken

Een index is de primaire methode voor het ordenen en zoeken van documenten in Azure, vergelijkbaar met de manier waarop records in een database worden geordend in een tabel zoeken. Elke index is een verzameling documenten die alle aan het schema van de index (namen van velden, gegevenstypen en eigenschappen voldoen), maar indexen ook aanvullende constructies (suggesters score profielen opties, en CORS) waarmee het gedrag van andere zoeken opgeven.

U kunt een nieuwe index maken in een Azure Search-service met behulp van een HTTP POST of PUT verzoek. Het hoofdgedeelte van de aanvraag is een JSON-schema dat geeft u de index en de configuratie-informatie.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

U kunt ook opslag gebruiken en geef de naam van de index op de URI. Als de index niet bestaat, wordt deze gemaakt.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Maken van een index bepaalt de structuur van de documenten opgeslagen en gebruikt in zoekbewerkingen. Vullen van de index is een afzonderlijke bewerking. Voor deze stap kunt u een [indexering](https://msdn.microsoft.com/library/azure/mt183328.aspx) (beschikbaar voor de ondersteunde gegevensbronnen) of een bewerking [toevoegen, bijwerken of verwijderen van documenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) . De omgekeerde index wordt gegenereerd wanneer de documenten worden geboekt.

**Opmerking**: het maximum aantal indexen is toegestaan, is afhankelijk van de prijzen laag. De gratis service kunnen maximaal 3 indexen. Standaard service kunnen 50 indexen per Search-service. Zie [beperkingen en limieten](http://msdn.microsoft.com/library/azure/dn798934.aspx) voor meer informatie.

**Aanvraag**

HTTPS is vereist voor alle aanvragen. De aanvraag **Create Index** kan worden samengesteld met behulp van een POST of een PUT-methode. Wanneer u boeken, moet u de indexnaam van een in het hoofdgedeelte van de aanvraag en de schemadefinitie voor index opgeven. De naam van de index is opgeslagen, deel van de URL. Als de index bestaat, wordt deze gemaakt. Als het al bestaat, wordt deze bijgewerkt met de nieuwe definitie.

De naam van de index moet worden in kleine letters, beginnen met een letter of cijfer hebben geen slashes of punten en minder dan 128 tekens bevatten. De rest van de naam kan na het starten van de naam van de index met een letter of cijfer bevatten een letter, getal en streepjes, zolang de streepjes niet aaneensluitend zijn.

De `api-version` is vereist. Zie [Search Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor een lijst met beschikbare versies.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `Content-Type`: Vereist. Deze worden ingesteld`application/json`
- `api-key`: Vereist. De `api-key` wordt gebruikt om
- het verzoek om de Search-service te verifiëren. Het is een tekenreekswaarde die uniek zijn voor uw service. De aanvraag **Create Index** omvat een `api-key` ingesteld aan de admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

<a name="RequestData"></a>
**Syntaxis voor hoofdtekst**

Het hoofdgedeelte van de aanvraag bevat de schemadefinitie van een, waarin u de lijst met gegevensvelden in documenten die zullen worden opgenomen in deze index, gegevenstypen, kenmerken, alsmede een optionele lijst met profielen die worden gebruikt voor overeenkomende documenten tegelijk query score scoren.

Houd er rekening mee dat voor een POST-aanvraag moet u de naam van de index in het hoofdgedeelte van de aanvraag.

Er kan niet meer dan één sleutelveld in de index. Er moet een tekenreeksveld. Dit veld geeft de unieke id voor elk document dat is opgeslagen in de index.

De belangrijkste onderdelen van een index omvatten het volgende:

- `name`
- `fields`die zal worden opgenomen in de index, met inbegrip van naam, gegevenstype en eigenschappen die de toegestane acties definiëren die op dat veld.
- `suggesters`voor automatisch aanvullen of type-ahead query's gebruikt.
- `scoringProfiles`gebruikt voor aangepaste zoekactie score rangorde. Zie [de score profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie.
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` gebruikt om te definiëren hoe uw documenten/query's worden gesplitst in tokens worden geïndexeerd/doorzoekbaar. Zie [analyse in Azure zoeken](https://aka.ms//azsanalysis) voor meer informatie.
- `defaultScoringProfile`gebruikt voor het overschrijven van de standaard gedrag te scoren.
- `corsOptions`cross-oorsprong query's op de index toestaan.

De syntaxis voor het structureren van de verzoek-nettolading is als volgt. Een voorbeeld van een aanvraag verder op in dit onderwerp wordt geleverd.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indexkenmerken**

De volgende kenmerken kunnen worden ingesteld bij het maken van een index. Zie de [score profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx)voor meer informatie over profielen score en score.

`name`-De naam van het veld ingesteld.

`type`-Hiermee stelt u het gegevenstype voor het veld. Zie [Gegevenstypen ondersteund](#DataTypes) voor een lijst met ondersteunde typen.

`searchable`-Geeft het veld full-text search kunnen. Dit betekent dat zij worden onderworpen aan analyses zoals woordafbreking tijdens het indexeren. Als u een `searchable` een waarde zoals 'zonnige dag', intern het veld wordt gesplitst in afzonderlijke tokens wordt gescheiden 'mooi' en 'dag'. Hiermee kunt volledige-tekstzoekacties voor deze termen. Velden van het type `Edm.String` of `Collection(Edm.String)` zijn `searchable` standaard. Velden met andere gegevenstypen kunnen niet worden `searchable`.

  - **Opmerking**: `searchable` velden verbruiken extra ruimte in de index omdat Azure Search een extra tokens versie van de waarde van het veld voor een volledige-tekstzoekacties slaat. Als u ruimte wilt besparen in de index en u niet een veld hoeft moeten worden opgenomen in zoekopdrachten, stelt `searchable` naar `false`.

`filterable`-Kan worden verwezen het veld `$filter` query's. `filterable`verschilt van `searchable` in de verwerking van tekenreeksen. Velden van het type `Edm.String` of `Collection(Edm.String)` die `filterable` geen woordafbreking, ondergaan, zodat vergelijkingen voor exact overeenkomt met alleen zijn. Bijvoorbeeld, als u een dergelijk veld instellen `f` 'zonnige dag' `$filter=f eq 'sunny'` vindt geen resultaten, maar `$filter=f eq 'sunny day'` wordt. Alle velden zijn `filterable` standaard.

`sortable`-Standaard het systeem resultaten sorteren op score, maar in veel ervaringen gebruikers wilt sorteren op de velden in de documenten. Velden van het type `Collection(Edm.String)` kan niet worden `sortable`. Alle andere velden zijn `sortable` standaard.

`facetable`-Meestal gebruikt in een presentatie van zoekresultaten met de treffertelling wordt per categorie (bijvoorbeeld, zoeken naar digitale camera's en Zie treffers op merk, door het aantal megapixels, door de prijs, enz.). Deze optie kan niet worden gebruikt met de velden van het type `Edm.GeographyPoint`. Alle andere velden zijn `facetable` standaard.

  - **Opmerking**: velden van het type `Edm.String` die `filterable`, `sortable`, of `facetable` mag niet meer dan 32 KB in lengte. Dit is omdat deze velden worden behandeld als een enkele zoekterm en de maximale lengte van een term in Azure Search 32KB is. Als u meer tekst dan deze opgeslagen in een veld één tekenreeks, moet u expliciet instellen `filterable`, `sortable`, en `facetable` op `false` in de definitie van de index.

  - **Opmerking**: als een veld geen van de bovenstaande kenmerken ingesteld heeft op `true` (`searchable`, `filterable`, `sortable`, of`facetable`) het veld daadwerkelijk wordt uitgesloten van de omgekeerde index. Deze optie is nuttig voor velden die niet worden gebruikt in query's, maar die nodig zijn in de zoekresultaten. Dergelijke velden uitsluiten in de index verbetert de prestaties.

`key`-Geeft het veld unieke id's voor documenten in de index bevatten. Precies één veld moet worden gekozen als de `key` moeten van het type veld en het `Edm.String`. Belangrijke velden kunnen worden gebruikt om documenten rechtstreeks via de [API opzoeken](#LookupAPI)op te zoeken.

`retrievable`-Bepaalt of het veld kan worden geretourneerd in een zoekresultaat.  Dit is handig wanneer u gebruiken (bijvoorbeeld marge) van een veld als een filter, wilt sorteren of mechanisme scoren, maar niet dat het veld wilt zichtbaar is voor de eindgebruiker. Dit kenmerk moet worden `true` voor `key` velden.

`analyzer`-Hiermee stelt u de naam van de analysator in gebruik voor het veld, op zoektijd en indexeren. Zie [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx)voor de toegestane waarden. Deze optie kan alleen worden gebruikt met `searchable` velden en het kunnen niet worden ingesteld met een `searchAnalyzer` of `indexAnalyzer`.  Zodra de analyzer is gekozen, worden niet gewijzigd voor het veld.

`searchAnalyzer`-De naam van de analysator gebruikt tijdens het zoeken voor het veld ingesteld. Zie [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx)voor de toegestane waarden. Deze optie kan alleen worden gebruikt met `searchable` velden. Moet worden ingesteld samen met `indexAnalyzer` en kan niet worden ingesteld samen met het `analyzer` optie. Deze analyzer kan worden bijgewerkt op een bestaand veld.

`indexAnalyzer`-De naam van de analysator gebruikt bij indexering voor het veld ingesteld. Zie [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx)voor de toegestane waarden. Deze optie kan alleen worden gebruikt met `searchable` velden. Moet worden ingesteld samen met `searchAnalyzer` en kan niet worden ingesteld samen met het `analyzer` optie. Zodra de analyzer is gekozen, worden niet gewijzigd voor het veld.

`suggesters`-Hiermee stelt u de modus en de velden die de bron van de inhoud voor suggesties vormen. Zie [Suggesters](#Suggesters) voor meer informatie.

`scoringProfiles`-Hiermee definieert u aangepaste score gedragingen die kunnen u bepalen welke objecten hoger in de zoekresultaten weergegeven. Score profielen bestaan uit het veld gewicht en functies. Zie [het scoren van de profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie over de kenmerken die worden gebruikt in een profiel score.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Ondersteuning voor talen**

Doorzoekbare velden worden onderworpen aan analyses die de meeste omvat vaak woordafbreking, tekst normalisatie en filteren van termen. Standaard worden doorzoekbare velden in Azure Search geanalyseerd met behulp van [Apache Lucene standaard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) die tekst wordt in de regels["segmentatie Unicode-tekst"](http://unicode.org/reports/tr29/) elementen. De standaard analyzer converteert bovendien alle tekens naar hun vorm in kleine letters. Zowel de geïndexeerde documenten en de zoektermen doorlopen voor de analyse tijdens het indexeren en het verwerken van query's.

Azure Search ondersteunt een groot aantal talen. Elke taal is een niet-standaard tekst analyzer die kenmerken van een bepaalde taal vereist. Azure Search biedt twee soorten analyzers:

- 35 analyzers ondersteund door Lucene.
- 50 analyzers gesteund door eigen Microsoft natuurlijke taal verwerking technologie die wordt gebruikt in Office en Bing.

Sommige ontwikkelaars liever de meer vertrouwde, eenvoudig, open source-oplossing van Lucene. Lucene analyzers sneller zijn, maar de Microsoft analyzers hebben geavanceerde mogelijkheden, zoals Lemmata, word decompounding (in talen zoals het Duits, Deens, Nederlands, Zweeds, Noors, Ests, voltooien, Hongaars, Slowaaks) en erkenning van de entiteit (URL's, e-mailberichten, datums, getallen). Indien mogelijk moet u vergelijkingen van zowel Microsoft als Lucene analyzers om te beslissen welke een beter geschikt is.

***Vergelijk deze***

De Lucene analyzer voor Engels breidt de standaard analyzer. Het verwijdert bezittelijke voornaamwoorden (achter de) van woorden, geldt als gevolg aan de hand van [Porter voortvloeiende algoritme](http://tartarus.org/~martin/PorterStemmer/), en Engelse [woorden te stoppen](http://en.wikipedia.org/wiki/Stop_words).

In de vergelijking voert de Microsoft analyzer Lemmata in plaats van als gevolg. Betekent dit dat deze kan omgaan met verbogen en onregelmatige woordvormen veel beter wat resulteert in meer relevante zoekresultaten (controle module 7 van [Azure Search MVA presentatie](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) voor meer informatie).

Indexeren met Microsoft analyzers is gemiddeld twee tot drie keer langzamer dan hun equivalenten Lucene, afhankelijk van de taal. Het zoeken moet geen noemenswaardige voor query's van gemiddelde grootte.

***Configuratie***

U kunt instellen voor elk veld in de definitie van de index, de `analyzer` eigenschap in op de naam van een analyzer die welke taal en leverancier aangeeft. Bij het indexeren en zoeken naar dat veld wordt dezelfde analysator toegepast.
U kunt bijvoorbeeld afzonderlijke velden voor Engels, Frans en Spaans hotel beschrijvingen die naast elkaar in dezelfde index bestaat. De [queryparameter 'searchFields'](#SearchQueryParameters) om aan te geven welk veld taalspecifieke tegen in uw query's zoeken gebruiken. Kunt u voorbeelden van query opnemen controleren de `analyzer` eigenschap in [Documenten zoeken](#SearchDocs). 

***Lijst Analyzer***

Hieronder vindt u de lijst met ondersteunde talen en de namen van Lucene en Microsoft analyzer.

<table style="font-size:12">
    <tr>
        <th>Taal</th>
        <th>Naam van de Microsoft-analyzer</th>
        <th>Naam van Lucene analyzer</th>
    </tr>
    <tr>
        <td>Arabisch</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>Armeens</td>
        <td></td>
        <td>Hy.lucene</td>
    </tr>
    <tr>
        <td>Bengaals</td>
        <td>bn.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Baskisch</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>Bulgaars</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>Catalaans</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
    </tr>
    <tr>
        <td>Vereenvoudigd Chinees</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>     
    </tr>
    <tr>
        <td>Traditioneel Chinees</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>     
    <tr>
    <tr>
        <td>Kroatisch</td>
        <td>HR.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Tsjechisch</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>      
    </tr>    
    <tr>
        <td>Deens</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>Nederlands</td>
        <td>nl.Microsoft</td>
        <td>nl.lucene</td>  
    </tr>    
    <tr>
        <td>Engels</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>      
    </tr>
    <tr>
        <td>Estisch</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Fins</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>      
    </tr>    
    <tr>
        <td>Frans</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>Galicisch</td>
        <td></td>
        <td>GL.lucene</td>      
    </tr>
    <tr>
        <td>Duits</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>Grieks</td>
        <td>El.Microsoft</td>
        <td>El.lucene</td>      
    </tr>
    <tr>
        <td>Gujarati</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebreeuws</td>
        <td>He.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>      
    </tr>
    <tr>
        <td>Hongaars</td>      
        <td>Hu.Microsoft</td>
        <td>Hu.lucene</td>
    </tr>
    <tr>
        <td>IJslands</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonesisch (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>Iers</td>
        <td></td>
        <td>Ga.lucene</td>
    </tr>
    <tr>
        <td>Italiaans</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>Japans</td>
        <td>Ja.Microsoft</td>
        <td>Ja.lucene</td>
        
    </tr>
    <tr>
        <td>Kannada</td>
        <td>Ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Koreaans</td>
        <td>Ko.Microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Lets</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>Litouws</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maleis (Latijn)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Noors</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>Perzisch</td>
        <td></td>
        <td>FA.lucene</td>      
    </tr>
    <tr>
        <td>Pools</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>      
    </tr>
    <tr>
        <td>Portugees (Brazilië)</td>
        <td>pt Br.microsoft</td>
        <td>pt Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugees (Portugal)</td>
        <td>pt Pt.microsoft</td>        
        <td>pt Pt.lucene</td>
    </tr>
    <tr>
        <td>Punjabi</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Roemeens</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Russisch</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>  
    </tr>
    <tr>
        <td>Servisch (Cyrillisch)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Servisch (Latijn)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slowaaks</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Sloveens</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Spaans</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>Zweeds</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>Tamil</td>
        <td>TA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thais</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>Turks</td>
        <td>TR.Microsoft</td>
        <td>TR.lucene</td>      
    </tr>
    <tr>
        <td>Oekraïens</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamees</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Bovendien biedt Azure Search taal agnostic analyzer configuraties</td>
    <tr>
        <td>Standaard-ASCII vouwen</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode-tekst segmentatie (standaard Basisvormherkenning)</li>
            <li>ASCII-opvouwbaar filter - converteert Unicode-tekens die niet behoren tot de eerste 127 ASCII-tekens in de ASCII-equivalenten. Dit is handig voor het verwijderen van diakritische tekens.</li>
        </ul>
        </td>
    </tr>
</table>

Alle analyzers met namen gemarkeerd met <i>lucene</i> worden aangedreven door [de Apache Lucene taal analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Meer informatie over het filter vouwen ASCII vindt u [hier](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` bepaalt u welke velden in een index worden gebruikt ter ondersteuning van automatisch aanvullen in zoekopdrachten. Gedeeltelijke zoekstrings worden meestal verzonden naar de [API suggesties](#Suggestions) terwijl de gebruiker een zoekopdracht typen is en de API een voorgestelde zinnen retourneert. Een suggester die u in de index definieert bepaalt welke velden worden gebruikt voor het bouwen van de type-ahead zoektermen. Zie [Suggesters](#Suggesters) voor configuratiedetails.

**Score van profielen**

A `scoringProfile` definieert aangepaste score gedragingen die kunnen u bepalen welke objecten hoger in de zoekresultaten weergegeven. Score profielen bestaan uit het veld gewicht en functies. Hiervoor geeft u een profiel met de naam van de query-tekenreeks.

Een standaard profiel scoren werkt achter de schermen om een zoekopdracht score voor elk item in een resultaatset te berekenen. U kunt de interne naamloos score profiel. U kunt ook instellen `defaultScoringProfile` gebruik van een aangepast profiel standaard geactiveerd wanneer u een aangepast profiel niet is opgegeven in de query-tekenreeks.

Zie [de score profielen toevoegen aan een zoekindex (Azure Search Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) voor meer informatie.

**CORS opties**

Client-side Javascript aanroepen niet alle API's standaard omdat de browser voorkomen alle verzoeken van verschillende oorsprong dat wordt. CORS (Cross-oorsprong resources delen) inschakelen door de `corsOptions` kenmerk cross oorsprong query's naar de index toestaan. Opmerking alleen query API's ondersteuning CORS om veiligheidsredenen. De volgende opties kunnen voor CORS worden ingesteld:

- `allowedOrigins`(vereist): dit is een lijst van oorsprong dat toegang tot de index wordt verleend. Dit betekent dat eventuele Javascript-code van de oorsprongen bediend kunnen worden uw (ervan uitgaande dat het biedt de juiste API-sleutel) index opvragen. Elke oorsprong heeft meestal de vorm `protocol://fully-qualified-domain-name:port` Hoewel de poort wordt vaak weggelaten. Raadpleeg [dit artikel](http://go.microsoft.com/fwlink/?LinkId=330822) voor meer informatie.
 - Als u toegang wilt verlenen aan alle oorsprongen, `*` als een afzonderlijk item in de `allowedOrigins` array. Houd er rekening mee dat **Dit wordt niet aanbevolen praktijken voor productie-zoekservices.** Echter kan het dienstig zijn voor de ontwikkeling en foutopsporing.
- `maxAgeInSeconds`(optioneel): Browsers deze waarde gebruiken om te bepalen van de duur (in seconden) cache CORS Preflight-antwoorden. Dit moet een niet-negatief geheel getal. Hoe hoger deze waarde is, worden de prestaties te verbeteren, maar hoe langer het duurt CORS wijzigingen worden pas van kracht. Als niet is ingesteld, wordt een standaardwaarde voor duur van 5 minuten gebruikt.

<a name="CreateUpdateIndexExample"></a>
**Voorbeeld van de instantie aanvragen**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Reactie**

Voor een succesvolle aanvraag: '201 gemaakt'.

Het hoofdgedeelte van de response bevat standaard de JSON voor de definitie van de index die is gemaakt. Als de `Prefer` verzoek-header is ingesteld op `return=minimal`, het hoofdgedeelte van de response is leeg en is de statuscode succes "204 geen inhoud ' in plaats van '201 gemaakt'. Dit geldt ook als PUT- of POST is gebruikt om de index te maken.

**Opmerkingen**

Momenteel is er beperkte ondersteuning voor het schema van index-updates. De schema-updates die worden opnieuw indexeren moeten zou zoals het wijzigen van veldtypen worden momenteel niet ondersteund. Hoewel de bestaande velden niet worden gewijzigd of verwijderd, worden nieuwe velden kunnen op elk gewenst moment worden toegevoegd aan een bestaande index. Wanneer u een nieuw veld toevoegt, hebben alle documenten in de index automatisch een null-waarde voor dat veld. Geen extra opslagruimte wordt verbruikt als nieuwe documenten zijn toegevoegd aan de index.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

De functie voor suggesties in Azure Search is een type-ahead of automatisch aanvullen query-mogelijkheid, met een lijst van mogelijke zoektermen in reactie op "inputs" gedeeltelijke tekenreeks ingevoerd in een zoekvak. U vast opgevallen Querysuggesties bij het gebruik van commerciële webzoekmachines: ".NET" typen in Bing produceert een lijst van termen voor ".NET 4.5 uitvoeren ', '.NET Framework 3.5", enzovoort. Wanneer u de zoekservice REST API, vereist ter uitvoering van de suggesties in een aangepaste toepassing voor Azure Search het volgende:

- Suggesties inschakelen door de bouw van een **suggester** in de index, waarbij de naam, de modus en een lijst met velden voor dat type-ahead wordt aangeroepen. Als u 'stad' als een bronveld gedeeltelijke zoektekenreeks 'Zee' te typen zal in "Seattle", "Strand" en "Seatac" (alle drie zijn werkelijke plaatsnamen) aangeboden als suggesties voor de query voor de gebruiker resulteren.

- Suggesties door het aanroepen van de [API van de suggesties](#Suggestions) in uw toepassingscode aanroepen. Gedeeltelijke zoekstrings worden meestal verzonden naar de service terwijl de gebruiker een zoekopdracht typen is en deze API een voorgestelde zinnen retourneert.

In dit artikel wordt uitgelegd hoe u een **suggester**configureren. Controleer ook de [Suggesties API](#Suggestions) voor meer informatie over hoe een suggester wordt gebruikt.

**Gebruik**

`Suggesters`worden gemaakt in de index en ideaal voor het voorstellen van specifieke documenten in plaats van losse termen of woordgroepen. De beste kandidaat zijn titels, namen en andere relatief korte zinnen die een item kunnen identificeren. Minder effectief worden herhalende velden, zoals tags en categorieën of zeer lange velden, zoals velden voor beschrijvingen of opmerkingen.

Als onderdeel van de definitie van de index, kunt u toevoegen een enkel suggester aan de `suggesters` collectie. De volgende zijn eigenschappen waarmee een suggester:

- `name`: De naam van de suggester. U gebruikt de naam van de suggester bij het aanroepen van de `suggest` API.
- `searchMode`: De strategie die wordt gebruikt om te zoeken naar woordgroepen kandidaat. De modus alleen ondersteund op dit moment is `analyzingInfixMatching`, die worden uitgevoerd overeenkomen met flexibele van vermeldingen aan het begin of in het midden van de zinnen.
- `sourceFields`: Een lijst met een of meer velden die de bron van de inhoud voor suggesties vormen. Alleen velden van het type `Edm.String` en `Collection(Edm.String)` mogelijk bronnen voor suggesties. Alleen de velden die een aangepaste taal analyzer instellen niet kunnen worden gebruikt.

**Voorbeeld van de suggester**

Een suggester maakt deel uit van de index. Slechts één suggester kan bestaan in de `suggesters` -collectie in de huidige versie, naast de fields-collectie en `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Als u de openbare preview-versie van Azure Search, `suggesters` wordt vervangen door een oudere Booleaanse eigenschap (`"suggestions": false`) die alleen voorvoegsel suggesties voor korte reeksen (3-25 tekens) worden ondersteund. De vervanging, `suggesters`, ondersteunt infix waarmee wordt gezocht naar overeenkomende termen aan het begin of in het midden van de veldinhoud van het, met betere tolerantie voor fouten in de zoekstrings overeenkomen. Beginnen met de algemeen beschikbare versie, is dit nu de enige implementatie van de suggesties API. De oudere `suggestions` die is geïntroduceerd in de eigenschap `api-version=2014-07-31-Preview` in die versie werkt nog steeds, maar is niet operationeel is in de `2015-02-28` of een latere versie van Azure Search.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Index bijwerken

U kunt een bestaande index in Azure zoeken op basis van een aanvraag voor het plaatsen van HTTP bijwerken. Updates kunnen nieuwe velden toe te voegen aan het bestaande schema, CORS opties wijzigen en wijzigen, score profielen bevatten. Zie [het scoren van de profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie. U de naam van de index bij te werken op de aanvraag-URI:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Belangrijk:** Ondersteuning voor schema-indexupdates is beperkt tot bewerkingen die niet opnieuw samenstellen van de zoekindex. De schema-updates die opnieuw indexeren, zoals het wijzigen van veldtypen, zou moeten worden momenteel niet ondersteund. Nieuwe velden kunnen op elk moment worden toegevoegd, hoewel bestaande velden niet worden gewijzigd of verwijderd. Hetzelfde geldt voor `suggesters`. Nieuwe velden kunnen worden toegevoegd aan een suggester op het moment dat de velden worden toegevoegd, maar velden kunnen niet worden verwijderd uit `suggesters` en de bestaande velden niet worden toegevoegd aan `suggesters`.

Wanneer u een nieuw veld toevoegt aan een index, hebben alle documenten in de index automatisch een null-waarde voor dat veld. Geen extra opslagruimte wordt verbruikt als nieuwe documenten zijn toegevoegd aan de index.

**Aanvraag**

HTTPS is vereist voor alle aanvragen. De **Index bijwerken** aanvraag gemaakt met HTTP-plaatsen. De naam van de index is opgeslagen, deel van de URL. Als de index bestaat, wordt deze gemaakt. Als de index al bestaat, wordt deze bijgewerkt met de nieuwe definitie.

De naam van de index moet worden in kleine letters, beginnen met een letter of cijfer hebben geen slashes of punten en minder dan 128 tekens bevatten. De rest van de naam kan na het starten van de naam van de index met een letter of cijfer bevatten een letter, getal en streepjes, zolang de streepjes niet aaneensluitend zijn.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `Content-Type`: Vereist. Deze worden ingesteld`application/json`
- `api-key`: Vereist. De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Het is een tekenreekswaarde die uniek zijn voor uw service. De **Index bijwerken** aanvraag moet bevatten een `api-key` ingesteld aan de admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Syntaxis voor hoofdtekst**

Bij het bijwerken van een bestaande index moet de instantie bevatten de oorspronkelijke schemadefinitie plus de nieuwe velden die u wilt toevoegen, evenals de gewijzigde score profielen, suggesters en CORS opties, indien van toepassing. Als u niet de score profielen en CORS opties wijzigt, moet u opnemen de originelen wanneer de index is gemaakt. In het algemeen het beste patroon voor updates is de definitie van de index met een GET ophalen, wijzigen en vervolgens bijwerken met PUT.

De schema-syntaxis gebruikt voor het maken van een index is hier opgenomen voor het gemak. Zie [Index maken](#CreateIndex) voor meer informatie.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Reactie**

Voor een succesvolle aanvraag: "204 geen inhoud '.

Standaard wordt het hoofdgedeelte van de response leeg zijn. Echter, als de `Prefer` verzoek-header is ingesteld op `return=representation`, de hoofdtekst van het antwoord bevat de JSON voor de definitie van de index die is bijgewerkt. In dit geval de statuscode succes zal worden "200 OK".

**Definitie van de index wordt bijgewerkt met aangepaste analyzers**

Zodra een analyzer, een Basisvormherkenning, een token filter of een char-filter is gedefinieerd, kunnen niet worden gewijzigd. Nieuwe kunnen alleen worden toegevoegd aan een bestaande index als de `allowIndexDowntime` vlag is ingesteld op true in de aanvraag van de index-update: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Houd er rekening mee dat deze bewerking zou de index offline voor ten minste een paar seconden, waardoor de indexering en query-aanvragen mislukken. Beschikbaarheid en wegschrijven van de index is een bijzondere waardevermindering gedurende enkele minuten nadat de index wordt bijgewerkt, of langer voor zeer grote indexen.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Lijst met indexen

De **Lijst indexen** bewerking wordt een lijst met de indexen in Azure-zoekservice.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor alle aanvragen. De **Lijst indexen** aanvraag kan worden samengesteld met de methode GET.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `api-key`: Vereist. De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Het is een tekenreekswaarde die uniek zijn voor uw service. De **Lijst indexen** aanvraag moet bevatten een `api-key` ingesteld op een admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Geen.

**Reactie**

: 200 OK de statuscode voor een positief antwoord.

Hier is een voorbeeld van de reactie instantie:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Houd er rekening mee dat u het antwoord naar eigenschappen die u geïnteresseerd bent in kunt filteren. Bijvoorbeeld gebruiken als u wilt dat alleen een lijst met indexnamen, de OData `$select` optie query:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

In dit geval wordt zou de reactie van het bovenstaande voorbeeld als volgt uitzien:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Dit is een handige techniek om bandbreedte te besparen als u een groot aantal indexen in de Search-service.

<a name="GetIndex"></a>
## <a name="get-index"></a>Index opvragen

De **Index Get** -bewerking wordt de definitie van de index in Azure zoeken.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor aanvragen. De aanvraag **Get-Index** kan worden samengesteld met de methode GET.

[Naam van de index] in de URI-aanvraag geeft aan welke index om uit de indexes-collectie te retourneren.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `api-key`: De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Het is een tekenreekswaarde die uniek zijn voor uw service. De **Index Get** -aanvraag moet bevatten een `api-key` ingesteld op een admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Geen.

**Reactie**

: 200 OK de statuscode voor een positief antwoord.

Zie het voorbeeld JSON in [maken en bijwerken van een Index](#CreateUpdateIndexExample) voor een voorbeeld van de nettolading van het antwoord.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Index verwijderen

De werking van de **Index verwijderen** verwijdert een index en de bijbehorende documentatie van uw Azure Search-service. U kunt de naam van de index van het servicedashboard in Azure portal of van de API. Zie de [Lijst met indexen](#ListIndexes) voor meer informatie.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor aanvragen. De **Index verwijderen** aanvraag kan worden samengesteld met behulp van de methode DELETE.

[Naam van de index] in de aanvraag-URI welke index verwijderen uit de indexes-collectie aangeeft.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `api-key`: Vereist. De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Een tekenreekswaarde die uniek zijn voor uw service-URL is. De **Index verwijderen** aanvraag moet bevatten een `api-key` ingesteld aan de admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Geen.

**Reactie**

Statuscode: 204 geen inhoud wordt geretourneerd voor een positief antwoord.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Indexstatistieken opvragen

De bewerking **Get indexstatistieken** geeft als resultaat van Azure zoeken een aantal documenten voor de huidige index plus gebruik van opslag.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Statistieken over het aantal en opslag documentgrootte verzameld om de paar minuten, niet in real-time. De statistieken die worden geretourneerd door deze API kan daarom niet overeen met wijzigingen veroorzaakt door recente indexing bewerkingen.

**Aanvraag**

HTTPS is vereist voor alle services aanvragen. De aanvraag **Get indexstatistieken** kan worden samengesteld met de methode GET.

[Naam van de index] in de aanvraag-URI wordt de service om terug te keren indexstatistieken voor de opgegeven index.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.


**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `api-key`: De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Het is een tekenreekswaarde die uniek zijn voor uw service. De aanvraag **Get indexstatistieken** moet bevatten een `api-key` ingesteld op een admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Geen.

**Reactie**

: 200 OK de statuscode voor een positief antwoord.

Het hoofdgedeelte van de response wordt in de volgende notatie:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Test Analyzer

De **API analyseren** ziet u hoe een analyzer-einden tekst in tokens.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor alle services aanvragen. Het **Analyseren van API** -verzoek kan worden samengesteld met de methode POST.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.


**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `api-key`: De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Het is een tekenreekswaarde die uniek zijn voor uw service. De **API analyseren** aanvraag moet bevatten een `api-key` ingesteld op een admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de index en de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

of

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

De `analyzer_name`, `tokenizer_name`, `token_filter_name` en `char_filter_name` moet een geldige naam van een vooraf gedefinieerde of aangepaste analyzers, tokenizers, token filters en char filters voor de index. Voor meer dat informatie over het proces van lexicale analyse Zie [analyse in Azure zoeken](https://aka.ms/azsanalysis).

**Reactie**

: 200 OK de statuscode voor een positief antwoord.

Het hoofdgedeelte van de response wordt in de volgende notatie:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Voorbeeld van de API analyseren**

**Aanvraag**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Reactie**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Document-bewerkingen

In Azure zoeken, een index wordt opgeslagen in de cloud en gevuld met JSON-documenten die u naar de service uploadt. Alle documenten die u uploadt omvat de corpus van de gegevens van uw zoekopdracht. Documenten bevatten, waarvan sommige zijn ge? exeerd in de zoektermen als ze worden geüpload. De `/docs` segment in de API Azure Search URL vertegenwoordigt de collectie van documenten in een index. Alle bewerkingen die worden uitgevoerd op de collectie zoals uploaden, samenvoegen, verwijderen of het opvragen van documenten nemen plaats in de context van een enkele index zodat de URL's voor deze bewerkingen begint altijd met `/indexes/[index name]/docs` voor een naam voor de opgegeven index.

Uw toepassingscode moet ofwel JSON-documenten uploaden naar Azure Search genereren of u kunt een [indexeerfunctie](https://msdn.microsoft.com/library/dn946891.aspx) documenten laden als de gegevensbron Azure SQL-Database of DocumentDB is. Indexen worden meestal gevuld uit een enkele dataset die u opgeeft.

U moet van plan bent met één document voor elk item dat u wilt zoeken. Een film verhuur toepassing mogelijk één document per film, een eBay-winkel toepassing mogelijk één document per SKU, een toepassing voor on line courseware mogelijk één document per cursus, een onderzoeksbureau kan er een document voor elke academische papier in de opslagplaats, enzovoort.

Documenten bestaan uit een of meer velden. Velden kunnen bevatten tekst die wordt ge? exeerd door Azure Search in zoektermen als niet ge? exeerd of niet-tekstwaarden die kunnen worden gebruikt in de filters of score profielen. De namen, gegevenstypen en zoekfuncties voor elk veld dat wordt ondersteund worden door het schema van de index bepaald. Een van de velden in elke index-schema moet worden aangewezen als een ID en elk document moet een waarde hebben voor de veld-ID die uniek dat document in de index. Alle andere documentvelden zijn optioneel en wordt standaard een null-waarde als niet opgegeven. Houd er rekening mee dat null-waarden geen ruimte in de zoekindex hebben.

Voordat u documenten uploaden kunt, moet u al hebben gemaakt de index van de service. Zie [Index maken](#CreateIndex) voor meer informatie over deze eerste stap.

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Toevoegen, bijwerken of verwijderen van documenten

U kunt uploaden, documenten samenvoegen, samenvoegen of uploaden of verwijderen uit een opgegeven index met behulp van HTTP POST. Voor een groot aantal updates, wordt batchen van documenten (documenten per batch 1000) of 16 MB per batch aanbevolen.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor alle aanvragen. U kunt uploaden, documenten samenvoegen, samenvoegen of uploaden of verwijderen uit een opgegeven index met behulp van HTTP POST.

De aanvraag-URI bevat [naam van de index], welke documenten boeken-index op te geven. U kunt alleen documenten één index tegelijkertijd boeken.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `Content-Type`: Vereist. Deze worden ingesteld`application/json`
- `api-key`: Vereist. De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Het is een tekenreekswaarde die uniek zijn voor uw service. De aanvraag **Documenten toevoegen** moet bevatten een `api-key` ingesteld aan de admin-sleutel (in plaats van een query-sleutel).

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

De hoofdtekst van de aanvraag bevat een of meer documenten moeten worden geïndexeerd. Documenten worden aangeduid met een unieke sleutel. Elk document dat is gekoppeld aan een actie: upload, samenvoegen, mergeOrUpload of verwijderen. Upload-aanvragen moeten de gegevens van het document als een set van sleutel/waarde-paren bevatten.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Document sleutels mogen alleen letters, cijfers, streepjes ('-'), onderstrepingstekens ('_') en gelijktekens ("="). Zie [Naming-regels](https://msdn.microsoft.com/library/azure/dn857353.aspx)voor meer informatie.

**Documentacties**

- `upload`: Een upload-actie is vergelijkbaar met een 'upsert' waar het document wordt ingevoegd als het nieuwe en bijgewerkt/vervangen als deze bestaat. Houd er rekening mee dat alle velden in het geval van de update zijn vervangen.
- `merge`: Een bestaand document samenvoegen bijgewerkt met de opgegeven velden. Als het document niet bestaat, mislukt het samenvoegen. Elk veld dat u in een samenvoegbewerking wordt vervangen door het bestaande veld in het document. Dit omvat velden van het type `Collection(Edm.String)`. Als het document 'tags' met een waarde van een veld bevat bijvoorbeeld `["budget"]` en uitvoeren van een samenvoegbewerking met de waarde `["economy", "pool"]` voor "codes", de laatste waarde van het veld 'tags' zijn `["economy", "pool"]`. Het zal **niet** worden `["budget", "economy", "pool"]`.
- `mergeOrUpload`: gedraagt zich als `merge` als een document met de opgegeven sleutel al in de index bestaat. Als het document nog niet bestaat gedraagt zich als `upload` met een nieuw document.
- `delete`: Verwijderen Hiermee verwijdert u het opgegeven document uit de index. Houd er rekening mee dat alle velden die u in een `delete` dan het sleutelveld wordt genegeerd. Als u een afzonderlijk veld verwijderen uit een document wilt, gebruikt u `merge` gewoon en in plaats daarvan stelt u het veld expliciet naar `null`.

**Reactie**

Statuscode 200 is (OK) geretourneerd voor een positief antwoord, wat betekent dat alle artikelen is geïndexeerd. Dit wordt aangegeven door de `status` wordt de eigenschap ingesteld op true voor alle items, ook als de `statusCode` eigenschap wordt ingesteld op 201 (voor zojuist geüploade documenten) of 200 (voor samengevoegde of verwijderde documenten):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

(Meerdere Status) 207 statuscode wanneer ten minste één item is niet geïndexeerd. Die niet zijn geïndexeerd, hebben de `status` veld is ingesteld op false. De `errorMessage` en `statusCode` eigenschappen zal geven aan de reden voor de indexing-fout:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

De volgende tabel beschrijft de verschillende per document statuscodes kunnen worden geretourneerd in het antwoord. Houd er rekening mee dat sommige duiden op problemen met het verzoek zelf, terwijl anderen tijdelijke foutsituaties geven. Deze die moet u opnieuw na een vertraging.

<table style="font-size:12">
    <tr>
        <th>Statuscode</th>
        <th>Betekenis</th>
        <th>Herstelbare</th>
        <th>Notities</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Document is gewijzigd of verwijderd.</td>
        <td>n.v.t.</td>
        <td>DELETE-bewerkingen zijn <a href="https://en.wikipedia.org/wiki/Idempotence">idempotency is ingeschakeld</a>. Dat wil zeggen, zelfs als een documentsleutel niet in de index bestaat, resulteert probeert een bewerking met de sleutel die in een statuscode van 200.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Document is gemaakt.</td>
        <td>n.v.t.</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Er is een fout in het document dat niet wordt geïndexeerd.</td>
        <td>Nee</td>
        <td>Het foutbericht in het antwoord wordt aangegeven wat is er mis met het document.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Het document kan niet worden samengevoegd omdat de opgegeven sleutel niet in de index bestaat.</td>
        <td>Nee</td>
        <td>Deze fout treedt niet op voor uploads, aangezien deze nieuwe documenten maken en treedt niet op bij verwijderd omdat ze <a href="https://en.wikipedia.org/wiki/Idempotence">idempotency is ingeschakeld</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Een conflict is gevonden wanneer u probeert een document te indexeren.</td>
        <td>Ja</td>
        <td>Dit kan gebeuren als u probeert meer dan één keer gelijktijdig in hetzelfde document indexeren.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>De index is tijdelijk niet beschikbaar omdat het met de vlag 'allowIndexDowntime' ingesteld op 'true' is bijgewerkt.</td>
        <td>Ja</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>De search-service is tijdelijk niet beschikbaar is, mogelijk als gevolg van zware belasting.</td>
        <td>Ja</td>
        <td>De code moet worden gewacht voordat opnieuw wordt geprobeerd het in dit geval of u risico-verlenging van de service niet beschikbaar.</td>
    </tr>
</table> 

**Opmerking**: als uw clientcode wordt vaak een 207 antwoord tegenkomt, een mogelijke reden hiervoor is dat het systeem onder belasting. U kunt dit controleren door het controleren van de `statusCode` eigenschap voor 503. Als dit het geval is, raden we aan ***indexing aanvragen te beperken***. Anders als indexing verkeer niet subside, kan start het systeem weigeren van alle aanvragen met een 503-fouten.

Statuscode 429 geeft aan dat de quota van het aantal documenten per index is overschreden. Een nieuwe index maken of bijwerken voor beperkingen van de hogere capaciteit.

**Voorbeeld:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Documenten zoeken

**Een zoekbewerking** als GET of POST-aanvraag wordt verstrekt en parameters die de criteria voor het selecteren van de overeenkomstige documenten geven.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Wanneer gebruikt u in plaats van GET boeken**

Als u HTTP GET **Search** API aanroept, moet u rekening houden dat de lengte van de aanvraag-URL mag niet meer dan 8 KB. Dit is meestal voldoende voor de meeste toepassingen. Echter, sommige toepassingen zeer grote query's of OData filterexpressies produceren. Voor deze toepassingen is met behulp van HTTP POST een goede keuze omdat daardoor grotere filters en query's dan ophalen. Met de POST is het aantal voorwaarden of componenten in een query de beperkende factor, niet de grootte van de ruwe query omdat de limiet voor de aanvraag voor een bericht 16 MB is.

> [AZURE.NOTE] Hoewel de limiet voor de POST-aanvraag zeer groot is, zoekopdrachten en filterexpressies mogen niet willekeurig complex. Zie [Lucene query-syntaxis](https://msdn.microsoft.com/library/mt589323.aspx) en [OData-syntaxis voor expressies](https://msdn.microsoft.com/library/dn798921.aspx) voor meer informatie over beperkingen voor search query- en complexiteit.

**Aanvraag**

HTTPS is vereist voor aanvragen. **De zoekopdracht** kan worden samengesteld met behulp van de methoden GET of POST.

De aanvraag-URI geeft aan welke voor alle documenten die overeenkomen met de parameters van query-index. Parameters zijn opgegeven in de query-tekenreeks voor GET-aanvragen en in de hoofdtekst van POST vraagt.

Aanbevolen bij het maken van GET-aanvragen, moet u specifieke query-parameters [URL coderen](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) bij het rechtstreeks aanroepen van de REST API. Voor **zoekopdrachten** omvat dit:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL-codering wordt alleen aanbevolen voor de bovenstaande query-parameters. Als u per ongeluk URL coderen de hele query-tekenreeks (alles na het?), aanvragen, worden verbroken.

Ook URL-codering is alleen nodig wanneer het direct ophalen met REST-API wordt aangeroepen. Geen URL-codering is noodzakelijk bij het aanroepen van **Zoeken** met de POST of bij het gebruik van de [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx)die URL-codering voor u verwerkt.

<a name="SearchQueryParameters"></a>
**Query-Parameters**

**Zoeken** accepteert van verschillende parameters die querycriteria geven en ook zoekgedrag opgeven. U opgeven dat deze parameters in de URL-queryreeks bij het aanroepen van **Zoeken** via Ontvang en als JSON-eigenschappen in het hoofdgedeelte van de aanvraag bij het aanroepen van **Zoeken** via de POST. De syntaxis voor sommige parameters verschilt enigszins tussen GET en POST. Deze verschillen worden aangegeven als onder die van toepassing zijn:

`search=[string]`(optioneel) - de tekst waarnaar wordt gezocht. Alle `searchable` velden worden standaard doorzocht, tenzij `searchFields` is opgegeven. Bij het zoeken naar `searchable` velden in de tekst zelf wordt ge? exeerd, zodat meerdere voorwaarden kunnen worden gescheiden door een spatie (bijvoorbeeld: `search=hello world`). Overeenkomt met de term, gebruikt u `*` (dit kan handig zijn voor query's de boolean-filter). Als u deze parameter weglaat, heeft hetzelfde effect als het instellen op `*`. Zie [Eenvoudige Query-syntaxis](https://msdn.microsoft.com/library/dn798920.aspx) voor specifieke informatie over de syntaxis van de zoekopdracht.

  - **Opmerking**: de resultaten kunnen soms verrassend zijn bij het opvragen via `searchable` velden. De tokenizer bevat de logica voor de afhandeling van aanvragen voor Engelse tekst zoals apostrofs, komma's in getallen, enz. Bijvoorbeeld `search=123,456` komt overeen met één term 123,456 in plaats van de afzonderlijke termen 123 en 456, omdat komma's worden gebruikt als duizend scheidingstekens voor grote getallen in het Engels. Daarom wordt aangeraden met een witte ruimte in plaats van leestekens te scheiden van de voorwaarden in de `search` parameter.

`searchMode=any|all`(optioneel, standaard `any`) - of één of alle zoektermen overeen moeten komen om te tellen van het document als een overeenkomst.

`searchFields=[string]`(optioneel) - de lijst met veldnamen te zoeken naar opgegeven tekst gescheiden door komma's. Doelvelden moeten worden gemarkeerd als `searchable`.

`queryType=simple|full`(optioneel, standaard `simple`) - wanneer ingesteld op "simple" tekst geïnterpreteerd met gebruikmaking van een eenvoudige query-taal waarmee voor symbolen, zoals +, * en ' '. Query's over alle doorzoekbare velden geëvalueerd (of velden die zijn aangegeven in `searchFields`) in elk document standaard. Wanneer het querytype is ingesteld op `full` tekst geïnterpreteerd met gebruikmaking van de querytaal Lucene, waardoor zoekacties veld-specifieke en gewogen. Zie [Eenvoudige Query-syntaxis](https://msdn.microsoft.com/library/dn798920.aspx) en [Lucene Query-syntaxis](https://msdn.microsoft.com/library/mt589323.aspx) voor specifieke informatie over de syntaxis van de zoekopdracht. 
 
> [AZURE.NOTE] Zoeken in de querytaal wordt niet ondersteund door de $filter die soortgelijke functionaliteit biedt Lucene liggen.

`moreLikeThis=[key]`(optioneel) **Belangrijk:** Deze functie is alleen beschikbaar in `2015-02-28-Preview`. Deze optie kan niet worden gebruikt in een query met de parameter text search `search=[string]`. De `moreLikeThis` parameter vindt documenten die vergelijkbaar met het document dat is opgegeven door de documentsleutel zijn. Wanneer een zoekopdracht wordt aangevraagd met `moreLikeThis`, een lijst van zoektermen die is gegenereerd op basis van de frequentie en de zeldzaamheid van de voorwaarden in het brondocument. Deze termen worden gebruikt voor de aanvraag. Standaard wordt de inhoud van alle `searchable` velden worden beschouwd, tenzij `searchFields` wordt gebruikt om te beperken welke velden worden doorzocht.  

`$skip=#`(optioneel) - het aantal zoekresultaten over te slaan; Kan niet groter zijn dan 100.000. Als u nodig hebt voor het scannen van documenten in de reeks, maar kan niet worden gebruikt `$skip` als gevolg van deze beperking, kunt u overwegen `$orderby` voor een sleutel volledig besteld en `$filter` met een bereik opvragen in plaats daarvan.

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `skip` in plaats van `$skip`.

`$top=#`(optioneel) - het aantal zoekresultaten op te halen. Dit kan worden gebruikt in combinatie met `$skip` voor de uitvoering van client-side paginering van de zoekresultaten.

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `top` in plaats van `$top`.

`$count=true|false`(optioneel, standaard `false`)-geeft aan of het totale aantal resultaten ophalen. Dit is het aantal van alle documenten die overeenkomen met de `search` en `$filter` parameters worden genegeerd `$top` en `$skip`. Als u deze waarde op `true` kunnen hebben invloed op de prestaties. Houd er rekening mee dat de geretourneerde telling een benadering is.

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `count` in plaats van `$count`.

`$orderby=[string]`(optioneel) - een lijst met expressies voor het sorteren van de resultaten door komma's gescheiden. Elke expressie kan bestaan uit een veldnaam of een aanroep van de `geo.distance()` functie. Elke expressie kan worden gevolgd door `asc` aangegeven oplopend, en `desc` om aan te geven aflopend. De standaardinstelling is oplopend. Ties wordt opgesplitst door de scores vergelijken van documenten. Als er geen `$orderby` is opgegeven, wordt de standaardsorteervolgorde op document match score is aflopend. Er is een limiet van 32 clausules voor `$orderby`.

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `orderby` in plaats van `$orderby`.

`$select=[string]`(optioneel) - een lijst met door komma's gescheiden velden op te halen. Als u niets opgeeft, worden alle velden die zijn gemarkeerd als worden opgehaald in het schema opgenomen. U kunt ook aanvragen van alle velden door deze parameter in te stellen op `*`.

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `select` in plaats van `$select`.

`facet=[string]`(nul of meer) - facet van een veld. De tekenreeks kan eventueel parameters voor het aanpassen van de faceting uitgedrukt als door komma's gescheiden `name:value` paren. Geldige parameters zijn:

- `count`(max. aantal voorwaarden facet; de standaardwaarde is 10). Er is geen maximum, maar hogere waarden een bijbehorende prestaties oplopen, vooral als het veld beperkt een groot aantal unieke termen bevat.
  - Bijvoorbeeld: `facet=category,count:5` wordt de eerste vijf categorieën in facet resultaten opgehaald.  
  - **Opmerking**: als de `count` parameter kleiner is dan het aantal unieke termen, de resultaten mogelijk niet correct. Dit komt door de manier waarop query's faceting zijn verdeeld over shards. Toenemende `count` in het algemeen verbetert de nauwkeurigheid van de term tellingen, maar tegen een kostprijs van de prestaties.
- `sort`(een van de `count` om te sorteren op *aflopende* te tellen, `-count` sorteren *Oplopend* aantal `value` sorteren *Oplopend* op waarde, of `-value` om te sorteren op *aflopende* waarde)
  - Bijvoorbeeld: `facet=category,count:3,sort:count` haalt de drie belangrijkste categorieën in facet resultaten in aflopende volgorde door het aantal documenten met de naam van elke stad. Bijvoorbeeld, als de bovenste drie categorieën Budget, Motel en luxe zijn, en Budget 5 treffers heeft, Motel 6 heeft en luxe 4 is, vervolgens de buckets niet in de volgorde Motel, Budget, luxe.
  - Bijvoorbeeld: `facet=rating,sort:-value` produceert buckets voor alle mogelijke classificaties in aflopende volgorde van waarde. Als de classificatie van 1 tot en met 5, wordt de buckets bestelde 5, 4, 3, 2, 1, ongeacht hoeveel documenten overeenkomen met elke beoordeling.
- `values`(numeriek pijp gescheiden of `Edm.DateTimeOffset` -waarden die een dynamische set van waarden facet)
  - Bijvoorbeeld: `facet=baseRate,values:10|20` produceert drie buckets: een basistarief 0 tot, maar niet met inbegrip van de 10, één voor 10 tot, maar niet met inbegrip van 20, en een voor 20 of hoger.
  - Bijvoorbeeld: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produceert twee buckets: één voor hotels renovated vóór februari 2010 en voor hotels renovated februari 1st, 2010 of later.
- `interval`(interval voor geheel getal groter dan 0 voor getallen, of `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` voor datum-tijdwaarden)
  - Bijvoorbeeld: `facet=baseRate,interval:100` op basis van de bereiken basistarief van grootte 100 buckets produceert. Bijvoorbeeld als basistarieven alle tussen $60 en $600 zijn, zullen er buckets voor 0-100, 100-200, 200-300 300-400, 400-500 en 500-600.
  - Bijvoorbeeld: `facet=lastRenovationDate,interval:year` produceert een Emmertje voor elk jaar bij de hotels zijn renovated.
- `timeoffset`([+-] uu: mm, [+-] UUMM, of [+-] hh) `timeoffset` is optioneel. Kan alleen worden gecombineerd met de `interval` optie, en alleen wanneer toegepast op een veld van het type `Edm.DateTimeOffset`. De waarde geeft de tijd de UTC-afwijking verantwoorden bij de vaststelling van de grenzen van tijd.
  - Bijvoorbeeld: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` wordt de rand van de dag die begint op 01:00:00 UTC (middernacht in de tijdzone van het doel)
- **Opmerking**: `count` en `sort` in de specificatie van hetzelfde onderdeel kunnen worden gecombineerd, maar kan niet worden gecombineerd met `interval` of `values`, en `interval` en `values` kunnen niet samen worden gecombineerd.
- **Opmerking**: Interval facetten op datum tijd worden berekend op basis van UTC-tijd als `timeoffset` niet is opgegeven. Bijvoorbeeld: voor `facet=lastRenovationDate,interval:day`, de dag grens om 00:00:00 UTC begint. 

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `facets` in plaats van `facet`. Ook geeft u deze als een JSON-matrix van tekenreeksen waarin elke tekenreeks een aparte facet-expressie is.

`$filter=[string]`(optioneel) - een gestructureerde zoekformule in standaard OData-syntaxis. Zie [OData-syntaxis voor expressies](#ODataExpressionSyntax) voor meer informatie over de subset van de OData-expressie grammatica die Azure wordt ondersteund.

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `filter` in plaats van `$filter`.

`highlight=[string]`(optioneel) - een set door komma's gescheiden veldnamen voor hit worden gemarkeerd. Alleen `searchable` velden kunnen worden gebruikt voor het markeren van treffers.

`highlightPreTag=[string]`(optioneel, standaard `<em>`) - A string code voegt toe om hooglichten geraakt. Moet worden ingesteld met `highlightPostTag`.

> [AZURE.NOTE] Wanneer bellen **Zoeken** met de GET, gereserveerde tekens in de URL moet worden procent-gecodeerd (bijvoorbeeld 23% in plaats van #).

`highlightPostTag=[string]`(optioneel, standaard `</em>`)-een label tekenreeks die wordt toegevoegd om hooglichten geraakt. Moet worden ingesteld met `highlightPreTag`.

> [AZURE.NOTE] Wanneer bellen **Zoeken** met de GET, gereserveerde tekens in de URL moet worden procent-gecodeerd (bijvoorbeeld 23% in plaats van #).

`scoringProfile=[string]`(optioneel) - de naam van een score profiel te evalueren overeen met de scores voor het vergelijken van documenten om de resultaten te sorteren.

`scoringParameter=[string]`(nul of meer) - geeft aan dat de waarden voor elke parameter die is gedefinieerd in de functie van een score (bijvoorbeeld: `referencePointParameter`) met de indeling `name-value1,value2,...`.

- Als de score profiel een functie met een parameter met de naam 'mylocation definieert' de optie query-tekenreeks zou bijvoorbeeld `&scoringParameter=mylocation--122.2,44.8`. Het eerste streepje van elkaar scheidt de naam uit de lijst met waarden, terwijl het tweede streepje onderdeel van de eerste waarde (lengtegraad in dit voorbeeld is).
- Score parameters kunt u deze waarden in de lijst met enkele aanhalingstekens ontsnappen, zoals voor code die verhogen kunt komma's bevatten. Als de waarden zelf enkele aanhalingstekens bevatten, kunt u ze ontsnappen door te verdubbelen.
  - Bijvoorbeeld als er een verhoging van de parameter met de naam "mytag" tag en u wilt verhogen met de code waarden "Hello, O'Brien" en "Smith", de query string optie zou zijn `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Aanhalingstekens zijn alleen nodig voor de waarden die door komma's bevatten.

> [AZURE.NOTE] Bij het aanroepen van **Search** boeken met deze parameter met de naam `scoringParameters` in plaats van `scoringParameter`. U ook, als het een JSON-matrix met tekenreeksen waarin elke tekenreeks een afzonderlijke is `name-values` combinatie.

`minimumCoverage`(optioneel, standaard ingesteld op 100)-een getal tussen 0 en 100 die het percentage van de index die moet worden gedekt door een zoekopdracht in om de query moet worden gerapporteerd als een succes aangeeft. Standaard moet de volledige index beschikbaar zijn of `Search` HTTP-statuscode 503 wordt geretourneerd. Als u `minimumCoverage` en `Search` is uitgevoerd, worden HTTP-200 terug en bevatten een `@search.coverage` waarde in de reactie die het percentage van de index die is opgenomen in de query aangeeft.

> [AZURE.NOTE] Als deze parameter een waarde lager dan 100 is handig om ervoor te zorgen dat de beschikbaarheid van zoeken zelfs voor services met slechts één replica. Niet alle overeenkomende documenten zijn echter gegarandeerd aanwezig zijn in de lijst met zoekresultaten. Als zoeken intrekken belangrijker voor uw toepassing dan beschikbaarheid is, is het laat `minimumCoverage` op de standaardwaarde van 100 euro.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

Opmerking: voor de bewerking, de `api-version` is opgegeven als een query-parameter in de URL, ongeacht of u **Zoeken** met GET of POST belt.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `api-key`: De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Een tekenreekswaarde die uniek zijn voor uw service-URL is. **De zoekopdracht** kunt opgeven voor een admin-sleutel of de sleutel van de query voor `api-key`.

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Voor GET: geen.

Voor boeken:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Voortzetting van de gedeeltelijke zoekresultaten**

Azure Search kan soms de gevraagde resultaten retourneren in één antwoord zoeken. Dit kan gebeuren om verschillende redenen, zoals wanneer de query te veel documenten aanvragen door te geven niet `$top` of een waarde opgeeft voor `$top` dat te groot is. In dit geval Azure ook gezocht moet worden de `@odata.nextLink` aantekening in het hoofdgedeelte van de response en ook `@search.nextPageParameters` als het was een POST-aanvraag. De waarden van deze aantekeningen kunt u een andere zoekopdracht om het volgende deel van het antwoord van de zoekactie te formuleren. Dit heet een ***voortzetting*** van de oorspronkelijke zoekopdracht en de aantekeningen worden in het algemeen ***voortzetting tokens***genoemd. Zie [het voorbeeld hieronder](#SearchResponse) voor meer informatie over de syntaxis van deze aantekeningen en waar deze worden weergegeven in het hoofdgedeelte van de response. 

De redenen waarom Azure Search tokens voortzetting mogelijk retourneren zijn specifieke implementatie- en kunnen worden gewijzigd. Robuuste clients moet altijd minder documenten dan verwacht worden geretourneerd en een voortzetting token is opgenomen om door te gaan met het ophalen van documenten te verwerken. Ook ziet u de HTTP-methode moet gebruiken als de oorspronkelijke aanvraag om door te gaan. Bijvoorbeeld als u een GET-aanvraag verzonden, u stuurt verzoeken voortzetting moeten ook gebruiken GET (en ook voor boeken).

<a name="SearchResponse"></a>
**Reactie**

: 200 OK de statuscode voor een positief antwoord.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Voorbeelden:**

Meer voorbeelden kunt u vinden op de pagina van de [OData-syntaxis voor expressies voor Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) .

1)  Zoek in de Index gesorteerd op datum aflopend.


    GET /indexes/hotels/docs? zoekopdracht = * & $orderby = lastRenovationDate desc & api versie = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie = 2015-02-28-voorbeeld {'zoeken': ' * ', 'sorteren op': "lastRenovationDate-desc"}

2)  Zoek in de index en facetten voor categorieën, classificatie, tags, alsmede artikelen met baseRate in een bepaald bereik ophalen in een beperkt naar:


    GET /indexes/hotels/docs? zoekopdracht = test & facet = categorie & facet = score & facet = tags & facet = baseRate, waarden: 80 | 150 | 220 & api versie = 2015-02-28-voorbeeld

    /Indexes/hotels/docs/search boeken? api versie = 2015-02-28-voorbeeld {'zoeken': 'test', 'facetten': ["categorie", "beoordeling", "tags", "baseRate, waarden: 80 | 150 | 220"]}

3)  De queryresultaten van de vorige beperkt met behulp van een filter beperken nadat de gebruiker op score 3 categorie "Motel":


    GET /indexes/hotels/docs? zoekopdracht = test & facet = tags & facet = baseRate, waarden: 80 | 150 | 220 & $filter = & api versie eq score 3 en categorie eq "Motel" = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie = 2015-02-28-voorbeeld {'zoeken': 'test', 'facetten': ['tags', ' baseRate, waarden: 80 | 150 | 220"],"filter":"eq score 3 en categorie eq "Motel" "}

4) Instellen in een beperkt naar de bovengrens op unieke voorwaarden die in een query wordt geretourneerd. De standaardwaarde is 10, maar u kunt vergroten of verkleinen deze waarde met behulp van de `count` parameter bij het `facet` kenmerk:


    GET /indexes/hotels/docs? zoekopdracht = test & facet = plaats, aantal: 5 & api versie = 2015-02-28-voorbeeld

    /Indexes/hotels/docs/search boeken? api versie 2015-02-28-voorbeeld = {'zoeken': 'test', 'facetten': ["plaats, aantal: 5"]}

5)  Zoek in de Index op specifieke gebieden; Een taalspecifieke-veld:


    GET /indexes/hotels/docs? zoekopdracht = hôtel & searchFields = description_fr & api versie = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie 2015-02-28-voorbeeld = {"search": "hôtel", "searchFields": "description_fr"}

6) Zoek in de Index voor meerdere velden. U kunt bijvoorbeeld opslaan en doorzoekbare velden in meerdere talen, alle binnen dezelfde index opvragen.  Als de Engelse en Franse omschrijvingen in hetzelfde document naast elkaar bestaan, kunt u een of alle in de queryresultaten retourneren:


    GET /indexes/hotels/docs? zoekopdracht = hotel & searchFields = beschrijving, description_fr & api versie = 2015-02-28-voorbeeld

    /Indexes/hotels/docs/search boeken? api versie 2015-02-28-voorbeeld = {"search": "hotel", "searchFields": "beschrijving, description_fr"}

Opmerking u kunt slechts één index opvragen per keer. Maak meerdere indexen voor elke taal niet tenzij u van plan bent een voor een query.

7)  Paging - de 1e pagina met items ophalen (paginaformaat is 10):


    GET /indexes/hotels/docs? zoekopdracht = * & $skip = 0 & $top = 10 & api versie = 2015-02-28-voorbeeld

    /Indexes/hotels/docs/search boeken? api versie 2015-02-28-voorbeeld = {'zoeken': ' * ', 'overslaan': 0 'top': 10}

8)  Paging - de 2e pagina met items ophalen (paginaformaat is 10):


    GET /indexes/hotels/docs? zoekopdracht = * & $skip = 10 & $top = 10 & api versie = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie 2015-02-28-voorbeeld = {'zoeken': ' * ', 'overslaan': 10 'top': 10}

9)  Ophalen van een bepaalde set van velden:


    GET /indexes/hotels/docs? zoekopdracht = * & $select = hotelName, beschrijving & api versie = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie = 2015-02-28-voorbeeld {'zoeken': "*", "select": "Beschrijving hotelName;"}

10)  Ophalen van documenten die overeenkomen met een bepaalde expressie:


    GET /indexes/hotels/docs? $filter = (baseRate ge 60 en baseRate lt 300) of hotelName eq decoratieve blijven & api versie = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie = 2015-02-28-voorbeeld {"filter": "(baseRate ge 60 en baseRate lt 300) of hotelName eq"Decoratieve verblijf""}

11) De index en terug fragmenten met hooglichten treffers zoeken


    GET /indexes/hotels/docs? zoekopdracht = iets & Markeer = beschrijving & api versie = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie 2015-02-28-voorbeeld = {"search": "iets", "markeren": "Beschrijving"}

12) Zoek in de index en de documenten gesorteerd op locatie dichter bij het verder nodig om een verwijzing te retourneren


    GET /indexes/hotels/docs? zoekopdracht = iets & $orderby=geo.distance (locatie, geography'POINT(-122.12315 47.88121) ") & api versie = 2015-02-28-voorbeeld

    POST /indexes/hotels/docs/search? api versie 2015-02-28-voorbeeld = {'zoeken': 'iets', 'sorteren op': ' geo.distance (locatie, geography'POINT(-122.12315 47.88121)') "}

13) Zoek in de index ervan uitgaande dat er een score profiel genaamd 'geo' met twee afstand score functies, één parameter definieert genaamd "currentLocation" en één voor het definiëren van een parameter met de naam "lastLocation"


    GET /indexes/hotels/docs? zoeken iets = & scoringProfile = geo & scoringParameter = currentLocation - 122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api versie = 2015-02-28-voorbeeld

    /Indexes/hotels/docs/search boeken? api versie = 2015-02-28-voorbeeld {"search": "iets", "scoringProfile": "geo", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}

14) Zoeken naar documenten in de index met behulp van [eenvoudige query-syntaxis](https://msdn.microsoft.com/library/dn798920.aspx). Deze query resulteert in hotels waar doorzoekbare velden de termen "comfort" en "locatie" maar niet "motel bevatten":


    GET /indexes/hotels/docs? zoekopdracht = comfort + locatie-motel & searchMode = all & api versie = 2015-02-28-voorbeeld

    /Indexes/hotels/docs/search boeken? api versie = 2015-02-28-voorbeeld {'zoeken': ' comfort + locatie-motel ","searchMode":"alle"}

Let op het gebruik van `searchMode=all` hierboven. Met deze parameter overschrijft de standaard van `searchMode=any`, ervoor zorgen dat die `-motel` 'En niet' in plaats van 'Of niet' betekent. Zonder `searchMode=all`dat u 'Of niet' die groter dan beperkt de zoekresultaten en dit is erg intuïtief voor sommige gebruikers.

15) Zoeken naar documenten in de index [lucene query-syntaxis](https://msdn.microsoft.com/library/mt589323.aspx)gebruiken. Deze query resulteert in hotels waarvan het categorieveld bevat de term 'budget' en alle doorzoekbare velden met het woord "onlangs renovated". Documenten met het woord "onlangs renovated" krijgen een hogere rang van de waarde van term boost (3)

    GET-/indexes/hotels/docs?search = categorie: budget en \"onlangs renovated\"^ 3 & searchMode = all & api versie 2015-02-28-voorbeeld = & querytype = full

    POST /indexes/hotels/docs/search?api-version = 2015-02-28-voorbeeld {"search": "categorie: budget en \"onlangs renovated\"^ 3", 'queryType': 'volledig', 'searchMode': "all"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Document zoeken

Een document het **Document opzoeken** bewerking opgehaald uit Azure zoeken. Dit is handig wanneer een gebruiker op een bepaald zoekresultaat klikt en u wilt dat om specifieke details over het document op te zoeken.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Aanvraag**

HTTPS is vereist voor aanvragen. De aanvraag **Document opzoeken** kan als volgt worden samengesteld.

    GET /indexes/[index name]/docs/key?[query parameters]

U kunt ook de traditionele OData-syntaxis voor de sleutel:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

De aanvraag-URI bevat [indexnaam] en [key], welk document op te halen uit welke index op te geven. U kunt alleen één document tegelijk opvragen. **Zoeken** gebruiken om meerdere documenten in een enkele aanvraag.

**Query-Parameters**

`$select=[string]`(optioneel) - een lijst met door komma's gescheiden velden op te halen. Indien niet opgegeven of is ingesteld op `*`, alle velden gemarkeerd als worden opgehaald in het schema worden opgenomen in de projectie.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

Opmerking: voor de bewerking, de `api-version` is opgegeven als een queryparameter.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `api-key`: De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Een tekenreekswaarde die uniek zijn voor uw service-URL is. De aanvraag **Document opzoeken** kunt opgeven van een admin-sleutel of de sleutel voor query `api-key`.

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Geen.

**Reactie**

: 200 OK de statuscode voor een positief antwoord.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Voorbeeld**

Het document dat de sleutel '2' lookup

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Het document dat is de sleutel met behulp van OData-syntaxis 3 opzoeken:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Aantal documenten

Het **Aantal documenten** bewerking haalt een telling van het aantal documenten in een zoekindex. De `$count` syntaxis is onderdeel van de OData-protocol.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Aanvraag**

HTTPS is vereist voor aanvragen. Het **Aantal documenten** verzoek kan worden samengesteld met de methode GET.

[Naam van de index] in de aanvraag-URI wordt de service voor een telling van alle items in de collectie van documenten van de opgegeven index.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

**Aanvraagheaders**

De volgende lijst bevat de vereiste en optionele aanvraagheaders.

- `Accept`: Deze waarde moet worden ingesteld op `text/plain`.
- `api-key`: De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Een tekenreekswaarde die uniek zijn voor uw service-URL is. Het **Aantal documenten** aanvraag kunt opgeven voor een admin-sleutel of de sleutel van de query voor `api-key`.

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Geen.

**Reactie**

: 200 OK de statuscode voor een positief antwoord.

Het hoofdgedeelte van de response bevat de waarde van count als een geheel getal in tekst zonder opmaak ingedeeld.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Suggesties

De bewerking **suggesties** opgehaald suggesties op basis van gedeeltelijke zoeken. Dit wordt meestal gebruikt in de zoekvakken naar type-ahead suggesties bieden gebruikers zoektermen invoeren.

Voorstel verzoeken gericht op voorstellen doeldocumenten, zodat de voorgestelde tekst kan worden herhaald als meerdere kandidaat documenten invoer dezelfde zoekopdracht overeenkomen. U kunt `$select` voor het ophalen van andere documentvelden (met inbegrip van de documentsleutel) zodat u kunt zien welk document is de bron voor elke suggestie.

Een bewerking **suggesties** wordt verleend als een aanvraag GET of POST.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Wanneer gebruikt u in plaats van GET boeken**

Als u HTTP GET **suggesties** API aanroept, moet u rekening houden dat de lengte van de aanvraag-URL mag niet meer dan 8 KB. Dit is meestal voldoende voor de meeste toepassingen. Sommige toepassingen produceren echter zeer grote query's specifiek OData filterexpressies. Voor deze toepassingen is met behulp van HTTP POST een goede keuze omdat daardoor grotere filters dan ophalen. Met de POST is het aantal componenten in een filter de beperkende factor, niet de grootte van de raw-filterreeks omdat de limiet voor de aanvraag voor een bericht 16 MB is.

> [AZURE.NOTE] Hoewel de limiet voor de POST-aanvraag erg groot is, kunnen geen filterexpressies willekeurig complex zijn. Zie [OData-syntaxis voor expressies](https://msdn.microsoft.com/library/dn798921.aspx) voor meer informatie over beperkingen voor filter complexiteit.

**Aanvraag**

HTTPS is vereist voor aanvragen. **Suggesties** -verzoek kan worden samengesteld met behulp van de methoden GET of POST.

De aanvraag-URI bevat de naam van de index aan de query. Parameters, zoals de gedeeltelijk ingevoerde zoekterm zijn opgegeven in de query-tekenreeks voor GET-aanvragen en in de hoofdtekst van POST vraagt.

Aanbevolen bij het maken van GET-aanvragen, moet u specifieke query-parameters [URL coderen](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) bij het rechtstreeks aanroepen van de REST API. Dit omvat voor **suggesties** -bewerkingen:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL-codering wordt alleen aanbevolen voor de bovenstaande query-parameters. Als u per ongeluk URL coderen de hele query-tekenreeks (alles na het?), aanvragen, worden verbroken.

Ook URL-codering is alleen nodig wanneer het direct ophalen met REST-API wordt aangeroepen. Geen URL-codering is noodzakelijk bij het aanroepen van **suggesties** via POST of bij het gebruik van de [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx)die URL-codering voor u verwerkt.

**Query-Parameters**

**Suggesties** accepteert van verschillende parameters die querycriteria geven en ook zoekgedrag opgeven. U bevatten dat deze parameters in de URL-queryreeks bij het aanroepen van **suggesties** via Ontvang en als JSON-eigenschappen in het hoofdgedeelte van de aanvraag bij het aanroepen van **suggesties** via POST. De syntaxis voor sommige parameters verschilt enigszins tussen GET en POST. Deze verschillen worden aangegeven als onder die van toepassing zijn:

`search=[string]`-de zoektekst met behulp van query's worden voorgesteld. Moet minimaal 1 teken en maximaal 100 tekens bevatten.

`highlightPreTag=[string]`(optioneel) - een tekenreeks van label die voegt toe treffers zoeken. Moet worden ingesteld met `highlightPostTag`.

> [AZURE.NOTE] Wanneer bellen **suggesties** met GET, gereserveerde tekens in de URL moet worden procent-gecodeerd (bijvoorbeeld 23% in plaats van #).

`highlightPostTag=[string]`(optioneel) - string code die wordt toegevoegd om treffers zoeken. Moet worden ingesteld met `highlightPreTag`.

> [AZURE.NOTE] Wanneer bellen **suggesties** met GET, gereserveerde tekens in de URL moet worden procent-gecodeerd (bijvoorbeeld 23% in plaats van #).

`suggesterName=[string]`-de naam van het suggester als omschreven in de `suggesters` -collectie die deel uitmaakt van de definitie van de index. A `suggester` bepaalt u welke velden worden gescand op de voorgestelde querytermen. Zie [Suggesters](#Suggesters) voor meer informatie.

`fuzzy=[boolean]`(optioneel, standaard = false)-als de waarde op waar deze API zal zoeken naar suggesties zelfs als er in de tekst een teken ontbreekt of vervangen. Het wordt geleverd op een prestaties omdat onduidelijk suggestie zoekacties trager en meer bronnen verbruiken terwijl dit een betere ervaring in bepaalde scenario's biedt.

`searchFields=[string]`(optioneel) - de lijst van door komma's gescheiden velden om te zoeken naar de opgegeven tekst. Doelvelden moeten zijn ingeschakeld voor suggesties.

`$top=#`(optioneel, standaard = 5)-het aantal suggesties om op te halen. Moet een getal tussen 1 en 100.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** voor boeken met deze parameter met de naam `top` in plaats van `$top`.

`$filter=[string]`(optioneel): een expressie waarmee de documenten beschouwd als voor suggesties.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** voor boeken met deze parameter met de naam `filter` in plaats van `$filter`.

`$orderby=[string]`(optioneel) - een lijst met expressies voor het sorteren van de resultaten door komma's gescheiden. Elke expressie kan bestaan uit een veldnaam of een aanroep van de `geo.distance()` functie. Elke expressie kan worden gevolgd door `asc` aangegeven oplopend, en `desc` om aan te geven aflopend. De standaardinstelling is oplopend. Er is een limiet van 32 clausules voor `$orderby`.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** voor boeken met deze parameter met de naam `orderby` in plaats van `$orderby`.

`$select=[string]`(optioneel) - een lijst met door komma's gescheiden velden op te halen. Als u niets opgeeft, wordt alleen de sleutel en voorstel tekst geretourneerd. U kunt alle velden aanvragen door deze parameter in te stellen op `*`.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** voor boeken met deze parameter met de naam `select` in plaats van `$select`.

`minimumCoverage`(optioneel, standaard ingesteld op 80)-een getal tussen 0 en 100 die het percentage van de index die moet worden beschreven met een query met suggesties voor de query moet worden gerapporteerd als een succes aangeeft. Standaard moet ten minste 80% van de index beschikbaar zijn of `Suggest` HTTP-statuscode 503 wordt geretourneerd. Als u `minimumCoverage` en `Suggest` is uitgevoerd, worden HTTP-200 terug en bevatten een `@search.coverage` waarde in de reactie die het percentage van de index die is opgenomen in de query aangeeft.

> [AZURE.NOTE] Als deze parameter een waarde lager dan 100 is handig om ervoor te zorgen dat de beschikbaarheid van zoeken zelfs voor services met slechts één replica. Echter zijn niet alle overeenkomende suggesties gegarandeerd aanwezig zijn in de resultaten. Als intrekken belangrijker voor uw toepassing dan beschikbaarheid is, is het beter niet verlagen `minimumCoverage` onder de standaardwaarde is 80.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) Zie voor details en alternatieve versies.

Opmerking: voor de bewerking, de `api-version` is opgegeven als een query-parameter in de URL, ongeacht of u **suggesties** met GET of POST belt.

**Aanvraagheaders**

De volgende lijst worden de vereiste en optionele aanvraagheaders

- `api-key`: De `api-key` wordt gebruikt voor de verificatie van de aanvraag voor de zoekservice. Een tekenreekswaarde die uniek zijn voor uw service-URL is. De aanvraag **suggesties** kunt opgeven voor een admin-sleutel of de sleutel van de query als de `api-key`.

Ook moet u de naam van de aanvraag-URL maken. U krijgt de servicenaam van de en `api-key` uit het servicedashboard in de Portal Azure. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor paginanavigatie helpen.

**Hoofdgedeelte van de aanvraag**

Voor GET: geen.

Voor boeken:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Reactie**

: 200 OK de statuscode voor een positief antwoord.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Als de optie projectie wordt gebruikt voor velden die zijn opgenomen in elk element van de matrix ophalen:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Voorbeeld**

5 suggesties waar de invoer met gedeeltelijke zoekopdracht is 'lux' ophalen

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
