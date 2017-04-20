<properties
    pageTitle="Off line synchronisatie inschakelen voor uw mobiele App in Azure (Xamarin.Forms) | Microsoft Azure"
    description="Informatie over het gebruik van App Service Mobile App voor off line cache en synchronisatie van gegevens in uw toepassing Xamarin.Forms"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Off line synchronisatie van uw mobiele app Xamarin.Forms inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie bevat de functie Off line synchronisatie van mobiele Apps in Azure voor Xamarin.Forms. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app--weergeven, toevoegen of wijzigen van gegevens, zelfs als er geen netwerkverbinding is. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

Deze zelfstudie is gebaseerd op de quickstart Xamarin.Forms oplossing voor mobiele toepassingen die u maakt wanneer u de zelfstudie [maken een iOS-app van Xamarin]. De quickstart-oplossing voor het Xamarin.Forms bevat de code voor de ondersteuning van offline synchroniseren die moet worden ingeschakeld. In deze zelfstudie kunt u de quickstart oplossing in te schakelen op de off line functies van Azure Mobile Apps bijwerken. Ook selecteren we de off line-specifieke code in de app. Als u niet de gedownloade quickstart oplossing, moet u de data access-extensie pakketten toevoegen aan uw project. Zie voor meer informatie over server-extensie [werken met de .NET back-endserver SDK voor Azure Mobile Apps][1].

