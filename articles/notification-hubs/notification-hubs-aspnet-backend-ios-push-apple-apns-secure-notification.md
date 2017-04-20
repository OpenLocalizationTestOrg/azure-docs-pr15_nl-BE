<properties
    pageTitle="Push-melding Azure Hubs veilig"
    description="Informatie over het veilig push-meldingen verzenden naar een iOS-app van Azure. Voorbeelden van code in de doelstelling-C en C# is geschreven."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Push-melding Azure Hubs veilig

> [AZURE.SELECTOR]
- [Universele Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Overzicht

Ondersteuning voor push-berichten in Microsoft Azure hebt u toegang tot een gemakkelijk te gebruiken, meerdere platforms, schaalvergroting push-infrastructuur aanzienlijk de implementatie van push-meldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms vereenvoudigt.

Vanwege wettelijke beveiligingsbeperkingen, soms een toepassing wil wellicht iets opnemen in het bericht dat niet kan worden verzonden via de standaard push notification-infrastructuur. In deze zelfstudie wordt beschreven hoe u de dezelfde ervaring door het verzenden van gevoelige informatie via een beveiligd, geverifieerd verbinding tussen het apparaat en de back-end app te bereiken.

Op een hoog niveau is de stroom als volgt uit:

1. App back-end:
    - Beveiligde payload winkels in back-enddatabase.
    - De ID van deze melding stuurt naar het apparaat (geen beveiligde gegevens verzonden).
2. De app op het apparaat bij de ontvangst van de kennisgeving:
    - Het apparaat contact op met de back-end de nettolading beveiligde aanvragen.
    - De app kan de nettolading weergeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), nemen we aan dat het apparaat een verificatietoken in lokale opslag opgeslagen nadat de gebruiker zich aanmeldt. Dit garandeert een volledig naadloze oplossing, zoals het apparaat kunt ophalen van de kennisgeving secure payload deze token gebruikt. Als uw toepassing niet-verificatietokens worden opgeslagen op het apparaat, of als deze tokens verlopen de app apparaat na ontvangst van de kennisgeving een algemene kennisgeving de gebruiker hierom te vragen voor het starten van de app moet worden weergegeven. De app vervolgens verifieert de gebruiker en de nettolading van de melding wordt weergegeven.

Deze Secure Push zelfstudie laat zien hoe veilig een push-bericht verzenden. De zelfstudie bouwt voort op de zelfstudie [Gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , dus u moet eerst de stappen in deze zelfstudie uitvoeren.

> [AZURE.NOTE] In deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw melding hub configureren zoals beschreven in het [Aan de slag met melding Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Het iOS-project wijzigen

Nu dat u uw app back-end om alleen de *id* van een melding verzenden hebt gewijzigd, moet u uw iOS-app die melding afhandelen en terugbellen voor uw back-end voor het ophalen van de veilige bericht wordt weergegeven als u wilt wijzigen.

Om dit doel te bereiken, hebben we de logica voor het ophalen van de beveiligde inhoud uit de app back-end schrijven.

1. **AppDelegate.m**, Controleer of de registers app voor stille meldingen zodat de bericht-id verwerkt verzonden vanuit de back-end. Toevoegen de optie **UIRemoteNotificationTypeNewsstandContentAvailability** in didFinishLaunchingWithOptions:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. Toevoegen in uw **AppDelegate.m** een implementatie sectie boven aan de volgende declaratie:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Klik in de sectie toepassing de volgende code toevoegen, vervangen door de tijdelijke aanduiding `{back-end endpoint}` met het eindpunt voor de back-end eerder verkregen:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Nu hebben we de binnenkomende melding afhandelen en gebruik de bovenstaande methode voor het ophalen van de inhoud weer te geven. We hebben eerst uw iOS-app op de achtergrond wordt uitgevoerd wanneer een push-bericht ontvangen inschakelen. Selecteer uw app-project in het linkerpaneel in **XCode**, vervolgens klikt u op het doel van de belangrijkste app in de sectie van de **doelen** van het centrale venster.

5. Vervolgens klikt u op het tabblad **mogelijkheden** boven aan het deelvenster centraal en schakel het selectievakje **Externe meldingen** .

    ![][IOS1]


6. Voeg **AppDelegate.m** toe aan de volgende methode voor het verwerken van push-meldingen:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Houd er rekening mee dat het verdient de voorkeur voor het verwerken van de gevallen van ontbrekende authentication header, eigenschap of afwijzing door de back-end. De specifieke behandeling van deze gevallen is afhankelijk van vooral de gebruikerservaring van uw doel. Eén optie is om een melding met een algemene aanwijzing voor de gebruiker te verifiëren voor het ophalen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

Als de toepassing wordt uitgevoerd, het volgende doen:

1. In XCode, de app op een fysiek i/o-apparaat (push notifications niet in de simulator werkt) worden uitgevoerd.

2. Geef een gebruikersnaam en wachtwoord in de iOS-app UI. Dit kunnen elke tekenreeks zijn, maar zij moeten dezelfde waarde.

3. In de iOS-app UI, klikt u op **aanmelden**. Klik vervolgens op **verzenden push**. Hier ziet u de beveiligde aanmelding wordt weergegeven in het midden van uw melding.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
