<properties
   pageTitle="Azure Active Directory developer's guide | Microsoft Azure"
   description="Dit artikel bevat een uitgebreide handleiding resources ontwikkelaar-georiënteerde voor Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Azure Active Directory developer's guide

## <a name="overview"></a>Overzicht
Een identiteitsbeheer als service (IDMaaS) platform biedt Azure Active Directory (AD) ontwikkelaars een effectieve manier om identity management te integreren in hun toepassingen. De volgende artikelen bevatten overzichten op de uitvoering en de belangrijkste functies van AD Azure. Wij stellen ze in volgorde lezen of om [aan de slag](#getting-started) gaan, bent u klaar om u te verdiepen.


1. [De voordelen van de integratie van de Azure AD](./develop/active-directory-how-to-integrate.md): Ontdek waarom integratie met AD Azure biedt de beste oplossing voor het veilig inloggen en autorisatie.

1. [Azure AD verificatie scenario's](active-directory-authentication-scenarios.md): profiteren van vereenvoudigde verificatie in Azure AD voor aanmelding bij uw toepassing.

1. [Integratie van toepassingen met Azure AD](active-directory-integrating-applications.md): informatie over het toevoegen, bijwerken en verwijderen van toepassingen van Azure AD, en over de richtlijnen van de huisstijl voor geïntegreerde apps.

1. [Azure AD Graph API](active-directory-graph-api.md): de Azure AD Graph API gebruiken via programmering toegang tot Azure AD via eindpunten REST API. De Azure AD Graph API is ook toegankelijk via [Microsoft Graph](https://graph.microsoft.io/). Microsoft Graph biedt een geünificeerde API waarmee toegang tot meerdere Microsoft cloud service API's, via een enkele REST API eindpunt en met een één toegangstoken.

1. [Azure AD verificatie bibliotheken](active-directory-authentication-libraries.md): eenvoudig worden gebruikers geverifieerd om toegangstokens verkrijgen via Azure AD verificatie bibliotheken voor .NET, JavaScript, doel-C en Android.


## <a name="getting-started"></a>Aan de slag

Deze zelfstudies zijn op maat gemaakt voor meerdere platforms en kunnen je helpen bij het snel ontwikkelen van Azure Active Directory. Als een vereiste is moet u [een huurder Azure Active Directory ophalen](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Mobiele en PC toepassing quick start-gidsen

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Universele Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Universele Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Rechtstreeks te integreren met OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Web application quick start-gidsen

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID-verbinding](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Rechtstreeks te integreren met OpenID verbinding](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Web API quick start-gidsen

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Bij het controleren van de directory-handleiding

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph API](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>Uitleg

Deze artikelen wordt beschreven hoe u specifieke taken uitvoeren met behulp van Azure Active Directory:

- [Een huurder Azure AD ophalen](active-directory-howto-tenant.md)
- [Gebruikers met de toepassing van meerdere huurder patroon Azure AD aanmelden](active-directory-devhowto-multi-tenant-overview.md)
- Cross-app SSO ADAL, met behulp van [Android](active-directory-sso-android.md) en [iOS](active-directory-sso-ios.md) -apparaten inschakelen
- [Uw toepassing AppSource is gecertificeerd voor Azure advertentie maken](active-directory-devhowto-appsource-certified.md)
- [Overzicht van de toepassing in de galerie met Azure AD toepassing](active-directory-app-gallery-listing.md)
- [Indienen van web apps voor Office 365 naar Verkoperprestaties](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Inzicht in het toepassingsmanifest Azure Active Directory](active-directory-application-manifest.md)
- [De huisstijl richtlijnen voor het aanmelden en de app aanschaf knoppen in de clienttoepassing begrijpen](active-directory-branding-guidelines.md)
- [Voorbeeld: Het bouwen van apps die gebruikers met accounts voor zowel persoonlijke & werk of school te ondertekenen](active-directory-appmodel-v2-overview.md)
- [Voorbeeld: Het bouwen van toepassingen die aanmelden & aanmelden consumenten](../active-directory-b2c/active-directory-b2c-overview.md)
- [Voorbeeld: token levensduur configureren in Azure AD](active-directory-configurable-token-lifetimes.md) met PowerShell. Zie [beleidstransacties](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) en de [entiteit beleid](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) voor meer informatie over het configureren van via de Azure AD Graph API.

## <a name="reference"></a>Referentie

Deze artikelen voorzien in een verwijzing foundation REST en verificatie bibliotheek API's, protocollen, fouten, codevoorbeelden en eindpunten.  

###  <a name="support"></a>Ondersteuning
- [Labels vragen](http://stackoverflow.com/questions/tagged/azure-active-directory): Azure Active Directory zoeken naar oplossingen op de Stack Overflow door te zoeken naar de tags [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) en [adal](http://stackoverflow.com/questions/tagged/adal).
- Zie de [verklarende woordenlijst Azure AD-ontwikkelaar](active-directory-dev-glossary.md) voor definities van een aantal veelgebruikte termen die betrekking hebben op de integratie en ontwikkeling van toepassingen.

### <a name="code"></a>Code

- [Azure Active Directory open source-bibliotheken](http://github.com/AzureAD): de eenvoudigste manier om te zoeken naar de bron van een bibliotheek met behulp van onze [lijst](active-directory-authentication-libraries.md)is.

- [Azure Active Directory-voorbeelden](https://github.com/azure-samples?query=active-directory): de eenvoudigste manier om te navigeren van de lijst van de monsters met behulp van de [index van de codevoorbeelden](active-directory-code-samples.md)is.

- [Active Directory verificatie bibliotheek (ADAL) voor .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - documentatie is beschikbaar voor zowel [de laatste primaire versie](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) en de [vorige primaire versie](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Graph API

- [Graph API reference](https://msdn.microsoft.com/library/azure/hh974476.aspx): REST verwijzing voor Azure Active Directory Graph API. [De interactieve grafiek API reference ervaring geven](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Graph API machtiging bereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0 machtiging scopes die worden gebruikt om de toegang van een app om Active directory-gegevens in een huurder te beheren.

### <a name="authentication-and-authorization-protocols"></a>Protocollen voor verificatie en autorisatie

- [Ondertekening sleutel Rollover in Azure AD](active-directory-signing-key-rollover.md): informatie over het ondertekenen van Azure AD cadence sleutel rollover en het bijwerken van de sleutel voor de meest voorkomende scenario's van toepassing.

- [2.0 OAuth-protocol: met het verlenen van vergunning code](active-directory-protocols-oauth-code.md): kunt u OAuth 2.0-protocol vergunning code verlenen toegang tot webtoepassingen toe te staan en pachters Web-API's in uw Azure Active Directory.

- [OAuth 2.0 protocol: wat zijn de impliciete verlening](active-directory-dev-understanding-oauth2-implicit-grant.md): meer informatie over de impliciete toestemming verlenen, en of dit geschikt is voor uw toepassing.

- [2.0 OAuth-protocol: Service oproepen met behulp van de referenties Client-Service](active-directory-protocols-oauth-service-to-service.md): subsidie voor de referenties OAuth 2.0-Client een webservice (een vertrouwelijke client) met een eigen referenties te verifiëren bij het aanroepen van een andere web-service in plaats van zich voordoen als een gebruiker toestaat. In dit scenario wordt de client is meestal een middle-tier-webservice, een daemon-service of website.

- [OpenID verbinding 1.0-protocol: verificatie- en](active-directory-protocols-openid-connect-code.md): de OpenID verbinding 1.0-protocol breidt OAuth 2.0 voor gebruik als een verificatieprotocol. Een clienttoepassing kan ontvangen een id_token voor het beheren van het proces-in of de autorisatie code stroom voor het ontvangen van een id_token en de autorisatie code verbeteren.

- [SAML 2.0 protocol verwijzing](active-directory-saml-protocol-reference.md): de SAML 2.0-protocol wordt gebruikt om een ervaring voor eenmalige aanmelding bieden aan hun gebruikers toepassingen.

- [1.2 WS-Federation-protocol](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory ondersteunt 1.2 aan de hand van de Web Services Federation versie 1.2 specificatie WS-Federation. Zie voor meer informatie over het document federation metagegevens [Metagegevens Federation](active-directory-federation-metadata.md).

- [Ondersteunde typen token en claim](active-directory-token-and-claims.md): U kunt deze handleiding te begrijpen en evalueren van de vorderingen in de tokens SAML 2.0 en JSON Web Tokens (JWT).

## <a name="videos"></a>Video 's

### <a name="build"></a>Bouwen

Deze presentaties overzicht over het ontwikkelen van toepassingen met Azure Active Directory zijn uitgerust met luidsprekers die rechtstreeks in het technische team werken. De presentaties die betrekking hebben op fundamentele onderwerpen, waaronder IDMaaS, verificatie, identity federation en eenmalige aanmelding.

- [Microsoft Identity: Staat van de Unie en de richting van de toekomst](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Identity management als een service voor moderne toepassingen](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Ontwikkelen van moderne webtoepassingen met Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Moderne native met Azure Active Directory-toepassingen ontwikkelen](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure vrijdag
[Azure vrijdag](https://azure.microsoft.com/documentation/videos/azure-friday/) is een terugkerend vrijdag 1:1 video-serie die gericht op te brengen u korte (10 – 15 minuten) met experts over allerlei onderwerpen Azure analyseren.  Gebruik de functie Filter voor Services op de pagina voor een overzicht van alle video's van Azure Active Directory.

- [Azure identiteit 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure identiteit 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure identiteit 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Sociale

- [Active Directory-Team blog](http://blogs.technet.com/b/ad/): de meest recente ontwikkelingen in de wereld van Azure Active Directory.

- [Azure Active Directory Graph Team blog](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory-informatie die specifiek is voor de Graph API.

- [Identiteit van de wolk](http://www.cloudidentity.net): gedachten op identiteitsbeheer als een service, van een principal Azure Active Directory PM.  

- [Azure Active Directory op Twitter](https://twitter.com/azuread): Azure Active Directory-meldingen in 140 tekens of minder.

## <a name="windows-server-on-premises-development"></a>Ontwikkeling van Windows Server op locatie
Zie voor instructies over het gebruik van Windows Server en Active Directory Federation Services (ADFS) ontwikkeling:

- [AD FS-scenario's voor ontwikkelaars](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): een overzicht van de AD FS-onderdelen en hoe het werkt, met details over de ondersteunde authentication/authorization-scenario's.
- [AD FS-scenario's](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): een lijst met artikelen voor overzicht die stapsgewijze instructies bieden over het implementeren van de bijbehorende verificatie/vergunning stromen.
