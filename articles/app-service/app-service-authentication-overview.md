<properties
    pageTitle="Verificatie en autorisatie in Azure App Service | Microsoft Azure"
    description="Overzicht van de verificatie en conceptuele verwijzing / vergunning beschikken voor Azure App Service"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App-Service

## <a name="what-is-app-service-authentication--authorization"></a>Wat is App Service verificatie / vergunning?

App-Service verificatie / vergunning is een functie waarmee uw toepassing gebruikers aanmelden zodat u niet hoeft te wijzigen code op de backend app. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker.

App-Service gebruikt een federatieve identiteit, waarbij een derde partij identiteitsprovider accounts worden opgeslagen en worden gebruikers geverifieerd. De toepassing afhankelijk is van de identiteitsgegevens van de provider, zodat de toepassing niet hoeft op te slaan die informatie zelf. App-Service ondersteunt vijf id-providers uit de doos: Azure Active Directory, Facebook, Google, Twitter en Microsoft-Account. Uw app kunt een willekeurig aantal aanbieders van deze identiteit aan uw gebruikers met opties voor de manier waarop ze zich aanmelden. Vouw de ingebouwde ondersteuning, kunt u een andere id-provider of [uw eigen aangepaste identiteit oplossing]integreren[custom-auth].

Als u meteen aan de slag wilt, raadpleegt u een van de volgende zelfstudies:

