<properties
    pageTitle="Azure AD Windows Phone aan de slag | Microsoft Azure"
    description="Het bouwen van een Windows Phone-toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>



# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Azure AD integreren met een Windows Phone-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Als u een Windows Phone 8.1-app ontwikkelt, kunt Azure AD u eenvoudig en ongecompliceerd voor u uw gebruikers met de Active Directory-accounts worden geverifieerd.  Bovendien kunt uw toepassing elk web API beschermd door Azure AD, zoals de Office 365-API of de API Azure veilig in beslag neemt.

> [AZURE.NOTE] Deze voorbeeldcode wordt gebruikgemaakt van ADAL v2.0.  Voor de nieuwste technologie kunt u in plaats daarvan proberen onze [Windows Universal zelfstudie met behulp van ADAL v3.0](active-directory-devquickstarts-windowsstore.md).  Als u inderdaad een app voor Windows Phone 8.1 maakt, is dit de juiste plaats.  ADAL v2.0 wordt nog steeds volledig ondersteund en is de aanbevolen manier van de ontwikkeling van apps agianst Windows Phone 8.1 met behulp van Azure AD.

Azure AD biedt voor .NET native clients die toegang moeten krijgen tot beveiligde bronnen, de bibliotheek van Active Directory-verificatie of ADAL.  ADAL van enige doel in het leven is kunt u gemakkelijk uw app toegangstokens ophalen.  U kunt zien hoe gemakkelijk het is, bouw hier we een 'Directory Searcher' Windows Phone 8.1 app die:

-   Haalt access tokens voor de Azure AD Graph API met het [verificatieprotocol OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)aanroept.
-   Zoekt in een map voor gebruikers met een bepaalde UPN.
-   Symptomen gebruikers.

Als u de volledige toepassing van de werken, moet u naar:

2. Uw toepassing met Azure Active Directory registreren.
3. Installeren en configureren van ADAL.
5. ADAL gebruiken om tokens uit Azure Active Directory.

