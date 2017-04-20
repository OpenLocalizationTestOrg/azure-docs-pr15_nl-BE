<properties
    pageTitle="B2C Azure Active Directory | Microsoft Azure"
    description="Het bouwen van een webtoepassing die aanmelden, inschrijf is en Profielbeheer via Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-net-web-app"></a>Azure AD B2C: Een .NET web app bouwen

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met behulp van B2C Azure Active Directory (AD Azure) kunt u krachtige Self-service identity management-functies toevoegen aan uw web app in een paar korte stappen. In dit artikel wordt uitgelegd hoe het maken van een .NET-Model-View-Controller (MVC) web app met gebruiker aanmelden, inloggen en beheer van gebruikersprofielen. De app biedt ondersteuning voor het aanmelden en aanmelden met een gebruikersnaam of e-mailadres en via sociale accounts zoals Facebook en Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Een AD-B2C Azure map ophalen

Voordat u AD-B2C Azure gebruiken kunt, moet u een map maken of pachters. Een directory is een container voor alle gebruikers, toepassingen en groepen.  Als u niet al een hebt, [een B2C-map maken](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u voor het maken van een app in de B2C-map. Dit geeft Azure AD-informatie die nodig is om veilig communiceren met uw app. Als u wilt een app maken, volg [de instructies](active-directory-b2c-app-registration.md).  Zorg ervoor dat:

- Een **web app web API** opnemen in de toepassing.
- Voer `https://localhost:44316/` als een **Redirect URI**. Het is de standaard-URL voor deze voorbeeldcode.
- Kopiëren van de **Toepassings-ID** die is toegewezen aan uw app.  U zal het later nodig hebt.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Uw beleid maken

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). In deze voorbeeldcode bevat drie identiteit ervaringen: aanmelden, inloggen en profiel bewerken. Moet u voor het maken van een beleid voor elk type, zoals beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).

>[AZURE.NOTE] Azure AD B2C ondersteunt ook een gecombineerde aanmelden of teken in een beleid dat niet in deze zelfstudie is uitgerust.  De aanmelding of teken in het beleid in [deze zelfstudie gelijkwaardig](active-directory-b2c-devquickstarts-web-dotnet-susi.md)wordt weergegeven.

Als u de drie beleidsregels maakt, moet u:

- Kies **Gebruikersnaam aanmelden** of **e-aanmelding** in de blade identiteit providers.
- Kies de **weergegeven naam** en andere kenmerken van de aanmelding in uw aanmelding beleid.
- Kies de claim **weergeven** als een toepassing claim in elk beleid. U kunt ook andere vorderingen.
- De **naam** van elk beleid kopiëren nadat u deze hebt gemaakt. Moet u deze namen beleid later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, bent u klaar om uw app te bouwen.  

## <a name="download-the-code-and-configure-authentication"></a>Download de code en -verificatie configureren

De code voor dit voorbeeld [wordt onderhouden op GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet). Voor het opbouwen van het monster als u gaat, kunt u [de overgebleven project als een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip). U kunt ook het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

Het voltooide monster is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Na het downloaden van de voorbeeldcode, opent u het bestand Visual Studio .sln aan de slag.

Uw app communiceert met Azure AD B2C door verificatieberichten die het beleid moet worden uitgevoerd als onderdeel van de HTTP-aanvraag te verzenden. .NET web-toepassingen, kunt u Microsoft OWIN middleware verificatieaanvragen OpenID verbinding verzenden, beleid uitvoeren, beheren van sessies en meer.

Toevoegen om te beginnen, de OWIN middleware NuGet pakketten aan het project met de Visual Studio Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

Open vervolgens de `web.config` bestand in de hoofdmap van het project en voer waarden van de systeemconfiguratie van uw app in de `<appSettings>` sectie vervangen de bestaande waarden.

