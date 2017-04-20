<properties
   pageTitle="Geheime sleutel kluis met bronbeheer sjabloon | Microsoft Azure"
   description="Laat zien hoe een geheim uit een sleutel kluis als parameter doorgeven tijdens de implementatie."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Veilige waarden doorgeven tijdens de implementatie

Wanneer moet u een veilige waarde (zoals een wachtwoord) als parameter doorgeven tijdens de implementatie kunt u waarde opslaan als een geheim in een [Azure sleutel kluis](./key-vault/key-vault-whatis.md) en verwijst naar de waarde in een andere Resource Manager-sjablonen. Neem alleen een verwijzing naar het geheim in de sjabloon zodat het geheim nooit weergegeven wordt en hoeft u niet de waarde handmatig invoeren voor het geheim elke keer dat u de resources implementeren. U opgeven welke gebruikers of service beveiligings-principals toegang krijgen het geheim tot.  

## <a name="deploy-a-key-vault-and-secret"></a>Implementeren van een sleutel kluis en geheim

Sleutel kluis die kan worden verwezen vanuit een andere Resource Manager-sjablonen maken, moet u de eigenschap **enabledForTemplateDeployment** ingesteld op **true**, en moet u toegang verlenen aan de gebruiker of service principal die de implementatie die verwijst naar het geheim wordt uitgevoerd.

Zie meer informatie over het implementeren van een sleutel kluis en geheim, [sleutel kluis schema](resource-manager-template-keyvault.md) en [geheime sleutel kluis-schema](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Verwijst naar een geheim met statische-id

U verwijst naar het geheim van in een parameterbestand met die waarden aan uw sjabloon. U verwijst naar het geheim door de bron-id van de sleutel kluis en de naam van het geheim. In dit voorbeeld wordt de geheime sleutel kluis moet al bestaan en u gebruikt een statische waarde voor deze resource-id.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Een hele parameterbestand uitzien als volgt:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

De parameter die het geheim accepteert, moet een **securestring**. In het volgende voorbeeld ziet u de relevante onderdelen van een sjabloon waarmee een SQL-server waarvoor u een administrator-wachtwoord.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Verwijst naar een geheim met dynamische-id

De vorige sectie bleek een statische resource-id voor de geheime sleutel kluis doorgeven. In sommige gevallen moet u echter verwijzen naar een kluis met sleutel geheim dat varieert op basis van de huidige installatie. U kunt in dat geval geen harde code de resource-id in het parameterbestand. Helaas, u kan dynamisch genereren de resource-id in het parameterbestand Sjabloonexpressies zijn niet toegestaan in het parameterbestand.

Dynamisch genereren van de resource-id voor een geheime sleutel kluis, moet u de resource die het geheim in een geneste sjabloon moet verplaatsen. In de hoofdsjabloon de geneste sjabloon toevoegen en een parameter met de dynamisch gegenereerde resource-id opgeven.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Volgende stappen

- Zie voor algemene informatie over de belangrijkste kluizen, [aan de slag met Azure sleutel kluis](./key-vault/key-vault-get-started.md).
- Zie [Security considerations voor Azure Resource Manager](best-practices-resource-manager-security.md)voor meer informatie over het gebruik van een sleutel kluis met een virtuele Machine.
- Zie voor volledige voorbeelden van verwijzingen naar belangrijke geheimen, [sleutel kluis voorbeelden](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

