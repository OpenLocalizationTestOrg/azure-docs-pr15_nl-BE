<properties
    pageTitle="Aan de slag met Azure mobiele Engagement voor Xamarin.iOS"
    description="Informatie over het Azure Mobile Engagement met Analytics en Push-meldingen te gebruiken voor Xamarin.iOS toepassingen."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Aan de slag met Azure mobiele Engagement voor Xamarin.iOS toepassingen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp wordt beschreven hoe u Azure Mobile Engagement gebruiken om te begrijpen van het gebruik van app en push-meldingen verzenden naar gesegmenteerde gebruikers in de toepassing van een Xamarin.iOS.
In deze zelfstudie maakt u een lege Xamarin.iOS app die verzamelt basisgegevens en push-meldingen met behulp van de Apple Push Notification System (APNS) ontvangt.

Deze zelfstudie is het volgende vereist:

+ [Xamarin Studio](http://xamarin.com/studio). U kunt ook Visual Studio met Xamarin, maar in deze zelfstudie wordt Xamarin Studio. Zie [Setup- en voor Visual Studio en Xamarin installeren](https://msdn.microsoft.com/library/mt613162.aspx)voor installatie-instructies. 
+ [Betrokkenheid bij de mobiele Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Betrokkenheid bij de mobiele installatie voor uw iOS-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app in de backend Mobile Engagement

Deze zelfstudie bevat een "basic integration" is de minimale vereiste gegevens verzamelen en verzenden van een push-bericht instellen.

Een eenvoudige app maken we met Xamarin om aan te tonen van de integratie:

###<a name="create-a-new-xamarinios-project"></a>Maak een nieuw project voor Xamarin.iOS

1. Xamarin Studio starten. Ga naar het **bestand** -> **nieuwe** -> **oplossing** 

    ![][1]

2. Selecteer **Één weergave App**, Controleer of de geselecteerde taal **C#** en klik op **volgende**.

    ![][2]

3. Vul de **Naam** en de **Organisatie-id** en klik op **volgende**. 

    ![][3]

    > [AZURE.IMPORTANT] Zorg dat het profiel voor publiceren u uiteindelijk implementeren met uw iOS-app is een App-ID die overeenkomt met exact met de bundel-id die u hier hebt. 

4. De **Naam van het Project**, de **Naam van de oplossing** en de **locatie** bijwerken indien nodig en klikt u op **maken**.

    ![][4]
 
Xamarin Studio maakt de demo app waarin we Mobile Engagement wordt geïntegreerd. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinding maken met uw app Mobile Engagement backend

1. De map **pakketten** in de oplossing windows Klik met de rechtermuisknop en selecteer **Pakketten toevoegen**

    ![][5]

2. De **SDK van Microsoft Azure Mobile Engagement Xamarin** zoeken en toevoegen aan uw oplossing.  

    ![][6]
   
3. **AppDelegate.cs** Open en voeg de volgende instructie:

        using Microsoft.Azure.Engagement.Xamarin;

4. Voeg de volgende om de betrokkenheid bij de mobiele back-end-verbinding initialiseren in de methode **FinishedLaunching** . Zorg ervoor dat uw **ConnectionString**toevoegen. Deze code wordt ook een dummy **NotificationIcon** die is toegevoegd door de Mobile Engagement SDK die u wilt vervangen. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Realtime-controle inschakelen

Gegevens verzenden en ervoor te zorgen dat de gebruikers actief zijn gestart, moet u ten minste één scherm verzenden op de backend Mobile Engagement.

1. **ViewController.cs** Open en voeg de volgende instructie:

        using Microsoft.Azure.Engagement.Xamarin;

2. Vervangen van de klasse waarvan `ViewController` overgenomen van `UIViewController` op `EngagementViewController`. 

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen inschakelen en messaging-app

Mobile Engagement kunt u interactief werken met de gebruikers en bereiken met push-meldingen en berichten in het kader van de campagnes in app. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
In de volgende secties, stel uw app ontvangen.

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

1. Open het **AppDelegate.cs** en voeg de volgende instructie:

        using System; 

2. Nu in de `FinishedLaunching` methode, voeg de volgende te registreren voor push-berichten na`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Ten slotte - bijwerken of toevoegen van de volgende methoden:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. Bevestig dat de **Bundel-id** overeenkomt met de **App-ID** in het midden van de Dev Apple uw provisioning-profiel in het **Info.plist** -bestand in de oplossing. 

    ![][7]

5. Zorg ervoor dat u de **Achtergrond modi inschakelen** en **Externe meldingen**hebt gecontroleerd in hetzelfde **Info.plist** -bestand. 

    ![][8]

6. De toepassing uitvoeren op het apparaat dat u hebt gekoppeld aan dit profiel publiceren. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
