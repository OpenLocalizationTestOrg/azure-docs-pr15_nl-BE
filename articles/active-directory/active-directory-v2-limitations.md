<properties
    pageTitle="v2.0 eindpunt beperkingen & beperkingen | Microsoft Azure"
    description="Een lijst met beperkingen en beperkingen aan het eindpunt Azure AD v2.0."
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

# <a name="should-i-use-the-v20-endpoint"></a>Moet ik het eindpunt v2.0 gebruiken?

Bij het bouwen van toepassingen die kunnen worden geïntegreerd met Azure Active Directory, moet u beslissen of de v2.0 eindpunt en verificatieprotocollen aan uw behoeften voldoen.  De oorspronkelijke Azure AD eindpunt wordt nog steeds volledig ondersteund en is in sommige opzichten, meer uitgebreide functionaliteit dan versie 2.0.  Echter, de v2.0 eindpunt [introduceert aanzienlijke voordelen](active-directory-v2-compare.md) voor ontwikkelaars die u met de nieuwe programmeermodel kunnen bewegen.

Op dit moment, is onze aanbeveling inzake het gebruik van het eindpunt v2.0 als volgt:

- Als u ondersteunen persoonlijke accounts van Microsoft in uw toepassing wilt, moet u het eindpunt v2.0.  Maar zorg ervoor dat de beperkingen die in dit artikel, met name die met betrekking tot de specifiek werk & school accounts.
- Als uw toepassing alleen ondersteunende werk & school-accounts vereist, moet u [de oorspronkelijke Azure AD eindpunten](active-directory-developers-guide.md).

Het eindpunt v2.0 zal na verloop van tijd toenemen tot het elimineren van de beperkingen die hier worden vermeld, zodat u alleen ooit moet u het eindpunt v2.0 gebruiken.  In de tussentijd, is dit artikel bedoeld om te bepalen als het eindpunt v2.0 voor u.  Wij blijven voor het bijwerken van dit artikel na verloop van tijd te weerspiegelen de huidige status van het eindpunt v2.0, dus controleer terug Herzie uw vereisten ten opzichte van de mogelijkheden van versie 2.0.

Als u een bestaande app met Azure AD die geen van het eindpunt v2.0 gebruikmaakt, behoeft niet helemaal.  In de toekomst, zullen we leveren een manier om uw bestaande Azure AD-toepassingen voor gebruik met het eindpunt v2.0 inschakelen.

## <a name="restrictions-on-apps"></a>Beperkingen van apps
De volgende soorten apps worden momenteel niet ondersteund door het eindpunt v2.0.  Raadpleeg [dit artikel](active-directory-v2-flows.md)voor een beschrijving van de ondersteunde soorten apps.

