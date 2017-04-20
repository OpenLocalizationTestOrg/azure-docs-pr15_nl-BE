<properties
    pageTitle="Push-meldingen toevoegen aan uw app Universal Windows Platform (UWP) | Azure mobiele Apps"
    description="Leren werken met Azure App Service Mobile Apps en Azure melding Hubs push-meldingen verzenden naar uw app Universal Windows Platform (UWP)."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Push-meldingen toevoegen aan uw Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie voegt u push-meldingen aan het project [snel Windows gestart](app-service-mobile-windows-store-dotnet-get-started.md) zodat een push-bericht wordt verzonden naar het apparaat elke keer dat een record wordt ingevoegd.

Als u niet het project van de server gedownload snel starten, moet u het pakket push notification-extensie. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="configure-hub"></a>Een Hub meldingen configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Uw app voor push-meldingen registreren

U moet de Windows Store-toepassingen verzenden, moet u uw serverproject te integreren met Windows Notification Services (WNS) voor het verzenden van push configureren.

1. In Visual Studio Solution Explorer met de rechtermuisknop op het project UWP app, klikt u op **winkel** > **App koppelen met de winkel...**. 

    ![App koppelen aan Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. In de wizard, klikt u op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor uw app in **Reserve een nieuwe naam voor de toepassing**en klik vervolgens op **Reserve**.

3. Nadat de registratie app is gemaakt, selecteert u de naam van de nieuwe app, klik op **volgende**en klik op **koppelen**. Hiermee voegt u de vereiste Windows Store registratiegegevens toe aan manifest van de toepassing.  

7. Ga naar de [Dev Center van Windows](https://dev.windows.com/en-us/overview)aanmelden met uw Microsoft-account, klik op de nieuwe inschrijving app in **Mijn apps**en vouw **Services** > **Push-meldingen**. 

8. Klik op de pagina **Push-meldingen** **Live Services-site** onder **Microsoft Azure mobiele Services**.

9. In de registratiepagina Noteer de waarde onder de **geheimen van de toepassing** en de **Pakket-SID**, die u vervolgens kunt configureren van uw mobiele app backend. 

    ![App koppelen aan Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Het geheim van de client en de SID-pakket zijn belangrijke referenties. Deze waarden te delen met iedereen of ze distribueren met uw app niet. De **Toepassings-Id** wordt gebruikt met het geheim Microsoft Account verificatie configureren.

##<a name="configure-the-backend-to-send-push-notifications"></a>Configureren van de back-end om push-meldingen te verzenden

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Update van de server om push-meldingen te verzenden

Gebruik de onderstaande procedure die overeenkomt met uw back-end-projecttype&mdash; [.NET back-end](#dotnet) of [Node.js backend](#nodejs).

### <a name="dotnet"></a>.NET back-end project

1. In Visual Studio met de rechtermuisknop op het serverproject **Beheren NuGet pakketten**, zoeken naar Microsoft.Azure.NotificationHubs, klik op **installeren**. Hiermee installeert u de melding Hubs client library.

2. Vouw **Controllers**, TodoItemController.cs open en voeg de volgende instructies:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. Voeg de volgende code na het aanroepen van **InsertAsync**in de methode **PostTodoItem** :

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Deze code geeft aan dat de kennisgeving hub voor het verzenden van een push-bericht wanneer een nieuw item ingevoegd wordt.

4. De serverproject publiceren.

### <a name="nodejs"></a>Node.js back-end project

1. Als u nog niet hebt gedaan, [het project quickstart downloaden](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) of anders de [online editor in Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)gebruiken.

2. De bestaande code in het bestand todoitem.js wordt vervangen door het volgende:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Dit stuurt een WNS toast-melding met het item.text wanneer een nieuw item van de taak wordt ingevoegd.

2. Bij het bewerken van het bestand op uw lokale computer publiceert u het serverproject.

##<a id="update-app"></a>Push-meldingen toevoegen aan uw app

Vervolgens wordt moet uw app registreren voor push-meldingen op opstarten. Als u al verificatie hebt ingeschakeld, moet u dat de gebruiker tekens in voordat u probeert te registreren voor push-meldingen.

1. Open het bestand **App.xaml.cs** en voeg de volgende `using` instructies:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Toevoegen in hetzelfde bestand, de volgende definitie van de **InitNotificationsAsync** -methode om de **App** -klasse:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Deze code haalt de ChannelURI voor de toepassing van de WNS en registreert vervolgens die ChannelURI met uw mobiele App in App-Service.

3. De optie **async** toevoegen aan de methodedefinitie van de aan de bovenkant van de gebeurtenis-handler voor de **OnLaunched** in **App.xaml.cs**en toevoegen de volgende aanroep van de nieuwe **InitNotificationsAsync** -methode, zoals in het volgende voorbeeld:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Dit zorgt ervoor dat de ChannelURI tijdelijk telkens wanneer die de toepassing wordt gestart, is geregistreerd.

4. Uw UWP app project opnieuw. Uw app is gereed om toast meldingen te ontvangen.

##<a id="test"></a>Push-meldingen in de app testen

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Volgende stappen

Meer informatie over push-meldingen:

* [Het gebruik van beheerde client voor Azure mobiele Apps](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Sjablonen geven u de flexibiliteit voor het verzenden van dit gereedschap duwt platforms en gelokaliseerde gereedschap duwt. Informatie over het registreren van sjablonen.

* [Vaststellen van problemen met push notification](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Er zijn verschillende redenen waarom berichten kunnen ophalen neergezet of sluit niet af op apparaten. In dit onderwerp wordt beschreven hoe u te analyseren en de hoofdoorzaak van push notification storingen uitzoeken. 

Kunt u door te gaan op een van de volgende cursussen:

+ [Verificatie toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Informatie over het verifiëren van gebruikers van uw toepassing met een id-provider.

+ [Off line synchronisatie van uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informatie over het off line ondersteuning van uw app in de backend voor een mobiele App toevoegen. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

