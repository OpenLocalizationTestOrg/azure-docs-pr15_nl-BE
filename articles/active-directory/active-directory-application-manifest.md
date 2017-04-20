<properties
   pageTitle="Inzicht in het toepassingsmanifest Azure Active Directory | Microsoft Azure"
   description="Gedetailleerde dekking van het toepassingsmanifest Azure Active Directory die staat voor de configuratie van een toepassing identiteit in een huurder Azure AD en zorgt voor OAuth vergunning, toestemming ervaring en meer."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Wat is het toepassingsmanifest Azure Active Directory?

Toepassingen die met Azure Active Directory (AD integreren) moeten worden geregistreerd met een huurder Azure AD, de configuratie van een permanente identiteit voor de toepassing. Deze configuratie wordt geraadpleegd tijdens runtime, scenario's die een toepassing laten uitbesteden en verificatie/machtiging via Azure AD broker inschakelen. Zie de [toevoegen, bijwerken, en een toepassing verwijderen] voor meer informatie over het model van de toepassing Azure AD[ ADD-UPD-RMV-APP] artikel.

## <a name="updating-an-applications-identity-configuration"></a>Configuratie van de identiteit van een toepassing bijwerken

Er zijn feitelijk meerdere opties beschikbaar voor het bijwerken van de eigenschappen voor configuratie van de identiteit van een toepassing, die in mogelijkheden en graden van problemen verschillen, waaronder de volgende:

- De ** [Azure klassieke portal] [ AZURE-CLASSIC-PORTAL] gebruikersinterface Web** kunt u de meest gebruikelijke eigenschappen van een toepassing bijwerken. Dit is de snelste en minste fout gevoelig manier bijwerken van de eigenschappen van de toepassing, maar geeft u volledige toegang tot alle eigenschappen, zoals de volgende twee methoden.
- Voor meer geavanceerde scenario's waarin u moet de eigenschappen die niet beschikbaar zijn in de klassieke Azure portal bijwerken, kunt u het **manifest van de toepassing**. Dit is de focus van dit artikel en in detail starten in het volgende gedeelte wordt beschreven.
- Het is ook mogelijk om **een toepassing schrijven die met de [Graph API] [ GRAPH-API] ** voor het bijwerken van de toepassing waarvoor u de meeste inspanning. Dit kan een aantrekkelijke optie zijn al, als u software voor schrijven of moet toepassingseigenschappen op geautomatiseerde wijze regelmatig bijwerken.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Met behulp van het manifest van de toepassing voor het bijwerken van de configuratie van de identiteit van een toepassing
Via de [portal voor Azure klassieke][AZURE-CLASSIC-PORTAL], kunt u de configuratie van de identiteit van uw toepassing, beheren door te downloaden en uploaden van een JSON representatie, die een manifest van de toepassing wordt aangeroepen bestand. Er is geen werkelijke bestand is opgeslagen in de directory. Manifest van de toepassing is slechts een HTTP GET-bewerking op de entiteit Azure AD Graph API-toepassing en het uploaden is een PATCH van de HTTP-bewerking op de entiteit voor de toepassing.

Als gevolg hiervan voordat u de opmaak en de eigenschappen van het manifest van de toepassing, moet u de Graph API [Toepassingsentiteit] verwijst naar[ APPLICATION-ENTITY] documentatie. Voorbeelden van updates die kunnen worden uitgevoerd, hoewel het manifest van de toepassing uploaden zijn:

