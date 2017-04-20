<properties
    pageTitle="B2C Azure Active Directory | Microsoft Azure"
    description="Het bouwen van een webtoepassing die een web-API aanroept met behulp van B2C van Azure Active Directory."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C: Een web-API aanroept vanuit een .NET web app

Met behulp van B2C Azure Active Directory (AD Azure) kunt u krachtige Self-service identity management-functies toevoegen aan uw web apps en web-API's in een paar korte stappen. In dit artikel wordt uitgelegd hoe het maken van een "Taakoverzicht".NET Model-View-Controller (MVC) web app die een web-API aanroept met behulp van tokens aan toonder

In dit artikel geldt niet voor het implementeren van aanmelden, aanmelding en beheer van gebruikersprofielen met Azure AD B2C. Het richt zich op de aanvragende web API's nadat de gebruiker is al geverifieerd. Als u nog niet gedaan hebt, moet u beginnen met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md) voor meer informatie over de basisbeginselen van Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Een AD-B2C Azure map ophalen

Voordat u AD-B2C Azure gebruiken kunt, moet u een map maken of pachters.  Een directory is een container voor alle gebruikers, toepassingen en groepen.  Als u niet al een hebt, [een B2C-map maken](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u voor het maken van een app in de B2C-map. Dit geeft Azure AD-informatie die nodig is om veilig communiceren met uw app. In dit geval wordt de web app en de web-API vertegenwoordigd door een enkele **Aanvraag-ID**omdat ze bestaan uit één logische app. Als u wilt een app maken, volg [de instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

- Een **web app web API** opnemen in de toepassing.
- Voer `https://localhost:44316/` als een **antwoord op URL**. Het is de standaard-URL voor deze voorbeeldcode.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. Ook moet u dit later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Uw beleid maken

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Dit web app bevat drie identiteit ervaringen: aanmelden, inloggen en profiel bewerken. Moet u voor het maken van een beleid voor elk type, zoals beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Als u de drie beleidsregels maakt, moet u:

- Kies de **weergegeven naam** en andere kenmerken van de aanmelding in uw aanmelding beleid.
- Kies de **weergavenaam** en de **Object-ID** application claims in elk beleid. U kunt ook andere vorderingen.
- De **naam** van elk beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`. Moet u deze namen beleid later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, bent u klaar om uw app te bouwen.

Houd er rekening mee dat in dit artikel heeft geen betrekking op het gebruik van het beleid dat u zojuist hebt gemaakt. Meer informatie over de werking van beleid in Azure AD B2C, start met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [wordt onderhouden op GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). Voor het opbouwen van het monster als u gaat, kunt u [de overgebleven project als een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). U kunt ook het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Na het downloaden van de voorbeeldcode, opent u het bestand Visual Studio .sln aan de slag.

## <a name="configure-the-task-web-app"></a>De taak web app configureren

Om `TaskWebApp` om te communiceren met Azure AD B2C, moet u een aantal algemene parameters. In de `TaskWebApp` project openen de `web.config` -bestand in de hoofdmap van het project en vervang de waarden in de `<appSettings>` sectie. Kunt u de `AadInstance`, `RedirectUri`, en `TaskServiceUrl` waarden zoals ze zijn.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Toegangstokens ophalen en roept de API van de taak

In dit gedeelte wordt uitgelegd hoe de token ontvangen tijdens het aanmelden met Azure AD B2C gebruiken voor toegang tot een web-API die ook is beveiligd met Azure AD B2C.

In dit artikel geldt niet voor de details van het beveiligen van de API. Als u wilt weten hoe een web API veilig worden geverifieerd aanvragen via Azure AD B2C, check de [web API aan de slag artikel](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Het teken slaat in token

Eerst, verificatie van de gebruiker (met behulp van een van uw beleid) en een token-in van Azure AD B2C ontvangt.  Als u niet zeker weet hoe u beleid voeren, Ga terug en probeer de [handleiding .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md) voor meer informatie over de basisbeginselen van Azure AD B2C.

Open het bestand `App_Start\Startup.Auth.cs`.  Er is één belangrijke wijziging moet u aanbrengen in de `OpenIdConnectAuthenticationOptions` -stelt u `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Een token ophalen in de controllers

De `TasksController` is verantwoordelijk voor de communicatie met het web API, het verzenden van HTTP-aanvragen naar de API te lezen, maken en verwijderen van taken.  Becuase die de API wordt beveiligd door de Azure AD B2C, moet u eerst het token dat u hebt opgeslagen in de bovenstaande stap ophalen.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

De `BootstrapContext` bevat het token dat u hebt verkregen door het uitvoeren van een van uw beleidsregels B2C-teken.

### <a name="read-tasks-from-the-web-api"></a>Taken van het web API te lezen

Wanneer u een token hebt, kunt u deze koppelen aan het HTTP `GET` aanvragen in de `Authorization` kop veilig bellen `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Maken en verwijderen van taken op het web API

Hetzelfde patroon volgen bij het verzenden van `POST` en `DELETE` aanvragen voor het web API, met behulp van de `BootstrapContext` voor het ophalen van het teken in het token. We actie maken voor u uitgevoerd. U kunt proberen te voltooien in de verwijderactie `TasksController.cs`.

## <a name="run-the-sample-app"></a>De steekproef toepassing uitvoeren

Ten slotte bouwen en uitvoeren van de app. Aanmelden inloggen en taken maken voor de gebruiker ingelogd. Log uit en log in als een andere gebruiker. Taken maken voor die gebruiker. U ziet hoe de taken worden opgeslagen per gebruiker op de API, omdat de API haalt de identiteit van de gebruiker uit het token is ontvangen.

Voor een verwijzing naar de voltooide monster [wordt geleverd als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). U kunt ook het klonen van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