##### <a name="standalone-web-apis"></a>Standalone Web-API 's
Met het eindpunt v2.0 hebt u de mogelijkheid om te [bouwen van een Web-API die is beveiligd met OAuth 2.0](active-directory-v2-flows.md#web-apis).  Echter dat Web-API is alleen mogelijk voor het ontvangen van tokens uit een toepassing die u dezelfde toepassings-id deelt.  Bouwen van een web-API die toegankelijk is vanaf een client met een andere toepassing-Id wordt niet ondersteund.  Dat de client niet mogelijk aan te vragen of de machtigingen voor uw web API te verkrijgen.

Het bouwen van een Web-API die tokens van een client met dezelfde App Id accepteert, Zie de voorbeelden v2.0 eindpunt Web API in [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Web API op-rekening-Of stroom
Vele architecturen zijn een Web-API die een andere downstream Web API, beide beveiligd door het eindpunt v2.0 aanroepen.  In dit scenario is het gebruikelijk in native clients die beschikken over een Web API backend, die een online service van Microsoft of een andere maat gemaakte web API die Azure AD ondersteunt.

In dit scenario kan worden ondersteund met de subsidie OAuth 2.0 Jwt aan toonder referentie, ook bekend als de On-Behalf-Of lopen.  Echter, de stroom aan-rekening-Of is momenteel niet ondersteund voor het eindpunt v2.0.  Voor een overzicht van de werking van deze overdracht in het algemeen beschikbare Azure AD service, raadpleegt u de [op-rekening-Of codevoorbeeld op GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Beperkingen voor app-registraties
Op dit moment, moeten alle toepassingen die u wilt integreren met het eindpunt v2.0 registratie van een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)maken.  Eventuele bestaande Azure AD apps van Microsoft-Account worden niet compatibel is met het eindpunt v2.0 of wordt geregistreerd in een portal naast de nieuwe App-Portal registratie van apps.  We willen bieden een manier om bestaande toepassingen voor gebruik als een app v2.0. Op dit moment, is er geen migratiepad voor een toepassing naar het eindpunt v2.0.

Op dezelfde manier werkt geregistreerd in de nieuwe App-Portal registratie van apps ten opzichte van het oorspronkelijke eindpunt van Azure AD-verificatie.  U kunt echter apps gemaakt in de App-Portal registratie te integreren is met het eindpunt Microsoft account verificatie, gebruiken `https://login.live.com`.

Bovendien hebben de app registraties gemaakt tijdens [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) het volgende voorbehoud:

- De eigenschap **homepage** , ook bekend als **URL aanmelding** wordt niet ondersteund.  Zonder een introductiepagina, worden deze toepassingen niet weergegeven in het deelvenster MyApps Office.
- Slechts twee app geheimen zijn per aanvraag-Id op dit moment toegestaan.
- De registratie van een app kan alleen worden bekeken en beheerd door een enkele developer-account.  Het kan niet worden gedeeld tussen meerdere ontwikkelaars.
- Er zijn verschillende beperkingen van de indeling van redirect_uri toegestaan.  Zie de volgende sectie voor meer informatie.

## <a name="restrictions-on-redirect-uris"></a>Beperkingen voor Redirect URI 's
Apps die zijn geregistreerd in de nieuwe Portal registratie van toepassing zijn op dit moment beperkt tot een beperkte set van waarden voor redirect_uri.  De redirect_uri voor web apps en services moet beginnen met het schema `https`, en alle waarden van de redirect_uri moeten delen één DNS-domein.  Het is bijvoorbeeld niet mogelijk een web app met redirect_uris registreren:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Het registratiesysteem vergelijkt de volledige DNS-naam van de bestaande redirect_uri met de DNS-naam van de redirect_uri die u wilt toevoegen. De aanvraag voor het toevoegen van de DNS-naam zal mislukken als een van de volgende voorwaarden wordt voldaan:  

- Als u de volledige DNS-naam van de nieuwe redirect_uri komt niet overeen met de DNS-naam van de bestaande redirect_uri
- Als u de volledige DNS-naam van de nieuwe redirect_uri is niet een subdomein van de bestaande redirect_uri

Bijvoorbeeld, als de app op dit moment heeft redirect_uri:

`https://login.contoso.com`

Is het mogelijk om toe te voegen:

`https://login.contoso.com/new`

die exact overeenkomt met de DNS-naam of:

`https://new.login.contoso.com`

Wat is een DNS-domein van login.contoso.com.  Als u wilt dat een app die aanmelding east.contoso.com heeft en login-west.contoso.com als redirect_uris, dan moet de volgende redirect_uris toevoegen in volgorde:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

De twee laatstgenoemde kunnen worden toegevoegd, omdat ze subdomeinen van de eerste redirect_uri zijn contoso.com. Deze beperking wordt in een toekomstige versie verwijderd.

Raadpleeg meer informatie over het registreren van een app in de nieuwe Portal registratie van toepassing, [in dit artikel](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Beperkingen van de services & API 's
Het eindpunt v2.0 momenteel ondersteunt aanmelden voor elke toepassing die is geregistreerd in de nieuwe toepassing-Portal registratie, mits deze lager in de lijst met [ondersteunde verificatie stromen](active-directory-v2-flows.md).  Deze apps zal echter alleen worden OAuth 2.0 toegangstokens voor een zeer beperkt aantal middelen te verkrijgen.  Het eindpunt v2.0 verleent slechts access_tokens voor:

- De app die het token wordt aangevraagd.  Een app kan een access_token aanschaffen voor zichzelf, als de logische app is samengesteld uit meerdere verschillende onderdelen of lagen.  Uitchecken om dit scenario in actie zien, onze zelfstudie [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) .
- Outlook E-mail, agenda en contactpersonen REST API's die allemaal zich op het https://outlook.office.com bevinden.  Meer informatie over het schrijven van een toepassing die toegang heeft tot deze API's, verwijzen naar deze zelfstudies [Office aan de slag](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- De API's van Microsoft Graph.  Meer informatie over het Microsoft Graph en de gegevens die beschikbaar is, gaat u naar [https://graph.microsoft.io](https://graph.microsoft.io).

Op dit moment worden geen andere services ondersteund.  Meer Microsoft Online services worden in de toekomst en ondersteuning voor uw eigen aangepaste ingebouwde Web-API's en services toegevoegd.

## <a name="restrictions-on-libraries--sdks"></a>Beperkingen op bibliotheken en SDK 's
Ondersteuning voor het eindpunt v2.0 bibliotheek is op dit moment vrij beperkt.  Als u het eindpunt v2.0 in een productietoepassing gebruiken wilt, hebt u de volgende opties:

- Als u een webtoepassing bouwt, kunt veilig u onze algemeen beschikbare server-side middleware inloggen uitvoeren en token validatie.  Deze omvatten de middleware OWIN Open ID verbinding maken voor ASP.NET en onze plugin NodeJS Passport.  Voorbeelden van code met behulp van onze middleware zijn beschikbaar in onze ook gedeelte [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) .
- Voor andere platforms en native & mobiele toepassingen, kunt u ook integreren met het eindpunt v2.0 door rechtstreeks verzenden & protocolberichten ontvangen in uw toepassingscode.  De v2.0 verbinding OpenID en OAuth protocollen [expliciet zijn gedocumenteerd](active-directory-v2-protocols.md) om u te helpen bij het uitvoeren van een dergelijke integratie.
- Ten slotte kunt u open-source-ID verbinding openen en OAuth bibliotheken te integreren met het eindpunt v2.0.  Het protocol v2.0 moet compatibel zijn met veel bibliotheken van open-source-protocol zonder ingrijpende wijzigingen.  De beschikbaarheid van deze bibliotheken varieert per taal en platform en de [Open ID Connect](http://openid.net/connect/) en [OAuth 2.0](http://oauth.net/2/) websites een lijst bijhouden van populaire implementaties. Zie [verificatie en v2.0 bibliotheken van Azure Active Directory (AD)](active-directory-v2-libraries.md) voor meer details en de lijst van open-source clientbibliotheken en monsters die zijn getest met het eindpunt v2.0.

We hebben ook een eerste voorbeeld van de [Bibliotheek voor Microsoft-verificatie (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) voor uitgebracht .NET alleen.  U bent Welkom bij het uitproberen van deze bibliotheek in .NET client- en servertoepassingen, maar als zij niet vergezeld van NH kwaliteit voorbeeld bibliotheek ondersteund.

## <a name="restrictions-on-protocols"></a>Beperkingen van de protocollen
Het eindpunt v2.0 biedt alleen ondersteuning voor Open-ID kunnnen OAuth 2.0.  Echter niet alle functies en mogelijkheden van elk protocol zijn opgenomen in het eindpunt v2.0, met inbegrip van:

- De verbinding van de OpenID `end_session_endpoint`, waardoor een app tot het einde van de sessie van de gebruiker aan het eindpunt v2.0.
- id_tokens, uitgegeven door het eindpunt v2.0 bevatten alleen een paarsgewijze id voor de gebruiker.  Dit betekent dat twee verschillende toepassingen verschillende id's voor dezelfde gebruiker ontvangt.  Merk op dat een query uitgevoerd op de Microsoft Graph `/me` eindpunt, is het mogelijk om een correlatable-ID voor de gebruiker die door toepassingen kan worden gebruikt.
- id_tokens, uitgegeven door het eindpunt v2.0 hebben niet een `email` claim voor de gebruiker op dit moment, zelfs als u toestemming van de gebruiker weer te geven van hun e-mailadres aan te schaffen.
- Het eindpunt van de OpenID verbinding gebruikersinfo. Het eindpunt van gebruikersgegevens is niet geïmplementeerd op het eindpunt v2.0 op dit moment.  Echter alle gebruikersprofielgegevens ontvangt u mogelijk op dit eindpunt is beschikbaar via het Microsoft Graph `/me` eindpunt.
- Rol & groepclaims.  Op dit moment is ondersteund het eindpunt v2.0 afgifte rol of groep vorderingen in id_tokens niet.

Lees onze [verbinding OpenID en OAuth 2.0 Protocol verwijzing](active-directory-v2-protocols.md)voor een beter begrip van de reikwijdte van protocol-functionaliteit ondersteund in het eindpunt v2.0.

## <a name="restrictions-for-work--school-accounts"></a>Beperkingen voor de school & werken
Er zijn een aantal functies die specifiek zijn voor Microsoft organisatie/zakelijke gebruikers die nog niet worden ondersteund door het eindpunt v2.0.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Voorwaardelijke toegang op basis van het apparaat, native en mobiele apps en de Microsoft Graph
Het eindpunt v2.0 ondersteunt nog geen verificatie apparaat voor mobiel en interne toepassingen, zoals native apps op iOS of Android.  Dit kan uw eigen toepassing aanroepen van de Microsoft Graph voor bepaalde organisaties blokkeren.  Apparaat-verificatie is vereist wanneer een beheerder een beleid van voorwaardelijke toegang op basis van het apparaat in een toepassing wordt.  Voor het eindpunt v2.0 is het meest waarschijnlijke geval van voorwaardelijke toegang op basis van het apparaat een beheerder een beleid instellen voor een resource in het Microsoft Graph, zoals de Outlook-API.  Als een beheerder dit beleid wordt ingesteld en de oorspronkelijke toepassing vraagt om een token voor het Microsoft Graph, mislukt de aanvraag uiteindelijk omdat verificatie apparaat wordt nog niet ondersteund.  Webtoepassingen die tokens naar de Microsoft Graph aanvragen echter ondersteund bij het beleid op basis van het apparaat zijn geconfigureerd.  In het web app scenario apparaat verificatie uitgevoerd via de webbrowser van de gebruiker.

Als ontwikkelaar hebt u waarschijnlijk geen zeggenschap hebben over wanneer beleid wordt ingesteld op Microsoft Graph bronnen, of dat het geval.  Als u een toepassing voor gebruikers van werk en school, moet u [het oorspronkelijke AD Azure eindpunt](active-directory-developers-guide.md) totdat het eindpunt v2.0 verificatie apparaat ondersteunt gebruiken.  Raadpleeg [dit artikel](active-directory-conditional-access.md#device-based-conditional-access)voor meer informatie over voorwaardelijke toegang op basis van het apparaat.

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Geïntegreerde Windows-verificatie voor federatieve huurders
Als u ADAL (en dus de oorspronkelijke Azure AD eindpunt) in Windows-toepassingen gebruikt hebt, kan u voordeel hebben gehaald uit wat de SAML assertion subsidie wordt genoemd.  Deze machtiging kan gebruikers van federatieve Azure AD huurders zonder bericht verifiëren met hun lokale Active Directory-instantie zonder dat hun referenties op te geven.  De SAML assertion subsidie wordt momenteel niet ondersteund op het eindpunt v2.0.
