<properties
pageTitle="Azure Active Directory v2.0 .NET Native App | Microsoft Azure"
description="Hoe u een .NET native app dat gebruikers met beide persoonlijke Microsoft-Account aangemeld en rekeningen voor werk of school."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Aanmelden toevoegen aan een Windows-bureaublad-app

Met de het eindpunt v2.0 kunt u snel werk of school accounts en verificatie van uw desktop apps met ondersteuning voor zowel persoonlijke Microsoft-accounts toevoegen.  U kunt er tevens uw app veilig communiceren met een back-end web api als [Het Microsoft Graph](https://graph.microsoft.io) en enkele van de [Office 365-Unified API](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Niet alle scenario's Azure Active Directory (AD) & functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

AD Azure biedt voor [.NET native apps die worden uitgevoerd op een apparaat](active-directory-v2-flows.md#mobile-and-native-apps), de bibliotheek van Microsoft Identity verificatie of MSAL.  Is het enige doel in het leven van MSAL kunt u gemakkelijk uw app tokens ophalen voor het aanroepen van webservices.  U kunt zien hoe gemakkelijk het is, bouw hier we een taakoverzicht van .NET WPF app die:

- De gebruiker aanmelden & krijgt toegang tot het [verificatieprotocol OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)tokens.
- Veilig roept een backend takenlijst webservice die ook door OAuth 2.0 is beveiligd.
- Meldt de gebruiker zich af.

## <a name="download-sample-code"></a>Voorbeeldcode downloaden

De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) of het skelet klonen:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

De voltooide toepassing wordt geleverd aan het einde van deze zelfstudie ook.

## <a name="register-an-app"></a>Registreren van een app
Maak een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- Het **mobiele** platform voor uw app toevoegen.

## <a name="install--configure-msal"></a>Installeren en configureren van MSAL
Nu dat er een app geregistreerd bij Microsoft, kunt u MSAL installeren en uw identiteit gerelateerde code te schrijven.  Voor de MSAL naar het eindpunt v2.0 communiceren moet, u te verstrekken informatie over de registratie van uw app.

-   Begin door MSAL toe te voegen aan het TodoListClient-project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   Open in het project TodoListClient `app.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie aangepast aan de waarden die u in de app-portal registratie hebt ingevoerd.  De code verwijst naar deze waarden wanneer MSAL wordt gebruikt.
    -   De `ida:ClientId` is de **Toepassings-Id** van uw app die u hebt gekopieerd vanaf de portal.

- Open in de takenlijst van Service project `web.config` in de hoofdmap van het project.  
    - Vervang de `ida:Audience` waarde met dezelfde **Id van de toepassing** van de portal.

## <a name="use-msal-to-get-tokens"></a>Gebruik MSAL om tokens
Het basisprincipe achter de MSAL is dat als uw app een toegangstoken moet, u gewoon belt `app.AcquireToken(...)`, en MSAL doet de rest.  

-   In de `TodoListClient` project geopend `MainWindow.xaml.cs` en zoek de `OnInitialized(...)` methode.  De eerste stap is het initialiseren van uw app `PublicClientApplication` -MSAL van primaire klasse die native toepassingen.  Dit is waar u MSAL de coördinaten die om te communiceren met Azure Active Directory en hoe deze tokens in de cache moet doorgeven.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Wanneer u de toepassing start, die wij willen controleren en als de gebruiker al is aangemeld bij de app te bekijken.  Maar we willen niet alleen nog aanroepen van een UI-in - de gebruiker op 'Aanmelden' hiervoor zorgen wij ervoor.  Ook in de `OnInitialized(...)` methode:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Als de gebruiker niet is aangemeld en de gebruiker klikt op de knop "Aanmelden", die we willen roepen van een UI-aanmelding en de gebruikers hun referenties invoeren.  De Sign In knop-handler implementeert:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
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


}
```

- Als de gebruiker met succes tekenen in MSAL ontvangt en u een token in de cache en kunt u doorgaan met het bellen de `GetTodoList()` methode met vertrouwen.  Links als u de taken van de gebruiker wordt voor de uitvoering van de `GetTodoList()` methode.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Uitvoeren

Gefeliciteerd! U hebt nu een werkende .NET WPF-app met de mogelijkheid om gebruikers te verifiëren en veilig gebruik OAuth 2.0 Web-API's aanroepen.  Beide projecten uitvoeren en log in met een persoonlijke Microsoft-account of een account voor werk of school.  Taken toevoegen aan de takenlijst van de gebruiker.  Afmelden en opnieuw aanmelden als een andere gebruiker bij hun takenlijst weergeven.  De toepassing sluiten en opnieuw worden uitgevoerd.  U ziet hoe de sessie blijft intact - dat is omdat de app tokens in een lokaal bestand in de cache opgeslagen.

MSAL kunt u eenvoudig algemene identiteit-functies opnemen in uw app, met persoonlijke en zakelijke accounts.  Dit zorgt voor het vuile werk voor u - Cachebeheer, ondersteuning OAuth-protocol, dat de gebruiker een login UI, het vernieuwen van verlopen tokens en nog veel meer.  Alles wat u moet weten is dat een enkele API-aanroep `app.AcquireTokenAsync(...)`.

Voor een verwijzing naar kunt het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [wordt geleverd als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)of u klonen het van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar de meer geavanceerde onderwerpen.  U kunt proberen:

- [De TodoListService Web API aan het eindpunt v2.0 beveiligen](active-directory-v2-devquickstarts-dotnet-api.md)

Aanvullende bronnen voor uitchecken:  

- [De handleiding voor ontwikkelaars van versie 2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "msal" tag >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door [deze pagina](https://technet.microsoft.com/security/dd252948) te bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
