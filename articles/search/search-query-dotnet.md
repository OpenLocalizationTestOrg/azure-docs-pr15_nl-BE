<properties
    pageTitle="Uw Azure Search-Index met de .NET SDK query | Microsoft Azure | De zoekservice hosted cloud"
    description="Een zoekopdracht in Azure zoekopdracht samenstellen en gebruiken van zoekparameters zoekresultaten filteren en sorteren."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Query uitvoeren op de Azure Search-index met de .NET SDK
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In dit artikel wordt beschreven hoe u query uitvoeren op een index met de [Azure Search.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Voordat u deze procedure begint, moet al [een Azure Search index gemaakt](search-what-is-an-index.md) en [gevuld met gegevens](search-what-is-data-import.md).

Houd er rekening mee dat alle van de voorbeeldcode in dit artikel wordt geschreven in C#. U vindt de volledige bron-code [op GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>I. Identificeren van uw Azure Search-service query api-sleutel
Nu u een Azure Search index hebt gemaakt, bent u bijna klaar om query's met behulp van de SDK voor .NET. Eerst moet u een van de query-api-sleutels die is gegenereerd voor het inrichten van zoekservice verkrijgen. De .NET SDK stuurt deze api-sleutel voor elke aanvraag met uw service. Met een geldige code stelt vertrouwen op een basis per aanvraag, tussen de toepassing voor het verzenden van de aanvraag en de service die het verwerkt.

1. Om van uw service api-toetsen u moet zich aanmelden bij de [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van de Azure Search-service
3. Klik op het pictogram "Toetsen"

De service heeft *admin* en *sleutels van de query*.

  - De primaire en secundaire *sleutels admin* volledige rechten toekennen voor alle bewerkingen, inclusief de mogelijkheid om de service beheren, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u gebruiken van de secundaire sleutel blijven kunt als u besluit om opnieuw te genereren voor de primaire sleutel, en vice versa.
  - De *sleutels van de query* alleen-lezen toegang verlenen tot documenten en indexen en zijn meestal verdeeld voor clienttoepassingen die verzoeken zoeken.

Voor de toepassing van het zoeken in een index, kunt u een van de sleutels van de query. Uw sleutels admin kunnen ook worden gebruikt voor query's, maar moet u de sleutel van een query in de toepassingscode als deze het [principe van minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege)beter volgt.

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Maak een instantie van de klasse SearchIndexClient
Voor het verlenen van query's met de .NET SDK Azure zoeken, moet u een exemplaar maken van de `SearchIndexClient` klasse. Deze klasse heeft verschillende constructors. Desgewenst neemt uw servicenaam, naam van de index, en een `SearchCredentials` object als parameters. `SearchCredentials`loopt uw api-sleutel.

De volgende code wordt een nieuw `SearchIndexClient` voor de "hotels" index (gemaakt in [een Azure Search-index met de SDK voor .NET maken](search-create-index-dotnet.md)) met behulp van waarden voor de zoeknaam van de service en de api-sleutel die zijn opgeslagen in het configuratiebestand van de toepassing (`app.config` of `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`heeft een `Documents` eigenschap. Met deze eigenschap wordt de methoden die u wilt opvragen van zoekindexen Azure.

## <a name="iii-query-your-index"></a>III. De index opvragen
Zoeken met de SDK voor .NET is zo eenvoudig telefoonkaart de `Documents.Search` methode van de `SearchIndexClient`. Deze methode heeft een aantal parameters, met inbegrip van de tekst samen met een `SearchParameters` -object dat kan worden gebruikt om de query verder te verfijnen.

#### <a name="types-of-queries"></a>Typen query 's
Zijn de twee belangrijkste [querytypen](search-query-overview.md#types-of-queries) gebruikt u `search` en `filter`. A `search` query zoekt u naar een of meer termen in alle _doorzoekbare_ velden in de index. A `filter` query wordt geëvalueerd als een Boole-expressie via alle _Filterbaar_ velden in een index.

Zowel zoekopdrachten en filters worden uitgevoerd met behulp van de `Documents.Search` methode. Een zoekopdracht kan worden doorgegeven de `searchText` parameter, terwijl een expressie kan worden doorgegeven de `Filter` eigenschap van de `SearchParameters` klasse. Als u wilt filteren zonder te zoeken, gewoon doorgeven `"*"` voor de `searchText` parameter. Als u wilt zoeken zonder te filteren, laat de `Filter` unset, eigenschap of niet door een `SearchParameters` exemplaar helemaal.

#### <a name="example-queries"></a>Voorbeeld van query 's

De volgende voorbeeldcode ziet u een aantal verschillende manieren naar de index "hotels" is gedefinieerd in [een Azure Search-index met de SDK voor .NET maken](search-create-index-dotnet.md#DefineIndex). De documenten met de zoekresultaten geretourneerd zijn exemplaren van de `Hotel` klasse, die is gedefinieerd in [Het importeren van gegevens in Azure zoeken met behulp van de SDK voor .NET](search-import-data-dotnet.md#HotelClass). De voorbeeldcode maakt gebruik van een `WriteDocuments` methode aan de lijst met zoekresultaten op de console-uitvoer. Deze methode is beschreven in de volgende sectie.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Resultaten verwerken
De `Documents.Search` methode geeft als resultaat een `DocumentSearchResult` -object waarin de resultaten van de query. In het voorbeeld in de vorige sectie, gebruikt een methode met de naam `WriteDocuments` aan de lijst met zoekresultaten op de console-uitvoer:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Hier ziet u hoe de resultaten van de query's in de vorige sectie, ervan uitgaande dat de "hotels" index wordt gevuld met de voorbeeldgegevens in de [Gegevens importeren in Azure zoeken met behulp van de SDK voor .NET](search-import-data-dotnet.md)eruit:

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

De bovenstaande voorbeeldcode wordt de console voor de uitvoer van de zoekresultaten. Ook moet u zoekresultaten weergeven in uw eigen toepassing. Zie [dit voorbeeld op GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) voor een voorbeeld van het genereren van zoekresultaten in een webtoepassing op basis van ASP.NET MVC.
