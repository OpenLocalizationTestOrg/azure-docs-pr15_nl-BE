<properties
    pageTitle="B2C Azure Active Directory | Microsoft Azure"
    description="Het bouwen van een Windows-bureaublad toepassing met aanmelden, inschrijf en Profielbeheer via Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Bouwen van een desktop app voor Windows

Met behulp van B2C Azure Active Directory (AD Azure) kunt u krachtige Self-service identity management-functies toevoegen aan uw bureaublad app in een paar korte stappen. In dit artikel wordt beschreven hoe u een .NET Windows Presentation Foundation (WPF) "taakoverzicht" app met gebruiker aanmelden, inloggen en beheer van gebruikersprofielen maken. De app biedt ondersteuning voor aanmelden en aanmelden met een gebruikersnaam of e-mailadres. Het biedt ook ondersteuning voor aanmelden en aanmelden via sociale accounts zoals Facebook en Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Een AD-B2C Azure map ophalen

Voordat u AD-B2C Azure gebruiken kunt, moet u een map maken of pachters.  Een directory is een container voor alle gebruikers, toepassingen en groepen. Als u niet al een hebt, [een B2C-map maken](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u voor het maken van een app in de B2C-map. Dit geeft Azure AD-informatie die nodig is om veilig communiceren met uw app. Als u wilt een app maken, volg [de instructies](active-directory-b2c-app-registration.md).  Zorg ervoor dat:

- Een **native client** in de aanvraag opnemen.
- Kopieer het **omleiden van URI** `urn:ietf:wg:oauth:2.0:oob`. Het is de standaard-URL voor deze voorbeeldcode.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. U zal het later nodig hebt.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Uw beleid maken

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). In deze voorbeeldcode bevat drie identiteit ervaringen: aanmelden, inloggen en profiel bewerken. Moet u een beleid voor elk type, zoals beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Als u de drie beleidsregels maakt, moet u:

