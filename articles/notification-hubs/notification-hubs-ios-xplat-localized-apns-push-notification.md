<properties
    pageTitle="Melding Hubs gelokaliseerd breken nieuws zelfstudie voor iOS"
    description="Informatie over het Azure Service Bus melding Hubs gebruiken voor het verzenden van meldingen van gelokaliseerde recente nieuws (iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Melding Hubs gelokaliseerde laatste nieuws verzenden naar iOS-apparaten gebruiken

> [AZURE.SELECTOR]
- [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u de [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) van Azure melding Hubs voor het uitzenden van recente nieuws meldingen die zijn aangepast door de taal- en gebruiken. In deze zelfstudie start u met de iOS-app in [Gebruik melding Hubs voor het laatste nieuws verzenden]gemaakt. Als alles klaar is, dat is het mogelijk om te registreren voor categorieën waarin u geïnteresseerd bent, geef de taal waarin u de meldingen wilt ontvangen en alleen push-meldingen voor de geselecteerde categorieën in die taal ontvangen.


Er zijn twee delen voor dit scenario:

- iOS-app kan de client-apparaten een taal opgeven en zich abonneren op verschillende recente nieuwscategorieën;

- de back-end zendt de kennisgevingen, met behulp van de **sjabloon** en **tag** funcites van Azure melding Hubs.



##<a name="prerequisites"></a>Vereisten

U moet al hebt voltooid de zelfstudie [Gebruiken kennisgeving Hubs voor het verzenden van de laatste nieuws] en de code beschikbaar, omdat deze zelfstudie is gebaseerd op die code rechtstreeks rijen.

Visual Studio 2012 of later is optioneel.



##<a name="template-concepts"></a>Sjabloon concepten

In [Gebruik melding Hubs voor het laatste nieuws verzenden] kunt u een app die **codes** gebruikt om u te abonneren op meldingen voor verschillende nieuwscategorieën gebouwd.
Veel apps, maar meerdere markten richten en lokalisatie nodig hebben. Dit betekent dat de inhoud van de kennisgevingen zelf moet worden vertaald en bezorgd in de juiste set apparaten.
We zien hoe de **sjabloon** van kennisgeving Hubs leveren gemakkelijk breken gelokaliseerde nieuws meldingen gebruiken in dit onderwerp.

Opmerking: een manier om gelokaliseerde meldingen te verzenden is voor het maken van meerdere versies van elke code. Bijvoorbeeld voor Engels, Frans en Mandarijns zou moeten we drie verschillende labels voor wereldnieuws: 'world_en', 'world_fr', en 'world_ch'. We hebben in dat een gelokaliseerde versie van het wereldnieuws verzenden naar elk van deze tags. In dit onderwerp gebruiken we sjablonen om te voorkomen dat de verspreiding van codes en de eis om meerdere berichten te verzenden.

Sjablonen zijn op een hoog niveau kunt u opgeven hoe een bepaald apparaat een melding ontvangt. De sjabloon geeft de indeling precies nettolading door te verwijzen naar eigenschappen die deel van het bericht is verzonden door uw app back-end uitmaken. In ons geval sturen we een landinstelling agnostic bericht met alle ondersteunde talen:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Vervolgens zullen we zorgen ervoor dat apparaten met een sjabloon die naar de juiste eigenschap verwijst registreren. Een iOS-app die voor Franse nieuws wil registreren registreren, bijvoorbeeld het volgende:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Sjablonen zijn een zeer krachtige functie die voor meer informatie over in het artikel van onze [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) .

##<a name="the-app-user-interface"></a>De gebruikersinterface van app

We zullen nu de app nieuws te analyseren die u hebt gemaakt in het onderwerp [Gebruik melding Hubs voor het laatste nieuws verzenden] verzenden gelokaliseerde laatste nieuws met behulp van sjablonen wijzigen.


In uw MainStoryboard_iPhone.storyboard, Voeg een gesegmenteerde besturingselement met de drie talen die door ons worden ondersteund: Engels, Frans en Mandarijns.

![][13]

Controleer of een IBOutlet in uw ViewController.h toevoegen, zoals hieronder wordt weergegeven:

![][14]

##<a name="building-the-ios-app"></a>Bouwen van de iOS-app


1. Voegt u de methode *retrieveLocale* in de Notification.h en de winkel wijzigen en abonneren methoden zoals hieronder wordt weergegeven:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Wijzigen in de Notification.m, de methode *storeCategoriesAndSubscribe* door de locale parameter toe te voegen en het opslaan in de standaardinstellingen voor gebruiker:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    De methode *abonneren* om de landinstelling wijzigt:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Houd er rekening mee hoe we zijn nu met behulp van de methode *registerTemplateWithDeviceToken*in plaats van *registerNativeWithDeviceToken*. Wanneer we voor een sjabloon registreren hebben we de json-sjabloon en ook een naam voor de sjabloon opgeven (zoals onze app kunt verschillende sjablonen registreren). Zorg ervoor dat uw rubrieken registreren als labels, als we willen om te controleren of de notifciations voor deze nieuws ontvangen.

    Een methode voor het ophalen van de landinstelling van de standaardinstellingen van de gebruiker toevoegen:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Nu dat we onze klasse meldingen hebt gewijzigd, hebben we om ervoor te zorgen dat onze ViewController wordt gebruik gemaakt van de nieuwe UISegmentControl. Voeg de volgende regel in de methode *viewDidLoad* om er zeker van te geven van de landinstelling die u hebt geselecteerd:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Wijzigen uw oproep aan de *storeCategoriesAndSubscribe* met de volgende in de methode *abonneren* :

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Ten slotte hebt voor het bijwerken van de methode *didRegisterForRemoteNotificationsWithDeviceToken* in de AppDelegate.m, zodat u uw inschrijving goed vernieuwen kunt wanneer de toepassing wordt gestart. De aanroep voor de methode *abonneren* van kennisgevingen aan het volgende wijzigen:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen verzenden vanuit .NET console app.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen van het apparaat verzenden

Als u geen toegang tot Visual Studio hebt of wilt testen de gelokaliseerde Sjabloonmeldingen verzenden direct vanuit de app op het apparaat.  U kunt eenvoudig toevoegen de gelokaliseerde Sjabloonparameters aan de `SendNotificationRESTAPI` methode die u hebt gedefinieerd in de vorige zelfstudie.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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




## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van sjablonen:

- [Waarschuw gebruikers met Hubs melding: ASP.NET]
- [Waarschuw gebruikers met Hubs melding: Mobile-Services]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Waarschuw gebruikers met Hubs melding: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Waarschuw gebruikers met Hubs melding: Mobile-Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