- **Declare machtiging scopes (oauth2Permissions)** die door uw web-API. Zie het onderwerp 'Bloot Web-API's aan andere toepassingen' in [Toepassingen integreren met Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] voor meer informatie over het implementeren van gebruikersimitatie toestemming bereik met behulp van de oauth2Permissions overgedragen. Zoals eerder vermeld, entiteitseigenschappen worden beschreven in de Graph API [entiteit en Complex Type] [ APPLICATION-ENTITY] artikel, met inbegrip van de oauth2Permissions-eigenschap die een collectie van het type [OAuth2Permission]is[APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Declare-toepassingsrollen (appRoles) die door uw app**. De eigenschap appRoles van de entiteit van de toepassing is een collectie van het type [AppRole][APPLICATION-ENTITY-APP-ROLE]. Zie de [Role-based access control in cloud-toepassingen met behulp van Azure AD] [ RBAC-CLOUD-APPS-AZUREAD] artikel voor een voorbeeld van de implementatie.
- **Declare bekende client applications (knownClientApplications)**, waarmee u de toestemming van de opgegeven client-toepassingen naar de bron/web API logisch te koppelen.
- **Azure AD groep lidmaatschappen claimen om aanvragen** voor de ingelogd gebruiker (groupMembershipClaims).  Dit kan ook worden geconfigureerd voor het uitgeven van claims over lidmaatschap van rollen directory van de gebruiker. Zie de [vergunning in Cloud-toepassingen met behulp van groepen in AD] [ AAD-GROUPS-FOR-AUTHORIZATION] artikel voor een voorbeeld van de implementatie.
- **Verlenen van de toepassing te ondersteunen OAuth 2.0 impliciete toestaan** stromen (oauth2AllowImplicitFlow). Dit soort subsidie stroom wordt gebruikt met ingesloten JavaScript webpagina's of toepassingen van één pagina (SPA). Zie voor meer informatie over het verlenen van impliciete toestemming, [Wat de impliciete OAuth2 verlenen stroom in Azure Active Directory][IMPLICIT-GRANT].
- **Als de geheime sleutel inschakelen gebruik van X509-certificaten** (keyCredentials). Zie [service en daemon toepassingen in Office 365 bouwen] [ O365-SERVICE-DAEMON-APPS] en een [handleiding voor ontwikkelaars voor verificatie met Azure Resource Manager API] [ DEV-GUIDE-TO-AUTH-WITH-ARM] artikelen voor voorbeelden van de implementatie.
- **Een nieuwe App-ID URI toevoegen** voor uw toepassing (identifierURIs[]). App ID URI's worden gebruikt als unieke aanduiding voor een toepassing binnen de huurder Azure AD (of meerdere huurders Azure AD, voor scenario's met meerdere huurder als gekwalificeerd via gecontroleerde aangepaste domein). Ze worden gebruikt bij het aanvragen van machtigingen voor een resource, of een toegangstoken voor de toepassing van een resource te verkrijgen. Tijdens het bijwerken van dit element is dezelfde update aangebracht in de bijbehorende service van de directeur van servicePrincipalNames [] collectie, die in het huis pachter van de toepassing woont.

Manifest van de toepassing biedt ook een goede manier voor het bijhouden van de status van de registratie van uw toepassing. Omdat in JSON-indeling beschikbaar is, kan de bestandsweergave van het in uw besturingselement, samen met de broncode van uw toepassing worden gecontroleerd.

## <a name="step-by-step-example"></a>Stap door stap voorbeeld
Nu kunt stapsgewijs door de vereiste stappen voor het bijwerken van de configuratie van uw toepassing identiteit via het manifest van de toepassing. Zal selecteren we een van de voorgaande voorbeelden waarin wordt getoond hoe een nieuwe machtiging scope declareren op een resource-toepassing:

1. Ga naar de [Azure klassieke portal] [ AZURE-CLASSIC-PORTAL] en meld u aan met een account die service-beheerder of beheerder collega heeft.

2. Nadat u hebt geverifieerd, schuif naar beneden en selecteert u de extensie Azure 'Active Directory' in het linkernavigatievenster (1), klik vervolgens op geregistreerd de Azure AD huurder waarin de toepassing is (2).

    ![Selecteer de huurder Azure AD][SELECT-AZURE-AD-TENANT]

3. Wanneer de pagina directory wordt, klikt u op 'Toepassingen' (1) boven aan de pagina voor een overzicht van toepassingen in de huurder zijn geregistreerd. Zoek de toepassing die u wilt bijwerken in de lijst en klikt u op (2).

    ![Selecteer de huurder Azure AD][SELECT-AZURE-AD-APP]

4. Nu dat u de belangrijkste pagina van de toepassing hebt geselecteerd, ziet u de functie 'Manifest beheren' aan de onderkant van de pagina (1). Als u op deze koppeling klikt, wordt u gevraagd het manifestbestand JSON uploaden of downloaden. Klik op "Download Manifest" (2). Dit wordt onmiddellijk gevolgd met de bevestigingsdialoogvenster waarin u kunt bevestigen door te klikken op 'Download Manifest"(3), en vervolgens op openen of opslaan van het bestand lokaal (4).

    ![Het manifest te beheren, optie downloaden][MANAGE-MANIFEST-DOWNLOAD]

    ![Het manifest downloaden][DOWNLOAD-MANIFEST]

