<properties
    pageTitle="Azure AD Xamarin aan de slag | Microsoft Azure"
    description="Het bouwen van een Xamarin-toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Azure AD integreren in een App Xamarin

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin kunt u mobiele apps schrijven in C# die op iOS, Android en Windows (mobiele apparaten en pc's). Als u een app met Xamarin maakt, kunt Azure AD u eenvoudig en ongecompliceerd voor u uw gebruikers met de Active Directory-accounts worden geverifieerd. Bovendien kunt uw toepassing elk web API beschermd door Azure AD, zoals de Office 365-API of de API Azure veilig in beslag neemt.

Xamarin apps die toegang moeten krijgen tot beveiligde bronnen, biedt Azure AD ADAL of de bibliotheek van Active Directory-verificatie. ADAL van enige doel in het leven is kunt u gemakkelijk uw app toegangstokens ophalen. U kunt zien hoe gemakkelijk het is, bouw hier we een app "Directory Searcher" die:

-   Op iOS, Android, Windows-bureaublad, Windows Phone en Windows Store wordt uitgevoerd.
- Een draagbare klassenbibliotheek (PCL) gebruikt voor de verificatie van gebruikers en tokens krijgen voor de Azure AD Graph API
-   Zoekt in een map voor gebruikers met een bepaalde UPN.

Als u de volledige toepassing van de werken, moet u naar:

2. Instellen van uw ontwikkelomgeving van Xamarin
2. Uw toepassing met Azure Active Directory registreren.
3. Installeren en configureren van ADAL.
5. ADAL gebruiken om tokens uit Azure Active Directory.

