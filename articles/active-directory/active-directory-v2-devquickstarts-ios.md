<properties
    pageTitle="Azure AD v2.0 iOS App | Microsoft Azure"
    description="Het bouwen van een iOS-app die werk of school accounts en gebruikers met zowel persoonlijke Microsoft-account aanmelden via bibliotheken van derden."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Aanmelden toevoegen aan een iOS-app met behulp van een externe bibliotheek met Graph API met behulp van het eindpunt v2.0

Het platform Microsoft identity maakt gebruik van open standaarden zoals OAuth2 en OpenID verbinden. Ontwikkelaars kunnen de bibliotheek willen integreren in onze services gebruiken. We enkele scenario's zoals dit om aan te tonen van het configureren van externe bibliotheken verbinding maken met het platform van Microsoft identity geschreven zodat ontwikkelaars gebruiken ons platform met andere bibliotheken. De meeste bibliotheken die [de spec RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) implementeren kunnen verbinding maken met het platform van Microsoft identity.

Met de toepassing die in deze procedure wordt gemaakt, kunnen gebruikers zich aanmelden bij hun organisatie en zoekt u naar anderen in hun organisatie met behulp van de API van de grafiek.

Als u nieuwe OAuth2 of OpenID, logisch veel van deze configuratie monster niet dat voor u. Wij raden aan voor achtergrond [v2.0 protocollen - OAuth 2.0 vergunning Code stroom](active-directory-v2-protocols-oauth-code.md) te lezen.


> [AZURE.NOTE]
    Sommige functies van ons platform waarin een expressie in de OAuth2 of OpenID verbinding normen, zoals voorwaardelijke toegang en beheer van Intune, moeten u onze open-source Microsoft Azure identiteit bibliotheken gebruiken.

Het eindpunt v2.0 biedt geen ondersteuning voor alle functies en Azure Active Directory-scenario's.

> [AZURE.NOTE]
    Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Code van GitHub downloaden
De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) of het skelet klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

