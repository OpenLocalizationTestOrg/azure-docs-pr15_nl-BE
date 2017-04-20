<properties
    pageTitle="Een met behulp van de sjabloon voor een Resource Manager-Service Bus-naamruimte maken | Microsoft Azure"
    description="Sjabloon van Azure Resource Manager gebruiken voor het maken van een naamruimte Service Bus"
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
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Een met behulp van een sjabloon Azure Resource Manager-Service Bus-naamruimte maken

In dit artikel wordt beschreven hoe een bronnenbeheerder Azure sjabloon dat een Service Bus-naamruimte van het type **Messaging** met een standaard en eenvoudige SKU maakt gebruiken. Het artikel definieert eveneens de parameters die zijn opgegeven voor het uitvoeren van de implementatie. U kunt deze sjabloon gebruiken voor uw eigen implementaties of aanpassen aan uw eisen voldoen.

Zie [sjablonen ontwerpen Azure Resource Manager][]voor meer informatie over het maken van sjablonen.

Zie voor de volledige sjabloon de [sjabloon voor Service Bus-naamruimte][] op GitHub.

>[AZURE.NOTE] De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor het downloaden en uitvoeren. 
>
>-    [Een gebeurtenis Hubs naamruimte maken met een groep gebeurtenis Hub en consument](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [De naamruimte voor een Service Bus met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
>-    [Een Service Bus naamruimte maken met het onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Een naamruimte Service Bus met wachtrij en autorisatie-regel maken](service-bus-resource-manager-namespace-auth-rule.md)
>
>Om te controleren of de meest recente sjablonen, gaat u naar de galerie met [Azure Quickstart sjablonen][] en zoeken naar Service Bus.

## <a name="what-will-you-deploy"></a>Wat gaat u implementeren?

Met deze sjabloon implementeert u een naamruimte Service Bus met een [Basic, Standard of Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

De installatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren op Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. Een parameter voor de waarden die verschillen op basis van het project dat u wilt implementeren of op basis van de omgeving die u levert, moet u definiëren. Definieert parameters voor de waarden die altijd hetzelfde zal blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd.

Deze sjabloon bepaalt de volgende parameters.

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

### <a name="servicebussku"></a>serviceBusSKU

De naam van de Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) maken.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

De sjabloon bepaalt de waarden die zijn toegestaan voor deze parameter (Basic, Standard of Premium) en een standaardwaarde (standaard) wordt toegewezen als er geen waarde is opgegeven.

Zie voor meer informatie over prijzen van Bus Service [Service Bus prijsstelling en facturering][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

De Service Bus API versie van de sjabloon.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Bronnen te implementeren

### <a name="service-bus-namespace"></a>De naamruimte Service Bus

Een standaard Service Bus-naamruimte van het type **Messaging**maakt.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van de implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gemaakt en geïmplementeerd met Azure Resource Manager bronnen, meer informatie over deze resources beheren door te lezen van deze artikelen:

- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Resources met de Explorer Service Bus Bus-Service beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Azure Resource Manager sjablonen ontwerpen]: ../resource-group-authoring-templates.md
  [Service Bus naamruimte sjabloon]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Bus service prijzen en facturering]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
