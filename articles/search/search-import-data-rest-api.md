<properties
    pageTitle="Het uploaden van gegevens in Azure zoeken met de REST API | Microsoft Azure | De zoekservice hosted cloud"
    description="Informatie over het uploaden van gegevens naar een index in Azure zoeken met behulp van de API van de REST."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Gegevens uploaden naar Azure zoeken op basis van de REST-API
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In dit artikel wordt beschreven hoe u de [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) gebruiken om gegevens te importeren in een zoekindex Azure.

Voordat u deze procedure begint, hebt u al [gemaakt een Azure Search-index](search-what-is-an-index.md).

Om de documenten in de index met de REST API push, verleent u aan het eindpunt van de index van de URL een HTTP POST-aanvraag. De hoofdtekst van het hoofdgedeelte van de HTTP-aanvraag is een JSON-object met de documenten moeten worden toegevoegd, gewijzigd of verwijderd.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>I. Identificeren van uw Azure Search service admin api-sleutel
Bij het verlenen van HTTP-aanvragen voor uw service door middel van de REST-API, moet *elke* API aanvraag de api-sleutel die is gegenereerd voor het inrichten van Search-service bevatten. Met een geldige code stelt vertrouwen op een basis per aanvraag, tussen de toepassing voor het verzenden van de aanvraag en de service die het verwerkt.