U kunt ook gewoon het programmeervoorbeeld downloaden en meteen aan de slag:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Registreren van een app
Een nieuwe toepassing maken op de [portal Registratie van toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of de gedetailleerde stappen bij [het registreren van een app aan het eindpunt v2.0](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- De **Toepassings-Id** die toegewezen aan uw app omdat het snel hebt je kopiëren.
- Het **mobiele** platform voor uw app toevoegen.
- Kopieer de **URI omgeleid** via de portal. Moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>De NXOAuth2-bibliotheek van derden downloaden en een werkruimte maken

Voor dit scenario gebruikt u de OAuth2Client van GitHub, dat is een OAuth2 bibliotheek voor Mac OS X en iOS (cacao en bereidingen aanraken). Deze bibliotheek is gebaseerd op een ontwerp voor 10 van de OAuth2-specificatie. Deze het profiel van de oorspronkelijke toepassing implementeert en ondersteunt het eindpunt van de vergunning van de gebruiker. Dit zijn de dingen die u wilt integreren met het Microsoft-platform identity.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>De bibliotheek aan uw project toevoegen met behulp van CocoaPods

CocoaPods is een afhankelijkheid manager voor Xcode projecten. De vorige installatiestappen beheert automatisch.

```
$ vi Podfile
```
1. Voeg het volgende toe aan dit podfile:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. De podfile worden geladen met behulp van CocoaPods. Hiermee maakt u een nieuwe werkruimte Xcode die worden geladen.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Ontdek de structuur van het project

De volgende structuur is ingesteld voor het project in het skelet:

- Een modelweergave met een UPN-zoeken
- Een gedetailleerde weergave voor de gegevens over de geselecteerde gebruiker
- Een aanmelding weergeven wanneer een gebruiker bij de app aanmelden zich query van de grafiek

Wij verplaatst naar verschillende bestanden in het skelet verificatie toevoegen. Andere delen van de code, zoals de visuele code niet van toepassing zijn op de identiteit, maar zijn beschikbaar voor u.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Instellen van het bestand settings.plst in de bibliotheek

-   Open in de QuickStart-project, de `settings.plist` bestand. Vervang de waarden van de elementen in de sectie aangepast aan de waarden die u hebt gebruikt in Azure portal. De code verwijst naar deze waarden wanneer u de bibliotheek voor verificatie van Active Directory wordt gebruikt.
    -   De `clientId` is de client-ID van de toepassing die u hebt gekopieerd vanaf de portal.
    -   De `redirectUri` is de omleidings-URL die de portal.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>De NXOAuth2Client-bibliotheek in uw LoginViewController instellen

De bibliotheek NXOAuth2Client is vereist voor bepaalde waarden ophalen instellen. Nadat u die taak hebt voltooid, kunt u de verworven token de Graph API aan te roepen. Omdat `LoginView` wordt aangeroepen telkens moet verifiëren, is het zinvol om configuratiewaarden in dat bestand.

- We toevoegen sommige waarden aan de `LoginViewController.m` bestand in te stellen van de context voor verificatie en machtiging. Details over de waarden als volgt de code.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Bekijk details over de code.

De eerste tekenreeks is voor `scopes`.  De `User.Read` waarde kunt u het profiel van de gebruiker ingelogd basis lezen.

U kunt meer informatie over de beschikbare scopes op [Microsoft Graph machtiging bereiken](https://graph.microsoft.io/docs/authorization/permission_scopes).

Voor `authURL`, `loginURL`, `bhh`, en `tokenURL`, moet u de eerder opgegeven waarden. Als u de bron openen Microsoft Azure identiteit bibliotheken, halen wij deze gegevens voor u met behulp van het eindpunt van onze metagegevens. We hebben het harde werk van deze waarden extraheren voor u gedaan.

De `keychain` waarde is de container waarin de NXOAuth2Client-bibliotheek gebruiken wilt voor het maken van een sleutelketen voor het opslaan van uw tokens. Als u wilt om eenmalige aanmelding (SSO) over talloze apps, kunt u de sleutelketen dezelfde opgeven in elk van uw toepassingen en het gebruik van die sleutelketen aanvragen in de rechten van de Xcode. Dit wordt uitgelegd in de documentatie van Apple.

De rest van deze waarden zijn vereist voor de bibliotheek en maakt u voor het vervoer van waarden aan de context.

### <a name="create-a-url-cache"></a>Een URL-cache maken

In `(void)viewDidLoad()`, die altijd wordt aangeroepen nadat de weergave is geladen, wordt de volgende code een cache voor onze primes.

Voeg de volgende code toe:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Maak een webweergave voor aanmelden

Een webweergave kan de gebruiker gevraagd om aanvullende factoren zoals SMS-bericht (indien geconfigureerd) of foutberichten retourneren naar de gebruiker. Hier stelt u de webweergave omhoog en vervolgens de code voor het verwerken van de retouraanroepen die in de webweergave van de identiteit services gebeuren zal later schrijven.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Negeren van de webweergave methoden voor het verwerken van verificatie

Als u wilt de webweergave vertellen wat er gebeurt wanneer een gebruiker aanmelden moet, zoals eerder besproken, kunt u de volgende code plakken.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Schrijven van code voor het verwerken van het resultaat van de aanvraag OAuth2

De volgende code wordt de URL-omleiding die uit de webweergave resulteert verwerken. Als de verificatie niet slaagt, probeer de code opnieuw. Ondertussen biedt de bibliotheek de fout die u kunt zien in de console of asynchroon verwerkt.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Stel de OAuth-Context (het accountarchief genoemd)

Hier kunt u bellen `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` op de gedeelde account winkel voor elke service die u wilt toegang tot de toepassing. Het accounttype is een tekenreeks die wordt gebruikt als id voor een bepaalde service. Omdat u toegang de API van de grafiek tot, wordt de code verwijst naar deze als `"myGraphService"`. U stelt vervolgens van een waarnemer die aangeven wanneer iets verandert met het token. Na het ophalen van het token u terugkeert de gebruiker terug naar de `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>De modelweergave zoeken en weergeven van de gebruikers van de Graph API instellen

Een model-View-Controller (MVC) app die de geretourneerde gegevens worden weergegeven in het raster buiten het bestek van dit overzicht is en veel on line zelfstudie wordt uitgelegd hoe u te ontwikkelen. Alle deze code wordt in het bestand skelet. U hoeft echter een paar dingen in deze toepassing MVC behandelt:

* Wanneer een gebruiker iets in het zoekveld onderscheppen
* Een object van de gegevens terug naar de MasterView bieden, zodat de resultaten kan worden weergegeven in het raster

We doen die hieronder.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Als u bent aangemeld gecontroleerd toevoegen

De toepassing heeft weinig als de gebruiker niet is aangemeld, dus is het verstandig om te controleren of er al een token in de cache. Als dat niet het geval is, wordt u omgeleid naar de LoginView voor de gebruiker aan te melden. Als u, de beste manier om acties uitvoeren wanneer een weergave wordt geladen is met de `viewDidLoad()` methode die Apple ons biedt.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>De weergave van de tabel bijwerken wanneer de gegevens worden ontvangen

Wanneer de Graph API gegevens retourneert, moet u de gegevens wilt weergeven. Om redenen van eenvoud is hier de code in de tabel bijwerken. U kunt alleen de juiste waarden in uw code MVC standaardtekst plakken.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Kunt u de Graph API aanroepen wanneer iemand in het zoekveld typt

Wanneer een gebruiker een zoekopdracht in het vak, moet u de shove die via de API van de grafiek. De `GraphAPICaller` klasse, die u in de volgende code wordt opgebouwd, scheidt de lookup-functionaliteit van de presentatie. Nu gaan we de code schrijven die tekens zoeken op de Graph API-feeds. We doen dit door middel van een methode met de naam `lookupInGraph`, die nodig is de tekenreeks die u moet zoeken.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Schrijf een helperklasse voor toegang tot de Graph API

Dit is de kern van de toepassing. Overwegende dat de rest code in het MVC standaard van Apple invoegen is, schrijven hier u code voor het diagram tijdens het typen query en vervolgens die gegevens. Dit is de code en een gedetailleerde uitleg volgt.

### <a name="create-a-new-objective-c-header-file"></a>Een nieuwe doelstelling C header-bestand maken

Geef het bestand de naam `GraphAPICaller.h`, en de volgende code toe te voegen.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

U ziet hier dat een opgegeven methode een tekenreeks en geeft als een completionBlock resultaat. Deze completionBlock, zoals u al dacht waarschijnlijk, wordt de tabel bijwerken door middel van een object met de ingevulde gegevens in real-time als de gebruiker.


### <a name="create-a-new-objective-c-file"></a>Een nieuwe doelstelling C-bestand maken

Geef het bestand de naam `GraphAPICaller.m`, en voegt u de volgende methode.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

We gaan door middel van deze methode gedetailleerd.

De kern van deze code is in de `NXOAuth2Request`, methode, die uitgaat van de parameters die u al hebt gedefinieerd in het bestand settings.plist.

De eerste stap is het maken van de juiste Graph API-aanroep. Omdat u verbinding maakt met `/users`, u dat door deze toe te voegen aan de resource Graph API en de versie opgeven. Is het verstandig om deze in een bestand met instellingen voor externe omdat deze wijzigen kunnen, zoals de API ontwikkeld.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Vervolgens moet u parameters opgeven die u ook aan de Graph API-aanroep wordt verstrekt. Het is *zeer belangrijk* plaats de parameters niet in de bron eindpunt omdat die te wassen om voor alle niet-URI conformerende tekens tijdens runtime achtergrondkoolwaterstoffen is. Alle code van een query moet worden opgegeven in de parameters.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Mogelijk ziet u dit vraagt een `convertParamsToDictionary` methode die u nog niet hebt geschreven. Laten we dit nu doen aan het einde van het bestand:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Vervolgens gebruiken we de `NXOAuth2Request` methode voor het terughalen van gegevens van de API in JSON-indeling.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Tot slot laten we bekijken hoe u de gegevens terug naar de MasterViewController. De gegevens retourneert als zodanig en moet worden gedeserialiseerd en geladen in een object dat het MainViewController kan worden gebruikt. Voor dit doel, het skelet is een `User.m/h` -bestand dat wordt gemaakt van een gebruikersobject. U vult die User-object met de gegevens van de grafiek.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Het programmeervoorbeeld uitvoeren

De toepassing moet nu worden uitgevoerd als u hebt gebruikt het skelet of samen met de procedure gevolgd. De simulator Start en klik op **aanmelden** om de toepassing te gebruiken.

## <a name="get-security-updates-for-our-product"></a>Beveiligingsupdates voor ons product

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door het [Security TechCenter](https://technet.microsoft.com/security/dd252948) bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
