<properties
   pageTitle="Sjabloon voor roltoewijzingen Resource Manager | Microsoft Azure"
   description="Vindt u het schema Resource Manager voor het implementeren van een roltoewijzing door middel van een sjabloon."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Schema voor toewijzingen rol

Een gebruiker, groep of service principal toewijst aan een rol op een opgegeven bereik.

## <a name="resource-format"></a>Opmaak van bron

Een roltoewijzing maken, toevoegen in het volgende schema aan de resources sectie van uw sjabloon.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Waarden

De volgende tabellen beschrijven de waarden die u nodig hebt om in te stellen in het schema.

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| type | Ja    | Het brontype te maken.<br /><br /> Voor de resourcegroep:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Voor resource:<br />**{provider naamruimte} / {brontype} providers/roleAssignments** |
| apiVersion |Ja | De versie van de API te gebruiken voor het maken van de resource.<br /><br /> **07-01-2015**gebruiken. | 
| naam | Ja | Een globaal unieke id voor de nieuwe functie toewijzen. |
| dependsOn | Nee | Een door komma's gescheiden reeks resource namen of unieke resource-id's.<br /><br />De collectie van deze roltoewijzing is afhankelijk van bronnen. Als voor het toewijzen van een rol die binnen het bereik van een bron en bron is geïmplementeerd in dezelfde sjabloon opnemen die resourcenaam in dit element kunt u dat de resource eerst wordt geïmplementeerd. |
| Eigenschappen | Ja | Het properties-object waarmee de rol, de opdrachtgever en de scope. |

### <a name="properties-object"></a>object eigenschappen

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| roleDefinitionId | Ja |  De id van een bestaande functiedefinitie in de roltoewijzing wordt gebruikt.<br /><br /> Gebruik de volgende notatie:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Ja | De globally unique identifier voor een bestaande principal. Deze waarde wordt toegewezen aan de id in de directory en kunnen verwijzen naar een gebruiker, groep of service principal. |
| toepassingsgebied | Nee | Het bereik waarop deze roltoewijzing is van toepassing op.<br /><br />Gebruik voor resourcegroepen:<br />**/Subscriptions/ {abonnement-id} /resourceGroups/ {bron groep naam}**  <br /><br />Gebruik voor resources:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Het gebruik van de rol van toewijzing bron

U toevoegen een roltoewijzing aan uw sjabloon als u een gebruiker, groep of service principal toevoegen aan een rol bij de implementatie. Roltoewijzingen worden overgenomen van hogere niveaus van scope, dus als u al een opdrachtgever aan een rol op het niveau van het abonnement hebt toegevoegd, u niet hoeft op voor de resource of resourcegroep toewijzen.

Er zijn veel id-waarden, u moet bij het werken met functietoewijzingen. U kunt de waarden via PowerShell of Azure CLI ophalen.

### <a name="powershell"></a>PowerShell

De naam van de functietoewijzing moet een globally unique identifier. U kunt een nieuwe **naam** met-id genereren:

    $name = [System.Guid]::NewGuid().toString()

U kunt de id voor de functiedefinitie met ophalen:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Voor de opdrachtgever met een van de volgende opdrachten kunt u de id ophalen.

Naam voor een groep **accountants**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Voor een gebruiker met de naam **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Voor een service principal met de naam **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure CLI

U kunt de id voor de functiedefinitie met ophalen:

    azure role show Reader --json | jq .[].Id -r

Voor de opdrachtgever met een van de volgende opdrachten kunt u de id ophalen.

Naam voor een groep **accountants**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Voor een gebruiker met de naam **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Voor een service principal met de naam **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Voorbeelden

De volgende sjabloon ontvangt een id voor een rol en een id voor een gebruiker, groep of service principal. De rol op het niveau van de resource wilt toewijzen.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



De volgende sjabloon maakt u een account voor de opslag en de rol van lezer toewijst aan de account van de opslag. De id's van de twee groepen en de rol van lezer zijn opgenomen in de sjabloon voor het vereenvoudigen van de implementatie. Deze waarden kunnen worden opgehaald bij de implementatie via script en als parameters doorgegeven.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>QuickStart-sjablonen

De volgende sjablonen wordt aangegeven hoe de rol van toewijzing bron:

- [Ingebouwde rol toewijzen aan een bronnengroep](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Ingebouwde rol toewijzen aan een bestaande VM](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Ingebouwde rol toewijzen aan meerdere bestaande VMs](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de sjabloonstructuur van de [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).
- Voor meer informatie over rollen gebaseerd toegangsbeheer Zie [Toegangsbeheer op basis van Azure Active Directory-rol](./active-directory/role-based-access-control-configure.md).
