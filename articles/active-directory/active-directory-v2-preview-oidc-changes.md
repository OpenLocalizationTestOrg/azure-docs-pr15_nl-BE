<properties
    pageTitle="Wijzigingen in het eindpunt Azure AD v2.0 | Microsoft Azure"
    description="Een beschrijving van de wijzigingen die worden aangebracht in de app model v2.0 public preview-protocollen."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Belangrijke Updates van de verificatieprotocollen v2.0
Ontwikkelaars aandacht! De volgende twee weken, wordt nog een paar updates aan onze v2.0 verificatieprotocollen die wijzigingen voor alle toepassingen die u hebt geschreven tijdens onze testperiode breken kunnen betekenen.  

## <a name="who-does-this-affect"></a>Wie heeft dit invloed op?
Alle toepassingen die zijn geschreven in de v2.0 gebruiken geconvergeerd verificatie-eindpunt

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Meer informatie over het eindpunt v2.0 vindt u [hier](active-directory-appmodel-v2-overview.md).

Als u een toepassing met behulp van het eindpunt v2.0 door codering rechtstreeks aan het protocol versie 2.0 hebt gemaakt, moet met behulp van onze website OpenID verbinden of OAuth middlewares of andere 3de partij bibliotheken gebruiken voor het uitvoeren van verificatie, u worden voorbereid voor het testen van uw projecten en indien nodig wijzigingen aanbrengen.

## <a name="who-doesnt-this-affect"></a>Die niet dit van invloed op?
Alle toepassingen die zijn geschreven ten opzichte van het eindpunt van de verificatie productie Azure AD

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Dit protocol is ingesteld in steen en worden geen wijzigingen problemen.

Bovendien, als uw app **alleen** onze ADAL bibliotheek gebruikt voor het uitvoeren van verificatie, u geen wijzigen.  ADAL is afgeschermd uw app van de wijzigingen.  

## <a name="what-are-the-changes"></a>Wat zijn de wijzigingen?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>De waarde x5t verwijderen van JWT kopteksten
Het eindpunt v2.0 worden tokens JWT uitgebreid, die een koptekstsectie parameters met relevante metagegevens over het token bevatten.  Als u de kop van een van onze huidige JWTs decoderen, zou u ongeveer als volgt vinden:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

De "x5t" en "onder geitenlammeren" eigenschappen geven aan waar de openbare sleutel die moet worden gebruikt voor het valideren van het token handtekening, opgehaald vanuit het eindpunt van de metagegevens OpenID verbinding.

De wijziging die we, hier aanbrengen is om de eigenschap 'x5t' te verwijderen.  U kan blijven gebruiken dezelfde mechanismen om tokens te valideren, maar alleen op de eigenschap "geitenlammeren" voor het ophalen van de juiste openbare sleutel, als omschreven in het protocol OpenID verbinding moet uitgaan. 

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw app is niet afhankelijk van het bestaan van de x5t-waarde.**

### <a name="removing-profileinfo"></a>Profile_info verwijderen
Eerder, het eindpunt v2.0 is is een base64-gecodeerde JSON-object te retourneren in token antwoorden genoemd `profile_info`.  Bij het aanvragen van een toegangstoken vanuit het eindpunt v2.0 door een aanvraag te verzenden:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Het antwoord eruit als het volgende JSON-object:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

De `profile_info` waarde uit informatie over de gebruiker die heeft aangemeld bij de app - hun naam, voornaam, achternaam, e-mailadres, id, enzovoort.  Voornamelijk, de `profile_info` voor token-caching is gebruikt en op het scherm.

We zijn nu verwijderen de `profile_info` waarde – maar maak je geen zorgen, we nog steeds verschaft u deze informatie voor ontwikkelaars op een iets andere plaats.  In plaats van `profile_info`, het eindpunt v2.0 resulteert vervolgens in een `id_token` in elk token antwoord:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

U kunt decoderen en de id_token voor het ophalen van dezelfde gegevens die u hebt ontvangen van profile_info parseren.  De id_token is een JSON Web Token (JWT), met inhoud zoals opgegeven door de OpenID verbinden.  De code voor dit moet dus vergelijkbaar – u hoeft alleen het middelste segment (body) van de id_token uitpakken en base64 om de JSON-object binnen het decoderen.

De volgende twee weken, moet u code uw app om op te halen de gebruikersgegevens uit het `id_token` of `profile_info`; indien deze is aanwezig.  Op deze manier wanneer de wijziging is aangebracht, uw app kan naadloos worden verwerkt de overgang van `profile_info` te `id_token` zonder onderbreking.

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw app is niet afhankelijk van het bestaan van de `profile_info` waarde.**

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in verwijderen
Vergelijkbaar met `profile_info`, verwijderen we ook de `id_token_expires_in` parameter van antwoorden.  Eerder, het eindpunt v2.0 retourneert een waarde voor `id_token_expires_in` met elk antwoord id_token, bijvoorbeeld in een antwoord machtigen:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Of als een token reactie:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

De `id_token_expires_in` waarde duidt dit op het aantal seconden dat het id_token geldig blijven zou.  We zijn nu verwijderen de `id_token_expires_in` waarde volledig.  In plaats daarvan, mag u de standaard verbinding OpenID `nbf` en `exp` vorderingen te onderzoeken, de geldigheid van een id_token.  Zie de [v2.0 token reference](active-directory-v2-tokens.md) voor meer informatie over deze vorderingen.

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw app is niet afhankelijk van het bestaan van de `id_token_expires_in` waarde.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Wijzigen van de claims die worden geretourneerd door de scope = openid
Deze wijziging wordt de meest significante – in feite, heeft dit gevolgen voor vrijwel elke toepassing die gebruikmaakt van het eindpunt v2.0.  Veel toepassingen aanvragen verzenden naar de v2.0 eindpunt met behulp van de `openid` het bereik, zoals:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Vandaag, wanneer de gebruiker toestemming voor verleent de `openid` bereik, uw app ontvangt een schat aan informatie over de gebruiker in de resulterende id_token.  Deze claims zijn hun naam, voorkeur gebruikersnaam, e-mailadres, object-ID en meer.

