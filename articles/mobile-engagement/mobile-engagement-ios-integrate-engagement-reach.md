<properties
    pageTitle="Azure Mobile Engagement iOS SDK bereiken integratie | Microsoft Azure"
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

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Het bereiken van betrokkenheid bij de integratie op iOS

U moet de integratie-procedure beschreven in [het Engagement op iOS document integreren](mobile-engagement-ios-integrate-engagement.md) voordat u deze handleiding volgen.

Deze documentatie is 8 XCode vereist. Als u echt afhankelijk van XCode 7 zijn kunt u de [iOS SDK betrokkenheid bij de v3.2.4](https://aka.ms/r6oouh). Er is een bekende bug in deze vorige versie tijdens de uitvoering van op 10 iOS apparaten: niet systeemmeldingen mailbericht zijn. Los je dit hebt u voor de uitvoering van de verminderde API `application:didReceiveRemoteNotification:` in uw app overdragen als volgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Deze tijdelijke oplossing wordt niet aanbevolen** als dit gedrag kunt wijzigen in een aanstaande (zelfs kleine) iOS versie-upgrade omdat dit iOS API is afgeschaft. U moet zo snel mogelijk overschakelen naar XCode 8.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Uw app Silent Push-meldingen ontvangen inschakelen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Stappen voor gegevensintegratie

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>De SDK voor het bereiken van betrokkenheid insluiten in uw iOS-project

-   De sdk bereiken toevoegen in uw project Xcode. In Xcode, gaat u naar **Project \> toevoegen aan het project** en kies de `EngagementReach` map.

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

-   Aan de bovenkant van uw implementatiebestand importeren het bereiken van betrokkenheid bij de module:

        [...]
        #import "AEReachModule.h"

-   Binnen de methode `applicationDidFinishLaunching:` of `application:didFinishLaunchingWithOptions:`, maak een reach-module en doorgegeven aan de bestaande betrokkenheid bij de initialisatie regel:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   **'Icon.png'** tekenreeks met de naam van de afbeelding die u wilt gebruiken als het pictogram wijzigen.
-   Als u wilt dat de optie *waarde badge* in Reach campagnes te gebruiken of als u met native push \<SaaS Reach/API/campagne indeling Native Push\> campagnes, moet u het bereik module beheren de badge pictogram zelf (wordt automatisch wissen van de badge van de toepassing en ook opnieuw instellen de waarde opgeslagen door de betrokkenheid van elke keer dat de toepassing wordt gestart of foregrounded) laten. Dit wordt gedaan door de volgende regel toe te voegen na bereiken module initialiseren:

        [reach setAutoBadgeEnabled:YES];

-   Als u bereiken gegevens push verwerken wilt, moet u laten de toepassing gemachtigde in overeenstemming zijn met de `AEReachDataPushDelegate` protocol. Voeg de volgende regel na bereiken module initialiseren:

        [reach setDataPushDelegate:self];

-   Vervolgens kunt u de methoden implementeert `onDataPushStringReceived:` en `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in uw toepassing gemachtigde:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Categorie

De categorieparameter is optioneel wanneer u een campagne Push-gegevens maken en Hiermee kunt dat u filtergegevens duwt. Dit is handig als u verschillende soorten push van `Base64` gegevens en wilt u het type te identificeren voordat ze worden geparseerd.

**De toepassing is nu klaar om te ontvangen en weergeven van de inhoud van het bereik!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Het ontvangen van meldingen en stemmingen op elk gewenst moment

Betrokkenheid kunt bereiken meldingen verzenden aan uw gebruikers op elk gewenst moment met behulp van de Apple Push Notification Service.

Als u deze functies inschakelt, hebt u voor het voorbereiden van uw toepassing voor Apple push-meldingen en uw gemachtigde toepassing wijzigen.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Voorbereiden van uw toepassing voor Apple push-meldingen

Volg de gids: [het voorbereiden van uw toepassing voor Apple Push-meldingen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>De vereiste code toevoegen

*De toepassing heeft nu een geregistreerd certificaat voor Apple push in de betrokkenheid bij de frontend.*

Als dit nog niet gedaan wordt, moet u het registreren van uw toepassing om push-meldingen te ontvangen.

* Importeren van de `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

* Voeg de volgende regel wanneer de toepassing wordt gestart (meestal in `application:didFinishLaunchingWithOptions:`):

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

Moet u voor de betrokkenheid van het apparaat-token die wordt geretourneerd door de Apple-servers. Dit gebeurt in de methode met de naam `application:didRegisterForRemoteNotificationsWithDeviceToken:` in uw toepassing gemachtigde:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Ten slotte hebt u de betrokkenheid bij de SDK op de hoogte wanneer uw toepassing een externe kennisgeving ontvangt. Hiertoe roept u de methode `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in uw toepassing gemachtigde:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] De bovenstaande methode is geïntroduceerd in iOS 7. Voor iOS < 7, zorg voor de uitvoering van de methode `application:didReceiveRemoteNotification:` in uw toepassing gemachtigde en de aanroep van `applicationDidReceiveRemoteNotification` op de EngagementAgent door een nul in plaats van de `handler` argument:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Standaard bepaalt overeenkomst bereiken de completionHandler. Als u handmatig te reageren wilt op de `handler` blokkeren in uw code, kunt u nul doorgeeft voor de `handler` argument en voltooiing van het besturingselement zelf blokkeren. Zie de `UIBackgroundFetchResult` type voor een lijst van mogelijke waarden.


### <a name="full-example"></a>Voorbeeld van de volledige

Hier volgt een voorbeeld van de volledige integratie van:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Als u uw eigen implementatie van UNUserNotificationCenterDelegate

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

##<a name="how-to-customize-campaigns"></a>Het aanpassen van campagnes

### <a name="notifications"></a>Meldingen

Er zijn twee soorten meldingen: systeem en in de app-meldingen.

Systeemmeldingen worden afgehandeld door iOS en kunnen niet worden aangepast.

In de app-meldingen zijn gemaakt van een weergave die dynamisch wordt toegevoegd aan het huidige venster. Dit heet een overlay melding. Melding overlays zijn ideaal voor een snelle integratie omdat zij u niet hoeft te wijzigen in de weergaven van uw toepassing.

#### <a name="layout"></a>Lay-out

Als u wilt wijzigen het uiterlijk van uw berichten-app, kunt u het bestand gewoon wijzigen `AENotificationView.xib` aan uw wensen, zolang u de labelwaarden en de bestaande subweergaven soorten houden.

Standaard worden in app-meldingen onder aan het scherm weergegeven. Als u liever ze aan de bovenkant van het scherm weergeven, bewerken de aangeleverde `AENotificationView.xib` en wijzig de `AutoSizing` eigenschap van de hoofdweergave zodat deze kan worden gehouden boven de superview.

#### <a name="categories"></a>Categorieën

Als u de opgegeven lay-out wijzigt, wijzigt u het uiterlijk van alle kennisgevingen in de. Categorieën kunnen u verschillende gerichte uiterlijk (eventueel gedrag) voor berichten definiëren. Een categorie kan worden opgegeven bij het maken van een campagne bereiken. Houd er rekening mee dat categorieën u ook aanpassen kunnen, aankondigingen en stemmingen, die verderop in dit document wordt beschreven.

Een handler categorie voor uw meldingen registreren, moet u een gesprek toevoegen wanneer de module reach is geïnitialiseerd.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`moet een exemplaar van een object dat in overeenstemming is met het protocol `AENotifier`.

U kunt de protocolmethoden implementeren door uzelf of kunt u de bestaande klasse reimplement `AEDefaultNotifier` al uitvoert, wordt het merendeel van het werk.

Als u wilt dat de weergave van de melding voor een specifieke categorie te definiëren, kunt u bijvoorbeeld in dit voorbeeld volgen:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Dit eenvoudige voorbeeld van categorie wordt ervan uitgegaan dat er een bestand met de naam `MyNotificationView.xib` in het hoofdvenster van de toepassing bundel. Als de methode niet kunt vinden op een bijbehorend `.xib`, wordt het bericht niet weergegeven en Engagement wordt een bericht weergegeven in de console-uitvoer.

Het bestand opgegeven kroontjespen met inachtneming van de volgende regels:

-   Slechts moet één weergave bevatten.
-   Subweergaven moeten van hetzelfde type als die in het bestand opgegeven kroontjespen naam`AENotificationView.xib`
-   Subweergaven hebben dezelfde tags hebben als die in het opgegeven bestand kroontjespen`AENotificationView.xib`

> [AZURE.TIP] Gewoon kopiëren van het bestand opgegeven kroontjespen, met de naam `AENotificationView.xib`, en start vanaf daar werken. Maar wees voorzichtig, wordt de weergave binnen deze kroontjespen-bestand is gekoppeld aan de klasse `AENotificationView`. Deze klasse opnieuw gedefinieerd voor de methode `layoutSubViews` te verplaatsen en het formaat van de subweergaven volgens context. U kunt vervangen door een `UIView` of u aangepaste weergave klasse.

Als u diepere aanpassing van uw berichten (als u bijvoorbeeld wilt dat uw weergave van de code rechtstreeks laden), wordt aanbevolen om Kijk eens naar de opgegeven bron code- en documentatie van `Protocol ReferencesDefaultNotifier` en `AENotifier`.

Houd er rekening mee dat u de kennisgever dezelfde voor meerdere categorieën gebruiken kunt.

U kunt ook de kennisgever standaard als volgt gedefinieerd:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Melding afhandelen

Als u de standaardcategorie, sommige levenscyclus-methoden worden aangeroepen op de `AEReachContent` object geven statistieken weer en de status van de campagne:

-   Wanneer het bericht wordt weergegeven in de toepassing, de `displayNotification` methode is aangeroepen (waarvoor statistieken) door `AEReachModule` als `handleNotification:` geeft als resultaat `YES`.
-   Als het bericht wordt gesloten, de `exitNotification` methode wordt aangeroepen, statistiek wordt gerapporteerd en volgende campagnes kunnen nu worden verwerkt.
-   Als u de melding klikt, `actionNotification` wordt aangeroepen, statistiek wordt gerapporteerd en de gekoppelde actie wordt uitgevoerd.

Als uw implementatie van `AENotifier` het standaardgedrag omzeilt u moet deze methoden levenscyclus zelf bellen. De volgende voorbeelden ziet enkele gevallen waar het standaardgedrag wordt genegeerd:

-   Reiken niet `AEDefaultNotifier`, b.v. u categorie verwerking helemaal geïmplementeerd.
-   U overrode `prepareNotificationView:forContent:`, moet u ten minste wijzen `onNotificationActioned` of `onNotificationExited` aan een van uw U.I bepaalt.

> [AZURE.WARNING] Als `handleNotification:` genereert een uitzondering, de inhoud wordt verwijderd en `drop` wordt aangeroepen, wordt dit gemeld in de statistieken en volgende campagnes kunnen nu worden verwerkt.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Melding als onderdeel van een bestaande weergave opnemen

Overlays kunnen zijn ideaal voor een snelle integratie, maar soms niet handig of ongewenste neveneffecten hebben.

Als u niet tevreden met het systeem overlay in sommige van uw weergaven bent, kunt u deze aanpassen voor deze weergaven.

U kunt onze indeling kennisgeving opnemen in uw bestaande weergaven. Hiervoor is er twee stijlen van toepassing:

1.  De weergave van de melding met interface builder toevoegen

    -   Open *Interface Builder*
    -   Plaats een 320 x 60 (of als u op de iPad 768 x 60) `UIView` waar u de melding worden weergegeven
    -   De waarde van de Tag voor deze weergave wilt instellen: **36822491**

2.  De weergave van meldingen via programmering toevoegen. Voeg de volgende code alleen als de weergave is geïnitialiseerd:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`macro kan worden gevonden in `AEDefaultNotifier.h`.

> [AZURE.NOTE] De kennisgever standaard detecteert automatisch dat de indeling van het bericht in deze weergave is opgenomen en niet een overlay voor het toevoegen wordt.

### <a name="announcements-and-polls"></a>Aankondigingen en polls

#### <a name="layouts"></a>Lay-outs

U kunt de bestanden wijzigen `AEDefaultAnnouncementView.xib` en `AEDefaultPollView.xib` als u de labelwaarden en de bestaande subweergaven soorten houden.

#### <a name="categories"></a>Categorieën

##### <a name="alternate-layouts"></a>Alternatieve indelingen

Zoals mededelingen, kan de categorie van de campagne men hebben ook alternatieve indelingen voor uw aankondigingen en stemmingen.

Om een categorie voor een aankondiging maakt, moet u de **AEAnnouncementViewController** uitbreiden en te registreren nadat de reach-module is geïnitialiseerd:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Telkens wanneer een gebruiker klikt op een kennisgeving voor een aankondiging met de categorie op "Mijn\_categorie", de geregistreerde view-controller (in dat geval `MyCustomAnnouncementViewController`) wordt geïnitialiseerd door het aanroepen van de methode `initWithAnnouncement:` en de weergave wordt toegevoegd aan het huidige toepassingsvenster.

Bij de implementatie van de `AEAnnouncementViewController` hebt u voor het lezen van de eigenschap klasse `announcement` uw subweergaven initialiseren. Houd rekening met het volgende voorbeeld, waarbij twee labels zijn geïnitialiseerd met behulp van `title` en `body` eigenschappen van de `AEReachAnnouncement` klasse:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Als u niet wilt dat uw weergaven laden zelf, maar u wilt gebruiken de standaard aankondiging lay-out, kunt u gewoon uw controller aangepaste weergave breidt de opgegeven klasse `AEDefaultAnnouncementViewController`. In dat geval het bestand kroontjespen dubbele `AEDefaultAnnouncementView.xib` en wijzig de naam zodat deze kan worden geladen door de aangepaste weergave-controller (voor een domeincontroller met de naam `CustomAnnouncementViewController`, moet u het bestand kroontjespen aanroepen `CustomAnnouncementView.xib`).

Ter vervanging van de standaardcategorie van aankondigingen te registreren voor de categorie die is opgegeven de aangepaste weergave-controller `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Polls kunnen op dezelfde manier worden aangepast:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Deze keer, de geleverde `MyCustomPollViewController` moet uitbreiden `AEPollViewController`. Of u kunt uitbreiden van de standaard-controller: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Vergeet niet om aan te roepen van een `action` (`submitAnswers:` voor aangepaste peiling weergave controllers) of `exit` methode voordat u de weergave-controller wordt gesloten. Anders statistieken niet verzonden (dat wil zeggen geen analytics van de campagne) en meer belangrijker is in volgende campagnes niet krijgt als het proces van de toepassing opnieuw wordt gestart.

##### <a name="implementation-example"></a>Voorbeeld van de implementatie

In deze implementatie is de aankondiging van aangepaste weergave van een externe xib-bestand geladen.

Net als voor melding van geavanceerde aanpassingen, wordt het aanbevolen te kijken naar de broncode van de standaardimplementatie.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
