<properties
    pageTitle="Beveilig uw sleutel kluis | Microsoft Azure"
    description="Beheer-machtigingen voor een sleutel kluis voor het beheren van kluizen en sleutels en geheimen. Model voor verificatie en machtiging voor sleutel kluis en het beveiligen van uw sleutel kluis"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Beveilig uw sleutel kluis

Azure sleutel kluis is een cloud service coderingssleutels en -geheimen voor uw cloud-toepassingen (zoals certificaten, tekenreeksen, wachtwoorden beschermt). Aangezien deze gegevens gevoelige en kritieke business, die u toegang tot uw belangrijke kluizen beveiligen wilt zodat alleen geautoriseerde toepassingen en gebruikers toegang tot belangrijke kluis. Dit artikel biedt een overzicht van belangrijke kluis toegangsmodel verificatie en autorisatie uitgelegd en wordt beschreven hoe u beveiligde toegang tot de sleutel kluis voor uw cloud-toepassingen met een voorbeeld.

## <a name="overview"></a>Overzicht

Toegang tot een sleutel kluis wordt bestuurd via twee verschillende interfaces: management plane en data-plane. Voor beide vlakken is goede verificatie en machtiging vereist voordat een beller (een gebruiker of een toepassing) toegang kan krijgen tot belangrijke kluis. Verificatie wordt de identiteit van de beller, vergunning bepaalt welke bewerkingen de beller is toegestaan om uit te voeren.

Voor verificatie gebruiken zowel management-plane en data plane Azure Active Directory. Beheervlak gebruikt voor een vergunning, op rollen gebaseerde toegangscontrole (RBAC) terwijl de gegevens vlak met sleutel kluis-beleid.

Hier volgt een kort overzicht van de onderwerpen worden besproken:

