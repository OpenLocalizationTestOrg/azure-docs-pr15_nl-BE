<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="De typen toepassingen die u kunt maken in de B2C van Azure Active Directory."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Typen toepassingen

Azure Active Directory (AD Azure) B2C ondersteunt verificatie voor tal van moderne app-architecturen. Deze zijn gebaseerd op de industriestandaardprotocollen [OAuth 2.0](active-directory-b2c-reference-protocols.md) of [OpenID verbinden](active-directory-b2c-reference-protocols.md). Dit document is een korte beschrijving van de typen toepassingen die u kunt maken, onafhankelijk van de taal of platform u de voorkeur geeft. Bovendien leert u de geavanceerde scenario's voordat u [begint met het bouwen van toepassingen](active-directory-b2c-overview.md#getting-started).

## <a name="the-basics"></a>De grondbeginselen
Elke toepassing die gebruikmaakt van Azure AD B2C moet worden geregistreerd in de [B2C-directory](active-directory-b2c-get-started.md) via het [Portal Azure](https://portal.azure.com/). Het registratieproces app verzamelt en een paar waarden worden toegewezen aan uw app:

- Een **Toepassings-ID** die een unieke identificatie van uw app.
- Een **Redirect URI** die kan worden gebruikt voor directe antwoorden terug naar uw app.
- Andere specifieke scenario-waarden. Informatie voor meer informatie over het [registreren van een app](active-directory-b2c-app-registration.md).

Nadat de app is geregistreerd, communiceren met Azure AD door het verzenden van verzoeken naar de Azure AD v2.0-eindpunt:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Elke aanvraag die wordt verzonden naar Azure AD B2C geeft een **beleid**. Een beleid bepaalt het gedrag van Azure AD. U kunt ook deze eindpunten voor het maken van een zeer aanpasbare set van ervaringen van gebruikers. Gemeenschappelijk beleid zijn aanmelding, aanmelden en beleid profiel bewerken. Als u niet bekend met beleid bent, moet u lezen over de Azure AD B2C [extensible beleidskader](active-directory-b2c-reference-policies.md) voordat u doorgaat.

De interactie tussen elke app en een eindpunt v2.0 volgt een soortgelijk patroon op hoog niveau:

1. De app zorgt ervoor dat de gebruiker naar het eindpunt v2.0 voor het uitvoeren van een [beleid](active-directory-b2c-reference-policies.md).
2. De gebruiker is voltooid voor het beleid voor de definitie van het beleid.
4. De app krijgt een soort beveiligingstoken vanuit het eindpunt v2.0.
5. De toepassing gebruikt het beveiligingstoken toegang te krijgen tot beveiligde gegevens of een beveiligde bron.
6. De bronserver valideert het beveiligingstoken om te controleren of dat toegang kan worden verleend.
7. De app wordt regelmatig vernieuwd voor het beveiligingstoken.

<!-- TODO: Need a page for libraries to link to -->
Deze stappen kunnen verschillen enigszins op basis van het type toepassing dat u bouwt. Open-source-bibliotheken kunnen de details voor u adresseren.

## <a name="web-apps"></a>Web apps
Voor web apps (inclusief .NET, PHP, Java, Ruby, Python en Node.js) die worden gehost op een server en toegankelijk via een browser, ondersteunt Azure AD B2C [OpenID verbinding](active-directory-b2c-reference-protocols.md) voor alle gebruikerservaringen. Hierbij aanmelden, aanmelding, en beheer van gebruikersprofielen. In de Azure AD B2C-implementatie van OpenID Connect Start uw web app deze ervaringen van gebruikers via de uitgifte van verificatieaanvragen naar Azure AD. Het resultaat van de aanvraag is een `id_token`. Deze beveiligingstoken staat voor de identiteit van de gebruiker. Het biedt ook informatie over de gebruiker in de vorm van vorderingen:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Meer informatie over de soorten tokens en claims die tot in de [B2C-token verwijst naar](active-directory-b2c-reference-tokens.md)een app beschikbaar.

In een web app wordt elke uitvoering van een [beleid](active-directory-b2c-reference-policies.md) op hoog niveau als volgt:

![Web App Swimlanes afbeelding](./media/active-directory-b2c-apps/webapp.png)

Validatie van de `id_token` met behulp van een openbare handtekeningsleutel die is ontvangen van AD Azure is voldoende om te controleren of de identiteit van de gebruiker. Hiermee stelt u een sessiecookie die kan worden gebruikt ter identificatie van de gebruiker op de volgende pagina-aanvragen.

Om dit scenario in actie zien, probeert u een van de web app-in codevoorbeelden in onze [dat sectie aan de slag](active-directory-b2c-overview.md#getting-started).

Naast het vergemakkelijken van eenvoudige aanmelden, moet misschien ook een server web app voor toegang tot een back-end-webservice. In dit geval kunt de web app uitvoeren van een iets andere [OpenID aansluiten stroom](active-directory-b2c-reference-oidc.md) en tokens ophalen met behulp van codes van de vergunning en tokens te vernieuwen. In dit scenario wordt weergegeven in de volgende [sectie voor Web-API's](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Web-API 's
U kunt Azure AD B2C secure webservices, zoals van uw app RESTful web API. Web API's kunt OAuth 2.0 gebruiken om hun gegevens te beveiligen door verificatie van binnenkomende HTTP-aanvragen met behulp van tokens. De aanvrager van een web-API wordt een token in de kop van de vergunning van een HTTP-aanvraag:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Het web API kunt het token vervolgens gebruiken om te controleren of de identiteit van de beller API en informatie over de beller ophalen uit schuldvorderingen die zijn gecodeerd in het token. Meer informatie over de soorten tokens en aanspraken die beschikbaar zijn op in het [token Azure AD B2C verwijst naar](active-directory-b2c-reference-tokens.md)een app.

> [AZURE.NOTE]
    Azure AD B2C ondersteunt momenteel alleen web API's die worden benaderd door hun eigen bekende clients. Uw volledige app kan bijvoorbeeld ook een iOS-app, een Android app en een back-end web API. Deze architectuur wordt volledig ondersteund. Waardoor een partner-client, zoals een andere iOS-app, toegang te krijgen tot hetzelfde web die API wordt momenteel niet ondersteund. Alle onderdelen van uw volledige app moet delen een enkele toepassing.

Een web API kunt tokens ontvangen van verschillende soorten clients, waaronder web apps, desktop en mobiele toepassingen één pagina apps, server-side daemons en andere web-API's. Hier volgt een voorbeeld van de volledige stroom voor een web app die een web-API aanroept:

![Web App API Swimlanes webafbeelding](./media/active-directory-b2c-apps/webapi.png)

Lees voor meer informatie over verificatie codes, tokens vernieuwen en de stappen voor het verkrijgen van tokens, het [OAuth 2.0-protocol](active-directory-b2c-reference-oauth-code.md).

Meer informatie over het beveiligen van een web API via Azure AD B2C, check de web API zelfstudies in onze [sectie aan de slag](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Mobile en native apps
Toepassingen die zijn geïnstalleerd op apparaten, zoals mobiele en desktop-apps moeten vaak toegang tot back-end services of web-API's voor gebruikers. U kunt aangepaste identity management ervaringen toevoegen aan uw native apps en veilig call back-end services met Azure AD B2C en de [OAuth 2.0 vergunning code stroom](active-directory-b2c-reference-oauth-code.md).  

In deze overdracht, de app voert [beleid](active-directory-b2c-reference-policies.md) uit en ontvangt een `authorization_code` uit Azure Active Directory als de gebruiker het beleid is voltooid. De `authorization_code` vertegenwoordigt van het app-machtiging aan te roepen van de back-end diensten voor rekening van de gebruiker die momenteel is aangemeld. De app kunt wisselen dan de `authorization_code` op de achtergrond voor een `id_token` en een `refresh_token`.  De app kunt gebruiken de `id_token` om een back-end web API in HTTP-aanvragen te verifiëren. Zo kunt u de `refresh_token` om een nieuwe `id_token` waarop een oudere verloopt.

> [AZURE.NOTE]
    Azure AD B2C ondersteunt momenteel alleen de tokens die worden gebruikt voor toegang tot een back-end web app eigen service. Uw volledige app kan bijvoorbeeld ook een iOS-app, een Android app en een back-end web API. Deze architectuur wordt volledig ondersteund. Zodat uw iOS-app toegang krijgen tot een partner web API door toegangstokens OAuth-2.0 wordt momenteel niet ondersteund. Alle onderdelen van uw volledige app moet delen een enkele toepassing.

![Native App Swimlanes afbeelding](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Huidige beperkingen
Azure AD B2C biedt momenteel geen ondersteuning voor de volgende soorten apps, maar ze zijn op de roadmapy. Extra en beperkingen die betrekking hebben op Azure AD B2C worden in [beperkingen](active-directory-b2c-limitations.md)beschreven.

### <a name="single-page-apps-javascript"></a>Eén pagina apps (JavaScript)
Veel moderne apps hebben een enkele pagina app-front-end voornamelijk in JavaScript worden geschreven. Ze gebruiken vaak een kader zoals AngularJS, Ember.js of Durandal. Algemeen beschikbare Azure AD-service ondersteunt deze apps met behulp van de impliciete OAuth 2.0-stroom. Echter, deze overdracht is nog niet beschikbaar in Azure AD B2C.

### <a name="daemonsserver-side-apps"></a>Daemons-serverzijde apps
Apps die langdurige processen bevatten of die worden toegepast zonder de aanwezigheid van een gebruiker moeten ook toegang krijgen tot beveiligde bronnen zoals web-API's. Deze apps kunnen verifiëren en tokens krijgen met behulp van de identiteit van de toepassing (in plaats van gedelegeerde identiteit van een gebruiker) en met behulp van de client OAuth 2.0 referenties stroom.

Deze overdracht wordt momenteel niet ondersteund door Azure AD B2C. Deze apps kunnen tokens krijgen alleen nadat er een interactieve gebruiker stroom heeft plaatsgevonden.

### <a name="web-api-chains-on-behalf-of-flow"></a>Web API is gekoppeld (op-rekening-of stroom)
Vele architecturen zijn een web-API die een ander downstream web-API aanroepen wanneer beide zijn beveiligd met Azure AD B2C. In dit scenario is het gebruikelijk in native clients die beschikken over een Web API back-end. Dit roept vervolgens een on line service van Microsoft zoals de Azure AD Graph API.

In dit scenario aaneengeschakelde web API kan met behulp van de subsidie in OAuth 2.0 JWT aan toonder referentie, ook bekend als de stroom op-rekening-of worden ondersteund.  Echter, de stroom op-rekening-of is momenteel niet geïmplementeerd in de B2C Azure AD.
