<properties
    pageTitle="Hubs melding laatste nieuws zelfstudie - iOS"
    description="Leren werken met Azure Service Bus melding Hubs recente nieuws om meldingen te verzenden naar iOS-apparaten."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Melding Hubs gebruiken voor het verzenden van laatste nieuws

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u Azure melding Hubs gebruiken voor het uitzenden van recente nieuws meldingen naar een iOS-app. Als alles klaar is, u kunnen registreren voor het breken van nieuwscategorieën waarin u geïnteresseerd bent, en alleen push meldingen ontvangen voor deze categorieën. Dit scenario is een gemeenschappelijk patroon voor veel toepassingen waar meldingen moeten worden verzonden naar groepen van gebruikers die eerder gedeclareerd belangstelling voor hen, zoals RSS-lezer, apps voor muziek, ventilatoren, enz.

Broadcast-scenario's zijn ingeschakeld door een of meer _tags_ bij het maken van een inschrijving in de kennisgeving hub. Wanneer berichten worden verzonden naar een tag, ontvangt alle apparaten die zijn geregistreerd voor de code de melding. Omdat codes gewoon tekenreeksen zijn, hoeven niet van tevoren worden ingericht. Raadpleeg [melding Hubs en Tag expressies](notification-hubs-tags-segment-push-message.md)voor meer informatie over tags.


##<a name="prerequisites"></a>Vereisten

Dit onderwerp is gebaseerd op de toepassing die u hebt gemaakt in het [aan de slag met Hubs melding][get-started]. Voordat u deze zelfstudie begint, moet reeds hebt voltooid [aan de slag met Hubs melding][get-started].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande storyboard waarmee de gebruiker kan de categorieën selecteren om te registreren. De categorieën die zijn geselecteerd door een gebruiker zijn op het apparaat opgeslagen. Wanneer de toepassing wordt gestart, wordt de apparaatregistratie van een in uw melding hub met geselecteerde categorieën als codes gemaakt.

1. Voeg de volgende onderdelen in de objectbibliotheek van uw MainStoryboard_iPhone.storyboard:
    + Een label met de tekst "Nieuws te analyseren",
    + Labels met Categorieteksten 'World', 'Politiek', 'Zakelijk', "Technology", "Wetenschap", "Sports",
    + Zes switches, één per categorie, elke switch **staat** te **zijn standaard** ingesteld.
    + Een knop met het label "Abonneren"

    Een storyboard moet er als volgt uitzien:

    ![][3]

2. In de Office-assistent-editor voor alle switches afzetmogelijkheden te maken en het aanroepen van "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Een actie maken voor de knop "abonneren" genoemd. De ViewController.h moet het volgende bevatten:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Maakt een nieuwe **Cacao Touch klasse** `Notifications`. Kopieer de volgende code in het gedeelte van de interface van het bestand Notifications.h:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. De volgende instructie voor invoer toevoegen aan Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Kopieer de volgende code in de sectie toepassing van het bestand Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Deze klasse gebruikt de lokale opslag opslaan en ophalen van de categorieën van het nieuws dat dit apparaat ontvangt. Het bevat ook een methode om te registreren voor deze categorieën met behulp van de registratie van een [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) .

7. In het bestand AppDelegate.h een importinstructie toevoegen voor Notifications.h en een eigenschap voor een instantie van de klasse meldingen toevoegen:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. De methode **didFinishLaunchingWithOptions** in AppDelegate.m, voeg de code voor het initialiseren van het exemplaar van de meldingen aan het begin van de methode.  
 
    `HUBNAME`en `HUBLISTENACCESS` (gedefinieerd in hubinfo.h) moeten al de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen vervangen door de naam van de hub van uw melding en de verbindingsreeks voor de *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Omdat de referenties die worden gedistribueerd met een client-app niet in het algemeen veilig zijn, moet u de sleutel voor het luisteren naar access alleen met uw app client distribueren. Access maakt de app te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd luisteren en meldingen kunnen niet worden verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-end-service voor het verzenden van meldingen en bestaande registraties wijzigen.


9. Vervang in de methode **didRegisterForRemoteNotificationsWithDeviceToken** in AppDelegate.m, de code in de methode met de volgende code naar het apparaat token aan de klasse meldingen doorgeven. De klasse meldingen wordt uitgevoerd de registratie voor meldingen met de categorieën. Als de gebruiker categorieselecties verandert, noemen we de `subscribeWithCategories` -methode in reactie op de knop **abonneren** te werken.

    > [AZURE.NOTE] Omdat het apparaat token toegewezen door de Apple Push Notification Service (APNS) op elk gewenst moment kunt kans, dient u te registreren voor meldingen regelmatig om te voorkomen dat storingen melding. In dit voorbeeld registreert voor melding elke keer dat de toepassing wordt gestart. Voor toepassingen die vaak worden uitgevoerd, kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Houd er rekening mee dat op dit punt moet er geen andere code in de methode **didRegisterForRemoteNotificationsWithDeviceToken** .