- [Verificatie toevoegen aan uw iOS-app] [ iOS] (of [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]of [Cordova])
- [Gebruikersverificatie voor Apps in Azure App Service API][apia-user]
- [Aan de slag met Azure App Service - deel 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>De werking van verificatie in de App-Service

Om te verifiëren met behulp van een van de id-providers, moet u eerst de id-provider moet weten over uw toepassing configureren. De identiteitsprovider krijgt vervolgens-id's en geheimen die u aan App-Service verstrekt. Hiermee wordt de vertrouwensrelatie zodat App Service gebruiker beweringen, zoals verificatietokens van de identiteitsprovider kan valideren.

Als een gebruiker met een van deze providers aanmelden, moet de gebruiker worden omgeleid naar een eindpunt dat gebruikers zich voor de provider aanmeldt. Als klanten met een webbrowser, kunt u App-Service automatisch alle niet-geverifieerde gebruikers verwijzen naar het eindpunt dat gebruikers zich aanmeldt hebt. Anders moet u uw klanten direct `{your App Service base URL}/.auth/login/<provider>`, waarbij `<provider>` is een van de volgende waarden: aad, facebook, google, microsoft of twitter. Mobiele en API's worden in de secties verderop in dit artikel beschreven.

Gebruikers die met de toepassing via een webbrowser werken hebben een cookie ingesteld zodat ze geverifieerd als ze de toepassing bladeren. Voor andere clienttypen zoals mobiel, een JSON-web-token (JWT), die dienen te worden gepresenteerd de `X-ZUMO-AUTH` kop wordt uitgegeven aan de client. De mobiele Apps client SDK's verwerken dit voor u. U kunt ook een token voor Azure Active Directory-identiteit of een toegangstoken mogelijk direct opgenomen in de `Authorization` kop als een [token aan toonder](https://tools.ietf.org/html/rfc6750).

App-Service wordt gevalideerd een cookie of een token dat uw toepassing problemen om gebruikers te verifiëren. Om te beperken wie toegang heeft tot uw toepassing, raadpleegt u de sectie [vergunning](#authorization) verderop in dit artikel.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobiele verificatie bij een provider SDK

Nadat alles op de backend is geconfigureerd, kunt u mobiele clients zich aanmelden met de App-Service wijzigen. Er zijn hier twee benaderingen:

- Gebruik een SDK die een bepaald id-provider publiceert om de identiteit en vervolgens toegang krijgen tot App-Service.
- Een regel code gebruiken zodat de mobiele Apps client SDK gebruikers kunt aanmelden.

>[AZURE.TIP] De meeste toepassingen moeten een provider SDK gebruiken om een meer consistente ervaring wanneer gebruikers zich aanmelden, vernieuwen ondersteuning gebruiken en andere voordelen die de provider aangeeft.

Als u een provider SDK, zich gebruikers aanmelden bij een ervaring die nauwer geïntegreerd met het besturingssysteem waarop de toepassing wordt uitgevoerd. Dit kunt u ook een token provider en gebruikersgegevens op de client, waardoor het veel gemakkelijker te verbruiken graph API's en de gebruikerservaring aanpassen. Af en toe op blogs en forums ziet u dit genoemd de stroom' client' of "client omgeleid flow" omdat de code op de client zich gebruikers en de clientcode aanmeldt toegang heeft tot een token provider.

Als een token provider staat, moet deze worden verzonden naar App Service voor validatie. Nadat het App-Service beoordeelt het token, maakt App-Service een nieuwe App Service token dat wordt geretourneerd naar de client. De mobiele Apps client SDK heeft methoden voor het beheren van deze uitwisseling en het token automatisch te koppelen aan alle aanvragen voor de toepassing back-end. Ontwikkelaars kunnen ook een verwijzing naar het token provider behouden desgewenst.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobiele verificatie zonder een provider SDK

Als u niet instellen van de SDK van een provider wilt, kunt u de functie voor mobiele Apps van Azure App Service aan te melden voor u. De mobiele Apps client SDK wordt een webweergave aan de provider van uw keuze opent en de gebruiker zich aanmeldt. Af en toe op blogs en forums ziet u dit genoemd de "flow server" of "server-gestuurde stroom" omdat de server beheert het proces dat gebruikers zich aanmeldt en de client-SDK nooit het token provider ontvangt.

Code voor het starten van deze overdracht is opgenomen in de zelfstudie verificatie voor elk platform. Aan het einde van de stroom, de client-SDK heeft een token App Service en het token worden automatisch gekoppeld aan alle aanvragen voor de toepassing back-end.

### <a name="service-to-service-authentication"></a>Verificatie Services

Hoewel u gebruikers toegang tot uw toepassing geven kunt, vertrouwt u ook een andere toepassing uw eigen API aan te roepen. U kan bijvoorbeeld een web app een API aanroept in een ander web app. In dit scenario kunt u referenties voor een service-account in plaats van de referenties van de gebruiker een token ophalen. Een service-account wordt ook wel een *service principal* in jargon Azure Active Directory en verificatie met behulp van een dergelijke account wordt ook wel een scenario Services.

>[AZURE.IMPORTANT] Omdat mobiele apps op klant-apparaten worden uitgevoerd, doet mobiele toepassingen _niet_ tellen als vertrouwde toepassingen, en moet een service principal stroom niet gebruiken. In plaats daarvan moeten ze gebruiken de stroom van een gebruiker die eerder is beschreven.

Services scenario's beveiligen App Service uw toepassing met Azure Active Directory. De aanroepende toepassing moet alleen Azure Active Directory service principal Autorisatietoken die wordt verkregen door de client-ID en het geheim van Azure Active Directory-client te bieden. Een voorbeeld van dit scenario met ASP.NET-API apps wordt verklaard door de zelfstudie, [Service principal verificatie voor API Apps][apia-service].

Als u verificatie gebruiken App-Service wilt voor het verwerken van een scenario Services, kunt u clientcertificaten of basisverificatie. Zie voor informatie over clientcertificaten in Azure, [Hoe te configureren TLS wederzijdse verificatie voor Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Zie voor informatie over basisverificatie in ASP.NET, [Filters voor verificatie in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Service verificatie van een App Service logica app naar een API-app is een speciaal geval dat wordt beschreven in [werken met uw aangepaste API op App Service met logica apps gehost](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="authorization"></a>De werking van machtiging in App-Service

U hebt volledige controle over de verzoeken die toegang heeft tot uw toepassing. App-Service verificatie / vergunning kan worden geconfigureerd met een van de volgende problemen:

- Alleen geverifieerde aanvragen toestaan om uw toepassing bereiken.

    Als een browser een anonieme aanvraag krijgt, wordt App Service omleiden naar een pagina voor de id-provider die u kiest, zodat gebruikers zich kunnen aanmelden. Als de aanvraag afkomstig van een mobiel apparaat is, is het geretourneerde antwoord een antwoord HTTP _401 Unauthorized_ .

    Met deze optie hoeft niet helemaal een verificatiecode te schrijven in uw app. Als u een fijnere vergunning nodig, is informatie over de gebruiker beschikbaar voor uw code.

- Toestaan dat alle aanvragen voor het bereiken van uw toepassing, maar doorgeven langs verificatie-informatie in de HTTP-headers geverifieerde aanvragen valideren.

    Deze optie past omleiding aan uw toepassingscode autorisatiebeslissingen te nemen. Biedt meer flexibiliteit bij het anonieme aanvragen, maar u hebt om code te schrijven.

- Alle aanvragen toestaan om uw toepassing te bereiken, en geen actie ondernemen op verificatie-informatie in de aanvragen.

    In dit geval, de verificatie / vergunning-functie is uitgeschakeld. De taken van de verificatie en autorisatie zijn geheel tot de code van uw toepassing.

Het vorige gedrag worden bestuurd door de optie **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** in de portal Azure. Als u * *zich in met *de providernaam* **, alle aanvragen moeten worden geverifieerd.** Aanvraag (geen actie) toestaan ** past omleiding het vergunningsbesluit aan uw code, maar biedt wel verificatie-informatie. Als u dat uw code verwerken alles wilt, kunt u de verificatie uitschakelen / machtigingsvoorziening.

## <a name="working-with-user-identities-in-your-application"></a>Werken met de identiteit van gebruikers in uw toepassing

App-Service geeft enkele gebruikersgegevens aan uw toepassing met behulp van speciale kop. Externe verzoeken verbieden deze headers en alleen aanwezig als ingesteld door App Service verificatie / vergunning. Enkele voorbeeld headers zijn:

* X-MS-CLIENT-PRINCIPAL-NAAM
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Code die is geschreven in een taal of kader krijgt de benodigde informatie uit deze headers. De **ClaimsPrincipal** is voor 4.6 voor ASP.NET-toepassingen automatisch ingesteld met de juiste waarden.

De toepassing ook aanvullende gegevens via een HTTP GET te vinden op de `/.auth/me` eindpunt van uw toepassing. Een geldige token die is opgenomen met het verzoek retourneert een JSON-nettolading met details over de provider die wordt gebruikt, de onderliggende provider token en andere gebruikersgegevens. De mobiele Apps server SDK's bieden methoden voor het werken met deze gegevens. Zie voor meer informatie [hoe u de SDK Azure Mobile Apps Node.js](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)en [werk met de back-end-server .NET SDK voor Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Documentatie en aanvullende bronnen

### <a name="identity-providers"></a>Id-providers
De volgende zelfstudies weergeven App-Service voor het gebruik van verschillende verificatieproviders configureren:

- [Het configureren van uw app voor het gebruik van Azure Active Directory-aanmelding][AAD]
- [Het configureren van uw app voor het gebruik van Facebook login][Facebook]
- [Het configureren van uw app voor het gebruik van Google-aanmelding][Google]
- [Het configureren van uw app voor het gebruik van Microsoft-Account aanmelden][MSA]
- [Het configureren van uw app voor het gebruik van Twitter login][Twitter]

Als u gebruiken een systeem identiteit dan die hier wilt, ook kunt u de [ondersteuning voor aangepaste verificatie in de mobiele Apps .NET server SDK bekijken][custom-auth], die kan worden gebruikt in web-apps, mobiele toepassingen of API apps.

### <a name="web-applications"></a>Webtoepassingen
De volgende zelfstudies weergeven verificatie toevoegen aan een webtoepassing:

- [Aan de slag met Azure App Service - deel 2][web-getstarted]

### <a name="mobile-applications"></a>Mobiele toepassingen
De volgende zelfstudies weergeven verificatie toevoegen aan uw mobiele clients met behulp van de server-gestuurde stroom:

- [Verificatie toevoegen aan uw iOS-app][iOS]
- [Verificatie van uw Android app toevoegen] [Android]
- [Verificatie van uw Windows-app toevoegen] [Windows]
- [Verificatie van uw app Xamarin.iOS toevoegen] [Xamarin.iOS]
- [Verificatie van uw app Xamarin.Android toevoegen] [Xamarin.Android]
- [Verificatie van uw app Xamarin.Forms toevoegen] [Xamarin.Forms]
- [Verificatie van uw app Cordova toevoegen] [Cordova]

Als u wilt dat de client omgeleid stroom gebruiken voor Azure Active Directory, gebruikt u de volgende bronnen:

- [De bibliotheek voor verificatie van Active Directory gebruiken voor iOS][ADAL-iOS]
- [De bibliotheek van Active Directory-verificatie gebruiken voor Android][ADAL-Android]
- [De bibliotheek van Active Directory-verificatie gebruiken voor Windows en Xamarin][ADAL-dotnet]

Als u wilt dat de client omgeleid stroom gebruiken voor Facebook, gebruikt u de volgende bronnen:

- [Gebruik de SDK Facebook voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Als u wilt dat de client omgeleid stroom gebruiken voor Twitter, gebruikt u de volgende bronnen:

- [Gebruik Twitter vervaardigd voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Als u wilt dat de client omgeleid stroom gebruiken voor Google, gebruikt u de volgende bronnen:

- [Gebruik de Google-In de SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API-toepassingen
De volgende zelfstudies voor weergeven het beveiligen van uw API apps:

- [Gebruikersverificatie voor Apps in Azure App Service API][apia-user]
- [Service principal verificatie voor Apps in Azure App Service API][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
