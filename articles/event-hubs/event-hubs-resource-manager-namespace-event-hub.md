<properties
    pageTitle="Een naamruimte Hubs gebeurtenis met gebeurtenis-Hub en consument met behulp van een sjabloon Azure Resource Manager maken | Microsoft Azure"
    description="Een naamruimte Hubs gebeurtenis met gebeurtenis Hub en consument groep met behulp van bronbeheer Azure-sjabloon maken"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Een naamruimte Hubs gebeurtenis met gebeurtenis-Hub en consument met behulp van een sjabloon Azure Resource Manager maken

Dit artikel wordt beschreven hoe u met een bronnenbeheerder Azure-sjabloon die u een naamruimte gebeurtenis Hubs met een Hub gebeurtenis en een groep consumenten maakt. U leert hoe om te bepalen welke bronnen worden geïmplementeerd en het definiëren van parameters die zijn opgegeven bij de installatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen

Zie [sjablonen ontwerpen Azure Resource Manager][]voor meer informatie over het maken van sjablonen.

Zie voor de volledige sjabloon de [gebeurtenis Hub en consumentsjabloon groep][] op GitHub.

>[AZURE.NOTE]
>Om te controleren of de meest recente sjablonen, gaat u naar de galerie met [Azure Quickstart sjablonen][] en zoeken naar de gebeurtenis Hubs.

## <a name="what-will-you-deploy"></a>Wat gaat u implementeren?

Met deze sjabloon implementeert u een naamruimte gebeurtenis Hubs met een Hub gebeurtenis en een groep consumenten.

[Hubs gebeurtenis](../event-hubs/event-hubs-what-is-event-hubs.md) is een gebeurtenis verwerkt gebruikt voor gebeurtenis- en telemetrie ingress naar Azure op grote schaal, met lage latentie en hoge betrouwbaarheid.

De installatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren op Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. Een parameter voor de waarden die verschillen op basis van het project dat u wilt implementeren of op basis van de omgeving die u levert, moet u definiëren. Definieert parameters voor de waarden die altijd hetzelfde zal blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

De naam van de naamruimte Hubs gebeurtenis te maken.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

De naam van de gebeurtenis Hub gemaakt in de naamruimte Hubs.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

De naam van de groep van de consument gemaakt voor de gebeurtenis Hub.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

De API-versie van de sjabloon.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Bronnen te implementeren

Maakt een naamruimte van het type **EventHubs**, met een Hub gebeurtenis en een groep consumenten.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Azure Resource Manager sjablonen ontwerpen]: ../resource-group-authoring-templates.md
[Azure Quickstart sjablonen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Sjabloon voor gebeurtenis Hub- en -groep]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
