<properties
    pageTitle="Off line synchronisatie inschakelen voor uw Azure Mobile App (iOS Xamarin)"
    description="Informatie over het gebruik van App Service Mobile App voor off line cache en synchronisatie van gegevens in uw toepassing Xamarin iOS"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Off line synchronisatie van uw mobiele app Xamarin.iOS inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie bevat de functie Off line synchronisatie van mobiele Apps in Azure voor Xamarin.iOS. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app--weergeven, toevoegen of wijzigen van gegevens, zelfs als er geen netwerkverbinding is. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

In deze zelfstudie, werken de Xamarin.iOS app-project vanaf [een iOS-app van Xamarin maken] ter ondersteuning van de off line functies van mobiele Apps in Azure. Als u niet het project van de server gedownload snel starten, moet u de data access-extensie pakketten toevoegen aan uw project. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie over server-extensie.

Zie voor meer informatie over de functie Off line synchronisatie, het onderwerp [Offline gegevens synchroniseren in Azure Mobile Apps].

## <a name="update-the-client-app-to-support-offline-features"></a>De clienttoepassing functies off line werken

Azure kunt Mobile App offline u interactief werken met een lokale database als u een off line scenario. Als u deze functies in uw app, een [SyncContext] aan een lokale winkel te initialiseren. Verwijzen naar de tabel via de interface [IMobileServiceSyncTable]. SQLite wordt gebruikt als de lokale opslagruimte op het apparaat.

1. De NuGet package manager in het project die u hebt uitgevoerd in de zelfstudie [maken een Xamarin iOS-app] openen en vervolgens zoeken naar en installeer het pakket NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

2. Open het bestand QSTodoService.cs en verwijder de opmerkingen van de `#define OFFLINE_SYNC_ENABLED` definitie.

3. Opnieuw opbouwen en de clienttoepassing uitvoeren. De app werkt hetzelfde als toen u de off line synchronisatie ingeschakeld. Echter, de lokale database is nu gevuld met gegevens die kunnen worden gebruikt in een off line scenario.

## <a name="update-sync"></a>Het loskoppelen van de back-end-toepassingen bijwerken

In dit gedeelte vindt verbreken u de verbinding naar uw mobiele App back-end voor het simuleren van een off line situatie. Wanneer u gegevens toevoegt, de uitzonderingshandler aangeeft dat de app in de off line modus. In deze status, nieuwe items toegevoegd in het lokale archief en wordt gesynchroniseerd met de mobiele app-end als push stand wordt uitgevoerd.

1. QSToDoService.cs in het gedeelde project bewerken. De **applicationURL** om te verwijzen naar een ongeldige URL wijzigen:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    U kunt ook off line gedrag aantonen door Wi-Fi en mobiele netwerken op het apparaat uit te schakelen of vliegtuig-modus.

2. Bouwen en uitvoeren van de app. U ziet de synchronisatie is mislukt bij vernieuwen wanneer de toepassing wordt gestart.

3. Nieuwe items en u ziet dat push niet werkt en een status [CancelledByNetworkError] telkens wanneer die u op **Opslaan**klikt. De nieuwe todo-items zijn echter in het lokale archief totdat ze op de mobiele app backend kunnen worden geduwd.  In een productie-app als u deze uitzonderingen onderdrukken de clienttoepassing gedraagt zich alsof nog steeds met de mobiele app-end verbonden is.

4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Als u Visual Studio geïnstalleerd op een PC hebt, opent u **Server Explorer**. Ga naar de database in **Azure**-> **SQL-Databases**. Klik met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar een SQL-databasetabel en de inhoud ervan. Controleer of de gegevens in de back-end-database is niet gewijzigd.

