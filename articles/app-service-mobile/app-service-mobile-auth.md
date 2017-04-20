<properties
    pageTitle="Verificatie en autorisatie in Azure mobiele Apps | Microsoft Azure"
    description="Overzicht van de verificatie en conceptuele verwijzing / vergunning functie voor Azure mobiele Apps"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Verificatie en autorisatie in Azure mobiele Apps

## <a name="what-is-app-service-authentication--authorization"></a>Wat is App Service verificatie / vergunning?

> [AZURE.NOTE] In dit onderwerp worden gemigreerd naar een geconsolideerde [App Service verificatie / vergunning](../app-service/app-service-authentication-overview.md) onderwerp Web en mobiele Apps API dekt.

App-Service verificatie / vergunning is een functie waarmee gebruikers zich aanmelden zonder code wijzigingen vereist op de backend app van uw toepassing. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker.

App-Service maakt gebruik van federatieve identiteiten, waarin een 3de partij **identiteitsprovider** ("IDP") accounts worden opgeslagen en worden gebruikers geverifieerd en de toepassing deze identiteit gebruikt in plaats van een eigen. App-Service ondersteunt vijf id-providers uit de doos: _Azure Active Directory_, _Facebook_, _Google_, _Microsoft-Account_en _Twitter_. U kunt ook ondersteuning voor uw toepassingen vergroten door een andere id-provider of uw eigen aangepaste identiteit oplossing integreren.

Uw app kunt profiteren van een aantal identiteitsaanbieders van deze, zodat u uw eindgebruikers in de opties voorzien kan voor de manier waarop ze zich aanmelden.

Als u meteen aan de slag wilt, raadpleegt u een van de volgende cursussen:

- [Verificatie toevoegen aan uw iOS-app]
- [Verificatie toevoegen aan uw app Xamarin.iOS]
- [Verificatie toevoegen aan uw app Xamarin.Android]
- [Verificatie toevoegen aan uw Windows-app]

## <a name="how-authentication-works"></a>De werking van verificatie

Om te verifiëren met behulp van een van de id-providers, moet u eerst de id-provider moet weten over uw toepassing configureren. De identiteitsprovider vervolgens krijgt u id's en geheimen die u terug naar de toepassing. Dit is de vertrouwensrelatie voltooid en kan valideren identiteiten die deze App Service.

Deze stappen worden beschreven in de volgende onderwerpen:

- [Het configureren van uw app voor het gebruik van Azure Active Directory-aanmelding]
- [Het configureren van uw app voor het gebruik van Facebook login]
- [Het configureren van uw app voor het gebruik van Google-aanmelding]
- [Het configureren van uw app voor het gebruik van Microsoft-Account aanmelden]
- [Het configureren van uw app voor het gebruik van Twitter login]

Zodra alles op de backend is geconfigureerd, kunt u uw client aan te melden. Er zijn hier twee benaderingen:

- Met behulp van een enkele regel code, kunt u de mobiele Apps client SDK gebruikers aanmelden.
- Maak gebruik van een SDK die wordt uitgegeven door een bepaalde identiteitsprovider identiteit en vervolgens toegang krijgen tot App-Service.

>[AZURE.TIP] De meeste toepassingen moeten een provider SDK gebruiken een eigen gevoel aanmelding ervaring en ondersteuning voor vernieuwen en andere provider-specifieke voordelen.

### <a name="how-authentication-without-a-provider-sdk-works"></a>De werking van verificatie zonder een provider SDK

Als u niet instellen van de SDK van een provider wilt, kunt u mobiele Apps voor de aanmelding voor u uitvoeren. De mobiele Apps client SDK openen een webweergave aan de provider van uw keuze en vul het aanmelden. Af en toe op blogs en forums, u dit genoemd als ziet "flow server" of 'server-gestuurde stroom' sinds de server is het beheren van de aanmelding en de client-SDK nooit het token provider ontvangt.

De code die nodig is voor het starten van deze overdracht wordt in de zelfstudie verificatie voor elk platform gedekt. Aan het einde van de stroom, de client-SDK heeft een token App Service en het token worden automatisch gekoppeld aan alle aanvragen voor de back-end.

### <a name="how-authentication-with-a-provider-sdk-works"></a>De werking van verificatie met een provider SDK

