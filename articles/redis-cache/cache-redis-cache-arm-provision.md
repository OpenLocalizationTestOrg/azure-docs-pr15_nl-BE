<properties 
    pageTitle="Inrichten van een Cache bestand Vgx. | Microsoft Azure" 
    description="Azure Resource Manager sjabloon gebruiken voor de implementatie van een Azure bestand Vgx. Cache." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Een Cache bestand Vgx. met behulp van een sjabloon maken

In dit onderwerp leert u het maken van een sjabloon Azure Resource Manager een Azure bestand Vgx. Cache implementeert. De cache kan worden gebruikt met een bestaande account voor opslag diagnostische gegevens op te slaan. U leert ook hoe om te bepalen welke bronnen worden geïmplementeerd en het definiëren van parameters die zijn opgegeven bij de installatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen.

Op dit moment diagnostische instellingen gelden voor alle caches in hetzelfde gebied voor een abonnement. Een cache in het gebied bij te werken, is van invloed op andere caches in de regio.

Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md).

Zie voor de volledige sjabloon [sjabloon bestand Vgx. Cache](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Sjablonen voor de nieuwe [laag Premium](cache-premium-tier-intro.md) Resource Manager zijn beschikbaar. 
>
>-    [Een Premium bestand Vgx. Cache met clusters maken](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Premium bestand Vgx. Cache met persistentie van gegevens maken](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Premium bestand Vgx. Cache met VNet en optionele clusters maken](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>[Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/) bekijken om te controleren of de meest recente sjablonen, en zoek naar `Redis Cache`.

## <a name="what-you-will-deploy"></a>U gaat implementeren

In deze sjabloon implementeert u een Azure bestand Vgx. Cache die gebruikmaakt van een bestaande account voor de opslag voor diagnostische gegevens.

De installatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren op Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters met alle parameterwaarden.
U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u om te implementeert definiëren. Definieert parameters voor de waarden die altijd hetzelfde blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

De locatie van het bestand Vgx. Cache. Gebruik dezelfde locatie als de app met de cache moet worden gebruikt voor de beste prestaties.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

De naam van de bestaande opslag account moet worden gebruikt voor diagnostische gegevens. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Een Booleaanse waarde die of aangeeft voor toegang via niet-SSL-poort.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Een waarde die aangeeft of diagnostische gegevens is ingeschakeld. Gebruik ON of OFF.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Bronnen te implementeren

### <a name="redis-cache"></a>Cache bestand Vgx.

Hiermee maakt u de Cache voor het bestand Vgx Azure.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


