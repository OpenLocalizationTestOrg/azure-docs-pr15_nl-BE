<properties
    pageTitle="Azure AD v2.0 .NET Web API | Microsoft Azure"
    description="Hoe u een .NET MVC Web Api die tokens van beide persoonlijke Microsoft-Account accepteert en rekeningen voor werk of school."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Beveiliging van een MVC web API

Kunt u een Web-API gebruikt [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) toegangstokens, zodat gebruikers met persoonlijke Microsoft-account en werk beschermen met Azure Active Directory het eindpunt v2.0 of school rekeningen voor veilig toegang tot uw Web-API.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

In ASP.NET web API's, kunt u dit doen met behulp van Microsoft OWIN middleware is opgenomen in .NET Framework 4.5.  We gebruiken hier OWIN bouwen van een "To Do List" MVC Web API waarmee clients kunnen maken en lezen van taken uit de takenlijst van de gebruiker.  Het web API worden gevalideerd dat binnenkomende aanvragen een geldig access token bevatten en afkeuren van verzoeken die niet zijn gevalideerd op een beveiligde route.  In dit voorbeeld is gemaakt met behulp van Visual Studio 2015.

## <a name="download"></a>Downloaden
De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) of het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

De overgebleven app bevat alle generieke code voor een eenvoudige API, maar alle van de stukken betrekking hebben op identiteit ontbreekt. Als u niet volgen wilt, kunt u in plaats daarvan kopiëren of [downloaden van het voltooide monster](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Registreren van een app
Maak een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.

Deze visual studio-oplossing bevat ook een 'TodoListClient', dat een eenvoudige WPF app is.  De TodoListClient wordt gebruikt om aan te tonen hoe een gebruiker tekenen in- en hoe een client aanvragen kan uitgeven aan uw Web-API.  In dit geval worden zowel de TodoListClient als de TodoListService vertegenwoordigd door de dezelfde app.  De TodoListClient configureren, moet u ook:

- Het **mobiele** platform voor uw app toevoegen.


## <a name="install-owin"></a>Installeren van OWIN

Nu u een app hebt geregistreerd, moet u uw app instellen om te communiceren met het eindpunt v2.0 om binnenkomende aanvragen & tokens te valideren.

- Als u wilt, opent u de oplossing en de OWIN middleware NuGet pakketten toevoegen aan het TodoListService-project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuth-verificatie configureren

- Een klasse OWIN opstarten toevoegen aan het project met de naam TodoListService `Startup.cs`.  Rechts klikken op het project **toevoegen**--> --> **Nieuw Item** zoeken naar "OWIN"-->.  De OWIN middleware roept de `Configuration(…)` methode wanneer de toepassing wordt gestart.
- De klasse aangifte wijzigen `public partial class Startup` -we hebben u al deel uit van deze klasse geïmplementeerd in een ander bestand.  In de `Configuration(…)` methode, een aanroep van ConfgureAuth(...) voor het instellen van verificatie voor uw web app.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(…)` methode, die de Web-API wordt ingesteld op tokens van het eindpunt v2.0 accepteren.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Nu kunt u `[Authorize]` kenmerken ter bescherming van uw domeincontrollers en acties met OAuth 2.0 aan toonder-verificatie.  Versieren de `Controllers\TodoListController.cs` klasse met een tag machtigen.  Zorgt u ervoor dat de gebruiker aan te melden voor de toegang tot die pagina.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Wanneer een gemachtigde beller is Hiermee wordt een van de `TodoListController` -API's, de actie mogelijk toegang tot informatie over de beller.  OWIN biedt toegang tot de vorderingen binnen het token aan toonder via de `ClaimsPrincpal` object.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Ten slotte opent de `web.config` bestand in de hoofdmap van het project TodoListService en voert u de configuratiewaarden in de `<appSettings>` sectie.
  - De `ida:Audience` is de **Toepassings-Id** van de toepassing die u hebt ingevoerd in de portal.

## <a name="configure-the-client-app"></a>De clienttoepassing configureren
Voordat u de Service Todo lijst in actie zien kunt, moet u de Todo lijst Client configureren zodat deze tokens krijgen vanuit het eindpunt versie 2.0 aanroepen naar de service.

- Open in het project TodoListClient `App.config` en voert u de configuratiewaarden in de `<appSettings>` sectie.
  - Uw `ida:ClientId` toepassings-Id die u hebt gekopieerd vanaf de portal.

Ten slotte, schoon, bouwen en uitvoeren van elk project!  U hebt nu een .NET MVC Web API die werk of school accounts en tokens van beide persoonlijke accounts van Microsoft accepteert.  Inloggen op de TodoListClient, en roept u uw web api taken toevoegen aan de takenlijst van de gebruiker.

Voor een verwijzing naar kunt het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [wordt geleverd als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip)of u klonen het van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Volgende stappen
U kunt nu verplaatsen naar andere onderwerpen.  U kunt proberen:

[Een Web API aanroepen vanuit een Web App >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Aanvullende bronnen voor uitchecken:
- [De handleiding voor ontwikkelaars van versie 2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow 'azure active directory'-code >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door [deze pagina](https://technet.microsoft.com/security/dd252948) te bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
