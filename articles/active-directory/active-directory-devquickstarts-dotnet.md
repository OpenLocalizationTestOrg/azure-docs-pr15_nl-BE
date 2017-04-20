<properties
    pageTitle="Azure AD .NET introductie | Microsoft Azure"
    description="Hoe u een .NET Windows-bureaublad-toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
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


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Azure AD integreren in een Windows-bureaublad WPF-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Als u een toepassing ontwikkelt, kunt Azure AD u eenvoudig en ongecompliceerd voor u uw gebruikers met de Active Directory-accounts worden geverifieerd.  Bovendien kunt uw toepassing elk web API beschermd door Azure AD, zoals de Office 365-API of de API Azure veilig in beslag neemt.

Azure AD biedt voor .NET native clients die toegang moeten krijgen tot beveiligde bronnen, de bibliotheek van Active Directory-verificatie of ADAL.  ADAL van enige doel in het leven is kunt u gemakkelijk uw app toegangstokens ophalen.  U kunt zien hoe gemakkelijk het is, bouw hier we een taakoverzicht van .NET WPF toepassing die:

-   Haalt access tokens voor de Azure AD Graph API met het [verificatieprotocol OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)aanroept.
-   Zoekt in een map voor gebruikers met een bepaalde alias.
-   Symptomen gebruikers.

Als u de volledige toepassing van de werken, moet u naar:

2. Uw toepassing met Azure Active Directory registreren.
3. Installeren en configureren van ADAL.
5. ADAL gebruiken om tokens uit Azure Active Directory.

