<properties
   pageTitle="Werken met identiteiten op basis van een claim in multitenant toepassingen | Microsoft Azure"
   description="Hoe een gebruik voor de validatie van de uitgevende instelling en machtiging vorderingen"
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
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Werken met identiteiten in multitenant-toepassingen op basis van claims

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dit artikel maakt [deel uit van een serie]. Er is ook een volledige [voorbeeldtoepassing] die bij deze reeks.

## <a name="claims-in-azure-ad"></a>Vorderingen in Azure AD

Wanneer een gebruiker zich aanmeldt, stuurt AD Azure een ID-token dat een aantal beweringen over de gebruiker bevat. Een claim is een stukje informatie, uitgedrukt als een sleutel/waarde-paar. Bijvoorbeeld: `email` = `bob@contoso.com`.  Vorderingen hebben een uitgevende instelling &mdash; in dit geval Azure AD &mdash; is de entiteit die de gebruiker wordt geverifieerd en de vorderingen maakt. U vertrouwt de vorderingen, omdat u de uitgever vertrouwt. (Omgekeerd, als u de uitgever niet vertrouwt, niet vertrouwt de claims!)

Op een hoog niveau:

1.  De gebruiker wordt geverifieerd.
2.  De IDP verzendt een aantal vorderingen.
3.  De app normaliseert of wordt de claims (optioneel).
4.  De app maakt gebruik van de claims autorisaties.

In OpenID verbinding maakt, wordt de set claims die u geregeld door de [parameter scope] van de verificatieaanvraag. Azure AD problemen echter met een beperkt aantal vorderingen tot en met OpenID verbinding; Zie [ondersteunde Token en typen]. Als u meer informatie over de gebruiker wilt, moet u de Azure AD Graph API gebruiken.

Hier volgen enkele van de claims van AAD die meestal een app mogelijk interesseren:

Claimtype ID token |    Beschrijving
-----------------------|--------------
AUD | Wie het token is uitgegeven voor. Dit is de toepassing van de client-ID. In het algemeen beter niet moet u zorgen te maken over deze vordering, omdat automatisch wordt gevalideerd in de middleware. Voorbeeld:`"91464657-d17a-4327-91f3-2ed99386406f"`
groepen   | Een lijst met AAD groepen waarvan de gebruiker lid is. Voorbeeld:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | De [uitgever] van het token van de OIDC. Voorbeeld:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
naam    | De weergavenaam van de gebruiker. Voorbeeld:`"Alice A."`
OID | De object-id voor de gebruiker in AAD. Deze waarde is de onveranderbare en herbruikbare-id van de gebruiker. Gebruik deze waarde, e-mail, als een unieke id voor de gebruikers. e-mailadressen kunnen wijzigen. Als u de Azure AD Graph API in uw app, is object-ID die waarde gebruikt om informatie-profiel. Voorbeeld:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
rollen   | Een lijst van de app rollen voor de gebruiker. Voorbeeld:`["SurveyCreator"]`
TID | De id van de huurder. Deze waarde is een unieke id voor de huurder in Azure AD. Voorbeeld:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Een mens leesbaar weergegeven naam van de gebruiker. Voorbeeld:`"alice@contoso.com"`
UPN | UPN-naam. Voorbeeld:`"alice@contoso.com"`

Deze tabel bevat de typen zoals ze worden weergegeven in het token ID. In ASP.NET 1.0 Core zet de middleware OpenID verbinding sommige van de typen wanneer deze wordt gevuld met de collectie Claims voor de gebruiker principal:

-   OID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   UPN >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Claims transformaties

Tijdens de verificatie-stroom wilt u mogelijk de claims die u van de IDP krijgt wijzigen. In ASP.NET Core 1.0, kunt u claims transformatie in de gebeurtenis **AuthenticationValidated** uitvoeren vanaf de middleware OpenID verbinding. (Zie [verificatiegebeurtenissen].)

Alle claims die u tijdens de **AuthenticationValidated toevoegt** worden opgeslagen in de verificatiecookie sessie. Ze krijgen niet terug geduwd naar Azure AD.

Hier volgen enkele voorbeelden van transformatie vorderingen:

-   **Normalisatie van claims**of die claims consistente tussen gebruikers. Dit is met name van belang als uw claims van meerdere IDPs, die verschillende claimtypen dergelijke informatie mogelijk gebruiken.
Azure AD stuurt, bijvoorbeeld een 'upn' claim dat e-mailadres van de gebruiker bevat. Andere IDPs kunnen een claim "email" verzenden. De volgende code wordt de claim 'upn' converteert naar een claim "email":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Toevoegen **aanspraak maken op standaard waarden** voor vorderingen die niet beschikbaar &mdash; bijvoorbeeld een gebruiker toewijzen aan een standaardrol. In sommige gevallen kan dit steekhoudende vereenvoudigen.
- **Type aangepaste claim** met specifieke informatie over de gebruiker toevoegen. U kunt bepaalde informatie over de gebruiker bijvoorbeeld opslaan in een database. U kunt een aangepaste claim met de volgende informatie toevoegen aan de verificatieticket. De claim is opgeslagen in een cookie, dus u alleen hoeft te downloaden uit de database eenmaal per aanmeldsessie. Aan de andere kant, wilt u ook geen uitzonderlijk groot cookies maken zodat moet u rekening houden met de verhouding tussen de grootte van de cookie versus database-lookups.   

