<properties
    pageTitle="Op rollen gebaseerde toegangscontrole (RBAC) met Azure CLI beheren | Microsoft Azure"
    description="Informatie over het beheren van op rollen gebaseerde toegangscontrole RBAC () met de opdrachtregelinterface van Azure door met rollen en rol acties en rollen toewijzen aan de bereiken abonnement en toepassing."
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

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Met de opdrachtregelinterface van Azure toegangsbeheer op basis van rollen beheren

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

Kunt u op rollen gebaseerde toegangscontrole RBAC () in de portal Azure en Azure Resource Manager API voor het beheren van bronnen op een niveau toegesneden op uw abonnement. Met deze functie kunt u sommige rollen toewijzen aan deze op een bepaald bereik verlenen toegang voor gebruikers, groepen of service beveiligings-principals van Active Directory.

Voordat u de Azure opdrachtregelinterface (CLI) gebruiken kunt voor het beheren van RBAC, hebt u het volgende:

- Azure CLI versie 0.8.8 of hoger. Zie de meest recente versie te installeren en te koppelen aan uw abonnement Azure, [installeren en configureren van de CLI Azure](../xplat-cli-install.md).
- Azure Resource Manager in Azure CLI. Ga naar [met behulp van de CLI Azure met Resource Manager](../xplat-cli-azure-resource-manager.md) voor meer informatie.

## <a name="list-roles"></a>Lijst met rollen

### <a name="list-all-available-roles"></a>Lijst van alle beschikbare rollen
U kunt alle beschikbare functies gebruiken:

        azure role list

