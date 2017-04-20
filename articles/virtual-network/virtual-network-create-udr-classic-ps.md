<properties 
   pageTitle="Routering beheren en gebruiken van virtuele apparaten met PowerShell in het implementatiemodel klassiek | Microsoft Azure"
   description="Informatie over het bepalen van de routering in VNets met PowerShell in het implementatiemodel klassiek"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Routering beheren en gebruiken met PowerShell virtuele toestellen (klassiek)

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Het monster Azure PowerShell onderstaande opdrachten een eenvoudige omgeving al gemaakt verwachten op basis van het bovenstaande scenario. Als u wilt dat de opdrachten uitvoeren zoals deze worden weergegeven in dit document, maken de omgeving weergegeven in het [maken van een VNet (klassiek) met PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor het subnet-front-end maken
Als u wilt maken in de tabel en de route die nodig zijn voor de front-end subnet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

3. Voer het **`New-AzureRouteTable`** cmdlet een routetabel voor het subnet-front-end maken.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Uitvoer:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Voer het **`Set-AzureRoute`** cmdlet een route wilt maken in de routetabel dat hierboven is gemaakt voor het verzenden van alle verkeer dat bestemd is voor de back-end subnet (192.168.2.0/24) **FW1** VM (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Uitvoer:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Voer het **`Set-AzureSubnetRouteTable`** cmdlet de routetabel koppelen dat hierboven is gemaakt met het subnet **FrontEnd** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor de back-end-subnet maken
Als u wilt maken in de routetabel en de route die nodig zijn voor de back-end-subnet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

3. Voer het **`New-AzureRouteTable`** cmdlet voor het maken van een tabel voor het subnet van de back-end.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Voer het **`Set-AzureRoute`** cmdlet een route wilt maken in de routetabel dat hierboven is gemaakt voor het verzenden van alle verkeer dat bestemd is voor het subnet-front-end (192.168.1.0/24) **FW1** VM (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Voer het **`Set-AzureSubnetRouteTable`** cmdlet de routetabel koppelen dat hierboven is gemaakt met de **back-end** -subnet.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Doorsturen via IP op de VM FW1 inschakelen
Volg de onderstaande stappen zodat IP forwarding in VM FW1.

1. Voer het **`Get-AzureIPForwarding`** cmdlet te controleren van de status van de IP-doorsturen

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Uitvoer:

        Disabled

2. Voer het **`Set-AzureIPForwarding`** opdracht doorsturen via IP voor *FW1* VM inschakelen.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
