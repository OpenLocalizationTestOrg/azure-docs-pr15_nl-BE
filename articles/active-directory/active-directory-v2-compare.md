<properties
    pageTitle="Het eindpunt Azure AD v2.0 | Microsoft Azure"
    description="Een vergelijking tussen de oorspronkelijke Azure advertentie en de eindpunten v2.0."
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

# <a name="whats-different-about-the-v20-endpoint"></a>Wat is er anders aan het eindpunt v2.0?

Als u bekend met Azure Active Directory bent of apps hebt geïntegreerd met Azure AD in het verleden, kunnen er enkele verschillen in de v2.0 eindpunt dat u niet zou verwachten.  Dit document wordt aangeroepen om die verschillen voor je begrip.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Azure AD rekeningen en Microsoft
het eindpunt v2.0 kunnen ontwikkelaars schrijven apps die aanmelden van zowel Microsoft-Accounts en Azure AD accounts, met behulp van het eindpunt van een enkele auth accepteren.  Dit geeft u de mogelijkheid om uw app volledig rekening-agnostic; het kan zijn van het type account waarmee de gebruiker zich met aanmeldt ignorant.  Natuurlijk, u *kunt* uw app op de hoogte van het type account dat wordt gebruikt in een bepaalde sessie, maar u hoeft niet te.

