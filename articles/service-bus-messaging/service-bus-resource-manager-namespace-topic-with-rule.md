<properties
    pageTitle="Een Service Bus-naamruimte maken met het onderwerp, abonnement, en de regels met een sjabloon van Azure Resource Manager | Microsoft Azure"
    description="Een naamruimte Service Bus met onderwerp, abonnement en een regel met behulp van bronbeheer Azure sjabloon maken"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Een Service Bus naamruimte maken met het onderwerp, abonnement en regel met behulp van een sjabloon Azure Resource Manager

Dit artikel wordt beschreven hoe u met een bronnenbeheerder Azure-sjabloon die wordt gemaakt van de naamruimte voor een Service Bus met een onderwerp, abonnement en regel (filter). U leert hoe om te bepalen welke bronnen worden ingezet en het definiëren van parameters die zijn opgegeven bij de installatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen

Zie voor meer informatie over het maken van sjablonen, [sjablonen ontwerpen Azure Resource Manager][].

Zie voor meer informatie over de praktijk en patronen op Azure bronnen voor naamgeving, [Conventies voor Azure bronnen][].

Zie voor de volledige sjabloon de sjabloon [Service Bus naamruimte met onderwerp, abonnement, en regel][] .

>[AZURE.NOTE] De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor het downloaden en uitvoeren.
>
>-    [Een naamruimte Service Bus met wachtrij en autorisatie-regel maken](service-bus-resource-manager-namespace-auth-rule.md)
>-    [De naamruimte voor een Service Bus met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
>-    [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
>-    [Een Service Bus naamruimte maken met het onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
>
>Om te controleren of de meest recente sjablonen, gaat u naar de galerie met [Azure Quickstart sjablonen][] en zoeken naar Service Bus.

## <a name="what-will-you-deploy"></a>Wat gaat u implementeren?

Met deze sjabloon kunt u een naamruimte Service Bus met onderwerp, abonnement en regel (filter) implementeren.

[Service Bus onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) bieden een een-op-veel-formulier van communicatie in een patroon *publiceren/abonneren* . Wanneer u onderwerpen en abonnementen, onderdelen van een gedistribueerde toepassing niet communiceren met elkaar, in plaats daarvan exchange zij-berichten via het onderwerp dat bemiddelt. Een abonnement op een onderwerp lijkt op een virtuele wachtrij kopieën van berichten die zijn verzonden naar het onderwerp. Een filter op een abonnement kunt u opgeven welke berichten worden verzonden naar een onderwerp moet worden weergegeven binnen een bepaald onderwerp-abonnement.

## <a name="what-are-rules-filters"></a>Wat zijn de regels (filters)?

In veel gevallen moeten de berichten met specifieke kenmerken op verschillende manieren worden verwerkt. U kunt abonnementen om te zoeken naar berichten die u hebt de gewenste eigenschappen en voert u bepaalde wijzigingen aanbrengen in de eigenschappen configureren om dit mogelijk. Terwijl abonnementen Service Bus alle berichten die naar het onderwerp ziet, kunt u alleen een subset van deze berichten kopiëren naar de wachtrij virtuele abonnement. Dit wordt bereikt met behulp van filters abonnement. Zie voor meer informatie op rules(filters), [Service Bus wachtrijen, onderwerpen, en abonnementen][].

De installatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren op Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager moet definiëren de parameters voor de waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u om te implementeert definiëren. Definieert parameters voor de waarden die altijd hetzelfde blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

De naam van de naamruimte Service Bus te maken.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

De naam van het onderwerp in de naamruimte Service Bus gemaakt.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

De naam van het abonnement dat in de naamruimte Service Bus.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName

De naam van de rule(filter) gemaakt in de naamruimte Service Bus.

```
   "serviceBusRuleName": {
   "type": "string",
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

Maakt een standaard Service Bus-naamruimte van het type **Messaging**, met onderwerp en abonnement en regels.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gemaakt en geïmplementeerd met Azure Resource Manager bronnen, meer informatie over deze resources beheren door deze artikelen te bekijken:

- [Azure Service Bus met Azure automatisering beheren](service-bus-automation-manage.md)
- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Resources met de Explorer Service Bus Bus-Service beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Azure Resource Manager sjablonen ontwerpen]: ../resource-group-authoring-templates.md
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Azure Resources Naming Conventions]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [De naamruimte Service Bus met onderwerp, abonnement en regel]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Service Bus wachtrijen, onderwerpen en abonnementen]:service-bus-queues-topics-subscriptions.md
  