Zie voor meer informatie over de functie Off line synchronisatie, het onderwerp [Offline gegevens synchroniseren in Azure Mobile Apps][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Synchronisatie van off line functionaliteit in de quickstart-oplossing

De off line Synchronisatiecode is opgenomen in het project met behulp van C# preprocessor-instructies. Wanneer de **OFFLINE\_SYNC\_ENABLED** symbool is gedefinieerd, wordt deze codepaden zijn opgenomen in de build. Voor Windows-toepassingen, moet u ook de SQLite-platform te installeren.

1. Met de rechtermuisknop op de oplossing in Visual Studio > **NuGet pakketten beheren voor oplossing...**en vervolgens zoeken naar en installeren van het pakket **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet voor alle projecten in de oplossing.

2. In de Solution Explorer, opent u het bestand TodoItemManager.cs van het project met **draagbare** in de naam, draagbare Class Library-project is, vervolgens Opmerkingen bij de volgende voorwerkingsinstructie:

        #define OFFLINE_SYNC_ENABLED

4. (Optioneel) Ter ondersteuning van Windows-apparaten, een van de volgende SQLite runtime-pakketten te installeren:

    * **Windows 8.1 Runtime:** [SQLite voor Windows 8.1]installeren[3].
    * **Windows Phone 8.1:** [SQLite voor Windows Phone 8.1]installeren[4].
    * **Universal Windows Platform** [SQLite voor het Universal universele Windows]installeren[5].

    Hoewel de quickstart niet een universele Windows-project bevat, wordt het Universal Windows platform ondersteund met Xamarin formulieren.

5. (Optioneel) In elk Windows-app-project, met de rechtermuisknop op **References** > **Verwijzing toevoegen**, vouw de map **Windows** > **extensies**.
    Schakel het juiste **SQLite voor Windows** SDK en de **Visual C++ 2013 Runtime voor Windows** SDK.
    De SQLite SDK namen variëren enigszins met elke Windows-platform.

## <a name="review-the-client-sync-code"></a>Bekijk de clientcode sync

Hier volgt een kort overzicht van wat al is opgenomen in de zelfstudie-code in de `#if OFFLINE_SYNC_ENABLED` richtlijnen. De synchronisatie van off line-functionaliteit is in het bestand TodoItemManager.cs in de draagbare Class Library-project. Zie voor een overzicht van de functie [Off line gegevens synchroniseren in Azure Mobile Apps][2].

* Voordat u een tabelbewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. De lokale database wordt geïnitialiseerd in de klassenconstructor **TodoItemManager** met de volgende code:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Deze code maakt een nieuwe lokale SQLite database met behulp van de klasse **MobileServiceSQLiteStore** .

    De methode **DefineTable** wordt een tabel gemaakt in het lokale archief dat overeenkomt met de velden in het opgegeven type.  Het type moet niet alle kolommen die in de externe database. Het is mogelijk om een subset van de kolommen opgeslagen.

* Het veld **todoTable** in de **TodoItemManager** is een **IMobileServiceSyncTable** in plaats van **IMobileServiceTable**. Deze klasse gebruikt de lokale database voor alle maken, lezen, bijwerken en verwijderen van bewerkingen (CRUD). U besluit wanneer deze wijzigingen worden op de backend Mobile App geduwd door het aanroepen van **PushAsync** in de **IMobileServiceSyncContext**. De sync-context kunt u relaties tussen tabellen te behouden door het bijhouden en het uploaden van wijzigingen in alle tabellen die een clienttoepassing is gewijzigd wanneer **PushAsync** wordt aangeroepen.

    De volgende **SyncAsync** -methode wordt aangeroepen om te synchroniseren met de mobiele App-end:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
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

            // Simple error/conflict handling.
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

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    In dit voorbeeld maakt gebruik van eenvoudige foutafhandeling met de standaard-handler voor synchronisatie. Een echte toepassing zou de verschillende fouten zoals netwerkomstandigheden en conflicten server verwerken met behulp van een aangepaste implementatie van **IMobileServiceSyncHandler** .

## <a name="offline-sync-considerations"></a>Overwegingen bij het synchroniseren van off line

In het voorbeeld wordt de methode **SyncAsync** alleen genoemd op het opstarten en een synchronisatie wordt aangevraagd.  U start de synchronisatie in een Android of iOS-app, Open in de lijst items; voor Windows, gebruikt u de knop **synchronisatie** . In een concrete toepassing, kan u de trigger synchroniseren wanneer de status van het netwerk verandert.

Als een pull is uitgevoerd op een tabel die in behandeling is lokale updates bijgehouden door de context die een pull-bewerking automatisch triggers een voorafgaande context push. Bij het vernieuwen, toe te voegen en items in dit voorbeeld is voltooid, kunt u de expliciete aanroep van **PushAsync** weglaten.

Alle records in de tabel met externe TodoItem worden doorzocht in de opgegeven code, maar het is ook mogelijk om records te filteren door een query-id en de query doorgegeven aan **PushAsync**. Zie voor meer informatie de sectie *Incrementele synchronisatie van* [off line gegevens]gesynchroniseerd in Azure Mobile Apps[2].

## <a name="run-the-client-app"></a>De clienttoepassing uitvoeren

Ten minste eenmaal uitgevoerd de clienttoepassing op elk platform voor het vullen van de lokale database met off line synchronisatie nu ingeschakeld. Later simuleren van een off line scenario en de gegevens in het lokale archief wijzigen terwijl de app offline is.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Update het sync-gedrag van de clienttoepassing

In deze sectie, het client-project om een off line scenario simuleren met behulp van een ongeldige URL voor uw back-end te wijzigen. U kunt ook netwerkverbindingen uitschakelen door het apparaat verplaatsen naar 'vliegtuig-modus.  Wanneer u gegevensitems toevoegen of wijzigen, zijn deze wijzigingen opgeslagen in het lokale archief, maar niet gesynchroniseerd met de back-end-gegevensopslag totdat de verbinding opnieuw tot stand gebracht.

1. Open het bestand Constants.cs uit de **draagbare** project in de Solution Explorer en wijzig de waarde van `ApplicationURL` om te verwijzen naar een ongeldige URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. Open het bestand TodoItemManager.cs vanaf de **draagbare** project en vervolgens een **werkelijk** voor de klasse base **uitzondering** toevoegen aan het blok **try... catch** in **SyncAsync**. Dit blok **catch** wordt de uitzondering op de console, als volgt:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Bouwen en uitvoeren van de clienttoepassing.  Sommige nieuwe items toevoegen. U ziet dat er een uitzondering wordt geregistreerd in de console voor elke poging om te synchroniseren met de back-end. Deze nieuwe objecten bestaan alleen in het lokale archief totdat ze op de mobiele backend kunnen worden geduwd. De clienttoepassing gedraagt zich alsof deze is aangesloten op de backend, ondersteunen die alle maken, lezen, bijwerken, verwijderen (CRUD).

4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Met Visual Studio kunt u uw tabel Azure SQL-Database om te zien of de gegevens in de back-end-database niet is gewijzigd.

    Open in Visual Studio **Server Explorer**. Ga naar de database in **Azure**->**SQL-Databases**. Klik met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar een SQL-databasetabel en de inhoud ervan.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Update de clienttoepassing voor de verbinding van uw mobiele back-end

In deze sectie, sluit de app op de mobiele backend die overeenkomt met de app terug zullen komen naar on line zijn. Wanneer u de penbeweging vernieuwen uitvoert, worden gegevens wordt gesynchroniseerd met uw mobiele back-end.

1. Constants.cs opnieuw te openen. Corrigeer de `applicationURL` om te verwijzen naar de juiste URL.

2. Opnieuw opbouwen en de clienttoepassing uitvoeren. De toepassing probeert te synchroniseren met de mobiele app-end na het starten. Controleer of dat er geen uitzonderingen worden geregistreerd in de Foutopsporingsconsole.

3. (Optioneel) Bekijk de bijgewerkte gegevens met behulp van SQL Server-Object Explorer of een REST hulpprogramma als Fiddler of [Postman][6]. Zoals u ziet dat is de gegevens tussen de backend-database en het lokale archief gesynchroniseerd.

    U ziet de gegevens gesynchroniseerd tussen de database en het lokale archief en bevat de items die u hebt toegevoegd terwijl uw app is verbroken.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Synchronisatie van off line gegevens in Azure mobiele Apps][2]
* [Azure mobiele Apps .NET SDK-procedure][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md