<properties
    pageTitle="Wat is er gebeurd met mijn WebApi-project (Visual Studio Azure Active Directory verbonden service) | Microsoft Azure "
    description="Hierin wordt beschreven wat er gebeurt met uw project MVC Azure AD aansluiten met behulp van Visual Studio WebApi"
  services="active-directory"
    documentationCenter=""
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

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn WebApi-project (Visual Studio Azure Active Directory verbonden service)

> [AZURE.SELECTOR]
> - [Aan de slag](vs-active-directory-webapi-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Verwijzingen zijn toegevoegd.

###<a name="nuget-package-references"></a>NuGet package verwijzingen

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.NET-verwijzingen

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Codewijzigingen

###<a name="code-files-were-added-to-your-project"></a>Codebestanden zijn toegevoegd aan het project

Een verificatie opstarten-klasse **App_Start/Startup.Auth.cs** is toegevoegd aan uw project met de logica voor de verificatie van AD Azure opstarten.

###<a name="startup-code-was-added-to-your-project"></a>Opstartcode is toegevoegd aan uw project

Als u al een Opstartklasse in uw project, de methode van de **configuratie** is bijgewerkt met een aanroep van `ConfigureAuth(app)`. Anders wordt is een Opstartklasse toegevoegd aan uw project.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>App.config- of web.config-bestand heeft de nieuwe waarden van de systeemconfiguratie.

De volgende configuratie-items zijn toegevoegd.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Een Azure AD App is gemaakt.

Een Azure AD-toepassing is gemaakt in de map die u hebt geselecteerd in de wizard.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Als ik gecontroleerd dat *afzonderlijke gebruikersaccounts verificatie uitschakelen*, wat aanvullende wijzigingen aan mijn project?
NuGet package verwijzingen zijn verwijderd en de bestanden zijn verwijderd en wordt een back-up. Afhankelijk van de status van uw project, kunt u wellicht handmatig aanvullende verwijzingen of bestanden verwijderen of de code desgewenst wijzigen.

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet package verwijzingen verwijderd (voor degenen die aanwezig zijn)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Codebestanden een back-up en verwijderd (voor degenen die aanwezig zijn)

Elk van de volgende bestanden een back-up en verwijderd uit het project. Back-upbestanden bevinden zich in een "Back-up"-map in de hoofdmap van de map van het project.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Codebestanden een back-up (voor degenen die aanwezig zijn)

Elk van de volgende bestanden back-up voordat het wordt vervangen. Back-upbestanden bevinden zich in een "Back-up"-map in de hoofdmap van de map van het project.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Als ik *alleen Active directory-gegevens*hebt ingeschakeld, wat aanvullende wijzigingen aan mijn project?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Aanvullende wijzigingen zijn aangebracht in uw app.config of web.config

De volgende aanvullende configuratie-items zijn toegevoegd.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Uw App in Azure Active Directory is bijgewerkt.
Uw Azure Active Directory App is bijgewerkt met de machtiging *gegevens lezen-directory* en de sleutel is gemaakt, die vervolgens werd gebruikt als het *Ida: wachtwoord* in de `web.config` bestand.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
