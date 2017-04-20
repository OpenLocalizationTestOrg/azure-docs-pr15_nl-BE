<properties
    pageTitle="Azure AD v2.0 impliciete stroom | Microsoft Azure"
    description="Bouw webtoepassingen met Azure AD v2.0 uitvoering van de impliciete stroom voor één pagina apps."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 protocollen - kuuroorden met behulp van de impliciete stroom
U kunt met het eindpunt v2.0 gebruikers inloggen op uw toepassingen één pagina met persoonlijke en werk/school accounts van Microsoft.  Enkele pagina en andere JavaScript-toepassingen uitvoeren die voornamelijk in een browser vlak een aantal interessante uitdagingen als het gaat om verificatie:

- De beveiligingskenmerken van deze apps zijn sterk afwijkt van de traditionele web van servertoepassingen.
- Veel servers vergunning & id-providers bieden geen ondersteuning voor het aanvragen van CORS.
- Volledige pagina browser wordt omgeleid van de app worden met name het gebruiksgemak invasieve.

Voor deze toepassingen (denk: AngularJS, Ember.js, React.js, enz) Azure AD de stroom impliciete verlening van OAuth 2.0 ondersteunt.  De impliciete stroom wordt beschreven in de [OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749#section-4.2).  Het belangrijkste voordeel is dat de app om tokens uit Azure Active Directory zonder het uitvoeren van een back-endserver referentie exchange.  Hierdoor kan de app de gebruiker aanmelden, sessie, en tokens op andere web-API's in de client JavaScript-code.  Er zijn een paar belangrijke overwegingen rekening te houden bij het gebruik van de impliciete stroom - specifiek rond de [client](http://tools.ietf.org/html/rfc6749#section-10.3) en de [gebruikersimitatie](http://tools.ietf.org/html/rfc6749#section-10.3).

Als u wilt dat de impliciete stroom en Azure AD verificatie toevoegen aan uw app JavaScript gebruikt, wordt u aangeraden dat u onze open-source JavaScript-bibliotheek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js)gebruiken.  Er zijn enkele AngularJS zelfstudies beschikbaar [hier](active-directory-appmodel-v2-overview.md#getting-started) om u aan de slag te helpen.  

Echter als u liever niet wilt gebruiken van een bibliotheek in uw pagina-app en protocolberichten verzenden zelf, volgt u de volgende algemene stappen uit.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Protocol-diagram
Het hele impliciete teken in stroom uitziet - de stappen worden hieronder uitvoerig beschreven.

![OpenId Swimlanes verbinding](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Stuur de aanvraag-in

Ondertekenen in eerste instantie de gebruiker in uw toepassing, kunt u een autorisatieaanvraag [OpenID verbinding](active-directory-v2-protocols-oidc.md) verzenden en krijgen een `id_token` van het endpoint v2.0:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Klik op de onderstaande koppeling voor het uitvoeren van dit verzoek. Na de ondertekening uw browser worden omgeleid naar `https://localhost/myapp/` met een `id_token` in de adresbalk.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| huurder | Vereist | De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's pachters.  Zie voor meer informatie [Basisbeginselen protocol](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| response_type | Vereist | Omvat `id_token` voor OpenID verbinding aanmelden.  Eventueel ook de response_type `token`. Met behulp van `token` kunt hier uw app voor het ontvangen van een toegangstoken direct vanuit het eindpunt machtigen zonder te hoeven maken van een tweede aanvraag naar het eindpunt van de machtigen.  Als u de `token` response_type, de `scope` parameter moet een scope die aangeeft welke bron voor het uitgeven van het token voor bevatten. |
| redirect_uri | aanbevolen | De redirect_uri van uw app, waarbij antwoorden verificatie kunnen worden verzonden en ontvangen door uw app.  Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, behalve het url-gecodeerd moet zijn. |
| toepassingsgebied | Vereist | Door spaties gescheiden lijst met bereiken.  OpenID verbinding maakt, moet deze het bereik opnemen `openid`, wat overeenkomt met de machtiging 'Aanmelden' in de gebruikersinterface van de toestemming.  Ook u kunt ook op te nemen de `email` of `profile` [scopes](active-directory-v2-scopes.md) voor toegang tot de aanvullende gegevens.  U mag ook andere scopes opnemen in deze aanvraag voor het verzoek om toestemming naar verschillende bronnen.  |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Moet `fragment` voor de impliciete stroom.  |
| staat | aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd.  Een tekenreeks met de inhoud die u kan zijn.  Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het [voorkomen van aanvallen van cross-site request vervalsing](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag, gegenereerd door de toepassing die u in de resulterende id_token als een claim opnemen wilt.  De app kan vervolgens controleren of deze waarde token replay-aanvallen te verminderen.  De waarde is meestal een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag.  |
| prompt | optioneel | Geeft het type van gebruikersinteractie is vereist.  De enige geldige waarden op dit moment zijn de 'aanmelden', 'geen' en 'toestemming'.  `prompt=login`Forceert de gebruikers hun referenties invoeren op dat verzoek negatief maken van eenmalige aanmelding.  `prompt=none`is het tegenovergestelde - zij zal erop toezien dat de gebruiker niet wordt weergegeven met welke vragen.  Het eindpunt v2.0 retourneert een fout als de aanvraag kan niet worden voltooid zonder via eenmalige aanmelding.  `prompt=consent`het dialoogvenster OAuth toestemming zal worden geactiveerd nadat de gebruiker zich aanmeldt, vraagt de gebruiker machtigen om de app. |
| login_hint | optioneel | Kan worden gebruikt voor het vooraf invullen van het veld gebruikersnaam/e-mailadres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tevoren.  Vaak apps Gebruik deze parameter tijdens vervolgverificatie dat de gebruikersnaam al geëxtraheerd uit een vorige-in met de `preferred_username` claimen. |
| domain_hint | optioneel | Een van `consumers` of `organizations`.  Als opgenomen, wordt deze overgeslagen op basis van een e-mailbericht detectieproces van de gebruiker wordt doorlopen op de pagina, v2.0 inloggen leidt tot een iets meer gestroomlijnde gebruikersinterface.  Vaak apps met deze parameter wordt gebruikt tijdens een nieuwe verificatie door uit te pakken de `tid` opvragen bij de id_token.  Als de `tid` claimen waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Op dit moment wordt de gebruiker gevraagd hun referenties invoeren en de verificatie te voltooien.  Het eindpunt v2.0 wordt er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter.  Als de gebruiker niet heeft ingestemd met een van deze machtigingen, het aan de gebruiker gevraagd om instemming met de vereiste machtigingen beschikt.  Details van [machtigingen, toestemming te geven, en de huurder met meerdere apps die hier](active-directory-v2-scopes.md).

Zodra de gebruiker wordt geverifieerd en toestemming verleent, het eindpunt v2.0 resulteert in een reactie op uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven de `response_mode` parameter.

#### <a name="successful-response"></a>Positief antwoord

Een positief antwoord met `response_mode=fragment` en `response_type=id_token+token` lijkt op het volgende, waarbij regeleinden voor leesbaarheid:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | Opgenomen als `response_type` bevat `token`. Het toegangstoken dat de app is aangevraagd, in dit geval voor de Microsoft Graph.  Het toegangstoken niet moet worden gedecodeerd of anderszins worden geïnspecteerd, kan deze worden behandeld als één tekenreeks. |
| token_type | Opgenomen als `response_type` bevat `token`.  Altijd `Bearer`. |
| expires_in | Opgenomen als `response_type` bevat `token`.  Geeft aan hoeveel seconden die het token geldig is voor caching doeleinden. |
| toepassingsgebied | Opgenomen als `response_type` bevat `token`.  Geeft aan dat de scope(s) waarvoor de access_token geldig zijn. |
| id_token | De id_token die de app heeft aangevraagd. De id_token kunt u controleren of de identiteit van de gebruiker en het begin van een sessie met de gebruiker.  Meer informatie over id_tokens en hun inhoud wordt opgenomen in de [token eindpuntreferentie v2.0](active-directory-v2-tokens.md).  |
| staat | Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |


#### <a name="error-response"></a>Fout antwoord
Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |

## <a name="validate-the-idtoken"></a>Valideren van de id_token
NET ontvangen een id_token is niet voldoende voor de verificatie van de gebruiker. u moet de handtekening van de id_token en controleer of de vorderingen in het token per eisen van uw app.  Het eindpunt v2.0 maakt gebruik van [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en openbare-sleutelcryptografie ondertekenen van tokens en controleer of deze geldig zijn.

Kunt u kiezen voor het valideren van de `id_token` in client-code, maar gebruikelijk is voor het verzenden van de `id_token` om een back-endserver en er de validatie uit te voeren.  Nadat u hebt geverifieerd dat de handtekening van de id_token, zijn er enkele vorderingen u moet controleren.  Zie voor meer informatie, met inbegrip van [Tokens valideren](active-directory-v2-tokens.md#validating-tokens) en [Belangrijke informatie over het ondertekenen van sleutel Rollover](active-directory-v2-tokens.md#validating-tokens) [v2.0 token verwijzing](active-directory-v2-tokens.md) .  Het is raadzaam om tokens die gebruikmaken van een bibliotheek voor het parseren en valideren - er ten minste één beschikbaar is voor de meeste talen en platforms.
<!--TODO: Improve the information on this-->

U kunt ook extra claims, afhankelijk van het scenario te valideren.  Sommige algemene validaties omvatten:

- Ervoor te zorgen dat de gebruikersorganisatie heeft aangemeld bij de app.
- Ervoor te zorgen dat de gebruiker heeft goede toestemming/rechten
- Ervoor te zorgen dat een bepaalde mate van verificatie heeft plaatsgevonden, zoals een meerledige verificatie.

Zie voor meer informatie over de vorderingen in een id_token, de [tokenverwijzing v2.0-eindpunt](active-directory-v2-tokens.md).

Zodra u hebt de id_token volledig gevalideerd, kunt u beginnen met een sessie van de gebruiker en de vorderingen in de id_token gebruiken voor informatie over de gebruiker in uw app.  Deze informatie kan worden gebruikt voor het weergeven, records, vergunningen, enz.

## <a name="get-access-tokens"></a>Toegangstokens ophalen

Nu dat u de gebruiker hebt aangemeld bij uw app voor één pagina, krijgt u toegangstokens voor aanvragende web API's beveiligd door Azure AD, zoals de [Microsoft Graph](https://graph.microsoft.io).  Zelfs als u al een token met behulp van de `token` response_type, kunt u deze methode tokens om extra bronnen te verkrijgen zonder dat de gebruiker opnieuw inloggen omleiden.

In de normale manier verbinding maken met OpenID/OAuth, u doet dit door een aanvraag naar de v2.0 `/token` eindpunt.  Echter biedt het eindpunt v2.0 geen ondersteuning voor CORS aanvragen, zodat gesprekken voor het ophalen en vernieuwen van tokens AJAX niet aan de orde is.  In plaats daarvan kunt u de impliciete stroom in een verborgen iframe nieuwe tokens krijgen voor andere web-API's: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Probeer te kopiëren en plakken de onderstaande aanvraag in een tabblad browser! (Vergeet niet te vervangen door de `domain_hint` en de `login_hint` waarden met de juiste waarden voor de gebruiker)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| huurder | Vereist | De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's pachters.  Zie voor meer informatie [Basisbeginselen protocol](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| response_type | Vereist | Omvat `id_token` voor OpenID verbinding aanmelden.  Het kan ook andere response_types, zoals `code`. |
| redirect_uri | aanbevolen | De redirect_uri van uw app, waarbij antwoorden verificatie kunnen worden verzonden en ontvangen door uw app.  Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, behalve het url-gecodeerd moet zijn. |
| toepassingsgebied | Vereist | Door spaties gescheiden lijst met bereiken.  Voor het ophalen van tokens, bevatten alle [scopes](active-directory-v2-scopes.md) u nodig hebt voor de bron is van belang.  |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Zijn `query`, `form_post`, of `fragment`.  |
| staat | aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd.  Een tekenreeks met de inhoud die u kan zijn.  Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het voorkomen van aanvallen van cross-site request vervalsing.  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag, gegenereerd door de toepassing die u in de resulterende id_token als een claim opnemen wilt.  De app kan vervolgens controleren of deze waarde token replay-aanvallen te verminderen.  De waarde is meestal een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag.  |
| prompt | Vereist | Gebruik voor het vernieuwen van & tokens ophalen in een verborgen iframe, `prompt=none` ervoor zorgen dat de iframe niet op de aanmeldingspagina v2.0 vastloopt nu en onmiddellijk geretourneerd. |
| login_hint | Vereist | Voor het vernieuwen van & tokens ophalen in een verborgen iframe, moet u de gebruikersnaam van de gebruiker opnemen in deze geheugensteun om onderscheid te maken tussen meerdere sessies die de gebruiker op een bepaald moment in de tijd hebben kan. U kunt de gebruikersnaam ophalen uit een vorige-in met de `preferred_username` claimen. |
| domain_hint | Vereist | Een van `consumers` of `organizations`.  Vernieuwen & tokens ophalen in een verborgen iframe, moet u de domain_hint in de aanvraag opnemen.  U moet uitpakken van de `tid` opvragen bij de id_token van een vorige aanmelden om te bepalen welke waarde u moet gebruiken.  Als de `tid` claimen waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Dank aan de `prompt=none` parameter, dit verzoek wordt ofwel slagen of mislukken onmiddellijk en terugkeren naar de toepassing.  Een positief antwoord verzonden naar uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven de `response_mode` parameter.

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord met `response_mode=fragment` eruit komt te zien:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | Het token dat de app aangevraagd. |
| token_type | Altijd `Bearer`. |
| staat | Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |
| expires_in | Hoe lang het toegangstoken geldig is (in seconden). |
| toepassingsgebied | De scopes die geldt voor het toegangstoken. |

#### <a name="error-response"></a>Fout antwoord
Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken.  In het geval van `prompt=none`, een verwachte fout is:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |

Als u deze fout in de iframe-aanvraag ontvangt, de gebruiker zich interactief aanmelden opnieuw voor het ophalen van een nieuwe beveiligingssleutel.  U kunt in dit geval in een manier die zinvol is voor uw toepassing.

## <a name="refreshing-tokens"></a>Tokens vernieuwen

Beide `id_token`s en `access_token`s verloopt na een korte periode, zodat uw app moet worden voorbereid voor het vernieuwen van deze tokens regelmatig.  Om beide typen token vernieuwt, kunt u uitvoeren dezelfde aanvraag verborgen iframe boven het gebruik van de `prompt=none` parameter Azure AD werking besturen.  Als u wilt ontvangen van een nieuw `id_token`, gebruik `response_type=id_token` en `scope=openid`, alsmede een `nonce` parameter.


## <a name="send-a-sign-out-request"></a>Een bord aanvraag verzenden

De OpenIdConnect `end_session_endpoint` wordt momenteel niet ondersteund door het eindpunt v2.0. Dit betekent dat de app niet kan een aanvraag verzenden naar het eindpunt v2.0 sessie van een gebruiker beëindigen en wissen van cookies die worden ingesteld door het eindpunt v2.0.
Ondertekenen van een gebruiker, kunt uw app gewoon beëindigen van de eigen sessie van de gebruiker en de sessie met de v2.0 eindpunt intact laat.  De volgende keer dat de gebruiker zich probeert aan te melden, zien ze een pagina "Kies de account" met hun actief ingelogd rekeningen.
Op deze pagina, kan de gebruiker kiezen voor het ondertekenen van een rekening, de sessie met het eindpunt v2.0 te beëindigen.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->