Nadat de stroom van de verificatie voltooid is, de claims zijn beschikbaar in `HttpContext.User`. Op dat moment u ze moet behandelen als een alleen-lezen collectie &mdash; ze bijvoorbeeld gebruiken om autorisatiebeslissingen te nemen.

## <a name="issuer-validation"></a>Validatie van de uitgevende instelling
In OpenID aansluit identificeert de uitgevende instelling claim ("iss') de IDP die het ID-token verleend. Deel van de stroom OIDC verificatie is om te controleren of het verzoek van de uitgevende instelling komt overeen met de werkelijke uitgever. De middleware OIDC doet dit.

In Azure AD, de waarde van de uitgevende instelling is uniek per huurder AD (`https://sts.windows.net/<tenantID>`). Een toepassing moet daarom een aanvullende controle, om ervoor te zorgen dat de uitgevende instelling vertegenwoordigt een huurder die zich aanmelden bij de app mag doen.

Voor een toepassing één huurder kunt u alleen controleren of de uitgevende instelling zelf huurder is. In feite de middleware OIDC gebeurt dit automatisch standaard. In een toepassing met meerdere huurder moet u toestaan voor meerdere uitgevende instellingen overeenkomen met de verschillende huurders. Dit is een algemene aanpak te gebruiken:

-   In de OIDC middleware-opties stelt u **ValidateIssuer** in op false. Hiermee schakelt u de automatische controle uit.
-   Als een huurder zich aanmeldt, de huurder en opslaan van de uitgevende instelling in uw gebruiker DB.
-   Wanneer een gebruiker zich aanmeldt, zoekt u de uitgevende instelling in de database. Als de uitgever niet wordt gevonden, betekent dit dat de huurder niet is aangemeld. U kunt ze omleiden naar een teken van de pagina.
-  U kunt ook bepaalde huurders; blacklist bijvoorbeeld: voor klanten die hun inschrijving niet heeft betaald.

Zie voor meer informatie over [-aanmelding en pachters onboarding in een toepassing multitenant][signup].

## <a name="using-claims-for-authorization"></a>Met behulp van aanvragen om toelating

Met claims is de identiteit van een gebruiker niet langer een monolithisch entiteit. Een gebruiker kan bijvoorbeeld een e-mailadres, telefoonnummer, verjaardag, geslacht, enz. Misschien IDP van de gebruiker alle opgeslagen gegevens. Maar wanneer de verificatie van de gebruiker gewoonlijk krijgt u een subset van deze vorderingen. In dit model wordt de identiteit van de gebruiker gewoon een bundel van claims. Wanneer u besluit over een gebruiker vergunning, zoekt u bepaalde soorten vorderingen. Met andere woorden, de vraag 'Is gebruiker X hebben claimen Z' uiteindelijk "Gebruiker X actie kunt uitvoeren Y" wordt.

Hier volgen enkele eenvoudige patronen voor de controle van de vorderingen.

-  Om te controleren of de gebruiker een bepaalde claim hebt met een bepaalde waarde heeft:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Deze code controleert of de gebruiker een claim rol met de waarde 'Admin heeft'. Het geval waar de gebruiker geen rol vordering of claims van meerdere rol heeft correct verwerkt.

    De klasse **ClaimTypes** definieert constanten voor veelgebruikte typen. U kunt echter een string-waarde voor het claimtype.

-   Een single-waarde opvragen voor een type identiteitsclaim wanneer u verwacht dat er maximaal één waarde:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Alle waarden voor een type identiteitsclaim ophalen:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Zie [rol- en resource vergunning in multitenant-toepassingen]voor meer informatie[authorization].

## <a name="next-steps"></a>Volgende stappen

- Lees het volgende artikel in deze serie: [aanmelden en pachters onboarding in een multitenant toepassing][signup]
- Meer informatie over [verificatie op basis van Claims] in de documentatie van ASP.NET Core 1.0

<!-- Links -->
[bij een reeks hoort]: guidance-multitenant-identity.md
[parameter scope]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Ondersteunde Token en typen]: ../active-directory/active-directory-token-and-claims.md
[uitgevende instelling]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Verificatiegebeurtenissen]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Op claims gebaseerde verificatie]: https://docs.asp.net/en/latest/security/authorization/claims.html
[voorbeeldtoepassing]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
