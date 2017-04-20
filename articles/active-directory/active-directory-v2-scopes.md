<properties
    pageTitle="Azure AD v2.0 Scopes, machtigingen & toestemming | Microsoft Azure"
    description="Een beschrijving van de vergunning in Azure AD v2.0 eindpunt, met inbegrip van scopes, machtigingen en toestemming."
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Scopes, machtigingen & toestemming in het eindpunt v2.0

Apps die met AD Azure integreren voert u een specifieke vergunning model waarmee gebruikers kunnen bepalen hoe een app toegang krijgen tot hun gegevens.  De v2.0-implementatie van dit model vergunning is bijgewerkt, hoe een app moet samenwerken met Azure Active Directory wijzigen.  In dit onderwerp worden de basisconcepten van dit model vergunning, met inbegrip van scopes, machtigingen en toestemming.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Scopes & machtigingen

Azure AD implementeert het [OAuth 2.0](active-directory-v2-protocols.md) vergunning-protocol, dat een methode is voor een 3de partij app toegang tot web gehost bronnen een gebruiker toestaan.  Alle bronnen voor web host die is geïntegreerd met AD Azure heeft een bron-id of **App-ID-URI**.  Enkele van Microsoft web host resources zijn bijvoorbeeld:

- De Office 365 Unified API Mail:`https://outlook.office.com`
- De Azure AD Graph API:`https://graph.windows.net`
- De grafiek van Microsoft:`https://graph.microsoft.com`

Hetzelfde geldt voor de 3e partij bronnen die met Azure Active Directory is geïntegreerd.  Ook kunt een van deze bronnen een verzameling machtigingen die kan worden gebruikt om de functionaliteit van die resource in kleinere stukken verdelen.  Als voorbeeld, is de Microsoft Graph een paar machtigingen gedefinieerd:

