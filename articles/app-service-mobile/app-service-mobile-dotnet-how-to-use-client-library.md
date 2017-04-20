<properties
    pageTitle="Werken met de bibliotheek App Service Mobile Apps beheerde client (Windows | Xamarin) | Microsoft Azure"
    description="Informatie over het gebruik van een .NET client voor Mobile Apps in Azure App Service met Windows en Xamarin apps."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Het gebruik van beheerde client voor Azure mobiele Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van de bibliotheek beheerde client voor Azure App-mobiele Apps voor Windows en Xamarin apps uitvoeren. Als u nieuwe mobiele Apps, kunt u overwegen de [quickstart Azure Mobile Apps] eerst voltooien[ 1] zelfstudie. In deze handleiding richten we ons op de client-side beheerde SDK. Zie de documentatie van de [SDK voor .NET Server] voor meer informatie over de server-side SDK's voor mobiele Apps,[ 2] of de [Server-SDK Node.js][3].

## <a name="reference-documentation"></a>Documentatie

De documentatie bij de client-SDK bevindt zich hier: [Azure Mobile Apps .NET client verwijzing][4].
Ook vindt u enkele voorbeelden van de client in de [opslagplaats Azure monsters GitHub][5].

## <a name="supported-platforms"></a>Ondersteunde Platforms

Het .NET Platform ondersteunt de volgende platforms:

* Xamarin Android releases voor API 19 tot en met 24 (KitKat via noga)
* Xamarin iOS releases voor iOS 8.0 en hoger
* Universal Windows Platform
* Windows Phone 8.1
* Windows Phone 8.0, met uitzondering van Silverlight-toepassingen

Een webweergave de "server-flow"-verificatie gebruikt voor de gebruikersinterface weergegeven.  Als het apparaat niet kunt presenteren een WebView UI, zijn andere methoden voor verificatie nodig.  Deze SDK is dus niet geschikt voor controle-type of ook beperkte apparaten.

##<a name="setup"></a>Installatie en vereisten

Nemen we aan dat u al gemaakt en gepubliceerd van uw mobiele App back-end-project, dat ten minste één tabel bevat.  In de code die wordt gebruikt in dit onderwerp wordt de tabel met de naam `TodoItem` en heeft de volgende kolommen: `Id`, `Text`, en `Complete`. Deze tabel wordt gemaakt wanneer u [Azure Mobile Apps quickstart] dezelfde tabel.

De bijbehorende getypte client-side-type in C# is de volgende klasse:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

De [JsonPropertyAttribute] [ 6] wordt gebruikt voor het definiëren van de *eigenschapnaam* toewijzing van het clienttype in de tabel.

Als u wilt weten hoe u tabellen maakt in de backend Mobile Apps, Zie de informatie in het [onderwerp van de SDK voor .NET Server] [ 7] of het [onderwerp Node.js Server SDK][8]. Als u uw mobiele App backend in Azure portal met de QuickStart hebt gemaakt, kun je de instelling **eenvoudig tabellen** in de [portal Azure].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Procedure: het beheerde client SDK-pakket installeren

Gebruik een van de volgende methoden de beheerde client SDK-pakket installeren voor mobiele Apps van [NuGet][9]:

+ **Visual Studio** Klik met de rechtermuisknop op het project, klikt u op **Beheren NuGet pakketten**, zoekt de `Microsoft.Azure.Mobile.Client` pakket en klik vervolgens op **installeren**.

+ **Xamarin Studio** Klik met de rechtermuisknop op het project, klikt u op **Add** > **NuGet pakketten toevoegen**, zoeken naar de `Microsoft.Azure.Mobile.Client `pakket en klik vervolgens op **Pakket toevoegen**.

Houd er rekening mee in uw bestand hoofdactiviteit toevoegen met de volgende instructie met **behulp van** :

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Procedure: werken met symbolen voor foutopsporing in Visual Studio

De symbolen voor de naamruimte Microsoft.Azure.Mobile zijn beschikbaar op [SymbolSource][10].  Raadpleeg de [instructies van de SymbolSource] [ 11] SymbolSource integratie met Visual Studio.

##<a name="create-client"></a>De client voor mobiele Apps maken

De volgende code wordt de [MobileServiceClient] [ 12] -object dat wordt gebruikt voor toegang tot uw backend Mobile App.

    var client = new MobileServiceClient("MOBILE_APP_URL");

In de voorgaande code vervangen `MOBILE_APP_URL` met de URL van de backend Mobile App, die is gevonden in het blad voor uw mobiele App back-end in de [portal Azure]. Het MobileServiceClient-object moet een singleton.

## <a name="work-with-tables"></a>Werken met tabellen

