<properties
    pageTitle="Hoe gebruik iOS SDK voor Azure mobiele Apps"
    description="Hoe gebruik iOS SDK voor Azure mobiele Apps"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Hoe gebruik iOS Client Library voor Azure mobiele Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u voor het uitvoeren van veelvoorkomende scenario's met behulp van de nieuwste [Azure Mobile Apps iOS SDK][1]. Bent u nieuw in Azure Mobile Apps, eerste volledige [Azure Mobile Apps Quick Start] voor het maken van een back-end een tabel maken en downloaden van een vooraf gebouwde iOS Xcode project. In deze handleiding richten we ons op de client-side iOS SDK. Zie voor meer informatie over de SDK aan de serverzijde voor de backend, de HOWTOs Server SDK.

## <a name="reference-documentation"></a>Documentatie

De documentatie bij de iOS client SDK bevindt zich hier: [Azure Mobile Apps iOS Client verwijzing][2].

## <a name="supported-platforms"></a>Ondersteunde Platforms

De iOS SDK ondersteunt doelstelling C projecten, projecten Swift 2.2 en Swift 2.3 projecten voor iOS versie 8.0 of hoger.

Een webweergave de "server-flow"-verificatie gebruikt voor de gebruikersinterface weergegeven.  Als het apparaat niet kan worden een UI WebView presenteren en vervolgens een andere verificatiemethode is vereist is die buiten de werkingssfeer van het product.  
Deze SDK is dus niet geschikt voor controle-type of ook beperkte apparaten.

##<a name="Setup"></a>Installatie en vereisten

Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel hetzelfde schema als de tabellen in deze zelfstudie. Deze handleiding wordt ervan uitgegaan dat in uw code u verwijzen naar `MicrosoftAzureMobile.framework` en `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Procedure: Client maken

Voor toegang tot een backend Azure Mobile Apps in uw project, maakt u een `MSClient`. Vervangen `AppUrl` met de URL van de app. U mag verlaten `gatewayURLString` en `applicationKey` leeg. Als u een gateway voor verificatie instellen, vullen `gatewayURLString` met de URL van de gateway.

**Doelstelling C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Procedure: tabelverwijzing maken

Access- of als u gegevens wilt maken van een verwijzing naar de back-end-tabel. Vervangen `TodoItem` met de naam van de tabel

**Doelstelling C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Procedure: gegevens opvragen

Query wilt maken van een databasequery, de `MSTable` object. De volgende query haalt u alle items in `TodoItem` en de tekst van elk item wordt vastgelegd.

**Doelstelling C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Procedure: Filter gegevens geretourneerd

Om resultaten te filteren, zijn er veel opties beschikbaar.

Als u een predikaat, gebruiken een `NSPredicate` en `readWithPredicate`. De volgende filters gegevens om alleen niet-voltooide items die moeten worden geretourneerd.

**Doelstelling C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Procedure: MSQuery gebruiken

Om een complexe query (inclusief sorteren en pagineren) uitvoert, maakt u een `MSQuery` object, rechtstreeks of via een predikaat:

**Doelstelling C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`kunt u verschillende query-gedrag bepalen.

* Geef de volgorde van de resultaten
* Beperken van velden die u wilt retourneren
* Beperken hoeveel records retourneren
* Totaal aantal reactie geven
* Aangepaste query string parameters opgeven in de aanvraag
* Extra functies toepassen

Uitvoeren van een `MSQuery` query door het aanroepen van `readWithCompletion` op het object.

## <a name="sorting"></a>Procedure: gegevens met MSQuery sorteren

Als u wilt sorteren van resultaten, we kijken naar een voorbeeld. Aanroepen om te sorteren op het veld 'tekst' oplopende, vervolgens op 'voltooid' aflopende, `MSQuery` als volgt:

**Doelstelling C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Procedure: velden beperken en queryreeksparameters met MSQuery uitvouwen

Geef de namen van de velden in de eigenschap **selectFields** alleen de velden in een query worden geretourneerd. In het volgende voorbeeld wordt alleen de tekst en de ingevulde velden:

