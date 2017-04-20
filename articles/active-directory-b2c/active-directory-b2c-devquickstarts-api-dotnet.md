<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Het bouwen van een .NET Web API met behulp van Azure Active Directory B2C, beveiligd met OAuth 2.0 toegangstokens voor verificatie."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Een .NET web API bouwen

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met B2C Azure Active Directory (AD Azure), kunt u een web API kunt beveiligen met behulp van toegangstokens OAuth 2.0. Deze tokens kunnen uw clienttoepassingen die Azure AD B2C gebruiken om de API te verifiëren. In dit artikel wordt beschreven hoe u een .NET-Model-View-Controller (MVC) "taakoverzicht" API waarmee gebruikers kunnen taken CRUD maken. De web-API is beveiligd met Azure AD B2C en kan alleen geverifieerde gebruikers voor het beheren van hun taak-overzicht.

## <a name="create-an-azure-ad-b2c-directory"></a>Maak een map Azure AD B2C

Voordat u AD-B2C Azure gebruiken kunt, moet u een map maken of pachters. Een directory is een container voor alle gebruikers, toepassingen en groepen. Als u niet al een hebt, [een B2C-map maken](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u voor het maken van een app in de B2C-map. Dit geeft Azure AD-informatie die nodig is om veilig communiceren met uw app. Als u wilt een app maken, volg [de instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

- Een **web app** of **web API** opnemen in de toepassing.
- Gebruik de **uniform resource identifier omleiden** `https://localhost:44316/` voor het web app. Dit is de standaardlocatie van de client web app voor deze voorbeeldcode.
- Kopieer de **toepassings-ID** die is toegewezen aan uw app. U zult het later nodig hebt.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Uw beleid maken

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). De client in deze voorbeeldcode bevat drie identiteit ervaringen: aanmelden, inloggen en profiel bewerken. Moet u voor het maken van een beleid voor elk type, zoals beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Als u de drie beleidsregels maakt, moet u:

- Kies **Gebruikersnaam aanmelden** of **aanmelden bij e-mailadres** in de blade identiteit providers.
- **Weergegeven naam** en andere kenmerken van de aanmelding in uw aanmelding beleid kiezen.
- Claims **weergavenaam** en de **Object-ID** kiezen als toepassing claims voor elk beleid. U kunt ook andere vorderingen.
- De **naam** van elk beleid kopiëren nadat u deze hebt gemaakt. Moet u deze namen beleid later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, bent u klaar om uw app te bouwen.

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [wordt onderhouden op GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Voor het opbouwen van het monster als u gaat, kunt u [een skelet project als een .zip-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). U kunt ook het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Na het downloaden van de voorbeeldcode, opent u het bestand Visual Studio .sln aan de slag. Het oplossingsbestand bevat twee projecten: `TaskWebApp` en `TaskService`. `TaskWebApp`een MVC web-toepassing die de gebruiker met werkt is. `TaskService`is de toepassing van de back-end web API die het taakoverzicht van elke gebruiker worden opgeslagen.

## <a name="configure-the-task-web-app"></a>De taak web app configureren

Wanneer een gebruiker communiceert met `TaskWebApp`, de client verzendt aanvragen naar Azure AD en haalt weer tokens die kunnen worden gebruikt om aan te roepen de `TaskService` web-API. Als de gebruiker zich aanmeldt en tokens ophalen, moet u `TaskWebApp` met informatie over uw app. In de `TaskWebApp` project openen de `web.config` -bestand in de hoofdmap van het project en vervang de waarden in de `<appSettings>` sectie.  Kunt u de `AadInstance`, `RedirectUri`, en `TaskServiceUrl` als waarden-is.

```
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
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

In dit artikel geldt niet voor gebouw de `TaskWebApp` client.  Als u wilt weten hoe u een web app met Azure AD B2C, Zie [onze zelfstudie .NET web app](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>De API beveiligen

Als u een client die de API voor gebruikers roept, kunt u beveiligen `TaskService` met behulp van tokens voor OAuth 2.0 aan toonder. De API kan accepteren en tokens valideren met geopende Web-Interface van Microsoft voor .NET (OWIN)-bibliotheek.

### <a name="install-owin"></a>Installeren van OWIN
Installeer eerst de de pijpleiding OWIN OAuth-verificatie:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Voer de gegevens van uw B2C
Open de `web.config` bestand in de hoofdmap van het `TaskService` project en vervang de waarden in de `<appSettings>` sectie. Deze waarden worden gebruikt in de bibliotheek van API en OWIN.  Kunt u de `AadInstance` waarde ongewijzigd.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Een klasse OWIN opstarten toevoegen
Een OWIN opstarten toevoegen aan de `TaskService` project met de naam `Startup.cs`.  Met de rechtermuisknop op het project en selecteer **toevoegen** en het **Nieuwe Item**vervolgens zoeken naar OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>2.0 OAuth-verificatie configureren
Open het bestand `App_Start\Startup.Auth.cs`, en de uitvoering van de `ConfigureAuth(...)` methode:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Beveiliging van de taakcontroller
Nadat de app voor 2.0 OAuth-verificatie is geconfigureerd, kunt u uw web API beveiligen door toevoegen van een `[Authorize]` tag op de taakcontroller. Dit is de controller waar alle to-do lijst manipulatie plaatsvindt, zodat u de hele controller op het klasseniveau van de moet beveiligen. U kunt ook toevoegen de `[Authorize]` tag op afzonderlijke acties voor meer nauwgezette controle.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Gebruikersinformatie ophalen uit het token
`TasksController`taken worden opgeslagen in een database waar elke taak een gebruiker die de taak heeft 'eigenaar'. De eigenaar van de wordt door de gebruiker zijn **object-ID**geïdentificeerd. (Dit is de reden waarom u nodig hebt om toe te voegen van de object-ID als een toepassing in de identiteitsclaim van het beleid.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>De steekproef toepassing uitvoeren

Ten slotte bouwen en uitvoeren van beide `TaskWebApp` en `TaskService`. Meld u met behulp van een e-mailadres of de gebruikersnaam voor de toepassing. Sommige taken maken in de takenlijst van de gebruiker en u ziet hoe ze in de API worden doorgevoerd nadat u stopt en start u de client opnieuw.

## <a name="edit-your-policies"></a>De beleidsregels bewerken

Nadat u een API zijn beveiligd via Azure AD B2C, kunt u experimenteren met het beleid van uw app en weergeven de effecten (of daarvan ontbreken) over de API. U kunt manipuleren van de claims van toepassing in het beleid en de gebruikersinformatie die beschikbaar is in het web API wijzigen. Alle claims die u toevoegt, zijn beschikbaar voor uw .NET MVC web API in de `ClaimsPrincipal` -object, zoals eerder in dit artikel wordt beschreven.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
