<properties
   pageTitle="Toegang en beveiliging in Azure Resource Manager Templates | Microsoft Azure" 
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

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Toegang en beveiliging in Azure Resource Manager-sjablonen

Toepassingen die worden gehost in Azure waarschijnlijk moeten toegang via internet of via een VPN / Route Express verbinding met Azure. Met het voorbeeld van de toepassing muziek winkel is de website beschikbaar gesteld op het internet met een openbaar IP-adres. Met toegang tot stand gebracht, moeten verbindingen met de toepassing en toegang tot de bronnen van de virtuele machine zelf worden beveiligd. Deze beveiliging is voorzien van een netwerkgroep voor beveiliging. 

Dit document details over hoe de toepassing muziek opslaan in de sjabloon voorbeeldgegevens Azure Resource Manager wordt beveiligd. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor de beste ervaring, een exemplaar van de oplossing voor uw abonnement Azure en werk samen met de sjabloon Azure Resource Manager vooraf te implementeren. De volledige sjabloon vindt u hier – [Muziek winkel implementatie op Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


## <a name="public-ip-address"></a>Openbare IP-adres

Om toegang tot een bron Azure, kan een openbare bron van het IP-adres worden gebruikt. Openbare IP-adres kan worden geconfigureerd met een statische of dynamische IP-adres. Als een dynamisch adres wordt gebruikt, en de virtuele machine wordt gestopt en wordt opgeheven, wordt de adressen verwijderd. Wanneer de computer opnieuw wordt gestart, worden deze mogelijk een andere openbare IP-adres toegewezen. Als u wilt voorkomen dat een IP-adres wijzigen, kan een gereserveerd IP-adres worden gebruikt. 

Een openbaar IP-adres kan worden toegevoegd aan een bronnenbeheerder Azure-sjabloon met behulp van de Visual Studio toevoegen Wizard Nieuwe bron, of door een geldige JSON invoegen in een sjabloon. 

Volg deze koppeling als u wilt zien van het monster JSON binnen de sjabloon Resource Manager – [Openbare IP-adres](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Een openbaar IP-adres kunnen worden gekoppeld aan een virtuele netwerkadapter of een Load Balancer. In dit voorbeeld wordt het openbare IP-adres gekoppeld aan de Load Balancer omdat de website muziek archief verdeeld over de verschillende virtuele machines is.

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – [vereniging van openbare IP-adres met taakverdeling](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208)wilt zien.

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

Het openbare IP-adres als gezien vanuit de portal Azure. U ziet dat het openbare IP-adres gekoppeld aan een load balancer en niet een virtuele machine is. Netwerktaakverdeling netwerk worden beschreven in het volgende document van deze reeks.

![Openbare IP-adres](./media/virtual-machines-linux-dotnet-core/pubip.png)

Zie voor meer informatie over Azure openbare IP-adressen, [IP-adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Network Security Group

Als u toegang tot bronnen Azure is vastgesteld, worden deze toegang beperkt. Beveiligde toegang wordt gedaan met behulp van een beveiligingsgroep netwerk voor Azure virtuele machines. Met het voorbeeld van de toepassing muziek winkel kan alle toegang tot de virtuele machine behalve via poort 80 voor HTTP-toegang en poort 22 voor SSH toegang. Een beveiligingsgroep netwerk kunnen worden toegevoegd aan een bronnenbeheerder Azure-sjabloon met behulp van de Visual Studio toevoegen Wizard Nieuwe bron of door geldige JSON invoegen in een sjabloon.

Volg deze koppeling om de JSON-voorbeeld in de sjabloon Resource Manager – [Groep netwerk](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```none
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

In dit voorbeeld is de netwerkgroep koppelen aan het subnetobject in de Virtual Network resource aangegeven. 

Volg deze koppeling om de JSON-voorbeeld binnen de sjabloon Resource Manager – [associatie met Virtual Network netwerk Security Group](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).


```none
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

Hier is de netwerkgroep ziet er als vanuit de portal Azure. U ziet dat een NSG in een subnet en / of netwerk interface kunt koppelen. In dit geval is de NSG gekoppeld aan een subnet. In deze configuratie de binnenkomende regels van toepassing op alle virtuele machines aangesloten op het subnet.

![Network Security Group](./media/virtual-machines-linux-dotnet-core/nsg.png)

Zie [een netwerk-beveiligingsgroep]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/)voor gedetailleerde informatie over netwerk-beveiligingsgroepen.

## <a name="next-step"></a>Volgende stap

<hr>

[Stap 3: de beschikbaarheid en de schaal in Azure Resource Manager-sjablonen](./virtual-machines-linux-dotnet-core-4-availability-scale.md)
