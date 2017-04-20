<properties
   pageTitle="Beschikbaarheid en de schaal in Azure Resource Manager Templates | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core zelfstudie"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Beschikbaarheid en de schaal in Azure Resource Manager-sjablonen

Beschikbaarheid en schaal verwijzen naar uptime en de mogelijkheid om te voldoen aan de vraag. Als een toepassing een 99,9% van de tijd moet, moet deze een architectuur waarmee meerdere gelijktijdige compute resources hebt. In plaats van één website, bijvoorbeeld, bevat een configuratie met een hogere mate van beschikbaarheid meerdere exemplaren van dezelfde site, met technologie voor deze verdeling. In deze configuratie fungeert kan één exemplaar van de toepassing worden genomen omlaag voor onderhoud, terwijl de resterende blijven functioneren. Schaal verwijst daarentegen naar een toepassingen kunnen dienen vraag. Met een lading kan evenwichtige toepassing, toevoegen of verwijderen van exemplaren van de groep een toepassing schalen om vraag te voldoen.

Dit document wordt beschreven hoe de voorbeelddistributie muziek winkel is geconfigureerd voor de beschikbaarheid en de schaal. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor de beste ervaring, een exemplaar van de oplossing voor uw abonnement Azure en werk samen met de sjabloon Azure Resource Manager vooraf te implementeren. De volledige sjabloon vindt u hier – [Muziek winkel implementatie op Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Beschikbaarheid instellen

Het instellen van een beschikbaarheid omvat logisch Azure virtuele Machines tussen fysieke hosts en andere infrastructurele onderdelen zoals voedingen en fysieke netwerkhardware. Beschikbaarheid sets zorgen ervoor dat tijdens het onderhoud, apparaatfout of andere inactieve tijd, niet alle virtuele machines worden gedaan. Het instellen van een beschikbaarheid kunnen worden toegevoegd aan een sjabloon Azure Resource Manager met behulp van de Visual Studio toevoegen Wizard Nieuwe bron of geldige JSON invoegen in een sjabloon.

Volg deze koppeling om de JSON-voorbeeld in de sjabloon Resource Manager – [Beschikbaar](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Het instellen van een beschikbaarheid is gedeclareerd als een eigenschap van de bron van een virtuele Machine. 

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – [koppeling beschikbaar met virtuele Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313)wilt zien.


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
De beschikbaarheid van gezien vanaf de portal Azure instellen. Elke virtuele machine en gedetailleerde informatie over de configuratie worden hier beschreven.

![Beschikbaarheid instellen](./media/virtual-machines-linux-dotnet-core/aset.png)

Zie voor uitgebreide informatie over de beschikbaarheid van Sets, [beschikbaarheid beheren van virtuele machines](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Netwerk-taakverdeling

Overwegende dat een set beschikbaarheid fouttolerantie van de toepassing biedt, wordt een load balancer veel exemplaren van de toepassing beschikbaar op één adres. Meerdere exemplaren van een toepassing kunnen worden gehost op veel virtuele computers een verbinding met een load balancer. Als de toepassing wordt geopend, routes de taakverdeling de inkomende aanvraag over de aangesloten leden. Een taakverdeling kan worden toegevoegd met de Visual Studio toevoegen Wizard Nieuwe bron of opgemaakt door goed JSON-bron in de sjabloon Azure Resource Manager.

Volg deze koppeling als de JSON-voorbeeld binnen de sjabloon Resource Manager – [Netwerk-taakverdeling](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208)wilt zien.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Omdat de voorbeeldtoepassing is toegankelijk via het internet met een openbaar IP-adres, is dit adres gekoppeld aan de taakverdeling. 

Volg deze koppeling als u wilt zien van het monster JSON binnen de sjabloon Resource Manager – [Network Load Balancer-koppeling met het openbare IP-adres](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Vanaf de portal Azure ziet de network load balancer overzicht u de koppeling met het openbare IP-adres.

![Netwerk-taakverdeling](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Load Balancer-regel

Wanneer u een load balancer, zijn regels die bepalen hoe verkeer wordt verdeeld over de beoogde bronnen geconfigureerd. Met de voorbeeldtoepassing muziek winkel verkeer binnenkomt op poort 80 van het openbare IP-adres en wordt verspreid via poort 80 van alle virtuele machines. 

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – de [Load Balancer regel](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270)wilt zien.


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Een weergave van de network load balancer regel vanaf de portal.

![Network Load Balancer-regel](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Load Balancer sonde

De taakverdeling moet ook controleren elke virtuele machine zodat alleen voor die systemen worden aanvragen verzonden. Deze controle vindt plaats door constante scannen van een vooraf gedefinieerde poort. De implementatie van de winkel is geconfigureerd voor poort 80 op alle opgenomen virtuele machines probe. 

Volg deze koppeling als de JSON-voorbeeld binnen de sjabloon Resource Manager – [Load Balancer Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257)wilt zien.


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

De load balancer sonde gezien vanaf de portal Azure.

![Network Load Balancer sonde](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>Regels van inkomende NAT

Wanneer u een Load Balancer, regels moeten worden op de plaats waar die niet load balanced toegang verlenen tot elke virtuele Machine. Bijvoorbeeld wanneer u een SSH-verbinding met elke virtuele machine, dit verkeer niet verdeeld moet worden, in plaats daarvan een vooraf bepaald pad moet worden geconfigureerd. vooraf bepaalde paden worden geconfigureerd met behulp van een resource NAT regel voor binnenkomende verbindingen. Met behulp van deze bron, worden inkomende communicatie toegewezen aan afzonderlijke virtuele Machines. 

Met de toepassing van de winkel, is een poort 5000 vanaf toegewezen aan poort 22 op elke virtuele Machine voor SSH toegang. De `copyindex()` functie wordt gebruikt om de binnenkomende poort te verhogen, dat de tweede virtuele Machine ontvangt een inkomende poort van 5001, de derde 5002 enzovoort.

Volg deze koppeling om de JSON-voorbeeld in de sjabloon Resource Manager – [Binnenkomende NAT regels](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Een voorbeeld van de inkomende NAT regel zoals weergegeven in de portal Azure. Een SSH NAT regel gemaakt voor elke virtuele machine in de installatie.

![Inkomende NAT regel](./media/virtual-machines-linux-dotnet-core/natrule.png)

Zie [Netwerktaakverdeling voor Azure infrastructuurservices](./virtual-machines-linux-load-balance.md)voor gedetailleerde informatie over de taakverdelingsvoorziening Azure netwerk.

## <a name="deploy-multiple-vms"></a>Meerdere VMs implementeren

Ten slotte zijn een beschikbaarheid instellen of Load Balancer effectief functioneren, meerdere virtuele machines. Meerdere VMs kunnen worden geïmplementeerd met behulp van de functie Azure Resource Manager sjabloon kopiëren. Met de functie kopiëren, is het niet nodig is voor het definiëren van een eindig aantal virtuele Machines, maar deze waarde dynamisch ten tijde van de implementatie kan worden geleverd. De functie kopiëren neemt het aantal exemplaren gemaakt en de grepen voor het implementeren van het juiste aantal virtuele machines en de bijbehorende bronnen.

In de sjabloon muziek winkel monster een parameter gedefinieerd waarmee een aantal exemplaar. Dit nummer wordt gebruikt in de sjabloon bij het maken van virtuele machines en verwante bronnen.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Op de bron van de virtuele Machine, wordt de lus kopie een naam en het aantal exemplaren parameter gebruikt om de resulterende aantal gegeven.

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – [Kopieerfunctie virtuele Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300)wilt zien. 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

De huidige iteratie van de kopieerfunctie is toegankelijk via de `copyIndex()` functie. De waarde van de index-functie kopiëren kan worden gebruikt als naam voor virtuele machines en andere bronnen. Bijvoorbeeld, als twee exemplaren van een virtuele machine worden gebruikt, moeten ze verschillende namen. De `copyIndex()` functie voor het maken van een unieke naam als deel van de naam van de virtuele machine kan worden gebruikt. Een voorbeeld van de `copyindex()` functie die wordt gebruikt voor de naamgeving van toepassing kan worden bekeken in de bron van de virtuele Machine. Hier, de computernaam is een samenvoeging van de `vmName` parameter, en de `copyIndex()` functie. 

Volg deze koppeling om de JSON-voorbeeld in de sjabloon Resource Manager – [De functie Index kopiëren](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

De `copyIndex` gebruikt verschillende keren in de winkel-voorbeeldsjabloon. Bronnen en het gebruik van functies `copyIndex` bevatten alles wat specifiek is voor één exemplaar van de virtuele machine zoals netwerk-interface, load balancer-regels, en alle functies afhankelijk. 

Zie [meerdere instanties maken van bronnen in Azure Resource Manager](../resource-group-create-multiple.md)voor meer informatie over de functie kopiëren.

## <a name="next-step"></a>Volgende stap

<hr>

[Stap 4 - installatie van toepassingen met Azure Resource Manager-sjablonen](./virtual-machines-linux-dotnet-core-5-app-deployment.md)