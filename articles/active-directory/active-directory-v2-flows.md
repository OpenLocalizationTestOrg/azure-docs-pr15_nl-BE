<properties
    pageTitle="Typen van het eindpunt v2.0 | Microsoft Azure"
    description="De typen toepassingen en scenario's worden ondersteund door het eindpunt Azure AD v2.0."
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

# <a name="types-of-apps-for-the-v20-endpoint"></a>Soorten apps voor het eindpunt v2.0
Het eindpunt v2.0 ondersteunt verificatie voor tal van moderne app architecturen, die allemaal zijn gebaseerd op de industriestandaardprotocollen [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) en/of [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Dit document is een korte beschrijving van de typen toepassingen die u kunt maken, onafhankelijk van de taal of platform u de voorkeur geeft.  Het helpt u inzicht in het hoogste niveau scenario's voordat u [meteen in de code](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De grondbeginselen
Elke toepassing die gebruikmaakt van het eindpunt v2.0 moet op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)worden geregistreerd.  Het registratieproces app zal verzamelen & een paar waarden toewijzen aan uw app:

- Een **Toepassings-Id** die een unieke identificatie van uw app
- Een **Redirect URI** die kan worden gebruikt voor directe antwoorden terug naar uw app
- Een paar andere scenario-specifieke waarden.  Voor meer gedetailleerde informatie over het [registreren van een app](active-directory-v2-app-registration.md).

