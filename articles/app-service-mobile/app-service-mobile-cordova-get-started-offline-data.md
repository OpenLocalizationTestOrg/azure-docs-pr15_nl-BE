<properties
    pageTitle="Off line synchronisatie inschakelen voor uw mobiele App in Azure (Cordova) | Microsoft Azure"
    description="Informatie over het gebruik van App Service Mobile App voor off line cache en synchronisatie van gegevens in uw toepassing Cordova"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Off line synchronisatie van uw mobiele app Cordova inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Deze zelfstudie bevat de functie Off line synchronisatie van mobiele Apps in Azure voor Cordova. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

Deze zelfstudie is gebaseerd op de quickstart Cordova oplossing voor mobiele toepassingen die u maakt wanneer u de zelfstudie [start Apache Cordova snel]voltooien. In deze zelfstudie werkt de oplossing quickstart off line functies van Azure Mobile Apps toe te voegen. Ook selecteren we de off line-specifieke code in de app.

Zie voor meer informatie over de functie Off line synchronisatie, het onderwerp [Offline gegevens synchroniseren in Azure Mobile Apps]. Zie het [Leesmij-] bestand in de invoegtoepassing voor meer informatie over gebruik van de API.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Off line synchronisatie aan de quickstart-oplossing toevoegen

De synchronisatiecode van off line moet worden toegevoegd aan de app. Off line synchronisatie vereist de cordova-sqlite-opslag-invoegtoepassing automatisch wordt toegevoegd aan uw app wanneer de invoegtoepassing voor Mobile Apps in Azure is opgenomen in het project. Het project Quickstart bevat beide van deze plug-ins.

1. In Visual Studio Solution Explorer, open index.js en vervang de code

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    Met deze code:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Vervang vervolgens de volgende code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Met deze code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    De voorgaande code toevoegingen initialiseren van het lokale archief en definieert u een lokale tabel die overeenkomt met de waarden van de kolom die wordt gebruikt in uw Azure back-end. (U hoeft niet alle kolomwaarden opnemen in deze code.)

    U krijgt een verwijzing naar de sync-context door het aanroepen van **getSyncContext**. De sync-context kunt u relaties tussen tabellen behouden door het bijhouden en het verleggen van wijzigingen in alle tabellen die een clienttoepassing heeft gewijzigd als **push** wordt aangeroepen.

3. URL van de toepassing naar de URL van uw mobiele App toepassing bijwerken.

4. Vervolgens wordt deze code vervangen:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    Met deze code:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    De bovenstaande code initialiseert de sync-context en roept vervolgens getSyncTable (in plaats van een getTable) als u een verwijzing naar een lokale tabel.

    Deze code wordt de lokale database voor alle maken, lezen, bijwerken en verwijderen van bewerkingen (CRUD).

    In dit voorbeeld voert eenvoudige foutafhandeling op synchronisatieconflicten. Een echte toepassing zou de verschillende fouten zoals netwerkomstandigheden, server conflicten en anderen te verwerken. Zie voor voorbeelden van programmacode, de [off line synchronisatie monster].

5. Vervolgens voegt u deze functie om de werkelijke synchronisatie uitvoeren.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    U besluit wanneer wijzigingen op de backend Mobile App duwen door het aanroepen van **push** op het **syncContext** -object dat door de client. Bijvoorbeeld, kunt u een aanroep naar **syncBackend** toevoegen aan de gebeurtenis-handler van een knop in de app bijvoorbeeld een nieuwe knop synchronisatie of zou u de aanroep van de functie **addItemHandler** om te synchroniseren telkens wanneer een nieuw item wordt toegevoegd.

##<a name="offline-sync-considerations"></a>Overwegingen bij het synchroniseren van off line

In het voorbeeld heet de **push** -methode van **syncContext** alleen bij het opstarten van de app in de functie terugbellen voor aanmelding.  In een concrete toepassing, kan u de functionaliteit van deze synchronisatie handmatig geactiveerd of wanneer de status van het netwerk verandert.

Wanneer een pull is uitgevoerd op een tabel die nog in behandeling zijnde lokale updates bijgehouden door de context, wordt die pullbewerking een voorafgaande context push automatisch geactiveerd. Bij het vernieuwen, toe te voegen en het voltooien van artikelen in dit voorbeeld kunt u weglaten de oproep expliciete **push** omdat redundante (eerste Controleer het [Leesmij-bestand] voor de huidige status van de functie) kan het zijn.

