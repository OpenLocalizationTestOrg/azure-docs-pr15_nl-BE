<properties
    pageTitle="Het uploaden van gegevens in Azure zoeken met behulp van de SDK voor .NET | Microsoft Azure | De zoekservice hosted cloud"
    description="Informatie over het uploaden van gegevens naar een index in Azure zoeken met behulp van de SDK voor .NET."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Gegevens uploaden naar Azure zoeken met behulp van de SDK voor .NET
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In dit artikel wordt beschreven hoe u de [Azure Search.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) gebruiken om gegevens te importeren in een zoekindex Azure.

Voordat u deze procedure begint, hebt u al [gemaakt een Azure Search-index](search-what-is-an-index.md). In dit artikel wordt ervan uitgegaan dat u al hebt gemaakt een `SearchServiceClient` object, zoals aangegeven in het vak [met de .NET SDK Azure Search index maken](search-create-index-dotnet.md#CreateSearchServiceClient).

Houd er rekening mee dat alle van de voorbeeldcode in dit artikel wordt geschreven in C#. U vindt de volledige bron-code [op GitHub](http://aka.ms/search-dotnet-howto).

Om de push-documenten in de index met de SDK voor .NET, moet u:

  1. Maak een `SearchIndexClient` -object om te verbinden met de zoekindex.
  2. Maak een `IndexBatch` met de documenten moeten worden toegevoegd, gewijzigd of verwijderd.
  3. Bel de `Documents.Index` methode van de `SearchIndexClient` voor het verzenden van de `IndexBatch` aan de zoekindex.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>I. Maak een instantie van de klasse SearchIndexClient
Om gegevens te importeren in de index met de .NET SDK van Azure zoeken, moet u een exemplaar maken van de `SearchIndexClient` klasse. U kunt maken deze instantie zelf, maar het gemakkelijker als u al een `SearchServiceClient` exemplaar aan te roepen de `Indexes.GetClient` methode. Bijvoorbeeld, hier is hoe u zou krijgen een `SearchIndexClient` voor de index met de naam "hotels" van een `SearchServiceClient` met de naam `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] In een typische zoektoepassing wordt indexbeheerservers en de bevolking uitgevoerd door een afzonderlijk onderdeel van zoekquery's. `Indexes.GetClient`is handig voor het vullen van een index omdat u de moeite bespaart om een andere `SearchCredentials`. Dit gebeurt door aan de admin-sleutel die is gemaakt door de `SearchServiceClient` aan de nieuwe `SearchIndexClient`. Echter in het deel van de toepassing die door query's worden uitgevoerd, is het beter voor het maken van de `SearchIndexClient` direct zodat u in de sleutel van een query in plaats van een admin-toets doorgeven kunt. Dit is in overeenstemming met het [principe van minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) en helpt uw toepassing om veiliger te maken. U vindt meer informatie over de admin en sleutels van de query in de [Azure Search REST API reference op MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

`SearchIndexClient`heeft een `Documents` eigenschap. Met deze eigenschap wordt de methoden die u wilt toevoegen, wijzigen, verwijderen of de documenten in de index opvragen.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Besluit welke indexing gebruiken
Om gegevens te importeren met behulp van de SDK voor .NET, moet u inpakken van gegevens in een `IndexBatch` object. Een `IndexBatch` ingekapseld door een verzameling van `IndexAction` -objecten, die elk een document en een eigenschap Azure zoeken welke actie moet worden uitgevoerd op dat document (uploaden, samenvoegen, verwijderen, enz.) bevat. Afhankelijk van welke van de onderstaande acties u kunt alleen bepaalde velden opgenomen voor elk document worden moeten:

Actie | Beschrijving | Vereiste velden voor elk document | Notities
--- | --- | --- | ---
`Upload` | Een `Upload` actie is vergelijkbaar met een 'upsert' waarin het document wordt ingevoegd als het nieuw is en bijgewerkt/vervangen als deze bestaat. | sleutel plus eventuele andere velden die u wilt definiëren | Tijdens het bijwerken van/vervangen van een bestaand document, heeft elk veld dat is opgegeven in de aanvraag veld is ingesteld op `null`. Dit gebeurt ook als het veld eerder op een null-waarde is ingesteld.
`Merge` | Een bestaand document wordt bijgewerkt met de opgegeven velden. Als het document niet in de index bestaat nog, mislukt het samenvoegen. | sleutel plus eventuele andere velden die u wilt definiëren | Elk veld dat u in een samenvoegbewerking wordt vervangen door het bestaande veld in het document. Dit omvat velden van het type `DataType.Collection(DataType.String)`. Als het document bevat een veld bijvoorbeeld `tags` met de waarde `["budget"]` en uitvoeren van een samenvoegbewerking met de waarde `["economy", "pool"]` voor `tags`, de uiteindelijke waarde van de `tags` veld `["economy", "pool"]`. Geen `["budget", "economy", "pool"]`.
`MergeOrUpload` | Deze actie werkt als `Merge` als een document met de opgegeven sleutel al in de index bestaat. Als het document niet bestaat nog, gedraagt zich als `Upload` met een nieuw document. | sleutel plus eventuele andere velden die u wilt definiëren | -
`Delete` | Hiermee verwijdert u het opgegeven document uit de index. | alleen de sleutel | Alle velden die u opgeeft, dan wordt het veld genegeerd. Als u een afzonderlijk veld verwijderen uit een document wilt, gebruikt u `Merge` in plaats daarvan en gewoon het veld expliciet instellen op null.

U kunt opgeven welke actie u wilt gebruiken voor de verschillende statische methoden van de `IndexBatch` en `IndexAction` -klassen, zoals in de volgende sectie.

## <a name="iii-construct-your-indexbatch"></a>III. De IndexBatch maken
Nu u weet welke acties op uw documenten, bent u klaar om samen te stellen de `IndexBatch`. In het volgende voorbeeld ziet u hoe een batch wilt maken met een aantal verschillende acties. Ons voorbeeld gebruikt een aangepaste klasse met de naam `Hotel` die wordt toegewezen aan een document in de index "hotels".

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

In dit geval gebruiken we `Upload`, `MergeOrUpload`, en `Delete` als onze acties zoeken, zoals aangegeven door de methoden aangeroepen voor het `IndexAction` klasse.

Stel dat in dit voorbeeld "hotels" index al wordt gevuld met een aantal documenten. Houd er rekening mee hoe we heeft niet alle documentvelden mogelijk opgeven wanneer u `MergeOrUpload` en hoe we de documentsleutel alleen opgegeven (`HotelId`) bij het gebruik van `Delete`.

Ook rekening mee dat u kunt alleen documenten maximaal 1000 in één aanvraag indexing.

> [AZURE.NOTE] In dit voorbeeld zijn we verschillende acties toepassen op andere documenten. Als u wilt dezelfde acties uitvoeren in alle documenten in de partij, in plaats van bellen `IndexBatch.New`, kunt u de statische methoden van `IndexBatch`. Zo kunt u batches door het aanroepen van `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, of `IndexBatch.Delete`. Deze methoden houden een verzameling documenten (objecten van het type `Hotel` in dit voorbeeld) in plaats van `IndexAction` objecten.

## <a name="iv-import-data-to-the-index"></a>IV. Importeren van gegevens in de index
Nu dat u een geïnitialiseerde hebt `IndexBatch` -object, kunt u deze verzenden aan de index door het aanroepen van `Documents.Index` op de `SearchIndexClient` object. In het volgende voorbeeld ziet u hoe aanroepen `Index`, evenals een aantal extra stappen moet uitvoeren:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Opmerking de `try` / `catch` rond het aanroepen van de `Index` methode. De catch-blok verwerkt een belangrijke fout melden voor indexering. Als uw Azure Search-service niet aan de index van de documenten in de partij, een `IndexBatchException` veroorzaakt door `Documents.Index`. Dit kan gebeuren als u documenten indexeert terwijl uw service zwaar wordt belast. **Het wordt aangeraden expliciet afhandeling van deze kwestie in uw code.** U kunt uitstellen en probeer het opnieuw indexeren van de documenten die zijn mislukt, of kunt u zich aanmelden en doorgaan met het monster is of iets anders, afhankelijk van de vereisten van uw toepassing consistentie kunt u doen.

Ten slotte, de code in het voorbeeld hierboven vertragingen gedurende twee seconden. Indexering gebeurt er asynchroon in Azure Search-service zodat de voorbeeldtoepassing wachten tot een korte tijd moet om ervoor te zorgen dat de documenten beschikbaar voor zoeken zijn. Vertragingen als volgt zijn meestal alleen nodig in de demo's, tests en voorbeeldtoepassingen.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Hoe documenten worden verwerkt door de .NET SDK

U vraagt zich misschien af hoe de Azure zoeken .NET SDK kunnen exemplaren van een door de gebruiker gedefinieerde klasse zoals uploaden is `Hotel` aan de index. Bij de beantwoording van die vraag, we kijken naar de `Hotel` klasse, die wordt toegewezen aan de index-schema dat is gedefinieerd in [een Azure Search-index met de SDK voor .NET maken](search-create-index-dotnet.md#DefineIndex):

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Het eerste dat opvalt is dat elke openbare eigenschap van `Hotel` komt overeen met een veld in de definitie van de index, maar met één belangrijk verschil: de naam van elk veld begint met een kleine letter ('kamelen geval"), terwijl de naam van elke openbare eigenschap van `Hotel` begint met een hoofdletter ("geval van Pascal"). Dit is een algemeen scenario in .NET-toepassingen die gegevensbinding uitvoeren waarbij het doelschema is buiten de controle van de ontwikkelaar van toepassingen. In plaats van het strijdig zijn met de .NET naamgevingsregels doordat de eigenschap namen kamelen-case, ziet u de SDK op de namen van eigenschappen toewijzen aan kamelen-geval automatisch met de `[SerializePropertyNamesAsCamelCase]` kenmerk.

> [AZURE.NOTE] De Azure Search .NET SDK gebruikt de bibliotheek [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) serialiseren en terugconverteren van uw aangepaste modelobjecten naar en van JSON. U kunt deze serialisatie aanpassen indien nodig. Meer informatie vindt u in [upgraden naar Azure Search SDK voor .NET versie 1.1](search-dotnet-sdk-migration.md#WhatsNew). Een voorbeeld hiervan is het gebruik van de `[JsonProperty]` van het kenmerk op het `DescriptionFr` eigenschap in de bovenstaande voorbeeldcode.

Het tweede belangrijke onderwerp over de `Hotel` klasse worden de gegevenstypen van de openbare eigenschappen. De .NET-typen van deze eigenschappen toewijzen aan het overeenkomstige veld type in de definitie van de index. Bijvoorbeeld, de `Category` wordt toegewezen aan de eigenschap de tekenreeks de `category` veld van het type `DataType.String`. Er zijn vergelijkbare toewijzingen tussen `bool?` en `DataType.Boolean`, `DateTimeOffset?` en `DataType.DateTimeOffset`, enz. De specifieke regels voor de toewijzing zijn gedocumenteerd met de `Documents.Get` methode op [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Deze mogelijkheid om uw eigen klassen gebruiken als documenten werkt in beide richtingen; U kunt ook zoekresultaten worden opgehaald en de SDK automatisch terugconverteren van hen op het type van uw keuze, zoals in het [volgende artikel](search-query-dotnet.md).

> [AZURE.NOTE] De Azure Search .NET SDK biedt ook ondersteuning voor dynamisch getypeerde documenten met behulp van de `Document` klasse die een sleutel/waarde-toewijzing van veldnamen naar waarden. Dit is nuttig in scenario's waar u het schema van de index in de ontwerpfase niet weet of indien het zou onhandig om te binden aan een specifiek model klassen zijn. Alle methoden in de SDK die betrekking op documenten hebben hebben overbelastingen die met werken de `Document` klasse, evenals sterk getypeerde overbelastingen die een algemeen typeparameter. Alleen de laatste worden in de voorbeeldcode in dit artikel gebruikt. U kunt Lees meer over de `Document` klasse [op MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Een belangrijke opmerking over gegevenstypen**

Bij het ontwerpen van uw eigen klassen model toe te wijzen aan een Azure zoekindex wordt aangeraden, zoals het declareren van eigenschappen van het type `bool` en `int` worden Null-waarden (bijvoorbeeld, `bool?` in plaats van `bool`). Als u een eigenschap niet null gebruikt, hebt u om ervoor te **zorgen** dat er geen documenten in de index voor het betreffende veld een null-waarde bevatten. De SDK noch de Azure Search-service kunt u dit afdwingen.

Dit is geen hypothetische belang: Stel een scenario waarin u een nieuw veld toevoegen aan een bestaande index die is van het type `DataType.Int32`. Na het bijwerken van de definitie van de index, hebben alle documenten van een null-waarde voor het nieuwe veld (Aangezien alle typen null-waarden in Azure zoeken). Als u vervolgens met een klasse model niet null `int` eigenschap voor het veld, ontvangt u een `JsonSerializationException` als volgt bij het ophalen van documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Om deze reden is het raadzaam gebruik te typen in uw model klassen beste.

## <a name="next"></a>Volgende
Na het vullen van de zoekindex Azure, zijn u kunt beginnen met het uitgeven van query's om te zoeken naar documenten. Zie [Uw Azure-zoekindex Query](search-query-overview.md) voor meer informatie.
