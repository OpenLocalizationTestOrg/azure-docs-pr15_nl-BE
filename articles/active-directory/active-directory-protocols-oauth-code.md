<properties
    pageTitle="Azure AD .NET Protocol overzicht | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe de toegang tot webtoepassingen en webservices API's in uw huurder met Azure Active Directory en OAuth 2.0 met HTTP-berichten."
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


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>De toegang tot webtoepassingen die via OAuth 2.0 en Azure Active Directory

Azure Active Directory (AD Azure) gebruikt OAuth 2.0 kunt u de toegang tot webtoepassingen en webservices API's in uw huurder Azure AD. Deze handleiding is taalonafhankelijk en wordt beschreven hoe u berichten verzenden en ontvangen HTTP-zonder gebruik van een van onze open source-bibliotheken.

De stroom OAuth 2.0 autorisatie-code is beschreven in [punt 4.1 van de OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749#section-4.1) . Het wordt gebruikt voor het uitvoeren van verificatie en autorisatie in de meeste typen app, met inbegrip van web apps en apps standaard geïnstalleerd.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 vergunning stroom

Op een hoog niveau, de stroom van de gehele vergunning voor een toepassing iets zien er zo uit:

![OAuth autorisatiecode stroom](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>De code van een vergunning aanvragen

De stroom van autorisatie code begint met de client om de gebruiker de `/authorize` eindpunt. In dit verzoek geeft de client de machtigingen die nodig zijn ter verwerving van de gebruiker. De eindpunten OAuth 2.0 kunt u krijgen via de pagina van uw toepassing in Azure klassieke Portal op de knop **Weergave eindpunten** in de onderste lade.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| huurder | Vereist | De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn bijvoorbeeld een huurder id's, `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tokens onafhankelijk van de huurder |
| client_id | Vereist | De toepassings-Id toegewezen aan uw app bij het registreren met Azure Active Directory. U kunt dit vinden in Azure Management Portal. Klik op **Active Directory**, klikt u op de map, klik op de toepassing en klikt u vervolgens op **configureren** |
| response_type | Vereist | Omvat `code` voor de autorisatie code stroom. |
| redirect_uri | aanbevolen | De redirect_uri van uw app, waarbij antwoorden verificatie kunnen worden verzonden en ontvangen door uw app.  Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal, behalve het url-gecodeerd moet zijn.  Voor native & mobiele toepassingen, moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Kan `query` of `form_post`.  |
| staat | aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het [voorkomen van aanvallen van cross-site request vervalsing](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |
| resource | optioneel | De App-ID de URI van het web API (beveiligde bron). De URI van de App-ID van het web API, zoekt in de Azure Management Portal, klik op **Active Directory**, klikt u op de map, klik op de toepassing en klik op **configureren**. |
| prompt | optioneel |  Geef het type interactie van de gebruiker die is vereist.<p> Geldige waarden zijn: <p> *inloggen*: de gebruiker moet worden gevraagd om hernieuwde verificatie. <p> *instemming*: gebruiker toestemming is verleend, maar moet worden bijgewerkt. De gebruiker moet worden gevraagd in te stemmen. <p> *admin_consent*: een beheerder moet worden gevraagd om toestemming voor alle gebruikers in hun organisatie |
| login_hint | optioneel | Kan worden gebruikt voor het vooraf invullen van het veld gebruikersnaam/e-mailadres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tevoren.  Vaak apps Gebruik deze parameter tijdens vervolgverificatie dat de gebruikersnaam al geëxtraheerd uit een vorige-in met de `preferred_username` claimen. |
| domain_hint | optioneel | Geeft een aanwijzing over de huurder of het domein dat de gebruiker aan te melden moet gebruiken. De waarde van de domain_hint is een geregistreerde domeinnaam voor de huurder. Als de huurder federated is met een map op de gebouwen, wordt AAD omgeleid naar de opgegeven huurder federation-server. |

> [AZURE.NOTE] Als de gebruiker deel van een organisatie uitmaakt, kan een beheerder van de organisatie toestemming weigeren voor rekening van de gebruiker of de gebruiker toe te staan. U krijgt de gebruiker de optie toe te staan, alleen als de beheerder het toestaat.

Op dit moment de gebruiker wordt gevraagd hun referenties invoeren en de instemming met de machtigingen die zijn aangegeven in de `scope` queryparameter. Zodra de gebruiker wordt geverifieerd en toestemming verleent, Azure AD stuurt een antwoord naar de app op de `redirect_uri` adres in uw aanvraag.

### <a name="successful-response"></a>Positief antwoord

Een positief antwoord kan uitzien:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschrijving |
| -----------------------| --------------- |
| admin_consent | De waarde is True als een beheerder ingestemd met een verzoek toestemming vragen.|
| code | De validatiecode op die de toepassing heeft aangevraagd. De toepassing kan de autorisatiecode voor het aanvragen van een toegangstoken voor de doelbron gebruiken. |
| session_state | Een unieke waarde die de huidige gebruikerssessie identificeert. Deze waarde is een GUID, maar moet worden behandeld als een ondoorzichtige waarde die wordt doorgegeven zonder onderzoek. |
| staat | Als een parameter status is opgenomen in de aanvraag, moet dezelfde waarde worden weergegeven in het antwoord. Het is raadzaam voor de toepassing om te controleren of de waarden staat in de aanvraag en het antwoord zijn identiek voordat u het antwoord. Dit helpt voor het detecteren van [aanvallen met Cross-Site Request vervalsing (CSRF)](https://tools.ietf.org/html/rfc6749#section-10.12) ten opzichte van de client.  

### <a name="error-response"></a>Fout antwoord

Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` , zodat de toepassing deze op de juiste wijze kan verwerken.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
|-----------|-------------|
| Fout | Een fout codewaarde gedefinieerd in paragraaf 5.2 van [OAuth 2.0 vergunning Framework](http://tools.ietf.org/html/rfc6749). De volgende tabel beschrijft de foutcodes die Azure AD als resultaat geeft. |
| error_description | Een meer gedetailleerde beschrijving van de fout. Dit bericht is niet bedoeld voor eindgebruikers friendly. |
| staat | De waarde staat is een willekeurige waarde die niet opnieuw gebruikt wordt verzonden in de aanvraag en antwoord voor cross-site request vervalsing (CSRF) aanvallen te voorkomen. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Gebruik de code van de vergunning voor het aanvragen van een toegangstoken

Nu dat u een autorisatiecode hebt aangeschaft en zijn gemachtigd door de gebruiker, kunt u de code voor een toegangstoken voor de gewenste resource inwisselen via een POST-aanvraag naar de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| huurder | Vereist |  De `{tenant}` de waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie je bij de toepassing aanmelden kunt.  De toegestane waarden zijn bijvoorbeeld een huurder id's, `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tokens onafhankelijk van de huurder |
| client_id | Vereist | De toepassings-Id toegewezen aan uw app bij het registreren met Azure Active Directory. U kunt dit vinden in de klassieke Azure-Portal. Klik op **Active Directory**, klikt u op de map, klik op de toepassing en klikt u vervolgens op **configureren** |
| grant_type | Vereist | Moet `authorization_code` voor de autorisatie code stroom. |
| code | Vereist | De `authorization_code` die u hebt verkregen in de vorige sectie   |
| redirect_uri | Vereist | Dezelfde `redirect_uri` -waarde die is gebruikt voor het verkrijgen van de `authorization_code`. |
| client_secret | vereist voor web apps | Het geheim van de toepassing die u voor uw app in de app-portal registratie gemaakt.  Het moet niet worden gebruikt in een native app, omdat client_secrets niet op betrouwbare wijze kunnen worden opgeslagen op apparaten.  Vereist voor web apps en web-API's, de mogelijkheid om op te slaan met de `client_secret` veilig op de server. |
| resource | Als in de code autorisatieaanvraag, anders optioneel opgegeven vereist | De App-ID de URI van het web API (beveiligde bron).
Met de App ID-URI vinden in Azure Management Portal, **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**.

### <a name="successful-response"></a>Positief antwoord

Azure AD geeft als resultaat een toegangstoken op een positief antwoord. Als het netwerk oproepen van de clienttoepassing en de bijbehorende Latentie minimaliseren, moet de clienttoepassing toegangstokens voor de levensduur van tokens die is opgegeven in het antwoord OAuth 2.0 cache. Gebruiken om te bepalen van de levensduur van tokens, de `expires_in` of `expires_on` parameterwaarden.

Als een web API resource geeft een `invalid_token` foutcode op, dit kan betekenen dat de resource is bepaald dat het token is verlopen. Als de client- en klok tijden verschillende (ook wel een 'scheeftrekken tijd'), overwegen de resource het token verlopen voordat het token van de client-cache is gewist. Als dit gebeurt, schakelt u het token van de cache, zelfs als het nog steeds binnen de berekende levensduur.

Een positief antwoord kan uitzien:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De aangevraagde toegangstoken. De app kan deze token gebruiken voor verificatie bij de beveiligde bron, zoals een web API. |
| token_type | Geeft de waarde van het type token. Het enige type dat AD Azure ondersteunt is aan toonder. Zie voor meer informatie over tokens aan toonder [OAuth2.0 vergunning Framework: aan toonder Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Hoe lang het toegangstoken geldig is (in seconden). |
| expires_on | Het tijdstip waarop het toegangstoken verloopt. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de verlooptijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| resource | De App-ID de URI van het web API (beveiligde bron).|
| toepassingsgebied | Imitatie-machtigingen toegekend aan de clienttoepassing. Standaard de machtiging is `user_impersonation`. De eigenaar van de beveiligde bron kunt u aanvullende waarden registreren in Azure AD.|
| refresh_token |  Een token van OAuth 2.0 vernieuwen. De app kunt deze token verkrijgen van extra toegangstokens na het verstrijken van het huidige toegangstoken.  Vernieuwen tokens zijn lange levensduur en kan worden gebruikt voor toegang tot bronnen voor langere tijd bewaren. |
| id_token | Een niet-ondertekende JSON Web Token (JWT). De base64Url app kan decoderen de segmenten van deze token informatie opvragen over de gebruiker die heeft ingelogd. De app kan de waarden in de cache en deze weer te geven, maar moet niet vertrouwen op hen voor elke vergunning of beveiligingsgrenzen. |

### <a name="jwt-token-claims"></a>JWT Token aanvragen
De JWT token in de waarde van de `id_token` parameter kan worden gedecodeerd naar de volgende argumenten:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

De `id_token` parameter bevat de volgende typen. Zie voor meer informatie over web-tokens JSON, de [IETF JWT conceptspecificatie](http://go.microsoft.com/fwlink/?LinkId=392344). Voor meer informatie over de typen token en claims Lees [Token ondersteund en typen](active-directory-token-and-claims.md)

| Claimtype | Beschrijving |
|------------|-------------|
| AUD | De doelgroep van het token. Als het token is uitgegeven aan een clienttoepassing, de doelgroep is de `client_id` van de client.
| EXP | Verlooptijd. Het tijdstip waarop het token verloopt. Voor het token om geldig te zijn, de huidige datum en tijd moet kleiner zijn dan of gelijk aan de `exp` waarde. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC tot het moment dat het token is uitgegeven. |
| family_name | Van gebruiker laatste naam of achternaam. De toepassing kunt deze waarde weergeven. |
| given_name | De voornaam van de gebruiker. De toepassing kunt deze waarde weergeven. |
| IAT | Op tijd afgegeven. Het tijdstip waarop de JWT is afgegeven. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC tot het moment dat het token is uitgegeven. |
| ISS | Identificeert de tokenuitgever |
| NBF | Niet eerder tijd. Het tijdstip waarop het token van kracht. Het token om geldig te zijn, moet de huidige datum en tijd groter dan of gelijk aan de waarde van de Nbf. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC tot het moment dat het token is uitgegeven. |
| OID | Object-id (ID) van het gebruikersobject in Azure AD. |
| Sub | Token onderwerp-id. Dit is een permanent en onveranderbaar id voor de gebruiker die het token wordt beschreven. Gebruik deze waarde in cache logica. |
| TID | Identificatie (ID) van de huurder Azure AD die token van de uitgegeven pachters. |
| unique_name | Een unieke identificatie voor die kan worden weergegeven voor de gebruiker. Dit is meestal een UPN (user Principal name). |
| UPN | Principal-naam van de gebruiker. |
| ver | Versie. De versie van het token JWT, meestal 1.0. |

### <a name="error-response"></a>Fout antwoord

De token uitgifte eindpunt fouten worden HTTP-foutcodes, omdat de client het eindpunt token uitgifte rechtstreeks wordt aangeroepen. Naast de HTTP-statuscode retourneert het eindpunt van de token uitgifte Azure AD ook een JSON-document met objecten die een beschrijving van de fout.

Een foutmelding monster kan uitzien:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |
| error_codes | Een lijst van STS specifieke foutcodes die in diagnostische gegevens helpen kunnen. |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u bij de diagnose helpen kan.  |
| correlation_id | Een unieke id voor de aanvraag die u diagnostische gegevens van alle componenten kunt.|

#### <a name="http-status-codes"></a>HTTP-statuscodes

De volgende tabel worden de HTTP-statuscodes die het token uitgifte eindpunt als resultaat geeft. In sommige gevallen, de foutcode is voldoende voor het beschrijven van het antwoord, maar in geval van fouten, moet u het geleidedocument JSON parseren en de foutcode te onderzoeken.

| HTTP-Code | Beschrijving |
|-----------|-------------|
| 400       | Standaard HTTP-code. In de meeste gevallen gebruikt en wordt meestal veroorzaakt door een verkeerd ingedeelde aanvraag. Herstel en de aanvraag opnieuw indienen. |
| 401       | Verificatie is mislukt. De aanvraag ontbreekt bijvoorbeeld de parameter client_secret.|
| 403       | Verificatie mislukt. De gebruiker heeft bijvoorbeeld geen machtiging voor toegang tot de bron. |
| 500       | Er is een interne fout opgetreden bij de service. Probeer de aanvraag opnieuw. |

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

## <a name="use-the-access-token-to-access-the-resource"></a>Het toegangstoken voor de bron gebruiken

Nu dat u hebt gekregen een `access_token`, kunt u de token aanvragen voor Web-API's door te nemen in de `Authorization` kop. De specificatie [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) wordt uitgelegd hoe u aan toonder tokens gebruiken in HTTP-aanvragen voor toegang tot beveiligde bronnen.

### <a name="sample-request"></a>Voorbeeld van een aanvraag

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Fout antwoord

Beveiligde bronnen en implementatie van RFC 6750 probleem HTTP-statuscodes. Als de aanvraag geen verificatiereferenties bevat of het token ontbreekt, wordt het antwoord bevat een `WWW-Authenticate` kop. Wanneer een aanvraag is mislukt, reageert de server van de resource met het een HTTP-statuscode en een foutcode.

Hier volgt een voorbeeld van een mislukt antwoord bij de aanvraag van de client het token aan toonder niet is opgenomen:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Foutparameters

| Parameter | Beschrijving |
|-----------|-------------|
| authorization_uri | De URI (fysieke eindpunt) van de server van de vergunning. Deze waarde wordt ook gebruikt als een zoeksleutel voor meer informatie over de server van een endpoint discovery. <p><p> De client moet valideren dat de vergunning server vertrouwd wordt. Wanneer de bron wordt beschermd door AD Azure, is het voldoende om te verifiëren dat de URL begint met https://login.windows.net of een andere hostnaam die Azure AD ondersteunt. Een huurder-specifieke resource moet altijd een huurder-specifieke vergunning URI retourneren. |
| Fout | Een fout codewaarde gedefinieerd in paragraaf 5.2 van [OAuth 2.0 vergunning Framework](http://tools.ietf.org/html/rfc6749).|
| error_description | Een meer gedetailleerde beschrijving van de fout. Dit bericht is niet bedoeld voor eindgebruikers friendly.|
| bron-id | Deze eigenschap retourneert de unieke id van de resource. De clienttoepassing kan deze id gebruiken als de waarde van de `resource` parameter na het aanvragen van een token voor de resource. <p><p> Het is heel belangrijk voor de clienttoepassing controleren of deze waarde, anders een schadelijke service mogelijk te induceren een aanval **kan leiden tot misbruik van bevoegdheden** <p><p> De aanbevolen strategie voor het voorkomen van een aanval is te controleren dat de `resource_id` overeenkomt met de basis van het web API-URL wordt geopend. Als https://service.contoso.com/data wordt gebruikt, bijvoorbeeld de `resource_id` htttps://service.contoso.com/ kan worden. De clienttoepassing moet weigeren een `resource_id` die niet begint met de basis-URL als een betrouwbare alternatieve manier om te controleren of de id. |

#### <a name="bearer-scheme-error-codes"></a>Aan toonder schema foutcodes

De specificatie RFC 6750 definieert de volgende fouten voor resources die worden gebruikt met de WWW-Authenticate-header en vruchtdragende schema in het antwoord.

| HTTP-statuscode | Foutcode | Beschrijving | Actie op de client |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | De aanvraag is niet correct opgemaakt. Het kan bijvoorbeeld worden ontbreekt een parameter of twee keer dezelfde parameter gebruiken. | Corrigeer de fout en proberen de aanvraag. Dit type fout moet plaatsvinden tijdens de ontwikkeling en in de eerste beproeving worden gedetecteerd. |
| 401 | invalid_token   | Het toegangstoken ontbreekt, is ongeldig of wordt ingetrokken. De waarde van de parameter error_description geeft u meer details. |  Een nieuwe beveiligingssleutel aanvragen van de vergunning-server. Als het nieuwe token mislukt, is een onverwachte fout opgetreden. Een foutbericht verzenden naar de gebruiker en probeer het opnieuw na een willekeurige vertraging. |
| 403 | insufficient_scope | Het toegangstoken bevat niet de imitatie vereiste machtigingen voor toegang tot de bron. | Een nieuwe vergunning aanvraag verzenden naar het eindpunt van de vergunning. Als het antwoord de parameter scope bevat, gebruikt u de waarde van de scope in de aanvraag voor de resource. |
| 403 | insufficient_access | Het onderwerp van het token beschikt niet over de machtigingen die vereist zijn voor toegang tot de bron. | De gebruiker met een andere account of machtigingen aanvragen om de opgegeven bron te vragen. |

## <a name="refreshing-the-access-tokens"></a>De toegangstokens vernieuwen

Toegangstokens zijn tijdelijk en vervallen als u wilt doorgaan met het verkrijgen van toegang tot bronnen moeten worden vernieuwd. U kunt vernieuwen de `access_token` door het indienen van een andere `POST` vragen aan de `/token` eindpunt, maar op dit moment bieden de `refresh_token` in plaats van de `code`.

Vernieuwen tokens hebt opgegeven levensduur. De levensduur van tokens vernieuwen zijn meestal relatief lang. Echter in sommige gevallen vernieuwen tokens laten verlopen, worden ingetrokken of niet over voldoende bevoegdheden voor de gewenste actie. Uw toepassing moet verwachten en de afhandeling van fouten die worden geretourneerd door het eindpunt token uitgifte goed.

Wanneer u een antwoord met een fout vernieuwen ontvangt, token van de huidige vernieuwen annuleren en een nieuwe vergunning code aanvragen of -toegangstoken. In het bijzonder wanneer met behulp van een vernieuwing token in de autorisatie Code toekennen, als er een reactie met de `interaction_required` of `invalid_grant` foutcodes, de token vernieuwen negeren en een nieuwe vergunning code aanvragen.

Een voorbeeld van een aanvraag het eindpunt van de **huurder-specifieke** (u kunt ook het **algemene** eindpunt) om een nieuwe access token met een token vernieuwd, ziet er:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parameter | Beschrijving |
|-----------|-------------|
| access_token | Het nieuwe toegangstoken dat is aangevraagd.|
| expires_in   | De resterende levensduur van het token in seconden. Een normale waarde is 3600 (1 uur). |
| expires_on   | De datum en tijd waarop het token is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de verlooptijd. |
| refresh_token | Een nieuwe OAuth 2.0 refresh_token die kunnen worden gebruikt voor het aanvragen van nieuwe toegangstokens wanneer in dit antwoord is verlopen. |
| resource     | Geeft de beveiligde bron die het toegangstoken kan worden gebruikt voor toegang. |
| toepassingsgebied        | Imitatie-machtigingen toegekend aan de native clienttoepassing. Standaard de machtiging is **user_impersonation**. De eigenaar van de doelbron kunt u alternatieve waarden registreren in Azure AD. |
| token_type   | Het type token. De enige ondersteunde waarde is **aan toonder**. |

### <a name="successful-response"></a>Positief antwoord

Een positief antwoord token wordt er als volgt uitzien:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Fout antwoord

Een foutmelding monster kan uitzien:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van soorten fouten die optreden, en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar de oorzaak van een verificatiefout identificeren.  |
| error_codes | Een lijst van STS specifieke foutcodes die in diagnostische gegevens helpen kunnen. |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u bij de diagnose helpen kan.  |
| correlation_id | Een unieke id voor de aanvraag die u diagnostische gegevens van alle componenten kunt.|

Zie voor een beschrijving van de foutcodes en de aanbevolen clientactie, [foutcodes voor token eindpunt fouten](#error-codes-for-token-endpoint-errors).
