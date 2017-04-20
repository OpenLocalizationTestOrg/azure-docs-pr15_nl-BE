<properties
    pageTitle="Een Service Bus-naamruimte maken met de wachtrij met behulp van een sjabloon Azure Resource Manager | Microsoft Azure"
    description="Een naamruimte Service Bus en een wachtrij met behulp van bronbeheer Azure-sjabloon maken"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Een naamruimte Service Bus en een wachtrij met een bronnenbeheerder Azure-sjabloon maken

In dit artikel ziet u hoe een Azure Resource Manager-sjabloon die u een naamruimte Service Bus en een wachtrij maakt te gebruiken. U leert hoe om te bepalen welke bronnen worden geïmplementeerd en het definiëren van parameters die zijn opgegeven bij de installatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen.

Zie [sjablonen ontwerpen Azure Resource Manager][]voor meer informatie over het maken van sjablonen.

Zie voor de volledige sjabloon de [Service Bus naamruimte en wachtrij-sjabloon][] op GitHub.

>[AZURE.NOTE] De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor het downloaden en uitvoeren.
>
>-    [Een naamruimte Service Bus met wachtrij en autorisatie-regel maken](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Een Service Bus naamruimte maken met het onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
>-    [Een gebeurtenis Hubs naamruimte maken met een groep gebeurtenis Hub en consument](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Om te controleren of de meest recente sjablonen, gaat u naar de galerie met [Azure Quickstart sjablonen][] en zoeken naar 'Service Bus'.

## <a name="what-will-you-deploy"></a>Wat gaat u implementeren?

Met deze sjabloon implementeert u een naamruimte Service Bus met een wachtrij.

First In, eerste Out (FIFO) berichtlevering aan een of meer concurrerende consumenten bieden een [Service Bus wachtrijen](service-bus-queues-topics-subscriptions.md#queues) .

De installatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren op Azure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. Een parameter voor de waarden die verschillen op basis van het project dat u wilt implementeren of op basis van de omgeving die u levert, moet u definiëren. Definieert parameters voor de waarden die altijd hetzelfde zal blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

De naam van de naamruimte Service Bus te maken.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

De naam van de wachtrij die is gemaakt in de naamruimte Service Bus.

```
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

De Service Bus API versie van de sjabloon.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Bronnen te implementeren

Maakt een standaard Service Bus-naamruimte van het type **e**, met een wachtrij.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gemaakt en geïmplementeerd met Azure Resource Manager bronnen, meer informatie over deze resources beheren door deze artikelen te bekijken:

- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Resources met de Explorer Service Bus Bus-Service beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Azure Resource Manager sjablonen ontwerpen]: ../resource-group-authoring-templates.md
  [De naamruimte en de wachtrij sjabloon Service Bus]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
