<properties
    pageTitle="Aan de slag met Azure Mobile Engagement voor iOS in doelstelling C | Microsoft Azure"
    description="Informatie over het gebruik van Azure Mobile Engagement met analytics en push-meldingen voor iOS-apps."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Aan de slag met Azure Mobile Engagement voor iOS-apps in doelstelling C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp wordt beschreven hoe u Azure Mobile Engagement gebruiken om te begrijpen van het gebruik van app en push-meldingen verzenden naar gesegmenteerde gebruikers naar een iOS-toepassing.
In deze zelfstudie maakt u een lege iOS-app die verzamelt basisgegevens en push-meldingen met behulp van de Apple Push Notification System (APNS) ontvangt.

Deze zelfstudie is het volgende vereist:

+ XCode-8, die u vanuit de MAC App Store installeren kunt
+ de [betrokkenheid bij de mobiele iOS SDK]

Voltooien van deze zelfstudie is een vereiste voor alle andere Mobile Engagement zelfstudies voor iOS apps.

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Betrokkenheid bij de mobiele installatie voor uw iOS-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app in de backend Mobile Engagement

Deze zelfstudie bevat een "basic integration", dat is de minimaal vereiste gegevens verzamelen en verzenden van een push-bericht. De volledige integratie van documentatie kan worden gevonden in de [betrokkenheid bij de mobiele iOS SDK integratie](mobile-engagement-ios-sdk-overview.md)

Een eenvoudige app maken we met XCode om aan te tonen van de integratie.

###<a name="create-a-new-ios-project"></a>Maak een nieuw project voor iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinding maken met uw app in de backend Mobile Engagement

1. De [betrokkenheid bij de mobiele iOS SDK]downloaden.
2. Pak het. tar.gz-bestand naar een map op uw computer.
3. Klik met de rechtermuisknop op het project en selecteer de **bestanden die u wilt toevoegen**.

    ![][1]

4. Ga naar de map waarin u de SDK, selecteer uitgepakt de `EngagementSDK` map en druk vervolgens op **OK**.

    ![][2]

5. Open het tabblad **Fasen maken** en toevoegen in het menu **Binaire koppeling met bibliotheken** de frameworks zoals hieronder wordt weergegeven:

    ![][3]

6. Ga terug naar de portal voor Azure in **Connection Info** -pagina van uw app en kopieer de verbindingsreeks.

    ![][4]

7. Voeg de volgende regel code in het bestand **AppDelegate.m** .

        #import "EngagementAgent.h"

8. Plak nu de verbindingsreeks in de `didFinishLaunchingWithOptions` overdragen.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`is een optionele instructie waarmee u logboeken van de SDK voor u problemen identificeren. 

##<a id="monitor"></a>Realtime-controle inschakelen

Gegevens verzenden en ervoor te zorgen dat de gebruikers actief zijn gestart, moet u ten minste één scherm (activiteit) sturen op de backend Mobile Engagement.

1. Open het bestand **ViewController.h** en **EngagementViewController.h**te importeren:

    `# import "EngagementViewController.h"`

2. Nu vervangen door de bovenliggende klasse van de interface **ViewController** door `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen inschakelen en messaging-app

Mobile Engagement kunt u interactief werken met de gebruikers en bereiken met push-meldingen en berichten in het kader van de campagnes in app. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
In de volgende secties, stel uw app ontvangen.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Uw app Silent Push-meldingen ontvangen inschakelen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>De Reach-bibliotheek aan uw project toevoegen

1. Met de rechtermuisknop op het project.
2. Selecteer **bestand dat u wilt toevoegen**.
3. Ga naar de map waarin u de SDK hebt uitgepakt.
4. Selecteer de `EngagementReach` map.
5. Klik op **toevoegen**.

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

1. Terug in **AppDeletegate.m** -bestand importeren de module Engagement bereiken.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. In de `application:didFinishLaunchingWithOptions` methode, maak een Reach-module en doorgegeven aan de bestaande betrokkenheid bij de initialisatie regel:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Uw app APNS Push-meldingen ontvangen inschakelen

1. Voeg de volgende regel aan de `application:didFinishLaunchingWithOptions` methode:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Voeg toe de `application:didRegisterForRemoteNotificationsWithDeviceToken` methode als volgt:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Voeg toe de `didFailToRegisterForRemoteNotificationsWithError` methode als volgt:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Voeg toe de `didReceiveRemoteNotification:fetchCompletionHandler` methode als volgt:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Betrokkenheid bij de mobiele iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

