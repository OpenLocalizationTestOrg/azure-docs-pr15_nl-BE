<properties 
    pageTitle="Brug iOS webweergave met eigen betrokkenheid bij de mobiele iOS SDK" 
    description="Hierin wordt beschreven hoe u een brug maken tussen Javascript en de eigen betrokkenheid bij de mobiele iOS SDK waarop Webweergave"      
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Brug iOS webweergave met eigen betrokkenheid bij de mobiele iOS SDK

> [AZURE.SELECTOR]
- [Android-brug](mobile-engagement-bridge-webview-native-android.md)
- [iOS-brug](mobile-engagement-bridge-webview-native-ios.md)

Sommige mobiele toepassingen zijn ontworpen als een hybride app waar de app zelf is ontwikkeld met behulp van native iOS doelstelling C ontwikkeling maar enkele of zelfs alle schermen worden weergegeven binnen een iOS webweergave. U kunt nog steeds Mobile Engagement iOS SDK in dergelijke apps verbruiken en in deze zelfstudie wordt beschreven hoe om te gaan hierover. 

Er zijn twee methoden om dit bereiken, maar beide niet gedocumenteerd zijn:

- Voor het eerst een wordt beschreven op deze [koppeling](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) waarbij de registratie van een `UIWebViewDelegate` op het web weergeven en de vangst-en-onmiddellijk-annuleren een locatie wijzigen in Javascript wordt gedaan. 
- Ten tweede is een op basis van deze [sessie van WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/615), een benadering die beter dan de eerste is en dat we voor deze handleiding volgen. Houd er rekening mee dat deze aanpak alleen op iOS7 en hoger werkt. 

Volg de onderstaande stappen voor de iOS bridge-voorbeeld:

1. Ten eerste moet u ervoor zorgen dat u onze [zelfstudie aan de slag](mobile-engagement-ios-get-started.md) voor de integratie van de betrokkenheid bij de mobiele iOS SDK in uw hybride app hebt doorlopen. U kunt eventueel ook inschakelen test als volgt registreren zodat u de SDK-methoden zien kunt als we ze de webweergave starten. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Nu controleren of uw hybride app heeft een scherm met een webweergave erop. U kunt toe te voegen aan de `Main.storyboard` van de app. 

3. Deze webweergave koppelen aan uw **ViewController** door te slepen de webweergave van de scène View Controller voor de `ViewController.h` bewerken scherm plaatsen net onder de `@interface` lijn. 

4. Als u dit doet, wordt een dialoogvenster weergegeven waarin wordt gevraagd om een naam. Geef de naam als de **webweergave**. Uw `ViewController.h` bestand ziet er als volgt:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. We zullen werken de `ViewController.m` bestand later maar eerst maken we de brug-bestand dat u Hiermee maakt u een wrapper voor sommige algemeen gebruikte mobiele Engagement iOS SDK methoden. Maak een nieuwe kop bestand met de naam **EngagementJsExports.h** gebruikt de `JSExport` mechanisme dat wordt beschreven in de bovengenoemde [sessie](https://developer.apple.com/videos/play/wwdc2013/615) stelt de native iOS-methoden. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Vervolgens maken we het tweede deel van het bestand van bridge. Maak een bestand met de naam **EngagementJsExports.m** die de uitvoering de werkelijke wrappers maken door het aanroepen van de methoden van betrokkenheid bij de mobiele iOS SDK bevat. Ook Opmerking: we zijn bij het parseren van het `extras` van de webweergave javascript worden doorgegeven en plaatsen die in een `NSMutableDictionary` object moeten worden doorgegeven met de methodeaanroepen van betrokkenheid bij de SDK.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Nu we terugkomen op de **ViewController.m** en werken met de volgende code: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Houd rekening met de volgende punten met betrekking tot het bestand **ViewController.m** :

    - In de `loadWebView` methode, zijn we een lokaal HTML-bestand met de naam **LocalPage.html** waarvan de code zullen wij nagaan of volgende laden. 
    - In de `webViewDidFinishLoad` -methode die we toepassen de `JsContext` en onze wrapperklasse koppelt. Hierdoor kan roepen onze wrapper SDK met de greep van **EngagementJs** van de webweergave. 

7. Maak een bestand met de naam **LocalPage.html** met de volgende code:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Houd rekening met de volgende punten met betrekking tot de bovenstaande HTML-bestand:

    -   Het bevat een aantal invoervakken waar u gegevens moet worden gebruikt als namen voor de gebeurtenis, taak, fout, AppInfo kan bieden. Wanneer u op de knop ernaast klikt, een aanroep naar de Javascript die uiteindelijk de methoden aanroept vanuit het bestand bridge deze aanroep van de betrokkenheid bij de mobiele iOS SDK doorgeven. 
    -   We zijn labels op enkele statische extra info voor gebeurtenissen, projecten en zelfs fouten om te demonstreren hoe dit kan worden gedaan. Deze extra info als het een JSON-tekenreeks die wordt verzonden als u kijkt de `EngagementJsExports.m` -bestand, wordt geparseerd en doorgegeven bij het verzenden van gebeurtenissen, taken, fouten. 
    -   Een mobiele betrokkenheid bij de taak wordt gestart met de naam die u opgeeft in het invoervak uitgevoerd gedurende 10 seconden en sluit af. 
    -   Een Mobile Engagement appinfo of tag wordt met "customer_name' als de statische sleutel en de waarde die u hebt ingevoerd in de invoer als waarde voor het label doorgegeven. 
 
9. Start de app en ziet u het volgende. Nu een naam voor een test-gebeurtenis als volgt uit en klik op **verzenden** ernaast. 

    ![][1]

10. Als u naar het tabblad **Monitor** van uw app en kijk onder **evenementen -> Details**gaat, ziet u nu deze gebeurtenis worden weergegeven samen met de statische app-info die we sturen. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