Zodra geregistreerd, communiceert de app Azure AD door het verzenden van verzoeken naar het eindpunt van de v2.0 Azure Active Directory.  Wij bieden open source frameworks & bibliotheken die voor de details van deze aanvragen zorgen of u kunt ook zelf de logica van de verificatie implementeren door aanvragen voor deze eindpunten:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web apps
Voor web apps (.NET, PHP, Java, Ruby, Python, knooppunt, enzovoort) die worden benaderd via een browser, kunt u uitvoeren gebruiker aanmelden met [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  In de OpenID verbinding maken met de web app ontvangt een `id_token`, een beveiligingstoken dat wordt gecontroleerd of de identiteit van de gebruiker en geeft informatie over de gebruiker in de vorm van vorderingen:

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

U kunt informatie over alle typen van tokens en vorderingen die aan een app in de [v2.0 token verwijzing](active-directory-v2-tokens.md)beschikbaar.

In web server apps neemt de stroom verificatie-in hoog niveau als volgt:

![Web App Swimlanes afbeelding](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

De validatie van de id_token met behulp van een openbare handtekeningsleutel heeft ontvangen van het eindpunt v2.0 is voldoende waarborgen van de identiteit van de gebruiker en stel een sessiecookie die kan worden gebruikt ter identificatie van de gebruiker op de volgende pagina-aanvragen.

Om dit scenario in actie ziet, probeer een van de web app-in codevoorbeelden in onze sectie [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) .

Naast het eenvoudig aanmelden moet misschien ook een server web app voor toegang tot sommige andere webservice zoals een REST API.  In dit geval kan de server web app in een gecombineerde verbinding OpenID en OAuth 2.0 flow, de [stroom OAuth 2.0 autorisatiecode](active-directory-v2-protocols.md#oauth2-authorization-code-flow)met treden. In dit scenario valt onder onze [onderwerp WebApp WebAPI aan de slag](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web-API 's
U kunt het eindpunt v2.0 secure webservices, zoals van uw app RESTful Web API.  In plaats van id_tokens en sessie-cookies, Web-API's OAuth 2.0 access_tokens gebruiken voor het beveiligen van gegevens en verificatie van binnenkomende aanvragen.  De aanvrager van een Web-API wordt een access_token in de kop van de vergunning van een HTTP-aanvraag:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

De Web-API kunt de access_token gebruiken om te controleren of de identiteit van de beller API en informatie over de beller ophalen uit claims die zijn gecodeerd in de access_token.  U kunt informatie over alle typen van tokens en vorderingen die aan een app in de [v2.0 token verwijzing](active-directory-v2-tokens.md)beschikbaar.

Web-API kan gebruikers kunnen opt-in/opt-out van bepaalde functionaliteit of gegevens geven door machtigingen, ook bekend als [scopes](active-directory-v2-scopes.md)bloot.  Voor een aanroep app toestemming om een bereik te verkrijgen, moet de gebruiker aan de scope toestemming tijdens een stroom.  Het eindpunt v2.0 zal zorgen voor de gebruiker om toestemming gevraagd en die machtigingen vastleggen in alle access_tokens die de Web-API ontvangt.  Alle Web-API moet zorgen te maken over valideren de ontvangen op elke aanroep van access_tokens en de juiste machtiging controles uitvoeren.

Web-API kunnen access_tokens ontvangen van alle soorten apps, met inbegrip van de web server apps, desktop en mobiele apps, apps voor één pagina, server side daemons en zelfs andere Web-API's.  De stroom hoog niveau voor webverificatie api is als volgt:

![Swimlanes webafbeelding API](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Lees voor meer informatie over authorization_codes, refresh_tokens en de gedetailleerde stappen voor het ophalen van access_tokens, het [OAuth 2.0-protocol](active-directory-v2-protocols-oauth-code.md).

Voor meer informatie over het beveiligen van een web api met access_tokens, OAuth2, bekijk de codevoorbeelden voor web-api in onze [sectie aan de slag](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile en native apps
Toepassingen die zijn geïnstalleerd op een apparaat, zoals mobiele en desktop-apps vaak toegang moeten krijgen tot backend-services of Web-API's die gegevens opslaan en uitvoeren van diverse functies voor een gebruiker.  Deze apps kunnen aanmelden en machtiging toevoegen aan de backend-services met behulp van de [stroom OAuth 2.0 autorisatiecode](active-directory-v2-protocols-oauth-code.md).  

In deze stroom, een de app ontvangt een authorization_code van het eindpunt v2.0 bij gebruiker aanmelden, dat staat voor de machtiging van de app aanroepen backend diensten voor rekening van de gebruiker die momenteel is aangemeld.  De app kunt wisselen dan de authoriztion_code op de achtergrond voor een 2.0 OAuth-access_token en een refresh_token.  De app de access_token kunt gebruiken voor het verifiëren van Web-API's in HTTP-aanvragen en de refresh_token kunt gebruiken om nieuwe access_tokens als oudere verstrijkt.

![Native App Swimlanes afbeelding](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Eén pagina apps (javascript)
Veel moderne apps hebben een enkele pagina App (SPA) front-end voornamelijk geschreven in javascript en vaak met frameworks zoals AngularJS, Ember.js, Durandal, enz.  Het eindpunt Azure AD v2.0 ondersteunt deze apps met behulp van de [Impliciete stromen van OAuth 2.0](active-directory-v2-protocols-implicit.md).

In deze overdracht, ontvangt de app tokens van de v2.0 machtigen eindpunt rechtstreeks, zonder het uitvoeren van een back-end-server naar server uitwisselingen.  Hierdoor kunnen alle logica voor verificatie en verwerking van de sessie te nemen plaats volledig in de javascript-client zonder extra pagina omleidingen.

![Impliciete stroom Swimlanes afbeelding](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Om dit scenario in actie ziet, probeer een van de enkele pagina app codevoorbeelden in onze sectie [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Daemons/server side apps
Apps die langdurige processen bevatten of die worden toegepast zonder de aanwezigheid van een gebruiker moeten ook toegang krijgen tot beveiligde bronnen, zoals Web-API's.  Deze apps kunnen verifiëren en tokens met de identiteit van de app (plaats overgedragen identiteit van een gebruiker) met behulp van de OAuth 2.0-client referenties stroom krijgt.

In deze overdracht, verkrijgt de app tokens door interactief werken met de `/token` eindpunt:

![Daemon App Swimlanes afbeelding](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Zie de documeenation client referenties in onze sectie [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) voor het opbouwen van een daemon-app, of verwijzen naar [deze .NET monster app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

## <a name="current-limitations"></a>Huidige beperkingen
Deze typen toepassingen worden momenteel niet ondersteund door het eindpunt v2.0, maar zijn op de routekaart.  En extra beperkingen voor het eindpunt v2.0 worden beschreven in het [artikel van v2.0 beperkingen](active-directory-v2-limitations.md).

### <a name="chained-web-apis-on-behalf-of"></a>Aaneengeschakelde web API's (op-rekening-of)
Vele architecturen zijn een Web-API die een andere downstream Web API, beide beveiligd door het eindpunt v2.0 aanroepen.  In dit scenario is het gebruikelijk in native clients die beschikken over een Web API backend, die op zijn beurt een Microsoft Online-service zoals Office 365 of de Graph API aanroept.

Dit aaneengeschakelde Web API scenario kan worden ondersteund met de subsidie OAuth 2.0 Jwt aan toonder referentie, ook bekend als de [On-Behalf-Of lopen](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow).  Echter, de stroom aan-rekening-Of is momenteel niet geïmplementeerd in het eindpunt v2.0.  Voor een overzicht van de werking van deze overdracht in het algemeen beschikbare Azure AD service, raadpleegt u de [op-rekening-Of codevoorbeeld op GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
