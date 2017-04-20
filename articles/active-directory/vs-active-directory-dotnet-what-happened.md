<properties
    pageTitle="Wat is er gebeurd met mijn MVC-project (Visual Studio Azure Active Directory verbonden service) | Microsoft Azure "
    description="Hierin wordt beschreven wat er gebeurt met uw project MVC wanneer u verbinding met Azure AD met behulp van Visual Studio verbonden services"
    services="active-directory"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn MVC-project (Visual Studio Azure Active Directory verbonden service)?

> [AZURE.SELECTOR]
> - [Aan de slag](vs-active-directory-dotnet-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Verwijzingen zijn toegevoegd.

### <a name="nuget-package-references"></a>NuGet package verwijzingen

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET-verwijzingen

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Code is toegevoegd.

### <a name="code-files-were-added-to-your-project"></a>Codebestanden zijn toegevoegd aan het project

Een verificatie opstarten-klasse **App_Start/Startup.Auth.cs** is toegevoegd aan uw project met de logica voor de verificatie van AD Azure opstarten. Een klasse controller, Controllers/AccountController.cs is ook toegevoegd waarin de methoden **SignIn()** en **SignOut()** . Ten slotte een gedeeltelijke weergave **Views/Shared/_LoginPartial.cshtml** met een actiekoppeling naar voor aanmelding/SignOut is toegevoegd.

### <a name="startup-code-was-added-to-your-project"></a>Opstartcode is toegevoegd aan uw project

Als u al een Opstartklasse in uw project, is de methode **configuratie** bijgewerkt met een aanroep van **ConfigureAuth(app)**. Anders wordt is een Opstartklasse toegevoegd aan uw project.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>App.config- of web.config heeft nieuwe waarden van de systeemconfiguratie

De volgende configuratie-items zijn toegevoegd.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Een App Azure Active Directory (AD) is gemaakt.
Een Azure AD-toepassing is gemaakt in de map die u hebt geselecteerd in de wizard.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Als ik gecontroleerd dat *afzonderlijke gebruikersaccounts verificatie uitschakelen*, wat aanvullende wijzigingen aan mijn project?
NuGet package verwijzingen zijn verwijderd en de bestanden zijn verwijderd en wordt een back-up. Afhankelijk van de status van uw project, kunt u wellicht handmatig aanvullende verwijzingen of bestanden verwijderen of de code desgewenst wijzigen.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet package verwijzingen verwijderd (voor degenen die aanwezig zijn)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Codebestanden een back-up en verwijderd (voor degenen die aanwezig zijn)

Elk van de volgende bestanden een back-up en verwijderd uit het project. Back-upbestanden bevinden zich in een "Back-up"-map in de hoofdmap van de map van het project.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Codebestanden een back-up (voor degenen die aanwezig zijn)

Elk van de volgende bestanden back-up voordat het wordt vervangen. Back-upbestanden bevinden zich in een "Back-up"-map in de hoofdmap van de map van het project.

- **Startup.cs**
- **App_Start\Startup.auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Als ik *alleen Active directory-gegevens*hebt ingeschakeld, wat aanvullende wijzigingen aan mijn project?

Aanvullende verwijzingen zijn toegevoegd.

###<a name="additional-nuget-package-references"></a>Aanvullende naslaginformatie voor NuGet package

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Aanvullende naslaginformatie van .NET

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Aanvullende codebestanden zijn toegevoegd aan het project

Twee bestanden zijn toegevoegd ter ondersteuning van token in cache plaatsen: **Models\ADALTokenCache.cs** en **Models\ApplicationDbContext.cs**.  Een extra domeincontroller en bekijken zijn om te illustreren bij toegang tot informatie over gebruikersprofielen met Azure graph API's toegevoegd.  Deze bestanden zijn **Controllers\UserProfileController.cs** en **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Aanvullende code voor opstarten is toegevoegd aan uw project

In het bestand **startup.auth.cs** is een nieuw **OpenIdConnectAuthenticationNotifications** -object toegevoegd aan het lid van de **kennisgevingen** van de **OpenIdConnectAuthenticationOptions**.  Dit is de code OAuth ontvangen-en exchange-voor een toegangstoken.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Aanvullende wijzigingen zijn aangebracht in uw app.config of web.config

De volgende aanvullende configuratie-items zijn toegevoegd.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

De volgende configuratiesecties en de verbindingsreeks hebt toegevoegd.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Uw App in Azure Active Directory is bijgewerkt.
Uw Azure Active Directory App is bijgewerkt met de machtiging *gegevens lezen-directory* en de sleutel is gemaakt die vervolgens werd gebruikt als de *ida: ClientSecret* in het bestand **web.config** .

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
