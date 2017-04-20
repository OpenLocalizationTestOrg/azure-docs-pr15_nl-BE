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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0 vergunning code stroom

De subsidie OAuth 2.0 vergunning code kan worden gebruikt in toepassingen die zijn geïnstalleerd op een apparaat dat toegang te krijgen tot beveiligde bronnen, zoals web-API's. U kunt via de B2C Azure Active Directory (AD Azure) uitvoering van OAuth 2.0 identiteit aanmelding aanmelden en andere beheertaken toevoegen aan uw mobiele en desktop apps. Deze handleiding is taalonafhankelijk. Dit wordt beschreven hoe u berichten verzenden en ontvangen HTTP-zonder gebruik van een van onze open source-bibliotheken.

<!-- TODO: Need link to libraries -->

De stroom OAuth 2.0 autorisatie-code is beschreven in [punt 4.1 van de OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749). U kunt het uitvoeren van verificatie en autorisatie in de meeste soorten app, met inbegrip van [web apps](active-directory-b2c-apps.md#web-apps) en [apps standaard geïnstalleerd](active-directory-b2c-apps.md#mobile-and-native-apps). U kunt apps veilig **access_tokens**, die kan worden gebruikt voor toegang tot bronnen die zijn beveiligd door een [vergunning server](active-directory-b2c-reference-protocols.md#the-basics)verkrijgen.

Deze handleiding zal zich richten op een bepaalde versie 2.0 OAuth vergunning code stroom--**openbare clients**. Een openbare-client is een clienttoepassing die niet kan vertrouwd worden voor het veilig onderhouden van de integriteit van een geheime vraag. Dit omvat mobiele apps, desktop apps en vrijwel elke toepassing die wordt uitgevoerd op een apparaat en moet verkrijgen van access_tokens. Als u met Azure AD B2C identiteitsbeheer toevoegen aan een web app wilt, moet u [Verbinding OpenID](active-directory-b2c-reference-oidc.md) in plaats van OAuth 2.0.

Azure AD B2C breidt de standaard die OAuth 2.0 loopt doet meer dan eenvoudige verificatie en machtiging. Het geeft de [**parameter van het beleid**](active-directory-b2c-reference-policies.md), waarmee u gebruik OAuth 2.0 gebruikerservaringen voor uw app, zoals aanmelding aanmelden en beheer van gebruikersprofielen. Hier laten we zien hoe elk van deze ervaringen in uw eigen toepassingen te implementeren met OAuth 2.0 en beleid. Ook laten we zien hoe u de access_tokens voor toegang tot web-API's.

Het onderstaande voorbeeld HTTP-aanvragen gebruikt onze monster B2C-directory, **fabrikamb2c.onmicrosoft.com**, evenals onze voorbeeldtoepassing en beleid. U bent vrij om te proberen de verzoeken van uzelf met behulp van deze waarden, of kunt u deze vervangen door uw eigen.
Meer informatie over hoe [u uw eigen directory B2C, toepassing, en beleid](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. een autorisatiecode ophalen
De stroom van autorisatie code begint met de client om de gebruiker de `/authorize` eindpunt. Dit is het interactieve deel van de stroom, waar de gebruiker zal daadwerkelijk actie ondernemen. In deze aanvraag, de client geeft aan dat de machtigingen die nodig zijn voor het verwerven van de gebruiker in de `scope` parameter en het beleid uit te voeren de `p` parameter. Drie voorbeelden zijn hieronder (met regeleinden voor leesbaarheid), elk met een ander beleid.

#### <a name="use-a-sign-in-policy"></a>Een beleid-in gebruik

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Een beleid aanmelding gebruiken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Gebruik een beleid profiel bewerken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Vereist | De toepassings-ID die de [Azure portal](https://portal.azure.com) die is toegewezen aan uw app. |
| response_type | Vereist | Het type reactie waarin `code` voor de autorisatie code stroom. |
| redirect_uri | Vereist | De redirect_uri van uw app, waarbij antwoorden verificatie kunnen worden verzonden en ontvangen door uw app. Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, met dien verstande dat het URL-gecodeerd moet zijn. |
| toepassingsgebied | Vereist | Door spaties gescheiden lijst met bereiken. Een waarde van één scope geeft Azure AD beide de machtigingen die worden aangevraagd. De client-ID gebruikt als de scope geeft aan dat de app moet een **toegangstoken** dat kan worden gebruikt voor uw eigen service of web API, vertegenwoordigd door hetzelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een **refresh_token** voor een lange levensduur van de toegang tot bronnen moet.  U kunt ook de `openid` bereik voor het aanvragen van een **id_token** van Azure AD B2C. |
| response_mode | Aanbevolen | De methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar uw app. Het kan zijn dat een 'query', 'form_post' of 'fragment'.
| staat | Aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks met de gewenste inhoud kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het voorkomen van aanvallen van cross-site request vervalsing. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld de pagina op of het beleid wordt uitgevoerd. |
| p | Vereist | Het beleid dat wordt uitgevoerd. Dit is de naam van een beleid dat wordt gemaakt in de B2C-map. De waarde voor de naam moet beginnen met "b2c\_1\_". Meer informatie over het beleid in [Extensible beleidskader](active-directory-b2c-reference-policies.md). |
| prompt | Optioneel | Het type gebruikersinteractie is vereist. De enige geldige waarde op dit moment is 'aanmelding', waardoor de gebruikers hun referenties invoeren op die aanvraag. Eenmalige aanmelding doorgevoerd niet. |

Op dit moment wordt de gebruiker gevraagd om het beleid van de werkstroom te voltooien. Hierbij kan de gebruiker invoeren van hun gebruikersnaam en wachtwoord aanmelden met een sociale identiteit van de map of een ander aantal stappen uit, afhankelijk van hoe het beleid wordt gedefinieerd.

Nadat de gebruiker is voltooid voor het beleid, Azure AD resulteert in een reactie op uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven de `response_mode` parameter. Het antwoord is precies hetzelfde voor elk van deze gevallen, onafhankelijk van het beleid dat is uitgevoerd.

Een positief antwoord met `response_mode=query` eruit komt te zien:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| code | De authorization_code die de app heeft aangevraagd. De app kunt de code van de vergunning voor het aanvragen van een access_token voor een resource doel gebruiken. Authorization_codes zijn zeer korte levensduur. Normaal, ze verlopen na ongeveer 10 minuten. |
| staat | Raadpleeg de volledige beschrijving in de vorige tabel. Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |

Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` , zodat de app deze op de juiste wijze kan verwerken:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van de soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar voor het identificeren van de hoofdoorzaak van een verificatiefout. |
| staat | Raadpleeg de volledige beschrijving van de eerste tabel in deze sectie. Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |


## <a name="2-get-a-token"></a>2. een token ophalen
Nu dat u een authorization_code hebt aangeschaft, kunt u inwisselen de `code` voor een token voor de gewenste resource door het verzenden van een `POST` vragen aan de `/token` eindpunt. In Azure AD B2C is de enige resource die u kunt aanvragen een token voor uw app eigen back-end web API. De overeenkomst die wordt gebruikt voor het aanvragen van een token aan uzelf is het gebruik van uw app in de client-ID als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| p | Vereist | Het beleid dat is gebruikt voor de verwerving van de code van de vergunning. U kunt een ander beleid in deze aanvraag niet gebruiken. Houd er rekening mee dat u deze parameter aan de *query-tekenreeks*, niet in de hoofdtekst van het bericht toevoegen. |
| client_id | Vereist | De toepassings-ID die de [Azure portal](https://portal.azure.com) die is toegewezen aan uw app. |
| grant_type | Vereist | Het type van de subsidie moet worden `authorization_code` voor de autorisatie code stroom. |
| toepassingsgebied | Reccommended | Door spaties gescheiden lijst met bereiken. Een waarde van één scope geeft Azure AD beide de machtigingen die worden aangevraagd. De client-ID gebruikt als de scope geeft aan dat de app moet een **toegangstoken** dat kan worden gebruikt voor uw eigen service of web API, vertegenwoordigd door hetzelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een **refresh_token** voor een lange levensduur van de toegang tot bronnen moet.  U kunt ook de `openid` bereik voor het aanvragen van een **id_token** van Azure AD B2C. |
| code | Vereist | De authorization_code die u hebt verkregen in de eerste zijde van de stroom. |
| redirect_uri | Vereist | De redirect_uri van de toepassing waarin u de authorization_code hebt ontvangen. |

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
| access_token | Ondertekende JSON Web Token (JWT)-token die u hebt aangevraagd. |
| toepassingsgebied | De scopes die het token is geldig voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd die het token geldig is (in seconden is). |
| refresh_token | Een refresh_token OAuth 2.0. De app kunt deze token verkrijgen van extra tokens na het verstrijken van het huidige token. Refresh_tokens worden langer bewaard moeten blijven en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd bewaren. Raadpleeg de [token B2C-verwijzing](active-directory-b2c-reference-tokens.md)voor meer details. |

Fout bij antwoorden eruit:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van de soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar voor het identificeren van de hoofdoorzaak van een verificatiefout. |

## <a name="3-use-the-token"></a>3. Gebruik de token
Nu dat u hebt gekregen een `access_token`, kunt u de token aanvragen voor uw back-end web-API's door te nemen in de `Authorization` kop:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. het token vernieuwen
Toegang & Id tokens zijn korte levensduur. U moet ze vervallen om door te gaan om toegang tot bronnen te kunnen vernieuwen. U kunt dit doen door het indienen van een andere `POST` vragen aan de `/token` eindpunt. Momenteel bieden de `refresh_token` in plaats van de `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | -------- |
| p | Vereist | Het beleid dat is gebruikt voor de verwerving van de oorspronkelijke refresh_token. U kunt een ander beleid in deze aanvraag niet gebruiken. Houd er rekening mee dat u deze parameter aan de *query-tekenreeks*, niet in de hoofdtekst van het bericht toevoegen. |
| client_id | Aanbevolen | De toepassings-ID die de [Azure portal](https://portal.azure.com) die is toegewezen aan uw app. |
| grant_type | Vereist | Het type van de subsidie moet worden `refresh_token` voor deze zijde van de autorisatie code stroom. |
| toepassingsgebied | Aanbevolen | Door spaties gescheiden lijst met bereiken. Een waarde van één scope geeft Azure AD beide de machtigingen die worden aangevraagd. De client-ID gebruikt als de scope geeft aan dat de app moet een **toegangstoken** dat kan worden gebruikt voor uw eigen service of web API, vertegenwoordigd door hetzelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een **refresh_token** voor een lange levensduur van de toegang tot bronnen moet.  U kunt ook de `openid` bereik voor het aanvragen van een **id_token** van Azure AD B2C. |
| redirect_uri | Optioneel | De redirect_uri van de toepassing waarin u de authorization_code hebt ontvangen. |
| refresh_token | Vereist | De oorspronkelijke refresh_token die u hebt verkregen in de tweede zijde van de stroom. |

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
| access_token | Ondertekende JSON Web Token (JWT)-token die u hebt aangevraagd. |
| toepassingsgebied | De scopes die het token is geldig voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd die het token geldig is (in seconden is). |
| refresh_token | Een refresh_token OAuth 2.0. De app kunt deze token verkrijgen van extra tokens na het verstrijken van het huidige token. Refresh_tokens worden langer bewaard moeten blijven en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd bewaren. Raadpleeg de [token B2C-verwijzing](active-directory-b2c-reference-tokens.md)voor meer details. |

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

## <a name="use-your-own-b2c-directory"></a>Gebruik uw eigen directory B2C

Als u proberen deze aanvragen voor uzelf wilt, moet u eerst deze drie stappen uitvoeren en vervolgens de voorbeeldwaarden hierboven vervangen door uw eigen:

- [Maken een B2C-map](active-directory-b2c-get-started.md) en gebruik de naam van de map in de aanvragen.
- [Een toepassing maken](active-directory-b2c-app-registration.md) om een toepassings-ID en een redirect_uri te verkrijgen. U wilt een **native client** in uw toepassing opnemen.
- [Uw beleid maken](active-directory-b2c-reference-policies.md) voor de namen van uw beleid.
