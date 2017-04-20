<properties
   pageTitle="Azure Active Directory rapport controlegebeurtenissen | Microsoft Azure"
   description="Gebeurtenissen die beschikbaar zijn voor het bekijken en downloaden van uw Azure Active Directory gecontroleerd"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory rapport controlegebeurtenissen

*Deze documentatie is onderdeel van de [Azure Active Directory Reporting Guide] (active-directory-rapportage-guide.md).*

Het auditrapport Azure Active Directory kunnen klanten identificeren beschermde acties die heeft plaatsgevonden in Azure Active Directory. Beschermde acties zijn bevoegdheden verandert (bijvoorbeeld een rol maken of opnieuw instellen van wachtwoorden), veranderende beleidsconfiguraties (bijvoorbeeld wachtwoordbeleid) of wijzigingen in de configuratie directory (bijvoorbeeld wijzigingen in de domeininstellingen van federation). De rapporten bevatten de audit record voor de naam van de gebeurtenis, de acteur die de actie, de doelbron is beïnvloed door de wijziging, en de datum en tijd (UTC) heeft uitgevoerd. Klanten kunnen de lijst met controlegebeurtenissen voor hun Azure Active Directory via het [Portal Azure](https://portal.azure.com/)ophalen zoals beschreven in de [weergave van de logboeken controleren](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Lijst met controlegebeurtenissen rapport
<!--- audit event descriptions should be in the past tense --->

Gebeurtenissen                               | Beschrijving van gebeurtenis
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Gebeurtenissen van gebruikers**                      |
Gebruiker toevoegen                             | Een gebruiker aan de directory toegevoegd.
Gebruiker verwijderen                          | Een gebruiker verwijderd uit de map.
Licentie-eigenschappen instellen               | De licentie-eigenschappen ingesteld voor een gebruiker in Active directory.
Gebruikerswachtwoord opnieuw instellen                  | Het wachtwoord voor een gebruiker in de map opnieuw instellen.
Gebruikerswachtwoord wijzigen                 | Het wachtwoord voor een gebruiker in Active directory wordt gewijzigd.
Licentie wijzigen                  | De licentie is toegewezen aan een gebruiker in Active directory wordt gewijzigd. Bekijk onderstaande eigenschappen voor de [gebruikersaccount van de Update](#update-user-attributes) om te zien welke licenties zijn bijgewerkt,
Gebruiker bijwerken                          | Een gebruiker in Active directory bijgewerkt. [Zie hieronder](#update-user-attributes) voor de kenmerken die kunnen worden bijgewerkt.
Instellen werking wijzigen van wachtwoord voor gebruiker       | Stel de eigenschap waardoor hun wachtwoord wijzigen voor aanmelding van een gebruiker.
Referenties van de gebruiker bijwerken        | Gebruiker het wachtwoord gewijzigd  
**Gebeurtenissen groeperen**                     |
Groep toevoegen                            | Een groep gemaakt in de map.
Groep bijwerken                         | Een groep in Active directory bijgewerkt. Raadpleeg [Groep eigenschappen gecontroleerd](#update-group-attributes) in de sectie hieronder om te zien welke eigenschappen zijn bijgewerkt,
Groep verwijderen                         | Een groep verwijderd uit de directory.
Lid aan een groep toevoegen            | Een lid toegevoegd aan een groep in Active directory.
Lid uit een groep verwijderen         | Een lid verwijderd van een groep in de directory.
CreateGroupSettings              | Groepsinstellingen
UpdateGroupSettings          | De instellingen bijgewerkt. Raadpleeg [Groep eigenschappen gecontroleerd](#update-group-attributes) in de sectie hieronder om te zien wat de instellingen zijn bijgewerkt,
DeleteGroupSettings            | Instellingen van de verwijderde groep
SetGroupLicense                  | Groep licenties instellen.
SetGroupManagedBy              | De groep wordt beheerd door de gebruiker instellen
AddGroupMember               | Lid toegevoegd aan een groep
RemoveGroupMember            | Lid uit een groep verwijderen
AddGroupOwner                | De eigenaar van de toegevoegde groep
RemoveGroupOwner                 | Verwijderde eigenaar van de groep
**Application-gebeurtenissen**               |
Service principal toevoegen                | Een service principal aan de directory toegevoegd.
Service principal verwijderen             | Een service principal verwijderd uit de map.
Service principal referenties toevoegen    | Toegevoegde referenties naar een service principal.
Service principal referenties verwijderen | De referenties van een service principal verwijderd.
Overdracht-item toevoegen                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) gemaakt in de map.
Set overdracht post                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) in de map wordt bijgewerkt.
Overdracht vermelding verwijderen              | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) in de map verwijderd.
**Rol van gebeurtenissen**                      |
Rollid toevoegen aan de rol              | Een gebruiker aan de rol van een map toegevoegd.
Rollid van rol verwijderen         | Een gebruiker verwijderd uit een map rol.
Bedrijf instellen      | Niveau van het bedrijf contact voorkeuren instellen. Dit omvat e-mailadressen voor de marketing, alsmede technische meldingen over Microsoft Online Services.
Overdracht-item toevoegen                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) gemaakt in de map.
Set overdracht post                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) in de map wordt bijgewerkt.
Overdracht vermelding verwijderen              | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) in de map verwijderd.
AddSevicePrincipalOwner          | Toegevoegde service principal eigenaar.
RemoveSevicePrincipalOwner   | De eigenaar van de service principal verwijderd.
AddApplication  | Toepassing toevoegen.
UpdateApplication | Toepassing bijwerken. Raadpleeg [Toepassing gecontroleerd van eigenschappen](#update-application-attributes) in de sectie hieronder om te zien welke app instellingen zijn bijgewerkt,
DeleteApplication | Toepassing verwijderen.
RestoreApplication|Toepassing herstellen.
AddApplicationOwner   |     Eigenaar toegevoegd aan de toepassing.
RemoveApplicationOwner    |     De eigenaar van de toepassing verwijderen.
**Rol van gebeurtenissen**                         |
Rollid toevoegen aan de rol                 | Een gebruiker aan de rol van een map toegevoegd.
Rollid van rol verwijderen            | Een gebruiker verwijderd uit een map rol.
AddRoleDefinition               | Toegevoegde roldefinitie.
UpdateRoleDefinition                | Roldefinitie bijgewerkt. Zie [Rol definitie eigenschappen gecontroleerd](#update-role-definition-attributes) in de sectie hieronder om te zien welke instellingen zijn bijgewerkt,
DeleteRoleDefinition                | Roldefinitie verwijderd.
AddRoleAssignmentToRoleDefinition | Extra toewijzing van rol.
RemoveRoleAssignmentFromRoleDefinition | Roltoewijzing uit rol verwijderd.
AddRoleFromTemplate     | De rol van sjabloon toegevoegd.
UpdateRole  | Bijgewerkte rol.
AddRoleScopeMemberToRole    | Bereik lid aan rol toegevoegd.
RemoveRoleScopedMemberFromRole  | Bereik van lid verwijderd uit rol.
**Apparaatgebeurtenissen (alle nieuwe gebeurtenissen)**                    |
AddDevice               | Apparaat.
UpdateDevice            | Bijgewerkte apparaatstuurprogramma. Als u wilt zien welke eigenschappen zijn bijgewerkt, verwijzen naar [Eigenschappen van een apparaat Audited](#update-device-attributes) in de sectie hieronder
DeleteDevice            | Verwijderde apparaat.
AddDeviceConfiguration      | Configuratie van apparaat.
UpdateDeviceConfiguration   | Bijgewerkte apparaatstuurprogramma de configuratie. Als u wilt zien welke configuratie-eigenschappen van het apparaat zijn bijgewerkt, verwijzen naar [Eigenschappen voor configuratie van apparaat Audited](#update-device-configuration-attributes) in de sectie hieronder
DeleteDeviceConfiguration   | Configuratie van het verwijderde apparaat.
AddRegisteredOwner     | Geregistreerde eigenaar toegevoegd aan het apparaat.
AddRegisteredUsers     | Geregistreerde gebruikers toegevoegd aan het apparaat.
RemoveRegisteredOwner    | Geregistreerde eigenaar van het apparaat verwijderen.
RemoveRegisteredUsers    | Geregistreerde gebruikers van het apparaat verwijderen.
RemoveDeviceCredentials  | Inloggegevens verwijderen.
**B2B-gebeurtenissen**                       |
Batch nodigt geüpload.              | Een beheerder kan een bestand met de uitnodigingen worden verzonden naar gebruikers van de partner is geüpload.
Batch nodigt verwerkt.             | Een bestand met uitnodigingen voor gebruikers van de partners is verwerkt.
Externe gebruiker uitnodigen.                | Een externe gebruiker is uitgenodigd voor de map.
Externe gebruiker uitnodigen inwisselen.         | Een externe gebruiker heeft de uitnodiging naar de map worden ingewisseld.
Externe gebruiker toevoegen aan de groep.          | Een externe gebruiker is lidmaatschap toegewezen aan een groep in Active directory.
Externe gebruiker toewijzen aan een toepassing. | Directe toegang tot is een externe gebruiker toegewezen aan een toepassing.
Virale huurder maken.               | Een nieuwe huurder is in Azure AD gemaakt door de terugbetaling van de uitnodiging.
Virale gebruiker maken.                 | Een gebruiker is gemaakt in een bestaande huurder in Azure AD door de terugbetaling van de uitnodiging.
**Administratieve eenheden (alle nieuwe gebeurtenissen)**             |
AddAdministrativeUnit   |   Administratieve eenheid toevoegen.
UpdateAdministrativeUnit|   Administratieve eenheid bijwerken. Raadpleeg [gecontroleerd administratieve eenheidseigenschappen](#update-administrative-unit-attributes) in de sectie hieronder om te zien welke eigenschappen administratieve eenheid zijn bijgewerkt,
DeleteAdministrativeUnit|   Administratieve eenheid te verwijderen.
AddMemberToAdministrativeUnit|  Lid toevoegen aan de administratieve eenheid.
RemoveMemberFromAdministrativeUnit|     Lid verwijderen uit de administratieve eenheid.
**Directory-gebeurtenissen**                 |
Partner aan het bedrijf toevoegen               | Een partner aan de directory toegevoegd.
Partner van het bedrijf verwijderen          | Een partner verwijderd uit de map.
DemotePartner   |   Partner degraderen.
Domein toevoegen aan bedrijf                | Een domein wordt toegevoegd aan de directory.
Domein verwijderen uit bedrijf           | Een domein verwijderd uit de map.
Domein bijwerken                        | Een domein op de map bijgewerkt. Zie [Eigenschappen voor domein gecontroleerd](#update-domain-attributes) in de sectie hieronder om te zien welke domein-eigenschappen zijn bijgewerkt,
Domein-verificatie instellen            | De standaardinstelling voor domein van het bedrijf wordt gewijzigd.
Bedrijf instellen      | Niveau van het bedrijf contact voorkeuren instellen. Dit omvat e-mailadressen voor de marketing, alsmede technische meldingen over Microsoft Online Services.
Federatie-instellingen ingesteld op domein    | De federation-instellingen voor een domein bijgewerkt.
Controleer domein                        | Een domein op de map wordt gecontroleerd.
Controleer of e-mailadres geverifieerd domein         | Een domein op de map met e-controle wordt gecontroleerd.
DirSyncEnabled-vlag instellen op bedrijf   | Stel de eigenschap waarmee een map voor Azure AD-synchronisatie.
Wachtwoordbeleid instellen                  | Lengte en teken beperkingen voor wachtwoorden instellen.
Bedrijfsgegevens instellen              | De gegevens van het niveau van het bedrijf bijgewerkt. Zie de PowerShell-cmdlet [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) voor meer informatie.
SetCompanyAllowedDataLocation  |    Set bedrijf gegevenslocatie toegestaan.
SetCompanyDirSyncEnabled    |   DirSyncEnabled-vlag instellen.
SetCompanyDirSyncFeature |  DirSync-functie instellen.
SetCompanyInformation |   Set bedrijfsgegevens.
SetCompanyMultiNationalEnabled    |     Stel multinationale bedrijf-functie ingeschakeld.
SetDirectoryFeatureOnTenant   |     Functie directory instellen op de huurder.
SetTenantLicenseProperties  |   Huurder licentie-eigenschappen ingesteld.
CreateCompanySettings   |   Instellingen van het bedrijf maken
UpdateCompanySettings   |   Instellingen van het bedrijf werken. Zie [Eigenschappen bedrijf gecontroleerd](#update-company-attributes) in de sectie hieronder om te zien welke eigenschappen van het bedrijf zijn bijgewerkt,
DeleteCompanySettings   |   Bedrijfsinstellingen verwijderen
SetAccidentalDeletionThreshold   |  Onopzettelijke verwijderingen drempel instellen.
SetRightsManagementProperties   |   Rights management-eigenschappen instellen.
PurgeRightsManagementProperties     |   Rights management-eigenschappen verwijderen.
UpdateExternalSecrets   |   Bijwerken van externe geheimen
**Beleidsgebeurtenissen (alle nieuwe gebeurtenissen)**           |
AddPolicy   |   Beleid toevoegen.
UpdatePolicy    |   Beleid bijwerken.
DeletePolicy    |   Beleid verwijderen.
AddDefaultPolicyApplication     |   Beleid toevoegen aan de toepassing.
AddDefaultPolicyServicePrincipal    |   Beleid toevoegen aan service principal.
RemoveDefaultPolicyApplication  |   Beleid verwijderen van toepassing.
RemoveDefaultPolicyServicePrincipal     |   Beleid verwijderen van een service principal.
RemovePolicyCredentials     |   Beleid referenties verwijderen.

## <a name="audit-report-retention"></a>Audit rapport bewaren
Gebeurtenissen in het controleverslag Azure AD worden voor 180 dagen bewaard. Zie voor meer informatie over de inhoudingen op rapporten, [Azure Active Directory rapport bewaarbeleid](active-directory-reporting-retention.md).

Voor klanten die willen hun controlegebeurtenissen langer bewaren op te slaan, kan de API voor het melden van gebeurtenissen controleren in een afzonderlijk gegevensarchief om regelmatig worden gebruikt. Zie [Aan de slag met de rapportage-API](active-directory-reporting-api-getting-started.md) voor meer informatie.

## <a name="properties-included-with-each-audit-event"></a>Eigenschappen die worden meegeleverd bij elke gebeurtenis

Eigenschap      | Beschrijving
------------- | --------------------------------------------------------------
Datum en tijd | De datum en tijd waarop de gebeurtenis is opgetreden
Acteur         | De gebruiker of service principal die de actie wordt uitgevoerd
Actie        | De actie die is uitgevoerd
Doel        | De gebruiker of service principal die de actie is uitgevoerd op


## <a name="update-user-attributes"></a>Kenmerken "Gebruiker bijwerken"
De "Update" audit Gebruikersgebeurtenis bevat aanvullende informatie over de kenmerken van de gebruiker zijn bijgewerkt. Voor elk kenmerk, zijn zowel de vorige waarde als de nieuwe waarde wordt opgenomen.

Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | De gebruiker kan zich aanmelden.
AssignedLicense                     | Alle licenties die zijn toegewezen aan de gebruiker.
AssignedPlan                        | Service plannen ten gevolge van de licenties die zijn toegewezen aan de gebruiker.
LicenseAssignmentDetail             | Gegevens over de licenties die zijn toegewezen aan de gebruiker. Bijvoorbeeld, als groep gebaseerde licentieverlening is betrokken, geldt dit voor de groep die de licentie verleend.
Mobile                              | GSM van de gebruiker.
OtherMail                           | Alternatief e-mailadres van de gebruiker.
OtherMobile                         | Andere mobiele telefoon van de gebruiker.
StrongAuthenticationMethod          | Een lijst met verificatiemethoden die worden ingesteld door de gebruiker voor de verificatie van meerdere factoren, zoals de bel, SMS of verificatie via programmacode vanuit een mobiele app.
StrongAuthenticationRequirement     | Meerledige verificatie wordt afgedwongen, ingeschakeld of uitgeschakeld voor deze gebruiker.
StrongAuthenticationUserDetails     | Nummer van de gebruiker, alternatieve telefoonnummers en e-mailadres gebruikt voor verificatie van meerledige verificatie en het wachtwoord opnieuw instellen.
StrongAuthenticationPhoneAppDetail  | Details forof telefoon apps geregistreerd voor het uitvoeren van 2FA-aanmelding
TelephoneNumber                     | Telefoonnummer van de gebruiker.
AlternativeSecurityId               | Een andere beveiligings-ID voor het object.
CreationType                        |Methode voor het maken van de gebruiker (door op uitnodiging of virale).
InviteTicket                        |Lijst van tickets van de uitnodiging voor de gebruiker.
InviteReplyUrl                      |Lijst met URL's te beantwoorden na instemming van de uitnodiging.
InviteResources                     |Lijst met bronnen die de gebruiker heeft uitgenodigd.
LastDirSyncTime                     |Laatste keer dat het object is bijgewerkt vanwege het synchroniseren van de gezaghebbende directory (klant, lokaal).
MSExchRemoteRecipientType           |Toewijzingen aan MBO geadresseerde typen. Verwijzen naar [MBO geadresseerde typen] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx voor typen geadresseerden
PreferredDataLocation               |De aanbevolen locatie voor de gebruiker, groep, van contact, openbare map of de gegevens van het apparaat.
ProxyAddresses                      |Het adres waarmee een object van Exchange Server-geadresseerden in een extern e-mailsysteem wordt herkend.
StsRefreshTokensValidFrom           |Uitvoert token Intrekkingsgegevens vernieuwen - een STS vernieuwen tokens verleend voordat deze tijd worden beschouwd als verlopen.
UserPrincipalName                   |De UPN die een Internet-stijl aanmeldingsnaam voor een gebruiker.
UserState                           |De status van de gebruiker PendingApproval/PendingAcceptance/geaccepteerd/PendingVerification.
UserStateChangedOn                  |De tijdstempel van de laatste wijziging van UserState. Gebruikt voor het starten van de levenscyclus van werkstromen.
UserType                            |Type gebruiker. Lid (0), Gast (1), Viral(2).

## <a name="update-group-attributes"></a>"Update" kenmerken
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Classificatie            | De indeling voor een centrale groep (HBI, MBI, enz).
Beschrijving               | Beschrijvende leesbare zinnen over het object.  
DisplayName               |De weergavenaam voor een object
DirSyncEnabled            |Geeft aan of synchronisatie van een gezaghebbende plaatsvindt directory (klant, lokaal).
GroupLicenseAssignment    |Toewijzing van de licentie van een groep
GroupType                 |Soort groep Unified (0)
IsMembershipRuleLocked    |Geeft aan dat de eigenschap MembershipRule wordt ingesteld door de groep zelf management-service kan niet worden gewijzigd door gebruikers. Alleen van toepassing op groepen waarbij GroupType GroupType.DynamicMembership bevat
IsPublic                  |Vlag om aan te geven als de groep openbare en persoonlijke.
LastDirSyncTime           |Laatste keer dat het object is bijgewerkt door het synchroniseren van de gezaghebbende directory (op lokale klant).
E-mail                      |Primaire e-mailadres
MailEnabled               |Geeft aan of deze groep e-mogelijkheden.
MailNickname              |Bijnaam voor een adresboek-object, meestal het gedeelte van de voorgaande e-mailnaam de "@" symbool.   
MembershipRule            |Een tekenreeks die aan de criteria die door de groep zelf management-service gebruikt om te bepalen welke leden aan deze groep moeten behoren. Zie ook IsMembershipRuleLocked. Alleen van toepassing op groepen waarbij GroupType GroupType.DynamicMembership bevat.
MembershipRuleProcessingState| Een enum-waarde die wordt gedefinieerd door de groep zelf management-service de status van het lidmaatschap voor deze groep te definiëren. Alleen van toepassing op groepen waarbij GroupType GroupType.DynamicMembership bevat.
ProxyAddresses            |Het adres waarmee een object van Exchange Server-geadresseerden in een extern e-mailsysteem wordt herkend.
RenewedDateTime           |De record van de tijdstempel van wanneer een groep voor het laatst is vernieuwd.   
SecurityEnabled           |Geeft aan of het lidmaatschap van de groep autorisatiebeslissingen kan beïnvloeden.
WellKnownObject           |Een Active directory-object, het aanwijzen als een van de vooraf gedefinieerde labels.

## <a name="update-device-attributes"></a>Kenmerken "Apparaat bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Geeft aan of een beveiligings-principal kan worden geverifieerd.
CloudAccountEnabled | Geeft aan of een beveiligings-principal kan worden geverifieerd. Geschreven door InTune wanneer het apparaat is gemaakt op de lokale.
CloudDeviceOSType | Het apparaat op basis van het besturingssysteem zoals Windows RT, iOS. Als de waarde van een cloud-service (zoals Intune), wordt dit kenmerk gemachtigd in Active directory voor DeviceOSType.
CloudDeviceOSVersion | De versie van het besturingssysteem. Als de waarde van een cloud-service (zoals Intune), wordt dit kenmerk gemachtigd in Active directory voor DeviceOSVersion.
CloudDisplayName | De waarde van het kenmerk displayName LDAP. Als de waarde van een cloud-service (zoals Intune), wordt dit kenmerk gemachtigd in Active directory voor displayName.
CloudCreated |Geeft aan of het object is gemaakt door cloud services.
CompliantUntil | Tot welke tijd apparaat compatibel beschouwd.
DeviceMetadata |Aangepaste metagegevens voor het apparaat
DeviceObjectVersion | Dit kenmerk wordt gebruikt ter identificatie van de versie van het apparaat.
DeviceOSType |Het apparaat op basis van het besturingssysteem zoals Windows RT, iOS. Geschreven door de registratieservice en bestemd om te worden bijgewerkt door de MDM licht service management of STS management-service.
DeviceOSVersion |De versie van het besturingssysteem. Geschreven door de registratieservice en bestemd om te worden bijgewerkt door de MDM licht service management of STS management-service.
DevicePhysicalIds |Kenmerk met meerdere waarden kunnen worden opgeslagen-id's van het fysieke apparaat. Dit kan ook BIOS-id's, TPM-vingerafdrukken, hardware-specifieke id's, enz.
DirSyncEnabled | Geeft aan of de synchronisatie van een gezaghebbende (klant, op de vooronderstelling plaatsvindt) directory.
DisplayName |De weergavenaam voor een object
IsCompliant | Dit kenmerk wordt gebruikt voor het beheren van het mobiele apparaat Beheerstatus van het apparaat.
IsManaged |Dit kenmerk wordt gebruikt om aan te geven van dat het apparaat wordt beheerd door een wolk van MDM.
LastDirSyncTime |Laatste keer dat het object is bijgewerkt vanwege het synchroniseren van de gezaghebbende directory (op lokale klant).

## <a name="update-device-configuration-attributes"></a>Kenmerken "Configuratie van bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Het maximum aantal dagen dat een apparaat niet-actief voordat het kan worden beschouwd als verwijderd.
RegistrationQuota   |Beleid wordt gebruikt voor het beperken van het aantal registraties van apparaat toegestaan voor één gebruiker.

## <a name="update-service-principal-configuration-attributes"></a>Kenmerken 'Update Service principal configuratie'
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Geeft aan of een beveiligings-principal kan worden geverifieerd.
AppPrincipalId | Externe, door de toepassing gedefinieerde identiteit voor een beveiligings-principal.
DisplayName |De weergavenaam voor een object
ServicePrincipalName    | Een service principal name "naam/instantie" naam bepaalt de waarde van de klasse toepassing waarbij instantie bevat ten minste met hostnaam [: poort] of "naam" die Hiermee geeft u een id voor de service principal.

## <a name="update-app-attributes"></a>Kenmerken 'App Update'
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |De set adressen (URL's omleiden) die zijn toegewezen aan een service principal.
Toepassings-id                               |Toepassings-ID van de App.   
AppIdentifierUri | Toepassing URI, die de toepassing wordt geïdentificeerd.  Het is meestal de URL van de toepassing toegang.
AppLogoUrl |De url van de toepassing logo-afbeelding opgeslagen in een CDN.
AvailableToOtherTenants | Waar de toepassing is met meerdere huurder (d.w.z. kunnen worden gebruikt door andere huurders).
DisplayName | De weergegeven naam voor de naam van een toepassing
Recht |Lijst van de rechten van toepassing.
ExternalUserAccountDelegationsAllowed |Vlag die aangeeft of toepassing van de resource een vertrouwde is en delegatie-vermeldingen voor externe gebruikersaccounts kunt maken.
GroupMembershipClaims |Het lidmaatschap van claims Groepsbeleid.
PublicClient | De waarde True als de client kan geen geheim bewaren (dat wil zeggen niet-vertrouwelijke client in OAuth2.0)
RecordConsentConditions | Voorwaarden van de toestemming, zoals gedefinieerd door de contractvoorwaarden: geen (0), SilentConsentForPartnerManagedApp(1). Deze waarde zal worden blootgesteld in het schema Graph API en kan alleen worden ingesteld of gewijzigd door beheerders van de huurder.
RequiredResourceAccess |XML-inhoud van een waarde van de eigenschap RequiredResourceAccess.
WebApp |Als de waarde true geeft aan dat deze toepassing een web app.
WwwHomepage |De primaire pagina.

## <a name="update-role-attributes"></a>Kenmerken "Rol bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |De set adressen (URL's omleiden) die zijn toegewezen aan een service principal.
BelongsToFirstLoginObjectSet |Als de waarde true is, geeft u aan dat dit object toe aan de verzameling objecten vereist behoort voor aanmelding van de eerste admin van een nieuwe huurder.
Builtin |Geeft aan of de levensduur van een object is eigendom van het systeem.
Beschrijving | Beschrijvende leesbare zinnen over het object.  
DisplayName |De weergavenaam voor een object
MailNickname | Bijnaam voor een adresboek-object, meestal het gedeelte van de voorgaande e-mailnaam de "@" symbool.
RoleDisabled |Geeft aan of de rol voor de toepassing van toegangscontroles moet worden genegeerd.
RoleTemplateId | De identiteit van de rolsjabloon.
ServiceInfo |Service-specifiek inrichten informatie die kan worden gebruikt door MOAC en/of andere service-exemplaren (van de servicesoorten dezelfde of een andere).
TaskSetScopeReference |Geeft een TaskSet en een aantal Scopes die zijn gekoppeld aan een rol of RoleTemplate.
ValidationError |Informatie gepubliceerd door een federatieve service met een beschrijving van een tijdelijke, servicespecifieke fout met betrekking tot de eigenschappen of de koppeling van een beheerdersactie bij object op te lossen.
WellKnownObject |Een Active directory-object, het aanwijzen als een van de vooraf gedefinieerde labels.

## <a name="update-role-definition-attributes"></a>Kenmerken "Roldefinitie bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Verzameling van vergunning scopes die bij het toewijzen van deze RoleDefinition aan een beveiligings-principal kan worden verwezen.
DisplayName     |De weergavenaam voor een object
GrantedPermissions  |Machtigingen die zijn toegekend door dit RoleDefinition.


## <a name="update-administrative-unit-attributes"></a>Kenmerken "Administratieve eenheid bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Beschrijving |   Deze eigenschap wordt bijgewerkt wanneer u de beschrijving van een administratieve eenheid wijzigen.
DisplayName |   Deze eigenschap wordt bijgewerkt wanneer u de naam van een administratieve eenheid wijzigen.

## <a name="update-company-attributes"></a>Kenmerken van de "Update-bedrijf"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Een locatie waar gebruikers van het bedrijf kunnen worden ingericht.
AuthorizedServiceInstance   | De namen van exemplaren van de service waarmee een plan kan worden ingezet.
DirSyncEnabled |Geeft aan of de synchronisatie van een gezaghebbende (klant, op de vooronderstelling plaatsvindt) directory.
DirSyncStatus |Geeft aan of synchronisatie van adresboek objecten in deze context van de huurder van een gezaghebbende (klant, op de vooronderstelling plaatsvindt) directory. een uitbreiding van de eigenschap DirSyncEnabled van objecten bedrijf.
DirSyncFeatures |Vlag bit set dirsync ingeschakelde en uitgeschakelde functies voor de huurder bij te houden.
DirectoryFeatures |Functies voor directory ingeschakeld/uitgeschakeld.
DirSyncConfiguration |Bevat alle DirSync-configuratie die specifiek zijn voor de huidige huurder.
DisplayName |De weergavenaam voor een object
IsMnc |Een Boole-vlag is ingesteld op 'true' iff die het bedrijf voor de multinationale onderneming-functie is ingeschakeld.
ObjectSettings | Een verzameling instellingen die van toepassing is op het bereik van het object.
PartnerCommerceUrl |URL van de Partner-commercesite.
PartnerHelpUrl |URL naar help-site van de Partner.
PartnerSupportEmail | De URL van de Partner ondersteuning voor e-mailberichten.
PartnerSupportTelephone |De URL van de Partner support-telefoonnummer.
PartnerSupportUrl |URL naar de ondersteuningswebsite van de Partner.
StrongAuthenticationDetails |Informatie over StrongAuthentication.
StrongAuthenticationPolicy |Beleid voor sterke verificatie voor het bedrijf.
TechnicalNotificationMail |E-mailadres op de hoogte stellen van technische problemen met betrekking tot een bedrijf.
TelephoneNumber |Telefoonnummers die voldoen aan de ITU-aanbeveling E.123.
TenantType |Het type van een huurder. Als deze waarde niet is opgegeven, is de huurder een bedrijf. Anders wordt de mogelijke waarden zijn: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Een set van DNS-domeinnamen afhankelijk is van een bedrijf.

## <a name="update-domain-attributes"></a>Kenmerken "Domein bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Mogelijkheden    |Bitvlaggen met een beschrijving van de mogelijkheden van het domein, indien van toepassing.
Standaard |Geeft aan of het domein de standaardwaarde is. bijvoorbeeld, de UserPrincipalName standaardachtervoegsel wanneer een beheerder een nieuwe gebruiker in MOAC maakt.
Eerste |Geeft aan of het domein het eerste domein voor het bedrijf, die zijn toegewezen door de OCP. Het eerste domein is een unieke subdomein van een domein van Microsoft Online. e.g.contoso3.microsoftonline.com.
LiveType    |Het type van de bijbehorende Windows Live naamruimte, indien van toepassing.
Naam    | De id voor het eindpunt.
PasswordNotificationWindowDays  |Het aantal dagen voordat de gebruiker een wachtwoord is verlopen, wordt medegedeeld.
PasswordValidityPeriodDays  | Het aantal dagen dat een wachtwoord voordat het goed is, moet worden gewijzigd.

Audit records zijn een vereiste voor veel regelgeving. Voor klanten met behulp van het rapport in de Audit Azure Active Directory om te voldoen aan de regelgeving, is het raadzaam dat de klant een exemplaar van dit help-onderwerp met de kopie van de geëxporteerde controleverslag van de klant om uit te leggen van de details van het rapport verzenden. Als de commissaris willen graag weten van de regelgeving die op dat moment voldoet aan de Azure, rechtstreeks de beoordelaar naar de [pagina naleven](https://azure.microsoft.com/support/trust-center/compliance/) van het Microsoft Azure Trust Center.