10. De volgende methoden moeten al aanwezig zijn in AppDelegate.m van de voltooiing van de [aan de slag met Hubs melding] [ get-started] zelfstudie.  Als dit niet het geval is, voegt u deze toe.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Deze methode zorgt voor meldingen ontvangen wanneer de toepassing wordt uitgevoerd door een eenvoudige **UIAlert**weer te geven.

11. Een importinstructie toe te voegen voor AppDelegate.h in ViewController.m, en kopieer de volgende code in de methode XCode gegenereerd **abonneren** . Deze code wordt de registratie voor het gebruik van de nieuwe categorie tags die de gebruiker heeft gekozen in de gebruikersinterface bijgewerkt.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Deze methode maakt een **NSMutableArray** van de categorieën en de klasse **meldingen** gebruikt voor het opslaan van de lijst in de lokale opslag en registreert de corresponderende codes op uw hub-melding. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

12. ViewController.m, voeg de volgende code in de methode **viewDidLoad** stelt de gebruikersinterface op basis van de eerder opgeslagen categorieën.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



De app kan een set categorieën nu opgeslagen in de lokale opslag van apparaat gebruikt om u te registreren bij de hub melding wanneer de toepassing wordt gestart.  De gebruiker kan de selectie van categorieën in runtime te wijzigen en klikt u op de methode **abonneren** om de registratie van het apparaat wordt bijgewerkt. Vervolgens moet u de app het laatste nieuws om meldingen te verzenden direct in de app zelf bijwerken.


##<a name="optional-sending-tagged-notifications"></a>(optioneel) Gecodeerde berichten verzenden

Als u geen toegang tot Visual Studio hebt, kunt u naar de volgende sectie overslaan en meldingen verzenden vanuit de app zelf. U kunt de juiste sjabloon melding ook verzenden vanuit [Azure klassieke Portal] met behulp van het tabblad foutopsporing voor uw melding hub. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(optioneel) Meldingen van het apparaat verzenden

Normaal gesproken zou meldingen worden verzonden door een back-end-service, maar recente nieuws om meldingen te verzenden direct vanuit de app. Dit zal worden bijgewerkt het `SendNotificationRESTAPI` methode die is gedefinieerd in de [aan de slag met Hubs melding] [ get-started] zelfstudie.


1. Update ViewController.m de `SendNotificationRESTAPI` de methode volgt zodat deze accepteert een parameter voor de categorie-tag en de juiste [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) melding heeft verzonden.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }



2. Update in ViewController.m de actie **Bericht verzenden** in de volgende code. Zodat het wordt met behulp van elk label afzonderlijk berichten verzenden en naar meerdere platforms verzenden.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Het project opnieuw en controleer of er geen fouten maken.


##<a name="run-the-app-and-generate-notifications"></a>De toepassing uitvoeren en meldingen genereren

1. Druk op de knop uitvoeren voor het bouwen van het project en de app start. Enkele recente nieuws opties selecteren om te abonneren op en druk vervolgens op de knop **abonneren** . U ziet een dialoogvenster dat aangeeft van dat de kennisgevingen zijn geabonneerd.

    ![][1]

    Wanneer u **abonneren**kiest, wordt de app zet de geselecteerde categorieën in tags en vraagt om een nieuwe registratie voor de geselecteerde codes van de hub van de kennisgeving.

2. Voer een bericht wordt verzonden als het laatste nieuws en vervolgens drukt u op de knop **Bericht verzenden** . Ook de .NET console app om meldingen te genereren uitvoeren.

    ![][2]


3. Elk apparaat geabonneerd op belangrijk nieuws, ontvangt de recente nieuws meldingen die u zojuist hebt verzonden.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de laatste nieuws uitzending per categorie. Houd rekening met voltooien van een van de volgende zelfstudies die andere geavanceerde scenario's voor melding Hubs markeren:

+ **[Melding Hubs gebruiken voor het uitzenden van gelokaliseerde laatste nieuws]**

    Informatie over het uitbreiden van het laatste nieuws app zodat gelokaliseerde verzenden van meldingen.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Melding Hubs gebruiken voor het uitzenden van gelokaliseerde laatste nieuws]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure klassieke Portal]: https://manage.windowsazure.com
