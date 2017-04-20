<properties
   pageTitle="Resource Manager sjabloon overzicht | Microsoft Azure"
   description="Een stap voor stap scenario van een resource manager sjabloon een basisarchitectuur Azure IaaS inrichten."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Resource Manager sjabloon scenario

Een van de eerste vragen bij het maken van een sjabloon is "how to start?". Een kunt beginnen met een lege sjabloon, na de basisstructuur die is beschreven in [artikel sjabloon ontwerpen](resource-group-authoring-templates.md#template-format), en de bronnen en de gewenste parameters en variabelen toevoegen. Een goed alternatief zou zijn om te beginnen door te gaan via de [quickstart galerie](https://github.com/Azure/azure-quickstart-templates) en zoek naar soortgelijke scenario's die u probeert te maken. U kunt verschillende sjablonen samenvoegen of een bestaande aanpassen aan uw eigen specifieke scenario bewerken. 

Laten we eens een gemeenschappelijke infrastructuur:

* Twee virtuele machines die dezelfde opslag account gebruiken, zijn in bepaalde beschikbaarheid en op hetzelfde subnet van een virtueel netwerk.
* Één Netwerkkaart en VM IP adres voor elke virtuele machine.
* Een taakverdeling met een regel op poort 80 voor taakverdeling

![architectuur](./media/resource-group-overview/arm_arch.png)

In dit onderwerp leidt u door de stappen voor het maken van een sjabloon Resource Manager voor deze infrastructuur. De laatste sjabloon die u maakt is gebaseerd op een Quickstart sjabloon [2 VMs in een Load Balancer en load-balancing regels](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/)genoemd.

Maar dat is veel te bouwen in één keer, dus laten we eerst een opslag account maken en deze implementeren. Nadat u hebt onder de knie het opslag-account maakt, wordt u de andere resources toevoegen en opnieuw de sjabloon om de infrastructuur te implementeren.

>[AZURE.NOTE] U kunt elk type editor gebruiken bij het maken van de sjabloon. Visual Studio biedt tools voor het vereenvoudigen van de sjabloon, maar u hoeft geen Visual Studio voor het voltooien van deze zelfstudie. Zie voor een zelfstudie over het maken van een Web App en de SQL-Database-implementatie met behulp van Visual Studio, [maken en implementeren van Azure resourcegroepen via Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Maak de sjabloon Resource Manager

De sjabloon is een JSON-bestand waarin alle bronnen die u gaat implementeren. Ook kunt u de parameters definiëren die tijdens de implementatie worden opgegeven variabelen die worden gemaakt op basis van andere waarden en expressies en uitgangen van de implementatie. 

Laten we beginnen met de eenvoudigste sjabloon:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Sla dit bestand op als **azuredeploy.json** (Houd er rekening mee dat de sjabloon kan elke gewenste naam hebben, maar dat het moet een json-bestand).

## <a name="create-a-storage-account"></a>Maak een account voor opslag
Toevoegen in de sectie **resources** een object dat de account van de opslag, zoals hieronder wordt weergegeven. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

U vraagt zich misschien af waar deze eigenschappen en waarden afkomstig zijn uit. De eigenschappen **type**, **naam**, **apiVersion**en **locatie** zijn standaard elementen die beschikbaar voor alle brontypen zijn. U kunt informatie over de algemene elementen in [bronnen](resource-group-authoring-templates.md#resources). **de naam** is ingesteld op een waarde voor de parameter die u tijdens de implementatie en de **locatie** als de locatie die wordt gebruikt door de resourcegroep doorgeven. Bekijken we hoe u **type** - en **apiVersion** in de onderstaande secties bepalen.

De sectie **-Eigenschappen** bevat alle eigenschappen die uniek voor een bepaald brontype zijn. De waarden die u in dit gedeelte exact opgeeft overeenkomen met de bewerking plaatsen uitgevoerd in de REST-API voor het maken van dat resourcetype. Wanneer u een opslag-account maakt, moet u een **accountType**opgeven. Zoals u ziet in de [REST-API voor het maken van een account opslag](https://msdn.microsoft.com/library/azure/mt163564.aspx) dat de sectie-eigenschappen van de REST-bewerking ook een eigenschap **accountType bevat** en de toegestane waarden zijn gedocumenteerd. In dit voorbeeld wordt het rekeningsoort is ingesteld op **Standard_LRS**, maar kan u een andere waarde opgeven of gebruikers het rekeningsoort als parameter doorgeeft.

Nu we gaan terug naar de sectie **parameters** en Zie hoe u de naam van de account voor de opslag definiëren. U kunt meer informatie over het gebruik van parameters op [Parameters](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Hier kunt u een parameter van het type tekenreeks waarin de naam van de account opslag gedefinieerd. De waarde voor deze parameter krijgt tijdens de sjabloonimplementatie.

## <a name="deploying-the-template"></a>De sjabloon distribueren
We hebben een volledige sjabloon voor het maken van een nieuwe account voor opslag. Zoals u, is de sjabloon opgeslagen in het bestand **azuredeploy.json** :

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Er zijn een aantal manieren voor de implementatie van een sjabloon, zoals u in de [implementatie van de Resource, artikel zien kunt](resource-group-template-deploy.md). Voor de implementatie van de sjabloon met Azure PowerShell gebruiken:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Of voor de implementatie van de sjabloon met Azure CLI gebruiken:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

U bent nu de trots eigenaar van een account voor opslag.

De volgende stappen worden alle resources die nodig zijn voor de implementatie van de architectuur wordt beschreven in het begin van deze zelfstudie. Voegt u deze bronnen in dezelfde sjabloon die u hebt gewerkt.

## <a name="availability-set"></a>Beschikbaarheid instellen
Na de definitie voor de opslag, Voeg een wanneer ingesteld voor de virtuele machines. In dit geval zijn er geen extra eigenschappen vereist, zodat de definitie vrij eenvoudig is. Zie de [REST API voor het maken van een beschikbaar stellen](https://msdn.microsoft.com/library/azure/mt163607.aspx) voor de volledige eigenschappen van sectie in het geval u wilt de update domein tellen en fouttolerantie aantal waarden definiëren.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

U ziet dat de **naam** is ingesteld op de waarde van een variabele. De naam van de set beschikbaar is voor deze sjabloon, in een paar verschillende plaatsen nodig. De sjabloon kunt u gemakkelijker onderhouden door die waarde eenmaal definiëren en gebruiken in meerdere plaatsen.

De waarde die u voor het **type opgeeft** bevat zowel de bron-provider en het resourcetype. Voor beschikbaarheid, de resource-provider is **Microsoft.Compute** en het brontype is **availabilitySets**. De lijst met beschikbare bron providers kunt u opvragen door de volgende PowerShell-opdracht uit te voeren:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Of als u met behulp van de CLI Azure, kunt u de volgende opdracht uitvoeren:
```
    azure provider list
```
Gezien het feit dat in dit onderwerp u met de opslag, de virtuele machines en een virtueel netwerk maakt, werkt u met:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Als u wilt zien welke resource voor een bepaalde aanbieder, kunt u de volgende PowerShell-opdracht uitvoeren:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Of voor Azure CLI, met de volgende opdracht wordt de beschikbare typen JSON-indeling en het in een bestand opslaan.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

**AvailabilitySets** moet u zien als een van de typen in de **Microsoft.Compute**. De volledige naam van het type is **Microsoft.Compute/availabilitySets**. U kunt de naam van de resource-type voor de resources in de sjabloon u bepalen.

## <a name="public-ip"></a>Openbare IP
Een openbaar IP-adres definiëren. Bekijk opnieuw de [REST API voor het openbare IP-adressen](https://msdn.microsoft.com/library/azure/mt163590.aspx) voor de eigenschappen in te stellen.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Methode voor de toewijzing is ingesteld op een **dynamische** , maar kan u deze instelt op de waarde die u nodig hebt of het accepteren van een waarde voor de parameter. U kunt gebruikers van uw sjabloon op te geven voor het domein naamlabel een waarde hebt ingeschakeld.

Nu gaan we kijken hoe het vaststellen van de **apiVersion**. De waarde die u opgeeft is gewoon komt overeen met de versie van de REST-API die u gebruiken wilt bij het maken van de resource. Zo kunt u de REST API-documentatie voor dat resourcetype bekijken. Of u kunt de volgende PowerShell-opdracht voor een bepaald type uitvoeren.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Dat de volgende waarden als resultaat:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Overzicht van de API-versies met Azure CLI uitvoeren dezelfde **azure-provider weergeven** opdracht eerder weergegeven.

Wanneer u een nieuwe sjabloon maakt, kiest u de meest recente versie van de API.

## <a name="virtual-network-and-subnet"></a>Virtueel netwerk en subnet
Maak een virtueel netwerk met één subnet. Bekijk de [REST API voor virtuele netwerken](https://msdn.microsoft.com/library/azure/mt163661.aspx) voor de eigenschappen in te stellen.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Taakverdeling
Nu maakt u een externe tegenoverliggende taakverdeling. Omdat deze taakverdeling het openbare IP-adres gebruikt, moet u een afhankelijkheid van het openbare IP-adres in de sectie **dependsOn** declareren. Dit betekent dat de taakverdeling niet ophalen ingezet tot het openbare IP-adres implementatie is voltooid. Zonder deze afhankelijkheid is gedefinieerd, ontvangt u een fout omdat de Resource Manager zal proberen de resources tegelijkertijd te implementeren en probeert in te stellen van de verdeling van de belasting op openbare IP-adres dat nog niet bestaat. 

Ook maakt u een back-end-adresgroep, een aantal binnenkomende NAT regels voor RDP in het VMs en een load balancing regel met een sonde tcp op poort 80 in de definitie van deze resource. De [REST API voor taakverdeling](https://msdn.microsoft.com/library/azure/mt163574.aspx) afhandeling voor alle eigenschappen.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Netwerk-interface
Maakt u 2 netwerkinterfaces, één voor elke VM. In plaats van dubbele posten voor de netwerkinterfaces wilt opnemen, kunt u de [functie copyIndex()](resource-group-create-multiple.md) doorlopen de kopie lus (hierna aangeduid als nicLoop) en maakt het aantal netwerkinterfaces als omschreven in de `numberOfInstances` variabelen. Maken van het virtuele netwerk en de verdeling van de belasting is afhankelijk van de netwerkinterface. Het subnet dat is gedefinieerd in het virtuele netwerk maken en de load balancer-id wordt gebruikt om de load balancer-adresgroep en de binnenkomende NAT regels te configureren.
Bekijk de [REST API voor netwerkinterfaces](https://msdn.microsoft.com/library/azure/mt163668.aspx) voor alle eigenschappen.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Virtuele machine
Maakt u 2 virtuele machines met behulp van de functie copyIndex(), zoals in het maken van de [Netwerkinterfaces](#network-interface).
Het maken van de VM is afhankelijk van de account van de opslag, netwerk-interface en de beschikbaarheid. Deze VM wordt gemaakt van een marketplace-afbeelding, zoals gedefinieerd in de `storageProfile` eigenschap - `imageReference` wordt gebruikt voor het definiëren van de uitgever van de afbeelding, aanbieding, sku en versie. Ten slotte is een diagnostisch profiel geconfigureerd voor het inschakelen van diagnostische gegevens voor de VM. 

Volg de relevante eigenschappen voor een afbeelding marketplace vindt de artikelen [Linux virtuele machine afbeeldingen selecteert u](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) of [Windows-installatiekopieën voor virtuele machine](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Voor afbeeldingen die zijn gepubliceerd door **3de leveranciers**, moet u een andere eigenschap met de naam `plan`. Een voorbeeld vindt u in [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) uit de galerie quickstart. 

U klaar bent met het definiëren van de bronnen voor de sjabloon.

## <a name="parameters"></a>Parameters

In de sectie parameters de waarden die kunnen worden opgegeven bij het implementeren van de sjabloon te definiëren. Alleen parameters definiëren voor waarden waarvan u denkt dat moet worden gewijzigd tijdens de implementatie. U kunt een standaardwaarde opgeven voor een parameter die wordt gebruikt als een is niet beschikbaar tijdens de implementatie. Ook kunt u de toegestane waarden voor de parameter **imageSKU** zoals.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Variabelen

In de sectie variabelen kunt u waarden die in meer dan één plaats in de sjabloon worden gebruikt of die zijn opgebouwd uit andere expressies of variabelen definiëren. Variabelen worden vaak gebruikt voor het vereenvoudigen van de syntaxis van de sjabloon.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

U kunt de sjabloon hebt voltooid! U kunt de sjabloon met de volledige sjabloon in de [Galerie quickstart](https://github.com/Azure/azure-quickstart-templates) onder [2 VMs met de belasting voor documentconversies en de verdeling van regels sjabloon laden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)vergelijken. De sjabloon mogelijk iets anders op basis van de verschillende versienummers. 

U kunt de sjabloon opnieuw implementeren met behulp van dezelfde opdrachten die u hebt gebruikt bij het implementeren van de opslag-account. U hoeft niet de opslag verwijderen alvorens opnieuw te implementeren omdat de Resource Manager slaat opnieuw maken van bronnen die al bestaan en niet zijn gewijzigd.

## <a name="next-steps"></a>Volgende stappen

- [Azure Resource Manager sjabloon Visualizer (ARMViz)](http://armviz.io/#/) is een uitstekend hulpmiddel visualiseren ARM sjablonen, zodra deze is te groot om te begrijpen alleen uit het lezen van het bestand json.
- Zie voor meer informatie over de structuur van een sjabloon, [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).
- Zie voor meer informatie over het implementeren van een sjabloon [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md)
