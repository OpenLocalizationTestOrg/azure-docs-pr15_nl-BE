<properties
    pageTitle="Azure AD .NET introductie | Microsoft Azure"
    description="Het bouwen van een .NET MVC Web API met Azure AD voor verificatie en machtiging integreert."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Bescherming van een Web-API aan toonder tokens uit Azure Active Directory gebruiken

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Als u een toepassing die toegang tot beveiligde bronnen geeft maakt moet u weten hoe deze bronnen beveiligen tegen onrechtmatige toegang.
Azure AD maakt het eenvoudig en ongecompliceerd is een web-API gebruikt OAuth aan toonder 2.0 toegangstokens met slechts een paar regels code beschermen.

In Asp.NET web apps, kunt u dit doen met behulp van de Microsoft implementatie van de OWIN Gemeenschap aangestuurde middleware opgenomen in .NET Framework 4.5.  Hier gebruiken we de OWIN te maken van een 'Takenlijst' web API die:
-   Geeft aan welke API's worden beschermd.
-   Dat de Web-API-oproepen een geldig Access Token bevatten worden gevalideerd.

Hiervoor moet u naar:

1. Een toepassing met Azure Active Directory registreren
2. De app gebruikt de pijpleiding OWIN verificatie instellen.
3. Configureer een clienttoepassing aan de te doen lijst Web API aanroepen

Ophalen is gestart, [het skelet app downloaden](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) of [het voltooide monster](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Elk is een oplossing met Visual Studio 2013.  U moet ook een huurder Azure AD waarin om uw toepassing te registreren.  Als u geen sjabloon hebt, [Lees hoe u een](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. een toepassing met Azure Active Directory registreren*
Beveiliging van uw toepassing, moet u eerst een toepassing maken in uw huurder en voorzien van een paar belangrijke stukjes informatie Azure AD.

-   Aanmelden bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de nav links op in **Active Directory**
-   Selecteer een huurder waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klik op **toevoegen** in de onderste lade.
-   Volg de aanwijzingen op het scherm en maak een nieuwe **webtoepassing en/of WebAPI**.
    -   De **naam** van de toepassing wordt uw toepassing om eindgebruikers te beschrijven.  Voer "Lijstjes Service".
    -   Het **Omleiden van Uri** is een schema en tekenreeks van de combinatie die Azure AD gebruiken zou om terug te keren de tokens uw app aangevraagd. Voer `https://localhost:44321/` voor deze waarde.
-   Als u de registratie hebt voltooid, gaat u naar het tabblad **configureren** en zoek het **App-ID URI** veld.  Invoeren van een huurder-specifieke id voor deze waarde, bv.`https://contoso.onmicrosoft.com/TodoListService`
- De configuratie op te slaan.  Laat de portal open - u moet ook uw clienttoepassing kort te registreren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. instellen van uw app de pijpleiding OWIN verificatie gebruiken*

Nu u een toepassing hebt geregistreerd met Azure Active Directory, moet u voor het instellen van uw toepassing communiceren met Azure Active Directory om te valideren van binnenkomende aanvragen & tokens.

-   Als u wilt, opent u de oplossing en de OWIN middleware NuGet pakketten toevoegen aan het TodoListService-project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Een klasse OWIN opstarten toevoegen aan het project met de naam TodoListService `Startup.cs`.  Rechts klikken op het project **toevoegen**--> --> **Nieuw Item** zoeken naar "OWIN"-->.  De OWIN middleware roept de `Configuration(…)` methode wanneer de toepassing wordt gestart.
-   De klasse aangifte wijzigen `public partial class Startup` -we hebben u al deel uit van deze klasse geïmplementeerd in een ander bestand.  In de `Configuration(…)` methode, een aanroep van ConfgureAuth(...) voor het instellen van verificatie voor uw web app.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(…)` methode.  De parameters die u opgeeft in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` dient als coördinaten voor uw app om te communiceren met Azure Active Directory.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Nu kunt u `[Authorize]` kenmerken ter bescherming van uw domeincontrollers en acties met JWT aan toonder-verificatie.  Versieren de `Controllers\TodoListController.cs` klasse met een tag machtigen.  Zorgt u ervoor dat de gebruiker aan te melden voor de toegang tot die pagina.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Wanneer een gemachtigde beller is Hiermee wordt een van de `TodoListController` -API's, de actie mogelijk toegang tot informatie over de beller.  OWIN biedt toegang tot de vorderingen binnen het token aan toonder via de `ClaimsPrincpal` object.  
- Een algemene vereiste voor web-API's te valideren "scopes" in het token aanwezig is - dit zorgt ervoor dat de gebruiker heeft ingestemd met de vereiste machtigingen voor toegang tot de Service Todo-lijst:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Ten slotte opent de `web.config` bestand in de hoofdmap van het project TodoListService en voert u de configuratiewaarden in de `<appSettings>` sectie.
  - De `ida:Tenant` is de naam van de huurder Azure AD, bijvoorbeeld 'contoso.onmicrosoft.com'.
  - De `ida:Audience` wordt de URI van de App-ID van de toepassing die u hebt ingevoerd in de Portal Azure.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. een clienttoepassing configureren en uitvoeren van de service*
Voordat u de Service Todo lijst in actie zien kunt, moet u de Todo lijst Client configureren zodat deze tokens krijgen van AAD gesprekken met de service.

- Ga terug naar de [Azure Management Portal](https://manage.windowsazure.com)
- Maak een nieuwe toepassing in uw huurder Azure AD en **Native Client-toepassing** te selecteren in de resulterende prompt.
    -   De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    -   Voer `http://TodoListClient/` voor het **Omleiden van Uri** -waarde.
- Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke **Client-Id**. Moet u deze waarde in de volgende stappen dus kopiëren vanaf het tabblad configureren.
- Ook in het tabblad **configureren** , Ga naar de sectie 'Machtigingen voor andere toepassingen'. Klik op 'Toepassing toevoegen'. Selecteer 'Alle Apps' in de vervolgkeuzelijst 'Show' en klikt u op de bovenste vinkje. Klik op de te doen lijst Service en klikt u op het selectievakje onder de toepassing toevoegen. Selecteer 'Toegang te doen lijst Service' in de vervolgkeuzelijst 'Overgedragen machtigingen' en de configuratie op te slaan.


- Open in Visual Studio `App.config` in het TodoListClient project en voert u de configuratiewaarden in de `<appSettings>` sectie.
  - De `ida:Tenant` is de naam van de huurder Azure AD, bijvoorbeeld 'contoso.onmicrosoft.com'.
  - Uw `ida:ClientId` app-ID die u hebt gekopieerd vanaf de Portal Azure.
  - De `todo:TodoListResourceId` wordt de URI van de App-ID van de toepassing te doen lijst-Service die u hebt ingevoerd in de Portal Azure.

Ten slotte, schoon, bouwen en uitvoeren van elk project!  Als u nog niet gedaan hebt, nu is de tijd voor het maken van een nieuwe gebruiker in uw huurder met een *. onmicrosoft.com domein.  Aanmelden bij de client takenlijst met die gebruiker en sommige taken toevoegen aan de lijst van de gebruiker te doen.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  U kunt nu meer scenario's voor extra identiteit op verplaatsen.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
