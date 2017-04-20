<properties
    pageTitle="Service Bus resources met behulp van bronbeheer Azure sjablonen maken | Microsoft Azure"
    description="Azure Resource Manager-sjablonen gebruiken voor het automatiseren van het maken van de Bus-Service"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Service Bus resources met behulp van bronbeheer Azure sjablonen maken

Dit artikel wordt beschreven hoe u kunt maken en implementeren van Bus-Service en gebeurtenis Hubs bronnen met behulp van sjablonen, PowerShell en de resource Bus Service provider Azure Resource Manager.

Azure Resource Manager-sjablonen kunt u de middelen te implementeren voor een oplossing en voor het opgeven van parameters en variabelen waarmee u waarden voor verschillende omgevingen invoer opgeven. De sjabloon bestaat uit JSON en expressies waarmee u waarden voor de implementatie samenstellen. Zie voor gedetailleerde informatie over het schrijven van sjablonen Azure Resource Manager en een bespreking van de sjabloonindeling [sjablonen ontwerpen Azure Resource Manager](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] De voorbeelden in dit artikel hoe Azure Resource Manager gebruiken voor het maken van een naamruimte Service Bus en messaging entiteit (wachtrij). Andere voorbeelden van de sjabloon, gaat u naar de [Azure Quickstart sjablonen galerie][] en zoek naar 'Service Bus'.

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Sjablonen voor Bus-service en gebeurtenis Hubs Resource Manager

Deze Bus-Service en gebeurtenis Hubs Azure Resource Manager-sjablonen zijn beschikbaar voor het downloaden en uitvoeren. Klik op de volgende koppelingen voor meer informatie over elke, met koppelingen naar sjablonen op GitHub: 

- [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
- [De naamruimte voor een Service Bus met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
- [Een Service Bus naamruimte maken met het onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
- [Een naamruimte Service Bus met wachtrij en autorisatie-regel maken](service-bus-resource-manager-namespace-auth-rule.md)
- [Een gebeurtenis Hubs naamruimte maken met een groep gebeurtenis Hub en consument](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Met PowerShell implementeren

De volgende procedure wordt beschreven hoe u een Azure Resource Manager-sjabloon die wordt gemaakt van de **naamruimte-tier-Service Bus** en een wachtrij binnen die naamruimte implementeren met PowerShell. In dit voorbeeld is gebaseerd op de sjabloon voor [een Service Bus naamruimte aan wachtrij maken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . De werkstroom bij benadering is als volgt:

1. PowerShell te installeren.
2. Maak de sjabloon en eventueel een parameterbestand.
2. In PowerShell, log in op uw account Azure.
3. Een nieuwe resourcegroep maken als deze nog niet bestaat.
4. De implementatie testen.
5. Indien gewenst kunt u de distributie-modus.
6. De sjabloon distribueren.

Zie voor volledige informatie over het distribueren van sjablonen Azure Resource Manager [bronnen distribueren met sjablonen Azure Resource Manager][].

### <a name="install-powershell"></a>PowerShell installeren

Azure PowerShell installeren door de aanwijzingen in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

### <a name="create-a-template"></a>Een sjabloon maken

Klonen of de sjabloon [201-servicebus-maken-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) van GitHub te kopiëren:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Maak een parameterbestand (optioneel)

Een optionele parameters als bestand wilt gebruiken, Kopieer het bestand [201-servicebus-maken-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) . Vervang de waarde van `serviceBusNamespaceName` met de naam van de Service Bus-naamruimte die u wilt maken in deze installatie en vervang de waarde van `serviceBusQueueName` met de naam van de wachtrij die u wilt maken. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Zie voor meer informatie het onderwerp [parameterbestand](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Log in op Azure en stelt u het abonnement Azure

Voer de volgende opdracht vanaf een PowerShell-prompt:

```
Login-AzureRmAccount
```

U wordt gevraagd aan te melden bij uw account Azure. Voer de volgende opdracht om de beschikbare abonnementen weergeven na aanmelding.

```
Get-AzureRMSubscription
```

Met deze opdracht geeft als resultaat een lijst met beschikbare Azure abonnementen. Kies een abonnement voor de huidige sessie door de volgende opdracht uit te voeren. Vervangen `<YourSubscriptionId>` met de GUID voor de Azure abonnement dat u wilt gebruiken.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>De resourcegroep instellen

Als u niet een bestaande resourcegroep, een nieuwe resourcegroep maken met de opdracht **Nieuw AzureRmResourceGroup** . Geef de naam van de resourcegroep en de locatie die u wilt gebruiken. Bijvoorbeeld:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Als dit lukt, wordt een overzicht van de nieuwe resourcegroep wordt weergegeven.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>De implementatie testen

Valideren van uw distributie door de `Test-AzureRmResourceGroupDeployment` cmdlet. Bij het testen van de implementatie bieden parameters precies zoals u zou doen bij het uitvoeren van de implementatie.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Maken de implementatie

U maakt de nieuwe implementatie, voeren de `New-AzureRmResourceGroupDeployment` opdracht en de benodigde parameters wanneer u wordt gevraagd. De parameters omvatten een naam voor de implementatie, de naam van de resourcegroep en het pad of de URL naar het sjabloonbestand. Als de parameter **Mode** niet is opgegeven, wordt de standaardwaarde van **incrementele** gebruikt. Zie voor meer informatie, [incrementele en volledige installaties](../resource-group-template-deploy.md#incremental-and-complete-deployments).

De volgende opdracht vraagt u om de drie parameters in het venster PowerShell:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Als u een bestand in plaats daarvan, gebruik de volgende opdracht.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

U kunt in line parameters wanneer u de implementatie-cmdlet uitvoert. De opdracht is als volgt:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Om een [volledige](../resource-group-template-deploy.md#incremental-and-complete-deployments) implementatie uitvoert, stelt u de parameter **Mode** op **voltooid**:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Controleer of de implementatie

Als de bronnen worden geïmplementeerd, wordt een samenvatting van de implementatie in het venster PowerShell weergegeven:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Volgende stappen

U hebt nu de opdrachten voor het implementeren van een sjabloon Azure Resource Manager en basiswerkstroom gezien. Ga naar de volgende koppelingen voor meer informatie:

- [Azure Resource Manager-overzicht][]
- [Resources met Azure Resource Manager templates implementeren][]
- [Sjablonen ontwerpen](../resource-group-authoring-templates.md)


[Azure Resource Manager-overzicht]: ../resource-group-overview.md
[Resources met Azure Resource Manager templates implementeren]: ../resource-group-template-deploy.md
[Azure Quickstart sjablonen Galerij]: https://azure.microsoft.com/documentation/templates/?term=service+bus