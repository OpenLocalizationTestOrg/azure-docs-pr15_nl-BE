<properties 
    pageTitle="Maken van een logica App Azure Resource Manager sjablonen met Azure App Service | Microsoft Azure" 
    description="Een sjabloon Azure Resource Manager gebruiken voor de implementatie van een lege logica App om workflows te definiëren." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Een logische App met behulp van een sjabloon maken

Een sjabloon Azure Resource Manager gebruiken voor het maken van een lege logica app die kan worden gebruikt voor het definiëren van werkstromen. U kunt definiëren welke bronnen worden geïmplementeerd en het definiëren van parameters die zijn opgegeven bij de installatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen.

Zie voor meer informatie over de eigenschappen van de app logica [Logica App Workflow Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Zie voor voorbeelden van de definitie zelf [auteur logica App definities](app-service-logic-author-definitions.md). 

Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md).

Zie voor de volledige sjabloon [sjabloon App logica](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>U gaat implementeren

Met deze sjabloon kunt u een app logica implementeren.

De installatie automatisch wordt uitgevoerd, selecteert u de volgende knop:  

[![Implementeren op Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Bronnen te implementeren

### <a name="logic-app"></a>Logica app

Maakt de app logica.

De sjablonen wordt de parameterwaarde voor de naam van de app logica. De locatie van de app logica wordt ingesteld op dezelfde locatie als de resourcegroep. 

Deze definitie bepaald eens per uur wordt uitgevoerd en de locatie die is opgegeven in de parameter **testUri** wordt gepingd. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
