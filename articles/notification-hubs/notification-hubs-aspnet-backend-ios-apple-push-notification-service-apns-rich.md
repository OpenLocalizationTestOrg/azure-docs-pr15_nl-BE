<properties
    pageTitle="Azure melding Hubs Rich Push"
    description="Informatie over het uitgebreide push-meldingen verzenden naar een iOS-app van Azure. Voorbeelden van code in de doelstelling-C en C# is geschreven."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure melding Hubs Rich Push


##<a name="overview"></a>Overzicht

Om gebruikers met instant rijke inhoud wordt benaderd, wilt een toepassing duwen dan tekst zonder opmaak. Deze meldingen bevorderen gebruikersinteracties en presentatie van inhoud zoals URL's, geluiden en afbeeldingen/coupons. Deze zelfstudie is gebaseerd op het onderwerp [Gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) en ziet u hoe u de push-meldingen met payloads (bijvoorbeeld afbeeldingen) te verzenden.


Deze zelfstudie is compatibel met iOS 7 & 8.

  ![][IOS1]

Op een hoog niveau:

1. De backend app:
    - De uitgebreide nettolading worden opgeslagen (in dit geval image) in de backend-database/lokale opslag
    - ID van dit uitgebreide bericht verzendt naar het apparaat
2. App op het apparaat:
    - Contact op met de back-end de uitgebreide nettolading met de ID ontvangen aanvragen
    - Hiermee worden gebruikers meldingen verzonden op het apparaat bij het ophalen van gegevens is voltooid en de nettolading wordt weergegeven zodra gebruikers Tik voor meer informatie


## <a name="webapi-project"></a>WebAPI Project

1. Open het project met **AppBackend** die u hebt gemaakt in de zelfstudie [Gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) in Visual Studio.
2. Een afbeelding die u wilt gebruikers met kennis en deze in een **img** -map in de projectmap verkrijgen.
3. Klik op **Alle bestanden weergeven** in de Solution Explorer en klik met de rechtermuisknop op de map die u wilt **Opnemen In Project**.
4. De afbeelding is geselecteerd, wijzigen de Build Action in het venster Eigenschappen op **Embedded Resource**.

    ![][IOS2]

5. In **Notifications.cs**, voeg de volgende instructie:

        using System.Reflection;

6. De gehele klasse van **meldingen** bijwerken met de volgende code. Zorg ervoor dat de tijdelijke aanduidingen vervangen door uw melding hub referenties en de bestandsnaam van de afbeelding.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (optioneel) Raadpleeg [insluiten en toegang tot bronnen met behulp van Visual C#](http://support.microsoft.com/kb/319292) voor meer informatie over het toevoegen en projectbronnen krijgen.

7. In **NotificationsController.cs**, **NotificationsController** met de volgende fragmenten opnieuw worden gedefinieerd. Dit een stille rijke vroegtijdig-id verzonden naar apparaat en staat aan de clientzijde voor het ophalen van afbeelding:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. We zullen dit app naar een Website Azure nu opnieuw te implementeren zodat het toegankelijk is vanaf alle apparaten. Klik met de rechtermuisknop op het **AppBackend** -project en selecteer **publiceren**.

9. Azure-Website selecteren als het doel van uw publicatie. Meld u aan met uw account Azure en selecteert u een bestaande of nieuwe Website en maak een notitie van de **doel-URL** -eigenschap in op het tabblad **verbinding** . Wordt verwezen naar deze URL als uw *back-end eindpunt* verderop in deze handleiding. Klik op **publiceren**.

## <a name="modify-the-ios-project"></a>Het iOS-project wijzigen

Nu dat u uw app back-end om alleen de *id* van een melding verzenden hebt gewijzigd, wijzigt u uw iOS-app om te verwerken die id en het uitgebreide bericht ophalen uit uw back-end.

1. Open het project iOS en externe meldingen inschakelen door te gaan naar het doel van de belangrijkste app in de sectie **doelen** .

2. Klik op **functies**, **Achtergrond modi**inschakelen en schakel het selectievakje **Externe meldingen** in.

    ![][IOS3]

3. Ga naar **Main.storyboard**en controleer of er een View-Controller (zoals als Home View Controller in deze zelfstudie) van [Gebruiker inlichten](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) zelfstudie.

4. Een **Controller voor navigatie** toevoegen aan het storyboard en control ingedrukt houden en op Home View Controller om de **weergave van de hoofdmap** van de navigatie. Zorg ervoor dat de **Eerste weergave Controller Is** in kenmerken Inspector-venster is geselecteerd voor de Controller navigatie alleen.

5. Een **Weergave Controller** storyboard en een **Afbeelding**toevoegen als u wilt toevoegen. Dit is de pagina die gebruikers zien wanneer ze ervoor kiezen om meer te weten door te klikken op de notifiication. Een storyboard moet er als volgt uitzien:

    ![][IOS4]

6. Klik op de **Home View-Controller** in het storyboard en zorg ervoor dat het **homeViewController** heeft als de **Aangepaste klasse** en **Storyboard-ID** onder de inspecteur identiteit.

7. Doe hetzelfde voor Image View Controller als **imageViewController**.

8. Maak vervolgens een nieuwe weergave Controller-klasse met de naam **imageViewController** voor het verwerken van de gebruikersinterface die u zojuist hebt gemaakt.

9. In **imageViewController.h**, voeg de volgende verklaringen van de controller van de interface. Controleer of met control ingedrukt houden en afbeeldingen van het storyboard naar deze eigenschappen aan elkaar koppelen:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. In **imageViewController.m**, Voeg het volgende toe aan het einde van de **viewDidload**:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. Importeren in **AppDelegate.m**, de afbeelding controller die u hebt gemaakt:

        #import "imageViewController.h"

12. Voeg een sectie van de interface met de volgende verklaring:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. In **AppDelegate**, zorg ervoor dat uw app registers voor stille meldingen in **toepassing: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Subsitute in de volgende uitvoering voor **toepassing: didRegisterForRemoteNotificationsWithDeviceToken** te nemen van het storyboard UI wordt gewijzigd in aanmerking:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Vervolgens voegt u de volgende methoden te **AppDelegate.m** de afbeelding ophalen uit uw eindpunt en een lokaal bericht verzenden wanneer het ophalen is voltooid. Vervang de tijdelijke aanduiding door `{backend endpoint}` met uw back-end-eindpunt:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Het lokale bericht hierboven verwerken door de openstelling van de controller afbeelding weergeven in **AppDelegate.m** met de volgende methoden:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

1. In XCode, de app op een fysiek i/o-apparaat (push notifications niet in de simulator werkt) worden uitgevoerd.

2. In de iOS-app UI, een gebruikersnaam en wachtwoord van dezelfde waarde voor verificatie en klik op **Log In**.

3. Klik op **verzenden push** en wordt er een waarschuwing-app. Als u op **meer**klikt, wordt u in de afbeelding die u wilt opnemen in de backend app gebracht.

4. U kunt ook op **push verzenden** en druk onmiddellijk op de home-knop van het apparaat. In enkele minuten ontvangt u een push-bericht. Als u erop tikt of klikt u op meer, wordt u in uw app en de rijke afbeeldingsinhoud gebracht.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
