<properties
    pageTitle="Azure Mobile Engagement iOS SDK-overzicht | Microsoft Azure"
    description="Meest recente updates en procedures voor de iOS SDK voor Azure Mobile Engagement"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK voor Azure Mobile Engagement

Start hier om alle details over het integreren van Azure Mobile Engagement in een iOS-App. Als u wilt eens proberen eerst, zorg ervoor dat doen u onze [zelfstudie 15 minuten](mobile-engagement-ios-get-started.md).

Klik hier voor een overzicht van de [SDK Content](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Integratie-procedures
1. Begin hier: [het integreren van Mobile Engagement in de iOS-app](mobile-engagement-ios-integrate-engagement.md)

2. Voor meldingen: [het integreren van Reach (meldingen) in uw iOS-app](mobile-engagement-ios-integrate-engagement-reach.md)

3. Label plan uitvoering: [het gebruik van geavanceerde mobiele aanstelling tagging API in de iOS-app](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Release-opmerkingen

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Vaste melding niet mailbericht op 10 iOS-apparaten.
-   XCode 7 afschaffen.

Zie de [volledige release-opmerkingen](mobile-engagement-ios-release-notes.md) voor een eerdere versie

##<a name="upgrade-procedures"></a>Upgradeprocedures

Als u hebt een oudere versie van de overeenkomst al geïntegreerd in uw toepassing, moet u rekening houden met de volgende punten bij het upgraden van de SDK.

Mogelijk moet verschillende procedures volgen als u verschillende versies van de SDK-Zie de volledige [Procedures Upgrade](mobile-engagement-ios-upgrade-procedure.md)gemist.

Voor elke nieuwe versie van de SDK, moet u eerst vervangen (verwijderen en opnieuw te importeren in xcode) de mappen EngagementSDK en EngagementReach.

###<a name="from-300-to-400"></a>Van 3.0.0 tot 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 is verplicht vanaf versie 4.0.0 van de SDK.

> [AZURE.NOTE] Als u echt afhankelijk van XCode 7 zijn kunt u de [iOS SDK betrokkenheid bij de v3.2.4](https://aka.ms/r6oouh). Er is een bekende bug in de module van het bereiken van deze eerdere versie tijdens de uitvoering van op 10 iOS apparaten: niet systeemmeldingen mailbericht zijn. Los je dit hebt u voor de uitvoering van de verminderde API `application:didReceiveRemoteNotification:` in uw app overdragen als volgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Deze tijdelijke oplossing wordt niet aanbevolen** als dit gedrag kunt wijzigen in een aanstaande (zelfs kleine) iOS versie-upgrade omdat dit iOS API is afgeschaft. U moet zo snel mogelijk overschakelen naar XCode 8.

#### <a name="usernotifications-framework"></a>UserNotifications-framework
U moet toevoegen de `UserNotifications` kader in de fasen maken.

in de Projectverkenner het project openen en selecteert u het juiste doel. Vervolgens opent u het tabblad **'Build fasen'** en toevoegen in het menu **"Link binaire met Libraries"** framework `UserNotifications.framework` -de koppeling als instellen`Optional`

#### <a name="application-push-capability"></a>Toepassing push-mogelijkheid
XCode-8 mogelijk opnieuw uw app push-mogelijkheid, dubbele Controleer dit de `capability` tabblad van het geselecteerde doel.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>De nieuwe iOS 10 melding registratiecode toevoegen
Oudere codefragment voor het registreren van de app om meldingen te werkt nog steeds, maar tijdens het uitvoeren van de iOS 10 afgeschafte API's gebruikt. 

Importeren van de `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

In uw toepassing gemachtigde `application:didFinishLaunchingWithOptions` methode vervangen:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

door:

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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Als u al uw eigen implementatie van UNUserNotificationCenterDelegate

De SDK bevat een eigen implementatie van het protocol van UNUserNotificationCenterDelegate. Het wordt gebruikt door de SDK voor het controleren van de levenscyclus van de betrokkenheid van meldingen op apparaten die op iOS 10 of hoger worden uitgevoerd. Als u de SDK detecteert uw gemachtigde wordt een eigen implementatie niet gebruikt omdat er slechts één UNUserNotificationCenter gemachtigde per toepassing. Dit betekent dat u de betrokkenheid bij de logica toevoegen aan uw eigen gemachtigde hebt.

Er zijn twee manieren om dit te bereiken.

Roept de SDK door simpelweg uw gemachtigde te sturen:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Of door het overnemen van de `AEUserNotificationHandler` klasse

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] U kunt bepalen of een bericht afkomstig is van betrokkenheid of niet door de `userInfo` woordenlijst aan de Agent `isEngagementPushPayload:` klasse methode.