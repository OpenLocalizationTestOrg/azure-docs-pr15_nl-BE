<properties
    pageTitle="iOS Push-meldingen met melding Hubs voor Xamarin apps | Microsoft Azure"
    description="In deze zelfstudie leert u hoe Azure melding Hubs met push-meldingen te verzenden naar een toepassing Xamarin iOS."
    services="notification-hubs"
    keywords="IOS push-meldingen, push-berichten, push-meldingen, push-bericht"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS Push-meldingen met melding Hubs voor Xamarin apps

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht
> [AZURE.IMPORTANT] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

In deze zelfstudie wordt beschreven hoe u met push-meldingen te verzenden naar een toepassing iOS Azure melding Hubs.
U maakt een lege Xamarin.iOS app die push-meldingen ontvangt met behulp van de [Apple Push Notification Service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). Wanneer u klaar bent, zult u uw melding hub gebruiken voor het uitzenden van push-meldingen voor alle apparaten die met uw app. De voltooide code is beschikbaar in de [app NotificationHubs] [ GitHub] monster.

Deze zelfstudie laat zien dat het bericht eenvoudige push scenario uitgezonden met melding Hubs.

##<a name="prerequisites"></a>Vereisten

Deze zelfstudie is het volgende vereist:

+ [Xcode 6.0][Install Xcode]
+ Een iOS 7.0 (of hoger) compatibele apparaat
+ iOS Developer programma lidmaatschap
+ [Xamarin Studio]

   > [AZURE.NOTE] Vanwege de configuratievereisten voor iOS push-meldingen, moet u implementeren en testen van de voorbeeldtoepassing op een fysiek i/o-apparaat (iPhone of iPad) in plaats van in de simulator.

Voltooien van deze zelfstudie is een vereiste voor een andere melding Hubs zelfstudies voor Xamarin iOS apps.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Uw melding hub configureren

In dit gedeelte leidt u door het maken van een nieuwe melding hub en verificatie configureren met het **.p12** push-certificaat dat u hebt gemaakt met APNS. Als u gebruiken een kennisgeving hub die u al hebt gemaakt wilt, kunt u verder met stap 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Als we willen de APNS-verbinding configureren in de Portal Azure, instellingen voor uw melding Hub, ande op <b>Notification Services</b>en klik vervolgens op het item <b>Apple (APNS)</b> in de lijst. Zodra u klaar bent, klik op <b>Certificaat uploaden</b> en selecteert u het certificaat <b>.p12</b> die u eerder hebt geëxporteerd en het wachtwoord voor het certificaat.</p>
<p>Zorg ervoor dat u push-berichten verzenden in een ontwikkelomgeving <b>sandbox-</b> modus selecteren. De <b>productie</b> -instelling alleen gebruiken als u push-meldingen verzenden naar gebruikers die al uw app in de winkel hebt gekocht.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


De hub van uw melding is nu geconfigureerd voor het werken met APNS, en u hebt de verbindingsreeksen voor het registreren van uw app en push-meldingen te verzenden.


##<a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinding met de melding hub

#### <a name="create-a-new-project"></a>Een nieuw project maken

1. Maak een nieuw project voor iOS in Xamarin Studio en selecteer de **Unified API** > toepassingssjabloon**één weergave** .

    ![Xamarin Studio - toepassing selecteren][31]

2. Voeg een verwijzing naar het onderdeel Messaging van Azure. Klik met de rechtermuisknop op de map met **componenten** voor uw project in de oplossing en **Meer onderdelen u**kiest. Zoek het onderdeel **Messaging van Azure** en het onderdeel toevoegt aan uw project.

3. In **AppDelegate.cs**, voeg de volgende instructie:

        using WindowsAzure.Messaging;

4. Verklaart een exemplaar van **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. Maak een **Constants.cs** -klasse met de volgende variabelen:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. Update in **AppDelegate.cs**, **FinishedLaunching()** , zodat deze overeenkomt met het volgende:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Overschrijf de methode **RegisteredForRemoteNotifications()** in **AppDelegate.cs**:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Overschrijf de methode **ReceivedRemoteNotification()** in **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Maak de volgende methode van **ProcessNotification()** in **AppDelegate.cs**:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] U kunt **FailedToRegisterForRemoteNotifications()** voor het afhandelen van situaties zoals geen netwerkverbinding overschrijven. Dit is vooral van belang waar de gebruiker kan de toepassing start in de offline modus (bijvoorbeeld vliegtuig) en u wilt push-scenario's voor uw app berichten verwerken.


10. De app op uw apparaat worden uitgevoerd.


## <a name="sending-push-notifications"></a>Push-meldingen te verzenden


U kunt testen push-meldingen ontvangen in uw app door het verzenden van meldingen in de [Portal Azure] via de functionaliteit **Testen verzenden** in de toolset **Probleemoplossing** , rechts in de pagina melding hub, zoals in het onderstaande scherm.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Push-meldingen worden meestal verzonden door middel van een back-end services, zoals mobiele diensten of met behulp van een compatibele bibliotheek. U kunt ook de REST API direct push om berichten te verzenden als een bibliotheek niet beschikbaar in uw scenario is. 

