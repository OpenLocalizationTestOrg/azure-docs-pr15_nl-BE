<properties 
    pageTitle="Een web app dat is gekoppeld aan een opslagplaats GitHub implementeren" 
    description="Een sjabloon Azure Resource Manager gebruiken voor de implementatie van een web app met een project uit een bibliotheek GitHub." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Een web app gekoppeld aan een opslagplaats GitHub implementeren

In dit onderwerp leert u hoe een bronnenbeheerder Azure sjabloon maken die wordt uitgevoerd, implementeert een web app dat is gekoppeld aan een project in een opslagplaats van GitHub. U leert hoe om te bepalen welke bronnen worden geïmplementeerd en het definiëren van parameters die zijn opgegeven bij de installatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen.

Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md).

Zie [Web App gekoppeld aan de sjabloon GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json)voor de complete sjabloon.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>U gaat implementeren

Met deze sjabloon implementeert u een web app met de code van een project in de GitHub.

De installatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren op Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

De URL van de opslagplaats van GitHub met het project te implementeren. Deze parameter bevat een standaardwaarde, maar deze waarde is alleen bedoeld voor hoe u de URL opgeven voor de opslagplaats. Is het verstandig om de URL van uw eigen bibliotheek bij het werken met de sjabloon, maar u kunt deze waarde bij het testen van de sjabloon.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>filiaal

De vertakking van de opslagplaats te gebruiken bij de implementatie van de toepassing. De standaardwaarde is master, maar u kunt de naam opgeven van een tak in de opslagplaats die u wilt implementeren.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Bronnen te implementeren

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Web app.

Hiermee maakt u de web-app die is gekoppeld aan het project in de GitHub. 

U geeft de naam van de web-app via de parameter **siteName** en de locatie van de web-app via de parameter **siteLocation** . In het element **dependsOn** definieert de sjabloon de web app als afhankelijk van de service hosting plan. Omdat deze afhankelijk van het hosting plan is, wordt de web app niet gemaakt totdat het hosting plan is voltooid, wordt gemaakt. Het element **dependsOn** wordt alleen gebruikt voor de implementatie van volgorde opgeven. Als u de web app als afhankelijk van het hosting plan niet inschakelt, Azure Resource Mananger probeert te maken van beide resources tegelijkertijd en er mogelijk een foutbericht als de web app is gemaakt voordat de hosting plan.

De web app heeft ook een onderliggende bron die is gedefinieerd in de sectie **bronnen** hieronder. Deze onderliggende bron definieert het besturingselement voor het project is geïmplementeerd met de web app. In deze sjabloon, is het besturingselement van de gegevensbron gekoppeld aan een bepaalde GitHub opslagplaats. De opslagplaats van GitHub is gedefinieerd met de code **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** mogelijk harde code de URL van de opslagplaats als u een sjabloon maken die herhaaldelijk een project implementeert tijdens het minimale aantal parameters vereist.
In plaats van hard te coderen de URL van de opslagplaats, kunt u een parameter toevoegen voor de URL van de opslagplaats en die waarde voor de eigenschap **RepoUrl** gebruiken.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
