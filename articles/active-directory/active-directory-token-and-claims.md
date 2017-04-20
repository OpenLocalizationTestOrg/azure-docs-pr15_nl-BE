 <properties
   pageTitle="Azure AD Token verwijzing | Microsoft Azure"
   description="Een handleiding voor het vaststellen en evalueren van de vorderingen in de SAML 2.0 en JSON Web Tokens (JWT) tokens die zijn uitgegeven door Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD tokenverwijzing

Azure Active Directory (AD Azure) genereert verschillende soorten beveiligingstokens in de verwerking van elke overdracht van verificatie. Dit document beschrijft de indeling, beveiligingskenmerken en inhoud van elk type token.

## <a name="types-of-tokens"></a>Soorten tokens

Azure AD ondersteunt het [OAuth 2.0 authorization protocol](active-directory-protocols-oauth-code.md), welke maakt gebruik van zowel access_tokens als refresh_tokens.  Ondersteunt ook verificatie en aanmelden via [OpenID verbinding](active-directory-protocols-openid-connect-code.md), die een derde type token, de id_token geïntroduceerd.  Elk van deze tokens wordt weergegeven als een token' aan toonder'.

Een token aan toonder is een lichtgewicht beveiligingstoken dat door de "houder" toegang tot een beveiligde bron verleent. In dit opzicht is de "houder" een partij die het token kan presenteren. Hoewel verificatie met Azure AD is vereist om te kunnen ontvangen van een token aan toonder, moeten maatregelen worden genomen voor het beveiligen van het token, om te voorkomen dat onderschept door een onbedoelde partij. Omdat een ingebouwd mechanisme om te voorkomen dat onbevoegden met behulp van deze tokens aan toonder niet hebt, moeten zij worden vervoerd in een beveiligd kanaal zoals transport layer security (HTTPS). Als een token aan toonder wordt verzonden in de Open, kan een man in het midden aanval worden gebruikt bij het aanschaffen van het token en ongeoorloofde toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer of aan toonder tokens voor later gebruik opslaan in de cache opslaan. Controleer altijd of uw app verzendt en aan toonder tokens worden opgeslagen op een veilige manier. Zie voor meer overwegingen over de beveiliging op tokens aan toonder, [RFC 6750 deel 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens die zijn uitgegeven door AD Azure worden geïmplementeerd als JSON Web Tokens of JWTs.  Een JWT is een compacte, veilige URL van het overbrengen van gegevens tussen twee partijen.  De informatie in de JWTs staan bekend als "claims" of verklaringen van informatie over de houder en het onderwerp van het token.  De vorderingen in JWTs zijn JSON objecten gecodeerd en serienummer voor verzending.  Aangezien de JWTs uitgegeven door AD Azure ondertekend, maar niet zijn gecodeerd, kunt u gemakkelijk de inhoud van een JWT controleren voor foutopsporing.  Er zijn verschillende hulpprogramma's beschikbaar zijn om te doen, zoals [jwt.calebb.net](http://jwt.calebb.net). Voor meer informatie over JWTs, kunt u verwijzen naar de [specificatie JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens zijn een vorm van-in-beveiligingstoken dat uw app bij het uitvoeren van verificatie met behulp van [OpenID verbinding](active-directory-protocols-openid-connect-code.md)ontvangt.  Ze worden weergegeven als [JWTs](#types-of-tokens)en vorderingen die u gebruiken kunt voor het aanmelden van de gebruiker bij uw app bevatten.  Als u naar eigen inzicht: vaak ze worden gebruikt voor het weergeven van gegevens of beslissingen access control in een app, kunt u de claims in een id_token.

Id_tokens ondertekend, maar niet op dit moment worden gecodeerd.  Wanneer uw toepassing een id_token ontvangt, moet deze [de handtekening valideren](#validating-tokens) om te bewijzen de echtheid van het token en valideren van enkele vorderingen in het token aan te tonen dat de geldigheid ervan.  De claims gevalideerd door een app variëren, afhankelijk van de vereisten voor scenario, maar er zijn enkele [gemeenschappelijke vordering validaties](#validating-tokens) die uw app in elk geval moet uitvoeren.

Zie de volgende sectie voor meer informatie op id_tokens vorderingen, alsmede een monster id_token.  Houd er rekening mee dat de vorderingen in de id_tokens niet in een bepaalde volgorde worden geretourneerd.  Bovendien nieuwe vorderingen wordt toegevoegd aan de id_tokens op elk punt in de tijd - uw app beter niet verbreken als nieuwe claims worden geïntroduceerd.  De volgende lijst bevat de claims die uw app op betrouwbare wijze op het moment van schrijven van dit interpreteren kan.  Indien nodig, nog meer details vindt u in de [specificatie OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Voorbeeld id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Probeer inspectie van de vorderingen in de id_token van het monster door plakken in [calebb.net](http://jwt.calebb.net)voor de praktijk.

#### <a name="claims-in-idtokens"></a>Vorderingen in id_tokens

| Claim JWT | Naam | Beschrijving |
|-----------|------|-------------|
| `appid`| Toepassings-ID | De toepassing waarmee het token wordt gebruikt voor toegang tot een bron wordt geïdentificeerd. De toepassing kan fungeren als zelf of namens een andere gebruiker. De toepassings-ID vertegenwoordigt meestal een application-object, maar het kan ook een service principal-object vertegenwoordigen in Azure AD. <br><br> **Voorbeeld JWT waarde**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Doelgroep | De geadresseerde van het token. De toepassing waarin het token moet controleren dat de waarde van het publiek juist is en de tokens die bestemd zijn voor een andere doelgroep afwijzen. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Voorbeeld JWT waarde**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Toepassing verificatie Context klassenverwijzing | Geeft aan hoe de client is geverifieerd. De waarde is 0 voor een openbare-client. Als de client-ID en het geheim van de client worden gebruikt, is de waarde 1. <br><br> **Voorbeeld JWT waarde**: <br> `"appidacr": "0"`|
| `acr`| Verwijzing naar de klasse van de verificatie Context | Geeft aan hoe het onderwerp is geverifieerd en de client in de claim toepassing verificatie Context klasse verwijst. Een waarde 0 (nul) geeft aan dat de verificatie van de eindgebruiker niet voldeed aan de eisen van ISO/IEC 29115. <br><br> **Voorbeeld JWT waarde**: <br> `"acr": "0"`|
| | Instant-verificatie | Registreert de datum en tijd waarop de verificatie heeft plaatsgevonden. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Verificatiemethode | Geeft aan hoe het onderwerp van het token is geverifieerd. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Voorbeeld JWT waarde**:`“amr”: ["pwd"]` |
| `given_name`| Voornaam | Biedt de eerste of "gegeven" naam van de gebruiker, zoals ingesteld op het AD Azure user-object. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"given_name": "Frank"` |
| `groups`| Groepen | Bevat de object-id's die staan voor de groepslidmaatschappen van de certificaathouder. Deze waarden zijn unieke (Zie Object-ID) en veilig kunnen worden gebruikt voor het beheer van toegang, zoals de machtiging voor toegang tot een bron af te dwingen. De groepen die zijn opgenomen in de groepen claim worden geconfigureerd op een basis per toepassing, via de eigenschap 'groupMembershipClaims' van het manifest van de toepassing. Alle groepen wordt uitgesloten door een null-waarde, een waarde van "SecurityGroup" bevat alleen uitgevoerd voor lidmaatschap van Active Directory-beveiligingsgroep en een waarde van "Alle" will beveiligingsgroepen en distributielijsten van Office 365. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | ID-Provider | De id-provider die het onderwerp van het token van de geverifieerde vastgelegd. Deze waarde is gelijk aan de waarde van de vordering van de uitgevende instelling, tenzij de gebruiker in een andere huurder dan de uitgevende instelling zijn. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Bevat de tijd waarop het token is uitgegeven. Vaak wordt gebruikt voor het meten van token versheid. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Voorbeeld JWT waarde**: <br> `"iat": 1390234181` |
| `iss` | Uitgevende instelling | Identificatie van de beveiligingstokenservice (STS) die wordt gemaakt en wordt het token. In de tokens die Azure AD als resultaat geeft, is de uitgevende instelling het sts.windows.net. De GUID in de waarde van de uitgevende instelling claim is de ID van de huurder van de map Azure AD. De ID van de huurder is een onveranderbaar en betrouwbare identificatie van de map. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Voorbeeld JWT waarde**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Achternaam | Bevat de laatste naam, voornaam of achternaam van de gebruiker zoals gedefinieerd in het gebruikersobject Azure AD. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"family_name": "Miller"` |
| `unique_name`| Naam | Biedt een mens leesbaar waarde waarmee het onderwerp van het token. Deze waarde is niet gegarandeerd uniek binnen een huurder en is ontworpen om te worden gebruikt alleen voor weergavedoeleinden. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Object-ID | Bevat een unieke id van een object in AD Azure. Deze waarde is onveranderbaar en kunnen niet worden toegewezen of opnieuw gebruikt. De object-ID gebruiken om een object in query's naar Azure AD te identificeren. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Rollen | Hiermee geeft u alle rollen van de toepassing die het onderwerp is verleend direct en indirect via een groepslidmaatschap en kan worden gebruikt voor op rollen gebaseerde toegangscontrole afdwingen. Toepassingsrollen worden gedefinieerd op basis van per toepassing via de `appRoles` eigenschap van het manifest van de toepassing. De `value` eigenschap van de toepassingsrol van elke is de waarde die wordt weergegeven in de claim rollen. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Toepassingsgebied | Geeft aan dat de imitatie-machtigingen toegekend aan de clienttoepassing. Standaard de machtiging is `user_impersonation`. De eigenaar van de beveiligde bron kunt u aanvullende waarden registreren in Azure AD. <br><br> **Voorbeeld JWT waarde**: <br> `"scp": "user_impersonation"`|
| `sub` |Onderwerp| Identificeert de aangever waarover het token wordt gesteld informatie zoals de gebruikersnaam van een toepassing. Deze waarde is onveranderbaar en kan niet opnieuw worden toegewezen of opnieuw gebruikt, zodat deze kan worden gebruikt controles uit te voeren vergunning veilig. Omdat het onderwerp altijd aanwezig in de tokens Azure AD problemen is, wordt afgeraden deze waarde in een algemene vergunning systeem te gebruiken. <br> `SubjectConfirmation`een vordering is. Hierin wordt beschreven hoe u het onderwerp van het token is geverifieerd. `Bearer`Geeft aan dat het voorwerp wordt bevestigd in hun bezit is van het token. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Voorbeeld JWT waarde**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Huurder-ID | Een onveranderbare, herbruikbare-id waarmee de huurder directory die het token uitgegeven. Deze waarde kunt u toegang krijgen tot bronnen in een toepassing met meerdere pachter huurder-specifieke directory. Bijvoorbeeld, kunt u deze waarde voor het identificeren van de huurder in een aanroep van de API van de grafiek. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Levensduur van tokens | Hiermee definieert u het tijdsinterval waarbinnen een token geldig is. De service die het token wordt gevalideerd moet controleren of de huidige datum in de levensduur van tokens, anders die moet hij het token afwijzen. De service mogelijk voor maximaal vijf minuten buiten het bereik van de levensduur van tokens voor eventuele verschillen in de kloktijd ('tijd schuintrekken") tussen de Azure AD en de service. <br><br> **Voorbeeld van de SAML-waarde**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Voorbeeld JWT waarde**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| UPN-naam | Bevat de naam van de UPN.<br><br> **Voorbeeld JWT waarde**: <br> `"upn": frankm@contoso.com`|
| `ver`| Versie | Het versienummer van het token wordt opgeslagen. <br><br> **Voorbeeld JWT waarde**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Toegangstokens

Toegangstokens zijn op dit moment alleen verbruikbare door Microsoft Services.  De apps hoeft niet voor het uitvoeren van validatie of inspectie van toegangstokens voor de ondersteunde scenario's.  Toegangstokens kan worden beschouwd als volledig dekkende - ze zijn alleen tekenreeksen die uw app aan Microsoft in HTTP-aanvragen doorgeven kunt.

Wanneer u een toegangstoken wordt aangevraagd, retourneert AD Azure ook sommige metagegevens over het toegangstoken voor consumptie van uw app.  Deze informatie omvat de verlooptijd van het toegangstoken en de scopes die geldig is.  Hierdoor is uw app voor het uitvoeren van intelligente caching van toegangstokens parseren openen zonder het toegangstoken zelf.

## <a name="refresh-tokens"></a>Tokens vernieuwen

Vernieuwen tokens worden beveiligingstokens die uw app gebruiken kunt voor het verwerven van nieuwe toegangstokens in een stroom OAuth 2.0.  Hiermee kunt uw app te verwezenlijken op lange termijn toegang tot bronnen een gebruiker zonder tussenkomst van de gebruiker.

Vernieuwen tokens zijn meerdere bron, wat betekent dat ze kunnen worden ontvangen tijdens een tokenaanvraag voor één resource, maar ingewisseld voor toegangstokens aan een andere resource. Als meerdere resource opgeven, stelt u de `resource` parameter in de aanvraag voor de desbetreffende resource.

Vernieuwen tokens zijn volledig dekkend aan uw app. Ze zijn lange levensduur, maar uw app niet verwachten dat een token vernieuwen voor een periode duurt moet worden geschreven.  Vernieuwen tokens kunnen op elk moment voor tal van redenen worden ongeldig gemaakt.  De enige manier om uw app te weten als een token vernieuwen geldig is is als u wilt dat door een aanvraag token naar Azure AD token eindpunt inwisselen.

Wanneer u een token vernieuwd voor een nieuwe toegangstoken inwisselen, ontvangt u een nieuwe beveiligingssleutel voor vernieuwen in het token antwoord.  Sla het token vernieuwen NET zijn uitgegeven, die u in de aanvraag te vervangen.  Hierdoor wordt gegarandeerd dat uw tokens vernieuwen geldig zo lang mogelijk blijven.

## <a name="validating-tokens"></a>Valideren van tokens

Het alleen token validatie van dat uw app client hoeft uit te voeren is op dit moment, id_tokens te valideren.  Te valideren in een id_token, moet het valideren van uw app zowel de handtekening van de id_token en de vorderingen in de id_token.

Wij bieden bibliotheken en codevoorbeelden die hoe gemakkelijk token validatie, tonen als u wilt weten van het onderliggende proces.  Er zijn ook verschillende bibliotheken van derden open bron beschikbaar voor de validatie van JWT, ten minste één optie voor bijna elk platform en de taal. Zie voor meer informatie over AD Azure verificatie bibliotheken en codevoorbeelden [Azure AD verificatie bibliotheken](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Verifiëren van de handtekening

Een JWT bevat drie segmenten die worden gescheiden door de `.` teken.  Het eerste segment staat bekend als de **koptekst**als **orgaan**van de tweede en de derde de **handtekening**.  Het segment van de handtekening kan worden gebruikt voor het valideren van de echtheid van het id_token zodat deze kan worden vertrouwd door uw app.

Id_Tokens zijn ondertekend met een industrie standaard asymmetrische coderingsalgoritmen, zoals RSA-256. De kop van de id_token bevat informatie over de sleutel en codering methode gebruikt voor het ondertekenen van het token:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

De `alg` claim geeft aan dat het algoritme dat werd gebruikt voor het ondertekenen van tokens, terwijl de `x5t` claim geeft aan dat de desbetreffende openbare sleutel die is gebruikt voor het ondertekenen van het token.

Azure AD mogelijk je je een id_token met een van een verzameling paren van openbare en persoonlijke sleutels op elk gewenst moment in de tijd. Azure AD draait de mogelijke reeks toetsen op een periodieke basis zodat uw app voor het automatisch verwerken van die belangrijke wijzigingen moet worden opgeslagen.  Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door AD Azure is elke 24 uur.

U kunt de ondertekening belangrijke gegevens om de handtekening te valideren met behulp van de OpenID verbinding metagegevensdocument in aanschaffen:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Probeer deze URL in een browser.

Deze metagegevensdocument is een handig stukjes informatie zoals de locatie van de verschillende eindpunten die nodig zijn voor het uitvoeren van verificatie OpenID verbinding met JSON-object.  

Het bevat ook een `jwks_uri`, waardoor de locatie van de set van openbare sleutels gebruikt voor het ondertekenen van tokens.  De JSON-document te vinden op de `jwks_uri` bevat alle informatie die openbare sleutel gebruikt die op dat moment bepaald.  Uw app kunt gebruiken de `kid` in de header JWT te selecteren welke openbare sleutel in dit document is gebruikt voor het ondertekenen van een bepaalde token claimen.  Validatie van handtekeningen met behulp van de juiste openbare sleutel en de algoritme aangegeven kan vervolgens uitvoeren.

Het valideren van een handtekening is buiten het bereik van dit document - er zijn veel open source-bibliotheken beschikbaar voor u doen indien nodig te helpen.

#### <a name="validating-the-claims"></a>Valideren van de schuldvorderingen

Als uw app een id_token bij het aanmelden van gebruiker ontvangt, het moet ook een paar voeren controles uit met de vorderingen in de id_token.  Deze omvatten maar zijn niet beperkt tot:

  - De **doelgroep** claim - te verifiëren dat de id_token is bedoeld om te worden gegeven aan uw app.
  - **Niet eerder** en **Verlooptijd** claims - om te controleren of de id_token nog niet is verstreken.
  - De **uitgevende instelling** claim - te verifiëren dat het token inderdaad aan uw app door AD Azure uitgegeven is.
  - De **Nonce** - een token replay-aanval te beperken.
  - en nog veel meer...

Raadpleeg de [specificatie OpenID verbinding](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation)voor een volledige lijst van claim validaties die uw app moet worden uitgevoerd.

Details van de verwachte waarden voor deze argumenten worden opgenomen in het voorgaande gedeelte in de [sectie id_token](#id-tokens) .

## <a name="sample-tokens"></a>Monster Tokens

Deze sectie bevat voorbeelden van SAML en JWT tokens die Azure AD als resultaat geeft. Deze voorbeelden kunnen u zien welke vorderingen in verband.
SAML-Token

Dit is een voorbeeld van een typische SAML-token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT Token - gebruikersimitatie

Dit is een voorbeeld van een typische JSON web-token (JWT) gebruikt in een stroom van autorisatie code toekennen.
Naast claims bevat het token een versienummer in **ver** - en **appidacr**, de verificatie context klassenverwijzing, waarmee wordt aangegeven hoe de client is geverifieerd. De waarde is 0 voor een openbare-client. Als een client-ID of het geheim van de client is gebruikt, is de waarde 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Verwante inhoud
- Zie de Azure AD Graph [beleidstransacties](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) en de [entiteit beleid](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity)voor meer informatie over het beheren van de levensduur van tokens beleid via de Azure AD Graph API.
- Zie voor meer informatie en voorbeelden over het beheren van beleid via PowerShell-cmdlets, met inbegrip van monsters, [configureerbare levensduur van token in Azure AD](active-directory-configurable-token-lifetimes.md). 
