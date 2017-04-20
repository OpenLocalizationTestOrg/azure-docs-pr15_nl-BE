<properties
    pageTitle="Off line synchronisatie inschakelen voor uw Azure Mobile App (Android)"
    description="Informatie over het gebruik van mobiele Apps in App-Service op off line cache en synchronisatie gegevens in uw toepassing voor Android"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Off line synchronisatie van uw Android mobiele app inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie heeft betrekking op de functie Off line synchronisatie van Azure Mobile Apps voor Android. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe back-end.

Als dit uw eerste ervaring met Azure Mobile Apps, moet u eerst de zelfstudie [een Android App maken]te voltooien. Als u niet het project van de server gedownload snel starten, moet u de data access-extensie pakketten toevoegen aan uw project. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie over server-extensie.

Zie voor meer informatie over de functie Off line synchronisatie, het onderwerp [Offline gegevens synchroniseren in Azure Mobile Apps].

## <a name="update-the-app-to-support-offline-sync"></a>Update de app voor de synchronisatie van off line ondersteuning

Met off line synchronisatie, te lezen en schrijven van een *tabel synchroniseren* (met de *IMobileServiceSyncTable* -interface), die deel uitmaakt van een **SQLite** database op uw apparaat.

Push-en pull-wijzigingen tussen het apparaat en Azure Mobile Services, moet u een *synchronisatiecontext* (*MobileServiceClient.SyncContext*), die u kunt met de lokale database initialiseren voor het opslaan van gegevens lokaal gebruiken.

1. In `TodoActivity.java`, commentaar uit de bestaande definitie van `mToDoTable` en opmerkingen bij de versie van de tabel synchronisatie:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. In de `onCreate` methode, opmerking uit de bestaande initialisatie van `mToDoTable` en opmerkingen bij deze definitie:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. In `refreshItemsFromTable` opmerking uit de definitie van `results` en opmerkingen bij deze definitie:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Opmerking van de definitie van `refreshItemsFromMobileServiceTable`.

5. Opmerkingen bij de definitie van `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Opmerkingen bij de definitie van `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>De app testen

In deze sectie, test het gedrag met Wi-Fi op en schakel Wi-Fi voor het maken van een off line scenario.

Wanneer u gegevens toevoegt, worden ze gehouden in het lokale archief voor SQLite, maar niet gesynchroniseerd met de mobiele service totdat u op de knop **vernieuwen** . Andere apps wellicht verschillende vereisten met betrekking tot wanneer gegevens moeten worden gesynchroniseerd, maar voor demonstratiedoeleinden deze zelfstudie is de gebruiker expliciet verzoek.

Wanneer u op die knop drukt, wordt een nieuwe achtergrondtaak gestart. Deze duwt eerst alle wijzigingen in het lokale archief met synchronisatiecontext, wordt de gegevens worden gewijzigd in Azure naar lokale tabel.

### <a name="offline-testing"></a>Off line testen

1. Plaats het apparaat of de simulator in *Vliegtuig modus*. Hiermee maakt u een off line scenario.

2. Sommige *ToDo* -items toevoegen of bepaalde items als voltooid markeren. Sluit het apparaat of de simulator (of de toepassing geforceerd sluiten) en opnieuw te starten. Controleer dat uw wijzigingen zijn blijven bestaan op het apparaat omdat ze worden gehouden in het lokale archief voor SQLite.

3. De inhoud van de Azure *TodoItem* -tabel met een SQL hulpprogramma zoals *SQL Server Management Studio*of een andere client zoals *Fiddler* of *Postman*bekijken. Controleer de nieuwe items _niet_ zijn gesynchroniseerd met de server

    + Een backend Node.js Ga de [Azure portal](https://portal.azure.com/), en met uw mobiele App back-end klikt u op **Eenvoudige tabellen** > **TodoItem** de inhoud van de `TodoItem` tabel.
    + Weergeven voor een .NET back-end, de inhoud van de tabel met een hulpprogramma SQL als *SQL Server Management Studio*of een andere client zoals *Fiddler* of *Postman*.

4. Wi-Fi inschakelen in het apparaat of de simulator. Druk vervolgens op de knop **vernieuwen** .

5. De TodoItem-gegevens weer in de portal Azure. De nieuwe en gewijzigde TodoItems moet nu worden weergegeven.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Synchronisatie van off line gegevens in Azure mobiele Apps]

* [Cloud Cover: Offline synchronisatie in Azure mobiele Services] \(Opmerking: de video op mobiele diensten, maar off line synchronisatie werkt op vergelijkbare wijze in Azure mobiele Apps\)


<!-- URLs. -->

[Synchronisatie van off line gegevens in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md

[Maak een Android App]: app-service-mobile-android-get-started.md

[Cloud Cover: Offline synchronisatie in Azure mobiele Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

