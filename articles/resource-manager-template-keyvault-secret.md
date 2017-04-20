<properties
   pageTitle="Sjabloon voor een geheim in een sleutel kluis Resource Manager | Microsoft Azure"
   description="Vindt u het schema Resource Manager voor het implementeren van sleutel kluis geheimen door middel van een sjabloon."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Schema voor geheime sleutel kluis

Hiermee maakt u een geheim dat wordt opgeslagen in een kluis met sleutel. Dit brontype is vaak geïmplementeerd als een onderliggende bron van [sleutel kluis](resource-manager-template-keyvault.md).

## <a name="schema-format"></a>Schema-indeling

Toevoegen als u wilt maken een geheime sleutel kluis, het volgende schema aan uw sjabloon. Het geheim kan worden gedefinieerd als een onderliggende bron van een sleutel kluis of als de resource op het hoogste niveau. U kunt definiëren als een onderliggende bron wanneer de sleutel kluis is geïmplementeerd in dezelfde sjabloon. U moet het geheim definiëren als een middel voor het hoogste niveau wanneer de sleutel kluis is niet geïmplementeerd in dezelfde sjabloon, of als u nodig hebt voor het maken van meerdere geheimen in een lus op het resourcetype. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Waarden

De volgende tabellen beschrijven de waarden die u nodig hebt om in te stellen in het schema.

| Naam | Waarde |
| ---- | ---- | 
| type | Enum<br />Vereist<br />**geheimen** (wanneer geïmplementeerd als een onderliggende bron van sleutel kluis) of<br /> **Microsoft.KeyVault/vaults/secrets** (wanneer geïmplementeerd als een middel voor het hoogste niveau)<br /><br />Het brontype te maken. |
| apiVersion | Enum<br />Vereist<br />**06-01-2015** of **2014-12-19-voorbeeld**<br /><br />De versie van de API te gebruiken voor het maken van de resource. | 
| naam | Tekenreeks<br />Vereist<br />Één woord geïmplementeerd als een onderliggende bron van een sleutel kluis of in de indeling **{sleutel kluis naam} / {naam geheim}** als geïmplementeerd als een bron op het hoogste niveau moet worden toegevoegd aan een bestaande sleutel kluis.<br /><br />De naam van het geheim te maken. |
| Eigenschappen | Object<br />Vereist<br />[object eigenschappen](#properties)<br /><br />Een object die de waarde van het geheim te maken. |
| dependsOn | Matrix<br />Optioneel<br />Een door komma's gescheiden lijst van een resource namen of unieke resource-id's.<br /><br />De collectie van de middelen hangt af van deze koppeling. Als de sleutel kluis voor het geheim is geïmplementeerd in dezelfde sjabloon, kunt u de naam van de sleutel kluis opnemen in dit element om ervoor te zorgen dat eerst wordt geïmplementeerd. |

<a id="properties" />
### <a name="properties-object"></a>object eigenschappen

| Naam | Waarde |
| ---- | ---- | 
| waarde | Tekenreeks<br />Vereist<br /><br />De geheime waarde in de sleutel kluis op te slaan. Bij het doorgeven van een waarde voor deze eigenschap, gebruikt u een parameter van het type **securestring**.  |

    
## <a name="examples"></a>Voorbeelden

Het eerste voorbeeld implementeert een geheim als een onderliggende bron van een sleutel kluis.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Het tweede voorbeeld implementeert het geheim als bron op het hoogste niveau die is opgeslagen in een bestaande sleutel kluis.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Volgende stappen

- Zie voor algemene informatie over de belangrijkste kluizen, [aan de slag met Azure sleutel kluis](./key-vault/key-vault-get-started.md).
- Zie voor een voorbeeld van een verwijzing naar een geheime sleutel kluis bij het distribueren van sjablonen, [veilige waarden tijdens de implementatie](resource-manager-keyvault-parameter.md).