In het volgende gedeelte wordt gedetailleerd beschreven hoe om te zoeken en ophalen van records en de gegevens in de tabel wijzigen.  De volgende onderwerpen komen aan bod:

* [Een tabel maken](#instantiating)
* [Gegevens opvragen](#querying)
* [Gegevens filteren](#filtering)
* [Gegevens sorteren](#sorting)
* [Gegevens weergeven op pagina 's](#paging)
* [Bepaalde kolommen selecteren](#selecting)
* [Een record door het Id zoeken](#lookingup)
* [Omgaan met zonder type query 's](#untypedqueries)
* [Gegevens invoegen](#inserting)
* [Bijwerken van gegevens](#updating)
* [Gegevens verwijderen](#deleting)
* [Conflictoplossing en optimistische gelijktijdigheid](#optimisticconcurrency)
* [Binden aan een Windows-gebruikersinterface](#binding)
* [Het paginaformaat wijzigen](#pagesize)

###<a name="instantiating"></a>Procedure: een tabelverwijzing maken

De code die wordt benaderd of gegevens wijzigt in de tabel van een back-end functies aanroept op de `MobileServiceTable` object. Een verwijzing naar de tabel verkrijgen door het aanroepen van de methode [GetTable] als volgt:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Het geretourneerde object gebruikmaakt van het getypte serialisatie. Een model zonder type serialisatie wordt ook ondersteund. Het volgende voorbeeld [wordt een verwijzing naar een tabel zonder typen]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

In query's zonder type, moet u de onderliggende query OData-tekenreeks.

###<a name="querying"></a>Procedure: gegevens van uw mobiele App opvragen

In deze sectie wordt beschreven hoe u query's op de backend Mobile App, inclusief de volgende functionaliteit:

- [Gegevens filteren](#filtering)
- [Gegevens sorteren](#sorting)
- [Gegevens weergeven op pagina 's](#paging)
- [Bepaalde kolommen selecteren](#selecting)
- [Gegevens door-ID opzoeken](#lookingup)

>[AZURE.NOTE]Een server op basis van beschikbare resources paginaformaat wordt toegepast om te voorkomen dat alle rijen worden geretourneerd.  Paginering standaard aanvragen voor grote gegevenssets, blijft de service nadelig beïnvloeden.  U kunt meer dan 50 rijen gebruiken de `Skip` en `Take` methode, zoals beschreven in [gegevens weergeven op pagina's].

###<a name="filtering"></a>Procedure: Filter gegevens geretourneerd

De volgende code ziet u hoe u gegevens filteren met inbegrip van een `Where` -component van een query. Het resultaat van alle items uit `todoTable` met `Complete` eigenschap is gelijk aan `false`. De functie [waar] van toepassing is een rij predikaat in om de query uit op de tabel te filteren.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

U kunt de URI van de aanvraag verzonden naar de back-end met bericht inspectie software, zoals browser ontwikkelprogramma's of [Fiddler]weergeven. Als u de aanvraag-URI bekijkt, ziet u dat de query-tekenreeks wordt gewijzigd:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

Deze OData-aanvraag wordt door de Server SDK omgezet in een SQL-query:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

De functie die wordt doorgegeven aan de `Where` methode kan een willekeurig aantal voorwaarden hebben.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

In dit voorbeeld zou worden vertaald in een SQL-query op de Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Deze query kan ook worden gesplitst in meerdere componenten:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

De twee methoden gelijkwaardig zijn en door elkaar worden gebruikt.  De eerste optie&mdash;van het samenvoegen van meerdere predikaten in één query&mdash;is compact en aanbevolen.

De `Where` component ondersteunt de bewerkingen die worden omgezet in de subset OData. Bewerkingen zijn onder meer:

* Relationele operators (==,! =, <, < =, >, > =),
* Rekenkundige operators (+, -, /, *, %),
* Numerieke precisie (Math.Floor, Math.Ceiling)
* Tekenreeks-functies (lengte, subtekenreeks, vervangen, IndexOf, StartsWith, EndsWith)
* Datumeigenschappen (jaar, maand, dag, uur, minuut, seconde)
* Toegang tot de eigenschappen van een object, en
* Expressies die een van deze bewerkingen te combineren.

Wanneer u wat de Server SDK ondersteunt, kunt u de [documentatie van de OData-v3]kunt overwegen.

###<a name="sorting"></a>Procedure: sorteren, gegevens geretourneerd

De volgende code ziet u hoe gegevens sorteren door een [OrderBy] of [OrderByDescending] -functie opneemt in de query. Het resultaat van items uit `todoTable` door oplopend gesorteerd de `Text` veld.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Procedure: gegevens ophalen in pagina's

De backend wordt standaard alleen de eerste 50 rijen. U kunt het aantal geretourneerde rijen vergroten door het aanroepen van de methode [nemen] . Gebruik `Take` in combinatie met de methode [overslaan] voor het aanvragen van een specifieke 'pagina' van het totale dataset die door de query. De volgende query wanneer uitgevoerd, retourneert de drie belangrijkste items in de tabel.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

De volgende gewijzigde query slaat de resultaten van de eerste drie en de volgende drie resultaten geretourneerd. De tweede 'pagina' van gegevens, waarbij het formaat van de drie items is, levert deze query.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

De methode [IncludeTotalCount] verzoekt het totale aantal voor _alle_ records die zou zijn geretourneerd, een wisselbestand/limiet component opgegeven wordt genegeerd:

    query = query.IncludeTotalCount();

In een echte wereld-app kunt u query's zoals in het voorgaande voorbeeld met een pagineringsbesturingselement of een vergelijkbare UI kunt navigeren tussen pagina's.

>[AZURE.NOTE]De limiet van 50-rij in een mobiele App backend overschrijven, moet u ook de [EnableQueryAttribute] van toepassing op de openbare GET-methode en de werking gepagineerde opgeven. Wanneer de methode wordt toegepast, wordt de volgende het maximum aantal geretourneerde rijen op 1000:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Procedure: specifieke kolommen selecteren

U kunt opgeven welke set eigenschappen op te nemen in de resultaten door een component [selecteert u] aan de query toe te voegen. De volgende code toont bijvoorbeeld hoe u kunt slechts één veld selecteren en ook selecteren en opmaken van meerdere velden:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Alle tot nu toe beschreven functies zijn toevoegingsmiddel, zodat we kunnen houden chaining ze. Elke aanroep van aaneengeschakelde is van invloed op meer van de query. Één meer voorbeeld:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Procedure: gegevens door-ID opzoeken

De functie [LookupAsync] kan worden gebruikt om objecten opzoeken in de database met een bepaalde ID.

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Procedure: zonder type query's uitvoeren

Bij het uitvoeren van een query op basis van een zonder type tabelobject moet expliciet de OData-queryreeks door het aanroepen van [ReadAsync], zoals in het volgende voorbeeld:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

U terughalen JSON-waarden die u als een zak eigenschap gebruiken kunt. Zie voor meer informatie over JToken en Newtonsoft Json.NET, de site [Json.NET] .

### <a name="inserting"></a>Procedure: gegevens invoegen in een mobiele App backend.

Alle clienttypen moeten een lid met de naam **Id**, is standaard een tekenreeks bevatten. Deze **Id** is vereist voor het uitvoeren van CRUD en voor off line synchronisatie. De volgende code ziet u hoe de methode [InsertAsync] gebruikt om nieuwe rijen in een tabel invoegen. De parameter bevat de gegevens moeten worden ingevoegd als een .NET-object.

    await todoTable.InsertAsync(todoItem);

Als een unieke aangepaste id-waarde is niet opgenomen in de `todoItem` tijdens een invoeging, een GUID wordt gegenereerd door de server.
Met een controle op het object nadat de aanroep is geretourneerd, kunt u de gegenereerde Id ophalen.

Zonder type als gegevens wilt invoegen, kunt u profiteren van Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Hier volgt een voorbeeld van een e-mailadres als een unieke tekenreeks-id:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Werken met id-waarden

Mobiele Apps ondersteunt unieke aangepaste string-waarden voor de kolom **-id** van de tabel. Een string-waarde kan toepassingen gebruik aangepaste waarden zoals e-mailadressen of gebruikersnamen voor de ID.  Tekenreeks-id's bieden de volgende voordelen:

* Id's worden gegenereerd zonder een retour in de database aanbrengen.
* Records zijn samenvoegen uit verschillende tabellen of databases gemakkelijker.
* Id-waarden kunnen beter worden geïntegreerd met de logica van de toepassing.

Als een tekenreeks-ID-waarde niet is ingesteld op een ingevoegde record, de mobiele App-end een unieke waarde genereert voor de ID. U kunt de methode [Guid.NewGuid] voor het genereren van uw eigen id-waarden, op de client of in de backend.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Procedure: gegevens in een backend Mobile App wijzigen

De volgende code ziet u hoe met de methode [UpdateAsync] een bestaande record kunt bijwerken met dezelfde ID met nieuwe informatie. De parameter bevat de gegevens moeten worden bijgewerkt als .NET-object.

    await todoTable.UpdateAsync(todoItem);

Zonder type als gegevens wilt bijwerken, kan u profiteren van [Json.NET] als volgt:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

Een `id` veld moet worden opgegeven bij het maken van een update. De backend wordt gebruikt de `id` om te bepalen welke rij moet worden bijgewerkt. De `id` veld kan worden verkregen uit het resultaat van de `InsertAsync` aanroepen. Een `ArgumentException` treedt op als u probeert een item bijwerken zonder dat u de `id` waarde.

###<a name="deleting"></a>Procedure: gegevens verwijderen uit een backend mobiele App

De volgende code ziet u hoe met de methode [DeleteAsync] kunt verwijderen van een bestaand exemplaar. De instantie wordt geïdentificeerd door de `id` veld is ingesteld op de `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Zonder type om gegevens te verwijderen, kunt u profiteren van Json.NET als volgt:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Wanneer u een verzoek tot verwijdering, moet een ID worden opgegeven. Andere eigenschappen worden niet doorgegeven aan de service of op de service worden genegeerd. Het resultaat van een `DeleteAsync` oproep is meestal `null`. De ID op te geven kunt u vinden in het resultaat van de `InsertAsync` aanroepen. A `MobileServiceInvalidOperationException` wordt gegenereerd wanneer u probeert een item verwijderen zonder de `id` veld.

###<a name="optimisticconcurrency"></a>Procedure: optimistische gelijktijdigheid voor conflictoplossing gebruiken

Twee of meer clients kunnen schrijven naar hetzelfde item op hetzelfde moment. Zonder detectie van adresconflicten overschrijft de laatste schrijfbewerking alle vorige updates. **Optimistic concurrency control** wordt ervan uitgegaan dat elke transactie doorvoeren kunt en dus geen vergrendeling van een resource gebruikt.  Voordat een transactie wordt doorgevoerd, OCC. wordt gecontroleerd of er geen andere transactie heeft de gegevens gewijzigd. Als de gegevens zijn gewijzigd, de committing transactie teruggedraaid.

Mobiele Apps OCC. ondersteunt door het bijhouden van wijzigingen in elk item met behulp van het `version` systeem de kolom die is gedefinieerd voor elke tabel in de backend Mobile App. Telkens wanneer een record wordt bijgewerkt, Mobile Apps stelt de `version` eigenschap voor die record op een nieuwe waarde. Bij elke aanvraag voor update, de `version` eigenschap van de record die is opgenomen in de aanvraag wordt vergeleken met dezelfde eigenschap voor de record op de server. Als de versie die is doorgegeven aan de aanvraag komt niet overeen met de back-end en vervolgens de clientbibliotheek leidt tot een `MobileServicePreconditionFailedException<T>` uitzondering. Het type dat is opgenomen met de uitzondering is de record uit de back-end met de versie van de servers van de record. De toepassing kan vervolgens deze informatie gebruiken om te bepalen of uitvoeren van de updateaanvraag opnieuw met de juiste `version` waarde van de end wijzigingen doorvoeren.

Definiëren van een kolom in de tabel klasse voor de `version` systeemeigenschap optimistische gelijktijdigheid inschakelen. Bijvoorbeeld:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Toepassingen die gebruikmaken van tabellen zonder type optimistische gelijktijdigheid inschakelen door de `Version` markering op de `SystemProperties` van de tabel als volgt.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Naast het inschakelen van optimistische gelijktijdigheid, u moet ook met een werkelijk de `MobileServicePreconditionFailedException<T>` uitzondering in de code bij het aanroepen van [UpdateAsync].  Het conflict oplossen door het toepassen van de juiste `version` met de bijgewerkte record en [UpdateAsync] gesprek met de omgezette record. De volgende code toont het oplossen van een conflict schrijven eenmaal gevonden:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Zie voor meer informatie het onderwerp [Off line gegevens synchroniseren in Azure Mobile Apps] .

###<a name="binding"></a>Procedure: gegevens naar een Windows-gebruikersinterface Bind mobiele Apps

In deze sectie wordt aangegeven hoe geretourneerde gegevens weergaveobjecten met gebruikersinterface-elementen in een Windows-toepassing.  De volgende voorbeeldcode wordt gebonden aan de bron van de lijst met een query voor het onvolledig items. De [MobileServiceCollection] maakt een mobiele Apps bewust binding-collectie.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Sommige besturingselementen in de beheerde runtime ondersteunen een interface [ISupportIncrementalLoading]aangeroepen. Deze interface kunt besturingselementen voor het aanvragen van extra gegevens wanneer de gebruiker. Er is een ingebouwde ondersteuning voor deze interface voor universal Windows apps via [MobileServiceIncrementalLoadingCollection], die de oproepen van de besturingselementen automatisch worden verwerkt. Gebruik `MobileServiceIncrementalLoadingCollection` in de Windows-apps als volgt:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

De nieuwe collectie van apps voor Windows Phone 8 en "Silverlight", gebruikt de `ToCollection` uitbreidingsmethoden op `IMobileServiceTableQuery<T>` en `IMobileServiceTable<T>`. Aanroepen om gegevens te laden, `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Wanneer gebruikt u de collectie die is gemaakt door het aanroepen van `ToCollectionAsync` of `ToCollection`, krijgt u een collectie die kan worden gebonden aan de UI-besturingselementen.  Deze collectie is pagineren bewust.  Omdat de collectie is laden van gegevens van het netwerk, mislukt soms laden. Voor het verwerken van dergelijke fouten, overschrijven de `OnException` methode op `MobileServiceIncrementalLoadingCollection` voor het verwerken van uitzonderingen ten gevolge van het aanroepen van `LoadMoreItemsAsync`.

U kunt als uw tabel veel velden bevat, maar u wilt alleen sommige van deze weergeven in het besturingselement. U kunt de richtlijnen in de vorige sectie "het[selecteren van specifieke kolommen](#selecting)" selecteren van specifieke kolommen worden weergegeven in de gebruikersinterface.

###<a name="pagesize"></a>Het paginaformaat wijzigen

Azure Mobile Apps retourneert een maximum van 50 objecten per aanvraag standaard.  U kunt de grootte van het wisselbestand wijzigen door de verhoging van de maximale grootte op de client en de server.  Voor het verhogen van de grootte van de aangevraagde pagina, opgeven `PullOptions` bij het gebruik van `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Ervan uitgaande dat u hebt gemaakt de `PageSize` gelijk is aan of groter dan 100 binnen de server een aanvraag maximaal 100 objecten als resultaat geeft.

##<a name="#offlinesync"></a>Werken met off line tabellen

Een lokale SQLite winkel gegevens off line tabellen gebruiken voor gebruik offline.  Alle tabel gebeurt tegen de lokale SQLite opslaan in plaats van het archief van de externe server.  Een off line als tabel wilt maken, moet u eerst uw project voorbereiden:

1. Met de rechtermuisknop op de oplossing in Visual Studio > **NuGet pakketten beheren voor oplossing...**en vervolgens zoeken naar en installeren van het pakket **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet voor alle projecten in de oplossing.

2. (Optioneel) Ter ondersteuning van Windows-apparaten, een van de volgende SQLite runtime-pakketten te installeren:

    * **Windows 8.1 Runtime:** [SQLite voor Windows 8.1]installeren[3].
    * **Windows Phone 8.1:** [SQLite voor Windows Phone 8.1]installeren[4].
    * **Universal Windows Platform** [SQLite voor het Universal universele Windows]installeren[5].

3. (Optioneel). Apparaten met Windows, klikt u op **verwijzingen** > **Verwijzing toevoegen**, vouw de map **Windows** > **extensies**en vervolgens de juiste **SQLite voor Windows** SDK en de **Visual C++ 2013 Runtime voor Windows** SDK inschakelen.
    De SQLite SDK namen variëren enigszins met elke Windows-platform.

Voordat u de verwijzing naar een tabel kunt maken, moet het lokale archief worden bereid:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Winkel initialisatie gebeurt normaal gesproken onmiddellijk nadat de client is gemaakt.  De **OfflineDbPath** moet een bestandsnaam die geschikt zijn voor gebruik op alle platforms die u ondersteunt.  Als het pad een volledig pad is (dat wil zeggen, begint met een slash), wordt dat pad gebruikt.  Als het pad is niet volledig gekwalificeerd, wordt het bestand in een platform-specifieke locatie geplaatst.

* Voor iOS en Android apparaten is het standaardpad voor de map 'Persoonlijke bestanden'.
* Het standaardpad is de toepassingsspecifieke "AppData" map voor Windows-apparaten.

De verwijzing naar een tabel kan worden verkregen met behulp van de `GetSyncTable<>` methode:

    var table = client.GetSyncTable<TodoItem>();

U hoeft niet te verifiëren als een tabel off line wilt gebruiken.  U hoeft alleen te verifiëren wanneer u met de backend-service communiceert.

###<a name="syncoffline"></a>Een tabel off line synchroniseren

Off line tabellen worden niet gesynchroniseerd met de back-end standaard.  Synchronisatie wordt in twee delen gesplitst.  U kunt wijzigingen afzonderlijk push nieuwe items worden gedownload.  Dit is een typische synchronisatiemethode:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
                "allTodoItems",
                this.todoTable.CreateQuery());
        }
        catch (MobileServicePushFailedException exc)
        {
            if (exc.PushResult != null)
            {
                syncErrors = exc.PushResult.Errors;
            }
        }

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
        if (syncErrors != null)
        {
            foreach (var error in syncErrors)
            {
                if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                {
                    //Update failed, reverting to server's copy.
                    await error.CancelAndUpdateItemAsync(error.Result);
                }
                else
                {
                    // Discard local change.
                    await error.CancelAndDiscardItemAsync();
                }

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Als het eerste argument bij `PullAsync` is null, incrementele synchronisatie wordt niet gebruikt.  Haalt alle records op elke synchronisatiebewerking uit.

De SDK wordt uitgevoerd een impliciete `PushAsync()` voor het ophalen van records.

Conflict verwerking gebeurt op een `PullAsync()` methode.  U kunt omgaan met conflicten op dezelfde manier als on line tabellen.  Het conflict wordt geproduceerd wanneer `PullAsync()` heet in plaats van tijdens het invoegen, bijwerken of verwijderen. Als er meerdere conflicten plaatsvinden, zijn ze gebundeld in een enkele MobileServicePushFailedException.  Elke storing afzonderlijk verwerkt.

##<a name="#customapi"></a>Werken met een aangepaste API

Een aangepaste API kunt u aangepaste eindpunten die functionaliteit die niet worden toegewezen aan een invoegen, bijwerken, verwijderen of leesbewerking definiëren. Met behulp van een aangepaste API kunt hebt u meer controle over messaging, inclusief lezen en HTTP-berichtheaders instellen en definiëren van een berichtindeling lichaam dan JSON.

U kunt een aangepaste API aanroepen door het aanroepen van een van de methoden van de [InvokeApiAsync] op de client. De volgende regel code wordt een POST-aanvraag verzonden naar de **completeAll** API op de backend:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Dit formulier is een getypte methodeaanroep en vereist dat het retourtype **MarkAllResult** is gedefinieerd. Getypte en zonder type methoden worden ondersteund.

##<a name="authentication"></a>Verificatie van gebruikers

Mobiele Apps biedt ondersteuning voor verificatie en autorisatie van app-gebruikers met verschillende externe id-providers: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen voor het beperken van toegang voor specifieke bewerkingen tot alleen geverifieerde gebruikers. U kunt ook de identiteit van geverifieerde gebruikers implementeren verificatieregels in serverscripts. Zie voor meer informatie de zelfstudie voor [verificatie van uw app toevoegen].

Twee stromen voor verificatie worden ondersteund: stroom _beheerde client_ en _server worden beheerd_ . De server beheerd stroom biedt de eenvoudigste verificatie ervaring, dit is afhankelijk van de webinterface voor verificatie van de provider. De managed client stroom staat diepere integratie met apparaatspecifieke mogelijkheden zoals deze is afhankelijk van de provider-specifieke apparaat-specifieke SDK's.

>[AZURE.NOTE] Wij raden u aan met behulp van een beheerde client stroom in uw productie-apps.

Als u verificatie instellen, moet u uw app met een of meer id-providers registreren.  De id-provider genereert een client-ID en een geheim van de client voor uw app.  Deze waarden worden vervolgens ingesteld in uw back-end Azure App Service verificatie/autorisatie inschakelen.  Volg de gedetailleerde aanwijzingen in de zelfstudie voor [verificatie van uw app toevoegen]voor meer informatie.

In deze sectie komen de volgende onderwerpen aan bod:

+ [Managed client verificatie](#clientflow)
+ [Verificatie server beheerd](#serverflow)
+ [De verificatietoken voor caching](#caching)

###<a name="clientflow"></a>Managed client verificatie

Uw app kan onafhankelijk contact op met de identiteitsprovider en geef vervolgens het resulterende token tijdens het aanmelden met uw back-end. Deze client-overdracht kunt u biedt gebruikers een ervaring voor één of meer gegevens ophaalt uit de identiteitsprovider. Clientverificatie stroom heeft de voorkeur voor het gebruik van een stroom van de server als de identiteitsprovider SDK een meer native UX feel biedt en voor extra aanpassingen zorgt.

Voorbeelden zijn beschikbaar voor de volgende client-flow verificatie patronen:

+ [Bibliotheek van Active Directory-verificatie](#adal)
+ [Facebook of Google](#client-facebook)
+ [Live SDK](#client-livesdk)

#### <a name="adal"></a>Verificatie van gebruikers met de Active Directory-verificatie-bibliotheek

U kunt Active Directory verificatie bibliotheek (ADAL) starten verificatie van de client met Azure Active Directory-verificatie.

1. Uw mobiele app back-end voor AAD eenmalige aanmelding configureren aan de hand van de zelfstudie voor [het configureren van App-Service voor Active Directory-aanmelding] . Zorg ervoor dat de optionele stap van het registreren van een native clienttoepassing.
2. Open het project in Visual Studio of Xamarin Studio en een verwijzing toevoegen aan de `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet package. Bij het zoeken zijn voorlopige versies.
3. De volgende code toevoegen aan uw toepassing, afhankelijk van het platform dat u gebruikt. In elk, moet u de volgende vervangingen:

    * **INSERT-hier-instantie** vervangen door de naam van de huurder waarin u uw toepassing hebt ingericht. De https://login.windows.net/contoso.onmicrosoft.com moet worden. Deze waarde kan worden gekopieerd vanaf het tabblad domein in uw Azure Active Directory in de [Azure klassieke portal].
    * **INSERT RESOURCE-ID hier** vervangen door de client-ID voor uw mobiele app backend. Op het tabblad **Geavanceerd** onder **Azure Active Directory-instellingen** in de portal kunt u de client-ID.
    * **INSERT CLIENT-ID hier** vervangen door de client-ID die u hebt gekopieerd uit de native clienttoepassing.
    * **INSERT REDIRECT-URI-hier -** vervangen van uw site _/.auth/login/done_ eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met _https://contoso.azurewebsites.net/.auth/login/done_.

    De code die nodig is voor elk platform volgt:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Eenmalige aanmelding met een token uit Facebook of Google

U kunt de client stroom zoals in dit fragment voor Facebook of Google.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Enkele inloggen met Microsoft Account met de Live SDK

Om gebruikers te verifiëren, moet u uw app op het Microsoft-account Developer Center te registreren. De registratiedetails configureren op uw mobiele App backend. Voer de stappen in [uw app voor het gebruik van een Microsoft-account inloggen registreren]registratie van een Microsoft-account maken en verbinding maken met uw mobiele App backend. Als u zowel Windows Store en Windows Phone 8/Silverlight versies van uw app, de versie te registreren Windows Store eerst.

De volgende code wordt geverifieerd met behulp van Live SDK en het resulterende token gebruikt om aan te melden op de backend Mobile App.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Zie de [SDK van Windows Live] -documentatie voor meer informatie.

###<a name="serverflow"></a>Verificatie server beheerd

Nadat u uw identiteitsprovider hebt geregistreerd, roept u de methode [LoginAsync] op de [MobileServiceClient] met de waarde [MobileServiceAuthenticationProvider] van uw provider. De volgende code start bijvoorbeeld een server stroom aanmelden via Facebook.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Als u een id-provider dan Facebook, de waarde wijzigen van [MobileServiceAuthenticationProvider] aan de waarde van uw provider.

In een stroom server beheert Azure App-Service de verificatie OAuth stroom door de aanmeldingspagina van de geselecteerde provider weer te geven.  Zodra de identity provider retourneert, Azure App Service genereert een verificatietoken App-Service. De methode [LoginAsync] retourneert een [MobileServiceUser], waarmee de [gebruikers-id] van de geverifieerde gebruiker zowel de [MobileServiceAuthenticationToken]als JSON web token (JWT). Dit token kan worden in de cache opgeslagen en opnieuw gebruikt totdat deze verloopt. Zie [de verificatietoken voor Caching](#caching)voor meer informatie.

###<a name="caching"></a>De verificatietoken voor caching

In sommige gevallen worden de aanroep van de methode login na de eerste succesvolle verificatie voorkomen doordat het verificatietoken van de provider.  Windows Store en UWP apps kunt [PasswordVault] als volgt de huidige verificatietoken na een geslaagde aanmelden, in cache:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

De gebruikers-id-waarde wordt opgeslagen als de gebruikersnaam van de referentie- en het token van de opgeslagen als het wachtwoord is. Op de daaropvolgende startende bedrijven, kunt u de **PasswordVault** in de cache opgeslagen referenties te controleren. In het volgende voorbeeld wordt in cache opgeslagen referenties gebruikt wanneer ze worden gevonden, en anders probeert opnieuw te identificeren met de back-end:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Wanneer u een gebruiker uitlogt, moet u de opgeslagen referenties ook als volgt verwijderen:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

[Xamarin.Auth] API's Xamarin apps gebruiken voor veilige opslag van referenties in een **Account** -object. Zie het bestand [AuthStore.cs] in de [ContosoMoments foto delen monster]voor een voorbeeld van het gebruik van deze API's.

Wanneer u een beheerde client verificatie gebruikt, kunt u ook het toegangstoken opgehaald van uw provider zoals Facebook of Twitter cache. Dit token kan worden geleverd om een nieuwe verificatietoken van de back-end als volgt:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Push-meldingen

Push-meldingen hebben betrekking op de volgende onderwerpen:

* [Register voor Push-meldingen](#register-for-push)
* [Verkrijgen van een pakket voor Windows Store SID](#package-sid)
* [Registreren met sjablonen voor verschillende platforms](#register-xplat)

###<a name="register-for-push"></a>Procedure: registreren voor Push-meldingen

De client voor mobiele Apps kunt u registreren voor push-meldingen met Azure melding Hubs. Als u registreert, krijgt u een ingang die u hebt verkregen van de platform-specifieke Push Notification Service (PNS). U geeft deze waarde samen met alle codes bij het maken van de registratie. De volgende code registreert uw Windows app voor push-meldingen met de Windows Notification Service (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Als u naar WNS zijn duwen, moet u [een pakket met Windows Store SID te verkrijgen](#package-sid).  Zie voor meer informatie over Windows-toepassingen, zoals het registreren voor registraties van de sjabloon, [push-meldingen toevoegen aan uw app].

Aanvragen van codes van de client wordt niet ondersteund.  Code aanvragen worden zonder bericht van inschrijving neergezet.
Als u wilt dat uw apparaat registreren met tags, maakt u een aangepaste API die de kennisgeving Hubs API gebruikt voor het uitvoeren van de registratie namens u.  [De aangepaste API aanroepen](#customapi) in plaats van de `RegisterNativeAsync()` methode.

###<a name="package-sid"></a>Procedure: een Windows Store pakket SID te verkrijgen

De SID van een pakket is nodig voor het inschakelen van push-meldingen in de Windows Store apps.  Registreren voor het ontvangen van een pakket SID, de toepassing met de Windows Store.

Als u deze waarde:

1. In Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project, klikt u op **winkel** > **App koppelen met de winkel...**.
2. In de wizard, klikt u op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor uw app in **Reserve een nieuwe naam voor de toepassing**en klik vervolgens op **Reserve**.
3. Nadat de registratie app is gemaakt, selecteert u de naam van de app, klik op **volgende**en klik op **koppelen**.
4. Aanmelden bij [Windows Dev Center] met uw Microsoft-Account. Klik onder **Mijn apps**, de app registratie die u hebt gemaakt.
5. Klik op **beheer App** > **identiteit**en schuif omlaag naar uw **Pakket SID**vinden.

Veel gebruik van het pakket SID Behandel dit als een URI, in welk geval u moet gebruiken _ms-app: / /_ als het schema. Noteer de versie van het pakket SID gevormd door samenvoeging van deze waarde als voorvoegsel.

Xamarin apps vereisen wat extra code kunnen voor het registreren van een app op de iOS- of Android-platforms worden uitgevoerd. Zie voor meer informatie het onderwerp voor uw platform:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Procedure: Register push-sjablonen voor het verzenden van meldingen voor meerdere platforms

Sjablonen registreren, gebruikt u de `RegisterAsync()` methode met de sjablonen als volgt:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

De sjablonen moeten worden `JObject` typt en meerdere sjablonen in de volgende indeling: JSON kan bevatten:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

De methode **RegisterAsync()** accepteert ook secundaire tegels:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Alle codes zijn te vertalen tijdens de registratie voor beveiliging. Zie tags toevoegen aan sjablonen in installaties of installaties, [werken met de back-end-server .NET SDK voor Azure Mobile Apps].

Om meldingen te verzenden met behulp van deze geregistreerde sjablonen, raadpleegt u de [Melding Hubs API's].

##<a name="misc"></a>Diverse onderwerpen

###<a name="errors"></a>Procedure: Fouten afhandelen

Wanneer een fout in de backend optreedt, de client SDK leidt tot een `MobileServiceInvalidOperationException`.  In het volgende voorbeeld ziet u hoe een uitzondering die wordt geretourneerd door de back-end:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Een ander voorbeeld van omgaan met fouten vindt u in het [Voorbeeld van mobiele Apps bestanden]. De [LoggingHandler] is een registratie gemachtigde handler (volgende) om te melden van de aanvragen wordt voor de back-end.

###<a name="headers"></a>Procedure: aanpassen headers aanvragen

Ter ondersteuning van uw specifieke app scenario, moet u mogelijk de communicatie met de back-end mobiele App aanpassen. U wilt bijvoorbeeld een aangepaste header toevoegen aan alle uitgaande aanvragen of antwoorden statuscodes zelfs wijzigen. U kunt een aangepaste [DelegatingHandler], zoals in het volgende voorbeeld:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Verificatie toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Synchronisatie van off line gegevens in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[Push-meldingen toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Uw app voor het gebruik van een Microsoft-account inloggen registreren]: app-service-mobile-how-to-configure-microsoft-authentication.md
[App-Service configureren voor Active Directory-aanmelding]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[Hiermee maakt u een verwijzing naar een tabel zonder type]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[Sorteren op]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Nemen]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Selecteer]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Overslaan]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Gebruikers-id]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Waar]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure portal]: https://portal.azure.com/
[Azure klassieke portal]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Dev Center van Windows]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live-SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Melding Hubs API 's]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobiele toepassingen bestanden monster]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData-v3-documentatie]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[ContosoMoments foto delen monster]: https://github.com/azure-appservice-samples/ContosoMoments