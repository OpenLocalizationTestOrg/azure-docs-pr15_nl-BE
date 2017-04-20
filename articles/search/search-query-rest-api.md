<properties
    pageTitle="Uw Azure Search-Index met de REST API query | Microsoft Azure | De zoekservice hosted cloud"
    description="Een zoekopdracht in Azure zoekopdracht samenstellen en gebruiken van zoekparameters zoekresultaten filteren en sorteren."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Query uitvoeren op de Azure-zoekindex REST API gebruiken
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In dit artikel wordt beschreven hoe u query uitvoeren op een index met de [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Voordat u deze procedure begint, moet al [een Azure Search index gemaakt](search-what-is-an-index.md) en [gevuld met gegevens](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>I. Identificeren van uw Azure Search-service query api-sleutel
Een belangrijk onderdeel van elke zoekbewerking tegen de Azure Search REST API is de *api-sleutel* die is gegenereerd voor het inrichten van service. Met een geldige code stelt vertrouwen op een basis per aanvraag, tussen de toepassing voor het verzenden van de aanvraag en de service die het verwerkt.

1. Om van uw service api-toetsen u moet zich aanmelden bij de [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van de Azure Search-service
3. Klik op het pictogram "Toetsen"

De service heeft *admin* en *sleutels van de query*.

 - De primaire en secundaire *sleutels admin* volledige rechten toekennen voor alle bewerkingen, inclusief de mogelijkheid om de service beheren, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u gebruiken van de secundaire sleutel blijven kunt als u besluit om opnieuw te genereren voor de primaire sleutel, en vice versa.
 - De *sleutels van de query* alleen-lezen toegang verlenen tot documenten en indexen en zijn meestal verdeeld voor clienttoepassingen die verzoeken zoeken.

Voor de toepassing van het zoeken in een index, kunt u een van de sleutels van de query. Uw sleutels admin kunnen ook worden gebruikt voor query's, maar moet u de sleutel van een query in de toepassingscode als deze het [principe van minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege)beter volgt.

## <a name="ii-formulate-your-query"></a>II. Het formuleren van de query
Er zijn twee manieren om [de index van de REST-API gebruiken](https://msdn.microsoft.com/library/azure/dn798927.aspx). Eén manier is om een HTTP POST-aanvraag waarin uw query-parameters worden gedefinieerd in een JSON-object in het hoofdgedeelte van de aanvraag. De andere manier is om een HTTP GET-verzoek waarin uw query-parameters worden gedefinieerd in de aanvraag-URL. Ziet dat boeken meer [versoepeld limieten](https://msdn.microsoft.com/library/azure/dn798927.aspx) voor de grootte van de parameters van query dan ophalen. Om deze reden is het raadzaam met boeken, tenzij er bijzondere omstandigheden waar GET gebruikt zou het handiger zijn.

POST en GET, moet u uw *naam*, *naam van de index*en de juiste *versie van de API* (de huidige versie van de API `2015-02-28` op het moment van publicatie van dit document) in de aanvraag-URL. Voor het ophalen worden de *query-tekenreeks* aan het einde van de URL waar u de queryparameters opgeven. Hieronder vindt u de URL-notatie:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

De indeling voor boeken is het hetzelfde, maar met alleen api versie in de parameters van query-tekenreeks.



#### <a name="example-queries"></a>Voorbeeld van query 's

Hier zijn een paar voorbeeld van query's op een index met de naam "hotels". Deze query's worden weergegeven in de indeling voor zowel GET en POST.

De volledige index zoeken naar de term 'budget' en retourneert alleen de `hotelName` veld:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Een filter toepassen op de index hotels goedkoper dan $150 per nacht te vinden en de `hotelId` en `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

De gehele order op een bepaald veld-index (`lastRenovationDate`) alleen weergeven en in aflopende volgorde, worden de resultaten van de twee belangrijkste `hotelName` en `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. De HTTP-aanvraag indienen
Nu u de query hebt geformuleerd als onderdeel van de URL van de HTTP-aanvraag (voor GET) of instantie (POST), kunt u definiëren de aanvraagheaders en indienen van uw query.

#### <a name="request-and-request-headers"></a>Aanvraag en aanvraagheaders
U moet twee aanvraagheaders definiëren voor ophalen of drie voor de POST:
1. De `api-key` kop moet worden ingesteld op de query-sleutel die u hebt gevonden in stap ik hierboven. Houd er rekening mee dat u ook een admin-toets als kunt de `api-key` kop, maar is het raadzaam een query-sleutel te gebruiken, aangezien deze uitsluitend alleen-lezen toegang verleent tot documenten en indexen.
2. De `Accept` kop moet worden ingesteld op `application/json`.
3. Voor boeken, de `Content-Type` kop moet ook worden ingesteld op `application/json`.

Zie hieronder voor een HTTP GET-verzoek naar de "hotels" index met Azure Search REST API met behulp van een eenvoudige query waarmee wordt gezocht naar de term "motel":

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Dit is dezelfde voorbeeldquery, met behulp van HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Een queryaanvraag voor een geslaagde resulteert in een statuscode van `200 OK` en de zoekresultaten worden geretourneerd als JSON in het hoofdgedeelte van de response. Hier is wat de resultaten voor de bovenstaande query uiterlijk, ervan uitgaande dat de "hotels" index wordt gevuld met de voorbeeldgegevens bij [Het importeren van gegevens in Azure zoeken met de REST API](search-import-data-rest-api.md) (Let erop dat de JSON is opgemaakt voor de duidelijkheid).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Voor meer informatie, Ga naar de sectie 'Antwoord' van [Documenten zoeken](https://msdn.microsoft.com/library/azure/dn798927.aspx). Zie voor meer informatie over andere HTTP-statuscodes die kan worden gegeven bij een storing, [HTTP-statuscodes (Azure Search)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
