
<properties
    pageTitle="Stroom-Azure AD v2.0 OAuth-Client referenties | Microsoft Azure"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>Protocollen v2.0 - OAuth 2.0 clientreferenties Flow

De [referenties van de client 2.0 OAuth verlenen](http://tools.ietf.org/html/rfc6749#section-4.4), soms aangeduid als "tweearmige OAuth" kan worden gebruikt voor toegang tot bronnen op het web geplaatst met behulp van de identiteit van een toepassing.  Dit wordt meestal gebruikt voor de communicatie van server naar server dat moeten worden uitgevoerd op de achtergrond zonder de directe precense van een eindgebruiker.  Dit soort toepassingen zijn vaak **daemons** of **serviceaccounts**genoemd.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

In de meer gebruikelijke drie "drie-legged OAuth," is een clienttoepassing machtiging voor toegang tot een bron voor een bepaalde gebruiker.  De machtiging is **overgedragen** van de gebruiker de toepassing, meestal tijdens de [toestemming](active-directory-v2-scopes.md) .  Echter, in de stroom van de referenties client machtigingen **rechtstreeks** op de toepassing zelf.  Wanneer de app een token aan een resource presenteert, de resource wordt afgedwongen dat de app zelf vergunning heeft voor het uitvoeren van een actie - niet dat een gebruiker machtiging heeft.

## <a name="protocol-diagram"></a>Protocol-diagram
De volledige client referenties stroom uitziet - de stappen worden hieronder uitvoerig beschreven.

![Client referenties stroom](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Directe vergunning ophalen 
Dat een app meestal ontvangt de directe machtiging voor toegang tot een bron op twee manieren: via een ACL (toegangsbeheerlijst) voor de bron of toepassing machtigingen worden toegewezen in AD Azure.  Er zijn diverse andere manieren waarop die een resource kiezen ervoor de clients toe te staan en elke server resource kan de methode kiezen die het meest zinvol voor de toepassing.  Deze twee methoden zijn de meest voorkomende in Azure AD en reccommended voor clients en bronnen die u wilt uitvoeren de stroom client referenties.

### <a name="access-control-lists"></a>Access control list
Een bepaalde bron provider kan dwingen een vergunning controle op basis van een lijst van application-id's het weet en een bepaalde mate van toegang verleent.  Als de resource een token van het eindpunt v2.0 ontvangt, kan het token decoderen en pak de toepassings-ID van de client van de `appid` en `iss` vorderingen.  Vergelijk kan vervolgens dat de toepassing tegen een toegangsbeheerlijst (ACL) behouden blijven.  De granulatie en de methode van de toegangsbeheerlijst kunnen variëren sterk van bron tot bron.

Een gemeenschappelijke use-case voor deze ACL's is lopers voor een webtoepassing testen of web-api.  Het web api kan slechts verlenen een subset van de volledige machtigingen aan verschillende clients.  Maar om complete tests op de api uitvoert, een testclient wordt gemaakt die tokens van het eindpunt v2.0 verwerft en verzendt deze naar de api.  De api kan vervolgens ACL de testclient toepassings-ID voor volledige toegang tot de volledige functionaliteit van de api's.  Houd er rekening mee dat als u deze lijst op uw service hebt, moet u ervoor dat u niet alleen valideren van de beller `appid`, maar ook controleren of de `iss` van het token wordt vertrouwd ook.

Dit type verificatie geldt voor daemons en serviceaccounts waarvoor toegang moeten krijgen tot gegevens die eigendom zijn van de consument gebruikers met een persoonlijke Microsoft-account.  Voor gegevens die eigendom zijn van organisaties, het raadzaam dat u de benodigde vergunning via de toepassing perimssions krijgt.

### <a name="application-permissions"></a>Machtigingen voor een toepassing
API's kan in plaats van ACL's, een set **machtigingen** die kunnen worden toegekend aan een toepassing worden blootgesteld.  Een toepassing toestemming wordt verleend aan een toepassing door een beheerder van een organisatie en kan alleen worden gebruikt voor toegang tot gegevens die het eigendom van die organisatie en haar medewerkers.  De Microsoft Graph beschrijft bijvoorbeeld verschillende machtigingen van toepassing:

- E-mail lezen in alle postvakken
- Lezen en schrijven van e-mailberichten in alle postvakken
- Als een gebruiker het verzenden van e-mail
- Active directory-gegevens lezen
- [+ meer](https://graph.microsoft.io)

Om deze machtigingen in uw app aanschaft, kunt u de volgende stappen uitvoeren.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in het app-portal Registratie aanvragen

- Navigeer naar de toepassing in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of [een app maken](active-directory-v2-app-registration.md) als u dat nog niet gedaan hebt.  U moet ervoor zorgen dat uw toepassing ten minste één toepassing geheim heeft gemaakt.
- Ga naar de sectie **Machtigingen voor directe toepassing** en de machtigingen voor uw app toevoegen.
- Controleer of op te **slaan** de app registratie

#### <a name="recommended-sign-the-user-into-your-app"></a>Aanbevolen: de gebruiker inloggen op uw app

Meestal bij het maken van een toepassing die gebruikmaakt van machtigingen voor een toepassing, moet de app een/paginaweergave die het goedkeuren van het app-machtigingen van de beheerder kan hebben.  Deze pagina kan deel uitmaken van aanmelding flow van de app, onderdeel van de instellingen van de app, of een speciale 'verbinden' stroom.  In veel gevallen is het zinvol voor de toepassing weer te geven 'verbinden' weergeven nadat een gebruiker is aangemeld met een werk of school Microsoft-account.

De gebruiker aanmelden bij de app kunt u identificeren van de organziation waartoe de gebruiker behoort voordat het verzoek om de machtigingen voor een toepassing goedkeuren.  Hoewel niet strikt noodzakelijk is, kunt u een meer intuïtieve ervaring voor gebruikers van uw organisatie te maken.  Ondertekenen van de gebruiker in, volg onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>De machtigingen van een map admin aanvragen

Wanneer u klaar voor toegang van de beheerder van het bedrijf bent, kunt u de gebruiker omleiden naar v2.0 **admin toestemming eindpunt**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| huurder | Vereist | De huurder directory die u wilt toestemming van aanvragen.  Beschrijvende naam of guid kan worden geleverd.  Als u niet weet welke huurder waartoe de gebruiker behoort en Vertel ze zich aanmelden met een huurder, gebruikt u `common`. |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| redirect_uri | Vereist | De redirect_uri waar u het antwoord verzonden voor uw app te verwerken.  Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, behalve het url-gecodeerd zijn en kan aanvullende padsegmenten moet. |
| staat | aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd.  Een tekenreeks met de inhoud die u kan zijn.  De status wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |

Op dit moment wordt Azure AD afdwingen dat de beheerder van een huurder aanmelden kunt om de aanvraag te voltooien.  De beheerder wordt gevraagd om goed te keuren alle rechtstreekse Toepassingsmachtigingen die u hebt aangevraagd voor uw app in de portal voor registratie. 

##### <a name="successful-response"></a>Positief antwoord
Als de beheerder de machtigingen voor uw toepassing goedkeurt, is positief antwoord:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| huurder | De huurder directory die uw toepassing de machtigingen aangevraagd, in de indeling van de guid. |
| staat | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd.  Een tekenreeks met de inhoud die u kan zijn.  De status wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |
| admin_consent | Wordt ingesteld op `True`. |


##### <a name="error-response"></a>Fout antwoord
Als de beheerder de machtigingen voor uw toepassing niet goedkeurt, is het antwoord is mislukt:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een fout vaststellen.  |

Zodra u een positief antwoord van het eindpunt wordt ingericht app hebt ontvangen, kan uw app de rechtstreekse Toepassingsmachtigingen die zij verzocht hebben opgedaan.  U kunt nu verplaatsen naar het aanvragen van een token voor de gewenste resource.

## <a name="get-a-token"></a>Een token ophalen
Nadat u de nodige vergunning hebt aangeschaft voor uw toepassing, kunt u doorgaan met het verkrijgen van toegangstokens voor API's.  Verzenden als u een token met behulp van de client de referenties subsidie, een POST-aanvraag naar de `/token` v2.0 eindpunt:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| toepassingsgebied | Vereist | De doorgegeven waarde voor de `scope` parameter in deze aanvraag moet de resource identifier (URI App-ID) van de gewenste resource, voorzien van de `.default` achtervoegsel.  Dus bijvoorbeeld de Microsoft Graph gegeven de waarde mag `https://graph.microsoft.com/.default`.  Deze waarde geeft aan het eindpunt v2.0 dat van alle rechtstreekse Toepassingsmachtigingen die u hebt geconfigureerd voor uw app, deze moet verlenen een token voor de regels met betrekking tot de gewenste resource. |
| client_secret | Vereist | Het geheim van toepassing die u hebt gegenereerd in de portal Registratie van uw app. |
| grant_type | Vereist | Moet zijn `client_credentials`. | 

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord zal de vorm aannemen:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De aangevraagde toegangstoken. De app kan deze token gebruiken voor verificatie bij de beveiligde bron, zoals een web API. |
| token_type | Geeft de waarde van het type token. Het enige type dat ondersteunt Azure AD `Bearer`.  |
| expires_in | Hoe lang het toegangstoken geldig is (in seconden). |

#### <a name="error-response"></a>Fout antwoord
Reactie op een fout, zal de vorm aannemen:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

## <a name="use-a-token"></a>Een token gebruik
Nu dat u een token hebt aangeschaft, kunt u deze token verzoeken om de resource te maken.  Wanneer het token is verlopen, Herhaal het verzoek aan de `/token` eindpunt een toegangstoken vers verkrijgen.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Voorbeeld van code
Verwijzen naar onze [v2.0 daemon codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)om een voorbeeld van een toepassing dat implementeert die de client_credentials verlenen met behulp van de admin toestemming eindpunt.
