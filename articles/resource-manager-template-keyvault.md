<properties
   pageTitle="Sjabloon voor sleutel kluis Resource Manager | Microsoft Azure"
   description="Vindt u het schema Resource Manager voor het implementeren van sleutels kluizen door middel van een sjabloon."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Schema voor sleutel kluis

Hiermee maakt u een sleutel kluis.

## <a name="schema-format"></a>Schema-indeling

Toevoegen in het volgende schema aan de sectie bronnen van de sjabloon wilt maken van een sleutel kluis.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Waarden

De volgende tabellen beschrijven de waarden die u nodig hebt om in te stellen in het schema.

| Naam | Waarde |
| ---- | ---- | 
| type | Enum<br />Vereist<br />**Microsoft.KeyVault/vaults**<br /><br />Het brontype te maken. |
| apiVersion | Enum<br />Vereist<br />**06-01-2015** of **2014-12-19-voorbeeld**<br /><br />De versie van de API te gebruiken voor het maken van de resource. | 
| naam | Tekenreeks<br />Vereist<br />Een naam die is uniek voor Azure.<br /><br />De naam van de sleutel kluis te maken. Kunt u met de functie [uniqueString](resource-group-template-functions.md#uniquestring) met de naamgevingsconventie maken een unieke naam, zoals in het volgende voorbeeld wordt getoond. |
| locatie | Tekenreeks<br />Vereist<br />Een geldige regio voor belangrijke kluizen. Geldige regio's vindt u [regio's ondersteund](resource-manager-supported-services.md#supported-regions).<br /><br />De regio als host voor de sleutel kluis. |
| Eigenschappen | Object<br />Vereist<br />[object eigenschappen](#properties)<br /><br />Een object dat het type sleutel kluis te maken. |
| bronnen | Matrix<br />Optioneel<br />Toegestane waarden: [geheime sleutel kluis-bronnen](resource-manager-template-keyvault-secret.md)<br /><br />Onderliggende bronnen voor de sleutel kluis. |

<a id="properties" />
### <a name="properties-object"></a>object eigenschappen

| Naam | Waarde |
| ---- | ---- | 
| enabledForDeployment | Boole-waarde<br />Optioneel<br />**waar** of **Onwaar**<br /><br />Hiermee geeft u aan of de kluis is ingeschakeld voor de virtuele Machine of Service Fabric-implementatie. |
| enabledForTemplateDeployment | Boole-waarde<br />Optioneel<br />**waar** of **Onwaar**<br /><br />Hiermee geeft u aan of de kluis is ingeschakeld voor gebruik in Bronbeheer sjabloon implementaties. Voor meer informatie, Zie [veilige waarden tijdens de implementatie](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Boole-waarde<br />Optioneel<br />**waar** of **Onwaar**<br /><br />Hiermee geeft u aan of de kluis is ingeschakeld voor de codering van het volume. |
| tenantId | Tekenreeks<br />Vereist<br />**Globaal unieke id**<br /><br />De id van de huurder voor het abonnement. U kunt deze ophalen met de PowerShell-cmdlet [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) of de **azure rekening toont** Azure CLI-opdracht. |
| accessPolicies | Matrix<br />Vereist<br />[accessPolicies, object](#accesspolicies)<br /><br />Een matrix van maximaal 16-objecten die de machtigingen voor de gebruiker of service principal opgeven. |
| SKU | Object<br />Vereist<br />[SKU-object](#sku)<br /><br />De SKU voor de sleutel kluis. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies, object

| Naam | Waarde |
| ---- | ---- | 
| tenantId | Tekenreeks<br />Vereist<br />**Globaal unieke id**<br /><br />De id van de huurder van de huurder Azure Active Directory is met de **object-id** in dit toegangsbeleid |
| object-id | Tekenreeks<br />Vereist<br />**Globaal unieke id**<br /><br />De object-id van de Azure Active Directory-gebruiker of service principal die toegang tot de kluis hebben. U kunt de waarde ophalen van de [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) of de [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) PowerShell-cmdlets of de **gebruiker azure ad** of **azure ad sp** Azure CLI-opdrachten. |
| machtigingen | Object<br />Vereist<br />[machtigingen voor object](#permissions)<br /><br />De machtigingen op deze kluis voor het Active Directory-object. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions, object

| Naam | Waarde |
| ---- | ---- | 
| toetsen | Matrix<br />Vereist<br />**alle**, **back-up**, **maken**, **decoderen**, **verwijderen**, **coderen**, **ophalen**, **importeren**, **lijst**, **herstellen**, **teken**, **unwrapkey**, **bijwerken**, **controleren**, **wrapkey**<br /><br />De machtigingen voor de sleutels in deze kluis op deze Active Directory-object. Deze waarde moet worden opgegeven als een array van een of meer toegestane waarden. |
| geheimen | Matrix<br />Vereist<br />**alle**, **verwijderen**, **opvragen**, **lijst**, **instellen**<br /><br />De machtigingen op geheimen in deze kluis op deze Active Directory-object. Deze waarde moet worden opgegeven als een array van een of meer toegestane waarden. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU, object

| Naam | Waarde |
| ---- | ---- | 
| naam | Enum<br />Vereist<br />**Standard**of **premium** <br /><br />De servicelaag van KeyVault te gebruiken.  Standaard ondersteunt geheimen en sleutels software wordt beschermd.  Premium wordt ondersteuning toegevoegd voor sleutels beveiligd met een HSM. |
| familie | Enum<br />Vereist<br />**A** <br /><br />De sku-familie te gebruiken. |
 
    
## <a name="examples"></a>Voorbeelden

In het volgende voorbeeld wordt uitgevoerd, implementeert een sleutel kluis en geheim.

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

## <a name="quickstart-templates"></a>QuickStart-sjablonen

De volgende quickstart sjabloon implementeert een sleutel kluis.

- [Maak de sleutel kluis](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Volgende stappen

- Zie voor algemene informatie over de belangrijkste kluizen, [aan de slag met Azure sleutel kluis](./key-vault/key-vault-get-started.md).
- Zie voor een voorbeeld van een verwijzing naar een geheime sleutel kluis bij het distribueren van sjablonen, [veilige waarden tijdens de implementatie](resource-manager-keyvault-parameter.md).

