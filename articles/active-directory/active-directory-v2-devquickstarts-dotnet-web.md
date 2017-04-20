<properties
    pageTitle="Azure AD v2.0 .NET Web App | Microsoft Azure"
    description="Hoe u een .NET MVC Web App dat gebruikers met beide persoonlijke Microsoft-Account aangemeld en rekeningen voor werk of school."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Aanmelden met een .NET MVC web app toevoegen

Met het eindpunt v2.0 kunt u snel werk of school-accounts en verificatie van uw web-apps met ondersteuning voor zowel persoonlijke Microsoft-accounts toevoegen.  In ASP.NET web apps, kunt u dit doen met behulp van Microsoft OWIN middleware is opgenomen in .NET Framework 4.5.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

 Hier zullen we een web app die gebruikmaakt van OWIN op aanmelden van de gebruiker bepaalde informatie weergeven over de gebruiker en het aanmelden van de gebruiker van de app bouwen.
 
 ## <a name="download"></a>Downloaden
De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) of het skelet klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

De voltooide toepassing wordt geleverd aan het einde van deze zelfstudie ook.

## <a name="register-an-app"></a>Registreren van een app
Maak een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- Het **Web** platform voor uw app toevoegen.
- Voer de juiste **Redirect URI**. Geeft de redirect uri Azure AD waarin verificatie reacties moeten worden gericht - de standaardwaarde voor deze zelfstudie is `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installeren en configureren van de verificatie van OWIN
Hier zullen we de OWIN middleware voor het gebruik van het verificatieprotocol OpenID verbinding configureren.  OWIN wordt gebruikt voor aanmeldings- en afmeldingstijden aanvragen en informatie over de gebruiker, onder andere gebruikerssessies beheren.

-   Open om te beginnen, de `web.config` in de hoofdmap van het project-bestand en voert u de configuratiewaarden van uw app in de `<appSettings>` sectie.
    -   De `ida:ClientId` is de **Toepassings-Id** toegewezen aan uw app in de portal voor registratie.
    -   De `ida:RedirectUri` wordt de **Uri omgeleid** u hebt ingevoerd in de portal.

-   Voeg vervolgens de OWIN middleware NuGet pakketten aan het project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Een "OWIN Opstartklasse toevoegen" aan het project genaamd `Startup.cs` rechts klikken op het project **toevoegen**--> --> **Nieuw Item** zoeken naar "OWIN"-->.  De OWIN middleware roept de `Configuration(...)` methode wanneer de toepassing wordt gestart.
-   De klasse aangifte wijzigen `public partial class Startup` -we hebben u al deel uit van deze klasse geïmplementeerd in een ander bestand.  In de `Configuration(...)` methode, een aanroep van ConfigureAuth(...) voor het instellen van verificatie voor uw web app  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIdConnectAuthenticationOptions` dient als coördinaten voor uw app om te communiceren met Azure Active Directory.  U moet ook Cookie-verificatie instellen - de middleware OpenID verbinding maakt gebruik van cookies onder de Kaften.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Verificatieaanvragen verzenden
Uw app is nu geconfigureerd om te communiceren met de v2.0 eindpunt met het verificatieprotocol OpenID verbinden.  OWIN heeft afgehandeld alle lelijke details verificatieberichten samenstellen, tokens van Azure AD valideren en onderhouden van de gebruikerssessie.  Het enige dat blijft is om uw gebruikers zich aanmelden en afmelden.

- U kunt codes op uw domeincontrollers vereisen dat de gebruiker zich aanmeldt voor de toegang tot een bepaalde pagina staan.  Open `Controllers\HomeController.cs`, en voeg de `[Authorize]` tag op de Info-controller.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   U kunt ook OWIN gebruiken om rechtstreeks verificatieaanvragen van uw code.  Open `Controllers\AccountController.cs`.  Afgifte respectievelijk OpenID verbinding vraag- en afmeldingstijden aanvragen, in de SignIn() en SignOut()-acties.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Open nu `Views\Shared\_LoginPartial.cshtml`.  Dit is waar u de gebruiker van uw app aanmeldings- en afmeldingstijden koppelingen weergeven, en de naam van de gebruiker in een weergave afdrukken.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="display-user-information"></a>Gebruikersgegevens weergeven
Bij het verifiëren van gebruikers met een OpenID verbinding, wordt het eindpunt v2.0 een id_token App die [claims](active-directory-v2-tokens.md#id_tokens)of verklaringen over de gebruiker bevat.  U kunt deze claims personaliseren van uw app:

- Open de `Controllers\HomeController.cs` bestand.  U hebt toegang tot de vorderingen van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` beveiligings-principal.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Uitvoeren

Ten slotte bouwen en uitvoeren van uw app!   Meld u aan met een persoonlijke Microsoft-Account of een account voor werk of school en u ziet hoe de identiteit van de gebruiker wordt weergegeven in de bovenste navigatiebalk.  U hebt nu een web app beveiligd met behulp van industriestandaardprotocollen die gebruikers met hun persoonlijke en werk/school accounts kunnen worden geverifieerd.

Voor een verwijzing naar kunt het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [wordt geleverd als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip)of u klonen het van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar de meer geavanceerde onderwerpen.  U kunt proberen:

[Beveiliging van een Web-API met de het eindpunt v2.0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Aanvullende bronnen voor uitchecken:
- [De handleiding voor ontwikkelaars van versie 2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow 'azure active directory'-code >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door [deze pagina](https://technet.microsoft.com/security/dd252948) te bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
