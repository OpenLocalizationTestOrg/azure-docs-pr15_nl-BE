<properties
    pageTitle="Op rollen gebaseerde toegangscontrole (RBAC) met Azure PowerShell beheren | Microsoft Azure"
    description="Het beheren van RBAC met Azure PowerShell, met inbegrip van de aanbieding van rollen, rollen toewijzen en verwijderen van roltoewijzingen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Role-Based Access Control met Azure PowerShell beheren

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)


Kunt u op rollen gebaseerde toegangscontrole RBAC () in de portal Azure en Azure Resource Management API toegang aan uw abonnement op een toegesneden niveau beheren. Met deze functie kunt u sommige rollen toewijzen aan deze op een bepaald bereik verlenen toegang voor gebruikers, groepen of service beveiligings-principals van Active Directory.

Voordat u PowerShell RBAC beheren kunt, hebt u het volgende:

- Azure PowerShell versie 0.8.8 of hoger. Zie de meest recente versie te installeren en te koppelen aan uw abonnement Azure, [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

- Azure Resource Manager-cmdlets. Installeer de [Azure Resource Manager-cmdlets](https://msdn.microsoft.com/library/mt125356.aspx) in PowerShell.

## <a name="list-roles"></a>Lijst met rollen

### <a name="list-all-available-roles"></a>Lijst van alle beschikbare rollen
Lijst RBAC-rollen die beschikbaar zijn voor toewijzing en gebruiken om te controleren de activiteiten die zij toegang verlenen `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Screenshot van RBAC PowerShell-Get AzureRmRoleDefinition-](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Met Lijstacties van een rol
U kunt de acties voor een specifieke rol te gebruiken `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Screenshot van RBAC PowerShell-Get AzureRmRoleDefinition voor een specifieke rol-](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Zien wie toegang heeft
Gebruiken om toewijzingen van RBAC toegang te geven `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Lijst met functietoewijzingen in een bepaald bereik
Hier ziet u alle toewijzingen toegang voor een opgegeven abonnement, resource of resourcegroep. Gebruik bijvoorbeeld een overzicht van alle actieve toewijzingen voor een resourcegroep, `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment voor een resourcegroep - screenshot](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Lijst met rollen die zijn toegewezen aan een gebruiker
Als u alle rollen die zijn toegewezen aan een specifieke gebruiker en de rollen die zijn toegewezen aan de groepen waartoe de gebruiker behoort, gebruikt u `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment voor een gebruiker - screenshot](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Lijst met functietoewijzingen klassieke service beheerder en coadmin
Aan de lijst toegang toewijzingen voor de klassieke abonnement beheerder en coadministrators, gebruik:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Toegang verlenen
### <a name="search-for-object-ids"></a>Zoeken naar object-id 's
Een rol toewijzen, moet u zowel het object (gebruiker, groep of toepassing) en het bereik bepalen.

Als u de abonnement-ID niet weet, kunt u deze vinden in het blad **abonnementen** op de Azure portal. Zie als u wilt weten hoe u de query voor de abonnement-ID, [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) op MSDN.

Als u de object-ID voor een groep AD Azure, gebruikt:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Als u de object-ID voor een principal van AD Azure service of toepassing, gebruikt u:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Een rol toewijzen aan een toepassing voor het bereik van het abonnement
Om toegang te verlenen aan een toepassing voor het bereik van het abonnement, gebruikt u:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Screenshot van RBAC PowerShell-nieuwe AzureRmRoleAssignment](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Een rol aan een gebruiker op het groepsbereik resource toewijzen
Om toegang te verlenen aan een gebruiker op het groepsbereik resource, gebruikt u:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Screenshot van RBAC PowerShell-nieuwe AzureRmRoleAssignment](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Een rol toewijzen aan een groep binnen het bereik van de bron
Om toegang te verlenen aan een groep binnen het bereik van de resource, gebruikt u:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Screenshot van RBAC PowerShell-nieuwe AzureRmRoleAssignment](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Toegang verwijderen
Toegang voor gebruikers, groepen en toepassingen verwijderen:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Screenshot van RBAC PowerShell-software AzureRmRoleAssignment-](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Aangepaste rollen maken
U kunt een aangepaste rol maken de `New-AzureRmRoleDefinition` opdracht.

Als u een aangepaste rol met PowerShell maakt, moet u beginnen met een van de [ingebouwde functies](role-based-access-built-in-roles.md). De kenmerken van de *Acties*, *notActions*of *bereiken* die u wilt toevoegen als u wilt bewerken en de wijzigingen op te slaan als een nieuwe rol.

In het volgende voorbeeld wordt gestart met de rol van *Inzender voor virtuele Machine* , en die wordt gebruikt voor het maken van een aangepaste rol met de naam van de *Virtuele Machine-Operator*. De nieuwe rol toegang verleent tot alle leesbewerkingen van providers van *Microsoft.Compute*, *Microsoft.Storage*en *Microsoft.Network* -bron toegang verleent tot het starten, opnieuw opstarten en virtuele machines te controleren. De aangepaste functie kan worden gebruikt in twee abonnementen.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![Screenshot van RBAC PowerShell-Get AzureRmRoleDefinition-](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Een aangepaste rol te wijzigen
Als u wilt wijzigen in een aangepaste rol, eerst met behulp van de `Get-AzureRmRoleDefinition` opdracht voor het ophalen van de roldefinitie. Ten tweede, breng de gewenste wijzigingen aan de roldefinitie. Ten slotte, de `Set-AzureRmRoleDefinition` opdracht voor het opslaan van de gewijzigde roldefinitie.

In het volgende voorbeeld wordt de `Microsoft.Insights/diagnosticSettings/*` bewerking aan de aangepaste functie *Virtual Machine-Operator* .

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Screenshot van RBAC PowerShell-Set AzureRmRoleDefinition-](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Het volgende voorbeeld wordt een Azure-abonnement op de toegewezen bereiken van de aangepaste functie *Virtual Machine-Operator* .

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![Screenshot van RBAC PowerShell-Set AzureRmRoleDefinition-](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Als u een aangepaste rol wilt verwijderen, gebruikt u de `Remove-AzureRmRoleDefinition` opdracht.

In het volgende voorbeeld wordt de aangepaste functie *Virtual Machine-Operator* verwijderd.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Screenshot van RBAC PowerShell-software AzureRmRoleDefinition-](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Lijst aangepaste rollen
Als u de functies die beschikbaar voor toewijzing aan een bereik zijn, gebruikt u de `Get-AzureRmRoleDefinition` opdracht.

In het volgende voorbeeld worden alle functies die beschikbaar voor de toewijzing in het geselecteerde abonnement zijn.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Screenshot van RBAC PowerShell-Get AzureRmRoleDefinition-](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

In het volgende voorbeeld wordt de aangepaste functie *Virtual Machine-Operator* is niet beschikbaar in het *Production4* abonnement omdat het abonnement niet in de **AssignableScopes** van de rol.

![Screenshot van RBAC PowerShell-Get AzureRmRoleDefinition-](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Zie ook
- [Met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