Ophalen is gestart, [het skelet app downloaden](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) of [het voltooide monster](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  U moet ook een huurder Azure AD kunt u gebruikers maken en registreren van een toepassing.  Als u een huurder, [Lees hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="1-register-the-directorysearcher-application"></a>*1. de toepassing van het DirectorySearcher registreren*
Als u wilt dat uw app tokens ophalen, moet u eerst te registreren in uw huurder Azure AD en verleent deze machtiging voor toegang tot de Azure AD Graph API:

-   Aanmelden bij de Azure Management Portal
-   Klik in de nav links op in **Active Directory**
-   Selecteer een huurder waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klik op **toevoegen** in de onderste lade.
-   Volg de aanwijzingen en een nieuwe **Native Client-toepassing**maken.
    -   De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    -   Het **Omleiden van Uri** is een schema en tekenreeks van de combinatie waarmee Azure AD token antwoorden terug.  Voert u een specifieke waarde aan uw toepassing, bv. `http://DirectorySearcher`.
-   Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke client-id.  Moet u deze waarde in de volgende secties, dus kopiëren vanaf het tabblad **configureren** .
- Ook in het tabblad **configureren** , Ga naar de sectie 'Machtigingen voor andere toepassingen'.  Voeg de machtiging **toegang tot uw organisatie Directory** onder **Overgedragen machtigingen**voor de toepassing 'Azure Active Directory'.  Hierdoor kan uw toepassing kan een query uitvoeren op de Graph API voor gebruikers.

## <a name="2-install--configure-adal"></a>*2. Installeer & ADAL configureren*
U kunt nu dat er een toepassing in Azure AD, ADAL installeren en uw identiteit gerelateerde code te schrijven.  Voor ADAL te kunnen communiceren met Azure Active Directory moet, u te verstrekken informatie over de registratie van uw app.
-   Begin met het ADAL aan het DirectorySearcher-project met de Package Manager-Console toe te voegen.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Open in het project DirectorySearcher `app.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie aangepast aan de waarden die u in de Portal Azure ingevoerd.  De code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `ida:Tenant` is het domein van de huurder Azure AD, bv. contoso.onmicrosoft.com
    -   De `ida:ClientId` is de clientId van de toepassing die u hebt gekopieerd vanaf de portal.
    -   De `ida:RedirectUri` is de omleiding url die u hebt geregistreerd in de portal.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Gebruik ADAL Tokens ophalen van AAD*
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, aanroept `authContext.AcquireTokenAsync(...)`, en ADAL doet de rest.  

-   In de `DirectorySearcher` project geopend `MainWindow.xaml.cs` en zoek de `MainWindow()` methode.  De eerste stap is het initialiseren van uw app `AuthenticationContext` -ADAL de primaire klasse.  Dit is waar u ADAL doorgeven de coördinaten die nodig is om te communiceren met Azure Active Directory en hoe deze tokens in de cache.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Zoek nu de `Search(...)` methode, die wordt aangeroepen wanneer de gebruiker cliks de 'Search' knop in de gebruikersinterface van de toepassing.  Deze methode wordt een GET-verzoek naar de Azure AD Graph API aan query voor gebruikers waarvoor UPN met de opgegeven zoekterm begint.  Maar query's op de Graph API, moet u een access_token in de `Authorization` kop van de aanvraag - dit is waar ADAL binnenkomt.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Als uw app een token aanvragen door het aanroepen van `AcquireTokenAsync(...)`, ADAL zal proberen om terug te keren een token zonder dat de gebruiker om referenties gevraagd.  Als ADAL bepaalt dat de gebruiker zich aanmelden moet bij een token ophalen, wordt het een aanmeldingsdialoogvenster weergegeven referenties van de gebruiker verzamelen en retourneren een token bij een geslaagde verificatie.  Als ADAL kan retourneren een token voor welke reden dan ook, het genereert een `AdalException`.
- Merk op dat de `AuthenticationResult` -object bevat een `UserInfo` -object dat kan worden gebruikt om uw app moet mogelijk informatie te verzamelen.  In de DirectorySearcher, `UserInfo` wordt gebruikt voor het aanpassen van de gebruikersinterface van de toepassing met de id van de gebruiker.

- Wanneer de gebruiker op de knop "Afmelden", willen we ervoor te zorgen dat de volgende aanroep `AcquireTokenAsync(...)` vraagt de gebruiker aan te melden.  Met ADAL is dit net zo eenvoudig als het token cache wissen:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Echter, als de gebruiker heeft niet op de knop "Afmelden", zult u aan de sessie van de gebruiker voor de volgende keer uitvoeren van de DirectorySearcher.  Wanneer de toepassing wordt gestart, kunt u de tokencache van ADAL voor een bestaande token controleren en de UI dienovereenkomstig bijwerken.  In de `CheckForCachedToken()` methode, maken een andere aanroep van `AcquireTokenAsync(...)`, deze tijd wordt doorgegeven de `PromptBehavior.Never` parameter.  `PromptBehavior.Never`hoogte ADAL dat de gebruiker niet moet worden gevraagd voor aanmelden en ADAL in plaats daarvan een uitzondering genereert moeten als het resultaat van een token kan niet.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Gefeliciteerd! U nu hebt een werkende .NET WPF toepassing die de mogelijkheid heeft om gebruikers te verifiëren, veilig gebruik OAuth 2.0 Web-API's aanroepen en elementaire informatie over de gebruiker.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van de huurder met sommige gebruikers.  Uitvoeren van uw DirectorySearcher-app en log in met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van de UPN.  De toepassing sluiten en opnieuw worden uitgevoerd.  U ziet hoe de sessie blijft intact.  Afmelden en opnieuw aanmelden als een andere gebruiker.

ADAL kunt u al deze gemeenschappelijke identiteit-functies opnemen in uw toepassing.  Dit zorgt voor het vuile werk voor u - Cachebeheer, ondersteuning OAuth-protocol, dat de gebruiker een login UI, het vernieuwen van verlopen tokens en nog veel meer.  Alles wat u moet weten is dat een enkele API-aanroep `authContext.AcquireTokenAsync(...)`.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  U kunt nu op verplaatsen naar nog meer scenario's.  U kunt proberen:

[Een .NET Web API met Azure Active Directory beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