In deze zelfstudie wordt Houd het simpel en gewoon aantonen dat uw client app testen door het verzenden van berichten met behulp van de .NET SDK voor kennisgeving hubs in een consoletoepassing in plaats van een back-end-service. Het is raadzaam de zelfstudie [Gebruiken kennisgeving Hubs op push-meldingen aan gebruikers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) als volgende stap voor het verzenden van meldingen van een ASP.NET-end. De volgende benaderingen kunnen echter worden gebruikt voor het verzenden van meldingen:

* **REST-Interface**: kunt u push-bericht ondersteunen op een willekeurig back-end-platform met behulp van de [interface van de REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure melding Hubs .NET SDK**: In de Nuget Package Manager voor Visual Studio [- Installatiepakket Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)uitvoeren.

* **Node.js** : [het gebruik van Hubs melding van Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobiele Apps**: Zie voor een voorbeeld van het verzenden van meldingen van een end Azure App Service Mobile Apps die geïntegreerd met de melding Hubs, [push-meldingen voor uw mobiele app toevoegen](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: Zie 'Het gebruik van Hubs melding van Java/PHP' voor een voorbeeld van de push-meldingen worden verzonden via de REST API's ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Optioneel) Push-meldingen verzenden vanuit een Console .NET App

In deze sectie sturen we push-meldingen met behulp van een eenvoudige app voor .NET-console. Voor de toepassing van dit voorbeeld zal we overschakelen op een Windows-ontwikkelomgeving met Visual Studio is geïnstalleerd.

1. Maak een nieuw Visual C#-consoletoepassing in Visual Studio:

    ![Visual Studio - Maak een nieuwe consoletoepassing][213]

2. Klik op **Extra**in Visual Studio naar **NuGet Package Manager**en klik vervolgens op **Package Manager-Console**.

    De package manager-console moet worden gekoppeld aan de onderkant van de Visual Studio-werkruimte weergegeven.

3. Het **project standaard** ingesteld voor de nieuwe console application-project in het consolevenster met Package Manager en klik in het consolevenster met de volgende opdracht worden uitgevoerd:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee wordt een verwijzing naar de Azure melding Hubs SDK met behulp van het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakket Microsoft.Azure.Notification Hubs NuGet</a>toegevoegd.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Open de `Program.cs` -bestand en voeg de volgende `using` -instructie, ervoor te zorgen dat we Azure klassen en functies binnen de hoofdklasse kunt gebruiken:

        using Microsoft.Azure.NotificationHubs;

3. In de `Program` klasse, voegt u de volgende methode (Vergeet niet ter vervanging van de **verbindingsreeks** en de **hubnaam**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Voeg de volgende regels in de `Main` methode:

         SendNotificationAsync();
         Console.ReadLine();

5. Druk op F5 voor het uitvoeren van de app. Binnen enkele seconden ziet u een push-bericht op uw apparaat worden weergegeven. Of u Wi-Fi of een mobiele-netwerk gebruikt, zorg ervoor dat er een actieve internetverbinding op het apparaat.

De mogelijke nettoladingen kunt u vinden in de Apple- [Local en Push-melding Programming Guide].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Optioneel) Meldingen verzenden via een mobiele Service

In deze sectie sturen we een mobile-service via een script knooppunt met push-meldingen.

Ga als volgt [aan de slag met mobiele Services], een bericht verzenden met behulp van een mobiele service, en vervolgens:

1. Aanmelden bij de [Klassieke Portal Azure]en selecteer uw mobiele service.

2. Selecteer het tabblad **Taakplanner** op de voorgrond.

    ![Klassieke Azure Portal - planner][215]

3. Een nieuwe geplande taak maken, Voeg een naam en selecteer **op aanvraag**.

    ![Azure klassieke Portal - nieuwe taak maken][216]

4. Wanneer de taak is gemaakt, klikt u op de taaknaam van de. Klik vervolgens op het tabblad **Script** op de bovenste balk.

5. Voeg het volgende script binnen de scheduler-functie. Zorg ervoor dat de tijdelijke aanduidingen vervangen door de naam van de hub van uw melding en de verbindingsreeks voor *DefaultFullSharedAccessSignature* die u eerder hebt gekregen. Klik op **Opslaan**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Klik **Eenmaal uitvoeren** op de onderste balk. U ontvangt een waarschuwing op het apparaat.

##<a name="next-steps"></a>Volgende stappen

In het volgende eenvoudige voorbeeld u push-meldingen voor alle iOS apparaten uitgezonden. Verwijzen naar de zelfstudie [Gebruiken kennisgeving Hubs op push-meldingen voor gebruikers]om specifieke doelpubliek. Als u wilt dat uw gebruikers door belangengroepen in segmenten, kunt u [Gebruik melding Hubs voor het verzenden van laatste nieuws]lezen. Meer informatie over het gebruik van Hubs kennisgeving in [Kennisgeving Hubs richtlijnen] en in de [Kennisgeving Hubs procedures voor iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Aan de slag met mobiele Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure klassieke Portal]: https://manage.windowsazure.com/
[Richtsnoeren voor kennisgeving Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Melding Hubs procedures voor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Melding Hubs gebruiken voor push-meldingen aan gebruikers]: /manage/services/notification-hubs/notify-users-aspnet
[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: /manage/services/notification-hubs/breaking-news-dotnet

[Lokale en Push-melding Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com
