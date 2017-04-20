<properties
   pageTitle="Een upgrade uitvoeren naar Azure Search SDK voor .NET versie 1.1 | Microsoft Azure | De zoekservice hosted cloud"
   description="Een upgrade uitvoeren naar Azure Search SDK voor .NET versie 1.1"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Een upgrade uitvoeren naar Azure Search .NET SDK versie 2.0-voorbeeld

Als u versie 1.1 of oudere van de [Azure Search.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), dit artikel helpt u bij het bijwerken van uw toepassing de volgende primaire versie 2.0 preview.

Zie voor een meer algemeen overzicht van de SDK, inclusief voorbeelden [Azure zoeken vanuit een toepassing .NET gebruiken](search-howto-dotnet-sdk.md).

Versie 2.0-voorbeeld van de Azure Search .NET SDK bevat enkele wijzigingen uit eerdere versies. Dit zijn meestal kleine, zodat uw programmacode wijzigen, moet alleen minimale inspanning vereisen. Zie de [stappen voor het bijwerken](#UpgradeSteps) voor instructies over het wijzigen van de code aan de nieuwe versie van de SDK.

> [AZURE.NOTE] Als u versie 0,13 preview of ouder gebruikt, moet u een upgrade uitvoert naar versie 1.1 eerste en vervolgens een upgrade uitvoert naar 2.0 preview. Zie [aanhangsel: stappen om te upgraden naar versie 1.1](#UpgradeStepsV1) voor instructies.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Wat is nieuw in versie 2.0-voorbeeld

Versie 2.0-voorbeeld wordt de eerste versie van de SDK voor .NET zoeken Azure als doel een voorbeeldversie van de Azure Search REST API, speciaal 2015-02-28-voorbeeld. Hierdoor kunt u veel voorbeeld mogelijkheden van Azure zoeken van een .NET-toepassing, waaronder de volgende:

- [Aangepaste analyzers](https://aka.ms/customanalyzers)
- Ondersteuning van [Azure Blob-opslag](search-howto-indexing-azure-blob-storage.md) en [Azure tabelopslag](search-howto-indexing-azure-tables.md) voor indexering
- Aanpassing via [veldtoewijzingen](search-indexer-field-mappings.md) voor indexering
- ETags ondersteunen zodat veilige gelijktijdig bijwerken van de index-definities, Indexeerfuncties en gegevensbronnen
- Ondersteuning voor Core .NET en .NET draagbare profiel 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Stappen voor het bijwerken

Eerst bijwerken NuGet referentie voor `Microsoft.Azure.Search` de NuGet Package Manager-console of door met de rechtermuisknop op het project verwijst en selecteren 'Beheren van de pakketten NuGet...' in Visual Studio. Zorg ervoor dat u voorlopige pakketten inschakelen door 'Voorlopige opnemen' selecteren in de NuGet 'Pakketten beheren' venster in Visual Studio of met behulp van de `-IncludePrerelease` overschakelen als u NuGet Package Manager-Console.

Als NuGet is gedownload, nieuwe pakketten en bijbehorende afhankelijkheden, opnieuw opbouwen van uw project. Afhankelijk van hoe de code van gestructureerd is, kan deze opnieuw met succes. Zo ja, bent u klaar om te gaan!

Als uw build is mislukt, ziet u een fout maken als volgt uit:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

De volgende stap is om deze build-fout te herstellen. Zie [wijzigingen in versie 2.0-voorvertoning te analyseren](#ListOfChanges) voor meer informatie over wat de fout veroorzaakt en hoe u het probleem oplossen.

Wordt er extra build-waarschuwingen verouderde methoden of eigenschappen. De waarschuwingen bevat instructies over wat te gebruiken in plaats van de functie vervangen. Bijvoorbeeld, als uw toepassing gebruikmaakt van de `SearchRequestOptions.RequestId` (eigenschap), ontvangt u een waarschuwing dat`"This property is deprecated. Please use ClientRequestId instead."`

Nadat u eventuele fouten build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functies als u wilt. Nieuwe functies in de SDK worden gegeven in [Wat is nieuw in versie 2.0-voorbeeld](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Verbreken van wijzigingen in versie 2.0-voorbeeld

Er is slechts één recente wijziging in versie 2.0-voorbeeld waarvoor codewijzigingen naast uw toepassing opnieuw samenstellen.

### <a name="indexesgetclient-return-type"></a>Het retourtype Indexes.GetClient

De `Indexes.GetClient` methode heeft een nieuwe retourtype. Eerder, heeft het `SearchIndexClient`, maar dit is gewijzigd in `ISearchIndexClient` in versie 2.0-voorbeeld. Dit is voor de ondersteuning van klanten die willen model de `GetClient` voor controles door het retourneren van een model implementatie van `ISearchIndexClient`.

#### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

U kunt deze wijzigen deze build fouten te verhelpen:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van de .NET SDK Azure Search, Zie onze onlangs bijgewerkte [procedures](search-howto-dotnet-sdk.md).

Uw feedback van harte Welkom op de SDK. Als u problemen ondervindt, gerust ons op de [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch)om hulp vragen. Als u een bug vindt, kunt u een probleem in de [opslagplaats Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues)bestand. Zorg ervoor dat de titel van uw probleem met het voorvoegsel ' Search SDK: ".

Dank u voor het gebruik van Azure Search!

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Aanhangsel: Stappen om te upgraden naar versie 1.1 

> [AZURE.NOTE] In deze sectie geldt alleen voor gebruikers van de versie van Azure Search .NET SDK 0,13 preview en ouder.

Eerst bijwerken NuGet referentie voor `Microsoft.Azure.Search` de NuGet Package Manager-console of door met de rechtermuisknop op het project verwijst en selecteren 'Beheren van de pakketten NuGet...' in Visual Studio.

Als NuGet is gedownload, nieuwe pakketten en bijbehorende afhankelijkheden, opnieuw opbouwen van uw project.

Als u eerder versie 1.0.0-preview, 1.0.1-preview of 1.0.2-preview, moet de build slagen en u bent klaar om te gaan!

Als u eerder versie 0.13.0-preview of ouder is, ziet u fouten als volgt maken:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

De volgende stap is het bouwen fouten één voor één te herstellen. De meeste moet wijzigen, sommige klasse- en namen die in de SDK hebt gekregen. [Lijst van de wijzigingen in versie 1.1](#ListOfChangesV1) bevat een lijst met deze naamswijzigingen.

Als u aangepaste klassen gebruikt als model voor uw documenten en die klassen eigenschappen van primitieve typen niet-null-waarden bevatten (bijvoorbeeld, `int` of `bool` in C#), er is een bug-fix in de 1.1 versie van de SDK die u moet zich bewust zijn. Zie [correcties in versie 1.1](#BugFixesV1) voor meer informatie.

Ten slotte, nadat u hebt verholpen fouten maken, u kunt wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functies als u wilt.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Lijst van de wijzigingen in versie 1.1

De volgende lijst besteld door de kans dat de wijziging van invloed op de code van uw toepassing.

#### <a name="indexbatch-and-indexaction-changes"></a>Wijzigingen in de IndexBatch en IndexAction

`IndexBatch.Create`is gewijzigd in `IndexBatch.New` en is niet langer een `params` argument. U kunt `IndexBatch.New` dat de partijen die het mengen van verschillende soorten acties (samenvoegen, verwijderen, enz.). Bovendien, er zijn nieuwe statische methoden voor het maken van batches waar alle acties hetzelfde zijn: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`.

`IndexAction`Openbare opbouwfuncties niet meer is en de eigenschappen zijn nu onveranderbaar. U moet de nieuwe statische methoden gebruiken voor het maken van acties voor verschillende doeleinden: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`. `IndexAction.Create`is verwijderd. Als u de overbelasting die alleen een document wordt gebruikt, moet u `Upload` in plaats daarvan.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

U kunt deze wijzigen deze build fouten te verhelpen:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Als u wilt, kunt u deze verder aan deze vereenvoudigen:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>Wijzigingen IndexBatchException

De `IndexBatchException.IndexResponse` eigenschap is gewijzigd in `IndexingResults`, en is nu het type `IList<IndexingResult>`.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

U kunt deze wijzigen deze build fouten te verhelpen:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Bewerking methode wijzigingen

Elke bewerking in de Azure Search .NET SDK is beschikbaar als een set van methode overbelastingen voor synchrone en asynchrone bellers. De handtekeningen en factoring van deze methode overbelastingen is in versie 1.1 gewijzigd.

De bewerking "Get Index Statistics" in oudere versies van de SDK blootgesteld bijvoorbeeld deze handtekeningen:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

De handtekeningen voor dezelfde bewerking in versie 1.1 ziet er zo uit:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Vanaf versie 1.1, organiseert .NET SDK Azure Search bewerking methoden anders:

 - Optionele parameters zijn nu gemodelleerd als standaard parameters liever dan extra overbelastingen. Dit vermindert het aantal methode overbelastingen, soms aanzienlijk.
 - De uitbreidingsmethoden verbergen nu veel van de overbodige details van HTTP van de beller. Bijvoorbeeld: oudere versies van de SDK geretourneerd voor een response-object met een HTTP-statuscode u vaak niet controleren moet omdat de bewerking methoden throw `CloudException` voor elke statuscode die een fout aangeeft. De nieuwe uitbreidingsmethoden u kunt modelobjecten, zodat u het niet meer hoeft uit te pakken ze in uw code.
 - De kern interfaces daarentegen nu expose methoden waarmee u meer controle op het niveau van de HTTP-als u deze nodig hebt. U kunt nu doorgeven in aangepaste HTTP-headers worden opgenomen in de aanvragen en de nieuwe `AzureOperationResponse<T>` retourtype biedt u directe toegang tot de `HttpRequestMessage` en `HttpResponseMessage` voor de bewerking. `AzureOperationResponse`is gedefinieerd in de `Microsoft.Rest.Azure` naamruimte en vervangt `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>Wijzigingen ScoringParameters

Een nieuwe klasse met de naam `ScoringParameter` is toegevoegd in de nieuwste SDK gemakkelijker om parameters voor het scoren van de profielen in een zoekopdracht. Eerder de `ScoringProfiles` eigenschap van de `SearchParameters` klasse is ingevoerd als `IList<string>`; Nu deze wordt getypt als `IList<ScoringParameter>`.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

U kunt deze wijzigen deze build fouten te verhelpen: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Wijzigingen in het objectmodel klasse

Als gevolg van de handtekening wordt beschreven in [methode bewerking wijzigt](#OperationMethodChanges), veel klassen in de `Microsoft.Azure.Search.Models` naamruimte hebt hernoemd of verwijderd. Bijvoorbeeld:

 - `IndexDefinitionResponse`is vervangen door`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`is gewijzigd in`DocumentSearchResult`
 - `IndexResult`is gewijzigd in`IndexingResult`
 - `Documents.Count()`nu geeft als resultaat een `long` met het aantal documenten in plaats van een`DocumentCountResponse`
 - `IndexGetStatisticsResponse`is gewijzigd in`IndexGetStatisticsResult`
 - `IndexListResponse`is gewijzigd in`IndexListResult`

Om samen te vatten, `OperationResponse`-afgeleide klassen die bestonden alleen laten teruglopen, een object model zijn verwijderd. De overige klassen hebben gehad hun achtervoegsel gewijzigd van `Response` op `Result`.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

U kunt deze wijzigen deze build fouten te verhelpen:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Antwoord klassen en IEnumerable

Een extra wijziging die van invloed kan zijn op uw code is antwoord klassen waarin verzamelingen niet langer implementeren `IEnumerable<T>`. In plaats daarvan kunt u de eigenschap collection rechtstreeks benaderen. Bijvoorbeeld, als uw code ziet er als volgt:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

U kunt deze wijzigen deze build fouten te verhelpen:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Belangrijke opmerking voor webtoepassingen

Als u een webtoepassing die serialiseert `DocumentSearchResponse` direct zoekresultaten naar de browser verzenden, moet u uw code te wijzigen of de resultaten worden niet correct geconverteerd. Bijvoorbeeld, als uw code ziet er als volgt:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

U kunt dit wijzigen door de `.Results` eigenschap van het antwoord van de zoekactie te zoeken resultaat weergave herstellen:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

U moet zoeken voor dergelijke gevallen in de code zelf; **De compiler wordt u niet gewaarschuwd** omdat `JsonResult.Data` van het type `object`.

#### <a name="cloudexception-changes"></a>Wijzigingen CloudException

De `CloudException` klasse heeft verplaatst van de `Hyak.Common` naamruimte aan de `Microsoft.Rest.Azure` naamruimte. Ook de `Error` eigenschap is gewijzigd in `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Wijzigingen in de SearchServiceClient en SearchIndexClient

Het type van de `Credentials` eigenschap is gewijzigd van `SearchCredentials` aan de basisklasse `ServiceClientCredentials`. Als u toegang wilt tot de `SearchCredentials` van een `SearchIndexClient` of `SearchServiceClient`, gebruikt u de nieuwe `SearchCredentials` eigenschap.

In oudere versies van de SDK, `SearchServiceClient` en `SearchIndexClient` had constructors die nam een `HttpClient` parameter. Deze zijn vervangen door constructors die een `HttpClientHandler` en een reeks `DelegatingHandler` objecten. Hierdoor kunt gemakkelijker aangepaste handlers voor het verwerken van HTTP-verzoeken indien nodig vooraf installeren.

Ten slotte de constructors die nam een `Uri` en `SearchCredentials` zijn gewijzigd. Bijvoorbeeld als u code die er als volgt uit:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

U kunt deze wijzigen deze build fouten te verhelpen:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ook de opmerking dat het type van de parameter referenties is gewijzigd in `ServiceClientCredentials`. Dit is waarschijnlijk niet van invloed zijn op uw code sinds `SearchCredentials` is afgeleid van `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Een aanvraag-ID wordt doorgegeven.

In oudere versies van de SDK, kan u een aanvraag-ID instellen op de `SearchServiceClient` of `SearchIndexClient` en zou worden opgenomen in elke aanvraag naar de REST API. Dit is handig voor het oplossen van problemen met uw zoekservice als u contact op met support. Het is echter gemakkelijker te gebruiken voor het instellen van een aanvraag voor de unieke ID voor elke bewerking in plaats van dezelfde ID gebruiken voor alle bewerkingen. Om deze reden, de `SetClientRequestId` methoden van `SearchServiceClient` en `SearchIndexClient` zijn verwijderd. In plaats daarvan kunt u een aanvraag-ID voor elke bewerkingsmethode doorgeven via de optionele `SearchRequestOptions` parameter.

> [AZURE.NOTE] In een toekomstige versie van de SDK, zullen we een nieuw mechanisme voor het instellen van een aanvraag-ID globaal op de client objecten is consistent met de benadering die wordt gebruikt door andere Azure SDK's toevoegen.

#### <a name="example"></a>Voorbeeld

Als u code die er als volgt uit:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

U kunt deze wijzigen deze build fouten te verhelpen:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Naamswijzigingen in de gebruikersinterface

De bewerking interfacenamen hebt gewijzigd zodat deze consistent zijn met de namen van hun overeenkomstige eigenschappen:

 - Het type `ISearchServiceClient.Indexes` de naam is gewijzigd van `IIndexOperations` op `IIndexesOperations`.
 - Het type `ISearchServiceClient.Indexers` de naam is gewijzigd van `IIndexerOperations` op `IIndexersOperations`.
 - Het type `ISearchServiceClient.DataSources` de naam is gewijzigd van `IDataSourceOperations` op `IDataSourcesOperations`.
 - Het type `ISearchIndexClient.Documents` de naam is gewijzigd van `IDocumentOperations` op `IDocumentsOperations`.

Deze wijziging is waarschijnlijk geen invloed op uw code tenzij u mocks van deze interfaces voor testdoeleinden gemaakt.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Correcties in versie 1.1

Er is een bug in oudere versies van de Azure Search .NET SDK met betrekking tot serialisatie van aangepaste model klassen. De fout kan optreden als u een aangepast model klasse met een eigenschap van een waardetype niet null gemaakt is.

#### <a name="steps-to-reproduce"></a>Stappen te reproduceren

Maak een aangepast model klasse met een eigenschap van het waardetype niet null. Toevoegen, bijvoorbeeld een openbare `UnitCount` eigenschap van het type `int` in plaats van `int?`.

Als u een document met de standaardwaarde van dit type index (bijvoorbeeld 0 voor `int`), het veld is leeg in Azure zoeken. Als u vervolgens voor het document zoeken, de `Search` oproep genereert `JsonSerializationException` klagen dat het niet kan converteren `null` naar `int`.

Ook filters werken mogelijk niet zoals verwacht omdat null is geschreven naar de index niet de juiste waarde.

#### <a name="fix-details"></a>Gegevens herstellen

We hebben dit probleem opgelost in versie 1.1 van de SDK. Nu hebt u een klasse model als volgt:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

en stelt u `IntValue` op 0, wordt die waarde is nu juist geserialiseerd als 0 op de kabel en opgeslagen als 0 in de index. Ophalen ook werkt zoals verwacht.

Er is een probleem met deze aanpak houden: als u een modeltype met een eigenschap niet null gebruikt, u moet **garanderen** dat er geen documenten in de index voor het betreffende veld een null-waarde bevatten. De SDK noch de Azure Search REST API kunt u dit afdwingen.

Dit is geen hypothetische belang: Stel een scenario waarin u een nieuw veld toevoegen aan een bestaande index die is van het type `Edm.Int32`. Na het bijwerken van de definitie van de index, hebben alle documenten van een null-waarde voor het nieuwe veld (Aangezien alle typen null-waarden in Azure zoeken). Als u vervolgens met een klasse model niet null `int` eigenschap voor het veld, ontvangt u een `JsonSerializationException` als volgt bij het ophalen van documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Om deze reden nog steeds raadzaam gebruik te typen in uw model klassen beste.

Zie voor meer informatie over deze fout en de correctie, [Dit probleem op GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