6. (Optioneel) Uw mobiele back-end, met een GET-query in het formulier een query met een REST-hulpprogramma zoals Fiddler of Postman `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update de app opnieuw verbinden uw mobiele App-backend

In deze sectie, sluit de app op de mobiele app backend. Het verplaatsen van de status off line naar on line zijn met de mobiele app-end app simuleert.   Als u het netwerk breuk gesimuleerd door het uitschakelen van verbinding met het netwerk, er zijn geen codewijzigingen nodig.
Schakel in het netwerk opnieuw.  Tijdens het uitvoeren van de toepassing, de `RefreshDataAsync` methode wordt aangeroepen. Dit op zijn beurt roept `SyncAsync` voor synchronisatie van uw lokale winkel met de backend-database.

1. QSToDoService.cs in het gedeelde project openen en de wijziging van de eigenschap **applicationURL** ongedaan maken.

2. Opnieuw opbouwen en de toepassing uitvoeren. De app wordt de lokale wijzigingen gesynchroniseerd met de mobiele App van Azure end met push- en pull-bewerkingen wanneer het `OnRefreshItemsSelected` methode wordt uitgevoerd.

3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server-Object Explorer of een hulpprogramma als Fiddler voor REST bekijken. Mededeling van de gegevens is tussen de Azure Mobile App backend-database en het lokale archief gesynchroniseerd.

4. In de app, klikt u op het selectievakje naast een paar items uit te voeren in het lokale archief.

  `CompleteItemAsync`oproepen `SyncAsync` item met de mobiele App-end sync elk voltooid. `SyncAsync`push- en pull roept.
  **Wanneer u een pull ten opzichte van een tabel die de client is gewijzigd, worden uitgevoerd op de clientcontext sync push is altijd het eerst uitgevoerd automatisch**. De impliciete push zorgt voor dat alle tabellen in het lokale archief en relaties consistent blijven. Zie voor meer informatie over dit probleem, [Off line gegevens synchroniseren in Azure Mobile Apps].

## <a name="review-the-client-sync-code"></a>Bekijk de clientcode sync

Het Xamarin client-project dat u hebt gedownload wanneer u de zelfstudie [maken een iOS-app van Xamarin] al voltooid bevat code die off line synchronisatie met behulp van een lokale SQLite database ondersteunt. Hier volgt een kort overzicht van wat al is opgenomen in de code van de zelfstudie. Zie voor een overzicht van de functie [Off line gegevens synchroniseren in Azure Mobile Apps].

* Voordat u een tabelbewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. De lokale database wordt geïnitialiseerd wanneer `QSTodoListViewController.ViewDidLoad()` voert `QSTodoService.InitializeStoreAsync()`. Deze methode maakt een nieuwe lokale SQLite database met behulp van de `MobileServiceSQLiteStore` class van de mobiele App van Azure client SDK.

    De `DefineTable` methode maakt u een tabel in het lokale archief dat overeenkomt met de velden in het opgegeven type, `ToDoItem` in dit geval. Het type moet niet alle kolommen die in de externe database. Het is mogelijk om alleen een subset van de kolommen opgeslagen.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }


* De `todoTable` lid van `QSTodoService` is van de `IMobileServiceSyncTable` typt in plaats van `IMobileServiceTable`. De IMobileServiceSyncTable zorgt ervoor dat alle maken, lezen, bijwerken en verwijderen van bewerkingen (CRUD) naar de lokale database.

    U besluit wanneer deze wijzigingen worden doorgegeven op de backend Azure Mobile App door het aanroepen van `IMobileServiceSyncContext.PushAsync()`. De sync-context kunt u relaties tussen tabellen behouden door het bijhouden en het verleggen van wijzigingen in alle tabellen die een clienttoepassing heeft gewijzigd als `PushAsync` wordt genoemd.

    De opgegeven code roept `QSTodoService.SyncAsync()` om te synchroniseren telkens wanneer u de lijst met todoitem wordt vernieuwd of als een todoitem wordt toegevoegd of wordt voltooid. De app wordt na elke lokale wijziging gesynchroniseerd. Als een pull is uitgevoerd op een tabel die nog in behandeling zijnde lokale updates bijgehouden door de context, wordt die pullbewerking automatisch geactiveerd een push context eerst.

    In de opgegeven code, die alle records in de externe `TodoItem` tabel gevraagd, maar het is ook mogelijk om records filteren door een query-id en een query naar `PushAsync`. Zie voor meer informatie de sectie *Incrementele synchronisatie van* [off line gegevens]gesynchroniseerd in Azure Mobile Apps.

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }


## <a name="additional-resources"></a>Aanvullende bronnen

* [Synchronisatie van off line gegevens in Azure mobiele Apps]
* [Azure mobiele Apps .NET SDK-procedure][8]

<!-- Images -->

<!-- URLs. -->
[Maak een iOS-app van Xamarin]: app-service-mobile-xamarin-ios-get-started.md
[Synchronisatie van off line gegevens in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md