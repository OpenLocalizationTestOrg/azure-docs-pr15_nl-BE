<properties
    pageTitle="Azure AD .NET introductie | Microsoft Azure"
    description="Het bouwen van een .NET MVC Web App met Azure AD voor aanmelden integreert."
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

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.NET Web App aanmelden en afmelden met Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD maakt het eenvoudig en ongecompliceerd voor het uitbesteden van uw web app identiteitsbeheer, bieden één aanmeldings- en afmeldingstijden met slechts een paar regels code.  In Asp.NET web apps, kunt u dit doen met behulp van de Microsoft implementatie van de OWIN Gemeenschap aangestuurde middleware opgenomen in .NET Framework 4.5.  Hier gebruiken we de OWIN aan:
-   De gebruiker inloggen op de app met Azure AD als de id-provider.
-   Sommige informatie weergeven over de gebruiker.
-   De gebruiker van de toepassing ondertekenen.

Hiervoor moet u naar:

1. Een toepassing met Azure Active Directory registreren
2. De app gebruikt de pijpleiding OWIN verificatie instellen.
3. Gebruik de OWIN op teken- en afmeldingstijden aanvragen op Azure AD.
4. Gegevens over de gebruiker afdrukken.

Ophalen is gestart, [het skelet app downloaden](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) of [het voltooide monster](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  U moet ook een huurder Azure AD waarin om uw toepassing te registreren.  Als u geen sjabloon hebt, [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. een toepassing met Azure Active Directory registreren*
Als u wilt dat uw app om gebruikers te verifiëren, moet u eerst registreren van een nieuwe toepassing in uw huurder.

- Aanmelden bij de Azure Management Portal.
- Klik in de nav links op **Active Directory**.
- Selecteer de huurder waar u wilt dat om de toepassing te registreren.
- Klik op het tabblad **toepassingen** en klik op toevoegen in de onderste lade.
- Volg de aanwijzingen op het scherm en maak een nieuwe **webtoepassing en/of WebAPI**.
    - De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    -   De **URL-aanmelding** is de basis-URL van uw app.  Het skelet van de standaardwaarde is `https://localhost:44320/`.
    - De **URI voor App-ID** is een unieke id voor uw toepassing.  Het Verdrag is met `https://<tenant-domain>/<app-name>`, bv.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke client-id.  Moet u deze waarde in de volgende secties, dus kopiëren vanaf het tabblad configureren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. instellen van uw app de pijpleiding OWIN verificatie gebruiken*
Hier zullen we de OWIN middleware voor het gebruik van het verificatieprotocol OpenID verbinding configureren.  OWIN wordt gebruikt voor aanmeldings- en afmeldingstijden aanvragen en informatie over de gebruiker, onder andere gebruikerssessies beheren.

-   Toevoegen om te beginnen, de OWIN middleware NuGet pakketten aan het project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Een klasse OWIN opstarten toevoegen aan het project met de naam `Startup.cs` rechts klikken op het project **toevoegen**--> --> **Nieuw Item** zoeken naar "OWIN"-->.  De OWIN middleware roept de `Configuration(...)` methode wanneer de toepassing wordt gestart.
-   De klasse aangifte wijzigen `public partial class Startup` -we hebben u al deel uit van deze klasse geïmplementeerd in een ander bestand.  In de `Configuration(...)` methode, een aanroep van ConfgureAuth(...) voor het instellen van verificatie voor uw web app  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIDConnectAuthenticationOptions` dient als coördinaten voor uw app om te communiceren met Azure Active Directory.  U moet ook Cookie-verificatie instellen - de middleware OpenID verbinding maakt gebruik van cookies onder de Kaften.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Ten slotte opent de `web.config` -bestand in de hoofdmap van het project, en voert u de configuratiewaarden in de `<appSettings>` sectie.
    -   Van uw app `ida:ClientId` is de Guid die u vanuit de Portal Azure in stap 1 hebt gekopieerd.
    -   De `ida:Tenant` is de naam van de huurder Azure AD, bijvoorbeeld 'contoso.onmicrosoft.com'.
    -   De `ida:PostLogoutRedirectUri` geeft aan dat Azure AD waarbij een gebruiker wordt omgeleid nadat een aanvraag afmelden met succes is voltooid.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. Gebruik OWIN op teken- en afmeldingstijden aanvragen op Azure AD*
Uw app is nu geconfigureerd om te kunnen communiceren met Azure Active Directory met het verificatieprotocol OpenID verbinding.  OWIN heeft afgehandeld alle lelijke details verificatieberichten samenstellen, tokens van Azure AD valideren en onderhouden van de gebruikerssessie.  Het enige dat blijft is om uw gebruikers zich aanmelden en afmelden.

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
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Open nu `Views\Shared\_LoginPartial.cshtml`.  Dit is waar u de gebruiker van uw app aanmeldings- en afmeldingstijden koppelingen weergeven, en de naam van de gebruiker in een weergave afdrukken.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
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

## <a name="4--display-user-information"></a>*4. de gebruikersgegevens weergeven*
Bij het verifiëren van gebruikers met een OpenID verbinding retourneert Azure AD een id_token aan de toepassing die 'vorderingen' of verklaringen over de gebruiker bevat.  U kunt deze claims personaliseren van uw app:

- Open de `Controllers\HomeController.cs` bestand.  U hebt toegang tot de vorderingen van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` beveiligings-principal.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Ten slotte bouwen en uitvoeren van uw app!  Als u nog niet gedaan hebt, nu is de tijd voor het maken van een nieuwe gebruiker in uw huurder met een *. onmicrosoft.com domein.  Die gebruiker aanmelden en u ziet hoe de identiteit van de gebruiker wordt weergegeven in de bovenste navigatiebalk.  Afmelden en opnieuw aanmelden als een andere gebruiker in uw huurder.  Als u zin in bijzonder ambitieus een hebt, registreren en uitvoeren van een ander exemplaar van deze toepassing (met een eigen clientId) en controle Zie eenmalige op in actie.

Voor een verwijzing naar het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [vindt u hier](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

U kunt nu verplaatsen naar de meer geavanceerde onderwerpen.  U kunt proberen:

[Een Web API met Azure Active Directory beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
