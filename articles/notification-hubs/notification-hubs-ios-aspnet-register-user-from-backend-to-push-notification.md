<properties
    pageTitle="De huidige gebruiker voor push-meldingen registreren via Web API | Microsoft Azure"
    description="Informatie over het push melding registratie in een iOS-app met Azure melding Hubs vragen wanneer registeration wordt uitgevoerd door ASP.NET Web API."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>De huidige gebruiker voor push-meldingen registreren met behulp van ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u push melding registratie met Azure melding Hubs aanvragen bij de registratie wordt uitgevoerd door ASP.NET Web API. In dit onderwerp wordt uitgebreid de zelfstudie [gebruikers waarschuwen kennisgeving Hubs]. U moet al hebt voltooid de vereiste stappen in deze zelfstudie voor het maken van de geverifieerde mobiele service. Zie voor meer informatie over het scenario waarschuwen gebruikers, [gebruikers waarschuwen kennisgeving Hubs].

##<a name="update-your-app"></a>Uw app bijwerken  

1. Uw MainStoryboard_iPhone.storyboard, voeg de volgende onderdelen in de objectbibliotheek van:

    + **Label**: "Push-gebruiker met Hubs melding"
    + **Label**: "Omwille van"
    + **Label**: "Gebruiker"
    + **Text-veld**: "Gebruiker"
    + **Label**: "Wachtwoord"
    + **Text-veld**: "Wachtwoord"
    + **Knop**: 'Aanmelding'

    Op dit moment een storyboard ziet er als volgt uit:

    ![][0]

2. In de Office-assistent-editor afzetmogelijkheden voor de geschakelde besturingselementen maken en noem ze verbinding maken met de tekstvelden met de View-Controller (gemachtigde) en maakt een **actie** voor de knop **aanmelden** .

    ![][1]

    Het bestand BreakingNewsViewController.h moet nu de volgende code bevat:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Een klasse met de naam **DeviceInfo**maken en kopieer de volgende code in de sectie van de interface van het bestand DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Kopieer de volgende code in de sectie toepassing van het bestand DeviceInfo.m:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. PushToUserAppDelegate.h, voeg de volgende eigenschap singleton:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. Voeg de volgende code in de methode **didFinishLaunchingWithOptions** in PushToUserAppDelegate.m:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    De eerste regel initialiseert de singleton **DeviceInfo** . De tweede regel begint de registratie voor push-meldingen, die al aanwezig is dat de zelfstudie [Aan de slag met Hubs kennisgeving] reeds hebt voltooid.

9. Implementatie van de methode **didRegisterForRemoteNotificationsWithDeviceToken** in de AppDelegate in PushToUserAppDelegate.m, en voeg de volgende code toe:

        self.deviceInfo.deviceToken = deviceToken;

    Hiermee stelt u het apparaat token voor de aanvraag.

    > [AZURE.NOTE] Op dit punt moet niet er een andere code in deze methode. Als u al een aanroep van de methode **registerNativeWithDeviceToken** , die is toegevoegd als u de zelfstudie [Aan de slag met Hubs melding](/manage/services/notification-hubs/get-started-notification-hubs-ios/) hebt voltooid, moeten comment out of verwijderen die aanroep.

10. Toevoegen in het bestand PushToUserAppDelegate.m de volgende handler-methode:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Met deze methode wordt een waarschuwing weergegeven in de gebruikersinterface wanneer uw app meldingen ontvangt terwijl deze wordt uitgevoerd.

9. Open het bestand PushToUserViewController.m en het toetsenbord terug in de volgende uitvoering:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. In de methode **viewDidLoad** in het bestand PushToUserViewController.m, het label van de installatie-id als volgt te initialiseren:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. De volgende eigenschappen in in de PushToUserViewController.m-interface toevoegen:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Voeg vervolgens de volgende uitvoering:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Kopieer de volgende code in de **aanmelding** handler-methode door XCode gemaakt:

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Deze methode haalt een installatie-ID en een kanaal voor push-meldingen en verzendt, wordt ook het apparaattype op de geverifieerde Web API-methode die wordt gemaakt van een registratie in kennisgeving Hubs. Deze Web-API is gedefinieerd in de [gebruikers waarschuwen kennisgeving Hubs].

De client app is bijgewerkt, terugkeren naar de [gebruikers waarschuwen kennisgeving Hubs] en de mobiele service om meldingen te verzenden via Hubs kennisgeving bijwerken.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Waarschuw gebruikers met melding Hubs]: /manage/services/notification-hubs/notify-users-aspnet

[Aan de slag met melding Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
