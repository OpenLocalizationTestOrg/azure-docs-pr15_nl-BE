<properties
    pageTitle="Azure AD .NET Protocol overzicht | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe de toegang tot webtoepassingen en web-API's in uw huurder met Azure Active Directory en OpenID verbinding met HTTP-berichten."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>De toegang tot webtoepassingen die via OpenID verbinding en Azure Active Directory

[OpenID-verbinding](http://openid.net/specs/openid-connect-core-1_0.html) is een eenvoudige identiteit laag bovenop de OAuth 2.0-protocol. OAuth 2.0 mechanismen voor het verkrijgen en gebruiken van **toegangstokens** voor toegang tot beveiligde bronnen definieert, maar definiëren niet standaard methoden om identiteit informatie te verstrekken. OpenID verbinding wordt de verificatie geïmplementeerd als een uitbreiding van het autorisatieproces OAuth 2.0 verstrekken van informatie over de gebruiker in de vorm van een `id_token` die controleert de identiteit van de gebruiker, alsmede informatie over de gebruiker basisprofiel.

OpenID verbinden is onze aanbeveling, als u een webtoepassing die wordt gehost op een server en is toegankelijk via een browser.

## <a name="authentication-flow-using-openid-connect"></a>Overdracht van verificatie OpenID verbinding maken met

De meest elementaire stroom-in bevat de volgende stappen - elk van hen is hieronder uitvoerig beschreven.

![OpenId stroom verificatie verbinding maken](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Stuur de aanvraag-in

Wanneer uw webtoepassing moet de gebruiker te verifiëren, moet deze verwijs de gebruiker naar de `/authorize` eindpunt. Deze aanvraag is vergelijkbaar met de eerste zijde van de [OAuth 2.0 vergunning Code stroom](active-directory-protocols-oauth-code.md), met enkele belangrijke verschillen:

- De aanvraag omvat het bereik `openid` in de `scope` parameter.
- De `response_type` moet `id_token`.
- De aanvraag moet bevatten de `nonce` parameter.

Om een voorbeeld van een aanvraag als volgt eruit:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| huurder | Vereist | De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn bijvoorbeeld een huurder id's, `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tokens onafhankelijk van de huurder |
| client_id | Vereist | De toepassings-Id toegewezen aan uw app bij het registreren met Azure Active Directory. U kunt dit vinden in de klassieke Azure-Portal. Klik op **Active Directory**, klikt u op de map, klik op de toepassing en klikt u vervolgens op **configureren** |
| response_type | Vereist | Omvat `id_token` voor OpenID verbinding aanmelden.  Het kan ook andere response_types, zoals `code`. |
| toepassingsgebied | Vereist | Door spaties gescheiden lijst met bereiken.  OpenID verbinding maakt, moet deze het bereik opnemen `openid`, wat overeenkomt met de machtiging 'Aanmelden' in de gebruikersinterface van de toestemming.  U mag ook andere scopes opnemen in dit verzoek om toestemming wordt gevraagd. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag, gegenereerd door de toepassing die u in de resulterende opnemen wilt `id_token` als een claim.  De app kan vervolgens controleren of deze waarde token replay-aanvallen te verminderen.  De waarde is meestal een willekeurige, unieke tekenreeks of de GUID die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag.  |
| redirect_uri | aanbevolen | De redirect_uri van uw app, waarbij antwoorden verificatie kunnen worden verzonden en ontvangen door uw app.  Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, behalve het url-gecodeerd moet zijn. |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar uw app.  Ondersteunde waarden zijn `form_post` voor *http-formulier boeken* of `fragment` voor *URL-fragment*.  Voor webtoepassingen raden wij aan met behulp van `response_mode=form_post` op de meest beveiligde overdracht van tokens voor uw toepassing.  
| staat | aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd.  Een tekenreeks met de inhoud die u kan zijn.  Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het [voorkomen van aanvallen van cross-site request vervalsing](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |
| prompt | optioneel | Geeft het type van gebruikersinteractie is vereist.  De enige geldige waarden op dit moment zijn de 'aanmelden', 'geen' en 'toestemming'.  `prompt=login`Forceert de gebruikers hun referenties invoeren op dat verzoek negatief maken van eenmalige aanmelding.  `prompt=none`is het tegenovergestelde - zij zal erop toezien dat de gebruiker niet wordt weergegeven met welke vragen.  Als de aanvraag kan niet worden voltooid in stille modus via eenmalige aanmelding, wordt het eindpunt een fout geretourneerd.  `prompt=consent`het dialoogvenster OAuth toestemming zal worden geactiveerd nadat de gebruiker zich aanmeldt, vraagt de gebruiker machtigen om de app. |
| login_hint | optioneel | Kan worden gebruikt voor het vooraf invullen van het veld gebruikersnaam/e-mailadres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tevoren.  Vaak apps Gebruik deze parameter tijdens vervolgverificatie dat de gebruikersnaam al geëxtraheerd uit een vorige-in met de `preferred_username` claimen. |


Op dit moment wordt de gebruiker gevraagd hun referenties invoeren en de verificatie te voltooien.

### <a name="sample-response"></a>Reactie van monster

Een antwoord monster kan nadat de gebruiker is geverifieerd, er zo uit:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| id_token | De `id_token` die de app heeft aangevraagd. U kunt de `id_token` om te controleren of de identiteit van de gebruiker en het begin van een sessie met de gebruiker.  |
| staat | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het [voorkomen van aanvallen van cross-site request vervalsing](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |

### <a name="error-response"></a>Fout antwoord
Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes voor autorisatie eindpunt fouten

De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` -parameter van de foutmelding.

| Foutcode | Beschrijving | Actie op de client |
|------------|-------------|---------------|
| invalid_request | Protocolfout, zoals een ontbrekende vereiste parameter. | Herstel en de aanvraag opnieuw indienen. Dit is een ontwikkeling fout wordt meestal tegengehouden tijdens de eerste test.|
| unauthorized_client | De clienttoepassing is niet toegestaan voor het aanvragen van een vergunning code. | Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of niet is toegevoegd aan de gebruiker Azure AD huurder. De toepassing kan de gebruiker gevraagd met instructies voor het installeren van de toepassing en toevoegen aan AD Azure. |
| ACCESS_DENIED | Resource eigenaar toestemming geweigerd | De clienttoepassing kan geven dat kan niet worden voortgezet tenzij de gebruiker akkoord gaat. |
| unsupported_response_type | De vergunning-server biedt geen ondersteuning voor het type reactie in de aanvraag. | Herstel en de aanvraag opnieuw indienen. Dit is een ontwikkeling fout wordt meestal tegengehouden tijdens de eerste test.|
|server_error | De server is een onverwachte fout opgetreden. | Probeer de aanvraag opnieuw. Deze fouten kunnen voortvloeien uit de tijdelijke omstandigheden. De clienttoepassing kan worden uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable | De server is tijdelijk bezet om de aanvraag te verwerken. | Probeer de aanvraag opnieuw. De clienttoepassing kan worden uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke situatie. |
| invalid_resource |De doelbron is ongeldig omdat niet bestaat of is niet correct geconfigureerd Azure AD kan deze niet vinden.| Dit betekent dat de resource, indien aanwezig, niet is geconfigureerd in de pachter. De toepassing kan de gebruiker gevraagd met instructies voor het installeren van de toepassing en toevoegen aan AD Azure. |

## <a name="validate-the-idtoken"></a>Valideren van de id_token

NET ontvangen een `id_token` is niet voldoende voor de verificatie van de gebruiker. u moet de handtekening te valideren en controleren of de vorderingen in de `id_token` per eisen van uw app. Het eindpunt Azure AD maakt gebruik van JSON Web Tokens (JWTs) en openbare-sleutelcryptografie ondertekenen van tokens en controleer of deze geldig zijn.

Kunt u kiezen voor het valideren van de `id_token` in client-code, maar gebruikelijk is voor het verzenden van de `id_token` om een back-endserver en er de validatie uit te voeren. Nadat u hebt geverifieerd dat de handtekening van de `id_token`, een paar claims, u moet controleren of er zijn.

U kunt ook extra claims, afhankelijk van het scenario te valideren. Sommige algemene validaties omvatten:

- Ervoor te zorgen dat de gebruikersorganisatie heeft aangemeld bij de app.
- Ervoor te zorgen dat de gebruiker heeft goede toestemming/rechten
- Ervoor te zorgen dat een bepaalde mate van verificatie heeft plaatsgevonden, zoals een meerledige verificatie.

Zodra u hebt volledig gevalideerd de `id_token`, u kunt beginnen met een sessie van de gebruiker en de vorderingen in de `id_token` voor informatie over de gebruiker in uw app. Deze informatie kan worden gebruikt voor het weergeven, records, vergunningen, enz. Lees voor meer informatie over de typen token en claims [Token ondersteund en typen](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Een bord aanvraag verzenden

Wanneer u zich aanmelden van de gebruiker van de app wilt, is het niet voldoende om te wissen van uw app cookies of anderszins einde de sessie met de gebruiker.  Ook moet u de gebruiker omleiden de `end_session_endpoint` voor afmelden.  Als u dit niet doet, wordt de gebruiker opnieuw verifiëren bij uw app zonder hun referenties opnieuw in te voeren omdat ze een geldige-on sessie met het eindpunt Azure AD worden.

U kunt gewoon de gebruiker omleiden de `end_session_endpoint` vermeld in het document verbinden OpenID metagegevens:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | aanbevolen | De URL die door de gebruiker worden omgeleid naar na succesvolle logout.  Als dat niet is opgenomen, wordt de gebruiker een algemeen bericht weergegeven.  |

## <a name="token-acquisition"></a>Token verkrijgen

Veel web apps moeten niet alleen de gebruiker zich aanmeldt, maar ook toegang tot een webservice die namens deze gebruiker gebruik OAuth. In dit scenario verbinding OpenID combineert voor verificatie bij het inlezen van tegelijkertijd een `authorization_code` die kunnen worden gebruikt om `access_tokens` met OAuth vergunning Code stromen.

## <a name="get-access-tokens"></a>Toegangstokens ophalen

Toegangstokens voltooien, moet u voor het wijzigen van de verzoek-in van boven:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Door machtiging bereiken met inbegrip van de aanvraag en het gebruik van `response_type=code+id_token`, de `authorize` eindpunt zorgt ervoor dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` parameter query te retourneren en uw app autorisatiecode in te wisselen voor een toegangstoken.

### <a name="successful-response"></a>Positief antwoord

Een positief antwoord met `response_mode=form_post` eruit komt te zien:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| id_token | De `id_token` die de app heeft aangevraagd. U kunt de `id_token` om te controleren of de identiteit van de gebruiker en het begin van een sessie met de gebruiker. |
| code | De authorization_code die de app heeft aangevraagd. De app kunt de code van de vergunning voor het aanvragen van een toegangstoken voor de doelbron gebruiken. Authorization_codes zijn zeer korte levensduur, meestal ze verlopen na ongeveer 10 minuten. |
| staat | Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |

### <a name="error-response"></a>Fout antwoord

Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |

Zie voor een beschrijving van de mogelijke foutcodes en hun maatregelen aanbevolen client [foutcodes voor autorisatie eindpunt fouten](#error-codes-for-authorization-endpoint-errors).

Als u toegang hebt verkregen van een vergunning `code` en een `id_token`, kunt u de gebruiker aanmelden en access tokens krijgen voor hun rekening.  Ondertekenen van de gebruiker in, moet u controleren de `id_token` precies zoals hierboven beschreven. Als u access tokens, kunt u de stappen beschreven in onze [documentatie OAuth-protocol](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token).