5. In dit voorbeeld opgeslagen we het bestand lokaal op en ons toestemming te openen in een editor, wijzigingen aanbrengen in de JSON en sla opnieuw op. Hier is wat de JSON-structuur ziet eruit als in de editor van Visual Studio JSON. Opmerking dat het schema voor de [Toepassingsentiteit volgt] [ APPLICATION-ENTITY] zoals eerder vermeld:

    ![Het manifest JSON bijwerken][UPDATE-MANIFEST]

    Bijvoorbeeld, als dat we willen implementeren/expose een nieuwe machtiging genaamd 'Employees.Read.All' op onze resource-toepassingen (API), u zou gewoon een nieuwe/tweede element toevoegen aan de collectie oauth2Permissions ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    De vermelding moet uniek zijn en daarom moet u een nieuwe Globally Unique ID (GUID) voor genereren de `"id"` eigenschap. In dit geval omdat we opgegeven `"type": "User"`, met deze machtiging kan worden ingestemd met de door elke account is geverifieerd door de Azure AD pachters in de toepassing van de resource/API is geregistreerd. Dit geeft de client toepassing machtiging ten behoeve van de account. De tekenreeksen met de beschrijving en een weergegeven naam worden gebruikt tijdens de toestemming en in de klassieke Azure portal worden weergegeven.

6. Wanneer u klaar bent met het bijwerken van het manifest, terug naar de pagina van de toepassing AD Azure in Azure klassieke portal en klik op de 'Beheren van het Manifest' functie opnieuw (1). Maar deze keer de optie "Uploaden Manifest" (2). Net als bij het downloaden, u zal worden wordt opnieuw met een tweede dialoogvenster waarin u wordt gevraagd de locatie van het bestand JSON. Klik op "Bladeren naar bestand..." (3), vervolgens het dialoogvenster 'Kies bestand te laden' om de JSON-bestand (4) te selecteren en druk op "Open". Nadat het dialoogvenster is verdwenen, schakel het selectievakje 'OK' (5) en het manifest worden geüpload.  

    ![Beheren van het manifest, uploaden, optie][MANAGE-MANIFEST-UPLOAD]

    ![Het manifest JSON uploaden][UPLOAD-MANIFEST]

    ![Upload het manifest JSON - bevestiging][UPLOAD-MANIFEST-CONFIRM]

Nu dat het manifest wordt opgeslagen, kunt u geven een geregistreerde client-toepassing toegang tot de nieuwe machtiging hebben we hierboven toegevoegd. Nu kunt u de Azure klassieke portal Web UI in plaats van de clienttoepassing manifest bewerken:  

1. Eerst gaat u naar de pagina 'Configureren' van de clienttoepassing die u wilt toegang tot de nieuwe API toevoegen en klik op 'Toepassing toevoegen'.
2. Vervolgens wordt weergegeven met de lijst met geregistreerde resource toepassingen (API's) van de huurder. Klik op de plusknop / + symbool naast de naam van de brontoepassing te selecteren.  
3. Vervolgens klikt u op het vinkje in de rechterbenedenhoek.
4. Wanneer u naar de sectie 'Toepassing toevoegen' in de pagina configuratie van de client terugkeert, ziet u de nieuwe toepassing van de resource in de lijst. Als u de sectie 'Overgedragen machtigingen' rechts van die rij aanwijst, ziet u een vervolgkeuzelijst weergegeven. Klik in de lijst en selecteer vervolgens de nieuwe machtiging om toe te voegen aan de gevraagde lijst met machtigingen van de client. Opmerking: deze nieuwe machtiging worden opgeslagen in de clienttoepassing identiteit configuratie in de eigenschap van de collectie 'requiredResourceAccess'.

![Machtigingen voor andere toepassingen][PERMS-TO-OTHER-APPS]

![Machtigingen voor andere toepassingen][PERMS-SELECT-APP]

![Machtigingen voor andere toepassingen][PERMS-SELECT-PERMS]

Dat is. Nu wordt uw toepassingen uitgevoerd met behulp van de configuratie van de nieuwe identiteit.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de relatie tussen de toepassing en Service Principal objecten van een toepassing, [toepassing en service-principals in Azure AD][AAD-APP-OBJECTS].
- Zie de [verklarende woordenlijst Azure AD-ontwikkelaar] [ AAD-DEVELOPER-GLOSSARY] voor definities van de belangrijkste Azure Active Directory (AD) developer begrippen.

Gebruik de DISQUS opmerkingen hieronder om feedback te geven en help ons verfijnen en vorm van de inhoud.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

