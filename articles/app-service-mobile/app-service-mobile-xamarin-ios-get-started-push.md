<properties
    pageTitle="Push-meldingen toevoegen aan uw Xamarin.iOS app met Azure App-Service"
    description="Informatie over het gebruik van App-Service Azure push-meldingen verzenden naar uw app Xamarin.iOS"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Push-meldingen toevoegen aan uw Xamarin.iOS App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie voegt u push-meldingen aan het project [Xamarin.iOS snelle start](app-service-mobile-xamarin-ios-get-started.md) zodat een push-bericht wordt verzonden naar het apparaat elke keer dat een record wordt ingevoegd.

Als u niet het project van de server gedownload snel starten, moet u het pakket push notification-extensie. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="prerequisites"></a>Vereisten

* Voltooi de zelfstudie [quickstart Xamarin.iOS](app-service-mobile-xamarin-ios-get-started.md) .

* Een fysieke iOS-apparaat. Push-meldingen worden niet ondersteund door de iOS simulator.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>De app voor push-meldingen op de Apple developer-portal registreren

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Configureer uw mobiele App om push-meldingen te verzenden

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Update van het serverproject om push-meldingen te verzenden

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Configureer uw project Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Push-meldingen toevoegen aan uw app

1. **QSTodoService**, voeg de volgende eigenschap zodat **AppDelegate** mobiele client kunt verkrijgen:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Voeg de volgende `using` -instructie aan de bovenkant van het bestand **AppDelegate.cs** .

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. In **AppDelegate**, overschrijven de gebeurtenis **FinishedLaunching** :

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Overschrijven in hetzelfde bestand, de **RegisteredForRemoteNotifications** -gebeurtenis. In deze code schrijft u voor een eenvoudige sjabloon melding die door de server worden verzonden voor alle ondersteunde platformen.

    Zie voor meer informatie over sjablonen met melding Hubs, [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Hef vervolgens de gebeurtenis **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

De app is nu bijgewerkt ter ondersteuning van push-meldingen.

## <a name="test"></a>Push-meldingen in de app testen

1. Druk op de knop **uitvoeren** voor het bouwen van het project en de app starten in een iOS-apparaat en klik vervolgens op **OK** om te accepteren van push-meldingen.

    > [AZURE.NOTE] U moet expliciet push-meldingen accepteren van uw app. Deze aanvraag komt alleen voor de eerste keer dat de toepassing wordt uitgevoerd.

2. Typ een taak in de app, en klik vervolgens op het plusteken (**+**) pictogram.

3. Controleer of dat een bericht wordt ontvangen en klik vervolgens op **OK** om het bericht te sluiten.

4. Herhaal stap 2 en onmiddellijk de toepassing sluiten en vervolgens controleren of dat een melding wordt weergegeven.

Deze zelfstudie hebt voltooid.

<!-- Images. -->

<!-- URLs. -->



