<properties
    pageTitle="Azure AD v2.0 token verwijzing | Microsoft Azure"
    description="De soorten tokens en claims uitgestoten door het eindpunt v2.0"
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

# <a name="v20-token-reference"></a>tokenverwijzing v2.0

Het eindpunt v2.0 zendt verschillende soorten beveiligingstokens in de verwerking van elke [overdracht van verificatie](active-directory-v2-flows.md). Dit document beschrijft de indeling, beveiligingskenmerken en inhoud van elk type token.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Soorten tokens

Het eindpunt v2.0 ondersteunt het [OAuth 2.0 authorization protocol](active-directory-v2-protocols.md), welke maakt gebruik van zowel access_tokens als refresh_tokens.  Ondersteunt ook verificatie en aanmelden via [OpenID verbinding](active-directory-v2-protocols.md#openid-connect-sign-in-flow), die een derde type token, de id_token geïntroduceerd.  Elk van deze tokens wordt weergegeven als een token' aan toonder'.

Een token aan toonder is een lichtgewicht beveiligingstoken dat door de "houder" toegang tot een beveiligde bron verleent. In dit opzicht is de "houder" een partij die het token kan presenteren. Als een partij moet eerst worden geverifieerd met Azure Active Directory voor het ontvangen van de token aan toonder als worden niet de vereiste stappen voor het beveiligen van de token in de overdracht en opslag is genomen, kan het worden onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens beschikken over een ingebouwd mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, kunnen aan toonder tokens hoeft dit mechanisme niet en moeten worden vervoerd in een beveiligd kanaal zoals transport layer security (HTTPS). Als een token aan toonder wordt verzonden in de Open, kan een man in het midden aanval worden gebruikt door een kwaadwillende partij de token verkrijgen en het gebruik voor een onbevoegde toegang tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer of aan toonder tokens voor later gebruik opslaan in de cache opslaan. Controleer altijd of uw app verzendt en aan toonder tokens worden opgeslagen op een veilige manier. Zie voor meer overwegingen over de beveiliging op tokens aan toonder, [RFC 6750 deel 5](http://tools.ietf.org/html/rfc6750).

Veel van de door het eindpunt v2.0 uitgegeven tokens worden geïmplementeerd als Json Web Tokens of JWTs.  Een JWT is een compacte, veilige URL van het overbrengen van gegevens tussen twee partijen.  De informatie in de JWTs staan bekend als "claims" of verklaringen van informatie over de houder en het onderwerp van het token.  De vorderingen in JWTs zijn JSON objecten gecodeerd en serienummer voor verzending.  Aangezien de JWTs afgegeven door het eindpunt v2.0 ondertekend, maar niet zijn gecodeerd, kunt u gemakkelijk de inhoud van een JWT controleren voor foutopsporing. Voor meer informatie over JWTs, kunt u verwijzen naar de [specificatie JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens zijn een vorm van-in-beveiligingstoken dat uw app bij het uitvoeren van verificatie met behulp van [OpenID verbinding](active-directory-v2-protocols.md#openid-connect-sign-in-flow)ontvangt.  Ze worden weergegeven als [JWTs](#types-of-tokens)en vorderingen die u gebruiken kunt voor het aanmelden van de gebruiker bij uw app bevatten.  Als u naar eigen inzicht: vaak ze worden gebruikt voor het weergeven van gegevens of beslissingen access control in een app, kunt u de claims in een id_token.  Het eindpunt v2.0 problemen slechts één type id_token met een consistente set van vorderingen, ongeacht het type van de gebruiker die is aangemeld.  Dat wil zeggen dat de indeling en inhoud van de id_tokens zal de gelijk voor beide persoonlijke Microsoft-Account gebruikers en accounts voor werk of school is.

Id_tokens ondertekend, maar niet op dit moment worden gecodeerd.  Wanneer uw toepassing een id_token ontvangt, moet deze [de handtekening valideren](#validating-tokens) om te bewijzen de echtheid van het token en valideren van enkele vorderingen in het token aan te tonen dat de geldigheid ervan.  De claims gevalideerd door een app variëren, afhankelijk van de vereisten voor scenario, maar er zijn enkele [gemeenschappelijke vordering validaties](#validating-tokens) die uw app in elk geval moet uitvoeren.

Alle details over de vorderingen in de id_tokens hieronder, maar ook een id_token monster.  Houd er rekening mee dat de vorderingen in de id_tokens niet in een bepaalde volgorde worden geretourneerd.  Bovendien nieuwe vorderingen wordt toegevoegd aan de id_tokens op elk punt in de tijd - uw app beter niet verbreken als nieuwe claims worden geïntroduceerd.  De volgende lijst bevat de claims die uw app op betrouwbare wijze op het moment van schrijven van dit interpreteren kan.  Indien nodig, nog meer details vindt u in de [specificatie OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Voorbeeld id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Probeer inspectie van de vorderingen in de id_token van het monster door plakken in [calebb.net](https://calebb.net)voor de praktijk.

#### <a name="claims-in-idtokens"></a>Vorderingen in id_tokens
| Naam | Claim | Van de Voorbeeldwaarde | Beschrijving |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Doelgroep | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Identificeert de bedoelde ontvanger van het token.  In id_tokens is de doelgroep van uw app toepassings-Id, zoals toegewezen aan uw app in de app-portal registratie.  Uw app moet deze waarde te valideren en weigeren het token als niet aan. |
| Uitgevende instelling | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Identificatie van de beveiligingstokenservice (STS) die wordt gemaakt en geeft als resultaat het token zoals de Azure AD huurder waarin de gebruiker is geverifieerd.  Uw app moet valideren de claim van de uitgevende instelling om ervoor te zorgen dat de token van het eindpunt v2.0 afkomstig is.  Het moet ook de guid-gedeelte van de vordering en Beperk het aantal huurders die mogen inloggen op de app gebruiken.  De guid die de gebruiker aangeeft is een consument gebruiker van Microsoft-account is `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Uitgegeven op | `iat` | `1452285331` | De tijd waarop het token is uitgegeven, vertegenwoordigd in de tijd epoche. |
| Verlooptijd | `exp` | `1452289231` | Het tijdstip waarop het token ongeldig, wordt weergegeven in epoch tijd.  Uw app moet dit argument gebruiken om de geldigheid van de levensduur van tokens.  |
| Niet vóór | `nbf` | `1452285331` |  Het tijdstip waarop het token geldig is, wordt weergegeven in epoch tijd. Het is meestal hetzelfde als het moment van uitgifte.  Uw app moet dit argument gebruiken om de geldigheid van de levensduur van tokens.  |
| Versie | `ver` | `2.0` | De versie van de id_token, zoals gedefinieerd door AD Azure.  Voor het eindpunt v2.0 de waarde is `2.0`. |
| Huurder-Id | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Een guid die de Azure AD pachters die afkomstig is van de gebruiker.  De guid is voor school- en werk de huurder onveranderbare-ID van de organisatie waartoe de gebruiker behoort.  Voor persoonlijke rekeningen, de waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`.  De `profile` bereik is vereist voor het ontvangen van deze claim. |
| Code-Hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | De hash code is opgenomen in de id_tokens alleen als de id_token naast een autorisatiecode OAuth 2.0 wordt uitgegeven.  Het kan worden gebruikt voor het valideren van de echtheid van een code van de vergunning.  Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie over het valideren van dit. |
| Access Token-Hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | De access token hash wordt opgenomen in id_tokens alleen bij de id_token naast een 2.0 OAuth-toegangstoken is uitgegeven.  Het kan worden gebruikt voor het valideren van de echtheid van een toegangstoken.  Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie over het valideren van dit. |
| Nonce | `nonce` | `12345` | De nonce is een strategie voor token replayaanvallen te beperken.  Uw app een nonce kunt opgeven in een aanvraag voor een vergunning met behulp van de `nonce` queryparameter.  De waarde die u in de aanvraag opgeeft zal worden uitgestoten in de id_token van `nonce` claim, niet gewijzigd.  Hiermee kunt uw app om te controleren of de waarde ten opzichte van de waarde voor die het opgegeven op de aanvraag, die de sessie van de toepassing worden gekoppeld aan een bepaalde id_token.  Deze validatie tijdens de id_token validatie moet worden uitgevoerd door uw app. |
| Naam | `name` | `Babe Ruth` | De claim biedt een mens leesbaar waarde waarmee het onderwerp van het token. Deze waarde is niet gegarandeerd uniek is veranderlijke en is ontworpen om te worden gebruikt alleen voor weergavedoeleinden.  De `profile` bereik is vereist voor het ontvangen van deze claim. |
| E-mail | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Het primaire e-mailadres dat is gekoppeld aan de gebruikersaccount, indien aanwezig.  De waarde ervan is veranderlijke en na verloop van tijd voor een bepaalde gebruiker kan wijzigen.  De `email` bereik is vereist voor het ontvangen van deze claim. |
| Gewenste gebruikersnaam | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | De primaire gebruikersnaam die wordt gebruikt voor de gebruiker in het eindpunt v2.0.  Is het mogelijk een e-mailadres, telefoonnummer of een algemene gebruikersnaam zonder een opgegeven indeling.  De waarde ervan is veranderlijke en na verloop van tijd voor een bepaalde gebruiker kan wijzigen.  De `profile` bereik is vereist voor het ontvangen van deze claim. |
| Onderwerp | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | De aangever waarover het token wordt gesteld informatie zoals de gebruikersnaam van een app. Deze waarde is onveranderbaar en kan niet opnieuw worden toegewezen of opnieuw gebruikt, zodat deze kan worden gebruikt controles uit te voeren vergunning veilig, zoals wanneer het token wordt gebruikt voor toegang tot een bron. Omdat het onderwerp altijd aanwezig in de tokens Azure AD problemen is, wordt afgeraden deze waarde in een algemene vergunning systeem te gebruiken. |
| Object-id | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | De object-Id van het werk of school in het systeem Azure AD.  Deze claim wordt niet verleend voor persoonlijke accounts van Microsoft.  De `profile` bereik is vereist voor het ontvangen van deze claim. |


## <a name="access-tokens"></a>Toegangstokens

Toegangstokens, uitgegeven door het eindpunt v2.0 zijn op dit moment alleen verbruikbare door Microsoft Services.  De apps hoeft niet voor het uitvoeren van validatie of inspectie van toegangstokens voor de ondersteunde scenario's.  Toegangstokens kan worden beschouwd als volledig dekkende - ze zijn alleen tekenreeksen die uw app aan Microsoft in HTTP-aanvragen doorgeven kunt.

Het eindpunt v2.0 zal in de nabije toekomst de mogelijkheid om uw app toegangstokens ontvangen van andere clients brengen.  Deze informatie wordt op dat moment bijgewerkt met de gegevens die uw app moet access token validatie en andere vergelijkbare taken uitvoeren.

Wanneer u een toegangstoken bij het eindpunt v2.0 aanvraagt, retourneert het eindpunt v2.0 ook sommige metagegevens over het toegangstoken voor consumptie van uw app.  Deze informatie omvat de verlooptijd van het toegangstoken en de scopes die geldig is.  Hierdoor is uw app voor het uitvoeren van intelligente caching van toegangstokens parseren openen zonder het toegangstoken zelf.

## <a name="refresh-tokens"></a>Tokens vernieuwen

Vernieuwen van tokens worden beveiligingstokens die uw app gebruiken kunt voor het verwerven van nieuwe access tokens in een stroom OAuth 2.0.  Hiermee kunt uw app te verwezenlijken op lange termijn toegang tot bronnen een gebruiker zonder tussenkomst van de gebruiker.

Vernieuwen tokens zijn meerdere resources.  Dat wil zeggen worden dat een vernieuwen token ontvangen tijdens een tokenaanvraag voor één resource kan ingewisseld voor toegangstokens aan een andere resource.

Vernieuwen in een token antwoord ontvangen, uw app moet aanvragen & verleend de `offline_acesss` bereik.   Voor meer informatie over de `offline_access` bereik, raadpleegt u de [toestemming & scopes artikel hier](active-directory-v2-scopes.md).

Tokens te vernieuwen, en altijd zullen zijn, uw app volledig dekkend.  Ze kunnen worden uitgegeven door het eindpunt Azure AD v2.0 en alleen worden gecontroleerd en geïnterpreteerd door het eindpunt v2.0.  Ze zijn lange levensduur, maar uw app niet verwachten dat een token vernieuwen voor een periode duurt moet worden geschreven.  Vernieuwen tokens kunnen op elk moment voor tal van redenen worden ongeldig gemaakt.  De enige manier om uw app te weten als een token vernieuwen geldig is is als u wilt dat door een aanvraag token naar het eindpunt v2.0 inwisselen.

Wanneer u een token vernieuwd voor een nieuwe toegangstoken verzilveren (en als uw app had gekregen de `offline_access` bereik), ontvangt u een nieuwe beveiligingssleutel voor vernieuwen in het token antwoord.  Sla het token vernieuwen NET zijn uitgegeven, die u in de aanvraag te vervangen.  Hierdoor wordt gegarandeerd dat uw tokens vernieuwen geldig zo lang mogelijk blijven.

## <a name="validating-tokens"></a>Valideren van tokens

Op dit moment, is het alleen token validatie van dat uw toepassingen uit te voeren moet de id_tokens valideren.  Te valideren in een id_token, moet het valideren van uw app zowel de handtekening van de id_token en de vorderingen in de id_token.

<!-- TODO: Link -->
Wij bieden bibliotheken & codevoorbeelden die laten zien hoe gemakkelijk het token validatie - de onderstaande informatie is gewoon bedoeld voor degenen die willen begrijpen van het onderliggende proces.  Er zijn ook diverse 3e partij open-source-bibliotheken beschikbaar voor JWT validatie - er is ten minste één optie voor bijna elk platform en de taal er.

#### <a name="validating-the-signature"></a>Verifiëren van de handtekening
Een JWT bevat drie segmenten die worden gescheiden door de `.` teken.  Het eerste segment staat bekend als de **koptekst**als **orgaan**van de tweede en de derde de **handtekening**.  Het segment van de handtekening kan worden gebruikt voor het valideren van de echtheid van het id_token zodat deze kan worden vertrouwd door uw app.

Id_Tokens zijn ondertekend met een industrie standaard asymmetrische coderingsalgoritmen, zoals RSA-256. De kop van de id_token bevat informatie over de sleutel en codering methode gebruikt voor het ondertekenen van het token:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

De `alg` claim geeft aan dat het algoritme dat werd gebruikt voor het ondertekenen van tokens, terwijl de `kid` claim geeft aan dat de desbetreffende openbare sleutel die is gebruikt voor het ondertekenen van het token.

Op elk gewenst moment in de tijd kan het eindpunt v2.0 ondertekenen een id_token met een van een verzameling paren van openbare en persoonlijke sleutels.  Het eindpunt v2.0 draait de mogelijke reeks toetsen op een periodieke basis zodat uw app voor het automatisch verwerken van die belangrijke wijzigingen moet worden opgeslagen.  Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door het eindpunt v2.0 is ongeveer 24 uur.

U kunt de ondertekening belangrijke gegevens om de handtekening te valideren met behulp van de OpenID verbinding metagegevensdocument in aanschaffen:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Probeer deze URL in een browser.

Deze metagegevensdocument is een handig stukjes informatie zoals de locatie van de verschillende eindpunten die nodig zijn voor het uitvoeren van verificatie OpenID verbinding met JSON-object.  

Het bevat ook een `jwks_uri`, waardoor de locatie van de set van openbare sleutels gebruikt voor het ondertekenen van tokens.  De JSON-document te vinden op de `jwks_uri` bevat alle informatie die openbare sleutel gebruikt die op dat moment bepaald.  Uw app kunt gebruiken de `kid` in de header JWT te selecteren welke openbare sleutel in dit document is gebruikt voor het ondertekenen van een bepaalde token claimen.  Validatie van handtekeningen met behulp van de juiste openbare sleutel en de algoritme aangegeven kan vervolgens uitvoeren.

Het valideren van een handtekening is buiten het bereik van dit document - er zijn veel open source-bibliotheken beschikbaar voor u doen indien nodig te helpen.

#### <a name="validating-the-claims"></a>Valideren van de schuldvorderingen
Als uw app een id_token bij het aanmelden van gebruiker ontvangt, het moet ook een paar voeren controles uit met de vorderingen in de id_token.  Deze omvatten maar zijn niet beperkt tot:

- De **doelgroep** claim - te verifiëren dat de id_token is bedoeld om te worden gegeven aan uw app.
- **Niet eerder** en **Verlooptijd** claims - om te controleren of de id_token nog niet is verstreken.
- De **uitgevende instelling** claim - te verifiëren dat het token inderdaad naar uw toepassing door het eindpunt v2.0 uitgegeven is.
- De **Nonce** - met een token replay-aanval.
- en nog veel meer...

Raadpleeg de [specificatie OpenID verbinding](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation)voor een volledige lijst van claim validaties die uw app moet worden uitgevoerd.

Details van de verwachte waarden voor deze argumenten worden opgenomen boven in de [sectie id_token](#id_tokens).


## <a name="token-lifetimes"></a>Levensduur van token

De volgende token levensduur dienen uitsluitend voor uw begrip, zoals zij helpen kunnen bij het ontwikkelen en debuggen van apps.  Uw apps niet te verwachten van deze levensduur ongewijzigd moeten worden geschreven - ze kunnen en van invloed op elk gewenst moment in de tijd.

| Token | Levensduur | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (werk of school rekeningen) | 1 uur | Id_Tokens zijn meestal geldig gedurende een uur.  Uw web app kan deze dezelfde levensduur in het onderhouden van de eigen sessie van de gebruiker (aanbevolen) of de levensduur van een geheel andere sessie.  Als uw app een nieuwe id_token te verkrijgen moet, moet deze gewoon nieuwe-in verzoeken aan de v2.0 eindpunt machtigen.  Als de gebruiker een geldige browsersessie met het eindpunt v2.0, soms ze niet moeten hun referenties opnieuw invoeren. |
| Id_Tokens (persoonlijke rekeningen) | 24 uur | Id_Tokens voor persoonlijke accounts zijn meestal geldig gedurende 24 uur.  Uw web app kan deze dezelfde levensduur in het onderhouden van de eigen sessie van de gebruiker (aanbevolen) of de levensduur van een geheel andere sessie.  Als uw app een nieuwe id_token te verkrijgen moet, moet deze gewoon nieuwe-in verzoeken aan de v2.0 eindpunt machtigen.  Als de gebruiker een geldige browsersessie met het eindpunt v2.0, soms ze niet moeten hun referenties opnieuw invoeren. |
| Toegangstokens (werk of school rekeningen) | 1 uur | Aangegeven in antwoorden van een token als onderdeel van de metagegevens van het token. |
| Toegangstokens (persoonlijke rekeningen) | 1 uur | Aangegeven in antwoorden van een token als onderdeel van de metagegevens van het token.  Access_tokens voor persoonlijke accounts uitgegeven kan worden geconfigureerd voor de levensduur van een andere, maar één uur is meestal het geval. |
| Vernieuwen van Tokens (werk of school account) | Maximaal 14 dagen | Een token één vernieuwen is geldig voor maximaal 14 dagen.  Echter kan het token vernieuwen ongeldig worden op elk gewenst moment voor een aantal redenen, zodat uw app uit te proberen een token vernieuwd totdat dit mislukt, of uw app wordt vervangen door een nieuwe vernieuwen token moet blijven.  Een token vernieuwen ook ongeldig als het is 90 dagen nadat de gebruiker heeft ingevoerd hun referenties. |
| Vernieuwen van Tokens (persoonlijke rekeningen) | Tot 1 jaar | Een token één vernieuwen is geldig voor een maximum van 1 jaar.  Echter kan het token vernieuwen ongeldig worden op elk gewenst moment voor een aantal redenen, zodat uw app uit te proberen een token vernieuwd totdat het niet meer moet worden voortgezet. |
| Vergunning Codes (werk of school rekeningen) | 10 minuten | Codes van de vergunning zijn opzet tijdelijk en onmiddellijk moeten worden ingewisseld voor access_tokens en refresh_tokens wanneer ze worden ontvangen. |
| Vergunning Codes (persoonlijke rekeningen) | 5 minuten | Codes van de vergunning zijn opzet tijdelijk en onmiddellijk moeten worden ingewisseld voor access_tokens en refresh_tokens wanneer ze worden ontvangen.  Vergunning zijn verleend voor persoonlijke accounts ook eenmalig gebruik. |
