<properties
    pageTitle="Azure AD v2.0 .NET Web App | Microsoft Azure"
    description="Het bouwen van een .NET MVC Web App dat gesprekken web services met behulp van persoonlijke accounts van Microsoft en werk of school rekeningen voor aanmelden."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Een web API aanroepen vanuit een .NET web app.

U kunt snel verificatie toevoegen aan uw web-apps en web-API's met ondersteuning voor zowel persoonlijke accounts van Microsoft en rekeningen voor werk of school met het eindpunt v2.0.  Hier zullen wij een MVC web app dat gebruikers bij het gebruik van OpenID verbinden, met wat hulp van Microsoft van OWIN middleware ondertekent bouwen.  De web app wordt OAuth 2.0 access tokens krijgen voor een web api beveiligd door OAuth 2.0 die u kunt maken, lezen en verwijderen op een bepaalde gebruiker 'takenlijst'.

Deze zelfstudie focus voornamelijk met behulp van MSAL te verwerven en access tokens gebruiken in een web app, dat wordt beschreven in de volledige [hier](active-directory-v2-flows.md#web-apps).  Vereist, kunt u eerst informatie over het [toevoegen van elementaire aanmelden bij een web app](active-directory-v2-devquickstarts-dotnet-web.md) of [een web API goed](active-directory-v2-devquickstarts-dotnet-api.md)te beveiligen.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Voorbeeldcode downloaden

De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) of het skelet klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

U kunt ook [downloaden van de app afgelopen als een .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) of de voltooide app klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registreren van een app
Maak een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- Een **Geheim App** van het type **wachtwoord** maken en kopiëren naar de waarde voor later
- Het **Web** platform voor uw app toevoegen.
- Voer de juiste **Redirect URI**. Geeft de redirect uri Azure AD waarin verificatie reacties moeten worden gericht - de standaardwaarde voor deze zelfstudie is `https://localhost:44326/`.


## <a name="install-owin"></a>Installeren van OWIN
Toevoegen van de OWIN middleware NuGet pakketten naar de `TodoList-WebApp` project met de Package Manager-Console.  De OWIN middleware wordt gebruikt voor aanmeldings- en afmeldingstijden aanvragen en informatie over de gebruiker, onder andere gebruikerssessies beheren.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>De gebruiker aanmelden
Nu de OWIN middleware voor het gebruik van het [verificatieprotocol OpenID verbinding](active-directory-v2-protocols.md#openid-connect-sign-in-flow)configureren.  

-   Open de `web.config` bestand in de hoofdmap van het `TodoList-WebApp` project en voert u de configuratiewaarden van uw app in de `<appSettings>` sectie.
    -   De `ida:ClientId` is de **Toepassings-Id** toegewezen aan uw app in de portal voor registratie.
    - De `ida:ClientSecret` is het **Geheim van App** u hebt gemaakt in de portal registratie.
    -   De `ida:RedirectUri` wordt de **Uri omgeleid** u hebt ingevoerd in de portal.
- Open de `web.config` bestand in de hoofdmap van het `TodoList-Service` project en vervang de `ida:Audience` met dezelfde **Toepassings-Id** die hierboven is beschreven.


- Open het bestand `App_Start\Startup.Auth.cs` en `using` -instructies voor de bibliotheken van boven.
- In hetzelfde bestand, implementatie van de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIDConnectAuthenticationOptions` dient als coördinaten voor uw app om te communiceren met Azure Active Directory.

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
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>MSAL gebruiken om access tokens
In de `AuthorizationCodeReceived` melding, willen we [OAuth 2.0 samen met OpenID verbinding](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) gebruiken om te wisselen van de authorization_code voor een toegangstoken voor de taak Service.  MSAL kan dit proces toegankelijk maken voor u:

- Installeer eerst de preview-versie van MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- En het toevoegen van een andere `using` -instructie aan de `App_Start\Startup.Auth.cs` voor MSAL.
- Nu een nieuwe methode toevoegen de `OnAuthorizationCodeReceived` gebeurtenis-handler.  Deze handler MSAL aan te schaffen een toegangstoken voor de lijst taak API gebruikt en het token wordt opgeslagen in de tokencache van MSAL voor later:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- In webtoepassingen heeft MSAL een uitbreidbare tokencache die kan worden gebruikt voor het opslaan van tokens.  In dit voorbeeld wordt de `NaiveSessionCache` die gebruikmaakt van HTTP-sessie opslag cache tokens.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Het Web API aanroepen
Nu is het feitelijk gebruik van de access_token die u in stap 3 hebt aangeschaft.  Open de web-app `Controllers\TodoListController.cs` -bestand, waardoor alle CRUD-aanvragen naar de API To-Do lijst.

- U kunt MSAL opnieuw hier access_tokens ophalen uit de cache MSAL.  Eerst voegen een `using` -instructie voor de MSAL naar dit bestand.

    `using Microsoft.Identity.Client;`

- In de `Index` van de actie, gebruik MSAL `AcquireTokenSilentAsync` methode voor het ophalen van een access_token die kan worden gebruikt om gegevens te lezen uit de takenlijst-service:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Vervolgens wordt het resulterende token aan de HTTP GET-aanvraag als het `Authorization` kop, die de takenlijst-service gebruikt voor de verificatie van de aanvraag.
- Als u de takenlijst wordt een `401 Unauthorized` reactie, de access_tokens in de MSAL niet meer geldig voor een of andere reden.  In dit geval te alle access_tokens uit de cache MSAL neerzetten en geven de gebruiker een bericht dat ze wellicht aan te melden, die de stroom token overname opnieuw wordt gestart.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Op dezelfde manier als MSAL kan een access_token retourneren voor een of andere reden, moet u moet de gebruiker zich weer aanmeldt.  Dit is net zo eenvoudig als het vangen van een `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- De exacte dezelfde `AcquireTokenSilentAsync` oproep is implementd in de `Create` en `Delete` acties.  In web apps, kunt u deze methode MSAL access_tokens krijgen wanneer u ze nodig hebt in uw app.  MSAL zal zorgen voor de aanschaf, caching en vernieuwen van tokens voor u.

Ten slotte bouwen en uitvoeren van uw app!  Aanmelden met een Microsoft-Account of een Account Azure AD en u ziet hoe de identiteit van de gebruiker wordt weergegeven in de bovenste navigatiebalk.  Toevoegen en verwijderen van sommige items in de takenlijst van de gebruiker tot de OAuth 2.0 beveiligde API-aanroepen in actie zien.  U hebt nu een web app & web-API, beide beveiligd met behulp van industriestandaardprotocollen, die gebruikers met hun persoonlijke en werk/school accounts kunnen worden geverifieerd.

Voor een verwijzing naar het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [vindt u hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Volgende stappen

Aanvullende bronnen voor uitchecken:
- [De handleiding voor ontwikkelaars van versie 2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow 'azure active directory'-code >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door [deze pagina](https://technet.microsoft.com/security/dd252948) te bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