- Kies **Gebruikersnaam aanmelden** of **aanmelden bij e-mailadres** in de blade identiteit providers.
- **Weergegeven naam** en andere kenmerken van de aanmelding in uw aanmelding beleid kiezen.
- Claims **weergavenaam** en de **Object-ID** kiezen als toepassing claims voor elk beleid. U kunt ook andere vorderingen.
- De **naam** van elk beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  Moet u deze namen beleid later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, bent u klaar om uw app te bouwen.

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [wordt onderhouden op GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Voor het opbouwen van het monster als u gaat, kunt u [een skelet project als een .zip-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). U kunt ook het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Na het downloaden van de voorbeeldcode, opent u het bestand Visual Studio .sln aan de slag. De `TaskClient` project wordt de WPF-bureaubladtoepassing die de gebruiker met werkt. Voor de toepassing van deze zelfstudie wordt een taak van de back-end web API, gehost in Azure, waarin het taakoverzicht van elke gebruiker opgeslagen.  U hoeft niet te maken van de web-API, we hebben voor u uitgevoerd.

Als u wilt weten hoe een web API veilig worden geverifieerd aanvragen via Azure AD B2C, check de [web API aan de slag artikel](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Beleid uitvoeren
Uw app communiceert met Azure AD B2C door verificatieberichten die het beleid moet worden uitgevoerd als onderdeel van de HTTP-aanvraag te verzenden. U kunt voor .NET desktoptoepassingen, bibliotheek voor Microsoft-verificatie (MSAL) van de voorvertoning gebruiken OAuth 2.0 verificatieberichten verzenden, beleid voeren en tokens die web-API's aanroepen krijgen.

### <a name="install-msal"></a>MSAL installeren
Toevoegen van MSAL aan de `TaskClient` project met de Visual Studio Package Manager-Console.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Voer de gegevens van uw B2C
Open het bestand `Globals.cs` en elk van de waarden van de eigenschap vervangen door uw eigen. Deze klasse wordt gebruikt in de gehele `TaskClient` aan de gebruikte referentiewaarden.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>De PublicClientApplication maken
De primaire categorie van MSAL is `PublicClientApplication`. Deze klasse vertegenwoordigt de toepassing in het systeem Azure AD B2C. Wanneer de app-initalizes maakt een exemplaar van `PublicClientApplication` in `MainWindow.xaml.cs`. Dit kan worden gebruikt in het venster.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Een stroom van aanmelding tot stand brengen
Wanneer een gebruiker ervoor om te tekenen van kiest, die u wilt starten van een aanmelding stroom met de aanmelding beleid dat u hebt gemaakt. Met behulp van MSAL, roept u NET `pca.AcquireTokenAsync(...)`. De parameters die u kunt doorgeven aan `AcquireTokenAsync(...)` bepalen welke token wordt weergegeven, het beleid dat wordt gebruikt in de verificatieaanvraag, en nog veel meer.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>Een stroom-in starten
U kunt een stroom-in starten op dezelfde manier als een stroom van aanmelding tot stand te brengen. Wanneer een gebruiker zich aanmeldt, moet dezelfde aanroep van MSAL, deze keer met behulp van het beleid-in:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Starten van een stroom profiel bewerken
Nogmaals, kunt u een beleid voor profiel bewerken op dezelfde manier uitvoeren:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

In al deze gevallen retourneert MSAL ofwel een token in `AuthenticationResult` of een uitzondering. Telkens wanneer u een token uit MSAL, kunt u de `AuthenticationResult.User` de gebruikersgegevens bijwerken in de app, zoals het UI-object. ADAL slaat ook het token voor gebruik in andere delen van de toepassing.


### <a name="check-for-tokens-on-app-start"></a>Controleren op tokens op toepassing starten
U kunt ook MSAL gebruiken voor het bijhouden van status-in van de gebruiker.  In dit app willen we de gebruiker ingelogd blijven ook nadat zij de toepassing sluiten en opnieuw openen.  Terug in de `OnInitialized` overschrijven, gebruikt u de MSAL van `AcquireTokenSilent` methode om te controleren of in de cache opgeslagen tokens:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>De taak API aanroepen
Nu hebt u MSAL gebruikt om te voeren beleid en tokens krijgen.  Als u een deze tokens gebruiken wilt om de taak API aanroept, kunt u opnieuw gebruiken van MSAL `AcquireTokenSilent` methode om te controleren of in de cache opgeslagen tokens:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Als de aanroep van `AcquireTokenSilentAsync(...)` is gelukt en een token in de cache wordt aangetroffen, kunt u het token voor toevoegen de `Authorization` header van de HTTP-aanvraag. De taak web API gebruikt voor de verificatie van de aanvraag voor het lezen van de takenlijst van de gebruiker deze header:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>De gebruiker afmelden
Ten slotte kunt u MSAL tot het einde van de sessie van een gebruiker met de app wanneer de gebruiker **zich afmeldt**.  Wanneer u MSAL, hiervoor alle de tokens van de tokencache uit te schakelen:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>De steekproef toepassing uitvoeren

Ten slotte bouwen en uitvoeren van het monster.  Meld u bij de app met behulp van de naam van een e-mailadres of gebruiker. Afmelden en opnieuw aanmelden als dezelfde gebruiker. Profiel van die gebruiker bewerken. Meldt u af en meld u aan met behulp van een andere gebruiker.

## <a name="add-social-idps"></a>Sociale IDPs toevoegen

De app ondersteunt momenteel alleen gebruiker aanmelden en aanmelden die gebruikmaken van **lokale accounts**. Deze zijn opgeslagen in de map B2C accounts die een gebruikersnaam wachtwoord en. U kunt ondersteuning voor andere identiteitsproviders (IDPs) met behulp van Azure AD B2C toevoegen zonder het wijzigen van de code.

Als sociale IDPs toevoegen aan uw app, eerst door de gedetailleerde instructies in deze artikelen. Voor elke IDP die u wilt ondersteunen, moet u een toepassing in dat systeem registreren en verkrijgen van een client-ID.

- [Facebook ingesteld als een IDP.](active-directory-b2c-setup-fb-app.md)
- [Google instellen als een IDP.](active-directory-b2c-setup-goog-app.md)
- [Amazon ingesteld als een IDP.](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn instellen als een IDP.](active-directory-b2c-setup-li-app.md)

Nadat u de id-providers aan uw B2C-map toevoegen, moet u elk van de drie beleidsregels op te nemen van de nieuwe IDPs te bewerken zoals wordt beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md). Nadat u het beleid opgeslagen, voert u de app opnieuw uit. Ziet u de nieuwe IDPs toegevoegd als aanmelden en ervaringen van de aanmeldingsopties voor elk van uw identiteit.

U kunt experimenteren met het beleid en houd rekening met de gevolgen voor uw app monster. Toevoegen of verwijderen van IDPs, claims van toepassing te manipuleren of aanmelding kenmerken wijzigen. Experimenteren totdat u kunt zien hoe beleid verificatieaanvragen en MSAL met elkaar verbinden.

Voor een verwijzing naar de voltooide monster [wordt geleverd als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). U kunt ook het klonen van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