**Doelstelling C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Extra queryreeksparameters opnemen in de serveraanvraag (bijvoorbeeld omdat ze maakt gebruik van een eigen server-side script), vullen `query.parameters` als volgt:

**Doelstelling C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Procedure: paginaformaat configureren

Met Azure Mobile Apps bepaalt de grootte van het aantal records die zijn opgehaald uit de back-end-tabellen tegelijk. Een aanroep naar `pull` gegevens zou vervolgens batch-up van gegevens, op basis van de paginagrootte van deze, totdat er geen records meer om op te halen.

Het is mogelijk voor het configureren van een formaat met behulp van **MSPullSettings** , zoals hieronder wordt weergegeven. De standaardgrootte van de pagina is 50 en in het onderstaande voorbeeld gewijzigd in 3.

U kunt een ander paginaformaat voor betere prestaties. Als er een groot aantal kleine records, minder een hoge paginaformaat server weergeeft. 

Deze instelling bepaalt u het paginaformaat op de client. Als de client om een groter formaat vraagt dan de backend Mobile Apps ondersteunt, wordt het paginaformaat afgetopt op het maximum dat de backend is geconfigureerd voor ondersteuning. 

Deze instelling is ook het _aantal_ records met gegevens, niet de _grootte in bytes_.

Als u het paginaformaat client, [moet u de paginagrootte op de server verhogen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size).

**Doelstelling C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Procedure: gegevens invoegen

Om een nieuwe tabelrij invoegt, maakt u een `NSDictionary` en het aanroepen van `table insert`. Als [Dynamische Schema] is ingeschakeld, genereert de mobiele backend Azure App-Service automatisch nieuwe kolommen op basis van de `NSDictionary`.

Als `id` wordt niet verstrekt de backend automatisch een nieuwe unieke ID genereert. Geef uw eigen `id` gebruik van e-mail adressen, gebruikersnamen, of uw eigen aangepaste waarden als ID. Biedt uw eigen ID kan eenvoudig joins en business-georiënteerde database logica.

De `result` bevat het nieuwe item dat is ingevoegd. Afhankelijk van de logica van de server, kan deze extra of gewijzigde gegevens in vergelijking met wat is doorgegeven aan de server hebben.

**Doelstelling C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Procedure: gegevens wijzigen

Wijzigen om te werken in een bestaande rij, een artikel en een aanroep van `update`:

**Doelstelling C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

U kunt ook de rij-ID en het bijgewerkte veld leveren:

**Doelstelling C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Ten minste de `id` kenmerk moet worden ingesteld bij het maken van updates.

##<a name="deleting"></a>Procedure: gegevens verwijderen

Aanroepen voor een item verwijdert, `delete` met het artikel:

**Doelstelling C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

U kunt ook verwijderen door middel van een rij-ID:

**Doelstelling C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Ten minste de `id` kenmerk moet worden ingesteld bij het vervaardigen worden verwijderd.

##<a name="customapi"></a>Procedure: aangepaste API aanroepen

