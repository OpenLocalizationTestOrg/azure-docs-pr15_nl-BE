<properties
    pageTitle="DocumentDB verbinden met Azure zoeken met behulp van indexeerfuncties | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u met Azure Search indexer met DocumentDB als gegevensbron."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>DocumentDB verbinden met Azure zoeken op basis van indexeerfuncties

Als u de implementatie van zoeken fantastische ervaringen over de DocumentDB gegevens, gebruikt u de Azure Search indexer voor DocumentDB! In dit artikel leert we u hoe Azure DocumentDB integreren met Azure zoeken zonder code te beheren infrastructuur voor indexering te schrijven!

Dit als u wilt instellen, moet [een Azure Search account setup](../search/search-create-service-portal.md) (u hoeft niet te upgraden naar de standard search) en roep vervolgens de [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) om een DocumentDB- **gegevensbron** en een **indexering** voor de gegevensbron te maken.

In bestelling verzenden aanvragen voor de interactie met de REST API's, kunt u [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)of een hulpprogramma van uw voorkeur.


##<a id="Concepts"></a>Azure Search indexer concepten

Azure zoeken ondersteunt het maken en beheren van gegevens-bronnen (inclusief DocumentDB) en indexeerfuncties die ten opzichte van die gegevensbronnen werken.

Een **gegevensbron** geeft aan welke gegevens moeten worden geïndexeerd, referenties voor toegang tot de gegevens, en het beleid om Azure te zoeken voor het efficiënt identificeren wijzigingen in de gegevens (zoals gewijzigd of documenten binnen de collectie wordt verwijderd). De gegevensbron is gedefinieerd als een onafhankelijke bron, zodat deze kan worden gebruikt door meerdere indexeerfuncties.

**Indexeerfunctie** wordt beschreven hoe de gegevens uit de gegevensbron loopt in de zoekindex van een doel. Plan voor het maken van een indexering voor elke combinatie doel index en bron. U kunt beschikken over meerdere indexeerfuncties in dezelfde index schrijven, kan alleen een indexeerfunctie schrijven in een enkele index. Indexeerfunctie wordt gebruikt om:

- Uitvoeren van een momentopname van de gegevens voor het vullen van een index.
- Een index met de wijzigingen in de gegevensbron van een planning worden gesynchroniseerd. De planning is onderdeel van de definitie van de indexeerfunctie.
- Aanroepen van updates op verzoek van een index, indien nodig.

##<a id="CreateDataSource"></a>Stap 1: Een gegevensbron maken

Afgifte van een HTTP POST-verzoek om een nieuwe gegevensbron maken in uw Azure Search service, met inbegrip van de volgende request-headers.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

De `api-version` is vereist. Geldige waarden zijn `2015-02-28` of een latere versie. Ga naar de [API-versies in Azure zoeken](../search/search-api-versions.md) als u wilt zien van alle ondersteunde versies van de zoek-API.

Het hoofdgedeelte van de aanvraag bevat de definitie van de gegevensbron, die moet de volgende velden bevatten:

- **naam**: Kies een naam voor de database DocumentDB.

- **type**: Gebruik `documentdb`.

- **referenties**:

    - **connectionString**: vereist. Geef de verbindingsgegevens aan uw database Azure DocumentDB in de volgende notatie:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **container**:

    - **naam**: vereist. Geef de id van de DocumentDB-collectie worden geïndexeerd.

    - **query**: optioneel. U kunt een query voor het afvlakken van een willekeurige JSON-document in een platte schema die Azure Search kunnen indexeren.

