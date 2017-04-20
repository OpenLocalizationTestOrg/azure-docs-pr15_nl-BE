<properties
    pageTitle="Gebruikersmachtigingen toewijzen aan specifieke lab beleid | Microsoft Azure"
    description="Informatie over het verlenen van machtigingen voor specifieke lab beleid in de DevTest Labs op basis van de behoeften van de gebruiker"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Gebruikersmachtigingen toewijzen aan specifieke lab beleid

## <a name="overview"></a>Overzicht

In dit artikel ziet u hoe gebruikersmachtigingen verlenen voor een bepaalde lab beleid met PowerShell. Op die manier kunnen machtigingen worden toegepast op basis van de behoeften van de gebruiker. U wilt een bepaalde gebruiker de mogelijkheid om te wijzigen de VM instellingen, maar niet het kostenbeleid verlenen.

## <a name="policies-as-resources"></a>Beleid als resources

Zoals besproken in het artikel [Azure op rollen gebaseerde toegangscontrole](../active-directory/role-based-access-control-configure.md) , kan RBAC geavanceerdere toegangsbeheer van middelen voor Azure. RBAC gebruikt, kunt u taken binnen uw team DevOps scheiden en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben om hun taken uitvoeren.

In de DevTest Labs is een beleid een middel waarmee de actie RBAC **Microsoft.DevTestLab/labs/policySets/policies/**. Elk beleid lab is een bron in het resourcetype en kan worden toegewezen als een bereik aan een RBAC-rol.

Bijvoorbeeld om aan te geven gebruikers lezen/schrijven toestemming aan het beleid **VM grootte toegestaan** maakt u een aangepaste rol die geschikt is voor de **Microsoft.DevTestLab/labs/policySets/policies/** * actie, en vervolgens de desbetreffende gebruikers toewijzen aan deze aangepaste rol binnen het bereik van * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Zie voor meer informatie over aangepaste RBAC-rollen, het [toegangsbeheer van aangepaste rollen](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Maken van een aangepaste rol van lab met PowerShell
Aan de slag, u hebt het volgende artikel, waarin wordt uitgelegd hoe installeer en configureer de Azure PowerShell-cmdlets lezen: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Nadat u de Azure PowerShell-cmdlets hebt ingesteld, kunt u de volgende taken uitvoeren:

- Lijst van alle bewerkingen/acties voor een resource-provider
- Met Lijstacties in een bepaalde rol:
- Aangepaste rollen maken

De volgende PowerShell script ziet u voorbeelden van hoe u deze taken uitvoert:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Het toewijzen van machtigingen aan een gebruiker voor een specifiek beleid met behulp van aangepaste rollen
Als u uw aangepaste rollen hebt gedefinieerd, kunt u deze toewijzen aan gebruikers. Een aangepaste rol aan een gebruiker toewijzen, moet u eerst de **object-id** van die gebruiker aanvragen. Hiertoe gebruikt u de cmdlet **Get-AzureRmADUser** .

In het volgende voorbeeld wordt de **object-id** van de gebruiker *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Als u de **object-id** voor de gebruiker en de naam van een aangepaste rol hebt, kunt u die rol toewijzen aan de gebruiker met de cmdlet **New-AzureRmRoleAssignment** :

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

In het vorige voorbeeld wordt het beleid **AllowedVmSizesInLab** gebruikt. U kunt een van de volgende beleidsregels:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen

Eenmaal u hebt de machtiging gebruiker aan specifieke lab-beleid, hier zijn enkele volgende stappen te overwegen:

- [Veilige toegang tot een lab](devtest-lab-add-devtest-user.md).

- [Lab-beleid instellen](devtest-lab-set-lab-policy.md).

- [Een lab-sjabloon maken](devtest-lab-create-template.md).

- [Aangepaste onderdelen voor uw VMs maken](devtest-lab-artifact-author.md).

- [Een VM met artefacten naar een lab toevoegen](devtest-lab-add-vm-with-artifacts.md).