Met een aangepaste API kan geen back-end-functionaliteit worden blootgesteld. Dit hoeft niet te worden toegewezen aan de bewerking van een tabel. Niet alleen doen krijgt u meer controle over de berichten, maar u kunt zelfs lezen of instellen, kop en de hoofdtekst antwoord wijzigen. Lees meer informatie over het maken van een aangepaste API op de backend, [Aangepaste API 's](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Om een aangepaste API aanroept, roept u `MSClient.invokeAPI`. De aanvraag en het antwoord inhoud worden behandeld als JSON. Gebruik van andere mediatypen [gebruik van de andere overbelasting van `invokeAPI` ] [ 5].  Maken een `GET` aanvragen in plaats van een `POST` aanvragen, set-parameter `HTTPMethod` naar `"GET"` en parameter `body` te `nil` (omdat de GET-aanvragen zijn geen berichtteksten.) Als uw aangepaste API ondersteunt andere HTTP-woorden, `HTTPMethod` op de juiste manier.

**Doelstelling C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Procedure: Register push-sjablonen voor het verzenden van meldingen voor meerdere platforms

Doorgeven voor het registreren van sjablonen, sjablonen met de methode **client.push registerDeviceToken** in de clienttoepassing.

**Doelstelling C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

De sjablonen van het type NSDictionary en kunnen bevatten meerdere sjablonen in de volgende notatie:

**Doelstelling C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Alle codes worden verwijderd uit de aanvraag voor beveiliging.  Zie [werken met de back-end-server .NET SDK voor Azure Mobile Apps]tags toevoegen aan sjablonen in installaties of installaties,[4].  Om meldingen te verzenden met behulp van deze geregistreerde sjablonen, werken met [Melding Hubs API's][3].

##<a name="errors"></a>Procedure: Fouten afhandelen

Wanneer u een mobiele backend Azure App Service aanroept, wordt het voltooiingspercentage blok bevat een `NSError` parameter. Wanneer een fout optreedt, wordt deze parameter niet nul is. In uw code, moet u deze parameter inschakelt en afhandelen van de fout als nodig is, zoals in de voorgaande codefragmenten.

Het bestand [`<WindowsAzureMobileServices/MSError.h>`] [6] definieert de constanten `MSErrorResponseKey`, `MSErrorRequestKey`, en `MSErrorServerItemKey`. Meer gegevens met betrekking tot de fout opvragen:

**Doelstelling C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Het bestand definieert ook constanten voor elke foutcode:

**Doelstelling C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Procedure: verificatie van gebruikers met de Active Directory-verificatie-bibliotheek

U kunt Active Directory verificatie bibliotheek (ADAL) gebruikers inloggen op uw toepassing met Azure Active Directory. Stroom-clientverificatie via een identiteitsprovider SDK verkiezen boven het gebruik is de `loginWithProvider:completion:` methode.  Clientverificatie stroom biedt een meer native UX feel en zorgt voor extra aanpassingen.

1. Uw mobiele app back-end voor aanmelding bij AAD configureren aan de hand van de [App-Service voor Active Directory-aanmelding configureren] [ 7] zelfstudie. Zorg ervoor dat de optionele stap van het registreren van een native clienttoepassing. Voor iOS, raden wij aan dat de omleiding URI van het formulier is `<app-scheme>://<bundle-id>`. Zie voor meer informatie het [ADAL iOS quickstart][8].

2. Installeer ADAL met Cocoapods. Uw Podfile om op te nemen van de volgende definitie van **Uw PROJECT** vervangen door de naam van het project Xcode bewerken:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   en de schil:

        pod 'ADALiOS'

3. Uitvoeren met behulp van de Terminal, `pod install` uit de map waarin u uw project en open vervolgens de gegenereerde Xcode werkruimte (niet in het project).

4. De volgende code toevoegen aan uw toepassing, afhankelijk van de taal die u gebruikt. Controleer in elk, deze vervangingen:

    * **INSERT-hier-instantie** vervangen door de naam van de huurder waarin u uw toepassing hebt ingericht. De https://login.windows.net/contoso.onmicrosoft.com moet worden. Deze waarde kan worden gekopieerd vanaf het tabblad domein in uw Azure Active Directory in de [Azure klassieke portal].
    * **INSERT RESOURCE-ID hier** vervangen door de client-ID voor uw mobiele app backend. Op het tabblad **Geavanceerd** onder **Azure Active Directory-instellingen** in de portal kunt u de client-ID.
    * **INSERT CLIENT-ID hier** vervangen door de client-ID die u hebt gekopieerd uit de native clienttoepassing.
    * **INSERT REDIRECT-URI-hier -** vervangen van uw site _/.auth/login/done_ eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met _https://contoso.azurewebsites.net/.auth/login/done_.

**Doelstelling C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Procedure: verificatie van gebruikers met de SDK Facebook voor iOS

U kunt de SDK Facebook voor iOS gebruikers inloggen op uw toepassing met behulp van Facebook.  Met behulp van een stroom clientverificatie is te verkiezen boven met behulp van de `loginWithProvider:completion:` methode.  De verificatie van de client stroom biedt een meer native UX feel en zorgt voor extra aanpassingen.

1. Uw backend mobiele app voor Facebook aanmelden configureren aan de hand van de [Service voor aanmelding Facebook App configureren] [ 9] zelfstudie.

2. Installeer de SDK Facebook voor iOS door de [Facebook-SDK voor iOS - introductie] [ 10] documentatie. U kunt het iOS platform in plaats van een app maken, toevoegen aan uw bestaande inschrijving. 

3. Facebook van documentatie bevat enkele doelstelling-C-code in de gemachtigde App. Als u van **Swift gebruikmaakt**, kunt u de volgende vertalingen voor AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Naast het toevoegen van `FBSDKCoreKit.framework` het project, ook een verwijzing toevoegen aan `FBSDKLoginKit.framework` op dezelfde manier. 

4. De volgende code toevoegen aan uw toepassing, afhankelijk van de taal die u gebruikt. 

**Doelstelling C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Procedure: verificatie van gebruikers met Twitter Fabric voor iOS

U kunt vervaardigd voor iOS gebruikers inloggen op uw toepassing met behulp van Twitter. Clientverificatie stroom is te verkiezen boven met behulp van de `loginWithProvider:completion:` de methode, biedt een meer native UX feel en zorgt voor extra aanpassingen.

1. Uw mobiele app back-end voor aanmelding bij Twitter instellen door de zelfstudie [Service voor aanmelding Twitter App configureren](app-service-mobile-how-to-configure-twitter-authentication.md) .

2. Weefsels aan uw project toevoegen door het volgende op de documentatie [vervaardigd voor iOS - aan de slag] en het instellen van TwitterKit.

    > [AZURE.NOTE] Standaard Fabric Twitter-toepassing voor u gemaakt. U kunt voorkomen dat een toepassing maken met het registreren van de sleutel van de consument en de consumentgeheim u eerder met de volgende codefragmenten hebt gemaakt.  U kunt ook de sleutel van de consument en de consumentgeheim waarden die u opgeeft met App-Service met de waarden die u in het [Dashboard weefsel ziet]vervangen. Als u deze optie kiest, moet u, zoals de callback-URL naar een tijdelijke waarde instellen `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Als u kiest voor de geheimen die u eerder hebt gemaakt, kunt u de volgende code toevoegen aan uw gemachtigde App:
    
    **Doelstelling C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. De volgende code toevoegen aan uw toepassing, afhankelijk van de taal die u gebruikt. 

**Doelstelling C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Procedure: verificatie van gebruikers met de Google-In de SDK voor iOS

U kunt de Google-In de SDK voor iOS gebruikers inloggen op uw toepassing met behulp van een Google-account.  Google kondigde onlangs wijzigingen in hun OAuth-beveiligingsbeleid.  Deze wijzigingen vereisen het gebruik van de SDK van Google in de toekomst.

1. Uw mobiele app back-end voor aanmelding bij Google instellen door de zelfstudie [App Service voor Google-aanmelding te configureren](app-service-mobile-how-to-configure-google-authentication.md) .

2. Installeer de Google SDK voor iOS door de [Start integreren Google aanmelden voor iOS -](https://developers.google.com/identity/sign-in/ios/start-integrating) documentatie. U kunt de sectie 'Verifiëren met een back-endserver' overslaan.

3. Voeg het volgende toe aan uw gemachtigde van `signIn:didSignInForUser:withError:` methode, afhankelijk van de taal die u gebruikt.

**Doelstelling C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Zorg ervoor dat u ook het volgende aan toevoegen `application:didFinishLaunchingWithOptions:` in uw gemachtigde app, "SERVER_CLIENT_ID" te vervangen door dezelfde ID die u gebruikt voor het configureren van de App Service in stap 1.

**Doelstelling C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Voeg de volgende code in uw toepassing in een UIViewController waarmee de `GIDSignInUIDelegate` -protocol, afhankelijk van de taal die u gebruikt.  U bent afgemeld voordat opnieuw wordt ondertekend en hoewel u niet hoeft te uw referenties opnieuw invoeren, ziet u een dialoogvenster toestemming.  Deze methode alleen oproepen als het sessietoken is verlopen.
 
 **Doelstelling C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure mobiele Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamische Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Fabric-Dashboard]: https://www.fabric.io/home
[Vervaardigd voor iOS - aan de slag]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
