<properties
   pageTitle="Azure Active Directory-verificatie bibliotheken | Microsoft Azure"
   description="Azure AD verificatie bibliotheek (ADAL) client kunnen ontwikkelaars gemakkelijk om gebruikers te verifiëren cloud of Active Directory (AD) ruimten en moet u toegangstokens voor het beveiligen van API-aanroepen."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-verificatie bibliotheken

De verificatie van AD Azure bibliotheek (ADAL) kan ontwikkelaars gemakkelijk om gebruikers te verifiëren cloud client of Active Directory (AD) ruimten en moet u toegangstokens voor het beveiligen van API-aanroepen. ADAL heeft veel functies dat verificatie moet eenvoudiger voor ontwikkelaars, zoals ondersteuning voor asynchrone, een configureerbare tokencache waarin toegangstokens en vernieuwen tokens, automatisch token vernieuwen wanneer een toegangstoken verloopt een token vernieuwen beschikbaar is, en nog veel meer. De meeste van de complexiteit kan verwerken, ADAL gefocust op zakelijke logica in hun toepassing developer en gemakkelijk bronnen beveiligen zonder een deskundige voor de zekerheid.

ADAL is beschikbaar op verschillende platforms.

|Platform|Pakketnaam|Client/Server|Downloaden|Broncode|Documentatie en monsters|
|---|---|---|---|---|---|
|.NET-client Windows Store, UWP, Xamarin iOS en Android|Active Directory-verificatie Library (ADAL) voor .NET v3 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Documentatie](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET-client Windows Store, Windows Phone 8.1 |Active Directory-verificatie-bibliotheek (ADAL) voor .NET v2 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Documentatie](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|Active Directory-verificatie-bibliotheek (ADAL) voor JavaScript|Client|[ADAL voor JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL voor JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Voorbeeld: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|OS X, iOS|Active Directory-verificatie-bibliotheek (ADAL) voor doelstelling-C|Client|[ADAL voor doelstelling-C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL voor doelstelling-C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Voorbeeld: [NativeClient-iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Active Directory-verificatie Library (ADAL) voor Android|Client|[ADAL voor Android (de centrale opslagplaats)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL voor Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Voorbeeld: [NativeClient-Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Active Directory-verificatie (ADAL) naar bibliotheek Node.js|Client|[ADAL voor Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL voor Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Voorbeeld: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory Passport-verificatie middleware voor knooppunt|Client|[Azure Active Directory paspoort voor Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory voor Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Active Directory-verificatie-bibliotheek (ADAL) voor Java|Client|[ADAL voor Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL voor Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Identiteit Protocol-extensies voor Microsoft .NET Framework 4.5|Server|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure AD identiteit model uitbreidingen voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|JSON Web Token-Handler voor het Microsoft .net Framework 4.5|Server|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure AD identiteit model uitbreidingen voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|OWIN middleware waarmee een toepassing van Microsoft-technologie gebruiken voor verificatie.|Server|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|OWIN middleware waarmee een toepassing voor het gebruik van OpenIDConnect voor verificatie.|Server|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Voorbeeld: [WebApp OpenIDConnecty DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN middleware waarmee een toepassing voor het gebruik van WS-Federation voor verificatie.|Server|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Voorbeeld: [WebApp WSFederation DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Scenario 's

Hier zijn drie veelvoorkomende scenario's waarin ADAL kan worden gebruikt voor verificatie.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Verificatie van gebruikers van een clienttoepassing naar een externe bron

In dit scenario heeft een ontwikkelaar een client, zoals een WPF toepassing die nodig zijn voor toegang tot een externe bron die is beveiligd door Azure AD, zoals web-API. Hij heeft een abonnement Azure weet hoe de downstream web API aanroepen en weet de huurder Azure AD die de web-API gebruikt. Hierdoor kan hij gebruiken ADAL verificatie met Azure AD, door volledig de ervaring ADAL verificatie overdragen of referenties van de gebruiker expliciet kan verwerken. ADAL maakt vraagt deze eenvoudige verificatie van de gebruiker, een toegangstoken en vernieuwen token verkrijgen van Azure AD en gebruik vervolgens het toegangstoken te maken op het web API.

Zie voor een voorbeeld dat toont dit scenario met verificatie van AD Azure [Native Client WPF toepassing Web API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Verificatie van een Server-toepassing naar een externe bron

In dit scenario heeft een ontwikkelaar een toepassing wordt uitgevoerd op een server die nodig zijn voor toegang tot een externe bron die is beveiligd door Azure AD, zoals web-API. Hij heeft een abonnement Azure weet hoe de downstream-service starten en weet dat de huurder Azure AD de web-API gebruikt. Als gevolg hiervan kunt hij ADAL verificatie met Azure AD vergemakkelijken door de referenties van de toepassing expliciet te verwerken. ADAL maakt eenvoudig een token met behulp van de toepassing clientreferenties ophalen Azure AD en gebruik deze token te maken op het web API-aanvragen. ADAL verwerkt ook de levensduur van het toegangstoken beheren door deze cache en het vernieuwen van deze indien nodig. Zie voor een voorbeeld dat dit scenario toont [Consoletoepassing voor Web-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Verificatie van een Server-toepassing namens een gebruiker toegang te krijgen tot een externe bron

In dit scenario heeft een ontwikkelaar een toepassing wordt uitgevoerd op een server die nodig zijn voor toegang tot een externe bron die is beveiligd door Azure AD, zoals web-API. De aanvraag moet ook worden gedaan voor een gebruiker in AD Azure. Hij heeft een abonnement Azure weet hoe de downstream web API aanroepen en weet de Azure AD pachters van de service wordt gebruikt. Als de gebruiker is geverifieerd voor de webtoepassing, kan de toepassing een autorisatiecode ophalen voor de gebruiker van Azure AD. De webtoepassing kunt ADAL vervolgens een toegangstoken ophalen en vernieuwen van het token voor een gebruiker met de machtiging en de client referenties die zijn gekoppeld aan de toepassing van Azure AD gebruiken. Als de webtoepassing in het bezit is van het toegangstoken is, kan het web API aanroepen totdat het token is verlopen. Als het token is verstreken, kunt de webtoepassing ADAL als u een nieuwe access token met behulp van de vernieuwing van de token dat eerder is ontvangen.


## <a name="see-also"></a>Zie ook

[De Azure Active Directory handleiding voor ontwikkelaars](active-directory-developers-guide.md)

[Verificatie-scenario's voor Azure Active directory](active-directory-authentication-scenarios.md)

[Azure Active Directory-codevoorbeelden](active-directory-code-samples.md)