Ophalen is gestart, [een skelet project downloaden](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) of [het monster voltooid](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Elk is een oplossing met Visual Studio 2013. U moet ook een huurder Azure AD kunt u gebruikers maken en registreren van een toepassing. Als u een huurder, [Lees hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. uw ontwikkelomgeving Xamarin instellen*
Aangezien deze zelfstudie projecten voor iOS, Android en Windows bevat, moet u Visual Studio en Xamarin samen. Als u wilt maken in de omgeving nodig, de instructies volledig op [instellen en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) op MSDN. Deze instructies bevatten materiaal die u controleren kunt als u wilt weten over Xamarin terwijl u voor de installatieprogramma's wacht te voltooien. 

Eenmaal u de benodigde instellingen hebt voltooid, opent u de oplossing in Visual Studio aan de slag. U vindt zes projecten: vijf platform-specifieke projecten en een draagbare class library die worden gedeeld voor alle platforms`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. de toepassing van de Searcher Directory registreren*
Als u wilt dat uw app tokens ophalen, moet u eerst te registreren in uw huurder Azure AD en verleent deze machtiging voor toegang tot de Azure AD Graph API:

-   Aanmelden bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de nav links op in **Active Directory**
-   Selecteer een huurder waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klik op **toevoegen** in de onderste lade.
-   Volg de aanwijzingen en een nieuwe **Native Client-toepassing**maken.
    -   De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    -   Het **Omleiden van Uri** is een schema en tekenreeks van de combinatie waarmee Azure AD token antwoorden terug. Voer een waarde, bijvoorbeeld `http://DirectorySearcher`.
-   Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke client-id. Moet u deze waarde in de volgende secties, dus kopiëren vanaf het tabblad **configureren** .
- Ook in het tabblad **configureren** , Ga naar de sectie 'Machtigingen voor andere toepassingen'. Voeg de machtiging **toegang tot uw organisatie Directory** onder **Overgedragen machtigingen**voor de toepassing 'Azure Active Directory'. Hierdoor kan uw toepassing kan een query uitvoeren op de Graph API voor gebruikers.

## <a name="2-install--configure-adal"></a>*2. Installeer & ADAL configureren*
U kunt nu dat er een toepassing in Azure AD, ADAL installeren en uw identiteit gerelateerde code te schrijven. Voor ADAL te kunnen communiceren met Azure Active Directory moet, u te verstrekken informatie over de registratie van uw app.
-   Begin met het ADAL aan elk van de projecten in de oplossing met de Package Manager-Console toe te voegen.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- U moet dat twee bibliotheekverwijzingen worden toegevoegd aan elk project - de PCL-gedeelte van het ADAL en een gedeelte van de platform-specifieke merken.

-   Open in het project DirectorySearcherLib `DirectorySearcher.cs`. De klasse lidwaarden om de waarden die u in de Portal Azure invoert wijzigen. De code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `tenant` is het domein van de huurder Azure AD, bv. contoso.onmicrosoft.com
    -   De `clientId` is de clientId van de toepassing die u hebt gekopieerd vanaf de portal.
    - De `returnUri` is de redirectUri die u hebt ingevoerd in het portaal, bijv. `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Gebruik ADAL Tokens ophalen van AAD*
*Almost* alle verificatie-logica van de app wordt veroorzaakt door `DirectorySearcher.SearchByAlias(...)`. Alles wat nodig is in de platform-specifieke projecten is een contextuele parameter doorgeven de `DirectorySearcher` PCL.

- Open eerst `DirectorySearcher.cs` en voeg een nieuwe parameter voor de `SearchByAlias(...)` methode. `IPlatformParameters`is de contextuele parameter die de platform-specifieke objecten die nodig zijn voor het uitvoeren van verificatie ADAL ingekapseld.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Vervolgens initialiseren van de `AuthenticationContext` -ADAL de primaire klasse. Dit is waar u ADAL doorgeven de coördinaten kan communiceren met Azure Active Directory. Contact opnemen met `AcquireTokenAsync(...)`, werken met de `IPlatformParameters` -object en wordt de verificatie stroom nodig om terug te keren een token aan de toepassing wordt aangeroepen.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`eerst wordt geprobeerd om terug te keren een token voor de aangevraagde bron op (de Graph API in dit geval) zonder dat de gebruiker om hun referenties invoeren (via het in cache opslaan of vernieuwen van oude tokens). Alleen indien nodig, weergegeven deze de gebruiker de Azure AD sign in pagina voordat de aangevraagde token verkrijgen.


- Vervolgens koppelt u het toegangstoken aan de aanvraag Graph API in de koptekst van de vergunning:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Dat is alles voor de `DirectorySearcher` PCL en de app de identiteit gerelateerde code.  Die blijft aan te roepen is de `SearchByAlias(...)` -methode in weergaven van elk platform, en waar nodig de code voor het verwerken van de levenscyclus van UI correct toevoegen.

####<a name="android"></a>Android:
- In `MainActivity.cs`, toevoegen van een aanroep naar `SearchByAlias(...)` Klik op de knop handler:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- U moet ook overschrijven de `OnActivityResult` lifecycle methode voor het doorsturen van de verificatie die wordt omgeleid naar de juiste methode.  ADAL biedt een helpmethode voor dit in Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windows-bureaublad:
- In `MainWindow.xaml.cs`, gewoon een gesprek tot stand brengen `SearchByAlias(...)` geven een `WindowInteropHelper` op het bureaublad van de `PlatformParameters` object:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- In `DirSearchClient_iOSViewController.cs`, de iOS `PlatformParameters` object gewoon krijgt een verwijzing naar de Controller weergeven:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Universele Windows:
- Open in Windows Universal, `MainPage.xaml.cs` en implementeren van de `Search` methode, die een helpmethode in een gedeeld project gebruikt voor de UI zo nodig bijwerken.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Gefeliciteerd! U hebt nu een werkende Xamarin app die de mogelijkheid heeft voor de verificatie van gebruikers en veilig OAuth 2.0 voor vijf verschillende platforms met Web-API's aanroepen. Als u nog niet gedaan hebt, is nu de tijd voor het vullen van de huurder met sommige gebruikers. Uitvoeren van uw DirectorySearcher-app en log in met een van deze gebruikers. Zoeken naar andere gebruikers op basis van de UPN.

ADAL gemakkelijk de gemeenschappelijke identiteit-functies opnemen in uw app. Dit zorgt voor het vuile werk voor u - Cachebeheer, ondersteuning OAuth-protocol, dat de gebruiker een login UI, het vernieuwen van verlopen tokens en nog veel meer. Alles wat u moet weten is dat een enkele API-aanroep `authContext.AcquireToken*(…)`.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). U kunt nu extra identiteit scenario's op verplaatsen. U kunt proberen:

[Een .NET Web API met Azure Active Directory beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
