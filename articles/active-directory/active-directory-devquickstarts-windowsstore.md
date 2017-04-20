<properties
    pageTitle="Archief van Windows Azure AD aan de slag | Microsoft Azure"
    description="Het bouwen van een Windows Store-toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Azure AD integreren met een Windows Store-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Als u een app voor de Windows Store ontwikkelt, kunt Azure AD u eenvoudig en ongecompliceerd voor u uw gebruikers met de Active Directory-accounts worden geverifieerd.  Bovendien kunt uw toepassing elk web API beschermd door Azure AD, zoals de Office 365-API of de API Azure veilig in beslag neemt.

Azure AD biedt Windows Store desktop apps die toegang moeten krijgen tot beveiligde bronnen, de bibliotheek van Active Directory-verificatie of ADAL.  ADAL van enige doel in het leven is kunt u gemakkelijk uw app toegangstokens ophalen.  U kunt zien hoe gemakkelijk het is, bouw hier we een 'Directory Searcher' Windows Store app die:

-   Haalt access tokens voor de Azure AD Graph API met het [verificatieprotocol OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)aanroept.
-   Zoekt in een map voor gebruikers met een bepaalde UPN.
-   Symptomen gebruikers.

Als u de volledige toepassing van de werken, moet u naar:

2. Uw toepassing met Azure Active Directory registreren.
3. Installeren en configureren van ADAL.
5. ADAL gebruiken om tokens uit Azure Active Directory.

Ophalen is gestart, [een skelet project downloaden](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) of [het monster voltooid](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Elk is een oplossing met Visual Studio 2015.  U moet ook een huurder Azure AD kunt u gebruikers maken en registreren van een toepassing.  Als u een huurder, [Lees hoe u een](active-directory-howto-tenant.md)nog geen hebt.

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
- Ook in het tabblad **configureren** , Ga naar de sectie 'Machtigingen voor andere toepassingen'.  Voeg de machtiging voor **toegang tot de map als de gebruiker ingelogd** onder **Overgedragen machtigingen**voor de toepassing 'Azure Active Directory'.  Hierdoor kan uw toepassing kan een query uitvoeren op de Graph API voor gebruikers.

## <a name="2-install--configure-adal"></a>*2. Installeer & ADAL configureren*
U kunt nu dat er een toepassing in Azure AD, ADAL installeren en uw identiteit gerelateerde code te schrijven.  Voor ADAL te kunnen communiceren met Azure Active Directory moet, u te verstrekken informatie over de registratie van uw app.
-   Begin met het ADAL aan het DirectorySearcher-project met de Package Manager-Console toe te voegen.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Open in het project DirectorySearcher `MainPage.xaml.cs`.  Vervang de waarden in de `Config Values` regio aan de waarden die u in de Portal Azure ingevoerd.  De code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `tenant` is het domein van de huurder Azure AD, bv. contoso.onmicrosoft.com
    -   De `clientId` is de clientId van de toepassing die u hebt gekopieerd vanaf de portal.
-   U moet nu de callback-uri voor uw Windows Store app ontdekken.  Een onderbrekingspunt instelt op deze regel in de `MainPage` methode:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- De oplossing bouwen, ervoor te zorgen dat alle verwijzingen naar het pakket worden hersteld.  Als pakketten ontbreken, van de Nuget Package Manager openen en herstellen van de pakketten.
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

    authContext = new AuthenticationContext(authority);
}
```

- Zoek nu de `Search(...)` methode, die wordt aangeroepen wanneer de gebruiker op de knop 'Zoeken' in de gebruikersinterface van de toepassing.  Deze methode wordt een GET-verzoek naar de Azure AD Graph API aan query voor gebruikers waarvoor UPN met de opgegeven zoekterm begint.  Maar query's op de Graph API, moet u een access_token in de `Authorization` kop van de aanvraag - dit is waar ADAL binnenkomt.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Als uw app een token aanvragen door het aanroepen van `AcquireTokenAsync(...)`, ADAL zal proberen om terug te keren een token zonder dat de gebruiker om referenties gevraagd.  Als ADAL bepaalt dat de gebruiker zich aanmelden moet bij een token ophalen, wordt het een aanmeldingsdialoogvenster weergegeven referenties van de gebruiker verzamelen en retourneren een token bij een geslaagde verificatie.  Als ADAL kan een token te retourneren voor een of andere reden is het `AuthenticationResult` status is een fout.

- Nu is het tijd om het gebruik van de de access_token die u zojuist hebt opgehaald.  Ook in de `Search(...)` methode, het token koppelen aan de aanvraag Graph API krijgen in de koptekst van de vergunning:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- U kunt ook de `AuthenticationResult` object weer te geven informatie over de gebruiker in uw app, zoals de id van de gebruiker:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Ten slotte kunt u ADAL voor het ondertekenen van de gebruiker uit de toepassing ook.  Wanneer de gebruiker op de knop "Afmelden", willen we ervoor te zorgen dat de volgende aanroep `AcquireTokenAsync(...)` een teken in de weergave worden weergegeven.  Met ADAL is dit net zo eenvoudig als het token cache wissen:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gefeliciteerd! U nu hebt een werkende Windows Store-app die de mogelijkheid heeft om gebruikers te verifiëren, veilig gebruik OAuth 2.0 Web-API's aanroepen en elementaire informatie over de gebruiker.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van de huurder met sommige gebruikers.  Uitvoeren van uw DirectorySearcher-app en log in met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van de UPN.  De toepassing sluiten en opnieuw worden uitgevoerd.  U ziet hoe de sessie blijft intact.  (Door met de rechtermuisknop op de onderste balk weergegeven) afmelden en opnieuw aanmelden als een andere gebruiker.

ADAL kunt u al deze gemeenschappelijke identiteit-functies opnemen in uw toepassing.  Dit zorgt voor het vuile werk voor u - Cachebeheer, ondersteuning OAuth-protocol, dat de gebruiker een login UI, het vernieuwen van verlopen tokens en nog veel meer.  Alles wat u moet weten is dat een enkele API-aanroep `authContext.AcquireToken*(…)`.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  U kunt nu extra identiteit scenario's op verplaatsen.  U kunt proberen:

[Een .NET Web API met Azure Active Directory beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