In het volgende voorbeeld ziet u de lijst met *alle beschikbare functies*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Screenshot van RBAC Azure opdrachtregel - lijst azure rol-](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Met Lijstacties van een rol
U kunt de acties van een functie gebruiken:

    azure role show "<role name>"

In het volgende voorbeeld ziet u de acties van de *Inzender* en *Inzender van de virtuele Machine* .

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure opdrachtregel - azure rol show - screenshot](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Toegang tot de lijst
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Lijst met functietoewijzingen effectief op een resourcegroep
U kunt de toewijzing van functies die aanwezig zijn in een resourcegroep gebruiken:

    azure role assignment list --resource-group <resource group name>

In het volgende voorbeeld ziet u de toewijzing van functies in de groep *pharma-verkoop-projecforcast* .

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Opdrachtregel Azure RBAC - rol azure toewijzingslijst per groep screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Lijst met functietoewijzingen voor een gebruiker
U kunt de toewijzing van functies voor een specifieke gebruiker en de toewijzingen die zijn toegewezen aan een groepen gebruiken:

    azure role assignment list --signInName <user email>

U ziet ook de toewijzing van functies die zijn overgenomen van de groepen door aanpassing van de opdracht:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

In het volgende voorbeeld ziet u de toewijzing van functies die worden toegekend aan de *sameert@aaddemo.com* gebruiker. Ook de rollen die zijn toegewezen aan de gebruiker rechtstreeks en rollen die zijn overgenomen van de groepen.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure opdrachtregel - azure rol lijst met toewijzingen door gebruiker - screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Toegang verlenen
Om toegang te verlenen nadat u hebt aangegeven dat de rol die u wilt toewijzen, gebruikt u:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Een rol toewijzen aan de groep voor het bereik van het abonnement
Als u een rol toewijst aan een groep voor het bereik van het abonnement, wilt gebruiken:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

In het volgende voorbeeld wordt de rol van *lezer* toegewezen *van Christine Koch Team* voor het bereik van het *abonnement* .


![RBAC Azure opdrachtregel - azure roltoewijzing maken groep screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Een rol toewijzen aan een toepassing voor het bereik van het abonnement
Als u een rol toewijst aan een toepassing voor het bereik van het abonnement, wilt gebruiken:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

In het volgende voorbeeld geeft de rol van *Inzender* voor een *Azure AD* toepassing op het geselecteerde abonnement.

 ![RBAC Azure opdrachtregel - azure roltoewijzing maken door toepassing](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Een rol aan een gebruiker op het groepsbereik resource toewijzen
Als u een rol toewijst aan een gebruiker op het bereik van de resource, wilt gebruiken:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

In het volgende voorbeeld verleent de rol van *Inzender voor virtuele Machine* naar *samert@aaddemo.com* gebruiker op het groepsbereik *Pharma-verkoop-ProjectForcast* bron.

![RBAC Azure opdrachtregel - azure roltoewijzing maken door gebruiker screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Een rol toewijzen aan een groep binnen het bereik van de bron
Als u een rol toewijst aan een groep binnen het bereik van de resource, wilt gebruiken:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

In het volgende voorbeeld verleent de rol van *Inzender voor virtuele Machine* *Azure AD* groepen in een *subnet*.

![RBAC Azure opdrachtregel - azure roltoewijzing maken groep screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Toegang verwijderen
Met verwijdert u een roltoewijzing:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Het volgende voorbeeld worden de *Virtuele Machine Inzender* functietoewijzing uit de *sammert@aaddemo.com* gebruiker op de resourcegroep *Pharma-verkoop-ProjectForcast* .
In het voorbeeld verwijdert de roltoewijzing uit een groep op het abonnement.

![RBAC Azure opdrachtregel - azure rol toewijzing verwijderen - screenshot](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Aangepaste rollen maken
Een aangepaste functie kunt maken:

    azure role create --inputfile <file path>

In het volgende voorbeeld maakt u een aangepaste rol met de naam van de *Virtuele Machine-Operator*. De aangepaste rol toegang verleent tot alle leesbewerkingen van providers van *Microsoft.Compute*, *Microsoft.Storage*en *Microsoft.Network* -bron toegang verleent tot het starten, opnieuw opstarten en virtuele machines te controleren. De aangepaste functie kan worden gebruikt in twee abonnementen. In dit voorbeeld wordt een JSON-bestand als input.

![Screenshot van JSON - definitie van aangepaste rol-](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure opdrachtregel - azure rol maken - screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Een aangepaste rol te wijzigen

Als u wilt een aangepaste rol wijzigen, gebruikt u de `azure role show` opdracht ophalen roldefinitie. Ten tweede, breng de gewenste wijzigingen aan het bestand met de formulierdefinitie rol. Ten slotte, `azure role set` de gewijzigde roldefinitie opslaan.

    azure role set --inputfile <file path>

Het volgende voorbeeld wordt de werking van de *Microsoft.Insights/diagnosticSettings/* aan de **Acties**en een Azure abonnement op de **AssignableScopes** van de aangepaste functie Virtual Machine-Operator.

![JSON - definitie van aangepaste beveiligingsrol - schermafbeelding wijzigen](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Screenshot van RBAC Azure opdrachtregel - set azure rol-](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Als u een aangepaste rol wilt verwijderen, gebruikt u de `azure role show` opdracht bepaalt de **ID** van de rol. Vervolgens gebruikt u de `azure role delete` opdracht voor het verwijderen van de rol van de **ID**opgeven.

In het volgende voorbeeld wordt de aangepaste functie *Virtual Machine-Operator* verwijderd.

![RBAC Azure opdrachtregel - azure rol verwijderen - screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Lijst aangepaste rollen

Als u de functies die beschikbaar voor toewijzing aan een bereik zijn, gebruikt u de `azure role list` opdracht.

In het volgende voorbeeld worden alle rollen die beschikbaar zijn voor de toewijzing in het geselecteerde abonnement.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Screenshot van RBAC Azure opdrachtregel - lijst azure rol-](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

In het volgende voorbeeld wordt de aangepaste functie *Virtual Machine-Operator* is niet beschikbaar in het *Production4* abonnement omdat het abonnement niet in de **AssignableScopes** van de rol.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure opdrachtregel - lijst azure functie voor aangepaste rollen - screenshot](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC onderwerpen
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
