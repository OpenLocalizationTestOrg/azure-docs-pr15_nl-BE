<properties
    pageTitle="Azure AD iOS aan de slag | Microsoft Azure"
    description="Het bouwen van een iOS-toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Azure AD integreren met een iOS-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD voorziet in de bibliotheek van Active Directory-verificatie of ADAL, iOS-clients die toegang moeten krijgen tot beveiligde bronnen.  ADAL van enige doel in het leven is kunt u gemakkelijk uw app toegangstokens ophalen.  Om aan te tonen hoe gemakkelijk het is, bouw hier we een toepassing doelstelling C takenlijst die:

-   Haalt access tokens voor de Azure AD Graph API met het [verificatieprotocol OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)aanroept.
-   Zoekt in een map voor gebruikers met een bepaalde alias.

Als u de volledige toepassing van de werken, moet u naar:

2. Uw toepassing met Azure Active Directory registreren.
3. Installeren en configureren van ADAL.
5. ADAL gebruiken om tokens uit Azure Active Directory.

Ophalen is gestart, [het skelet app downloaden](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) of [het voltooide monster](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  U moet ook een huurder Azure AD kunt u gebruikers maken en registreren van een toepassing.  Als u een huurder, [Lees hoe u een](active-directory-howto-tenant.md)nog geen hebt.

> [AZURE.TIP] Probeer het voorbeeld van onze nieuwe [developer-portal](https://identity.microsoft.com/Docs/iOS) waarmee u slag met Azure Active Directory in een paar minuten!  De developer-portal begeleidt u door het proces van het registreren van een app en Azure AD integreren in uw code.  Wanneer u klaar bent, hebt u een eenvoudige toepassing die gebruikers in uw huurder en een backend die kunnen tokens ontvangen en gevalideerd kan worden geverifieerd. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. bepalen wat uw URI omleiden voor iOS*

Om veilig uw toepassingen in bepaalde scenario's voor eenmalige aanmelding te starten moet u een **Redirect URI** maken in een bepaalde indeling. Een Redirect URI wordt gebruikt om ervoor te zorgen dat de tokens terugkeren naar de juiste toepassing die voor hen wordt gevraagd.

De iOS-indeling voor een Redirect URI is:

```
<app-scheme>://<bundle-id>
```

-   **aap schema** - dit is geregistreerd in uw project XCode. Het is hoe andere toepassingen u kunnen bellen. U vindt dit onder het Info.plist -> URL-types -> URL-id. Als u niet al een of meer andere zijn geconfigureerd, moet u een maken.
-   **bundel-id** - dit is de bundel-id vinden onder 'identiteit' un de projectinstellingen in XCode.

Een voorbeeld voor deze QuickStart-code is: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. de toepassing van het DirectorySearcher registreren*
Als u wilt dat uw app tokens ophalen, moet u eerst te registreren in uw huurder Azure AD en verleent deze machtiging voor toegang tot de Azure AD Graph API:

-   Aanmelden bij de Azure Management Portal
-   Klik in de nav links op in **Active Directory**
-   Selecteer een huurder waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klik op **toevoegen** in de onderste lade.
-   Volg de aanwijzingen en een nieuwe **Native Client-toepassing**maken.
    -   De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    -   Het **Omleiden van Uri** is een schema en tekenreeks van de combinatie waarmee Azure AD token antwoorden terug.  Een specifieke waarde aan uw toepassing op basis van de bovenstaande gegevens invoeren.
-   Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke client-id.  Moet u deze waarde in de volgende secties, dus kopiëren vanaf het tabblad **configureren** .
- Ook in het tabblad **configureren** , Ga naar de sectie 'Machtigingen voor andere toepassingen'.  Voeg de machtiging **toegang tot uw organisatie Directory** onder **Overgedragen machtigingen**voor de toepassing 'Azure Active Directory'.  Hierdoor kan uw toepassing kan een query uitvoeren op de Graph API voor gebruikers.

## <a name="3-install--configure-adal"></a>*3. Installeer & ADAL configureren*
U kunt nu dat er een toepassing in Azure AD, ADAL installeren en uw identiteit gerelateerde code te schrijven.  Voor ADAL te kunnen communiceren met Azure Active Directory moet, u te verstrekken informatie over de registratie van uw app.
-   Begin met het ADAL toe te voegen aan het DirectorySearcher-project met behulp van Cocapods.

```
$ vi Podfile
```
Voeg het volgende toe aan dit podfile:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Nu laden de podfile met cocoapods. Hiermee maakt u een nieuwe XCode-werkruimte worden geladen.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Open het bestand plist in het project QuickStart `settings.plist`.  Vervang de waarden van de elementen in de sectie aangepast aan de waarden die u in de Portal Azure ingevoerd.  De code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `tenant` is het domein van de huurder Azure AD, bv. contoso.onmicrosoft.com
    -   De `clientId` is de clientId van de toepassing die u hebt gekopieerd vanaf de portal.
    -   De `redirectUri` is de omleiding url die u hebt geregistreerd in de portal.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. Gebruik ADAL Tokens ophalen van AAD*
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, gewoon een completionBlock roept `+(void) getToken : `, en ADAL doet de rest.  

-   In de `QuickStart` project geopend `GraphAPICaller.m` en zoek de `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` commentaar bij de bovenkant.  Dit is waar u ADAL doorgeven de coördinaten via een CompletionBlock om te communiceren met Azure Active Directory en hoe deze tokens in de cache.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Nu moeten we deze token gebruikt om te zoeken naar gebruikers in de grafiek. Zoek de `// TODO: implement SearchUsersList` commentThis-methode maakt een GET-aanvraag naar de Azure AD Graph API aan query voor waarvan UPN met de opgegeven zoekterm begint.  Maar query's op de Graph API, moet u een access_token in de `Authorization` kop van de aanvraag - dit is waar ADAL binnenkomt.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Als uw app een token aanvragen door het aanroepen van `getToken(...)`, ADAL zal proberen om terug te keren een token zonder dat de gebruiker om referenties gevraagd.  Als ADAL bepaalt dat de gebruiker zich aanmelden moet bij een token ophalen, wordt het een aanmeldingsdialoogvenster weergegeven referenties van de gebruiker verzamelen en retourneren een token bij een geslaagde verificatie.  Als ADAL kan retourneren een token voor welke reden dan ook, het genereert een `AdalException`.
- Merk op dat de `AuthenticationResult` -object bevat een `tokenCacheStoreItem` -object dat kan worden gebruikt om uw app moet mogelijk informatie te verzamelen.  In de QuickStart, `tokenCacheStoreItem` wordt gebruikt om te bepalen als authenitcation al heeft plaatsgevonden.


## <a name="step-5-build-and-run-the-application"></a>Stap 5: Bouwen en uitvoeren van de toepassing



Gefeliciteerd! U nu beschikken over een werkende iOS-toepassing die de mogelijkheid heeft om gebruikers te verifiëren, veilig gebruik OAuth 2.0 Web-API's aanroepen en elementaire informatie over de gebruiker.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van de huurder met sommige gebruikers.  Voer uw app QuickStart en meld u aan met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van de UPN.  De toepassing sluiten en opnieuw worden uitgevoerd.  U ziet hoe de sessie blijft intact.

ADAL kunt u al deze gemeenschappelijke identiteit-functies opnemen in uw toepassing.  Dit zorgt voor het vuile werk voor u - Cachebeheer, ondersteuning OAuth-protocol, dat de gebruiker een login UI, het vernieuwen van verlopen tokens en nog veel meer.  Alles wat u moet weten is dat een enkele API-aanroep `getToken`.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Aanvullende scenario 's
U kunt nu op verplaatsen naar nog meer scenario's.  U kunt proberen:

- [Een Node.JS Web API met Azure Active Directory beveiligen](active-directory-devquickstarts-webapi-nodejs.md)
- Informatie over [het inschakelen van SSO cross-app op iOS met ADAL](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
