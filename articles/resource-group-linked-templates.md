<properties
   pageTitle="Sjablonen met Resource Manager gekoppeld | Microsoft Azure"
   description="Beschrijving van het gekoppelde sjablonen in een sjabloon Azure Resource Manager gebruiken voor het maken van een voor modulaire sjabloonoplossing. Laat zien hoe u de parameterwaarden doorgeven, een parameterbestand en dynamisch gemaakte URL's opgeven."
   services="azure-resource-manager"
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
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Met behulp van gekoppelde sjablonen met Azure Resource Manager

Van binnen één bronnenbeheerder Azure sjabloon kunt u koppelen aan een andere sjabloon waarmee u om uw implementatie in een reeks gerichte, specifieke doel-sjablonen. Net als bij een toepassing in de verschillende codeklassen ontleedt, biedt ontleding voordelen testen, hergebruik en leesbaarheid.  

U kunt vanuit een sjabloon van de belangrijkste parameters doorgeven aan een gekoppelde sjabloon en deze parameters rechtstreeks kunnen toewijzen aan parameters of variabelen die door de aanroepende sjabloon weergegeven. De gekoppelde sjabloon kunt ook doorgeven aan een variabele output terug naar de bronsjabloon, waardoor een tweeweg data uitwisseling tussen sjablonen.

## <a name="linking-to-a-template"></a>Koppelen aan een sjabloon

U maakt een koppeling tussen twee sjablonen door een resource implementatie binnen de hoofdsjabloon die naar de gekoppelde sjabloon verwijst toe te voegen. U kunt de eigenschap **templateLink** instellen op de URI van de gekoppelde sjabloon. U kunt bieden parameterwaarden voor de gekoppelde sjabloon door de waarden rechtstreeks in de sjabloon op te geven of door te koppelen aan een parameterbestand. In het volgende voorbeeld wordt de eigenschap **parameters** rechtstreeks een parameterwaarde opgeven.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

De Resource Manager-service moet toegang kunnen krijgen tot de gekoppelde sjabloon. U kunt een lokaal bestand of een bestand dat alleen beschikbaar is op uw lokale netwerk voor de gekoppelde sjabloon niet opgeven. U kunt alleen een URI-waarde met **http** of **https**opgeven. Een optie is de gekoppelde sjabloon op een rekening voor opslag en gebruik van de URI voor het item, zoals in het volgende voorbeeld.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Hoewel de gekoppelde sjabloon extern beschikbaar zijn moet, hoeft deze niet algemeen beschikbaar voor het publiek. U kunt de sjabloon naar een particuliere opslag die toegankelijk is voor alleen de rekeninghouder opslag toevoegen. Maak vervolgens een gedeelde-handtekening (SAS) toegangstoken voor toegang tijdens de implementatie. U voegt dat SAS-token aan de URI voor de gekoppelde sjabloon. Zie voor stapsgewijze instructies voor het instellen van een sjabloon in een opslag-account en een SAS-token te genereren, [resources, distribueren met sjablonen Resource Manager en Azure PowerShell](resource-group-template-deploy.md) of [resources, distribueren met sjablonen Resource Manager en Azure CLI](resource-group-template-deploy-cli.md). 

In het volgende voorbeeld ziet u een bovenliggende sjabloon die is gekoppeld aan een andere sjabloon. De gekoppelde sjabloon wordt geopend met een SAS-token dat wordt doorgegeven als een parameter.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Hoewel het token wordt doorgegeven als een tekenreeks veilig, de URI van de gekoppelde sjabloon, met inbegrip van het token SAS vastgelegd in de implementatie-activiteiten voor de betreffende bronnengroep. Wilt beperken van blootstelling, stelt u een vervaldatum voor de token.

## <a name="linking-to-a-parameter-file"></a>Koppeling maken naar een parameterbestand

In het volgende voorbeeld wordt de eigenschap **parametersLink** om te koppelen aan een parameterbestand.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

De URI-waarde voor de parameter gekoppelde bestand kan niet een lokaal bestand en **http** of **https**moet bevatten. De parameterbestand kan ook worden beperkt tot toegang via een SAS-token.

