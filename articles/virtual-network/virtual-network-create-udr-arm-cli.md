<properties 
   pageTitle="Routering beheren en gebruiken van virtuele apparaten in Resource Manager, met behulp van de CLI Azure | Microsoft Azure"
   description="Meer informatie over het beheren van Routering en het gebruik van virtuele apparaten met behulp van de CLI Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Maak Routes (UDR) in de CLI Azure gedefinieerd door de gebruiker

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook de [UDRs in de klassieke implementatiemodel maken](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Het onderstaande voorbeeld Azure CLI opdrachten verwachten een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u wilt dat de opdrachten uitvoeren zoals deze worden weergegeven in dit document, eerst de testomgeving bouwen door het implementeren van [deze sjabloon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klik op **Deploy om Azure**standaardparameterwaarden indien nodig vervangen en volg de instructies in de portal.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor het subnet-front-end maken
Als u wilt maken in de tabel en de route die nodig zijn voor de front-end subnet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

3. Voer de **`azure network route-table create`** de opdracht voor het maken van een tabel voor het subnet-front-end.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Uitvoer:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parameters:
    - **-g (of--resource-groep)**. De naam van de bronnengroep waarin de UDR wordt gemaakt. Voor ons scenario *TestRG*.
    - **-l (of--locatie)**. Azure regio waar de nieuwe UDR worden gemaakt. Voor ons scenario *westus*.
    - **-n (of -naam)**. De naam voor de nieuwe UDR. Voor ons scenario *UDR FrontEnd*.

4. Voer de **`azure network route-table route create`** de opdracht voor het maken van een route in de tabel hierboven is gemaakt voor het verzenden van alle verkeer dat bestemd is voor de back-end subnet (192.168.2.0/24) **FW1** VM (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Uitvoer:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parameters:
    - **-r (of--route tabelnaam)**. De naam van de tabel waar de route wordt toegevoegd. Voor ons scenario *UDR FrontEnd*.
    - **-een (of--- adresprefix)**. Voorvoegsel van het subnet waarop pakketten bestemd zijn voor het adres. Voor ons scenario *192.168.2.0/24*.
    - **-y (of--volgende-hop-type)**. Type object verkeer ontvangt. Mogelijke waarden zijn *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*of *geen*.
    - **-p (of--volgende-hop-ip-adres**). IP-adres voor de volgende hop. Voor ons scenario *192.168.0.4*.

5. Voer het **`azure network vnet subnet set`** opdracht aan de routetabel dat hierboven is gemaakt met het **front-end** subnet koppelen.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Uitvoer:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parameters:
    - **-e (of--vnet-naam)**. De naam van de VNet waarin het subnet bevindt. Voor ons scenario *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor de back-end-subnet maken
Als u wilt maken in de routetabel en de route die nodig zijn voor de back-end-subnet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

1. Voer de **`azure network route-table create`** de opdracht voor het maken van een tabel voor het subnet van de back-end.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Voer de **`azure network route-table route create`** de opdracht voor het maken van een route in de tabel hierboven is gemaakt voor het verzenden van alle verkeer dat bestemd is voor het subnet-front-end (192.168.1.0/24) **FW1** VM (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Voer het **`azure network vnet subnet set`** opdracht aan de routetabel dat hierboven is gemaakt met de **back-end** -subnet koppelen.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>FW1 IP-doorsturen inschakelen
Volg de onderstaande stappen zodat IP forwarding in de Netwerkinterfacekaart die wordt gebruikt door **FW1**.

1. Voer het **`azure network nic show`** opdracht en Let op de waarde voor het **doorsturen via IP inschakelen**. Deze moet worden ingesteld op *false*.

        azure network nic show -g TestRG -n NICFW1

    Uitvoer:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Voer het **`azure network nic set`** opdracht doorsturen via IP inschakelen.

        azure network nic set -g TestRG -n NICFW1 -f true

    Uitvoer:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parameters:

    - **-f (of--enable-ip-doorstuuradres)**. *waar* of *Onwaar*.