[Verificatie met behulp van Azure Active Directory](#authentication-using-azure-active-direcrory) - in deze sectie wordt uitgelegd hoe een beller wordt geverifieerd met Active Directory toegang te krijgen tot een sleutel kluis via beheer plane en data-plane Azure. 

[Management plane en data-plane](#management-plane-and-data-plane) - Management plane en data-plane zijn twee vlakken die access gebruikt voor toegang tot de sleutel kluis. Elk vlak met toegang tot ondersteunt specifieke bewerkingen. In deze sectie worden de eindpunten toegang, bewerkingen ondersteund en access control methode die wordt gebruikt door elk vlak. 

[Toegangsbeheer voor management vlak](#management-plane-access-control) - In deze sectie die zullen we toegang tot beheertaken vlak uit te voeren met behulp van toegangsbeheer op basis van rollen.

[Data access control vlak](#data-plane-access-control) - in deze sectie wordt beschreven hoe u sleutel kluis-beleid gebruiken om toegang tot gegevens vlak.

[Voorbeeld](#example) : in dit voorbeeld wordt beschreven hoe u voor het instellen van toegangsbeheer voor uw sleutel kluis om drie verschillende teams (security team, ontwikkelaars/operators en accountants) specifieke taken uitvoeren om te ontwikkelen, beheren en controleren van een toepassing in Azure.


## <a name="authentication-using-azure-active-directory"></a>Verificatie met behulp van Azure Active Directory

Wanneer u een sleutel kluis in een Azure abonnement maakt, is het automatisch gekoppeld aan de huurder voor het abonnement van Azure Active Directory. Alle bellers (gebruikers en toepassingen) moeten worden geregistreerd in deze huurder voor toegang tot deze sleutel kluis. Een toepassing of een gebruiker moet verifiëren met Azure Active Directory toegang tot sleutel kluis. Dit geldt voor zowel access management plane en data vlak. In beide gevallen wordt een toepassing toegang krijgen tot belangrijke kluis op twee manieren:

-  **gebruiker + app toegang** - meestal is dit voor toepassingen die toegang hebben tot de sleutel kluis namens een gebruiker ingelogd. Azure PowerShell, Azure Portal zijn voorbeelden van dit type toegang. Er zijn twee manieren om toegang te verlenen aan gebruikers: een manier is om toegang te verlenen aan gebruikers zodat sleutel kluis is toegankelijk vanuit elke toepassing en de andere manier is door een gebruikerstoegang verlenen tot belangrijke kluis alleen wanneer ze een bepaalde toepassing (als samengestelde-id genoemd) gebruiken. 
-  **alleen app** - voor toepassingen die daemon services worden uitgevoerd, achtergrond taken enz. De identiteit van de toepassing toegang heeft tot de sleutel kluis.

In beide typen van toepassingen de toepassing wordt geverifieerd met Azure Active Directory met behulp van een van de [ondersteunde verificatiemethoden](../active-directory/active-directory-authentication-scenarios.md) en verwerft een token. Verificatiemethode gebruikt, is afhankelijk van het toepassingstype. Vervolgens wordt de toepassing gebruikt deze token en REST API-aanvraag verzendt naar sleutel kluis. Bij een vlak beheertoegang tot worden de aanvragen gerouteerd via Azure Resource Manager eindpunt. Bij het benaderen van gegevens vlak, de lezingen toepassingen rechtstreeks naar een sleutel kluis eindpunt. Zie meer details op de [stroom van de gehele verificatie](../active-directory/active-directory-protocols-oauth-code.md). 

De naam van de resource waarvoor de toepassing vraagt om een token is afhankelijk van of de toepassing toegang heeft tot vlak management of data vlak. Naam van de resource is daarom op management vlak of gegevens vlak de eindpunten die worden beschreven in de tabel verderop, afhankelijk van de Azure-omgeving.

Met één enkel mechanisme voor verificatie bij het beheer en de gegevens vlak heeft zijn eigen voordelen:

- Organisaties kunnen toegang tot alle belangrijke kluizen in hun organisatie centraal beheren
- Als een gebruiker verlaat, verliest ze onmiddellijk toegang tot alle belangrijke kluizen in de organisatie
- Organisaties kunnen de verificatie via de opties in Azure Active Directory (bijvoorbeeld inschakelen meerledige verificatie voor extra beveiliging) aanpassen

## <a name="management-plane-and-data-plane"></a>Management plane en data-plane

Azure sleutel kluis is een Azure beschikbaar via implementatiemodel Azure Resource Manager-service. Wanneer u een sleutel kluis maakt, krijgt u een virtuele container binnen die kunt u andere objecten zoals sleutels, geheimen en certificaten. Open uw sleutel kluis management plane en data-plane met bepaalde bewerkingen uit te voeren. Beheerinterface vlak wordt gebruikt voor het beheren van uw sleutel kluis zelf, zoals het maken, verwijderen, sleutel kluis kenmerken bijwerken en -beleid voor vlak voor gegevens instellen. Gegevens vlak interface wordt gebruikt voor het toevoegen, verwijderen, wijzigen en gebruiken van de sleutels, geheimen en certificaten die zijn opgeslagen in de sleutel kluis.

De gegevens en vlak vlak beheerinterfaces via verschillende eindpunten (Zie tabel). De tweede kolom in de tabel beschrijft de DNS-namen voor deze eindpunten in verschillende omgevingen met Azure. De derde kolom Beschrijving van de bewerkingen die u vanaf elk vlak toegang uitvoeren kunt. Elk vlak access beschikt ook over een eigen mechanisme voor toegangsbeheer: voor toegangsbeheer voor management vlak is ingesteld met behulp van Azure Resource Manager Role-Based Access Control RBAC (), terwijl voor toegangsbeheer voor gegevens vlak met sleutel kluis-beleid is ingesteld.

| Access-vlak | Access-eindpunten | Bewerkingen | Mechanisme voor toegangsbeheer|
|--------------|------------------|--------------------|--------|
| Beheervlak|**Globale:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure Amerikaanse overheid:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Duitsland:**<br> Management.microsoftazure.de:443 | Sleutel kluis maken/lezen/bijwerken/verwijderen <br> Toegangsbeleid voor kluis met sleutel instellen<br>Set codes voor sleutel kluis | Azure Resource Manager Role-Based Access Control RBAC) |
| Gegevens vlak | **Globale:**<br> &lt;Vault-naam&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;Vault-naam&gt;. vault.azure.cn:443<br><br> **Azure Amerikaanse overheid:**<br> &lt;Vault-naam&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;Vault-naam&gt;. vault.microsoftazure.de:443 | Voor sleutels: Decoderen, coderen, UnwrapKey, WrapKey, verifiëren, ondertekenen, Get, lijst, bijwerken, maken, importeren, verwijderen, back-up, terugzetten<br><br> Voor geheimen: U, lijst, reeks, verwijderen | Sleutel kluis-beleid|

De management plane en data vlak access besturingselementen werken onafhankelijk van elkaar. Bijvoorbeeld, als u toegang verlenen een toepassing wilt voor het gebruik van sleutels in een kluis met sleutel, alleen moet u toegang geeft tot gegevens vlak met een sleutel kluis-beleid en geen management vlak nodig is voor deze toepassing. En omgekeerd, als u wilt dat een gebruiker kan tags en eigenschappen van de kluis lezen, maar niet beschikken over alle toetsen, geheimen of certificaten, kunt u deze gebruiker, geven 'lezen' toegang met behulp van RBAC en geen toegang tot gegevens vlak is vereist.

## <a name="management-plane-access-control"></a>Beheer vlak toegangsbeheer

Het beheervlak bestaat uit bewerkingen die betrekking hebben op de sleutel kluis zelf. U kunt bijvoorbeeld, maken of verwijderen van een sleutel kluis. U kunt een lijst met kluizen in een abonnement. U kunt ophalen van de sleutel kluis eigenschappen (zoals SKU, codes) en stelt sleutel kluis-beleid waarmee de gebruikers en toepassingen die toegang hebben tot sleutels en geheimen in de sleutel kluis. Beheer vlak wordt gebruikgemaakt van RBAC. Zie de volledige lijst met sleutel kluis bewerkingen die kunnen worden uitgevoerd via beheervlak in de tabel in de voorgaande sectie. 

### <a name="role-based-access-control-rbac"></a>Role-based Access Control RBAC)
Elk abonnement Azure heeft een Azure Active Directory. Gebruikers, groepen en toepassingen van deze directory kunnen toegang worden verleend voor het beheer van bronnen in het abonnement Azure het implementatiemodel Azure Resource Manager gebruiken. Dit type toegangsbeheer op rollen gebaseerde toegangscontrole RBAC () genoemd. Om deze toegang te beheren, kunt u de [Azure portal](https://portal.azure.com/), [Extra CLI Azure](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)of de [Azure Resource Manager REST API's](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Met het model Azure Resource Manager maakt u uw sleutel kluis in een groep en beheer van toegang tot bronnen op het beheervlak van deze sleutel kluis met Azure Active Directory. U kunt bijvoorbeeld gebruikers of een groep kan beheren sleutel kluizen in een bepaalde resourcegroep verlenen.

Juiste RBAC-rollen toewijzen kunt u toegang verlenen aan gebruikers, groepen en toepassingen op een specifieke scope. Om toegang te verlenen aan een gebruiker voor het beheren van sleutels kluizen kunt u een vooraf gedefinieerde rol 'sleutel kluis Inzender' wilt toewijzen aan deze gebruiker aan een specifieke scope. Het bereik zou in dat geval een abonnement, een groep of alleen een bepaalde sleutel kluis. Een rol die wordt toegewezen aan het abonnement is van toepassing op alle bronnengroepen en bronnen in het abonnement. Een rol is toegewezen op het niveau van de resource van toepassing is op alle bronnen in de resourcegroep. Een rol die wordt toegewezen voor een specifieke resource geldt alleen voor die resource. Er zijn diverse vooraf gedefinieerde rollen (Zie [RBAC: ingebouwde functies](../active-directory/role-based-access-built-in-roles.md)), en als de vooraf gedefinieerde rollen niet aan uw behoeften aanpassen kunt u ook uw eigen rollen definiëren.

>[AZURE.IMPORTANT] Houd er rekening mee dat als een gebruiker machtigingen (RBAC) op een sleutel kluis management vlak, ze kan zichzelf geven Inzender toegang heeft gegevens vlak, door het instellen van sleutel kluis-beleid bepaalt de toegang tot gegevens vlak. Het verdient daarom precies bepalen wie 'Inzender' toegang tot uw sleutel kluizen heeft zodat alleen geautoriseerde personen toegang tot en beheer uw sleutel kluizen, sleutels, geheimen en certificaten.

## <a name="data-plane-access-control"></a>Gegevens vlak toegangsbeheer

De sleutel kluis gegevens vlak bestaat uit bewerkingen die van invloed zijn op de objecten in een sleutel kluis, zoals sleutels, geheimen en certificaten.  Dit omvat de sleutel bewerkingen zoals maken, importeren, bijwerken, lijst, back-up en terugzetten, cryptografische sleutels bewerkingen als ondertekenen, controleren, coderen, decoderen, laten lopen, en pak en tags en andere kenmerken voor de toetsen instellen. Op dezelfde manier voor geheimen bevat, ophalen, instellen, lijst, verwijderen.

Gegevens vlak toegang wordt verleend door-beleid voor een sleutel kluis. Een gebruiker, groep of een toepassing moet gemachtigd Inzender (RBAC) voor beheervlak voor een sleutel kluis te kunnen instellen-beleid voor die sleutel kluis. Een gebruiker, groep of toepassing kan toegang worden verleend voor het uitvoeren van specifieke bewerkingen voor sleutels of geheimen in een sleutel kluis. sleutel kluis ondersteunen maximaal 16 access policy posten voor een sleutel kluis. Maak een Azure Active Directory-beveiligingsgroep en Voeg gebruikers toe aan deze groep gegevens vlak om toegang te verlenen aan meerdere gebruikers op een sleutel kluis.

### <a name="key-vault-access-policies"></a>sleutel kluis-beleid

sleutel kluis-beleid machtigen om certificaten en sleutels, geheimen afzonderlijk. U kunt bijvoorbeeld een gebruikerstoegang tot alleen de sleutels, maar geen machtigingen voor de geheimen geven. Machtigingen voor toegang tot de sleutels of geheimen of certificaten zijn echter op het niveau van de kluis. Met andere woorden, ondersteunt sleutel kluis-beleid geen objectmachtigingen op gebruikersniveau. U kunt [Azure portal](https://portal.azure.com/), [Extra CLI Azure](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)of de [sleutel kluis REST API's](https://msdn.microsoft.com/library/azure/mt620024.aspx) instellen-beleid voor een sleutel kluis.

>[AZURE.IMPORTANT] Houd er rekening mee dat belangrijke vault-beleid van toepassing zijn op het niveau van de kluis. Bijvoorbeeld wanneer een gebruiker is gemachtigd voor het maken en verwijderen van sleutels, zij kan die bewerkingen uitvoeren op alle sleutels in deze sleutel kluis.

## <a name="example"></a>Voorbeeld

Stel dat u ontwikkelt een toepassing die gebruikmaakt van een certificaat voor SSL, Azure opslagruimte voor het opslaan van gegevens en een RSA 2048-bits sleutel gebruikt voor bewerkingen van teken. Stel dat deze toepassing wordt uitgevoerd in een VM (of een VM schaal ingesteld). U kunt een sleutel kluis gebruiken voor het opslaan van alle geheimen van de toepassing en de sleutel kluis gebruiken voor het opslaan van het bootstrap-certificaat dat door de toepassing wordt gebruikt voor verificatie met Azure Active Directory.

Dus volgt hier een overzicht van alle sleutels en geheimen worden opgeslagen in een kluis met sleutel.

- **SSL-Cert** - gebruikt voor SSL
- **Storage Key** - die wordt gebruikt voor toegang tot opslag account
- **RSA 2048-bits sleutel** - gebruikt voor het aanmelden
- **Certificaat bootstrap** - gebruikt voor verificatie met Azure Active Directory voor toegang tot de sleutel kluis ophalen van de sleutel voor de opslag en RSA-sleutel gebruiken voor ondertekening.

Nu gaan we voldoen aan de mensen die het beheren, implementeren en controleren van deze toepassing. In dit voorbeeld gebruiken we drie rollen.

- **Security team** - dit zijn meestal "kantoor van de BBF (Chief Security Officer)" IT-personeel of equivalent, die verantwoordelijk is voor de goede bewaring van geheimen zoals SSL-certificaten voor het ondertekenen van tekenreeksen voor verbindingen voor databases, opslag account sleutels gebruikt RSA-sleutels.
- **Ontwikkelaars/operators** - dit zijn de mensen die deze toepassing ontwikkelt en implementeert u deze in Azure. Meestal ze geen deel uitmaken van het team veiligheid en dus ze hoeven geen toegang tot gevoelige gegevens, zoals SSL-certificaten, RSA-sleutels, maar de toepassing die u moet toegang hebben tot de.
- **Accountants** - dit is meestal een andere groep van mensen, geïsoleerd is van de algemene IT-medewerkers en ontwikkelaars. Hun verantwoordelijkheid is correct gebruik en onderhoud van certificaten, sleutels, enz., en ervoor zorgen dat aan de veiligheidsnormen van de gegevens. 

Er is één meer functie die buiten het toepassingsgebied van deze toepassing maar relevante hier te vermelden en dat het abonnement (of groep) is de beheerder. Abonnement beheerder stelt u de initiële toegangsmachtigingen voor het security team. Hier nemen we aan dat de abonnement-beheerder heeft toegang tot het security team aan een resourcegroep in welke alle bronnen die nodig zijn voor deze toepassing wonen.

Nu laten we zien welke acties elke functie worden uitgevoerd in de context van deze toepassing.

- **Security team**
    - Sleutel kluizen maken
    - Hiermee schakelt u op de sleutel kluis logboekregistratie
    - Toetsen/geheimen toevoegen
    - Maak een back-up van sleutels voor noodherstel
    - Sleutel kluis-beleid om machtigingen te verlenen aan gebruikers en toepassingen voor het uitvoeren van specifieke bewerkingen instellen
    - Vorig periodiek toetsen/geheimen
- **Ontwikkelaars/operators**
    - Verwijzingen naar de bootstrap en SSL-certificaten (vingerafdrukken), opslag sleutel (geheime URI) en de ondertekening van de sleutel (Key URI) van security team
    - Ontwikkelen en implementeren van de toepassing die via programmacode toegang krijgt sleutels en geheimen tot
- **Accountants**
    - Gebruikslogboeken bekijken sleutel/geheim gebruikt en wat de naleving van de veiligheidsnormen gegevens bevestigen

Nu laten we zien welke toegangsmachtigingen tot sleutel kluis nodig zijn voor elke rol (en de toepassing) hun toegewezen taken uit te voeren. 

| Rol van de gebruiker    | Beheermachtigingen voor vlak | Machtigingen voor gegevens vlak |
|--------------|------------------------------|------------------------|
|Security Team|sleutel kluis Inzender|Sleutels: back-up maken, maken, verwijderen, opvragen, importeren, lijst, herstellen <br> Geheimen: alle|
|Ontwikkelaars/Operator| sleutel kluis toestemming gedistribueerd, zodat de VMs ze implementeren geheimen uit de sleutel kluis kunnen ophalen | Geen |
|Accountants| Geen | Sleutels: lijst<br>Geheimen: lijst|
|Toepassing| Geen | Sleutels: sign<br>Geheimen: ophalen |

>[AZURE.NOTE] Accountants moeten toestemming voor sleutels en geheimen weergeven zodat u ze kunnen inspecteren kenmerken voor sleutels en geheimen die niet worden uitgestoten in de logboeken, zoals labels, activering en vervaldatums.

Naast machtiging voor sleutel kluis moeten alle drie rollen ook toegang tot andere bronnen. Om bijvoorbeeld te kunnen inzetten VMs (of Web Apps enz.) Ontwikkelaars/Operators moet ook 'Inzender' toegang tot deze brontypen. Accountants moeten leestoegang hebben tot de opslag rekening waar de sleutel kluis logboeken worden opgeslagen.

Omdat de focus van dit artikel is de beveiliging van de toegang tot de sleutel kluis, we alleen illustreren de relevante delen met betrekking tot dit onderwerp en details over de implementatie van certificaten, sleutels en geheimen openen via programmering enz overslaan. Deze gegevens zijn al elders. Certificaten die zijn opgeslagen in de sleutel kluis aan VMs implementeren in een [blog posten](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)wordt gedekt en [voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=45343) beschikbaar die hoe bootstrap certificaat gebruiken illustreert voor verificatie bij Azure AD voor toegang tot de sleutel kluis.

De meeste van de toegangsmachtigingen kunnen worden toegekend met Azure portal, maar gedetailleerde machtigingen wilt geven moet u mogelijk Azure PowerShell (of Azure CLI) om het gewenste resultaat. 

De volgende PowerShell codefragmenten wordt ervan uitgegaan dat:

- De Azure Active Directory-beheerder heeft de beveiligingsgroepen die voor de drie rollen, namelijk Contoso Security Team Contoso App Devops, Contoso App Auditors gemaakt. De beheerder heeft ook gebruikers toegevoegd aan de groepen waartoe ze behoren.

- **ContosoAppRG** is de bronnengroep waarin alle resources zich bevinden. **contosologstorage** is waar de logboekbestanden worden opgeslagen. 

- Sleutel kluis **ContosoKeyVault** en opslag account voor sleutel kluis logs **contosologstorage** moet zich in dezelfde locatie Azure


Eerst de beheerder van het abonnement wordt toegewezen 'sleutel kluis Inzender' en ' gebruiker toegang ' beheerdersrollen aan het security team. Hierdoor wordt het security team voor het beheren van toegang tot andere bronnen en sleutel kluizen in de resourcegroep ContosoAppRG beheren.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Het volgende script ziet u hoe het security team een sleutel kluis, logboekregistratie en machtigingen instellen voor andere functies en de toepassing kunt maken. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

De aangepaste rol is gedefinieerd, kan worden alleen toegewezen aan het abonnement waar de resourcegroep ContosoAppRG wordt gemaakt. Als dezelfde aangepaste rollen worden gebruikt voor andere projecten in andere abonnementen, kan de scope toegevoegd abonnementen hebben.

De aangepaste roltoewijzing voor de ontwikkelaars/operators voor de machtiging "implementeren/actie" is binnen het bereik van de resourcegroep. Op deze manier alleen de VMs in de resourcegroep 'ContosoAppRG' gemaakt, krijgt de geheimen (SSL-certificaat en bootstrap cert). Een VMs dat een lid van het team dev/ops in andere bronnengroepen maakt niet mogelijk om deze geheimen zelfs als zij wisten dat de geheime URI's.

In dit voorbeeld ziet u een eenvoudige scenario. Echte leven scenario's mogelijk ingewikkelder en moet u de machtigingen voor de sleutel kluis op basis van uw behoeften aanpassen. Bijvoorbeeld, in ons voorbeeld nemen we aan dat security team krijgt de sleutel en geheime verwijzingen (URI's en vingerafdrukken) dat ontwikkelaars/operators team moet verwijzen in hun toepassingen. Daarom hoeven niet ontwikkelaars/exploitanten alle gegevens vlak om toegang te verlenen. Let er ook op dat in dit voorbeeld ligt de nadruk op de beveiliging van uw sleutel kluis. Soortgelijke moet worden overwogen [de VMs](https://azure.microsoft.com/services/virtual-machines/security/), [opslag, rekeningen](../storage/storage-security-guide.md) en andere Azure bronnen te beveiligen.

>[AZURE.NOTE] Opmerking: In dit voorbeeld ziet u hoe sleutel kluis toegang worden vergrendeld in productie. De ontwikkelaars moeten hebben hun eigen abonnement of resourcegroup waar ze hebben volledige machtigingen voor het beheren van hun kluizen, VMs en opslag account waar zij de toepassing ontwikkelen.


## <a name="resources"></a>Bronnen

-   [Azure Active Directory toegang op basis van rollen beheren](../active-directory/role-based-access-control-configure.md)

    In dit artikel wordt uitgelegd en werking van het toegangsbeheer op basis van Azure Active Directory-rol.

-   [RBAC: Ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md)

    In dit artikel details beschikbaar in RBAC alle ingebouwde functies.

-   [Informatie over implementatie resourcemanager en klassieke implementatie](../resource-manager-deployment-model.md)

    In dit artikel worden de bronnenbeheerder implementatie en klassieke implementatiemodellen en uitleg over de voordelen van het gebruik van de Resource Manager en resource-groepen

-    [Role-Based Access Control met Azure PowerShell beheren](../active-directory/role-based-access-control-manage-access-powershell.md)

     In dit artikel wordt uitgelegd hoe u op rollen gebaseerde toegangscontrole met Azure PowerShell beheren

-   [Toegangsbeheer met de REST API op basis van rollen beheren](../active-directory/role-based-access-control-manage-access-rest.md)

    In dit artikel ziet u hoe de REST-API gebruiken voor het beheren van RBAC.

-   [Toegangsbeheer op basis van rollen voor Microsoft Azure van Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Dit is een koppeling naar een video op Channel 9 van de Conferentie 2015 MS Ignite. In deze sessie, ze praten over toegang tot de beheer- en rapportagefuncties in Azure en ontdek de beste praktijken om de toegang tot Azure abonnementen met Azure Active Directory beveiligen.

-   [De toegang tot webtoepassingen die via OAuth 2.0 en Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)

    Dit artikel beschrijft de complete OAuth 2.0-stroom voor het verifiëren van Azure Active Directory.

-   [sleutel kluis REST API 's](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Dit document is de referentie voor de REST API's voor het beheren van uw sleutel kluis via programmacode, met inbegrip van de sleutel kluis-beleid instellen.

-   [sleutel kluis REST API 's](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Een koppeling naar de sleutel kluis REST API-documentatie.

-   [Belangrijkste toegangsbeheer](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Een koppeling naar de sleutel access control-documentatie.

-   [Geheime toegangsbeheer](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Een koppeling naar de sleutel access control-documentatie.

-   [Instellen](https://msdn.microsoft.com/library/mt603625.aspx) en [verwijderen](https://msdn.microsoft.com/library/mt619427.aspx) sleutel kluis-beleid met PowerShell

    Koppelingen naar documentatie voor PowerShell-cmdlets voor het beheren van belangrijke vault-beleid.

## <a name="next-steps"></a>Volgende stappen

Zie [Aan de slag met Azure sleutel kluis](key-vault-get-started.md)voor een ophalen begonnen zelfstudie voor een beheerder.

Zie [Azure sleutel kluis vastleggen](key-vault-logging.md)voor meer informatie over het gebruik van logboekregistratie voor sleutel kluis.

Zie voor meer informatie over het gebruik van sleutels en geheimen met Azure sleutel kluis [over sleutels en geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Als u vragen over sleutel kluis hebt, gaat u naar de [Azure sleutel kluis Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
