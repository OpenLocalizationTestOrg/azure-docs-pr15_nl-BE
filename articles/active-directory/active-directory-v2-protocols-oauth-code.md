

<properties
    pageTitle="Azure AD v2.0 OAuth vergunning Code stroom | Microsoft Azure"
    description="Bouw webtoepassingen met Azure AD uitvoering van het verificatieprotocol OAuth 2.0."
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>Protocollen v2.0 - OAuth 2.0 autorisatiecode Flow

De subsidie OAuth 2.0 vergunning code kan worden gebruikt in toepassingen die zijn geïnstalleerd op een apparaat dat toegang te krijgen tot beveiligde bronnen, zoals web-API's.  Met de app model v2.0 uitvoering van OAuth 2.0, kunt u inloggen en API-toegang tot uw mobiele en desktop apps toevoegen.  Deze handleiding is taalonafhankelijk en wordt beschreven hoe u berichten verzenden en ontvangen HTTP-zonder gebruik van een van onze open source-bibliotheken.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

In wordt de stroom OAuth 2.0 vergunning code beschreven in [punt 4.1 van de OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749).  Het wordt gebruikt voor het uitvoeren van verificatie en autorisatie in de meeste soorten app, met inbegrip van [web apps](active-directory-v2-flows.md#web-apps) en [apps standaard geïnstalleerd](active-directory-v2-flows.md#mobile-and-native-apps).  U kunt apps access_tokens die kunnen worden gebruikt voor toegang tot bronnen die zijn beveiligd met behulp van het eindpunt v2.0 veilig te verkrijgen.  

## <a name="protocol-diagram"></a>Protocol-diagram
Op een hoog niveau, de volledige verificatie stroom voor een native/mobiele toepassing iets zien er zo uit:

![OAuth autorisatiecode stroom](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>De code van een vergunning aanvragen
De stroom van autorisatie code begint met de client om de gebruiker de `/authorize` eindpunt.  In dit verzoek geeft de client aan het verwerven van de gebruiker moet machtigingen:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Klik op de onderstaande koppeling voor het uitvoeren van dit verzoek. Na de ondertekening uw browser worden omgeleid naar `https://localhost/myapp/` met een `code` in de adresbalk.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| huurder | Vereist | De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's pachters.  Zie voor meer informatie [Basisbeginselen protocol](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| response_type | Vereist | Omvat `code` voor de autorisatie code stroom. |
| redirect_uri | aanbevolen | De redirect_uri van uw app, waarbij antwoorden verificatie kunnen worden verzonden en ontvangen door uw app.  Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, behalve het url-gecodeerd moet zijn.  Voor native & mobiele toepassingen, moet u de standaardwaarde van `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| toepassingsgebied | Vereist | Een door spaties gescheiden lijst met [scopes](active-directory-v2-scopes.md) die u wilt dat de gebruiker toe te staan.  |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Kan `query` of `form_post`.  |
| staat | aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd.  Een tekenreeks met de inhoud die u kan zijn.  Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het [voorkomen van aanvallen van cross-site request vervalsing](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |
| prompt | optioneel | Geeft het type van gebruikersinteractie is vereist.  De enige geldige waarden op dit moment zijn de 'aanmelden', 'geen' en 'toestemming'.  `prompt=login`Forceert de gebruikers hun referenties invoeren op dat verzoek negatief maken van eenmalige aanmelding.  `prompt=none`is het tegenovergestelde - zij zal erop toezien dat de gebruiker niet wordt weergegeven met welke vragen.  Het eindpunt v2.0 retourneert een fout als de aanvraag kan niet worden voltooid zonder via eenmalige aanmelding.  `prompt=consent`het dialoogvenster OAuth toestemming zal worden geactiveerd nadat de gebruiker zich aanmeldt, vraagt de gebruiker machtigen om de app. |
| login_hint | optioneel | Kan worden gebruikt voor het vooraf invullen van het veld gebruikersnaam/e-mailadres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tevoren.  Vaak apps Gebruik deze parameter tijdens vervolgverificatie dat de gebruikersnaam al geëxtraheerd uit een vorige-in met de `preferred_username` claimen. |
| domain_hint | optioneel | Een van `consumers` of `organizations`.  Als opgenomen, wordt deze overgeslagen op basis van een e-mailbericht detectieproces van de gebruiker wordt doorlopen op de pagina, v2.0 inloggen leidt tot een iets meer gestroomlijnde gebruikersinterface.  Vaak apps met deze parameter wordt gebruikt tijdens een nieuwe verificatie door uit te pakken de `tid` van een vorige aanmelden.  Als de `tid` claimen waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Op dit moment wordt de gebruiker gevraagd hun referenties invoeren en de verificatie te voltooien.  Het eindpunt v2.0 wordt er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter.  Als de gebruiker niet heeft ingestemd met een van deze machtigingen, het aan de gebruiker gevraagd om instemming met de vereiste machtigingen beschikt.  Details van [machtigingen, toestemming te geven, en de huurder met meerdere apps die hier](active-directory-v2-scopes.md).

Zodra de gebruiker wordt geverifieerd en toestemming verleent, het eindpunt v2.0 resulteert in een reactie op uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven de `response_mode` parameter.

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord met `response_mode=query` eruit komt te zien:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| code | De authorization_code die de app heeft aangevraagd. De app kunt de code van de vergunning voor het aanvragen van een toegangstoken voor de doelbron gebruiken.  Authorization_codes zijn zeer korte levensduur, meestal ze verlopen na ongeveer 10 minuten. |
| staat | Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. De app moet controleren of de waarden staat in de aanvraag en het antwoord zijn identiek. |

#### <a name="error-response"></a>Fout antwoord
Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
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

## <a name="request-an-access-token"></a>Een toegangstoken aanvragen
Nu dat u een authorization_code hebt aangeschaft en zijn gemachtigd door de gebruiker, kunt u inwisselen de `code` voor een `access_token` naar de gewenste bron, door het verzenden van een `POST` wil de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Probeer deze aanvraag wordt uitgevoerd in de Postman! (Vergeet niet te vervangen door de `code`)     [ ![Uitvoeren in de Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| huurder | Vereist | De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's pachters.  Zie voor meer informatie [Basisbeginselen protocol](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| grant_type | Vereist | Moet `authorization_code` voor de autorisatie code stroom. |
| toepassingsgebied | Vereist | Door spaties gescheiden lijst met bereiken.  De scopes in deze poot verzocht moet gelijk aan of een subset van de bereiken in het eerste bEen wordt aangevraagd.  Als de bereiken die in deze aanvraag meerdere bronservers omspannen, wordt het eindpunt v2.0 een token voor de resource die is opgegeven in het eerste bereik retourneren.  Zie [machtigingen, toestemming, en bereiken](active-directory-v2-scopes.md)voor een meer gedetailleerde uitleg van scopes.  |
| code | Vereist | De authorization_code die u hebt verkregen in de eerste zijde van de stroom.   |
| redirect_uri | Vereist | Dezelfde redirect_uri-waarde die is gebruikt voor de verwerving van de authorization_code. |
| client_secret | vereist voor web apps | Het geheim van de toepassing die u voor uw app in de app-portal registratie gemaakt.  Het moet niet worden gebruikt in een native app, omdat client_secrets niet op betrouwbare wijze kunnen worden opgeslagen op apparaten.  Het is vereist voor web apps en web-API's, die de mogelijkheid hebben om de client_secret veilig opslaan op de server. |

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord token wordt er als volgt uitzien:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De aangevraagde toegangstoken. De app kan deze token gebruiken voor verificatie bij de beveiligde bron, zoals een web API. |
| token_type | Geeft de waarde van het type token. Het enige type dat AD Azure ondersteunt is aan toonder  |
| expires_in | Hoe lang het toegangstoken geldig is (in seconden). |
| toepassingsgebied | De scopes die geldt voor de access_token. |
| refresh_token |  Een token van OAuth 2.0 vernieuwen. De app kunt gebruiken deze token verkrijgen van extra toegangstokens na het verstrijken van het huidige toegangstoken.  Refresh_tokens zijn lange levensduur en kan worden gebruikt voor toegang tot bronnen voor langere tijd bewaren.  Raadpleeg de [token verwijzing v2.0](active-directory-v2-tokens.md)voor meer details.  |
| id_token | Een niet-ondertekende JSON Web Token (JWT). De base64Url app kan decoderen de segmenten van deze token informatie opvragen over de gebruiker die heeft ingelogd. De app kan de waarden in de cache en deze weer te geven, maar moet niet vertrouwen op hen voor elke vergunning of beveiligingsgrenzen.  Zie voor meer informatie over id_tokens [v2.0 eindpunt token verwijzing](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Fout antwoord
Fout bij antwoorden eruit:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |
| error_codes | Een lijst van STS specifieke foutcodes die in diagnostische gegevens helpen kunnen.  |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u bij de diagnose helpen kan.  |
| correlation_id | Een unieke id voor de aanvraag die u diagnostische gegevens van alle componenten kunt. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Foutcodes voor token eindpunt fouten

| Foutcode | Beschrijving | Actie op de client |
|------------|-------------|---------------|
| invalid_request | Protocolfout, zoals een ontbrekende vereiste parameter. | Herstellen en de aanvraag opnieuw indienen |
| invalid_grant | De code van de vergunning is ongeldig of is verlopen. | Probeer een nieuwe aanvraag naar de `/authorize` eindpunt |
| unauthorized_client | De geverifieerde client is niet geautoriseerd gebruik van dit type vergunning verlenen. | Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of niet is toegevoegd aan de gebruiker Azure AD huurder. De toepassing kan de gebruiker gevraagd met instructies voor het installeren van de toepassing en toevoegen aan AD Azure. |
| invalid_client | Clientverificatie mislukt. | De referenties van de client zijn niet geldig. Om op te lossen, werkt de beheerder van de toepassing de referenties. |
| unsupported_grant_type | De vergunning-server biedt geen ondersteuning voor het type vergunning verlenen. | Wijzig het subsidietype in de aanvraag. Dit type fout moet plaatsvinden tijdens de ontwikkeling en tijdens de eerste beproeving worden gedetecteerd. |
| invalid_resource | De doelbron is ongeldig omdat niet bestaat of is niet correct geconfigureerd Azure AD kan deze niet vinden. | Dit betekent dat de resource, indien aanwezig, niet is geconfigureerd in de pachter. De toepassing kan de gebruiker gevraagd met instructies voor het installeren van de toepassing en toevoegen aan AD Azure. |
| interaction_required | Het verzoek is gebruikersinteractie vereist. Bijvoorbeeld is een stap extra verificatie vereist. | De aanvraag met dezelfde resource opnieuw. |
| temporarily_unavailable | De server is tijdelijk bezet om de aanvraag te verwerken. | Probeer de aanvraag opnieuw. De clienttoepassing kan worden uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke situatie.|

## <a name="use-the-access-token"></a>Gebruik het toegangstoken
Nu dat u hebt gekregen een `access_token`, kunt u de token aanvragen voor Web-API's door te nemen in de `Authorization` kop:

> [AZURE.TIP] Uitvoeren van dit verzoek in Postman! (Vervangen door de `Authorization` koptekst eerste)     [ ![Uitvoeren in de Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Het toegangstoken vernieuwen
Access_tokens kort zijn gehouden en moet u ze vernieuwen nadat ze zijn verlopen als u wilt doorgaan met het verkrijgen van toegang tot bronnen.  Kunt u doen door het indienen van een andere `POST` vragen aan de `/token` eindpunt, lang mits de `refresh_token` in plaats van de `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Probeer deze aanvraag wordt uitgevoerd in de Postman! (Vergeet niet te vervangen door de `refresh_token`)     [ ![Uitvoeren in de Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | -------- |
| huurder | Vereist | De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's pachters.  Zie voor meer informatie [Basisbeginselen protocol](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| grant_type | Vereist | Moet `refresh_token` voor deze zijde van de autorisatie code stroom. |
| toepassingsgebied | Vereist | Door spaties gescheiden lijst met bereiken.  De scopes in deze poot verzocht moet gelijk aan of een subset van de bereiken in de oorspronkelijke authorization_code aanvraag poot verzocht.  Als de bereiken die in deze aanvraag meerdere bronservers omspannen, wordt het eindpunt v2.0 een token voor de resource die is opgegeven in het eerste bereik retourneren.  Zie [machtigingen, toestemming, en bereiken](active-directory-v2-scopes.md)voor een meer gedetailleerde uitleg van scopes.  |
| refresh_token | Vereist | De refresh_token die u hebt verkregen in de tweede zijde van de stroom.   |
| redirect_uri | Vereist | Dezelfde redirect_uri-waarde die is gebruikt voor de verwerving van de authorization_code. |
| client_secret | vereist voor web apps | Het geheim van de toepassing die u voor uw app in de app-portal registratie gemaakt.  Het moet niet worden gebruikt in een native app, omdat client_secrets niet op betrouwbare wijze kunnen worden opgeslagen op apparaten.  Het is vereist voor web apps en web-API's, die de mogelijkheid hebben om de client_secret veilig opslaan op de server. |

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord token wordt er als volgt uitzien:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De aangevraagde toegangstoken. De app kan deze token gebruiken voor verificatie bij de beveiligde bron, zoals een web API. |
| token_type | Geeft de waarde van het type token. Het enige type dat AD Azure ondersteunt is aan toonder  |
| expires_in | Hoe lang het toegangstoken geldig is (in seconden). |
| toepassingsgebied | De scopes die geldt voor de access_token. |
| refresh_token |  Een nieuwe 2.0 OAuth vernieuwen token. Met deze token recent overgenomen vernieuwen om ervoor te zorgen dat uw tokens vernieuwen blijven geldig zo lang mogelijk moet u het oude vernieuwen token vervangen.  |
| id_token | Een niet-ondertekende JSON Web Token (JWT). De base64Url app kan decoderen de segmenten van deze token informatie opvragen over de gebruiker die heeft ingelogd. De app kan de waarden in de cache en deze weer te geven, maar moet niet vertrouwen op hen voor elke vergunning of beveiligingsgrenzen.  Zie voor meer informatie over id_tokens [v2.0 eindpunt token verwijzing](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Fout antwoord
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |
| error_codes | Een lijst van STS specifieke foutcodes die in diagnostische gegevens helpen kunnen.  |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u bij de diagnose helpen kan.  |
| correlation_id | Een unieke id voor de aanvraag die u diagnostische gegevens van alle componenten kunt. |

Zie voor een beschrijving van de foutcodes en de aanbevolen clientactie, [foutcodes voor token eindpunt fouten](#error-codes-for-token-endpoint-errors).
