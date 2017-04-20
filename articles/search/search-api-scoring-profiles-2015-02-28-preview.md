<properties
    pageTitle="Score (Azure Search REST API versie 2015-02-28-Preview) profielen | Microsoft Azure | Voorbeeld van Azure Search API"
    description="Azure Search is een gehoste cloud-zoekservice die afstemmen van rangorde resultaten op basis van door de gebruiker gedefinieerde score profielen ondersteunt."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Score van profielen (Azure Search REST API versie 2015-02-28-voorbeeld)

> [AZURE.NOTE] In dit artikel wordt beschreven score profielen in de [2015-02-28-voorbeeld](search-api-2015-02-28-preview.md). Er is momenteel geen verschil tussen de `2015-02-28` die op [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) en de `2015-02-28-Preview` versie die hier worden beschreven, maar bieden we dit document toch te voorzien in de gehele API documentdekking.

## <a name="overview"></a>Overzicht

Score heeft betrekking op de berekening van een zoekopdracht score voor elk item in de zoekresultaten geretourneerd. De score is een indicatie van de relevantie van een item in het kader van de huidige zoekbewerking. Hoe hoger de score, de betreffende het artikel. In de zoekresultaten zijn artikelen rang gerangschikt van hoog naar laag, op basis van de zoekopdracht score berekend voor elk artikel.

Azure Search scoren om te komen tot een eerste score standaard gebruikt, maar u kunt de berekening door middel van een score profiel aanpassen. Score profielen geven u meer controle over de rangschikking van items in de zoekresultaten. U wilt bijvoorbeeld items op basis van hun potentiële omzet te verhogen, nieuwe objecten promoten of misschien stimuleren artikelen die in voorraad te lang zijn.

Een score profiel maakt deel uit van de definitie van de index, bestaat uit velden, functies en parameters.

