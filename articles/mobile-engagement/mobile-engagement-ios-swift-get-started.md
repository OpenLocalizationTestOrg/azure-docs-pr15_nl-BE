<properties
    pageTitle="Aan de slag met Azure Mobile Engagement voor iOS in Swift | Microsoft Azure"
    description="Informatie over het gebruik van Azure Mobile Engagement met Analytics en Push-meldingen voor iOS Apps."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Aan de slag met Azure Mobile Engagement voor iOS Apps in Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp wordt beschreven hoe u Azure Mobile Engagement gebruiken om te begrijpen van het gebruik van app en push-meldingen verzenden naar gesegmenteerde gebruikers naar een iOS-toepassing.
In deze zelfstudie maakt u een lege iOS-app die verzamelt basisgegevens en push-meldingen met behulp van de Apple Push Notification System (APNS) ontvangt.

Deze zelfstudie is het volgende vereist:

+ XCode-8, die u vanuit de MAC App Store installeren kunt
+ de [betrokkenheid bij de mobiele iOS SDK]
+ Push notification certificaat (.p12) die u op uw Apple Dev Center krijgen kunt

> [AZURE.NOTE] In deze zelfstudie wordt de Swift-versie 3.0. 

Voltooien van deze zelfstudie is een vereiste voor alle andere Mobile Engagement zelfstudies voor iOS apps.

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Betrokkenheid bij de mobiele installatie voor uw iOS-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app in de backend Mobile Engagement

Deze zelfstudie bevat een "basic integration", dat is de minimaal vereiste gegevens verzamelen en verzenden van een push-bericht. De volledige integratie van documentatie kan worden gevonden in de [betrokkenheid bij de mobiele iOS SDK integratie](mobile-engagement-ios-sdk-overview.md)

Een eenvoudige app maken we met XCode om aan te tonen van de integratie:

###<a name="create-a-new-ios-project"></a>Maak een nieuw project voor iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinding maken met uw app Mobile Engagement backend

1. De [betrokkenheid bij de mobiele iOS SDK] downloaden
2. Pak het. tar.gz-bestand naar een map op uw computer
3. Klik met de rechtermuisknop op het project en selecteer "Voeg bestanden toe aan..."

    ![][1]

4. Ga naar de map waarin u de SDK en selecteer uitgepakt de `EngagementSDK` map drukt u op OK.

    ![][2]

5. Open de `Build Phases` tabblad en in de `Link Binary With Libraries` menu de kaders toevoegen zoals hieronder wordt weergegeven:

    ![][3]

8. Maken een Bridging-koptekst als u de SDK van doelstelling C API's kunt gebruiken met de opdracht Bestand > Nieuw > Bestand > iOS > bron > Header-bestand.

    ![][4]

9. Bewerk het overbruggen headerbestand blootstellen Mobile Engagement doelstelling-C-code voor de Swift-code, het toevoegen van de volgende invoer:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Controleer of de doelstelling C Bridging kop build onder Compiler Swift - Code genereren als u een pad naar deze header heeft onder instellingen maken. Hier volgt een voorbeeld van een pad: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (afhankelijk van het pad)**

    ![][6]

11. Ga terug naar de portal voor Azure in *Connection Info* -pagina van uw app en kopieert u de verbindingsreeks

    ![][5]

12. Plak nu de verbindingsreeks in de `didFinishLaunchingWithOptions` overdragen

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Realtime-controle inschakelen

Gegevens verzenden en ervoor te zorgen dat de gebruikers actief zijn gestart, moet u ten minste één scherm (activiteit) sturen op de backend Mobile Engagement.

1. Open het bestand **ViewController.swift** en de basisklasse van **ViewController** naar **EngagementViewController**worden vervangen door:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en messaging-app

Mobile Engagement kunt u interactie en bereiken met uw gebruikers met Push-meldingen en Messaging-app in de context van campagnes. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
In de volgende secties wordt uw app voor het ontvangen van deze instellingen.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Uw app Silent Push-meldingen ontvangen inschakelen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>De Reach-bibliotheek aan uw project toevoegen

1. Klik met de rechtermuisknop op uw project
2. Selecteer`Add file to ...`
3. Ga naar de map waarin u de SDK hebt uitgepakt.
4. Selecteer de `EngagementReach` map
5. Klik op toevoegen
6. Het overbruggen headerbestand bewerken als Mobile Engagement doelstelling C bereiken headers worden blootgesteld en voeg de volgende invoer:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

1. In de `didFinishLaunchingWithOptions` - Maak een reach-module en doorgegeven aan de bestaande betrokkenheid bij de initialisatie regel:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Uw app APNS Push-meldingen ontvangen inschakelen
1. Voeg de volgende regel aan de `didFinishLaunchingWithOptions` methode:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Voeg toe de `didRegisterForRemoteNotificationsWithDeviceToken` methode als volgt:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Voeg toe de `didReceiveRemoteNotification:fetchCompletionHandler:` methode als volgt:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Betrokkenheid bij de mobiele iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