- De agenda van een gebruiker lezen
- Schrijven naar de agenda van een gebruiker
- E-mail verzenden als een gebruiker
- [+ meer](https://graph.microsoft.io)

Als u deze machtigingen, kan de resource verfijnde controle over de gegevens en hoe deze wordt blootgesteld aan de buitenwereld hebben.  Een 3de partij app vervolgens deze machtigingen kan aanvragen van een eindgebruiker - en de eindgebruiker de machtigingen moet goedkeuren voordat de app kan namens hun.  Met de functionaliteit van de resource in kleinere machtigingensets logische groepen te verdelen, worden 3de partij apps om alleen de specifieke machtigingen die ze nodig hebben om hun rechten uitgerust.  U kunt ook gebruikers weten precies hoe een app hun gegevens gebruikt, zodat ze zeker de app werkt anders met slechte bedoelingen.

In Azure AD en OAuth, deze machtigingen worden genoemd **bereiken**.  Ook ziet u ze wel **oAuth2Permissions**genoemd.  Een scope in Azure AD voorgesteld als een string-waarde.  U doorgaat met het voorbeeld van Microsoft Graph, is de bereikwaarde voor elke machtiging:

- De agenda van een gebruiker lezen:`Calendar.Read`
- Schrijven naar de agenda van de gebruiker:`Mail.ReadWrite`
- E-mail verzenden als een gebruiker:`Mail.Send`

Een app kan deze machtigingen aanvragen door de bereiken opgeven in aanvragen voor het eindpunt v2.0, zoals hieronder beschreven.

## <a name="openid-connect-scopes"></a>OpenId verbinding scopes

De uitvoering v2.0 van OpenID verbinding heeft een paar goed gedefinieerde scopes die niet van toepassing op een bepaalde bron - `openid`, `email`, `profile`, en `offline_access`.

#### <a name="openid"></a>OpenId

Als een toepassing wordt uitgevoerd-in met [OpenID verbinding](active-directory-v2-protocols.md#openid-connect-sign-in-flow), verzoekt zij de `openid` bereik.  De `openid` bereik wordt weergegeven in het scherm werk account toestemming als de machtiging 'Aanmelden' en in het Microsoft-account toestemming scherm als de machtiging 'Uw profiel bekijken en verbinding maken met de toepassingen en services met behulp van uw Microsoft-account'.  Met deze machtiging kan een app voor het ontvangen van een unieke id voor de gebruiker in de vorm van de `sub` claimen.  Zij kan ook de app toegang tot het eindpunt gebruiker info.  De `openid` bereik kan ook worden gebruikt op het eindpunt van de v2.0 token verkrijgen van id_tokens, die kan worden gebruikt om beveiligde HTTP-aanroepen tussen verschillende onderdelen van een app.

#### <a name="email"></a>E-mail

De `email` bereik kan worden opgenomen in combinatie met de `openid` bereik en andere.  Het biedt de app toegang tot de primaire e-mailadres van de gebruiker in de vorm van de `email` claimen.  De `email` claim wordt alleen opgenomen in tokens als een e-mailadres gekoppeld aan de gebruikersaccount is, niet altijd het geval is.  Als de `email` bereik, uw app dient te worden opgesteld voor het verwerken van de gevallen waarin de `email` claim in het token niet bestaat.

#### <a name="profile"></a>Profiel

De `profile` bereik kan worden opgenomen in combinatie met de `openid` bereik en andere.  Zij kan de app toegang tot een schat aan informatie over de gebruiker.  Dit omvat, maar is niet beperkt tot, van de gebruiker opgegeven naam, achternaam, gebruikersnaam voorkeur, object-ID, enzovoort.  Raadpleeg de [token v2.0-verwijzing](active-directory-v2-tokens.md)voor een volledige lijst van de schuldvorderingen profiel beschikbaar in de id_tokens voor een bepaalde gebruiker.

#### <a name="offlineaccess"></a>Offline_access

De [ `offline_access` bereik](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) uw app voor toegang tot bronnen voor de gebruiker kan voor langere tijd.  Dit bereik wordt weergegeven in het scherm werk account toestemming als de machtiging "Toegang tot uw gegevens op elk gewenst moment".  In het persoonlijke Microsoft-account toestemming scherm wordt weergegeven als de machtiging "Toegang tot uw gegevens op elk gewenst moment".  Wanneer een gebruiker keurt de `offline_access` bereik, uw app vernieuwen tokens ontvangen van het token eindpunt v2.0 wordt ingeschakeld.  Vernieuwen tokens zijn lange levensduur en laat uw app aan te schaffen toegangstokens nieuwe als oudere verloopt.

Als uw app vraagt niet om de `offline_access` bereik, ontvangt geen refresh_tokens.  Dit betekent dat wanneer je een authorization_code in de [OAuth 2.0 vergunning code stroom](active-directory-v2-protocols.md#oauth2-authorization-code-flow), u alleen weer een access_token van ontvangt de `/token` eindpunt.  Die access_token blijft geldig gedurende korte tijd (meestal 1 uur), maar uiteindelijk zal verlopen.  Op dat moment, uw app wilt omleiden van de gebruiker terug naar de `/authorize` eindpunt voor het ophalen van een nieuwe authorization_code.  Tijdens deze omleiding, de gebruiker al dan niet nodig om hun referenties opnieuw invoeren of opnieuw toestemming voor machtigingen, afhankelijk van de het soort app.

Tokens vernieuwen voor meer informatie over het verkrijgen en gebruiken, raadpleegt u de [v2.0 protocol verwijzing](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Individuele gebruiker toestemming wordt gevraagd

In een autorisatieaanvraag [OpenID verbinding of OAuth 2.0](active-directory-v2-protocols.md) een app kan verzoeken om de machtigingen die nodig zijn met behulp van de `scope` queryparameter.  Wanneer een gebruiker zich bij een app aanmeldt, zou de toepassing een verzoek als volgt uit (met regeleinden voor leesbaarheid) verzenden:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

De `scope` parameter is een door spaties gescheiden lijst met bereiken die de toepassing aanvraagt.  Elke afzonderlijke scope wordt aangegeven door de bereikwaarde op van de resource identifier (URI App ID) toe te voegen.  De bovenstaande aanvraag geeft aan dat de app toestemming is nodig om de agenda van de gebruiker lees- en e-mailberichten verzenden als de gebruiker.

Nadat de gebruikers hun referenties invoert, wordt het eindpunt v2.0 voor een overeenkomende record van de **gebruiker toestemming**gecontroleerd.  Als de gebruiker in het verleden niet heeft ingestemd met een van de machtigingen die zijn aangevraagd, de v2.0 eindpunt aan de gebruiker gevraagd om de gevraagde machtigingen te verlenen.  

![Werk Account toestemming Screenshot](../media/active-directory-v2-flows/work_account_consent.png)

Wanneer de gebruiker de machtiging goedkeurt, wordt de toestemming vastgelegd zodat de gebruiker heeft geen opnieuw toestemming voor volgende aanmeldingen.

## <a name="requesting-consent-for-an-entire-tenant"></a>Toestemming voor een hele huurder aanvragen

Vaak wanneer een bedrijf een licentie of abonnement op een toepassing koopt, willen zij volledig inrichten voor hun werknemers.  Als onderdeel van dit proces kan een systeembeheerder van bedrijf toestemming voor de toepassing om te handelen namens een werknemer toekennen.  Door het verlenen van toestemming voor een hele huurder zal werknemers van die organisatie treedt niet op het scherm van de toestemming voor de toepassing.

Uw app kunt wilt aanvragen toestemming voor alle gebruikers in een huurder, gebruiken de **admin toestemming eindpunt**, zoals hieronder beschreven.

## <a name="admin-restricted-scopes"></a>Scopes Admin beperkt

Bepaalde machtigingen in het Microsoft ecosysteem hoge bevoegdheid kunnen worden gemarkeerd als **administrator beperkt**.  Voorbeelden van dergelijke bereiken zijn:

- Een organizaion van Active directory-gegevens worden gelezen:`Directory.Read`
- Schrijven van gegevens naar de map van een organisatie:`Directory.ReadWrite`
- Beveiligingsgroepen in de directory van een organisatie worden gelezen:`Groups.Read.All`

Terwijl een gebruiker van de consument kan een toepassing toegang tot dergelijke gegevens verlenen, worden organisatie-gebruikers beperkte verlenen van toegang tot dezelfde reeks gevoelige bedrijfsinformatie.  Als uw toepassing vraagt om toegang tot één van deze machtigingen van een organisatie-gebruiker, ontvangt de gebruiker een foutbericht met de mededeling dat zij onbevoegde toe te staan van uw app-machtigingen zijn.

Als uw app toegang tot deze scopes admin beperkt voor organisaties vereist, moet u deze aanvragen rechtstreeks van de beheerder van een bedrijf ook met behulp van de **admin toestemming eindpunt**, zoals hieronder beschreven.

Wanneer een beheerder machtigingen via het eindpunt admin toestemming verleent, wordt toestemming verleend voor alle gebruikers in de pachter, zoals hierboven beschreven.

## <a name="using-the-admin-consent-endpoint"></a>Met behulp van het eindpunt van de toestemming admin

Volgt te werk om uw app kunnen worden voor het verzamelen van machtigingen voor alle gebruikers in een bepaalde huurder, met inbegrip van scopes admin beperkt.  Verwijzen naar de [admin beperkt scopes monster](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2)een codevoorbeeld waarmee de inventarisaties stappen hieronder vindt.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in het app-portal Registratie aanvragen

- Navigeer naar de toepassing in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of [een app maken](active-directory-v2-app-registration.md) als u dat nog niet gedaan hebt.
- Ga naar de sectie **Machtigingen van Microsoft Graph** en de machtigingen voor uw app toevoegen.
- Controleer of op te **slaan** de app registratie

#### <a name="recommended-sign-the-user-into-your-app"></a>Aanbevolen: de gebruiker inloggen op uw app

Meestal bij het maken van een toepassing die gebruikmaakt van het admin eindpunt instemt, moet de app een/paginaweergave die het goedkeuren van het app-machtigingen van de beheerder kan hebben.  Deze pagina kan deel uitmaken van aanmelding flow van de app, onderdeel van de instellingen van de app, of een speciale 'verbinden' stroom.  In veel gevallen is het zinvol voor de toepassing weer te geven 'verbinden' weergeven nadat een gebruiker is aangemeld met een werk of school Microsoft-account.

De gebruiker aanmelden bij de app kunt u de organziation waartoe admin behoort voor het goedkeuren van de vereiste machtigingen op te geven.  Hoewel niet strikt noodzakelijk is, kunt u een meer intuïtieve ervaring voor gebruikers van uw organisatie te maken.  Ondertekenen van de gebruiker in, volg onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

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
| huurder | Vereist | De huurder directory die u wilt toestemming van aanvragen.  Beschrijvende naam of guid kan worden geleverd. |
| client_id | Vereist | De toepassings-Id dat de portal Registratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) van uw app toegewezen. |
| redirect_uri | Vereist | De redirect_uri waar u het antwoord verzonden voor uw app te verwerken.  Moet precies overeenkomen met een van de redirect_uris die u hebt geregistreerd in de portal. |
| staat | aanbevolen | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd.  Een tekenreeks met de inhoud die u kan zijn.  De status wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag is opgetreden, bijvoorbeeld een pagina of weergave op. |

Op dit moment wordt Azure AD afdwingen dat de beheerder van een huurder aanmelden kunt om de aanvraag te voltooien.  De beheerder wordt gevraagd om goed te keuren alle machtigingen die u hebt aangevraagd voor uw app in de portal voor registratie. 

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

Zodra u een positief antwoord van het eindpunt admin toestemming hebt ontvangen, kan uw app de machtigingen die daarom verzocht hebben opgedaan.  U kunt nu verplaatsen naar een token voor de gewenste bron aanvraagt, zoals hieronder beschreven.

## <a name="using-permissions"></a>Met behulp van machtigingen

Nadat de gebruiker met de machtigingen voor uw app instemt, kan uw app toegangstokens voor machtiging voor toegang tot een bron in sommige capaciteit van uw app aanschaffen.  Een bepaalde access token kan alleen worden gebruikt voor een enkele resorce, maar hierin gecodeerd worden elke machtiging die uw app voor de resource is toegekend.  Voor een toegangstoken kunt uw app een aanvraag aanbrengen in het token v2.0-eindpunt:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Het resulterende toegangstoken kan vervolgens worden gebruikt in de HTTP-aanvragen naar de bron - het wordt op betrouwbare wijze aan de resource duidelijk dat uw app de juiste machtiging heeft voor het uitvoeren van een bepaalde taak.  

Zie voor meer informatie over het protocol OAuth 2.0 en access tokens aanschaffen, [v2.0 eindpunt protocol verwijzing](active-directory-v2-protocols.md).
