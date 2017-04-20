<properties
    pageTitle="B2C Azure Active Directory | Microsoft Azure"
    description="De soorten tokens in de B2C van Azure Active Directory uitgegeven."
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


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Token verwijzing

Azure Active Directory (AD Azure) B2C zendt verschillende soorten beveiligingstokens tijdens de verwerking van elke [overdracht van verificatie](active-directory-b2c-apps.md). Dit document beschrijft de indeling, beveiligingskenmerken en inhoud van elk type token.

## <a name="types-of-tokens"></a>Soorten tokens

Azure AD B2C ondersteunt het [OAuth 2.0 authorization protocol](active-directory-b2c-reference-protocols.md), welke maakt gebruik van zowel access tokens en tokens vernieuwen. Het biedt ook ondersteuning voor verificatie en aanmelden via [OpenID verbinding](active-directory-b2c-reference-protocols.md), die een derde type token introduceert: de ID-token. Elk van deze tokens wordt weergegeven als een token aan toonder.

Een token aan toonder is een lichtgewicht beveiligingstoken dat door de "houder" toegang tot een beveiligde bron verleent. De drager is een partij die het token kan presenteren. Azure AD een partij moet eerst worden geverifieerd voordat het een token aan toonder kan ontvangen. Maar als de vereiste stappen niet voor het beveiligen van de token in de overdracht en opslag zijn genomen, kunnen worden onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens hebben een ingebouwd mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, maar aan toonder tokens hoeft dit mechanisme niet. Zij moeten worden vervoerd in een beveiligd kanaal, zoals transport layer security (HTTPS).

Als een token aan toonder buiten een beveiligd kanaal wordt verzonden, kunnen een kwaadwillende partij een man-in-the-middle-aanval kunt gebruiken om het token verkrijgen en gebruiken om onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes toepassen als aan toonder tokens worden opgeslagen of voor later gebruik opgeslagen. Controleer altijd of uw app verzendt en aan toonder tokens worden opgeslagen op een veilige manier.

Zie [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750)voor extra beveiligingsoverwegingen op tokens aan toonder.

Veel van de tokens die Azure AD B2C problemen worden geïmplementeerd als JSON web tokens (JWTs). Een JWT is een compacte, veilige URL van het overbrengen van gegevens tussen twee partijen. JWTs bevatten gegevens die claims genoemd. Dit zijn bevestigingen van informatie over de houder en het onderwerp van het token. De vorderingen in JWTs zijn JSON-objecten die zijn gecodeerd en serienummer voor verzending. Omdat de JWTs uitgegeven door Azure AD B2C zijn ondertekend maar niet zijn gecodeerd, kunt u eenvoudig controleren de inhoud van een JWT op te sporen. Er zijn verschillende hulpprogramma's beschikbaar die hiervoor, met inbegrip van [calebb.net](http://calebb.net). Raadpleeg voor meer informatie over JWTs, [JWT specificaties](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID tokens

