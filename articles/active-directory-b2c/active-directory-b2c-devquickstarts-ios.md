<properties
    pageTitle="Azure Active Directory B2C: Een web-API aanroept vanuit een derde partij bibliotheken met iOS-toepassing | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u voor het maken van een taak-overzicht-app voor iOS die een Node.js web API aanroept met behulp van OAuth 2.0 aan toonder tokens met behulp van een derde partij-bibliotheek"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< tags "active directory b2c" ms.service= ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "artikel hero"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Een web-API aanroept vanuit een iOS-toepassing met behulp van een derde partij-bibliotheek

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Het platform Microsoft identity maakt gebruik van open standaarden zoals OAuth2 en OpenID verbinden. Hierdoor kunnen ontwikkelaars gebruikmaken van een bibliotheek die zij wensen te integreren in onze services. Ontwikkelaars helpt bij het gebruik van ons platform met andere bibliotheken we enkele scenario's zoals dit te tonen hebt geschreven bibliotheken van derden verbinding maken met het Microsoft-platform identiteit configureren. De meeste bibliotheken die [de spec RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) implementeren kunnen verbinding maken met het platform van Microsoft Identity worden.


Als u OAuth2 of OpenID verbinding maken logisch veel van deze configuratie monster niet dat veel aan u. Het is raadzaam dat u kijkt naar een beknopt [overzicht van het protocol dat wij hier hebt gedocumenteerd](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Sommige functies van ons platform waarin een expressie in deze normen, zoals beheer van voorwaardelijke toegang en Intune, moeten u onze open-source Microsoft Azure identiteit bibliotheken gebruiken. 
   
Niet alle scenario's Azure Active Directory en functies worden ondersteund door de B2C-platform.  Lezen om te bepalen als u de B2C-platform te gebruiken, over [B2C beperkingen](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Een AD-B2C Azure map ophalen

Voordat u AD-B2C Azure gebruiken kunt, moet u een map maken of pachters. Een directory is een container voor alle gebruikers, toepassingen en groepen. Als je niet hebt al verder [een B2C-map maken](active-directory-b2c-get-started.md) voordat u.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u voor het maken van een app in de B2C-map. Dit geeft Azure AD-informatie die nodig is om veilig te communiceren met uw app. Zowel de app en het web API worden vertegenwoordigd door een enkele **Aanvraag-ID** in dit geval, omdat ze bestaan uit één logische app. Als u wilt een app maken, volg [de instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

- Een **mobiel apparaat** in de toepassing opnemen.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. Ook moet u dit later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Uw beleid maken

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Deze app bevat één identiteit ervaring: een gecombineerde sign in- en aanmelding. Moet u voor het maken van dit beleid voor elk type, zoals beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Als u het beleid maakt, moet u:

- Kies de **weergavenaam** en aanmelding kenmerken in het beleid.
- Kies de **weergavenaam** en de **Object-ID** application claims in elk beleid. U kunt ook andere vorderingen.
- De **naam** van elk beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  Naam van het beleid hebt u later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u het beleid hebt gemaakt, bent u klaar om uw app te bouwen.


## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Als u wilt volgen, kunt u [downloaden van de app als een .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)-/archive/master.zip) of het klonen:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Of download de volledige code en meteen aan de slag: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>De bibliotheek-nxoauth2 van derden downloaden en starten van een werkruimte

Voor dit scenario gebruiken we de OAuth2Client van GitHub, een bibliotheek met OAuth2 voor Mac OS X en iOS (cacao & cacao aanraken). Deze bibliotheek is gebaseerd op een ontwerp voor 10 van de OAuth2-specificatie. Het implementeert het profiel van de oorspronkelijke toepassing en het eindpunt van de vergunning eindgebruiker ondersteunt. Dit zijn alle dingen we om te integrat met Microsoft identity-platform moeten.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>De bibliotheek toe te voegen aan uw project met behulp van CocoaPods

CocoaPods is een afhankelijkheid manager voor Xcode projecten. De bovenstaande installatiestappen beheert automatisch.

```
$ vi Podfile
```
Voeg het volgende toe aan dit podfile:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Nu laden de podfile met cocoapods. Hiermee maakt u een nieuwe XCode-werkruimte worden geladen.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>De structuur van het project

We hebben de volgende structuur voor ons project in het skelet instellen:

* Een **Modelweergave** met een taakvenster
* Een **Taakweergave toevoegen** voor de gegevens over de geselecteerde taak.
* Een **Aanmelding weergeven** waarmee een gebruiker aanmelden bij de app.

We springt verschillende bestanden in het project toe te voegen verificatie. Andere delen van de code zoals de visuele code is geen identiteit van de belangrijkste en zijn beschikbaar voor u.

## <a name="create-the-settingsplist-file-for-your-application"></a>Maak de `settings.plist` -bestand voor uw toepassing

Het is gemakkelijker voor het configureren van de toepassing als we een centrale locatie hebben voor onze waarden van de systeemconfiguratie. Bovendien leert u hoe elke instelling werkt in uw toepassing. We maken gebruik van de *Lijst met eigenschappen* als een manier om deze waarden aan de toepassing.

* Maken openen de `settings.plist` in het bestand `Supporting Files` in de werkruimte van de toepassing

* Voer de volgende waarden (kijken we naar hen in detail spoedig)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

We gaan deze in detail.


Voor `authURL`, `loginURL`, `bhh`, `tokenURL` u zult merken dat u moet invullen in de naam van de huurder. Dit is de naam van de huurder van uw B2C huurder die aan u is toegewezen. Bijvoorbeeld, `kidventusb2c.onmicrosoft.com`. Als u onze open-source Microsoft Azure identiteit bibliotheken gebruiken zou we halen deze gegevens voor u met onze eindpunt metagegevens. We hebben het harde werk van deze waarden extraheren voor u gedaan.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

De `keychain` waarde is de container waarin de NXOAuth2Client-bibliotheek gebruiken wilt voor het maken van een sleutelketen voor het opslaan van uw tokens. Als u eenmalige aanmelding via diverse apps krijgt kunt u de sleutelketen dezelfde opgeven in elke toepassing, alsmede het gebruik van die sleutelketen aanvragen in de XCode-entitements. Hierover vindt u in de documentatie van Apple.

De `<policy name>` aan het einde van elke URL zijn de plaatsen waar u wilt plaatsen, het beleid dat u hierboven hebt gemaakt. De toepassing roept dit beleid is afhankelijk van de stroom.

De `taskAPI` wordt de REST eindpunt noemen we met de B2C-token aan taken of bestaande query-taken toevoegen. Deze is speciaal voor dit voorbeeld is ingesteld. U hoeft niet te wijzigen om het monster te werken.

De rest van deze waarden zijn vereist voor de bibliotheek gebruiken en plaatsen waar u de waarden voor de context te maken.

Nu de `settings.plist` -bestand gemaakt, moeten we de code om deze te lezen.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Een klasse AppData instellen voor het lezen van onze instellingen.

We maken een enkelvoudig bestand dat alleen wordt geparseerd onze `settngs.plist` we dat hierboven is gemaakt en die instellingen avaialble in de toekomst te maken voor alle klassen. Omdat we niet willen voor het maken van een nieuwe kopie van de gegevens telkens wanneer een klasse wordt gevraagd deze, we gebruiken een Singleton patroon en dezelfde instantie gemaakt telkens wanneer die een aanvraag wordt gedaan voor de instellingen alleen terug

* Maak een `AppData.h` bestand:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Maak een `AppData.m` bestand:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Nu gemakkelijk we onze gegevens krijgen kunnen door te roepen `  AppData *data = [AppData getInstance];` in een van onze klassen als u ziet hieronder.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>De NXOAuth2Client-bibliotheek in uw AppDelegate instellen

De bibliotheek NXOAuthClient is vereist voor bepaalde waarden ophalen instellen. Eenmaal is voltooid kunt u de token aquired de REST API aan te roepen. Omdat we dat weten de `AppDelegate` wordt aangeroepen telkens wanneer we de toepassing laden is het verstandig dat we onze waarden in dat bestand plaatsen.
* Open `AppDelegate.m` bestand

* Sommige we later gebruiken header-bestanden importeren.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Voeg toe de `setupOAuth2AccountStore` -methode in de AppDelegate

We moeten een AccountStore maken en voer deze de gegevens die we net gelezen uit de `settings.plist` bestand.

Er zijn enkele dingen die u moet rekening houden met met betrekking tot de B2C-service op dit moment wordt deze code begrijpelijker maken:


1. Azure AD B2C gebruikt het *beleid* worden geleverd door de queryparameters aan uw aanvraag. Hierdoor Azure Active Directory om te fungeren als een onafhankelijke dienst voor uw toepassing. Om deze extra queryparameters moeten we bieden de `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` methode met onze parameters aangepast beleid. 

2. Azure AD B2C maakt gebruik van scopes op ongeveer dezelfde manier als andere OAuth2 servers. Maar omdat het gebruik van B2C zo veel mogelijk is over de verificatie van een gebruiker als de toegang tot bronnen sommige bereiken absoluut nodig zijn om de stroom correct te laten werken. Dit is de `openid` bereik. De identiteit van onze Microsoft SDK's automatisch bieden de `openid` voor u zodat u die niet in onze SDK-configuratie zien. Aangezien we met behulp van de bibliotheek van een derde partij, echter moeten we deze scope opgeven.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Vervolgens Zorg ervoor dat u deze aanroepen in de AppDelegate onder `didFinishLaunchingWithOptions:` methode. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Maak een `LoginViewController` klasse die we gebruiken voor het verwerken van verificatieaanvragen

We gebruiken een webweergave voor aanmelding bij account. Deze manier kunnen wij de gebruiker vragen om aanvullende factoren zoals SMS-bericht (indien geconfigureerd) of foutberichten weer te geven voor de gebruiker. We zullen hier instellen van de webweergave en vervolgens de code voor het verwerken van de retouraanroepen die in de webweergave van de Service Microsoft Identity gebeuren zal later schrijven.

* Maak een `LoginViewController.h` klasse

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

We gaan deze onderstaande methoden maken.

> [AZURE.NOTE] 
    Zorg ervoor dat u afhankelijk maken van de `loginView` aan de werkelijke webweergave in een storyboard. Anders heb je geen een webweergave die kan worden weergegeven wanneer het tijd is om te verifiëren.

* Maak een `LoginViewController.m` klasse

* Sommige variabelen te verrichten staat als wij verifiëren toevoegen

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Negeren van de webweergave methoden voor het verwerken van verificatie

We moeten zien de webweergave van het gedrag dat we willen dat wanneer een gebruiker moet aanmelden, zoals hierboven wordt beschreven. U kunt gewoon Knip en plak de onderstaande code.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Schrijven van code voor het verwerken van het resultaat van de aanvraag OAuth2

Moeten we de code waarmee de URL-omleiding die van de webweergave terugkomt wordt verwerkt. Als het niet lukt, wordt geprobeerd opnieuw. Ondertussen biedt de bibliotheek de fout zien in de console of asyncronously te verwerken. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* De fabrieken meldingen instellen.

We maken dezelfde methode die we hebben gedaan in de `AppDelegate` hierboven, maar deze keer zullen we enkele toevoegen `NSNotification`s ons laten weten wat er gebeurt in onze service. Stelt een waarnemer die wordt laat ons weten wanneer iets verandert met het token. Zodra we het token krijgen we de gebruiker terug naar de `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Code toevoegen die de gebruiker afhandelt wanneer een aanvraag voor aanmelden native is gestart

We maken een methode die wordt aangeroepen wanneer we een verzoek voor verificatie hebben. Dit is de methode die u een webweergave maakt

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Tenslotte al deze methoden die we boven elke keer hebt geschreven eens roept de `LoginViewController` wordt geladen. We dit doen door deze methoden voor het toevoegen van onze `viewDidLoad` methode Apple geeft ons

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

U bent nu klaar met het maken van de belangrijkste manier waarop we zullen door onze toepassing voor het aanmelden. Nadat we hebt aangemeld, moet u gebruik van onze tokens die wij hebben ontvangen. Dat maken we code helper REST API's voor ons met behulp van deze bibliotheek wordt aangeroepen.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Maak een `GraphAPICaller` klasse voor het verwerken van onze aanvragen voor REST-API

We hebben een configuratie geladen telkens als we onze app laden. Nu moeten we er iets mee doen als er een token. 

* Maak een `GraphAPICaller.h` bestand

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Ziet u in deze code dat we twee methoden zullen maken: één voor de taken van een API en een andere taken toevoegen aan de API.

Nu dat we onze interface hebt ingesteld, voegen we de daadwerkelijke uitvoering:

* Maak een`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>De steekproef toepassing uitvoeren

Ten slotte bouwen en uitvoeren van de app in Xcode. Aanmelden of aanmelden bij de app en taken maken voor een gebruiker ingelogd. Taken maken voor die gebruiker afmelden en opnieuw aanmelden als een andere gebruiker.

U ziet dat de taken opgeslagen per gebruiker op de API, omdat de API haalt de identiteit van de gebruiker uit het toegangstoken die zij ontvangt.


## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar onderwerpen met meer geavanceerde B2C. U kunt het volgende proberen:

[Een Node.js web API aanroepen vanuit een Node.js web app.]()

[De UX voor een B2C-app aanpassen]()