Werken met een provider kan SDK de ervaring aanmelden dichter interactie met het platform OS dat op de toepassing wordt uitgevoerd. Dit kunt u ook een token provider en gebruikersgegevens op de client, waardoor het veel gemakkelijker te verbruiken graph API's en de gebruikerservaring aanpassen. Af en toe op blogs en forums ziet u dit genoemd de stroom' client' of 'client omgeleid flow"sinds de code op de client is aanmelding bij de verwerking en de clientcode toegang heeft tot een token provider.

Als u een token provider is verkregen, moet deze worden verzonden naar App Service voor validatie. Aan het einde van de stroom, de client-SDK heeft een token App Service en het token worden automatisch gekoppeld aan alle aanvragen voor de back-end. De ontwikkelaar kan ook een verwijzing naar het token provider behouden desgewenst.

## <a name="how-authorization-works"></a>Werking van machtiging

App-Service verificatie / vergunning beschrijft verschillende opties voor de **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd**. Voordat u uw code een bepaalde aanvraag ontvangt, kunt u laten App-Service wordt gecontroleerd om te zien als de aanvraag geverifieerd en als niet weigeren en proberen om de gebruiker zich aanmelden voordat u opnieuw probeert.

Een optie is het hebben van niet-geverifieerde aanvragen omleiden naar een van de id-providers. Dit zou worden de gebruiker naar een nieuwe pagina in een webbrowser. Echter uw mobiele client op deze manier niet worden omgeleid en niet-geverifieerde antwoorden ontvangt een antwoord HTTP _401 Unauthorized_ . Dit gegeven, de eerste aanvraag, de client maakt moet altijd het eindpunt van de aanmelding en vervolgens kunt u andere API's aanroepen. Als u een andere API aanroepen probeert voordat u zich aanmeldt, ontvangt de client een foutmelding.

Als u meer gedetailleerde wilt laten bepalen via welke eindpunten verificatie vereist is, kunt u kiezen "geen actie (toestaan aanvraag)" voor niet-geverifieerde aanvragen. In dit geval worden alle besluiten van verificatie uitgesteld uw programmacode. Dit kunt u ook toegang tot specifieke gebruikers op basis van aangepaste verificatieregels.

## <a name="documentation"></a>Documentatie

De volgende zelfstudies weergeven verificatie toevoegen aan uw mobiele clients met App-Service:

- [Verificatie toevoegen aan uw iOS-app]
- [Verificatie toevoegen aan uw app Xamarin.iOS]
- [Verificatie toevoegen aan uw app Xamarin.Android]
- [Verificatie toevoegen aan uw Windows-app]

De volgende zelfstudies weergeven App-Service als u wilt gebruikmaken van verschillende verificatieproviders configureren:

- [Het configureren van uw app voor het gebruik van Azure Active Directory-aanmelding]
- [Het configureren van uw app voor het gebruik van Facebook login]
- [Het configureren van uw app voor het gebruik van Google-aanmelding]
- [Het configureren van uw app voor het gebruik van Microsoft-Account aanmelden]
- [Het configureren van uw app voor het gebruik van Twitter login]

Als u wilt dat een systeem identiteit dan die hier gebruiken, kunt u ook gebruikmaken van de [ondersteuning voor aangepaste verificatie op de server .NET SDK bekijken](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Verificatie toevoegen aan uw iOS-app]: app-service-mobile-ios-get-started-users.md
[Verificatie toevoegen aan uw app Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started-users.md
[Verificatie toevoegen aan uw app Xamarin.Android]: app-service-mobile-xamarin-android-get-started-users.md
[Verificatie toevoegen aan uw Windows-app]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Het configureren van uw app voor het gebruik van Azure Active Directory-aanmelding]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Het configureren van uw app voor het gebruik van Facebook login]: app-service-mobile-how-to-configure-facebook-authentication.md
[Het configureren van uw app voor het gebruik van Google-aanmelding]: app-service-mobile-how-to-configure-google-authentication.md
[Het configureren van uw app voor het gebruik van Microsoft-Account aanmelden]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Het configureren van uw app voor het gebruik van Twitter login]: app-service-mobile-how-to-configure-twitter-authentication.md
