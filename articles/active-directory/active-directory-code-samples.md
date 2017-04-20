<properties
   pageTitle="Codevoorbeelden Azure Active Directory | Microsoft Azure"
   description="Een index van de codevoorbeelden Azure Active Directory, georganiseerd door scenario."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Codevoorbeelden Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

U kunt Microsoft Azure Active Directory (AD Azure) verificatie en machtiging toevoegen aan uw webtoepassingen en de web-API's. In deze sectie een koppeling naar de codevoorbeelden die tonen hoe dit gebeurt en codefragmenten die u in uw toepassingen gebruiken kunt. Op de voorbeeldpagina code vindt u gedetailleerde read-me onderwerpen die bij de vereisten, installatie en configuratie helpen. En de code is u inzicht geven in de essentiële gedeelten toegelicht.

Zie verificatie-scenario's voor Azure AD inzicht in het basisscenario voor elk monster.

Bijdragen aan onze monsters op GitHub: [Microsoft Azure Active Directory-voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Webbrowser-webtoepassing

Deze voorbeelden laten zien hoe een webtoepassing die zorgt ervoor de browser van de gebruiker dat aanmelden naar Azure AD schrijven.



| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Gebruik OpenID (ASP.Net OpenID verbinding OWIN middleware) verbinding maken om gebruikers van een huurder Azure AD te verifiëren.
| C# / .NET | [WebApp MultiTenant-OpenIdConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Een multi huurder .NET MVC webtoepassing die gebruikmaakt van OpenID (ASP.Net OpenID verbinding OWIN middleware) verbinding maken om gebruikers te verifiëren van meerdere AD Azure huurders.
| C# / .NET | [WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | WS-Federation (WS-Federation-OWIN ASP.Net middleware) gebruiken om gebruikers te verifiëren van een huurder Azure AD.






## <a name="single-page-application-spa"></a>Toepassing van één pagina (SPA)

In dit voorbeeld ziet u hoe een toepassing één pagina is beveiligd met Azure AD schrijven.  

| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| JavaScript, C# / .NET | [SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | ADAL voor JavaScript en Azure AD gebruiken voor het beveiligen van een enkele pagina op basis van AngularJS app geïmplementeerd met een ASP.NET web API back-end.


## <a name="native-application-to-web-api"></a>Oorspronkelijke toepassing web API

Deze codevoorbeelden laten zien hoe bouwen native client die belt web API's die zijn beveiligd met AD Azure. Deze [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) en [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)gebruiken.

| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| JavaScript | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Gebruik de invoegtoepassing ADAL voor Apache Cordova bouwen een Apache Cordova app die een web-API aanroept en Azure AD gebruikt voor verificatie.
| C# / .NET | [NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | Een .NET WPF toepassing die een web-API die aanroept is beveiligd met AD Azure.
| C# / .NET | [NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Een archief van Windows-toepassing die een web-API die aanroept is beveiligd met AD Azure.
| C# / .NET | [NativeClient, WebAPI, MultiTenant, WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Windows Store-toepassing aanroepen van een huurder met meerdere web-API die is beveiligd met Azure Active Directory.
| C# / .NET | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Een native clienttoepassing die een web-API die een token om te handelen namens de oorspronkelijke gebruiker opgehaald en gebruikt vervolgens het token aan te roepen van een andere web-API wordt aangeroepen.
| C# / .NET | [NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Een toepassing voor Windows Store voor Windows Phone 8.1 die een web-API die aanroept is beveiligd met AD Azure.
| ObjC | [NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) | Een iOS-toepassing die een web-API die aanroept is Azure AD voor verificatie vereist.
| C# / .NET | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Een native clienttoepassing die gebruikmaakt van de logica voor het verwerken van een token JWT in een web-API in plaats van OWIN middleware.
| C# / Xamarin | [NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Een Xamarin binding aan de oorspronkelijke Azure AD verificatie bibliotheek (ADAL) voor de Android-bibliotheek.
| C# / Xamarin | [NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Een Xamarin binding aan de oorspronkelijke Azure AD verificatie bibliotheek (ADAL) voor iOS.
| C# / Xamarin | [NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Een project van Xamarin die vijf platforms doelen en een web API aanroept die is beveiligd met AD Azure.
| C# / .NET | [NativeClient Headless DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Een eigen toepassing die verificatie van niet-interactief wordt uitgevoerd en een web API aanroept die is beveiligd met AD Azure.



## <a name="web-application-to-web-api"></a>Webtoepassing voor Web-API

Deze codevoorbeelden [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) weergeven hoe gebruiken om webtoepassingen bouwen die call web API's die zijn beveiligd met AD Azure.

| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [WebApp, WebAPI, OpenIDConnect, DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Een web API met ondertekend in machtigingen van de gebruiker belt.
|  C# / .NET | [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Een web-API met de machtigingen van de toepassing aanroepen.
| C# / .NET | [WebApp-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Toevoegen aan een bestaande webtoepassing vergunning met [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) zodat een web API kan worden aangeroepen.
| JavaScript | [WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Stel een REST API-service die geïntegreerd met Azure Active Directory voor de bescherming van de API. Bevat een Node.js-server met een Web-API.
| C# / .NET | [WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Een multi-huurder MVC web toepassing die gebruikmaakt van OpenID (ASP.Net OpenID verbinding OWIN middleware) verbinding maken om gebruikers te verifiëren van een huurder Azure AD. Een autorisatiecode wordt de Graph API aanroepen.

## <a name="server-or-daemon-application-to-web-api"></a>Server of daemontoepassing op Web API

Deze codevoorbeelden ziet u hoe u een daemon of server-toepassing die bronnen uit een web API haalt met [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) en [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [Daemon DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Een console-programma een web-API wordt aangeroepen. De clientreferenties is een wachtwoord.
| C# / .NET | [Daemon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Een consoletoepassing die een web-API aanroept. De clientreferenties is een certificaat.


## <a name="calling-azure-ad-graph-api"></a>Azure AD Graph API aanroept

Deze voorbeeldcode wordt aangegeven hoe u toepassingen maken die roept de API Azure AD grafiek lezen en schrijven van Active directory-gegevens.

| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| Java | [Java-GraphAPI-WebApp](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Een webtoepassing die de Graph API voor toegang tot directorygegevens Azure AD.
| PHP | [WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Een webtoepassing die de Graph API voor toegang tot directorygegevens Azure AD.
| C# / .NET | [WebApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Een webtoepassing die de Graph API voor toegang tot directorygegevens Azure AD.
| C# / .NET | [ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Deze console app ziet u algemene lees- en schrijfmachtigingen aanroepen van de Graph API en het uitvoeren van de gebruiker-licentie is toegewezen en de miniatuurfoto en koppelingen van een gebruiker bijwerken wordt weergegeven.
| C# / .NET | [ConsoleApp, GraphAPI, DiffQuery, DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Een consoletoepassing die de differentiële query in de Graph API gebruikt om periodieke wijzigingen in gebruikersobjecten in een huurder Azure AD.
| C# / .NET | [WebApp, GraphAPI, DirectoryExtensions, DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Een toepassing MVC wordt gebruikgemaakt van Graph API's voor het genereren van een organigram bedrijf eenvoudig.
| PHP | [WebApp, GraphAPI, DirectoryExtensions, PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Een PHP-toepassing die roept de Graph API voor het registreren van een uitbreiding en vervolgens lezen, bijwerken en verwijderen van de waarden in het kenmerk extensie.


## <a name="authorization"></a>Autorisatie

Deze codevoorbeelden hoe Azure AD gebruiken voor verificatie.

| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Rol gebaseerd toegangsbeheer (RBAC) op groepclaims Azure Active Directory gebruiken in een toepassing die is geïntegreerd met AD Azure uitvoeren.
| C# / .NET | [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Rol gebaseerd toegangsbeheer (RBAC) op rollen Azure Active Directory-toepassing te gebruiken in een toepassing die is geïntegreerd met AD Azure uitvoeren.


## <a name="legacy-walkthroughs"></a>Legacy-scenario 's

Deze scenario's iets oudere technologie gebruiken, maar nog steeds interessant zijn.

| Taal/Platform | Monster | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [Rol- en ACL machtigingen in een toepassing van Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) | Op rollen gebaseerde verificatie (RBAC) en autorisatie op basis van de ACL in een toepassing die is geïntegreerd met AD Azure uitvoeren.
| C# / .NET |  [AAL - Windows Store app REST service - verificatie](http://go.microsoft.com/fwlink/?LinkId=330605) |  [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) (voorheen AAL) voor Windows Store Beta verificatiefuncties gebruiker toevoegen aan een Windows Store-app gebruiken.
| C# / .NET | [ADAL - Native App REST service - verificatie met AAD via het dialoogvenster Browser](http://go.microsoft.com/fwlink/?LinkId=259814) |  [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) verificatiefuncties gebruiker toevoegen aan een WPF-client gebruiken.
| C# / .NET | [ADAL - Native App REST service - verificatie met ACS via de Browser, dialoogvenster](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) en [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) verificatiefuncties gebruiker toevoegen aan een WPF-client gebruiken.
| C# / .NET | [ADAL - Server naar Server-verificatie](http://go.microsoft.com/fwlink/?LinkId=259816) | [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) gebruiken voor het beveiligen van serviceoproepen van het proces een kant van een MVC4 Web API REST service.
| C# / .NET | [Aanmelding toevoegen aan uw webtoepassing met Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Configureren van een toepassing .NET web eenmalige aanmelding voor uw onderneming Azure AD directory uitvoeren.
| C# / .NET | [Ontwikkeling van webtoepassingen met meerdere huurder met Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Azure AD toe te voegen aan de eenmalige aanmelding en de toegangsmogelijkheden directory van een toepassing die .NET via meerdere organisaties gebruiken.
JAVA | [Java monster App voor Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) | Gebruik de Graph API voor toegang tot Active directory-gegevens uit Azure Active Directory.
PHP | [PHP monster App voor Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Gebruik de Graph API voor toegang tot Active directory-gegevens uit Azure Active Directory.
| C# / .NET | [Monster App voor Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) | Gebruik de Graph API voor toegang tot Active directory-gegevens uit Azure Active Directory.
| C# / .NET | [Monster App voor differentiële Query Azure AD grafiek](http://go.microsoft.com/fwlink/?LinkId=275398) | De differentiële query in de Graph API gebruiken om periodieke wijzigingen in gebruikersobjecten in een huurder Azure AD.
| C# / .NET | [Monster App voor het integreren van meerdere huurder wolk toepassing voor Azure advertentie](http://go.microsoft.com/fwlink/?LinkId=275397) | Een toepassing met meerdere huurder integreren met AD Azure.
| C# / .NET | [Beveiligen van een toepassing van de Windows Store en de REST-webservice met Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Een eenvoudige API-webbron en een archief van Windows client-toepassing met behulp van Azure AD maken en de [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [Met behulp van de Graph API Azure AD opvragen](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Configureer een Microsoft .NET-toepassing de Azure AD Graph API gebruiken voor toegang tot gegevens uit een huurder Azure AD directory.

## <a name="see-also"></a>Zie ook

##### <a name="other-resources"></a>Andere bronnen

[Azure Active Directory Developer's Guide](active-directory-developers-guide.md)

[Azure AD Graph API conceptuele en verwijzing](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Helper Library](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

