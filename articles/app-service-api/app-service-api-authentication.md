<properties
    pageTitle="Verificatie en machtiging van Apps in Azure App Service API | Microsoft Azure"
    description="Informatie over de services voor verificatie en autorisatie waarmee u Azure App Service API Apps."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Verificatie en machtiging van Apps in Azure App Service API

## <a name="overview"></a>Overzicht 

> [AZURE.NOTE] In dit onderwerp worden gemigreerd naar een geconsolideerde [App Service verificatie / vergunning](../app-service/app-service-authentication-overview.md) onderwerp Web en mobiele Apps API dekt.

Azure App Service biedt geïntegreerde services voor verificatie en autorisatie die [2.0 OAuth](#oauth) en [OpenID verbinding](#oauth)implementeren. In dit artikel beschrijft de services en opties die beschikbaar voor Apps in Azure App Service API zijn.

In het volgende diagram ziet u enkele belangrijke kenmerken van App-Service verificatie:

* Het verwerkt inkomende aanvragen API, wat betekent dat het werkt met elke taal of framework ondersteund door App-Service.
* Het biedt u verschillende opties voor hoeveel verificatie werkt u wilt doen in uw eigen code.
* Het werkt voor zowel eindgebruikers als service accountverificatie. 
* Vijf id-providers ondersteund: Azure Active Directory, Facebook, Google, Twitter en Microsoft-Account.
* Dit werkt hetzelfde voor API Apps, Web Apps en mobiele Apps.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Agnostic taal

App-Service verificatie verwerking gebeurt voordat aanvragen de API-app, wat betekent bereiken dat de verificatiefuncties werken voor API-toepassingen die zijn geschreven in een taal of kader.  De API kan worden gebaseerd op ASP.NET, Java, Node.js of een framework dat App-Service ondersteunt.

App-Service doorgeeft op het JSON web-token (JWT) in de koptekst van de vergunning van een HTTP-aanvraag en programmacode die is geschreven in een taal of een kader krijgt de benodigde gegevens uit het token. Bovendien App Service geeft u gemakkelijker toegang tot de meest gebruikte claims door enkele speciale koppen, zoals de volgende:

* X-MS-CLIENT-PRINCIPAL-NAAM
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
In een .NET API, kunt u de `Authorize` van het kenmerk en code op basis van de claims toegesneden vergunning kunt u gemakkelijk schrijven omdat claims gegevens in klassen van .NET is gevuld.

## <a name="multiple-protection-options"></a>Meerdere opties voor de beveiliging

App-Service kunt voorkomen dat anonieme HTTP-aanvragen voor het bereiken van uw app API kan doorgeven aan alle aanvragen en valideren van tokens voor aanvragen die ze bevatten of kunnen door alle verzoeken alle zonder actie te ondernemen op deze:

1. Alleen geverifieerde aanvragen toestaan om uw app API te bereiken.

    Als een anonieme aanvraag wordt ontvangen van een browser, App-Service worden omgeleid naar een aanmeldingspagina voor verificatie (Azure AD, Google, Twitter, enz.) die u kiest. 

    Met deze optie, moet u niet helemaal een verificatiecode te schrijven in uw app en autorisatiecode is vereenvoudigd, omdat de belangrijkste vorderingen u in de HTTP-headers vindt.

2. Alle aanvragen toestaan om uw app API bereiken maar geverifieerde aanvragen worden gevalideerd en geven verificatie-informatie in de HTTP-headers.

    Deze optie biedt u meer flexibiliteit bij het anonieme aanvragen, maar u hebt om code te schrijven als u wilt voorkomen dat anonieme gebruikers met behulp van de API. Omdat de meest populaire claims worden doorgegeven in de headers van HTTP-aanvragen, is autorisatiecode relatief eenvoudig.
    
3. Alle aanvragen toestaan om de API te bereiken, geen actie ondernemen op verificatie-informatie in de aanvragen.

    Deze optie blijven de taken van de verificatie en machtiging helemaal tot aan de code van de toepassing.

In [Azure portal](https://portal.azure.com/), selecteert u de gewenste optie in de **Authentication / Authorization** blade.

![](./media/app-service-api-authentication/authblade.png)

Voor de opties 1 en 2, **App Service verificatie**inschakelen en kies in de vervolgkeuzelijst **actie moet worden ondernomen wanneer de aanvraag wordt niet geverifieerd** **Log in** of **aanvraag toestaan (geen actie)**.  Als u **zich aanmeldt**, hebt u een verificatieprovider kiezen en configureren van die provider.

![](./media/app-service-api-authentication/actiontotake.png)

Zie voor gedetailleerde informatie over het configureren van verificatie, [het configureren van uw App-servicetoepassing Azure Active Directory-aanmelding gebruiken](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Het artikel is van toepassing op API apps zoals mobiele apps en koppelingen naar andere artikelen voor de verificatieproviders.
 
## <a id="internal"></a>Verificatie van service

Verificatie van App-Service werkt voor interne scenario's, zoals voor het bellen vanaf één API app naar een andere toepassing van de API. In dit scenario krijgt u een token met behulp van referenties voor een service-account in plaats van de referenties van de gebruiker. Een service-account is ook bekend als een *service principal* in Azure Active Directory en verificatie met behulp van een dergelijke account wordt ook wel een scenario Services. 

De naam API app met Azure Active Directory beveiligen voor Services scenario's en AAD service principal Autorisatietoken bieden bij het aanroepen van de API-app. Door middel van de client-ID en het geheim van de toepassing AAD client krijgt u een token. Geen speciale alleen Azure-code is vereist, zoals die worden gebruikt voor het verwerken van het token mobiele Services Zumo waar. Een voorbeeld van dit scenario met behulp van ASP.NET-API apps wordt gedekt door de zelfstudie [Service principal verificatie voor API Apps](app-service-api-dotnet-service-principal-auth.md).

Als u wilt dat een service-service-scenario verwerken zonder verificatie van App-Service, kunt u clientcertificaten of basisverificatie. Zie voor informatie over clientcertificaten in Azure, [Hoe te configureren TLS wederzijdse verificatie voor Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Zie voor informatie over basisverificatie in ASP.NET, [Filters voor verificatie in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Service verificatie van een App Service logica app naar een API-app is een speciaal geval dat wordt beschreven in [werken met uw aangepaste API op App Service met logica apps gehost](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobiele client-verificatie

Zie de [documentatie over verificatie voor mobiele toepassingen](../app-service-mobile/app-service-mobile-ios-get-started-users.md)voor meer informatie over het afhandelen van verificatie van mobiele clients. Verificatie van App-Service werkt op dezelfde manier voor mobiele toepassingen en API apps.
  
## <a name="more-information"></a>Meer informatie

Zie de volgende bronnen voor meer informatie over verificatie en autorisatie in Azure App-Service:

* [Groeiende App Service verificatie / vergunning](/blog/announcing-app-service-authentication-authorization/)
* [Het configureren van uw App-servicetoepassing Azure Active Directory-aanmelding gebruiken](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Inclusief koppelingen voor andere verificatieproviders boven aan de pagina.) 

Zie de volgende bronnen voor meer informatie over 2.0 OAuth OpenID verbinding en JSON Web Tokens (JWT).

* [Aan de slag met OAuth 2.0] (http://shop.oreilly.com/product/0636920021810.do "Aan de slag met OAuth 2.0") 
* [Inleiding tot OAuth2, OpenID verbinden en JSON Web Tokens (JWT) - cursus van PluralSight](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Bouwen en beveiligen van een RESTful API voor meerdere Clients in ASP.NET - cursus van PluralSight](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Zie de volgende bronnen voor meer informatie over Azure Active Directory.

* [Azure AD-scenario 's](http://aka.ms/aadscenarios)
* [Azure AD ontwikkelaarshandleiding](http://aka.ms/aaddev)
* [Azure AD monsters](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Volgende stappen

Dit artikel heeft functies voor verificatie en autorisatie van App-Service die u voor API apps gebruiken kunt uitgelegd. De volgende zelfstudie in de ophalen gestarte serie laat zien hoe [de gebruikersverificatie in App Service API Apps](app-service-api-dotnet-user-principal-auth.md)implementeren.