Een ID-token is een vorm van het beveiligingstoken dat uw app van de B2C Azure AD ontvangt `authorize` en `token` eindpunten. ID-tokens worden weergegeven als [JWTs](#types-of-tokens)en vorderingen die u gebruiken kunt voor het identificeren van gebruikers in uw app bevatten. Als id-tokens worden verkregen uit de `authorize` eindpunt, ze worden vaak gebruikt voor het aanmelden van gebruikers voor webtoepassingen. Als id-tokens worden verkregen uit de `token` eindpunt, ze kunnen worden verzonden in een HTTP-aanvragen tijdens de communicatie tussen twee onderdelen van dezelfde toepassing of service. U kunt de claims in een token ID wens naar. Ze worden vaak gebruikt om gegevens weer te geven of access control om beslissingen te nemen in een app.  

ID-tokens worden ondertekend, maar ze op dit moment niet zijn gecodeerd. Als uw app of API ontvangt een ID-token, moet dit [de handtekening valideren](#token-validation) om te bewijzen dat het token authentiek is. Uw app of API moet ook controleren van enkele vorderingen in het token aan te tonen dat geldig is. Claims gevalideerd door een app kunnen variëren afhankelijk van het scenario, maar uw app sommige [gemeenschappelijke vordering validaties](#token-validation) in elk scenario moet uitvoeren.

#### <a name="sample-id-token"></a>Voorbeeld-id-token
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Toegangstokens

Een toegangstoken is ook een vorm van het beveiligingstoken dat uw app van de B2C Azure AD ontvangt `authorize` en `token` eindpunten. Toegangstokens worden ook weergegeven als [JWTs](#types-of-tokens)en vorderingen die u gebruiken kunt voor het identificeren van gebruikers in uw webservices & API's bevatten.

Toegangstokens worden ondertekend, maar niet zijn gecodeerd op dit moment- en vergelijkbaar met het id-tokens.  Toegangstokens moeten worden gebruikt voor toegang tot de webservices en API's te identificeren en verificatie van de gebruiker in deze services.  Ze bieden echter niet de bewering van de vergunning op deze diensten.  Dat wil zeggen dat de `scp` in de identiteitsclaim van toegangstokens niet beperken of anderszins vertegenwoordigen de toegang die het onderwerp van het token is toegekend.

Wanneer de API een toegangstoken ontvangt, moet deze [de handtekening valideren](#token-validation) om te bewijzen dat het token authentiek is. De API moet ook valideren van enkele vorderingen in het token aan te tonen dat geldig is. Claims gevalideerd door een app kunnen variëren afhankelijk van het scenario, maar uw app sommige [gemeenschappelijke vordering validaties](#token-validation) in elk scenario moet uitvoeren.

### <a name="claims-in-id--access-tokens"></a>Vorderingen in ID & toegangstokens

Wanneer u Azure AD B2C gebruikt, hebt u verfijnde controle over de inhoud van uw tokens. U kunt [beleid](active-directory-b2c-reference-policies.md) voor het verzenden van bepaalde sets van gebruikersgegevens in schuldvorderingen waarvoor uw app voor haar activiteiten. Deze vorderingen kunnen standaardeigenschappen zoals van de gebruiker bevatten `displayName` en `emailAddress`. Ze kunnen ook [aangepaste kenmerken](active-directory-b2c-reference-custom-attr.md) die u in de B2C-map definiëren kunt. Elke ID & access token die u ontvangt bevat een set claims-beveiliging. Uw toepassingen kunt deze claims veilig verifiëren van gebruikers en aanvragen.

Houd er rekening mee dat de vorderingen in tokens ID niet in een bepaalde volgorde worden geretourneerd. Bovendien kunnen u nieuwe vorderingen geïntroduceerd in ID tokens op elk gewenst moment. Uw app beter niet verbreken als nieuwe claims worden geïntroduceerd. Hier worden de claims die u verwacht in ID & access tokens die zijn uitgegeven door Azure AD B2C bestaan. Geen extra claims wordt bepaald door het beleid. Probeer inspectie van de claims in het token monster-ID door te plakken in de [calebb.net](http://calebb.net)voor de praktijk. Meer informatie vindt u in de [specificatie OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html).

| Naam | Claim | Van de Voorbeeldwaarde | Beschrijving |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Doelgroep | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Een claim publiek identificeert de bedoelde ontvanger van het token. Voor AD-B2C Azure is de doelgroep van uw app toepassings-ID, zoals toegewezen aan uw app in de app-portal registratie. Uw app moet deze waarde te valideren en weigeren het token als niet aan. |
| Uitgevende instelling | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Dit argument geeft de beveiligingstokenservice (STS) die wordt gemaakt en wordt het token. Ook worden de Azure AD-map waarin de gebruiker is geverifieerd. Uw app moet valideren de claim van de uitgevende instelling om ervoor te zorgen dat de token van het eindpunt v2.0 afkomstig is. |
| Uitgegeven op | `iat` | `1438535543` | Deze claim is de tijd waarop het token is uitgegeven, vertegenwoordigd in de tijd van epoch. |
| Verlooptijd | `exp` | `1438539443` | De verlooptijd claim is het tijdstip waarop het token ongeldig, wordt weergegeven in epoch tijd. Uw app moet dit argument gebruiken om de geldigheid van de levensduur van tokens.  |
| Niet vóór | `nbf` | `1438535543` | Deze claim is het tijdstip waarop het token geldig, in tijd epoche vertegenwoordigde wordt. Dit is meestal hetzelfde als de tijd die het token is uitgegeven. Uw app moet dit argument gebruiken om de geldigheid van de levensduur van tokens.  |
| Versie | `ver` | `1.0` | Dit is de versie van het token ID die door AD Azure. |
| Code-hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Een hash code is opgenomen in een token ID alleen als het token is uitgegeven met een autorisatiecode OAuth 2.0. Een hash code kan worden gebruikt voor het valideren van de echtheid van een code van de vergunning. Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie over het uitvoeren van deze validatie. |
| Access token-hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Een access-token-hash is opgenomen in een token ID alleen als het token met een 2.0 OAuth-toegangstoken is uitgegeven. Een token hash toegang kan worden gebruikt voor het valideren van de echtheid van een toegangstoken. Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie over het uitvoeren van deze validatie. |
| Nonce | `nonce` | `12345` | Een nonce is een strategie die wordt gebruikt voor het token replay-aanvallen te verminderen. Uw app een nonce kunt opgeven in een aanvraag voor een vergunning met behulp van de `nonce` queryparameter. De waarde die u in de aanvraag opgeeft worden verzonden niet worden gewijzigd de `nonce` van een ID-token alleen aanspraak. Hiermee kunt uw app om te controleren of de waarde ten opzichte van de waarde voor die het opgegeven op de aanvraag, die de sessie van de toepassing worden gekoppeld aan een bepaalde ID-token. Deze validatie tijdens de ID token validatie moet worden uitgevoerd door uw app. |
| Onderwerp | `sub` | `Not supported currently. Use oid claim.` | Dit is een principal waarover het token wordt gesteld informatie zoals de gebruikersnaam van een app. Deze waarde is onveranderbaar en kunnen niet worden toegewezen of opnieuw gebruikt. Deze kan controles uit te voeren vergunning veilig, zoals wanneer het token wordt gebruikt voor toegang tot een bron worden gebruikt. Echter is het argument onderwerp nog niet geïmplementeerd in de B2C Azure AD. U dient uw beleid definiëren om te bevatten de object-ID `oid` claimen en de waarde ervan gebruiken om gebruikers te identificeren, in plaats het onderwerp verzoek om toelating. |
| Verwijzing naar de klasse van de verificatie context | `acr` | `b2c_1_sign_in` | Dit is de naam van het beleid dat is gebruikt voor de verwerving van het ID-token.  |
| Tijd voor verificatie | `auth_time` | `1438535543` | Dit verzoek is de tijd waarop een laatste ingevoerde gebruikersreferenties, in de tijd epoche vertegenwoordigd. |


### <a name="refresh-tokens"></a>Tokens vernieuwen

Vernieuwen tokens worden beveiligingstokens die uw app gebruiken kunt bij het aanschaffen van nieuwe ID tokens en access tokens in een stroom OAuth 2.0. Zij bieden uw app op lange termijn toegang tot bronnen voor gebruikers zonder tussenkomst van gebruikers.

Een vernieuwingsbewerking ontvangt in een token antwoord token uw app moet aanvragen het `offline_acesss` bereik. Voor meer informatie over de `offline_access` bereik, raadpleegt u de [verwijzing naar Azure AD B2C-protocol](active-directory-b2c-reference-protocols.md).

Tokens te vernieuwen, en altijd zullen zijn, uw app volledig dekkend. Worden uitgegeven door AD Azure kunnen en worden gecontroleerd en alleen door AD Azure geïnterpreteerd. Ze zijn lange levensduur, maar uw app moet niet worden geschreven met de verwachting die een token vernieuwd voor een bepaalde periode loopt. Vernieuwen tokens kunnen te allen tijde voor tal van redenen worden ongeldig gemaakt. De enige manier om uw app te weten als een token vernieuwen geldig is is als u wilt dat door een aanvraag token naar Azure AD inwisselen.

Wanneer je een token vernieuwd voor een nieuwe beveiligingssleutel (en als uw app is voorzien van de `offline_access` bereik), ontvangt u een nieuwe beveiligingssleutel voor vernieuwen in het token antwoord. Sla het token vernieuwen NET zijn uitgegeven. Het vervangt het vernieuwen token dat u eerder hebt gebruikt in de aanvraag. Dit helpt te garanderen dat uw tokens vernieuwen geldig zo lang mogelijk blijven.

## <a name="token-validation"></a>Token validatie

Voor het valideren van een token, Controleer uw app in de handtekening en de vorderingen van het token.

Veel open source-bibliotheken zijn beschikbaar voor het valideren van JWTs, afhankelijk van uw voorkeurstaal. Wij raden aan dat u deze opties verkennen in plaats van uw eigen validatielogica te implementeren. De informatie in deze handleiding kunt u meer informatie over het correct gebruik van deze bibliotheken.

### <a name="validate-the-signature"></a>De handtekening valideren
Een JWT bevat drie segmenten, gescheiden door de `.` teken. Het eerste segment van de **kop**is, de tweede is de **instantie**en het derde is de **handtekening**. Het segment van de handtekening kan worden gebruikt voor het valideren van de echtheid van het token zodat deze kan worden vertrouwd door uw app.

Azure AD B2C-tokens zijn ondertekend met behulp van industriestandaard asymmetrische coderingsalgoritmen, zoals RSA-256. De kop van het token bevat informatie over de sleutel en codering methode gebruikt voor het ondertekenen van het token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

De `alg` claim geeft aan dat het algoritme dat werd gebruikt voor het ondertekenen van het token. De `kid` claim geeft aan dat de desbetreffende openbare sleutel die is gebruikt voor het ondertekenen van het token.

Op een bepaald moment kan Azure AD een token met behulp van een verzameling paren van openbare en persoonlijke sleutels een ondertekenen. Azure AD draait de mogelijke set sleutels voor regelmatig, zodat uw app voor het automatisch verwerken van die belangrijke wijzigingen moet worden opgeslagen. Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door AD Azure is elke 24 uur.

Azure AD B2C heeft een eindpunt metagegevens OpenID verbinden. Zo kunt apps voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat de eindpunten, inhoud van tokens en token-ondertekening van sleutels. De B2C-map bevat een document JSON metagegevens voor elk beleid. Bijvoorbeeld de metagegevensdocument voor de `b2c_1_sign_in` beleid in de `fabrikamb2c.onmicrosoft.com` bevindt zich op:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`de B2C-map die wordt gebruikt voor het verifiëren van de gebruiker, en `b2c_1_sign_in` is het beleid dat wordt gebruikt voor de verwerving van het token. Om te bepalen welk beleid is gebruikt voor het ondertekenen van een token (en waar u de metagegevens ophalen), hebt u twee opties. Eerst de naam van het beleid is opgenomen in de `acr` claim in het token. Door base-64, de berichttekst te decoderen en deserialisatie de JSON-tekenreeks die het resultaat kunt u de vorderingen van de hoofdtekst van de JWT parseren. De `acr` claim moet de naam van het beleid dat is gebruikt voor het uitgeven van het token.  De andere optie is voor het coderen van het beleid in de waarde van de `state` parameter wanneer u het verzoek te verzenden, en vervolgens decoderen om te bepalen welk beleid is gebruikt. De methode is geldig.

De metagegevensdocument is een JSON-object dat handig stukjes informatie bevat. Deze omvatten de locatie van de eindpunten voor het uitvoeren van verificatie OpenID verbinding vereist. Ze bevatten ook `jwks_uri`, waardoor de locatie van de set van openbare sleutels die worden gebruikt voor het ondertekenen van tokens. Die locatie u hier vindt, maar is het beter om de locatie dynamisch ophalen met behulp van de metagegevensdocument en parseren `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

De JSON-document te vinden op deze URL bevat alle van de openbare-sleutelgegevens in gebruik op een bepaald moment. Uw app kunt gebruiken de `kid` in de header JWT te selecteren van de openbare sleutel in het JSON-document dat wordt gebruikt voor het ondertekenen van een bepaalde token claimen. Validatie van handtekeningen kan vervolgens uitvoeren met behulp van de juiste openbare sleutel en de algoritme aangegeven.

Een beschrijving van het uitvoeren van verificatie van handtekening is buiten het bereik van dit document. Veel open source-bibliotheken zijn beschikbaar om u te helpen met deze als u deze nodig hebt.

### <a name="validate-the-claims"></a>Valideren van de schuldvorderingen
Wanneer uw app of API ontvangt een ID-token, moet deze ook verschillende controles met de vorderingen in het token ID uitvoeren. Deze omvatten, maar zijn niet beperkt tot:

- De claim van de **doelgroep** : Hiermee wordt gecontroleerd dat de ID-token is bestemd voor uw app.
- De claims **niet vóór** en **Verloopdatum** : deze controleren of de ID-token nog niet is verstreken.
- Het verzoek van de **uitgevende instelling** : Hiermee wordt het token is uitgegeven aan uw app door AD Azure gecontroleerd.
- De **nonce**: dit is een strategie voor de beperking van token replay-aanval.

Raadpleeg de [specificatie OpenID verbinding](https://openid.net)voor een volledige lijst van validaties die uw app moet worden uitgevoerd. Details van de verwachte waarden voor deze argumenten worden opgenomen in de voorgaande [sectie token](#types-of-tokens).  

## <a name="token-lifetimes"></a>Levensduur van token

De volgende token levensduur zijn bedoeld om uw kennis verder. Zij kunnen u helpen bij het ontwikkelen en debuggen van apps. Houd er rekening mee dat uw toepassingen niet worden geschreven in te verwachten van deze levensduur constant blijft. Ze kunnen en wordt gewijzigd.  U kunt meer lezen over het aanpassen van de levensduur van token in Azure AD B2C [hier](active-directory-b2c-token-session-sso.md).

| Token | Levensduur | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| ID tokens | Een uur | ID-tokens zijn meestal geldig gedurende een uur. Uw web app kunt deze levensduur eigen sessies onderhouden met gebruikers (aanbevolen). U kunt ook de levensduur van een andere sessie. Als uw app een nieuwe ID token ophalen moet, hoeft deze alleen maar een nieuwe aanvraag-in om aan te brengen Azure AD. Als een gebruiker een geldige browsersessie met Azure Active Directory, kan die gebruiker niet worden vereist referenties opnieuw invoeren. |
| Tokens vernieuwen | Maximaal 14 dagen | Een token één vernieuwen is geldig voor maximaal 14 dagen. Een token vernieuwen kan echter worden ongeldig op elk gewenst moment voor een aantal redenen. Uw app moet probeert te gebruiken een token vernieuwd totdat de aanvraag niet gehonoreerd, of uw app vervangt het token vernieuwen met een nieuwe blijven.  Een token vernieuwen kan ook ongeldig als de 90 dagen is verstreken sinds de gebruiker referenties laatst ingevoerd. |
| Codes van de vergunning | Vijf minuten | Codes van de vergunning zijn opzettelijk tijdelijk. Zij moeten worden ingewisseld onmiddellijk voor toegangstokens, ID tokens of tokens vernieuwen wanneer ze worden ontvangen. |
