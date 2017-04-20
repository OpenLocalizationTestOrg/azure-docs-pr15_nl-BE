<properties
    pageTitle="Een verificatieregel in met behulp van een sjabloon Azure Resource Manager-Service Bus maken | Microsoft Azure"
    description="Een verificatieregel Service Bus voor de naamruimte en de wachtrij met behulp van bronbeheer Azure-sjabloon maken"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Een verificatieregel Service Bus voor de naamruimte en de wachtrij met behulp van een sjabloon Azure Resource Manager maken

Dit artikel wordt het gebruik van een resourcemanager Azure-sjabloon die een [verificatieregel](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) voor een Service Bus-naamruimte en de wachtrij wordt gemaakt. U leert hoe om te bepalen welke bronnen worden geïmplementeerd en het definiëren van parameters die zijn opgegeven bij de installatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen.

Zie [sjablonen ontwerpen Azure Resource Manager][]voor meer informatie over het maken van sjablonen.

Zie voor de volledige sjabloon de [sjabloon auth Service Bus][] op GitHub.

>[AZURE.NOTE] De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor het downloaden en uitvoeren.
>
>-    [Een gebeurtenis Hubs naamruimte maken met een groep gebeurtenis Hub en consument](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [De naamruimte voor een Service Bus met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
>-    [Een Service Bus naamruimte maken met het onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
>
>Om te controleren of de meest recente sjablonen, gaat u naar de galerie met [Azure Quickstart sjablonen][] en zoeken naar 'Service Bus'.

## <a name="what-will-you-deploy"></a>Wat gaat u implementeren?

Met deze sjabloon implementeert u een verificatieregel Service Bus voor een naamruimte en messaging entiteit (in dit geval een wachtrij).

Deze sjabloon worden [Gedeelde toegang handtekening (SAS)](service-bus-sas-overview.md) gebruikt voor verificatie. SAS kan toepassingen worden geverifieerd bij een Service Bus met een toegangstoets geconfigureerd op de naamruimte of messaging entiteit (wachtrij of onderwerp) waaraan specifieke rechten zijn gekoppeld. Vervolgens kunt u deze sleutel voor het genereren van een token SAS die clients op hun beurt gebruiken kunnen om Service Bus te verifiëren.

De installatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren op Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. Een parameter voor de waarden die verschillen op basis van het project dat u wilt implementeren of op basis van de omgeving die u levert, moet u definiëren. Definieert parameters voor de waarden die altijd hetzelfde zal blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

De naam van de naamruimte Service Bus te maken.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

De naam van de verificatieregel voor de naamruimte.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

De naam van de wachtrij in de naamruimte Service Bus.

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

Een standaard Service Bus-naamruimte van het type **Messaging**en een verificatieregel Service Bus voor de naamruimte en entiteit gemaakt.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gemaakt en geïmplementeerd met Azure Resource Manager bronnen, meer informatie over deze resources beheren door deze artikelen te bekijken:

- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Resources met de Explorer Service Bus Bus-Service beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Bus service verificatie en autorisatie](service-bus-authentication-and-authorization.md)

  [Azure Resource Manager sjablonen ontwerpen]: ../resource-group-authoring-templates.md
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus auth regelsjabloon]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
