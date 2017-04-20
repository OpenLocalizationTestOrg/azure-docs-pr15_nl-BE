<properties
    pageTitle="Off line synchronisatie van uw app Universal Windows Platform (UWP) met mobiele Apps inschakelen | Azure App-Service"
    description="Informatie over het gebruik van een App Azure Mobile cache en synchronisatie van off line gegevens in uw app Universal Windows Platform (UWP)."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Off line synchronisatie van uw Windows-app inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u off line ondersteuning toevoegen aan een Universal Windows Platform (UWP) app met behulp van een backend Azure Mobile App. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app--weergeven, toevoegen of wijzigen van gegevens - zelfs als er geen netwerkverbinding is. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe back-end.

In deze zelfstudie kunt u het project UWP app van de zelfstudie [een Windows app maken] ter ondersteuning van de off line functies van Azure Mobile Apps bijwerken. Als u niet het project van de server gedownload snel starten, moet u de data access-extensie pakketten toevoegen aan uw project. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie over server-extensie.

Zie voor meer informatie over de functie Off line synchronisatie, het onderwerp [Offline gegevens synchroniseren in Azure Mobile Apps].

## <a name="requirements"></a>Vereisten

Deze zelfstudie is vereist voor de volgende minimumvereisten:

* Visual Studio 2013 uitvoeren onder Windows 8.1 of hoger.
* Voltooiing van het [maken van een Windows app][een windows-toepassing maken].
* [Winkel Azure Mobile Services-SQLite][sqlite store nuget]
* [SQLite voor Universal Windows Platform development](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>De clienttoepassing functies off line werken

Azure kunt Mobile App offline u interactief werken met een lokale database als u een off line scenario. Als u deze functies in uw app, u een [SyncContext] initialiseren[ synccontext] aan een lokale winkel. Vervolgens verwijzen naar de tabel via de interface van de[IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite wordt gebruikt als de lokale opslagruimte op het apparaat.

1. [SQLite runtime voor het Universal Windows Platform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix)te installeren.

2. Open in Visual Studio NuGet package manager voor het UWP app-project dat u in het [maken van een Windows app] zelfstudie hebt voltooid.
    Zoeken naar en installeren van het pakket **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet.

4. In de Solution Explorer met de rechtermuisknop op **References** > **Verwijzing toevoegen**  >  **Universele Windows** > **extensies**, schakelt u zowel **SQLite voor Universal Windows Platform** en **Visual C++ 2015 Runtime voor Universal Windows Platform apps**.

    ![SQLite UWP verwijzing toevoegen][1]

5. Open het bestand MainPage.xaml.cs en verwijder de opmerkingen van de `#define OFFLINE_SYNC_ENABLED` definitie.

6. Druk op **F5 om te bouwen en uitvoeren van de clienttoepassing** in Visual Studio. De app werkt hetzelfde als toen u de off line synchronisatie ingeschakeld. Echter, de lokale database is nu gevuld met gegevens die kunnen worden gebruikt in een off line scenario.

## <a name="update-sync"></a>Het loskoppelen van de back-end-toepassingen bijwerken

In dit gedeelte vindt verbreken u de verbinding naar uw mobiele App back-end voor het simuleren van een off line situatie. Wanneer u gegevens toevoegt, de uitzonderingshandler aangeeft dat de app in de off line modus. In deze status, nieuwe items toegevoegd in het lokale archief en wordt gesynchroniseerd met de mobiele app-end als push stand wordt uitgevoerd.

1. App.xaml.cs in het gedeelde project bewerken. Reactie op de initialisatie van de **MobileServiceClient** en voeg de volgende regel, waarbij een URL ongeldig mobiele app:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    U kunt ook offlinewerking aantonen door Wi-Fi en mobiele netwerken op het apparaat uit te schakelen of vliegtuig-modus gebruiken.

2. Druk op **F5** om te bouwen en uitvoeren van de app. U ziet de synchronisatie is mislukt bij vernieuwen wanneer de toepassing wordt gestart.

3. Nieuwe items en u ziet dat push niet werkt en de status van een [CancelledByNetworkError] wanneer die u op **Opslaan**klikt. De nieuwe todo-items zijn echter in het lokale archief totdat ze op de mobiele app backend kunnen worden geduwd.  In een productie-app als u deze uitzonderingen onderdrukken de clienttoepassing gedraagt zich alsof nog steeds met de mobiele app-end verbonden is.

4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Open in Visual Studio **Server Explorer**. Ga naar de database in **Azure**->**SQL-Databases**. Klik met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar een SQL-databasetabel en de inhoud ervan. Controleer of de gegevens in de back-end-database is niet gewijzigd.

6. (Optioneel) Uw mobiele back-end, met een GET-query in het formulier een query met een REST-hulpprogramma zoals Fiddler of Postman `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update de app opnieuw verbinden uw mobiele App-backend

In deze sectie kunt herstellen u de app op de mobiele app backend. Deze wijzigingen simuleren op de app vanaf netwerk.

Tijdens het uitvoeren van de toepassing, de `OnNavigatedTo` aanroepen gebeurtenis-handler `InitLocalStoreAsync`. Met deze methode wordt aangeroepen `SyncAsync` voor synchronisatie van uw lokale winkel met de backend-database. De toepassing probeert te synchroniseren bij het opstarten.

1. App.xaml.cs in het gedeelde project openen en verwijder de opmerkingen van de vorige initialisatie van `MobileServiceClient` de juiste URL van de mobiele app gebruiken.

2. Druk op **F5 om te bouwen en uitvoeren van de app** . De app wordt de lokale wijzigingen gesynchroniseerd met de mobiele App van Azure end met push- en pull-bewerkingen wanneer het `OnNavigatedTo` gebeurtenis-handler wordt uitgevoerd.

3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server-Object Explorer of een hulpprogramma als Fiddler voor REST bekijken. Mededeling van de gegevens is tussen de Azure Mobile App backend-database en het lokale archief gesynchroniseerd.

4. In de app, klikt u op het selectievakje naast een paar items uit te voeren in het lokale archief.

  `UpdateCheckedTodoItem`oproepen `SyncAsync` item met de mobiele App-end sync elk voltooid. `SyncAsync`push- en pull roept. Echter, **wanneer het uitvoeren van een pull ten opzichte van een tabel die de client heeft gewijzigd, een push altijd automatisch wordt uitgevoerd**. Dit gedrag zorgt voor dat alle tabellen in het lokale archief en relaties consistent blijven. Dit probleem kan resulteren in een onverwachte push.  Zie voor meer informatie over dit probleem, [Off line gegevens synchroniseren in Azure Mobile Apps].


##<a name="api-summary"></a>API-overzicht

Ter ondersteuning van de off line functies van mobiele services, gebruikt u de [IMobileServiceSyncTable] -interface en we geïnitialiseerd [MobileServiceClient.SyncContext] [ synccontext] met een lokale SQLite database. Wanneer u offline bent, werken de normale bewerkingen CRUD voor mobiele toepassingen als de toepassing nog steeds verbonden is, terwijl de bewerkingen tegen het lokale archief plaatsvinden. De volgende methoden worden gebruikt voor het lokale archief gesynchroniseerd met de server:

*  **[PushAsync]** Deze methode maakt deel uit van de [IMobileServicesSyncContext], worden wijzigingen in alle tabellen op de backend geduwd. Alleen de records met de lokale wijzigingen worden naar de server verzonden.

* **[PullAsync] ** 
   een pull is gestart vanuit een [IMobileServiceSyncTable]. Als er wijzigingen in de tabel, wordt een impliciete push uitvoeren om ervoor te zorgen dat alle tabellen in het lokale archief en relaties consistent blijven. De parameter *pushOtherTables* bepaalt of de andere tabellen in de context worden geplaatst in een impliciete push. De parameter *query* neemt een [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   of OData-querytekenreeks om de geretourneerde gegevens te filteren. De parameter *queryId* wordt gebruikt voor incrementele synchronisatie definiëren. Zie  [Off line gegevens synchroniseren in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works)voor meer informatie.

* **[PurgeAsync]** Uw app moet periodiek aanroepen van deze methode voor het verwijderen van verouderde gegevens uit het lokale archief. Gebruik de parameter *dwingen* wanneer u wilt verwijderen van alle wijzigingen die nog niet zijn gesynchroniseerd.

Zie voor meer informatie over deze concepten [Off line gegevens synchroniseren in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Meer info

De volgende onderwerpen bevatten meer achtergrondinformatie over de functie Off line synchronisatie van mobiele Apps:

* [Synchronisatie van off line gegevens in Azure mobiele Apps]
* [Azure mobiele Apps .NET SDK-procedure][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Synchronisatie van off line gegevens in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[Maak een windows-app]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
