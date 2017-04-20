<properties
    pageTitle="Push-meldingen verzenden naar iOS met Azure melding Hubs | Microsoft Azure"
    description="In deze zelfstudie leert u hoe Azure melding Hubs met push-meldingen te verzenden naar een iOS-toepassing."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="push-meldingen, push-meldingen ios push-meldingen"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Push-meldingen verzenden naar iOS met Azure melding Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

In deze zelfstudie wordt beschreven hoe u met push-meldingen te verzenden naar een toepassing iOS Azure melding Hubs. U maakt een lege iOS-app die push-meldingen ontvangt met behulp van de [Apple Push Notification service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Wanneer u klaar bent, zult u uw melding hub gebruiken voor het uitzenden van push-meldingen voor alle apparaten die met uw app.

## <a name="before-you-begin"></a>Voordat u begint

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De voltooide code voor deze zelfstudie kunt u vinden [op GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Vereisten

Deze zelfstudie is het volgende vereist:

+ [IOS mobiele Services SDK versie 1.2.4]
+ Meest recente versie van [Xcode]
+ Een iOS 8 (of hoger)-compatibel apparaat
+ Lidmaatschap van de [Apple Developer-programma](https://developer.apple.com/programs/) .

   > [AZURE.NOTE] Vanwege de configuratievereisten voor push-meldingen, moet u implementeren en testen van push-meldingen op een fysiek i/o-apparaat (iPhone of iPad) in plaats van de iOS Simulator.

Voltooien van deze zelfstudie is een vereiste voor alle andere Hubs melding zelfstudies voor iOS-apps.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Uw melding Hub voor voor iOS push-meldingen configureren

In dit gedeelte leidt u door het maken van een nieuwe melding hub en verificatie configureren met het **.p12** push-certificaat dat u hebt gemaakt met APNS. Als u gebruiken een kennisgeving hub die u al hebt gemaakt wilt, kunt u verder met stap 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Klik op de knop <b>Notification Services</b> in de blade <b>Instellingen</b> en selecteer vervolgens <b>Apple (APNS)</b>. Klik op <b>Certificaat uploaden</b> en selecteer de <b>.p12</b> -bestand dat u eerder hebt geëxporteerd. Zorg ervoor dat u ook het juiste wachtwoord opgeven.</p>
<p>Zorg dat de <b>sandbox-</b> modus te selecteren omdat dit voor de ontwikkeling is. De <b>productie</b> alleen gebruiken als u push-meldingen verzenden naar gebruikers die uw app in de winkel hebt gekocht.</p>
</li>
</ol>
&emsp;&emsp;![APNS in Azure Portal configureren](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![APNS-certificering in Azure Portal configureren](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



De hub van uw melding is nu geconfigureerd voor het werken met APNS, en u hebt de verbindingsreeksen voor het registreren van uw app en push-meldingen te verzenden.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Verbinding maken met uw iOS-app melding Hubs

1. In Xcode, maak een nieuw iOS-project en selecteer de sjabloon **Verzamelaanvraag weergeven** .

    ![Xcode - toepassing één weergeven][8]

2. Als u de opties voor het nieuwe project, moet u de **Naam van het Product** en de **Organisatie-id** die u gebruikte toen u eerder de bundel-ID op de Apple Developer-portal instellen.

    ![Xcode - Projectopties][11]

3. Onder **doelen**, klikt u op de naam van het project klikt u op het tabblad **Instellingen voor het maken** en **Code-ondertekening identiteit**uitbreiden en onder de **sporen**, stelt u uw identiteit code-ondertekening. **Niveaus** van **standaardvolumes** naar **alle**in-of uitschakelen en **Provisioning profiel** ingesteld op het provisioning profiel dat u eerder hebt gemaakt.

    Vernieuw de profielen voor uw identiteit ondertekenen als het provisioning profiel dat u hebt gemaakt in Xcode niet wordt weergegeven. **Xcode** Klik op de menubalk, **Voorkeuren**, klik op het tabblad **Account** klikt u op de knop **Details weergeven** , klik op de identiteit van het ondertekenen en klik vervolgens op de knop Vernieuwen in de rechterbenedenhoek.

    ![Xcode - provisioning profiel][9]

4. Download de [iOS mobiele Services SDK versie 1.2.4] en pak het bestand uit. Klik met de rechtermuisknop op het project in Xcode, en klik op de optie **Bestanden toevoegen aan** de map **WindowsAzureMessaging.framework** toevoegen aan uw project Xcode. Selecteer **items desgewenst kopiëren**en klik vervolgens op **toevoegen**.

    >[AZURE.NOTE] De kennisgeving hubs SDK ondersteunt momenteel geen bitcode op Xcode-7.  U moet **Bitcode inschakelen** voor uw project ingesteld op **Nee** in de **Opties maken** .

    ![Unzip Azure SDK][10]

5. Een nieuwe header-bestand toevoegen aan uw project met de naam `HubInfo.h`. Dit bestand bevat de constanten voor de hub van uw melding.  Voeg de volgende definities en de tijdelijke aanduidingen voor letterlijke tekenreeks vervangen door de *hubnaam* en de *DefaultListenSharedAccessSignature* die u eerder hebt genoteerd.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Open uw `AppDelegate.h` bestand toevoegen de volgende richtlijnen voor importeren:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. In de `AppDelegate.m file`, voeg de volgende code in de `didFinishLaunchingWithOptions` methode op basis van uw versie van iOS. Deze code registreert uw ingang met APNs:

    Voor iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    Voor iOS-versies ouder dan 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. In hetzelfde bestand, voegt u de volgende methoden. Deze code maakt verbinding met de melding hub met de verbindingsgegevens die u hebt opgegeven in HubInfo.h. Vervolgens wordt het token van het apparaat op de hub melding zodat de hub kennisgeving om meldingen te verzenden:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. In hetzelfde bestand, voegt u de volgende methode een **UIAlert** weergeven als de kennisgeving is ontvangen, terwijl de toepassing actief is:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Bouwen en uitvoeren van de app op het apparaat om te controleren of er geen fouten zijn.

## <a name="send-test-push-notifications"></a>Test push-meldingen te verzenden


U kunt testen meldingen ontvangen in uw app via push-meldingen in de [Portal Azure] via de sectie **Probleemoplossing** in de hub blade (Gebruik de optie *Test verzenden* ).

![Azure Portal - Test verzenden][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Optioneel) Push-meldingen verzenden vanuit de app.

>[AZURE.IMPORTANT] In het volgende voorbeeld uit het verzenden van meldingen van de client app is beschikbaar voor alleen leermateriaal. Aangezien dit houdt in dat de `DefaultFullSharedAccessSignature` worden op de client-app, uw hub-kennisgeving aan het risico dat een gebruiker kan toegang door onbevoegden om meldingen te verzenden naar uw clients worden getoond.

Als u verzenden, push-meldingen uit een app wilt, vindt hier u een voorbeeld van hoe u dit doet met de interface van de REST.

1. Open in Xcode, `Main.storyboard` en voeg de volgende UI-componenten in de objectbibliotheek kan de gebruiker voor het verzenden van push-meldingen in de app:

    - Een label met de labeltekst van geen. Deze worden gebruikt om fouten te melden bij het verzenden van meldingen. De eigenschap **Lines** moet worden ingesteld op **0** , zodat deze automatisch wordt grootte is beperkt tot de rechter- en linkermarge en de bovenkant van de weergave.
    - Een veld voor tekst met **tijdelijke** tekst ingesteld op **Bericht invoeren**. Het veld direct onder het label beperken zoals hieronder wordt weergegeven. De Controller weergave instellen als de gemachtigde van de uitgang.
    - Een **Bericht verzenden** met de titel knop beperkt net onder het tekstveld en in het horizontale middelpunt.

    De weergave moet er als volgt uitzien:

    ![Xcode designer][32]


2. [Uitgangen toevoegen](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) voor het veld label en het tekstvak uw weergave, aangesloten en bijwerken van uw `interface` definitie ter ondersteuning van `UITextFieldDelegate` en `NSXMLParserDelegate`. De eigenschap drie verklaringen voor de ondersteuning van de REST-API aanroept en bij het parseren van het antwoord hieronder toevoegen.

    Het bestand ViewController.h ziet er als volgt uit:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Open `HubInfo.h` en voeg de volgende constanten die worden gebruikt voor het verzenden van meldingen op uw hub. De tekenreeks van de tijdelijke aanduiding vervangen door de werkelijke *DefaultFullSharedAccessSignature* -verbindingsreeks.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Voeg de volgende `#import` instructies voor uw `ViewController.h` bestand.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. In `ViewController.m` de volgende code toevoegen aan de interface-implementatie. Deze code wordt de verbindingsreeks van de *DefaultFullSharedAccessSignature* te parseren. Zoals vermeld in de [REST API reference](http://msdn.microsoft.com/library/azure/dn495627.aspx), worden deze gegevens geparseerd voor het genereren van een SaS-token voor de **autorisatie** verzoek-header gebruikt.

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. In `ViewController.m`, werken de `viewDidLoad` methode voor het parseren van de verbindingstekenreeks wanneer de weergave wordt geladen. Ook de nuttige methoden, die hieronder worden weergegeven voor de implementatie van de interface toevoegen.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. In `ViewController.m`, de volgende code toevoegen aan de uitvoering van de interface voor het genereren van de SaS Autorisatietoken die u in de kop van de **vergunning** , als bedoeld in de [REST API Reference](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + slepen van de **Kennisgeving verzenden** knop naar `ViewController.m` een **SendNotificationMessage** met de naam voor de gebeurtenis **Touch omlaag** actie toevoegen. De methode update met de volgende code voor het verzenden van het bericht met de REST API.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. In `ViewController.m`, voegt u de volgende gemachtigdenmethode ter ondersteuning van het toetsenbord voor het tekstveld te sluiten. CTRL + slepen vanuit het veld naar het pictogram weergave-Controller in de controller weergave instellen als de gemachtigde uitgang interface designer.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. In `ViewController.m`, toevoegen van de volgende methoden gemachtigde ter ondersteuning bij het parseren van het antwoord met behulp van `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Compileer het project en controleer of er geen fouten zijn.


> [AZURE.NOTE] Als u een build-fout in Xcode7 over bitcode ondersteuning, moet u de **Instellingen voor het maken**van de > **Bitcode inschakelen (ENABLE_BITCODE)** op **Nee** in Xcode. De kennisgeving Hubs SDK biedt momenteel geen ondersteuning voor bitcode. 

In de Apple- [Local en Push-melding Programming Guide]vindt u alle mogelijke kennisgeving-nettoladingen.


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Als uw app push-meldingen kunt ontvangen controleren

Om te testen op iOS push-meldingen, moet u de app installeren op een fysiek i/o-apparaat. U kan geen Apple push-meldingen verzenden met behulp van de iOS Simulator.

1. De toepassing uitvoeren en controleren of de registratie is voltooid en drukt u vervolgens op **OK**.

    ![iOS-App Push-melding registratie Test][33]

2. Zoals hierboven beschreven, kunt u een test push-bericht verzenden vanuit de [Portal Azure]. Als u de code voor het verzenden van push-meldingen in de app toegevoegd, raakt u in het tekstveld in te voeren van een melding. Druk op de knop **verzenden** op het toetsenbord of de knop **Bericht verzenden** in de weergave voor het verzenden van het bericht.

    ![iOS-App Push-melding Test verzenden][34]

3. De push-bericht wordt verzonden naar alle apparaten die zijn geregistreerd om de meldingen ontvangen van de desbetreffende kennisgeving Hub.

    ![iOS-App Push-meldingen ontvangen Test][35]


##<a name="next-steps"></a>Volgende stappen

In het volgende eenvoudige voorbeeld u push-meldingen voor al uw geregistreerde iOS apparaten uitgezonden. Wij stellen voor een volgende stap in het leren, die u verder met de zelfstudie [Azure melding Hubs gebruikers waarschuwen voor iOS met .NET back-end] , die u helpt bij het maken van een back-end tags met push-meldingen worden verzonden. 

Als u wilt dat uw gebruikers door belangengroepen in segmenten, kunt u bovendien op verplaatsen naar de zelfstudie [Gebruiken kennisgeving Hubs voor het laatste nieuws te verzenden] . 

Zie voor algemene informatie over Hubs kennisgeving [Richtsnoeren voor kennisgeving Hubs].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[IOS mobiele Services SDK versie 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Richtsnoeren voor kennisgeving Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure melding Hubs gebruikers waarschuwen voor iOS met .NET back-end]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Lokale en Push-melding Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com