Bijvoorbeeld, als uw app de [Microsoft Graph roept](https://graph.microsoft.io), sommige extra functionaliteit en gegevens niet beschikbaar voor ondernemingsgebruikers, zoals hun SharePoint-sites of Active Directory-gegevens.  Maar voor veel acties, zoals het [lezen van e-mail van een gebruiker](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), de code kan worden geschreven precies hetzelfde voor zowel Microsoft-Accounts en Azure AD-accounts.  

Uw app integratie met Microsoft-Accounts en Azure AD accounts is een eenvoudig proces.  U kunt één set van eindpunten, één bibliotheek en de registratie van een enkele app toegang te krijgen tot de consumenten- en enterprise-werelden.  Bekijk [het overzicht](active-directory-appmodel-v2-overview.md)voor meer informatie over het eindpunt v2.0.


## <a name="new-app-registration-portal"></a>Nieuwe app-portal Registratie van
het eindpunt v2.0 kan alleen worden geregistreerd op een nieuwe locatie: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dit is de portal waar kunt u een toepassings-Id, het aanpassen van de aanmeldingspagina van uw app en nog veel meer.  Alles wat u nodig hebt voor toegang tot de portal is een aangedreven Microsoft account - account voor persoonlijke of werk/school.  

Wij blijven meer en meer functies toevoegen aan deze App-Portal registratie na verloop van tijd.  De bedoeling is dat deze portal de nieuwe locatie waar u kunt en alles wat die worden te maken hebben met uw Microsoft-apps te beheren.


## <a name="one-app-id-for-all-platforms"></a>Één app Id voor alle platforms
In de oorspronkelijke Azure Active Directory-service, kunnen er verschillende andere apps voor één project hebt geregistreerd.  U moest gebruiken afzonderlijke app registraties voor de native clients en webtoepassingen:

![Registratie van oude toepassing UI](../media/active-directory-v2-flows/old_app_registration.PNG)

Bijvoorbeeld, als u zowel een website en een iOS-app gemaakt, moest u deze afzonderlijk registreren met behulp van twee verschillende id van toepassing.  Als u een website en een web api backend had, kunt u elk als een aparte app geregistreerd in Azure AD.  Als u had een iOS-app en een Android app, zou u ook hebben geregistreerd twee verschillende apps.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Alles wat u nodig is nu een enkele app registratie en een enkele toepassings-Id voor elk van uw projecten.  U kunt verschillende 'platformen' toevoegen aan een ieder project en de juiste gegevens leveren voor elk platform dat u toevoegt.  Natuurlijk kunt u zo veel apps als u afhankelijk van uw behoeften wilt, maar voor de meeste gevallen slechts één aanvraag-Id nodig moet.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Ons doel is dit zal leiden tot een meer vereenvoudigd beheer van app en ontwikkelen en een meer geconsolideerde weergave van een enkel project dat u werkt op maken.


## <a name="scopes-not-resources"></a>Scopes, resources niet
In de oorspronkelijke Azure AD-service kan een app gedragen zich als een **resource**of een ontvanger van tokens.  Een resource kan een aantal **scopes** of **oAuth2Permissions** wordt begrepen, definiëren zodat client apps voor het aanvragen van tokens die resource voor een bepaalde set van scopes.  Bekijk de Azure AD Graph API als een voorbeeld van een bron:

- Bron-id of `AppID URI`:`https://graph.windows.net/`
- Scopes, of `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, enz.  

Dit alles geldt voor de het eindpunt v2.0.  Een app kan nog steeds gedragen bereiken definiëren als bron, en worden geïdentificeerd met een URI.  Clienttoepassingen kunnen nog steeds toegang tot deze scopes aanvragen.  De manier waarin een client hierom die machtigingen vraagt is echter gewijzigd.  In het verleden een 2.0 OAuth machtigen aanvraag naar Azure AD staande kan hebben:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

de clienttoepassing vraagt vergunning voor de parameter **resource** aangegeven welke resource.  Azure AD berekend voor de machtigingen die nodig zijn door de app op basis van statische configuratie in de Portal Azure en tokens dienovereenkomstig uitgegeven.  Nu staan de dezelfde OAuth 2.0 aanvraag ziet er als volgt:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

waarbij de parameter **scope** geeft aan welke resource en machtigingen voor de app vraagt toestemming voor. De gewenste resource nog steeds zeer aanwezig is in de aanvraag - het is gewoon in elk van de waarden van de parameter scope wordt omsloten.  Met behulp van de parameter scope op deze manier kunt het eindpunt v2.0 worden beter in overeenstemming met de OAuth 2.0-specificatie en beter overeenstemt met de gangbare praktijken.  U kunt ook toepassingen uit te voeren, [incrementele toestemming](#incremental-and-dynamic-consent), die wordt beschreven in de volgende sectie.

## <a name="incremental-and-dynamic-consent"></a>Incrementele en dynamische toestemming
Apps in het algemeen beschikbare Azure AD geregistreerde service die nodig zijn voor de vereiste machtigingen voor OAuth 2.0 in de Portal Azure opgeven tijdens het maken van een app:

![Machtigingen voor inschrijving UI](../media/active-directory-v2-flows/app_reg_permissions.PNG)

De machtigingen voor een toepassing vereist zijn **statisch**geconfigureerd.  Tijdens deze configuratie van de app te bestaan in de Portal Azure toegestaan en de code leuk en eenvoudig gehouden, biedt het een paar problemen voor ontwikkelaars:

- Een app heeft alle machtigingen die ooit tijdens het maken van een app moet weten.  Machtigingen toe te voegen na verloop van tijd was een moeilijk proces.
- Een app moest alle bronnen die zou het ooit toegang tijdig weten.  Was het moeilijk om apps die toegang een willekeurig aantal resources tot te maken.
- Een app heeft alle machtigingen die ooit bij de eerste aanmelden van de gebruiker moet aanvragen.  In sommige gevallen leidde dit tot een zeer lange lijst met machtigingen die ontmoedigd eindgebruikers van het goedkeuren van de app toegang op het eerste teken in.

Met het eindpunt v2.0, kunt u de machtigingen die de app moet **dynamisch**tijdens runtime tijdens normaal gebruik van uw app.  Hiervoor kunt u de bereiken de app op elk gewenst moment in de tijd moet door ze in de `scope` -parameter van de aanvraag voor een vergunning:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

De bovenstaande aanvragen machtiging voor de toepassing van een gebruiker Azure AD directory om gegevens te lezen, en schrijven van gegevens naar de map.  Als de gebruiker heeft ingestemd met de machtigingen in het verleden voor dit bepaalde app, ze eenvoudigweg hun referenties en aangemeld bij de app.  Als de gebruiker niet heeft ingestemd met een van deze machtigingen, het eindpunt v2.0 aan de gebruiker gevraagd om toestemming om deze machtigingen.  Klik voor meer informatie kunt u lezen van op [machtigingen, toestemming, en bereiken](active-directory-v2-scopes.md).

Een dynamisch via machtigingen voor verzoeken-toepassingen de `scope` parameter geeft u volledige controle over uw gebruikerservaring.  Als u wilt, kunt u frontload uw toestemming optreden en vraagt u om alle machtigingen in een verzoek van de oorspronkelijke vergunning.  Of als uw app een groot aantal machtigingen vereist, kunt u kiezen voor het verzamelen van de machtigingen van de gebruiker stap voor stap als ze proberen om het gebruik van bepaalde functies van uw app na verloop van tijd.

## <a name="well-known-scopes"></a>Bekende scopes

#### <a name="offline-access"></a>Off line toegang
het eindpunt v2.0 is mogelijk het gebruik van een nieuwe bekende machtiging voor apps - de `offline_access` bereik.  Alle apps moet deze toestemming vragen als ze nodig hebben om toegang tot bronnen op de naam van een gebruiker voor een langere periode van tijd, zelfs wanneer de gebruiker mogelijk niet actief gebruik van de app.  De `offline_access` bereik wordt weergegeven aan de gebruiker in de dialoogvensters toestemming als "Toegang tot uw gegevens off line", die de gebruiker moet accepteren.  Aanvragen van de `offline_access` toestemming uw web app OAuth 2.0 refresh_tokens ontvangen van het eindpunt v2.0 wordt ingeschakeld.  Refresh_tokens zijn lange levensduur en voor nieuwe OAuth 2.0 access_tokens gedurende langere perioden van access kunnen worden uitgewisseld.  

Als uw app vraagt niet om de `offline_access` bereik, ontvangt geen refresh_tokens.  Dit betekent dat wanneer je een authorization_code in de [OAuth 2.0 vergunning code stroom](active-directory-v2-protocols.md#oauth2-authorization-code-flow), u alleen weer een access_token van ontvangt de `/token` eindpunt.  Die access_token blijft geldig gedurende korte tijd (meestal 1 uur), maar uiteindelijk zal verlopen.  Op dat moment, uw app wilt omleiden van de gebruiker terug naar de `/authorize` eindpunt voor het ophalen van een nieuwe authorization_code.  Tijdens deze omleiding, de gebruiker al dan niet nodig om hun referenties opnieuw invoeren of opnieuw toestemming voor machtigingen, afhankelijk van de het soort app.

Bekijk voor meer informatie over OAuth 2.0, refresh_tokens en access_tokens, de [v2.0 protocol verwijzing](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, profiel en e-mailadres

In de oorspronkelijke Azure Active Directory-service biedt de meest elementaire verbinding OpenID-in stroom een schat aan informatie over de gebruiker in de resulterende id_token.  De claims in een id_token kunnen zijn van de gebruiker naam voorkeur gebruikersnaam, e-mailadres, object-ID en meer.

We zijn nu de informatie beperken die de `openid` bereik biedt de app toegang tot.  Het bereik 'openid' wordt alleen toestaan uw app voor het ondertekenen van de gebruiker in en ontvangt een specifieke app-id voor de gebruiker.  Als u wilt dat persoonlijke gegevens worden verzameld over de gebruiker in uw app te verkrijgen, moet uw app om aanvullende machtigingen vragen van de gebruiker.  We zijn introductie van twee nieuwe scopes – de `email` en `profile` bereiken – zodat u kunt doen.

De `email` bereik is heel eenvoudig: het biedt de app toegang tot primaire e-mailadres van de gebruiker via de `email` in de id_token claimen.  De `profile` bereik biedt de app toegang tot alle andere basisinformatie over de gebruiker – de naam, de gewenste gebruikersnaam, object-ID, enzovoort.

Hiermee kunt u de code van uw app in een minimale informatieverschaffing – vraag van de gebruiker is alleen mogelijk voor de set met gegevens dat uw app vereist om haar werk te doen.  Raadpleeg voor meer informatie over deze scopes, [de scope v2.0 reference](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Token aanvragen

De vorderingen in door het eindpunt v2.0 uitgegeven tokens worden niet identiek zijn aan de tokens die zijn uitgegeven door de algemeen beschikbare eindpunten Azure AD - apps migreren naar de nieuwe service niet uitgaan in id_tokens of access_tokens een bepaalde claim aanwezig.   Tokens die worden uitgegeven door het eindpunt v2.0 compatibel zijn met de specificaties 2.0 OAuth en OpenID aansluiten, maar mag volgen verschillende semantiek dan algemeen beschikbaar Azure AD-service.

Meer informatie over de specifieke claims in tokens v2.0 uitgestoten, Zie de [tokenverwijzing v2.0](active-directory-v2-tokens.md).

## <a name="limitations"></a>Beperkingen
Er zijn enkele beperkingen, te weten wanneer u het punt v2.0.  Raadpleeg de [v2.0 beperkingen doc](active-directory-v2-limitations.md) te zien als een van deze beperkingen van toepassing op uw specifieke scenario.