## <a name="using-variables-to-link-templates"></a>Variabelen gebruiken sjablonen koppelen

De voorgaande voorbeelden werd hardgecodeerde URL waarden voor de formuliersjabloon een koppeling. Deze benadering kan werken voor een eenvoudige sjabloon, maar werkt niet goed wanneer u werkt met een grote verzameling van modulaire sjablonen. In plaats daarvan kunt u een statische variabele met een basis-URL voor de belangrijkste sjabloon maken en vervolgens de URL voor de gekoppelde sjablonen uit de basis-URL die dynamisch maken. Het voordeel van deze aanpak is eenvoudig verplaatsen of de sjabloon zich splitsen omdat u alleen hoeft de statische variabele in de belangrijkste sjabloon te wijzigen. De belangrijkste sjabloon geeft de juiste URI's in de sjabloon opgesplitste.

In het volgende voorbeeld ziet u hoe u een basis-URL voor het maken van twee URL's voor gekoppelde sjablonen (**sharedTemplateUrl** en **vmTemplate**). 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

U kunt ook [deployment()](resource-group-template-functions.md#deployment) gebruiken om de basis-URL voor de huidige sjabloon en gebruik die voor de URL voor andere sjablonen op dezelfde locatie. Deze methode is handig als uw sjabloon wordt gewijzigd (misschien door versioning) of als u wilt voorkomen dat de URL's in het sjabloonbestand hard te coderen. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Voorwaardelijk koppelen aan sjablonen

U kunt koppelen aan verschillende sjablonen door doorgeven in een waarde voor de parameter die wordt gebruikt om de URI van de gekoppelde sjabloon samen te stellen. Deze methode werkt goed wanneer u opgeven tijdens de implementatie die sjabloon moet gekoppelde te gebruiken. Zo kunt u een sjabloon wilt gebruiken voor een bestaande account voor opslag en een andere sjabloon wilt gebruiken voor een nieuwe account voor opslag.

In het volgende voorbeeld ziet u een parameter voor de naam van een opslag en een parameter op te geven of de opslag nieuw of bestaand is.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

U een variabele maken voor de sjabloon URI die de waarde van de nieuwe of bestaande parameter bevat.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

U opgeven de waarde van die variabele voor de bron van de implementatie.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

De URI omgezet in een sjabloon met de naam **existingStorageAccount.json** of **newStorageAccount.json**. Sjablonen maken voor deze URI's.

In het volgende voorbeeld wordt de sjabloon **existingStorageAccount.json** .

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

In het volgende voorbeeld wordt de sjabloon **newStorageAccount.json** . Zoals u ziet, zoals de opslag van de bestaande accountsjabloon het accountobject van de opslag geretourneerd in de uitvoer. De hoofdsjabloon werkt met beide gekoppelde sjabloon.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Voorbeeld van de volledige

Het volgende voorbeeld sjablonen bevatten een vereenvoudigde regeling van de gekoppelde sjablonen te worden enkele van de concepten in dit artikel. Wordt ervan uitgegaan dat de sjablonen zijn toegevoegd aan dezelfde container in een opslag rekening met publieke toegang is uitgeschakeld. De gekoppelde sjabloon geeft een waarde terug aan de belangrijkste sjabloon in de sectie **uitvoer** .

Het bestand **parent.json** bestaat uit:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Het bestand **helloworld.json** bestaat uit:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
In PowerShell, een token krijgen voor de container en de sjablonen met implementeren:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

In Azure CLI, een token krijgen voor de container en de sjablonen met de volgende code implementeert. U moet een naam voor de implementatie op dit moment opgeven wanneer u een sjabloon URI met een SAS-token.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

U gevraagd om het token SAS als parameter. U moet het token met **?**begint.

## <a name="next-steps"></a>Volgende stappen
- Zie meer informatie over het definiëren van de volgorde van de implementatie van uw resources, [afhankelijkheden in Azure Resource Manager sjablonen definiëren](resource-group-define-dependencies.md)
- Zie informatie over het definiëren van een resource, maar veel exemplaren van het maken, [meerdere exemplaren van de resources in Azure Resource Manager maken](resource-group-create-multiple.md)
