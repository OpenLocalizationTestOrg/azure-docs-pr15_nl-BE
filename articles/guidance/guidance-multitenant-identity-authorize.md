<properties
   pageTitle="Vergunning in multitenant toepassingen | Microsoft Azure"
   description="Het uitvoeren van verificatie in een multitenant-toepassing"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Rol- en resource vergunning in multitenant-toepassingen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dit artikel maakt [deel uit van een serie]. Er is ook een volledige [voorbeeldtoepassing] die bij deze reeks.

Onze [referentie-implementatie] is een toepassing van ASP.NET Core 1.0. In dit artikel zullen we twee algemene benaderingen te machtigen, met de vergunning verstrekt in ASP.NET Core 1.0 API's.

-   **Verificatie op basis van rollen**. Een actie op basis van de rollen die zijn toegewezen aan een gebruiker toestemming. Voor sommige bewerkingen moet bijvoorbeeld een beheerdersrol.
-   **Verificatie op basis van een resource**. Machtiging van een actie op basis van een bepaalde resource. Elke resource heeft bijvoorbeeld een eigenaar. De eigenaar kan verwijderen van de bron; andere gebruikers kunnen niet.

Een typische app maakt gebruik van een combinatie van beide. Bijvoorbeeld, als u een bron verwijdert, moet de gebruiker zijn de bron eigenaar _of_ een beheerder.


## <a name="role-based-authorization"></a>Verificatie op basis van rollen

De [Enquêtes van Tailspin] [ Tailspin] toepassing definieert de volgende rollen:

- Beheerder. Kan alle CRUD bewerkingen uitvoeren op een enquête die bij deze huurder hoort.
- Maker. Nieuwe enquêtes kunt maken
- Lezer. Eventuele onderzoeken die tot de huurder die behoren kan worden gelezen.

Rollen van toepassing op _gebruikers_ van de toepassing. In de toepassing onderzoeken is een gebruiker een beheerder, auteur of lezer.

Zie voor een bespreking van het definiëren en beheren van rollen, [rollen van de toepassing].

Ongeacht hoe u de rollen beheren, zal uw validatiecode lijken. Core ASP.NET 1.0 introduceert een abstractie [Autorisatiebeleid]genaamd[policies]. Met deze functie autorisatiebeleid in code definiëren en vervolgens beleid toepassen op acties van de controller. Het beleid wordt van de domeincontroller ontkoppeld.

### <a name="create-policies"></a>Beleid maken

Als u een beleid definieert, maakt u eerst een klasse die implementeert `IAuthorizationRequirement`. Het gemakkelijkst is afgeleid van `AuthorizationHandler`. In de `Handle` methode, de relevante claim(s) te onderzoeken.

Hier volgt een voorbeeld van de toepassing van Tailspin enquêtes:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Zie [SurveyCreatorRequirement.cs]

Deze klasse definieert het vereiste voor een gebruiker een nieuwe enquête te maken. De gebruiker moet zich in de rol van SurveyAdmin of SurveyCreator.

Definieert een benoemde beleid met een of meer vereisten in uw Opstartklasse. Als er meerdere voorwaarden zijn, moet _elke_ behoefte te mogen voldoen aan de gebruiker. De volgende code definieert twee beleidsregels:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Zie [Startup.cs]

Deze code wordt ook het verificatieschema, wordt ASP.NET welke middleware verificatie moet worden uitgevoerd als de verificatie mislukt. In dit geval geven we de middleware cookie-verificatie omdat de middleware cookie-verificatie de gebruiker naar een pagina 'omleiden kan'. De locatie van de pagina is ingesteld met de optie AccessDeniedPath voor de cookie middleware; Zie [de middleware verificatie configureren].

### <a name="authorize-controller-actions"></a>Controller acties toestaan

Stel ten slotte voor het machtigen van een actie in een MVC controller het beleid de `Authorize` kenmerk:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

In eerdere versies van ASP.NET stelt u de eigenschap **rollen** op het kenmerk:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

Dit wordt nog steeds ondersteund in ASP.NET Core 1.0, maar heeft enkele nadelen ten opzichte van autorisatiebeleid:

