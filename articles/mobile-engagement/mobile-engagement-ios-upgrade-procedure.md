<properties
    pageTitle="Azure Mobile Engagement iOS SDK Upgrade Procedure | Microsoft Azure"
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

#<a name="upgrade-procedures"></a>Upgradeprocedures

Als u hebt een oudere versie van de overeenkomst al geïntegreerd in uw toepassing, moet u rekening houden met de volgende punten bij het upgraden van de SDK.

Voor elke nieuwe versie van de SDK, moet u eerst vervangen (verwijderen en opnieuw te importeren in xcode) de mappen EngagementSDK en EngagementReach.

##<a name="from-300-to-400"></a>Van 3.0.0 tot 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 is verplicht vanaf versie 4.0.0 van de SDK.

> [AZURE.NOTE] Als u echt afhankelijk van XCode 7 zijn kunt u de [iOS SDK betrokkenheid bij de v3.2.4](https://aka.ms/r6oouh). Er is een bekende bug in de module van het bereiken van deze eerdere versie tijdens de uitvoering van op 10 iOS apparaten: niet systeemmeldingen mailbericht zijn. Los je dit hebt u voor de uitvoering van de verminderde API `application:didReceiveRemoteNotification:` in uw app overdragen als volgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Deze tijdelijke oplossing wordt niet aanbevolen** als dit gedrag kunt wijzigen in een aanstaande (zelfs kleine) iOS versie-upgrade omdat dit iOS API is afgeschaft. U moet zo snel mogelijk overschakelen naar XCode 8.

### <a name="usernotifications-framework"></a>UserNotifications-framework
U moet toevoegen de `UserNotifications` kader in de fasen maken.

in de Projectverkenner het project openen en selecteert u het juiste doel. Vervolgens opent u het tabblad **'Build fasen'** en toevoegen in het menu **"Link binaire met Libraries"** framework `UserNotifications.framework` -de koppeling als instellen`Optional`

### <a name="application-push-capability"></a>Toepassing push-mogelijkheid
XCode-8 mogelijk opnieuw uw app push-mogelijkheid, dubbele Controleer dit de `capability` tabblad van het geselecteerde doel.

### <a name="add-the-new-ios-10-notification-registration-code"></a>De nieuwe iOS 10 melding registratiecode toevoegen
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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Als u al uw eigen implementatie van UNUserNotificationCenterDelegate

De SDK bevat ook een eigen implementatie van het UNUserNotificationCenterDelegate-protocol. Het wordt gebruikt door de SDK voor het controleren van de levenscyclus van de betrokkenheid van meldingen op apparaten die op iOS 10 of hoger worden uitgevoerd. Als u de SDK detecteert uw gemachtigde wordt een eigen implementatie niet gebruikt omdat er slechts één UNUserNotificationCenter gemachtigde per toepassing. Dit betekent dat u de betrokkenheid bij de logica toevoegen aan uw eigen gemachtigde hebt.

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

##<a name="from-200-to-300"></a>Van 2.0.0 aan 3.0.0
Ondersteuning voor iOS neergezet 4.X. Vanaf deze versie het implementatiedoel van de toepassing moet minimaal iOS 6.

Als u bereiken in uw toepassing gebruikt, moet u toevoegen `remote-notification` waarde op de `UIBackgroundModes` matrix in het bestand Info.plist externe meldingen ontvangen.

De methode `application:didReceiveRemoteNotification:` moet worden vervangen door `application:didReceiveRemoteNotification:fetchCompletionHandler:` in uw toepassing gemachtigde.

"AEPushDelegate.h" is afgekeurd-interface en u moet alle verwijzingen verwijderen. Dit omvat het verwijderen van `[[EngagementAgent shared] setPushDelegate:self]` en de methoden van de gemachtigde van de gemachtigde van toepassing:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Van 1.16.0 naar 2.0.0
Hieronder wordt beschreven hoe u voor het migreren van een SDK-integratie van de Capptain-service aangeboden door Capptain SAS in een aangedreven door betrokkenheid bij de Azure Mobile app.
Als u een migratie vanuit een eerdere versie, raadpleegt u de website Capptain te migreren naar 1.16 eerst en vervolgens de volgende procedure van toepassing.

>[AZURE.IMPORTANT] Capptain en Mobile Engagement zijn niet dezelfde diensten en de onderstaande procedure alleen markeert het migreren van de clienttoepassing. Migreren van de SDK in de app wordt niet migreren van uw gegevens de Capptain naar de servers van Mobile Engagement

### <a name="agent"></a>Agent

De methode `registerApp:` is vervangen door de nieuwe methode `init:`. Uw gemachtigde toepassing dienovereenkomstig moeten worden bijgewerkt en gebruikt de verbindingsreeks:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Bijhouden van de SmartAd is verwijderd uit de SDK die u zojuist hebt verwijdert alle exemplaren van `AETrackModule` klasse

### <a name="class-name-changes"></a>Klasse naamswijzigingen

Als onderdeel van de rebranding zijn er enkele klasse/bestandsnamen die moeten worden gewijzigd.

Alle klassen die worden voorafgegaan door 'CP' worden gewijzigd met het voorvoegsel 'AE'.

Voorbeeld:

-   `CPModule.h`de naam is gewijzigd in `AEModule.h`.

Alle klassen die worden voorafgegaan door "Capptain" worden gewijzigd met het voorvoegsel 'Engagement'.

Voorbeelden:

-   De klasse `CapptainAgent` gewijzigd in `EngagementAgent`.
-   De klasse `CapptainTableViewController` gewijzigd in `EngagementTableViewController`.
-   De klasse `CapptainUtils` gewijzigd in `EngagementUtils`.
-   De klasse `CapptainViewController` gewijzigd in `EngagementViewController`.
