<properties
   pageTitle="Resources met Azure Resource Manager Templates implementeren berekenen | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core zelfstudie"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Toepassingsarchitectuur met Azure Resource Manager-sjablonen

Bij het ontwikkelen van een bronnenbeheerder Azure-implementatie moeten compute vereisten worden toegewezen aan Azure bronnen en services. Als een toepassing uit verschillende HTTP-eindpunten, een database en een gegevens in de cache van de service bestaat, de Azure bronnen die host elk van deze onderdelen moet worden gerationaliseerd. De voorbeeldtoepassing muziek archief bevat bijvoorbeeld een webtoepassing die wordt gehost op een virtuele machine en een SQL-database wordt gehost in Azure SQL-database. 

Dit document details over hoe de muziek winkel compute-bronnen in de sjabloon voorbeeldgegevens Azure Resource Manager zijn geconfigureerd. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor de beste ervaring, een exemplaar van de oplossing voor uw abonnement Azure en werk samen met de sjabloon Azure Resource Manager vooraf te implementeren. De volledige sjabloon vindt u hier – [Muziek winkel Deployment in Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="virtual-machine"></a>Virtuele Machine

De winkel-toepassing bevat een webtoepassing waar klanten kunnen bladeren en muziek. Er zijn verschillende Azure services die u van webtoepassingen, bijvoorbeeld hosten kunnen, wordt een virtuele Machine gebruikt. Met de sjabloon voorbeeldgegevens muziek archief wordt geïmplementeerd met een virtuele machine, een webserver installeren en de website muziek archief geïnstalleerd en geconfigureerd. Voor dit artikel, wordt de implementatie van de virtuele machine beschreven. De configuratie van de webserver en de toepassing wordt besproken in een later artikel.

Een virtuele machine kan worden toegevoegd aan een sjabloon met de wizard Visual Studio Add New Resource of door middel van geldige JSON in de sjabloon voor de implementatie. Bij de implementatie van een virtuele machine, zijn er ook verschillende verwante bronnen nodig. Als u Visual Studio om de sjabloon te maken, worden deze bronnen gemaakt. Als de sjabloon handmatig maken, deze bronnen moeten worden ingevoegd en geconfigureerd.

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – [JSON virtuele Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285)wilt zien.

```none
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Eenmaal geïmplementeerd, kunnen u de eigenschappen van de virtuele machine in Azure portal bekijken.

![Virtuele Machine](./media/virtual-machines-windows-dotnet-core/vm-win.png)

## <a name="storage-account"></a>Opslag Account

Opslag-accounts hebben veel mogelijkheden en opties voor opslag. Een opslag-account bevat voor de context van Azure Virtual machines, de virtuele harde schijven van de virtuele machine en alle gegevensschijven meer. Het monster muziek winkel bevat één opslag account voor de virtuele harde schijf van elke virtuele machine in de installatie. 

Volg deze koppeling om de JSON-voorbeeld in de sjabloon Resource Manager – [Opslag Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Een opslag-account is koppelen aan een virtuele machine in de Resource Manager sjabloondeclaratie van de virtuele machine. 

Volg deze koppeling om het monster JSON binnen de sjabloon Resource Manager – [virtuele Machine en opslag Account koppeling](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321)zien.

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Na de implementatie, kan de opslag-account in de Azure portal worden weergegeven.

![Opslag Account](./media/virtual-machines-windows-dotnet-core/storacct-win.png)

Op naar de container opslag account blob, kan de virtuele harde schijf bestand voor elke virtuele machine geïmplementeerd met de sjabloon worden gezien.

![Virtuele harde schijven](./media/virtual-machines-windows-dotnet-core/vhd-win.png)

Zie voor meer informatie over Azure opslag [opslag Azure-documentatie](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtueel netwerk

Als een virtuele machine is vereist voor interne netwerken, zoals de mogelijkheid om te communiceren met andere virtuele machines en Azure bronnen, is een virtueel netwerk Azure vereist.  Een virtueel netwerk maakt geen de virtuele machine toegankelijk via het internet. Openbare verbinding moet een openbaar IP-adres, die later in deze reeks wordt besproken.

Volg deze koppeling om de JSON-voorbeeld in de sjabloon Resource Manager – [virtuele netwerk en de subnetten](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
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
    ]
  }
}
```

Vanuit de portal Azure is het virtuele netwerk ziet eruit als in de volgende afbeelding. U ziet dat alle virtuele machines die worden geïmplementeerd met de sjabloon zijn gekoppeld aan het virtuele netwerk.

![Virtueel netwerk](./media/virtual-machines-windows-dotnet-core/vnet-win.png)

## <a name="network-interface"></a>Netwerk-Interface

 Een virtuele machine verbinding een netwerkinterface met een virtueel netwerk, meer in het bijzonder aan een subnet dat is gedefinieerd in het virtuele netwerk. 
 
 Volg deze koppeling om de JSON-voorbeeld in de sjabloon Resource Manager – [Netwerk-Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

De bron van elke virtuele machine bevat een netwerkprofiel. De netwerkinterface is gekoppeld aan de virtuele machine in dit profiel.  

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – [Netwerkprofiel virtuele Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330)wilt zien.


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Vanuit de portal Azure lijkt de netwerkinterface op de volgende afbeelding. Het interne IP-adres en de koppeling van de virtuele machine kunnen worden gezien op de netwerkbron interface.

![Netwerk-Interface](./media/virtual-machines-windows-dotnet-core/nic-win.png)

Zie voor meer informatie over virtuele netwerken Azure, [Azure Virtual Network-documentatie](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL-Database

Azure SQL-Database wordt naast een virtuele machine die als host fungeert voor de website van de winkel, geïmplementeerd als host voor de database met muziek. Het voordeel van Azure SQL Database hier is dat een tweede set van virtuele machines niet vereist is en schaal en beschikbaarheid is ingebouwd in de service.

Azure SQL-database kan worden toegevoegd met behulp van de Visual Studio Add New Resource wizard of door een geldige JSON invoegen in een sjabloon. De SQL Server-bron bevat een gebruikersnaam en wachtwoord dat u beheerdersrechten op de SQL-instantie wordt verleend. Ook wordt een SQL-bron firewall toegevoegd. Toepassingen die worden gehost in Azure zijn standaard verbinding maken met de SQL-exemplaar. Die een SQL Server Management studio verbinding maken met de SQL-instantie, de firewall moet worden geconfigureerd zodat externe toepassing. De standaardconfiguratie is om de demo muziek winkel geen probleem. 

Volg deze koppeling als u wilt zien van het monster JSON binnen de sjabloon Resource Manager – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Een weergave van de SQL server en database MusicStore zoals weergegeven in de portal Azure.

![SQL Server](./media/virtual-machines-windows-dotnet-core/sql-win.png)

Zie de [documentatie Azure SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)voor meer informatie over het implementeren van Azure SQL-Database.

## <a name="next-step"></a>Volgende stap

<hr>

[Stap 2 - toegang en beveiliging in Azure Resource Manager-sjablonen](./virtual-machines-windows-dotnet-core-3-access-security.md)