-   Er wordt een bepaalde claimtype aangenomen. Beleid kunnen voor elk type identiteitsclaim controleren. Rollen zijn een type claim.
-   Naam van de rol is hard gecodeerd in het kenmerk. Beleid is de logica van de vergunning op één plaats, waardoor het gemakkelijker bijwerken of zelfs laden van de configuratie-instellingen.
-   Beleid meer complexe autorisatiebeslissingen inschakelen (bijv. de leeftijd > = 21) die door het lidmaatschap van een eenvoudige rol kan niet worden uitgedrukt.

## <a name="resource-based-authorization"></a>Vergunning op basis van bron

_Verificatie op basis van een resource_ treedt op wanneer de vergunning afhankelijk is van een bepaalde resource die worden beïnvloed door een bewerking. In de toepassing onderzoeken Tailspin heeft elke enquête een eigenaar en medewerkers van nul-op-veel.

-   De eigenaar kan lezen, bijwerken, verwijderen, publiceren en de enquête ongedaan.
-   De eigenaar kunt u medewerkers toewijzen aan de enquête.
-   Inzenders kunnen lezen en bijwerken van de enquête.

'Eigenaar' en 'contributor' zijn geen rollen van toepassing; ze worden opgeslagen per enquête in de database van de toepassing. Als u wilt controleren of een gebruiker een enquête kunt verwijderen, bijvoorbeeld, controleert de app of de gebruiker de eigenaar van dat onderzoek is.

In ASP.NET 1.0 Core implementeren autorisatie op basis van een resource die voortvloeien uit de **AuthorizationHandler** en de methode **verwerkt** .

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

U ziet dat deze klasse wordt ingevoerd voor enquête-objecten.  De klasse voor DI registreren bij het opstarten:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Gebruik voor het uitvoeren van controles van de vergunning, de **IAuthorizationService** -interface kunt u uw domeincontrollers injecteren. De volgende code wordt gecontroleerd of een gebruiker een enquête kan lezen:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Omdat geven we in een `Survey` object, roept deze aanroep van de `SurveyAuthorizationHandler`.

Een goede aanpak is in uw validatiecode statistische alle van de rol- en resource machtigingen van de gebruiker, is het selectievakje de statistische waarde instellen op de gewenste bewerking.
Hier volgt een voorbeeld van de toepassing van de enquêtes. De toepassing definieert verschillende typen van machtiging:

- Admin
- Inzender
- De maker
- Eigenaar
- Reader

De toepassing definieert ook een aantal mogelijke bewerkingen op onderzoek:

- Maken
- Lezen
- Update
- Verwijderen
- Publiceren
- Unpublsh

De volgende code maakt een lijst met machtigingen voor een bepaalde gebruiker en onderzoek. U ziet dat deze code wordt gekeken naar de gebruikersrollen app en de eigenaar/Inzender velden in de enquête.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Zie [SurveyAuthorizationHandler.cs].

In een toepassing met meerdere huurder moet u ervoor zorgen dat machtigingen niet '' om de gegevens van een andere huurder lekken. In de toepassing onderzoeken de inzender machtiging is toegestaan tussen huurders &mdash; kunt u iemand van een andere huurder als een contriubutor. Andere machtigingstypen zijn beperkt tot de resources die bij de huurder van de gebruiker horen. Als u deze vereiste, controleert de code de huurder-ID voordat de machtiging wordt verleend. (De `TenantId` veld bij de enquête is toegewezen.)

De volgende stap is om te controleren de bewerking tegen de machtigingen (lezen, bijwerken, verwijderen, enz.). In deze stap implementeert de enquêtes app met behulp van een opzoektabel van functies:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Volgende stappen

- Lees het volgende artikel in deze serie: [beveiliging van een back-end web-API in een multitenant-toepassing][web-api]
- Zie voor meer informatie over de resource op basis van autorisatie in ASP.NET 1.0 kern, [Bron-autorisatie op basis van][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[bij een reeks hoort]: guidance-multitenant-identity.md
[Toepassingsrollen]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[referentie-implementatie]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[De middleware verificatie configureren]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[voorbeeldtoepassing]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
