<properties
    pageTitle="Met de SDK voor .NET Azure Search index maken | Microsoft Azure | De zoekservice hosted cloud"
    description="Een index maken in de SDK voor .NET zoeken Azure-code."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Met de SDK voor .NET Azure Search index maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


Dit artikel begeleidt u door het proces van het maken van een Azure Search [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) met de [Azure Search.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Voordat u deze handleiding te volgen en het maken van een index, hebt u al [een Azure Search-service gemaakt](search-create-service-portal.md).

Houd er rekening mee dat alle van de voorbeeldcode in dit artikel wordt geschreven in C#. U vindt de volledige bron-code [op GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>I. Identificeren van uw Azure Search service admin api-sleutel
Nu u een zoekservice Azure hebt ingericht, u bent bijna klaar om te verzoeken tegen uw service-eindpunt met de SDK voor .NET. Eerst moet u een van de beheer-api-sleutels die is gegenereerd voor het inrichten van zoekservice verkrijgen. De .NET SDK stuurt deze api-sleutel voor elke aanvraag met uw service. Met een geldige code stelt vertrouwen op een basis per aanvraag, tussen de toepassing voor het verzenden van de aanvraag en de service die het verwerkt.

1. Om van uw service api-toetsen u moet zich aanmelden bij de [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van de Azure Search-service
3. Klik op het pictogram "Toetsen"

De service heeft *admin* en *sleutels van de query*.

  - De primaire en secundaire *sleutels admin* volledige rechten toekennen voor alle bewerkingen, inclusief de mogelijkheid om de service beheren, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u gebruiken van de secundaire sleutel blijven kunt als u besluit om opnieuw te genereren voor de primaire sleutel, en vice versa.
  - De *sleutels van de query* alleen-lezen toegang verlenen tot documenten en indexen en zijn meestal verdeeld voor clienttoepassingen die verzoeken zoeken.

Voor de toepassing van het maken van een index, kunt u ofwel uw admin primaire of secundaire sleutel.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Maak een instantie van de klasse SearchServiceClient
Om te beginnen met de .NET SDK van Azure zoeken, moet u een exemplaar maken van de `SearchServiceClient` klasse. Deze klasse heeft verschillende constructors. Gewenste wordt de zoeknaam van de service en een `SearchCredentials` object als parameters. `SearchCredentials`loopt uw api-sleutel.

De volgende code wordt een nieuw `SearchServiceClient` met waarden voor de zoeknaam van de service en de api-sleutel die zijn opgeslagen in het configuratiebestand van de toepassing (`app.config` of `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`heeft een `Indexes` eigenschap. Met deze eigenschap wordt de methoden die u maken wilt, weergeven, bijwerken of verwijderen van zoekindexen Azure.

> [AZURE.NOTE] De `SearchServiceClient` klasse beheert u verbindingen met uw zoekservice. Om te voorkomen dat te veel verbindingen, moet u proberen te delen één exemplaar van de `SearchServiceClient` in uw toepassing, indien mogelijk. De methoden zijn thread-veilige dergelijke delen inschakelen.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Definieer uw Azure Search index met behulp van de `Index` klasse
Een enkele aanroep van de `Indexes.Create` methode maakt u de index. Deze methode heeft als parameter een `Index` -object dat de Azure zoekindex gedefinieerd. U nodig hebt voor het maken van een `Index` -object en wordt geïnitialiseerd als volgt:

1. Stel de `Name` eigenschap van de `Index` object op de naam van de index.
2. Stel de `Fields` eigenschap van de `Index` object naar een matrix van `Field` objecten. Elk van de `Field` objecten bepaalt het gedrag van een veld in de index. U kunt de naam van het veld voor de constructor, samen met het gegevenstype (of analyzer voor reeks velden) kunt opgeven. U kunt ook instellen met andere eigenschappen zoals `IsSearchable`, `IsFilterable`, enz.

Het is belangrijk uw zoekopdracht ervaring en zakelijke behoeften van de gebruiker in het achterhoofd te houden bij het ontwerpen van de index als elk `Field` de [juiste eigenschappen](https://msdn.microsoft.com/library/azure/dn798941.aspx)moeten worden toegewezen. Deze eigenschappen bepalen welke functies (filteren, faceting, sorteren, zoeken in volledige tekst, enz.) zoeken van toepassing op velden. Voor elke eigenschap die u niet expliciet instelt, de `Field` klasse wordt standaard ingesteld op de bijbehorende zoekfunctie uitschakelen tenzij expliciet inschakelt.

In ons voorbeeld hebben we onze index "hotels" genoemd en onze velden als volgt gedefinieerd:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

We zorgvuldig gekozen waarden van de eigenschappen voor elk `Field` op basis van hoe we denken ze in een toepassing worden gebruikt. Bijvoorbeeld, is het waarschijnlijk dat mensen zoeken naar hotels geïnteresseerd in trefwoord resultaten op de `description` veld, zodat we voor dat veld zoeken in volledige tekst inschakelen door `IsSearchable` naar `true`.

Houd er rekening mee dat slechts één veld in de index van het type `DataType.String` moet de aangewezen als _het sleutelveld_ door `IsKey` op `true` (Zie `hotelId` in het bovenstaande voorbeeld).

De bovenstaande definitie van de index wordt gebruikt voor een aangepaste taal analyzer voor het `description_fr` omdat het is bedoeld voor het opslaan van Franse tekst veld. Zie [de taal ondersteunen onderwerp op MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) , alsmede de bijbehorende [blogberichten](https://azure.microsoft.com/blog/language-support-in-azure-search/) voor meer informatie over taal analyzers.

> [AZURE.NOTE]  Merk op dat door `AnalyzerName.FrLucene` in de constructor, de `Field` worden automatisch van het type `DataType.String` en `IsSearchable` ingesteld op `true`.

## <a name="iv-create-the-index"></a>IV. De index maken
Nu dat u een geïnitialiseerde hebt `Index` -object, kunt u de index maken door het aanroepen van `Indexes.Create` op de `SearchServiceClient` object:

```csharp
serviceClient.Indexes.Create(definition);
```

Voor een succesvolle aanvraag wordt normaal gesproken de methode geretourneerd. Als er een probleem met de aanvraag zoals een ongeldige parameter, genereert de methode `CloudException`.

Als u klaar bent met een index wilt verwijderen, maar bel de `Indexes.Delete` methode op uw `SearchServiceClient`. Dit is bijvoorbeeld hoe we de "hotels" index wilt verwijderen:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] De voorbeeldcode in dit artikel gebruikt de synchrone methoden van de Azure Search .NET SDK voor eenvoud. Het is raadzaam de asynchrone methoden te gebruiken in uw eigen toepassingen te bewaren, schaalbare en responsieve. Bijvoorbeeld, in bovenstaande voorbeelden kunt u `CreateAsync` en `DeleteAsync` in plaats van `Create` en `Delete`.

## <a name="next"></a>Volgende
Na het maken van een zoekindex Azure, zult u klaar voor het [uploaden van uw inhoud in de index](search-what-is-data-import.md) zodat u kunt beginnen met het zoeken van uw gegevens.