In deze update, zijn we de informatie wijzigen die de `openid` bereik biedt de app toegang, tot betere comform met de specificatie OpenID verbinden.  De `openid` bereik wordt alleen toestaan uw app voor het ondertekenen van de gebruiker in en ontvangt u een specifieke app-id voor de gebruiker in de `sub` van de id_token claimen.  De claims in een id_token met alleen de `openid` bereik verleend worden persoonlijke gegevens.  Voorbeeld id_token claims zijn:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Als u wilt dat persoonlijke gegevens worden verzameld over de gebruiker in uw app te verkrijgen, moet uw app om aanvullende machtigingen vragen van de gebruiker.  We introduceert ondersteuning voor twee nieuwe scopes uit de spec OpenID verbinding – de `email` en `profile` bereiken – zodat u kunt doen.

- De `email` bereik is heel eenvoudig: het biedt de app toegang tot primaire e-mailadres van de gebruiker via de `email` in de id_token claimen.  Houd er rekening mee dat de `email` claim niet altijd aanwezig zijn in de id_tokens – deze alleen worden opgenomen als in het profiel van de gebruiker beschikbaar.
- De `profile` bereik biedt de app toegang tot alle andere basisinformatie over de gebruiker – de naam, de gewenste gebruikersnaam, object-ID, enzovoort.

Hiermee kunt u de code van uw app in een minimale informatieverschaffing: vraagt u de gebruiker voor de set informatie die uw app nodig om haar werk te doen.  Als u doorgaan met het ophalen van de volledige lijst met gebruikersgegevens die op dit moment uw app ontvangt wilt, moet u alle scopes in drie opnemen in uw vergunning aanvragen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Uw app kunt beginnen met het verzenden van de `email` en `profile` direct bereiken en het eindpunt v2.0 zal accepteren deze twee bereiken en beginnen met het aanvragen van machtigingen van gebruikers als u nodig hebt.  Echter, de wijziging in de interpretatie van de `openid` bereik wordt pas van kracht voor een paar weken.

> [AZURE.IMPORTANT] **Uw taak: toevoegen de `profile` en `email` -bereiken als uw app gegevens over de gebruiker vereist.**  Rekening mee dat ADAL worden deze beide machtigingen in aanvragen standaard. 

### <a name="removing-the-issuer-trailing-slash"></a>Het verwijderen van de uitgevende instelling afsluitende slash.
Eerder, de uitgevende instelling waarde die wordt weergegeven in tokens vanuit het eindpunt v2.0 vond het formulier

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

De guid is waar de tenantId van de pachter Azure AD die het token heeft afgegeven.  Met deze wijzigingen wordt de waarde van de uitgevende instelling

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

in beide tokens en in de discovery-document OpenID verbinden.

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw toepassing de waarde van de uitgevende instelling zowel met als zonder een slash accepteert tijdens de validatie van de uitgevende instelling.**

## <a name="why-change"></a>Waarom wijzigen?
De primaire motivatie voor de invoering van deze wijzigingen is compatibel met de standaardspecificatie OpenID verbinden.  Door OpenID compatibele verbinding, we hopen dat verschillen tussen integratie met Microsoft-services voor identiteit en met andere diensten van de identiteit van de industrie te minimaliseren.  Wij willen kunnen ontwikkelaars de bibliotheken van hun favoriete open-source-verificatie gebruiken zonder dat de bibliotheken wijzigen voor verschillende Microsoft.

## <a name="what-can-you-do"></a>Wat kunt u doen?
Vanaf vandaag, kunt u beginnen met het maken van alle bovengenoemde wijzigingen.  U moet onmiddellijk:

1.  **Verwijder de afhankelijkheden van de `x5t` headerparameter.**
2.  **De overgang van de juiste manier verwerken `profile_info` te `id_token` in antwoorden van een token.**
3.  **Verwijder de afhankelijkheden van het `id_token_expires_in` reactie-parameter.**
3.  **Voeg toe de `profile` en `email` bereiken op uw app als uw app basic gebruikersinformatie nodig heeft.**
4.  **Waarden van de uitgevende instelling in tokens, zowel met als zonder een slash accepteren.**

Onze [documentatie v2.0-protocol](active-directory-v2-protocols.md) is al bijgewerkt met deze wijzigingen, zodat u deze als referentie gebruiken kunt bij het bijwerken van uw code.

Als u meer vragen over de omvang van de wijzigingen hebt, neem gerust bereiken ons op Twitter op @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Hoe vaak wijzigingen in protocol plaatsvindt?
Wij doen niet voorzien om eventuele verdere overtredingen van de wijzigingen in de verificatieprotocollen.  We bundelt deze wijzigingen in één release zodat u niet steeds te gaan door middel van dit type updateproces elke keer opnieuw spoedig opzettelijk.  Natuurlijk zullen wij de functies toevoegen aan de geconvergeerde v2.0 authentication-service die u van profiteren kunt, maar deze wijzigingen moeten toevoegingsmiddel en niet-bestaande code einde.

Ten slotte willen we zeggen dank u voor het uitproberen van dingen van tijdens de proefperiode.  De inzichten en ervaringen van onze vroege gebruikers zijn onschatbare waarde tot nu toe en we hopen dat u zult uw mening en ideeën te delen.

Plezier met het coderen!

De afdeling Microsoft Identity
