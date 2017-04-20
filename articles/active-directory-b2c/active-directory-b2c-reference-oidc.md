<properties
    pageTitle="B2C Azure Active Directory | Microsoft Azure"
    description="Het bouwen van webtoepassingen met behulp van de Azure Active Directory-implementatie van het verificatieprotocol OpenID verbinden."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web aanmelden met OpenID verbinding

OpenID-verbinding is een protocol voor verificatie, gebaseerd op OAuth 2.0, die kan worden gebruikt voor het veilig melden gebruikers in webtoepassingen.  Uitbesteden van aanmelding, aanmelden via de B2C Azure Active Directory (AD Azure) uitvoering van OpenID Connect en andere identiteitsbeheer ervaringen in uw webtoepassingen naar Azure AD. Deze handleiding wordt beschreven hoe u te doen op een taal-onafhankelijke manier. Dit wordt beschreven hoe te verzenden en ontvangen van HTTP-berichten zonder gebruik van een van onze open source-bibliotheken.

[OpenID verbinding](http://openid.net/specs/openid-connect-core-1_0.html) breidt het protocol OAuth 2.0 *vergunning* voor gebruik als *een verificatieprotocol* . Hiermee kunt u eenmalige aanmelding via OAuth uitvoeren. Het geeft het concept van een `id_token`, die een beveiligingstoken dat kan de client de identiteit van de gebruiker en basisprofiel informatie opvragen over de gebruiker is.

Omdat dit OAuth 2.0 doorloopt, kunt u eveneens apps veilig **access_tokens**verkrijgen. U kunt de access_tokens voor toegang tot bronnen die zijn beveiligd door een [vergunning server](active-directory-b2c-reference-protocols.md#the-basics)gebruiken. Wij raden aan OpenID verbinding maken als u een webtoepassing die is ondergebracht op een server en is toegankelijk via een browser maakt. Als u met Azure AD B2C identiteitsbeheer toevoegen aan uw mobiele of desktop-toepassingen wilt, moet u [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) in plaats van OpenID verbinden.

Azure AD B2C breidt het standaardprotocol voor OpenID verbinding doet meer dan eenvoudige verificatie en machtiging. Het geeft de [**parameter van het beleid**](active-directory-b2c-reference-policies.md)kunt u gebruiken verbinding OpenID gebruikerservaringen, zoals toevoegen aan uw app--Inschrijf-in en beheer van gebruikersprofielen. Hier ziet u het gebruik van OpenID verbinding maken en het beleid voor de uitvoering van elk van deze ervaringen in uw webtoepassingen. Ook leert u hoe u de access_tokens voor toegang tot web-API's.

Het onderstaande voorbeeld HTTP-aanvragen worden gebruikt onze monster B2C-directory, **fabrikamb2c.onmicrosoft.com**, evenals onze sample application **https://aadb2cplayground.azurewebsites.net** en beleid. U bent vrij om te proberen de aanvragen zelf met behulp van deze waarden, of kunt u deze vervangen door uw eigen.
Meer informatie over hoe [u uw eigen B2C huurder, toepassing, en het beleid](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Verificatieaanvragen verzenden
Wanneer uw web app moet de gebruiker te verifiëren en een beleid uit te voeren, kan de gebruiker worden doorgestuurd de `/authorize` eindpunt. Dit is het interactieve gedeelte van de flow, waarin de gebruiker zal daadwerkelijk actie ondernemen, afhankelijk van het beleid.

In deze aanvraag, de client geeft aan dat de machtigingen die nodig zijn voor het verwerven van de gebruiker in de `scope` parameter en het beleid uit te voeren de `p` parameter. Drie voorbeelden zijn hieronder (met regeleinden voor leesbaarheid), elk met een ander beleid. Om een idee krijgen hoe elke aanvraag werkt, probeer de aanvraag in een browser te plakken en het uitvoeren.

#### <a name="use-a-sign-in-policy"></a>Een beleid-in gebruik

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Een beleid aanmelding gebruiken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Gebruik een beleid profiel bewerken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Vereist | De toepassings-ID die de [Azure portal](https://portal.azure.com/) die is toegewezen aan uw app. |
| response_type | Vereist | Het type reactie waarin `id_token` voor het verbinden van OpenID. Als uw web app ook tokens moet voor een web-API aanroept, kunt u `code+id_token`, zoals we hier hebben gedaan. |
| redirect_uri | Aanbevolen | De redirect_uri van uw app, waarbij antwoorden verificatie kunnen worden verzonden en ontvangen door uw app. Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, met dien verstande dat het URL-gecodeerd moet zijn. |
| toepassingsgebied | Vereist | Door spaties gescheiden lijst met bereiken. Een waarde van één scope geeft Azure AD beide de machtigingen die worden aangevraagd. De `openid` bereik geeft een machtiging voor de gebruiker zich aanmeldt en de gegevens over de gebruiker in de vorm van **id_tokens** (later meer hierover later in dit artikel). De `offline_access` bereik is optioneel voor web apps. Hiermee wordt aangegeven dat uw app een **refresh_token** voor een lange levensduur van de toegang tot bronnen moet. |
| response_mode | Aanbevolen | De methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar uw app. Het kan zijn dat een 'query', 'form_post' of 'fragment'.  'form_post' wordt aanbevolen voor de beste beveiliging. |
| staat | Aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks met de gewenste inhoud kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het voorkomen van aanvallen van cross-site request vervalsing. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld de pagina op. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag (gegenereerd door de app) die in de resulterende id_token als een claim worden opgenomen. De app kan vervolgens controleren of deze waarde token replay-aanvallen te verminderen. De waarde is meestal een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| p | Vereist | Het beleid dat wordt uitgevoerd. Dit is de naam van een beleid dat is gemaakt in uw huurder B2C. De waarde voor de naam moet beginnen met "b2c\_1\_". Meer informatie over het beleid in [Extensible beleidskader](active-directory-b2c-reference-policies.md). |
| prompt | Optioneel | Het type gebruikersinteractie is vereist. De enige geldige waarde op dit moment is 'aanmelding', waardoor de gebruikers hun referenties invoeren op die aanvraag. Eenmalige aanmelding doorgevoerd niet. |

Op dit moment wordt de gebruiker gevraagd om het beleid van de werkstroom te voltooien. Hierbij kan de gebruiker invoeren van hun gebruikersnaam en wachtwoord aanmelden met een sociale identiteit van de map of een ander aantal stappen uit, afhankelijk van hoe het beleid wordt gedefinieerd.

Nadat de gebruiker is voltooid voor het beleid, Azure AD resulteert in een reactie op uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven in de `response_mode` parameter. Het antwoord is precies hetzelfde voor elk van deze gevallen, onafhankelijk van het beleid dat is uitgevoerd.

Een positief antwoord met `response_mode=fragment` er als volgt uitzien:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| id_token | De id_token die de app heeft aangevraagd. De id_token kunt u controleren of de identiteit van de gebruiker en het begin van een sessie met de gebruiker. Meer informatie over id_tokens en de inhoud ervan worden opgenomen in de [token Azure AD B2C-verwijzing](active-directory-b2c-reference-tokens.md). |
| code | De authorization_code die de app heeft aangevraagd, als u `response_type=code+id_token`. De app kunt de code van de vergunning voor het aanvragen van een access_token voor een resource doel gebruiken. Authorization_codes zijn zeer korte levensduur. Normaal, ze verlopen na ongeveer 10 minuten. |
| staat | Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` , zodat de app deze op de juiste wijze kan verwerken:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren. |
| staat | Raadpleeg de volledige beschrijving in de vorige tabel. Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |


## <a name="validate-the-idtoken"></a>Valideren van de id_token
Ontvangen alleen een id_token is niet voldoende voor de verificatie van de gebruiker, moet u controleren of de vorderingen in het token aan de hand van de vereisten van uw app en handtekening van de id_token. Azure AD B2C maakt gebruik van [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en openbare-sleutelcryptografie ondertekenen van tokens en controleer of deze geldig zijn.

Er zijn veel open source-bibliotheken die beschikbaar zijn voor het valideren van JWTs, afhankelijk van de taal van voorkeur. Het is raadzaam deze opties verkennen dan uw eigen validatielogica te implementeren. De informatie hier is handig in hoe goed die bibliotheken gebruiken.

Azure AD B2C heeft een eindpunt verbinding OpenID metagegevens waarmee een app voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat de eindpunten, inhoud van tokens en token-ondertekening van sleutels. Er is een document JSON metagegevens voor elk beleid in de B2C-huurder. Bijvoorbeeld de metagegevensdocument van de `b2c_1_sign_in` beleid in de `fabrikamb2c.onmicrosoft.com` bevindt zich op:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van het document van deze configuratie is de `jwks_uri`, waarvan de waarde voor hetzelfde beleid zou zijn:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Om te bepalen welk beleid is gebruikt voor het ondertekenen van een id_token (en waar voor het ophalen van de metagegevens van), hebt u twee opties. Eerst de naam van het beleid is opgenomen in de `acr` in de id_token claimen. Zie voor informatie over het verdelen van de claims van een id_token, de [token Azure AD B2C-verwijzing](active-directory-b2c-reference-tokens.md). De andere optie is voor het coderen van het beleid in de waarde van de `state` parameter wanneer u het verzoek te verzenden, en vervolgens decoderen om te bepalen welk beleid is gebruikt. Beide methoden is perfect geldig.

Nadat u de metagegevensdocument van het eindpunt van de metagegevens OpenID verbinding hebt aangeschaft, kunt u de openbare 256 RSA-sleutels (die zich op dit eindpunt) voor het valideren van de handtekening van de id_token. Kunnen er meerdere sleutels die aan dit eindpunt op elk gewenst moment in de tijd, te herkennen aan een `kid`. De kop van de id_token bevat ook een `kid` aanspraak maken, waarmee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van de id_token. Zie voor meer informatie, met inbegrip van de secties op [tokens te valideren](active-directory-b2c-reference-tokens.md#validating-tokens) en [belangrijke informatie over het ondertekenen van sleutels rollover](active-directory-b2c-reference-tokens.md#validating-tokens) [Azure AD B2C token verwijzing](active-directory-b2c-reference-tokens.md) .
<!--TODO: Improve the information on this-->

Nadat u hebt geverifieerd dat de handtekening van de id_token, zijn er verschillende claims die u controleren moet, bijvoorbeeld:

- U moet controleren de `nonce` claimen token replay-aanvallen te voorkomen. De waarde moet worden opgegeven in de aanvraag voor aanmelden.
- U moet controleren de `aud` claimen om ervoor te zorgen dat de id_token voor uw app werd uitgegeven. De waarde moet de toepassings-ID van uw app.
- U moet controleren de `iat` en `exp` vorderingen om ervoor te zorgen dat de id_token niet is verlopen.

Er zijn verschillende meer validaties die u uitvoeren moet, in de [OpenID verbinding Core Spec](http://openid.net/specs/openid-connect-core-1_0.html)in detail beschreven.  U kunt ook extra claims, afhankelijk van het scenario te valideren. Sommige algemene validaties omvatten:

- Ervoor te zorgen dat de gebruikersorganisatie heeft aangemeld voor de app.
- Ervoor te zorgen dat de gebruiker de juiste vergunning/bevoegdheden heeft.
- Ervoor te zorgen dat een bepaalde mate van verificatie heeft plaatsgevonden, zoals Azure meerledige verificatie.

Zie voor meer informatie over de vorderingen in een id_token, de [token Azure AD B2C-verwijzing](active-directory-b2c-reference-tokens.md).

Nadat u hebt de id_token volledig gevalideerd, kunt u een sessie met de gebruiker begint en de vorderingen in de id_token gebruiken voor informatie over de gebruiker in uw app. Deze informatie kan worden gebruikt voor het weergeven, records en vergunning.

## <a name="get-a-token"></a>Een token ophalen
Als uw web app moet gebeuren is dat alle beleid uitvoeren, kunt u in de volgende secties overslaan. Deze secties zijn alleen van toepassing op web apps die nodig zijn om een web API-aanroepen geverifieerd en worden ook beschermd door Azure AD B2C.

Kan je de authorization_code die u hebt aangeschaft (met behulp van `response_type=code+id_token`) voor een token voor de gewenste resource door het verzenden van een `POST` aanvragen voor de `/token` eindpunt. Momenteel is de enige resource die u kunt aanvragen een token voor uw app eigen back-end web API. De overeenkomst voor het aanvragen van een token aan uzelf is het gebruik van uw app in de client-ID als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| p | Vereist | Het beleid dat is gebruikt voor de verwerving van de code van de vergunning. U kunt een ander beleid in deze aanvraag niet gebruiken. Houd er rekening mee dat u deze parameter aan de **query-tekenreeks**, niet in de hoofdtekst van het bericht toevoegen. |
| client_id | Vereist | De toepassings-ID die de [Azure portal](https://portal.azure.com/) die is toegewezen aan uw app. |
| grant_type | Vereist | Het type van de subsidie moet worden `authorization_code` voor de autorisatie code stroom. |
| toepassingsgebied | Aanbevolen | Door spaties gescheiden lijst met bereiken. Een waarde van één scope geeft Azure AD beide de machtigingen die worden aangevraagd. De `openid` bereik geeft een machtiging voor de gebruiker zich aanmeldt en de gegevens over de gebruiker in de vorm van **id_tokens**. Het kan worden gebruikt om tokens op uw app eigen back-end web API die wordt vertegenwoordigd door dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw app een **refresh_token** voor een lange levensduur van de toegang tot bronnen moet. |
| code | Vereist | De authorization_code die u hebt verkregen in de eerste zijde van de stroom. |
| redirect_uri | Vereist | De redirect_uri van de toepassing waarin u de authorization_code hebt ontvangen. |
| client_secret | Vereist | Het geheim van toepassing die u hebt gegenereerd in de [portal Azure](https://portal.azure.com/). Het geheim van deze toepassing is een belangrijke artefact. U moet het veilig opslaan op de server. U moet ook zorgen deze geheim van de client op een periodieke basis draaien. |

Een positief antwoord token wordt er als volgt uitzien:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| not_before | Het tijdstip waarop het token wordt als geldig beschouwd, tijdig epoch. |
| token_type | De waarde van het type token. Het enige type dat AD Azure ondersteunt is aan toonder. |
| access_token | De ondertekende JWT token die u hebt aangevraagd. |
| toepassingsgebied | De scopes die het token is geldig voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd dat de access_token geldig is (in seconden). |
| refresh_token | Een refresh_token OAuth 2.0. De app kunt deze token verkrijgen van extra tokens na het verstrijken van het huidige token.  Refresh_tokens worden langer bewaard moeten blijven en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd bewaren. Raadpleeg de [token B2C-verwijzing](active-directory-b2c-reference-tokens.md)voor meer details. Opmerking moet hebben die u het bereik `offline_access` in de vergunning en de token aanvragen ontvangen van een refresh_token. |

Fout bij antwoorden eruit:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren. |

## <a name="use-the-token"></a>Gebruik het token
Nu dat u hebt gekregen een `access_token`, kunt u de token aanvragen voor uw back-end web-API's door te nemen in de `Authorization` kop:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Het token vernieuwen
De id_tokens zijn korte levensduur. U moet ze vervallen om door te gaan om toegang tot bronnen te kunnen vernieuwen. U kunt dit doen door het indienen van een andere `POST` vragen aan de `/token` eindpunt. Momenteel bieden de `refresh_token` in plaats van de `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Beschrijving |
| ----------------------- | ------------------------------- | -------- |
| p | Vereist | Het beleid dat is gebruikt voor de verwerving van de oorspronkelijke refresh_token. U kunt een ander beleid in deze aanvraag niet gebruiken. Houd er rekening mee dat u deze parameter aan de **query-tekenreeks**, niet in de hoofdtekst van het bericht toevoegen. |
| client_id | Vereist | De toepassings-ID die de [Azure portal](https://portal.azure.com/) die is toegewezen aan uw app. |
| grant_type | Vereist | Het type van de subsidie moet worden `refresh_token` voor deze zijde van de autorisatie code stroom. |
| toepassingsgebied | Aanbevolen | Door spaties gescheiden lijst met bereiken. Een waarde van één scope geeft Azure AD beide de machtigingen die worden aangevraagd. De `openid` bereik geeft een machtiging voor de gebruiker zich aanmeldt en de gegevens over de gebruiker in de vorm van **id_tokens**. Het kan worden gebruikt om tokens op uw app eigen back-end web API die wordt vertegenwoordigd door dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw app een **refresh_token** voor een lange levensduur van de toegang tot bronnen moet. |
| redirect_uri | Aanbevolen | De redirect_uri van de toepassing waarin u de authorization_code hebt ontvangen. |
| refresh_token | Vereist | De oorspronkelijke refresh_token die u hebt verkregen in de tweede zijde van de stroom. Opmerking moet hebben die u het bereik `offline_access` in de vergunning en de token aanvragen ontvangen van een refresh_token. |
| client_secret | Vereist | Het geheim van toepassing die u hebt gegenereerd in de [portal Azure](https://portal.azure.com/). Het geheim van deze toepassing is een belangrijke artefact. U moet het veilig opslaan op de server. U moet ook zorgen deze geheim van de client op een periodieke basis draaien. |

Een positief antwoord token wordt er als volgt uitzien:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| not_before | Het tijdstip waarop het token wordt als geldig beschouwd, tijdig epoch. |
| token_type | De waarde van het type token. Het enige type dat AD Azure ondersteunt is aan toonder. |
| access_token | De ondertekende JWT token die u hebt aangevraagd. |
| toepassingsgebied | De scopes die het token is geldig voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd dat de access_token geldig is (in seconden). |
| refresh_token | Een refresh_token OAuth 2.0. De app kunt deze token verkrijgen van extra tokens na het verstrijken van het huidige token.  Refresh_tokens worden langer bewaard moeten blijven en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd bewaren. Raadpleeg de [token B2C-verwijzing](active-directory-b2c-reference-tokens.md)voor meer details. |

Fout bij antwoorden eruit:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren. |


## <a name="send-a-sign-out-request"></a>Afmelden aanvragen

Als u wilt dat de gebruiker van de toepassing ondertekenen, is het niet voldoende om te wissen van uw app cookies of anderszins einde de sessie met de gebruiker. Ook moet u de gebruiker omleiden naar Azure AD af te melden. Als u dit niet doet, kan de gebruiker mogelijk om andere referenties voor uw app zonder hun referenties opnieuw in te voeren. Dit komt omdat er een geldige single sign-on sessie met Azure Active Directory.

U kunt gewoon de gebruiker omleiden de `end_session_endpoint` die in hetzelfde OpenID verbinding metagegevensdocument dat wordt beschreven in de sectie 'De id_token valideren' wordt weergegeven:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| p | Vereist | Het beleid dat u gebruiken wilt voor het ondertekenen van de gebruiker uit uw toepassing. |
| post_logout_redirect_uri | Aanbevolen | De URL die door de gebruiker worden omgeleid naar na succesvolle afmelden. Als het niet opgenomen is, wordt de gebruiker weergegeven een algemeen bericht door Azure AD B2C.  |

> [AZURE.NOTE]
    Terwijl de gebruiker sturen de `end_session_endpoint` wordt gewist van de status van de gebruiker één aanmelding met Azure AD B2C, ondertekent de gebruiker van de sociale identiteit provider (IDP) sessie geen. Als de gebruiker de dezelfde IDP tijdens een latere aanmelden selecteert, zullen ze worden geverifieerd, zonder hun referenties in te voeren. Als een gebruiker moet zich afmelden bij uw toepassing B2C, is dit niet noodzakelijkerwijs dat ze willen volledig afmelden bij hun Facebook-account. Echter voor lokale accounts wordt de sessie beëindigd goed.

## <a name="use-your-own-b2c-tenant"></a>Gebruik uw eigen huurder B2C

Als u proberen deze aanvragen voor uzelf wilt, moet u eerst deze drie stappen uitvoeren en vervolgens de voorbeeldwaarden hierboven vervangen door uw eigen:

- [Een huurder B2C maken](active-directory-b2c-get-started.md)en gebruik de naam van de huurder in de aanvragen.
- [Een toepassing maken](active-directory-b2c-app-registration.md) om een toepassings-ID en een redirect_uri te verkrijgen. U wilt opnemen een **web app/web api** in uw app en, optioneel, een **geheim toepassing**maken.
- [Uw beleid maken](active-directory-b2c-reference-policies.md) voor de namen van uw beleid.