- **dataChangeDetectionPolicy**: optioneel. Zie [gegevens wijzigen detectie beleid](#DataChangeDetectionPolicy) hieronder.

- **dataDeletionDetectionPolicy**: optioneel. Zie [beleid voor verwijdering detectie gegevens](#DataDeletionDetectionPolicy) hieronder.

Zie hieronder voor een [voorbeeld van de hoofdgedeelte van de aanvraag](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Gewijzigde documenten vastleggen

Het doel van een beleid voor detectie van gegevens wijzigen is efficiënt gewijzigde gegevens om items te identificeren. Momenteel de enige ondersteunde beleid is de `High Water Mark` beleid met behulp van de `_ts` laatste wijziging tijdstempel eigenschap verstrekt door DocumentDB - die is opgegeven als volgt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

U moet ook toevoegen `_ts` in de projectie en `WHERE` -component voor de query. Bijvoorbeeld:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Verwijderde documenten vastleggen

Wanneer de rijen uit de tabel worden verwijderd, moet u deze rijen verwijderen uit de search-index. Het doel van een beleid voor detectie van gegevens verwijderen is efficiënt verwijderde gegevens om items te identificeren. Op dit moment de enige ondersteunde beleid is de `Soft Delete` beleid (verwijdering is gemarkeerd met een vlag zonnestelsels), die wordt als volgt opgegeven:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] U moet de eigenschap softDeleteColumnName in uw SELECT-component opnemen als u een aangepaste projectie.

###<a id="CreateDataSourceExample"></a>Voorbeeld van de instantie aanvragen

In het volgende voorbeeld wordt een gegevensbron gemaakt met een aangepaste query- en tips:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Reactie

U ontvangt een antwoord HTTP-201 gemaakt als de gegevensbron is gemaakt.

##<a id="CreateIndex"></a>Stap 2: Een index maken

Een doel Azure Search-index maken als u geen sjabloon hebt. U kunt dit doen vanuit de [Azure Portal UI](../search/search-create-index-portal.md) of met behulp van de [API voor Index maken](https://msdn.microsoft.com/library/azure/dn798941.aspx).

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Zorg ervoor dat het schema van de index doel compatibel met het schema van de bron JSON-documenten of de uitvoer van de projectie van de aangepaste query is.

>[AZURE.NOTE] Voor de gepartitioneerde collecties, de standaardsleutel document is DocumentDB van `_rid` eigenschap, die wordt gewijzigd in `rid` in Azure zoeken. Ook DocumentDB van `_rid` waarden bevatten tekens worden gebruikt in Azure Search toetsen; dus de `_rid` waarden Base64-gecodeerd zijn.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Figuur A: toewijzing tussen de gegevenstypen JSON en Azure Search-gegevenstypen

| JSON-GEGEVENSTYPE|   INDEX DOEL COMPATIBELE TYPEN|
|---|---|
|BOOL|Edm.Boolean, Edm.String|
|Getallen die eruit als gehele getallen zien|Edm.Int32, Edm.Int64, Edm.String|
|Getallen die zijn opgemaakt als een zwevende punten|Edm.Double, Edm.String|
|Tekenreeks|Edm.String|
|Matrices van primitieve typen zoals "a", "b", "c" |Collection(EDM.String)|
|Tekenreeksen die er als datums uitzien| Edm.DateTimeOffset, Edm.String|
|GeoJSON objecten bijvoorbeeld {"type": "Punt", "coördinaten": [lange, lat]} | Edm.GeographyPoint |
|Andere objecten JSON|N.V.T.|

###<a id="CreateIndexExample"></a>Voorbeeld van de instantie aanvragen

In het volgende voorbeeld wordt een index gemaakt met een veld-id en omschrijving:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Reactie

U ontvangt een antwoord HTTP-201 gemaakt als de index is gemaakt.

##<a id="CreateIndexer"></a>Stap 3: Maak een indexeerfunctie

Met behulp van een HTTP POST-verzoek met de volgende headers kunt u een nieuwe indexeerfunctie binnen een Azure Search-service.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Het hoofdgedeelte van de aanvraag bevat een definitie van de indexer die de volgende velden bevatten moet:

- **naam**: vereist. De naam van de indexeerfunctie.

- **de gegevensbronnaam**: vereist. De naam van een bestaande gegevensbron.

- **targetIndexName**: vereist. De naam van een bestaande index.

- **schema**: optioneel. Zie [Indexing schema](#IndexingSchedule) hieronder.

###<a id="IndexingSchedule"></a>Indexeerfuncties volgens een schema uitgevoerd

Een indexeerfunctie kan eventueel een planning opgeven. Als u een schema aanwezig is, wordt de indexeerfunctie periodiek volgens planning uitgevoerd. Schema heeft de volgende kenmerken:

- **interval**: vereist. Een waarde voor de duur die een interval of periode voor indexering wordt uitgevoerd. De minimaal toegestane interval is 5 minuten. de langste is één dag. Als het is geformatteerd als een XSD-'dayTimeDuration'-waarde (een beperkte subset van de waarde van een [ISO 8601 duur](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Het patroon voor dit is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur.

- **Starttijd**: vereist. Een UTC datetime die aangeeft wanneer de indexeerfunctie moet beginnen met.

###<a id="CreateIndexerExample"></a>Voorbeeld van de instantie aanvragen

Het volgende voorbeeld wordt een indexeerfunctie die gegevens opgehaald uit de collectie waarnaar wordt verwezen door de `myDocDbDataSource` de gegevensbron aan de `mySearchIndex` index volgens een schema dat begint op 1 januari 2015 UTC en wordt elk uur wordt uitgevoerd.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Reactie

U ontvangt een antwoord HTTP-201 gemaakt als de indexeerfunctie is gemaakt.

##<a id="RunIndexer"></a>Stap 4: Een indexering uitvoeren

Naast het uitvoeren van periodiek volgens een schema, kan een indexeerfunctie ook worden opgeroepen op verzoek door de volgende HTTP POST-aanvraag:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Reactie

U ontvangt een antwoord HTTP-202 geaccepteerd als de indexeerfunctie is geactiveerd.

##<a name="GetIndexerStatus"></a>Stap 5: Status van de indexeerfunctie ophalen

U kunt een HTTP GET-verzoek voor het ophalen van de huidige status en de uitvoering van de geschiedenis van een indexeerfunctie uitgeven:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Reactie

Er verschijnt een HTTP 200 OK antwoord en een hoofdgedeelte van de response met informatie over de algemene gezondheidstoestand van de indexering, de laatste aanroep van de indexer, evenals de geschiedenis van recente indexeerfunctie aanroepen (indien aanwezig).

Het antwoord ziet er ongeveer als volgt:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Uitvoering van geschiedenis bevat tot de 50 meest recente voltooide uitvoering en in omgekeerde chronologische volgorde worden gesorteerd (dus de laatste uitvoering eerst in het antwoord komt).

##<a name="NextSteps"></a>Volgende stappen

Gefeliciteerd! U hebt geleerd integreren Azure DocumentDB met Azure zoeken met behulp van de indexering voor DocumentDB.

 - Hoe meer informatie over Azure DocumentDB, Zie de [pagina DocumentDB-service](https://azure.microsoft.com/services/documentdb/).

 - Als u wilt weten hoe de informatie over Azure zoeken, Zie de [zoekpagina service](https://azure.microsoft.com/services/search/).