1. Om van uw service api-toetsen u moet zich aanmelden bij de [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van de Azure Search-service
3. Klik op het pictogram "Toetsen"

De service heeft *admin* en *sleutels van de query*.

  - De primaire en secundaire *sleutels admin* volledige rechten toekennen voor alle bewerkingen, inclusief de mogelijkheid om de service beheren, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u gebruiken van de secundaire sleutel blijven kunt als u besluit om opnieuw te genereren voor de primaire sleutel, en vice versa.
  - De *sleutels van de query* alleen-lezen toegang verlenen tot documenten en indexen en zijn meestal verdeeld voor clienttoepassingen die verzoeken zoeken.

Voor de toepassing van het importeren van gegevens in een index, kunt u ofwel uw admin primaire of secundaire sleutel.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Besluit welke indexing gebruiken
Wanneer u de REST API, verleent u HTTP POST-aanvragen met JSON aanvraag instanties naar eindpunt-URL van uw Azure Search-index. De JSON-object in het hoofdgedeelte van de HTTP-aanvraag bevat één JSON-array met de naam "waarde" met JSON-objecten die de documenten die u wilt toevoegen aan de index, bijwerken of verwijderen.

Elk object JSON in de "waarde" matrix vertegenwoordigt een document moeten worden geïndexeerd. Elk van deze objecten bevat de sleutel van het document en geeft u de gewenste actie indexing (uploaden, samenvoegen, verwijderen, enz.). Afhankelijk van welke van de onderstaande acties u kunt alleen bepaalde velden opgenomen voor elk document worden moeten:

@search.action | Beschrijving | Vereiste velden voor elk document | Notities
--- | --- | --- | ---
`upload` | Een `upload` actie is vergelijkbaar met een 'upsert' waarin het document wordt ingevoegd als het nieuw is en bijgewerkt/vervangen als deze bestaat. | sleutel plus eventuele andere velden die u wilt definiëren | Tijdens het bijwerken van/vervangen van een bestaand document, heeft elk veld dat is opgegeven in de aanvraag veld is ingesteld op `null`. Dit gebeurt ook als het veld eerder op een null-waarde is ingesteld.
`merge` | Een bestaand document wordt bijgewerkt met de opgegeven velden. Als het document niet in de index bestaat nog, mislukt het samenvoegen. | sleutel plus eventuele andere velden die u wilt definiëren | Elk veld dat u in een samenvoegbewerking wordt vervangen door het bestaande veld in het document. Dit omvat velden van het type `Collection(Edm.String)`. Als het document bevat een veld bijvoorbeeld `tags` met de waarde `["budget"]` en uitvoeren van een samenvoegbewerking met de waarde `["economy", "pool"]` voor `tags`, de uiteindelijke waarde van de `tags` veld `["economy", "pool"]`. Geen `["budget", "economy", "pool"]`.
`mergeOrUpload` | Deze actie werkt als `merge` als een document met de opgegeven sleutel al in de index bestaat. Als het document niet bestaat nog, gedraagt zich als `upload` met een nieuw document. | sleutel plus eventuele andere velden die u wilt definiëren | -
`delete` | Hiermee verwijdert u het opgegeven document uit de index. | alleen de sleutel | Alle velden die u opgeeft, dan wordt het veld genegeerd. Als u een afzonderlijk veld verwijderen uit een document wilt, gebruikt u `merge` in plaats daarvan en gewoon het veld expliciet instellen op null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. De HTTP-aanvraag samenstelt en body aanvragen
Nu u hebt de vereiste waarden voor uw Indexacties verzameld, bent u klaar om samen te stellen van de werkelijke HTTP-aanvraag en JSON hoofdgedeelte van de aanvraag om uw gegevens te importeren.

#### <a name="request-and-request-headers"></a>Aanvraag en aanvraagheaders
In de URL, u moet uw naam, naam van de index ("hotels" in dit geval), alsmede de juiste API versie (de huidige versie van de API `2015-02-28` op het moment van publicatie van dit document). Moet u voor het definiëren van de `Content-Type` en `api-key` request headers. Voor deze laatste een admin-sleutels van uw service te gebruiken.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Hoofdgedeelte van de aanvraag

```JSON
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

In dit geval gebruiken we `upload`, `mergeOrUpload`, en `delete` als onze acties zoeken.

Stel dat in dit voorbeeld "hotels" index al wordt gevuld met een aantal documenten. Houd er rekening mee hoe we heeft niet alle documentvelden mogelijk opgeven wanneer u `mergeOrUpload` en hoe we de documentsleutel alleen opgegeven (`hotelId`) bij het gebruik van `delete`.

Ook rekening mee dat u kunt alleen tot 1000 documenten (of 16 MB) in een enkele aanvraag indexing.

## <a name="iv-understand-your-http-response-code"></a>IV. De HTTP-reactiecode begrijpen
#### <a name="200"></a>200
Na het indienen van een aanvraag voor een geslaagde indexering ontvangt u een HTTP-antwoord met een statuscode van `200 OK`. De JSON-instantie van het HTTP-antwoord is:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
De statuscode `207` wordt geretourneerd wanneer ten minste één item is niet geïndexeerd. De JSON-instantie van het HTTP-antwoord bevat informatie over de mislukte documenten.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] Dit betekent vaak dat de belasting van de zoekservice bereiken van een punt waar aanvragen indexing beginnen zal om terug te keren `503` antwoorden. In dit geval we sterk aanbevelen die uw clientcode weer af en wacht voordat opnieuw wordt geprobeerd. Hierdoor krijgt het systeem enige tijd om te herstellen, vergroten de kans dat toekomstige aanvragen worden verwerkt. Snel opnieuw uw verzoeken, zal de situatie alleen verlengen.

#### <a name="429"></a>429
De statuscode `429` wordt geretourneerd wanneer u uw quotum op het aantal documenten per index hebt overschreden.

#### <a name="503"></a>503
De statuscode `503` wordt geretourneerd als geen van de items in de aanvraag zijn geïndexeerd. Deze fout geeft aan dat het systeem zwaar wordt belast is en dat op dit moment uw aanvraag niet verwerken.

> [AZURE.NOTE] In dit geval we sterk aanbevelen die uw clientcode weer af en wacht voordat opnieuw wordt geprobeerd. Hierdoor krijgt het systeem enige tijd om te herstellen, vergroten de kans dat toekomstige aanvragen worden verwerkt. Snel opnieuw uw verzoeken, zal de situatie alleen verlengen.

Raadpleeg voor meer informatie over acties en reacties van succes-/ foutmeldingen, [toevoegen, bijwerken of verwijderen van documenten](https://msdn.microsoft.com/library/azure/dn798930.aspx). Zie voor meer informatie over andere HTTP-statuscodes die kan worden gegeven bij een storing, [HTTP-statuscodes (Azure Search)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Volgende
Na het vullen van de zoekindex Azure, zijn u kunt beginnen met het uitgeven van query's om te zoeken naar documenten. Zie [Uw Azure-zoekindex Query](search-query-overview.md) voor meer informatie.