Alle records in de tabel met externe todoItem worden doorzocht in de opgegeven code, maar het is ook mogelijk om records te filteren door een query-id en de query doorgegeven aan de **push**. Zie voor meer informatie de sectie *Incrementele synchronisatie van* [off line gegevens]gesynchroniseerd in Azure Mobile Apps.

## <a name="optional-disable-authentication"></a>(Optioneel) Verificatie uitschakelen

Als u verificatie van niet hebt ingesteld en u wilt verificatie voordat tests off line synchronisatie instellen, een reactie van de callback-functie voor aanmelding, maar laat u de code in de functie terugbellen zonder opmerkingen.

De code ziet er zo uit na de regels voor de aanmelding van een commentaarteken.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

De app wordt nu gesynchroniseerd met de Azure end tijdens het uitvoeren van de app in plaats van wanneer u zich aanmeldt.

## <a name="run-the-client-app"></a>De clienttoepassing uitvoeren

Met off line synchronisatie is nu ingeschakeld, kunt u nu uitvoeren de clienttoepassing ten minste eenmaal op elk platform voor het vullen van de lokale database. U wordt later een off line scenario simuleren en de gegevens in het lokale archief wijzigen terwijl de app offline is.

## <a name="optional-test-the-sync-behavior"></a>(Optioneel) Test het sync-gedrag

In dit gedeelte wijzigt u het project op client om een off line scenario simuleren met behulp van een ongeldige URL voor uw back-end. Wanneer u gegevensitems toevoegen of wijzigen, worden deze wijzigingen opgeslagen in het lokale archief, maar niet gesynchroniseerd met de back-end-gegevensopslag totdat de verbinding opnieuw tot stand gebracht.

1. In de Solution Explorer, open het bestand index.js en wijzig de URL van de toepassing om te verwijzen naar een ongeldige URL, zoals de volgende:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Werk de CSP in index.html, `<meta>` -element met dezelfde URL ongeldig.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Bouwen en de clienttoepassing wordt uitgevoerd en u ziet dat er een uitzondering in de console wordt geregistreerd wanneer de app probeert te synchroniseren met de back-end na aanmelding. Eventuele nieuwe items die u toevoegt wordt alleen in het lokale archief bestaan totdat ze op de mobiele backend kunnen worden geduwd. De clienttoepassing gedraagt zich als de backend, ondersteunen die alle maken, lezen, bijwerken, verwijderen (CRUD) is verbonden.

4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Met Visual Studio kunt u uw tabel Azure SQL-Database om te zien of de gegevens in de back-end-database niet is gewijzigd.

    Open in Visual Studio **Server Explorer**. Ga naar de database in **Azure**->**SQL-Databases**. Klik met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar een SQL-databasetabel en de inhoud ervan.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Optioneel) Het opnieuw verbinding maken met uw mobiele backend testen

In deze sectie wordt u opnieuw verbinding maakt de app op de mobiele backend die overeenkomt met de app terug zullen komen naar on line zijn. Wanneer u zich aanmeldt, worden de gegevens wordt gesynchroniseerd met uw mobiele back-end.

1. Open index.js en corrigeer de URL van de toepassing om te verwijzen naar de juiste URL.

2. Open index.html en corrigeer de URL van de toepassing in de CSP `<meta>` element.

3. Opnieuw opbouwen en de clienttoepassing uitvoeren. De toepassing probeert te synchroniseren met de mobiele app-end na aanmelding. Controleer of dat er geen uitzonderingen worden geregistreerd in de Foutopsporingsconsole.

4. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server-Object Explorer of een hulpprogramma als Fiddler voor REST bekijken. Zoals u ziet dat is de gegevens tussen de backend-database en het lokale archief gesynchroniseerd.

    U ziet de gegevens gesynchroniseerd tussen de database en het lokale archief en bevat de items die u hebt toegevoegd terwijl uw app is verbroken.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Synchronisatie van off line gegevens in Azure mobiele Apps]

* [Cloud Cover: Offline synchronisatie in Azure mobiele Services] \(Opmerking: de video op mobiele diensten, maar off line synchronisatie werkt op vergelijkbare wijze in Azure mobiele Apps\)

* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Volgende stappen

* Bekijk meer geavanceerde synchronisatiefuncties, off line zoals conflictoplossing in de [off line synchronisatie monster]
* Bekijk de off line synchronisatie API reference in het [Leesmij-bestand]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Snelle start Apache Cordova]: app-service-mobile-cordova-get-started.md
[LEESMIJ-BESTAND]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[off line synchronisatie monster]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Synchronisatie van off line gegevens in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline synchronisatie in Azure mobiele Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