```
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Vervolgens voegt u een klasse OWIN opstarten om het project met de naam `Startup.cs`. Met de rechtermuisknop op het project, selecteer **toevoegen** en een **Nieuw artikel**en zoek naar 'OWIN'. **Zorg ervoor dat de klasse aangifte wijzigen `public partial class Startup` **. We deel uitmaken van deze klasse geïmplementeerd voor u in een ander bestand. De OWIN middleware roept de `Configuration(...)` methode wanneer de toepassing wordt gestart. Bij deze methode wordt gebeld om te `ConfigureAuth(...)`, waar u verificatie voor uw app instellen.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIdConnectAuthenticationOptions` fungeren als coördinaten voor uw app om te communiceren met Azure Active Directory. Moet u ook Cookieverificatie instellen. De middleware OpenID verbinding gebruikt cookies zodat gebruikerssessies, onder andere.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SignUpPolicyId = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string SignInPolicyId = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string ProfilePolicyId = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignUpPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(ProfilePolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignInPolicyId));
    }

    // Used for avoiding yellow-screen-of-death
    private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        notification.HandleResponse();
        if (notification.Exception.Message == "access_denied")
        {
            notification.Response.Redirect("/");
        }
        else
        {
            notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
        }

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
```

## <a name="send-authentication-requests-to-azure-ad"></a>Verificatieaanvragen verzenden naar Azure AD
Uw app is nu juist geconfigureerd om te communiceren met Azure AD B2C met behulp van het verificatieprotocol OpenID verbinden.  OWIN heeft afgehandeld alle details verificatieberichten samenstellen, tokens van Azure AD valideren en onderhouden van de gebruikerssessie.  Die blijft is de stroom van de gebruiker te initiëren.

Wanneer een gebruiker **aanmelden**, **inloggen**of **profiel bewerken** in de web app selecteert, de gekoppelde actie wordt aangeroepen in de `Controllers\AccountController.cs`. In beide gevallen kunt u ingebouwde OWIN methoden voor het starten van het juiste beleid:

```C#
// Controllers\AccountController.cs

public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties () { RedirectUri = "/" }, Startup.SignInPolicyId);
    }
}

public void SignUp()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SignUpPolicyId);
    }
}


public void Profile()
{
    if (Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.ProfilePolicyId);
    }
}
```

U kunt ook een `[Authorize]` -tag in uw domeincontrollers die de uitvoering van een bepaald beleid is vereist als de gebruiker niet is aangemeld. Open `Controllers\HomeController.cs` en voeg de `[Authorize]` tag op de controller vorderingen.  OWIN selecteert het laatste beleid is geconfigureerd voor uitvoeren als de tag machtigen is gestart.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a policy if the user is not already signed in the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

U kunt ook OWIN afmelden van de gebruiker van de app. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Gebruikersgegevens weergeven
Wanneer u gebruikers verifiëren met behulp van OpenID verbinding retourneert Azure AD een token ID App met **vorderingen**. Dit zijn verklaringen over de gebruiker. Vorderingen kunt u uw toepassing aanpassen.  

Open de `Controllers\HomeController.cs` bestand. U hebt toegang tot vorderingen van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` beveiligings-principal.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

U hebt toegang tot alle vorderingen die uw toepassing op dezelfde manier krijgt.  Een lijst van alle vorderingen de app ontvangt is beschikbaar voor u op de pagina **aanvragen** .

## <a name="run-the-sample-app"></a>De steekproef toepassing uitvoeren

U kunt ten slotte bouwen en uitvoeren van uw app. Meld u bij de app met behulp van de naam van een e-mailadres of gebruiker. Afmelden en opnieuw aanmelden als dezelfde gebruiker. Profiel van die gebruiker bewerken. Meldt u af en meld u aan als een andere gebruiker. Houd er rekening mee dat de informatie die wordt weergegeven op het tabblad **Claims** komt overeen met de informatie die u hebt geconfigureerd in het beleid.

## <a name="add-social-idps"></a>Sociale IDPs toevoegen

De app ondersteunt momenteel gebruiker aanmelden en aanmelden met behulp van **lokale accounts**. Deze zijn opgeslagen in de map B2C accounts die een gebruikersnaam wachtwoord en. U kunt ondersteuning voor andere **id-providers** (IDPs) met behulp van Azure AD B2C toevoegen zonder het wijzigen van de code.

Als sociale IDPs toevoegen aan uw app, eerst door de gedetailleerde instructies in deze artikelen. Voor elke IDP die u wilt ondersteunen, moet u een toepassing in dat systeem registreren en verkrijgen van een client-ID.

- [Facebook ingesteld als een IDP.](active-directory-b2c-setup-fb-app.md)
- [Google instellen als een IDP.](active-directory-b2c-setup-goog-app.md)
- [Amazon ingesteld als een IDP.](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn instellen als een IDP.](active-directory-b2c-setup-li-app.md)

Nadat u de id-providers aan uw B2C-map toevoegen, moet u elk van de drie beleidsregels op te nemen van de nieuwe IDPs te bewerken zoals wordt beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md). Nadat u het beleid opgeslagen, voert u de app opnieuw uit.  Ziet u de nieuwe IDPs toegevoegd als aanmelden en ervaringen van de aanmeldingsopties voor elk van uw identiteit.

U kunt experimenteren met het beleid en het effect op uw app monster. Toevoegen of verwijderen van IDPs, claims van toepassing te manipuleren of aanmelding kenmerken wijzigen. Experimenteren totdat u kunt zien hoe beleid verificatieaanvragen en OWIN met elkaar verbinden.

Voor een verwijzing naar het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [wordt geleverd als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip). U kunt ook het klonen van GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