Ophalen is gestart, [een skelet project downloaden](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) of [het monster voltooid](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Elk is een oplossing met Visual Studio 2013.  U moet ook een huurder Azure AD kunt u gebruikers maken en registreren van een toepassing.  Als u een huurder, [Lees hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="1-register-the-directory-searcher-application"></a>*1. de toepassing van de Searcher Directory registreren*
Als u wilt dat uw app tokens ophalen, moet u eerst te registreren in uw huurder Azure AD en verleent deze machtiging voor toegang tot de Azure AD Graph API:

-   Aanmelden bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de nav links op in **Active Directory**
-   Selecteer een huurder waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klik op **toevoegen** in de onderste lade.
-   Volg de aanwijzingen en een nieuwe **Native Client-toepassing**maken.
    -   De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    -   Het **Omleiden van Uri** is een schema en tekenreeks van de combinatie waarmee Azure AD token antwoorden terug.  Tijdelijke voor een aanduidingswaarde invoert en nu bijvoorbeeld `http://DirectorySearcher`.  We zullen later deze waarde vervangen.
-   Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke client-id.  Moet u deze waarde in de volgende secties, dus kopiëren vanaf het tabblad **configureren** .
- Ook in het tabblad **configureren** , Ga naar de sectie 'Machtigingen voor andere toepassingen'.  Voeg de machtiging **toegang tot uw organisatie Directory** onder **Overgedragen machtigingen**voor de toepassing 'Azure Active Directory'.  Hierdoor kan uw toepassing kan een query uitvoeren op de Graph API voor gebruikers.

## <a name="2-install--configure-adal"></a>*2. Installeer & ADAL configureren*
U kunt nu dat er een toepassing in Azure AD, ADAL installeren en uw identiteit gerelateerde code te schrijven.  Voor ADAL te kunnen communiceren met Azure Active Directory moet, u te verstrekken informatie over de registratie van uw app.
-   Begin met het ADAL aan het DirectorySearcher-project met de Package Manager-Console toe te voegen.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Open in het project DirectorySearcher `MainPage.xaml.cs`.  Vervang de waarden in de `Config Values` regio aan de waarden die u in de Portal Azure ingevoerd.  De code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `tenant` is het domein van de huurder Azure AD, bv. contoso.onmicrosoft.com
    -   De `clientId` is de clientId van de toepassing die u hebt gekopieerd vanaf de portal.
-   U moet nu de callback-uri voor uw Windows Phone-app ontdekken.  Een onderbrekingspunt instelt op deze regel in de `MainPage` methode:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- De toepassing uitvoeren en kopieer gereserveerd de waarde van `redirectUri` wanneer het onderbrekingspunt wordt bereikt.  Het bestand ziet er ongeveer als volgt

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Vervang de waarde van de **RedirectUri** met deze waarde weer in het tabblad **configureren** van uw toepassing in Azure Management Portal.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Gebruik ADAL Tokens ophalen van AAD*
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, aanroept `authContext.AcquireToken(…)`, en ADAL doet de rest.  

-   De eerste stap is het initialiseren van uw app `AuthenticationContext` -ADAL de primaire klasse.  Dit is waar u ADAL doorgeven de coördinaten die nodig is om te communiceren met Azure Active Directory en hoe deze tokens in de cache.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

- Zoek nu de `Search(...)` methode, die wordt aangeroepen wanneer de gebruiker cliks de 'Search' knop in de gebruikersinterface van de toepassing.  Deze methode wordt een GET-verzoek naar de Azure AD Graph API aan query voor gebruikers waarvoor UPN met de opgegeven zoekterm begint.  Maar query's op de Graph API, moet u een access_token in de `Authorization` kop van de aanvraag - dit is waar ADAL binnenkomt.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
- Interactieve verificatie nodig is, ADAL van Windows Phone Web Authentication Broker (WAB) wordt gebruikt als [voortzetting model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) om de Azure advertentie weer te geven pagina aanmelden.  Wanneer de gebruiker zich aanmeldt, moet uw app ADAL de resultaten van de interactie van het Windows-Adresboek.  Dit is net zo eenvoudig als de uitvoering van de `ContinueWebAuthentication` interface:

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- Nu is het tijd om het gebruik van de `AuthenticationResult` die ADAL aan uw app geretourneerd.  In de `QueryGraph(...)` terugbellen, de access_token die u hebt verkregen, koppelen aan het GET-verzoek in de koptekst van de vergunning:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
- U kunt ook de `AuthenticationResult` object weer te geven informatie over de gebruiker in uw app. In de `QueryGraph(...)` -methode worden gebruikt om de id van de gebruiker op de pagina weergeven:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Ten slotte kunt u ADAL voor het ondertekenen van de gebruiker uit de toepassing ook.  Wanneer de gebruiker op de knop "Afmelden", willen we ervoor te zorgen dat de volgende aanroep `AcquireTokenSilentAsync(...)` mislukt.  Met ADAL is dit net zo eenvoudig als het token cache wissen:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gefeliciteerd! U nu hebt een werkende Windows Phone-app die de mogelijkheid heeft om gebruikers te verifiëren, veilig gebruik OAuth 2.0 Web-API's aanroepen en elementaire informatie over de gebruiker.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van de huurder met sommige gebruikers.  Uitvoeren van uw DirectorySearcher-app en log in met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van de UPN.  De toepassing sluiten en opnieuw worden uitgevoerd.  U ziet hoe de sessie blijft intact.  Afmelden en opnieuw aanmelden als een andere gebruiker.

ADAL kunt u al deze gemeenschappelijke identiteit-functies opnemen in uw toepassing.  Dit zorgt voor het vuile werk voor u - Cachebeheer, ondersteuning OAuth-protocol, dat de gebruiker een login UI, het vernieuwen van verlopen tokens en nog veel meer.  Alles wat u moet weten is dat een enkele API-aanroep `authContext.AcquireToken*(…)`.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  U kunt nu extra identiteit scenario's op verplaatsen.  U kunt proberen:

[Een .NET Web API met Azure Active Directory beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]