Om u een idee geven van hoe een score profiel eruit ziet, ziet in het volgende voorbeeld u een eenvoudige profiel met de naam 'geo'. Dit verhoogt de items met de zoekterm in de `hotelName` veld. Verder wordt de `distance` functie voorkeur krijgen voor items die zich binnen tien kilometer van de huidige locatie. Als iemand op de term 'inn zoekt' en 'inn' deel uitmaakt van de naam van het hotel gebeurt, weergegeven documenten met hotels met 'inn' die hoger in de zoekresultaten.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Deze score om profiel te gebruiken, wordt de query geformuleerd om het profiel op de query-tekenreeks. In de query hieronder ziet u de queryparameter `scoringProfile=geo` in de aanvraag.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Deze query wordt gezocht op de term 'inn' en geeft op de huidige locatie. Opmerking dat deze query andere parameters, zoals bevat `scoringParameter`. Query-parameters worden beschreven in [Documenten zoeken (Azure Search API)](search-api-2015-02-28-preview.md#SearchDocs).

Klik op [voorbeeld](#example) als u wilt een meer gedetailleerd voorbeeld van een score profiel bekijken.

## <a name="what-is-default-scoring"></a>Wat is standaard scoren?

Score berekent een zoekopdracht score voor elk item in een absolute gesorteerde resultatenset. Elk item in een resultaatset zoeken is toegewezen een score zoeken en vervolgens hoog naar laag gerangschikt. Items met de hogere scores worden geretourneerd aan de toepassing. Standaard de top 50 worden geretourneerd, maar u kunt de `$top` parameter als resultaat een kleiner of groter aantal items (maximaal 1000 in één antwoord).

Standaard wordt een zoekopdracht score berekend op basis van statistische eigenschappen van de gegevens en de query. Azure Search zoekt documenten met de zoektermen die in de query-tekenreeks (sommige of alle, afhankelijk van de `searchMode`), voorkeur van documenten die veel exemplaren van de zoekterm bevatten. De score zoeken gaat omhoog zelfs hoger als de term zeldzaam in het corpus van gegevens, maar gemeenschappelijk in het document is. De basis voor deze aanpak van computergebruik relevantie TF IDF wordt genoemd of (frequentie term frequentie inverse document).

Ervan uitgaande dat er geen aangepaste sorteervolgorde, resultaten worden vervolgens informatie gerangschikt volgens score zoeken voordat ze worden teruggestuurd naar de aanroepende toepassing. Als `$top` niet is opgegeven, 50 items met de hoogste zoeken score worden geretourneerd.

Score van zoekwaarden kunnen worden herhaald in een resultaatset. U hebt bijvoorbeeld 10 items met een score van 1.2, 20 items met een score van 1.0 en 20 items met een score van 0,5. Wanneer meerdere hits dezelfde zoekopdracht score hebben, de volgorde van dezelfde items gescoord is niet gedefinieerd en is niet stabiel. Opnieuw de query en u ziet mogelijk uitvoeren items shift positie. Twee artikelen met een identieke score gegeven, is niet zeker welke als eerste weergegeven.

## <a name="when-to-use-custom-scoring"></a>Wanneer gebruikt u aangepaste scoren

Maak score profielen voor een of meer als standaard rangorde gedrag niet ver genoeg in voldoen aan uw zakelijke doelstellingen. Bijvoorbeeld, misschien dat zoekrelevantie toegevoegde items voorrang moet geven. U hebt ook een veld met de winstmarge, of een ander veld potentiële omzet die aangeeft. Verhoging van de treffers die voordelen voor uw bedrijf is een belangrijke factor bij het bepalen van de score profielen gebruiken.

Bestellen op basis van relevantie wordt ook ingevoerd door profielen te scoren. U kunt pagina's met zoekresultaten u hebt gebruikt in het verleden die kunt u sorteren op prijs, datum, waardering of relevantie. Score profielen station in Azure zoeken de optie 'relevantie'. De definitie van de relevantie wordt bepaald door u echter afhankelijk van de zakelijke doelstellingen en het type zoekfunctie die u wilt leveren.

<a name="example"></a>
## <a name="example"></a>Voorbeeld

Zoals opgemerkt, wordt aangepaste scoren geïmplementeerd via het scoren van de profielen die zijn gedefinieerd in het schema van een index.

In dit voorbeeld wordt het schema van een index met twee score-profielen (`boostGenre`, `newAndHighlyRated`). Een query op deze index met een profiel, zoals een queryparameter het profiel wordt gebruikt voor het verkrijgen van de resultaatset.

[Dit voorbeeld](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Workflow

Voor het implementeren van aangepaste score gedrag, moet u een score profiel toevoegen met het schema dat u de index definieert. U kunt meerdere profielen van score in een index hebben, maar u kunt slechts één profiel tegelijk in een bepaalde query opgeven.

Beginnen met de [sjabloon](#bkmk_template) die in dit onderwerp.

Geef een naam op. Score profielen zijn optioneel, maar als u een toevoegen, de naam is vereist. Volg de naamgeving van velden (begint met een letter, vermijdt u speciale tekens en gereserveerde woorden). Zie [De regels voor naamgeving](http://msdn.microsoft.com/library/azure/dn857353.aspx) voor meer informatie.

De hoofdtekst van het profiel van de score is opgebouwd uit gewogen velden en functies.

### <a name="weights"></a>Gewichten ###

De `weights` eigenschap van een score profiel geeft u de naam / waarde-paren die een relatieve gewicht aan een veld toewijzen. In het [voorbeeld](#example)de velden albumTitle, genre en artistName worden gestimuleerd 1.5, 5 en 2, respectievelijk. Waarom wordt genre versterkt nog veel hoger dan de andere? Als de zoekopdracht wordt uitgevoerd via gegevens die enigszins homogeen is (zoals het geval met 'genre' in de `musicstoreindex`), moet u een groter verschil in het relatieve gewicht. Bijvoorbeeld, in de `musicstoreindex`, 'rock' weergegeven als beide een genre en in identieke wijze geformuleerd genre beschrijvingen. Als u wilt dat genre naar genre omschrijving zwaarder wegen dan moet het genre veld een veel hogere relatieve gewicht.

### <a name="functions"></a>Functies ###

Functies worden gebruikt wanneer aanvullende berekeningen nodig om specifieke contexten zijn. Geldige functietypen zijn `freshness`, `magnitude`, `distance` en `tag`. Elke functie heeft unieke parameters.

  - `freshness`moet worden gebruikt als u wilt verhogen door het nieuwe of oude een item is. Deze functie kan alleen worden gebruikt met datum/tijd-velden (`Edm.DataTimeOffset`). Opmerking de `boostingDuration` kenmerk wordt alleen gebruikt met de functie versheid.
  - `magnitude`moet worden gebruikt als u verhogen op basis wilt van hoe hoog of laag een numerieke waarde is. Scenario's die voor deze functie aanroepen zijn waarbij de winstmarge, de hoogste prijs, de laagste prijs of aantal downloads. U kunt het bereik, hoog naar laag, omkeren als u wilt dat het omgekeerde patroon (bijvoorbeeld om de lagere prijs boost items meer dan hoger geprijsde artikelen). Gegeven een bereik van de prijzen van $100 tot $1, stelt u `boostingRangeStart` 100 en `boostingRangeEnd` bij 1 vergroten de lager geprijsde artikelen. Deze functie kan alleen worden gebruikt met dubbel en integer-velden.
  - `distance`moet worden gebruikt als u wilt vergroten door nabijheid of geografische locatie. Deze functie kan alleen worden gebruikt met `Edm.GeographyPoint` velden.
  - `tag`moet worden gebruikt als u wilt vergroten door codes gemeenschappelijk tussen documenten en zoekopdrachten. Deze functie kan alleen worden gebruikt met `Edm.String` en `Collection(Edm.String)` velden.
  
#### <a name="rules-for-using-functions"></a>Regels voor het gebruik van functies ####

  - Functietype (versheid, grootte, afstand, tag) moet in kleine letters.
  - Functies kunnen niet null of lege waarden bevatten. Met name als u fieldname, moet u op een waarde ingesteld.
  - Functies kunnen alleen worden toegepast op velden Filterbaar. Zie [Index maken](search-api-2015-02-28-preview.md#CreateIndex) voor meer informatie over velden Filterbaar.
  - Functies kunnen alleen worden toegepast op velden die zijn gedefinieerd in de fields-collectie van een index.

Nadat de index is gedefinieerd, moet u de index samen door het schema van de index, gevolgd door documenten te uploaden. Zie [Index maken](search-api-2015-02-28-preview.md#CreateIndex) en [toevoegen of bijwerken, documenten](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) voor meer informatie over deze bewerkingen. Als de index is ontwikkeld, moet er een functionele score profiel die geschikt is voor uw zoekgegevens.

<a name="bkmk_template"></a>
## <a name="template"></a>Sjabloon
In deze sectie ziet u de syntaxis en de sjabloon voor het scoren van de profielen. Raadpleeg [de verwijzing naar Index](#bkmk_indexref) in het volgende gedeelte voor een beschrijving van de kenmerken.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Score van profiel-eigenschappen-naslaginformatie

**Opmerking** Een score-functie kan alleen worden toegepast op velden die Filterbaar zijn.

| Eigenschap | Beschrijving |
|----------|-------------|
| `name`   | Vereist. Dit is de naam van het profiel van de score. Het volgt de dezelfde naamgevingsconventies van een veld. Het moet beginnen met een letter, mogen geen punten, dubbele punten bevatten of @ symbolen en mag niet beginnen met het woord 'azureSearch' (hoofdlettergevoelig). |
| `text` | De eigenschap gewicht bevat. |
| `weights` | Dit is optioneel. Een naam-waardepaar waarmee een veldnaam en het relatieve gewicht. Relatieve gewicht moet een positief geheel getal of getal met drijvende komma. U kunt de naam van het veld zonder een overeenkomstige gewicht opgeven. Gewichten worden gebruikt om aan te geven van het belang van één veld ten opzichte van andere. |
| `functions` | Dit is optioneel. Opmerking een score-functie kan alleen worden toegepast op velden die Filterbaar zijn. |
| `type` | Vereist voor het scoren van functies. Geeft het type van de functie te gebruiken. Geldige waarden zijn `magnitude`, `freshness`, `distance` en `tag`. U kunt meer dan één functie opnemen in elk profiel score. De naam van de functie moet in kleine letters. |
| `boost` | Vereist voor het scoren van functies. Een positief getal voor ruwe score als de vermenigvuldiger wordt gebruikt. Het kan niet gelijk is aan 1. |
| `fieldName` | Vereist voor het scoren van functies. Een score-functie kan alleen worden toegepast op velden die deel uitmaken van de collectie van het veld van de index en die Filterbaar zijn. Elk functietype bevat bovendien extra beperkingen (versheid wordt gebruikt met datum/tijd-velden, omvang met gehele getallen of dubbele velden, afstand met locatievelden en een label met een tekenreeks of een reeks verzameling velden). U kunt slechts één veld per functiedefinitie opgeven. Bijvoorbeeld om de omvang tweemaal in hetzelfde profiel wilt gebruiken, moet u twee definities hoogte, één voor elk veld opnemen. |
| `interpolation` | Vereist voor het scoren van functies. Definieert de richting waarvoor de score waarbij de verhogingen, vanaf het begin van het bereik naar het einde van het bereik. Geldige waarden zijn `linear` (standaard), `constant`, `quadratic`, en `logarithmic`. Zie [interpolations](#bkmk_interpolation) voor meer informatie. |
| `magnitude` | De omvang scoren functie wordt gebruikt om de rangorde op basis van het bereik van waarden voor een numeriek veld wijzigen. Enkele van de meest voorkomende voorbeelden van het gebruik van deze zijn:<ul><li>Sterclassificatie: het scoren op basis van de waarde in het veld "Ster classificatie" veranderen. Wanneer twee artikelen betrokken zijn, wordt eerst het item met de hogere classificatie weergegeven.</li><li>Marge: Wanneer twee documenten relevant zijn, een detailhandelaar kunt stimuleren documenten met eerst hogere marges.</li><li>Klik op telt: voor toepassingen die volgen, klikt u op acties, producten of pagina's, kunt u gebruiken om items boost die doorgaans het meeste verkeer krijgen.</li><li>Telt het aantal downloaden: toepassingen bijhouden downloads, de omvang-functie kunt u betere items die de meeste downloads zijn.</li></ul> |
| `magnitude:boostingRangeStart` | Hiermee stelt u de beginwaarde van het bereik via welke omvang is gescoord. De waarde moet een geheel getal of getal met drijvende komma. Dit kunnen voor sterwaarderingen van 1 tot en met 4, 1. Dit zou voor meer dan 50%, 50. |
| `magnitude:boostingRangeEnd` | Hiermee stelt u de waarde van het bereik via welke omvang is gescoord. De waarde moet een geheel getal of getal met drijvende komma. Dit kunnen voor sterwaarderingen van 1 tot en met 4, 4. |
| `magnitude:constantBoostBeyondRange` | Geldige waarden zijn true of false (standaard). Als de waarde true wordt de volledige boost blijven toepassen op documenten die een waarde voor het veld met het doel die hoger is dan het bovenste einde van het bereik. Als de waarde false is, wordt de boost van deze functie niet toegepast op documenten met een waarde voor het veld met het doel die buiten het bereik valt. |
| `freshness` | De functie scoren versheid wordt gebruikt om scores voor artikelen op basis van waarden in velden van de DateTimeOffset rangschikking wijzigen. Bijvoorbeeld een item met een recentere datum kan worden krijgen een hogere rang dan oudere items. (Houd er rekening mee dat het is ook mogelijk om absolute items zoals agenda-items met toekomstige datums die dichter bij de huidige items kunnen worden krijgen een hogere rang dan de items verder in de toekomst.) In de huidige versie van de service wordt een van de uiteinden van het bereik vastgesteld aan de huidige tijd. De andere kant is een tijd in het verleden op basis van de `boostingDuration`. Vergroten gebruiken een aantal keren in de toekomst een negatieve `boostingDuration`. De snelheid waarmee de versterking van het wijzigen van een maximale en minimale bereik wordt bepaald door de interpolatie toegepast op het scorepatroon profiel (Zie de onderstaande afbeelding). Om terug te keren de prestatieverbetering factor die wordt toegepast, kiest u een boost factor van minder dan 1. |
| `freshness:boostingDuration` | Stelt een verloopperiode na verhoogd voor een bepaald document wordt gestopt. Zie [Set boostingDuration] [bkmk_boostdur #] in het volgende gedeelte voor de syntaxis en voorbeelden. |
| `distance` | Sluit u de score van documenten op basis van hoe de score-functie wordt gebruikt om de invloed op afstand of tot nu toe zijn gebaseerd op een geografische verwijzingslocatie. De locatie van de verwijzing is opgegeven als onderdeel van de query een parameter (met de `scoringParameter` queryparameter) als een ion, lat-argument. |
| `distance:referencePointParameter` | Een parameter in de query's te gebruiken als locatie voor de verwijzing worden doorgegeven. scoringParameter is een queryparameter. Zie [Documenten zoeken](search-api-2015-02-28-preview.md#SearchDocs) voor een beschrijving van de query-parameters. |
| `distance:boostingDistance` | Een getal dat de afstand in kilometers van de referentie-locatie waar de prestatieverbetering bereik eindigt aangeeft. |
| `tag` | De functie scoren tag wordt gebruikt om de invloed op de score van documenten op basis van labels in documenten en zoekopdrachten. Documenten met tags, net als bij de query zal worden versterkt. De labels voor de zoekopdracht wordt geleverd als een parameter score bij elke zoekaanvraag (met behulp van de `scoringParameter` parameter query). |
| `tag:tagsParameter` | Een parameter in de query op te geven voor een bepaalde aanvraag worden doorgegeven. `scoringParameter`is een queryparameter. Zie [Documenten zoeken](search-api-2015-02-28-preview.md#SearchDocs) voor een beschrijving van de query-parameters. |
| `functionAggregation` | Dit is optioneel. Geldt alleen wanneer functies is opgegeven. Geldige waarden zijn: `sum` (standaard), `average`, `minimum`, `maximum`, en `firstMatching`. Een score van zoeken is een single-waarde die is berekend uit meerdere variabelen, met inbegrip van meerdere functies. Deze kenmerken geeft aan hoe de verhoogt van alle functies worden gecombineerd tot een enkele statistische boost die vervolgens wordt toegepast op het basisdocument score. De basisscore is gebaseerd op de tf idf-waarde berekend uit het document en de query. |
| `defaultScoringProfile` | Bij het uitvoeren van een zoekopdracht als score profiel wordt standaard scoren gebruikt (tf-idf alleen). Profielnaam scoring standaard worden ingesteld, Azure zoeken dat profiel gebruiken wanneer u geen specifiek profiel wordt gegeven in de zoekaanvraag veroorzaakt. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Set interpolations

Interpolations kunt u de richtingscoëfficiënt definiëren waarvoor de score waarbij de verhogingen, vanaf het begin van het bereik naar het einde van het bereik. De volgende interpolations kunnen worden gebruikt:

  - `Linear`: Voor de items die zich binnen het bereik max en min, worden de verbetering van het item in een voortdurend afnemende bedrag uitgevoerd. Lineaire is de standaard interpolatie score profiel.
  - `Constant`: Voor items die zich binnen de begin- en einddatum bereik, worden een constante verbetering van de resultaten van de rang toegepast.
  - `Quadratic`: ABC neemt in eerste instantie kleinere tempo In vergelijking met een lineaire interpolatie met een voortdurend afnemende boost, en als het nadert het einde van het bereik, dit neemt af met een veel hogere interval. Deze optie interpolatie is niet toegestaan in de tag functies scoren.
  - `Logarithmic`: Logaritmische neemt in eerste instantie in hoger tempo In vergelijking met een lineaire interpolatie met een voortdurend afnemende boost, en als het nadert het einde van het bereik, dit neemt af met een veel kleiner interval. Deze optie interpolatie is niet toegestaan in de tag functies scoren.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>Set boostingDuration

`boostingDuration`is een kenmerk van de functie van versheid. Hierin kunt u een verloopdatum instellen voor een bepaald document verhoogd wordt gestopt na de periode. Om te stimuleren een productreeks of merk gedurende een 10-dagen periode, zou u bijvoorbeeld de 10 dagen als 'P10D' voor deze documenten opgeven. Of vergroten komende evenementen in de volgende week "-P7D".

`boostingDuration`Als een waarde in XSD-'dayTimeDuration' (een beperkte subset van de waarde van de duur van een ISO 8601) moet worden geformatteerd. Het patroon voor dit is: `[-]P[nD][T[nH][nM][nS]]`.

De volgende tabel bevat enkele voorbeelden.

| Duur | boostingDuration |
|----------|------------------|
| 1 dag | "P1D" |
| 2 dagen en 12 uur | "P2DT12H" |
| 15 minuten | "PT15M" |
| 30 dagen, 5 uur, 10 minuten en 6.334 seconden | "P30DT5H10M6.334S" |

Zie voor meer voorbeelden van [XML-Schema: gegevenstypen (website W3.org)](http://www.w3.org/TR/xmlschema11-2/).

**Zie ook**
[Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) op MSDN <br/>
[Index (Azure Search API) maken](http://msdn.microsoft.com/library/azure/dn798941.aspx) op MSDN<br/>
[Een zoekindex score profiel toevoegen](http://msdn.microsoft.com/library/azure/dn798928.aspx) op MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
