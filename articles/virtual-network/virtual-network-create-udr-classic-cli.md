<properties 
   pageTitle="Routering beheren en gebruiken van virtuele apparaten met behulp van de CLI Azure in het implementatiemodel klassiek | Microsoft Azure"
   description="Informatie over het bepalen van de routering in VNets met behulp van de CLI Azure in het implementatiemodel klassiek"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Routering beheren en gebruiken met behulp van de CLI Azure virtuele toestellen (klassiek)

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel. U kunt ook de [Routering en virtuele toestellen in het implementatiemodel Resource Manager gebruiken](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Het onderstaande voorbeeld Azure CLI opdrachten verwachten een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u wilt dat de opdrachten uitvoeren zoals deze worden weergegeven in dit document, maken de omgeving in [een VNet (klassiek) met behulp van de CLI Azure maken](virtual-networks-create-vnet-classic-cli.md)wordt weergegeven.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor het subnet-front-end maken
Als u wilt maken in de tabel en de route die nodig zijn voor de front-end subnet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

1. Voer het **`azure config mode`** overschakelen naar de klassieke modus.

        azure config mode asm

    Uitvoer:

        info:    New mode is asm

3. Voer de **`azure network route-table create`** de opdracht voor het maken van een tabel voor het subnet-front-end.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Uitvoer:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parameters:
    - **-l (of--locatie)**. Azure regio waar de nieuwe NSG worden gemaakt. Voor ons scenario *westus*.
    - **-n (of -naam)**. De naam voor de nieuwe NSG. Voor ons scenario *NSG FrontEnd*.

4. Voer de **`azure network route-table route set`** de opdracht voor het maken van een route in de tabel hierboven is gemaakt voor het verzenden van alle verkeer dat bestemd is voor de back-end subnet (192.168.2.0/24) **FW1** VM (192.168.0.4).

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Uitvoer:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parameters:
    - **-r (of--route tabelnaam)**. De naam van de tabel waar de route wordt toegevoegd. Voor ons scenario *UDR FrontEnd*.
    - **-een (of--- adresprefix)**. Voorvoegsel van het subnet waarop pakketten bestemd zijn voor het adres. Voor ons scenario *192.168.2.0/24*.
    - **-t (of--volgende-hop-type)**. Type object verkeer ontvangt. Mogelijke waarden zijn *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*of *geen*.
    - **-p (of--volgende-hop-ip-adres**). IP-adres voor de volgende hop. Voor ons scenario *192.168.0.4*.

5. Voer het **`azure network vnet subnet route-table add`** opdracht aan de routetabel dat hierboven is gemaakt met het **front-end** subnet koppelen.

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Uitvoer:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parameters:
    - **-t (of--vnet-naam)**. De naam van de VNet waarin het subnet bevindt. Voor ons scenario *TestVNet*.
    - **- n (of--subnet-naam**. De naam van het subnet dat aan de routetabel worden toegevoegd. Voor ons scenario *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor de back-end-subnet maken
Als u wilt maken in de routetabel en de route die nodig zijn voor de back-end-subnet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

3. Voer de **`azure network route-table create`** de opdracht voor het maken van een tabel voor het subnet van de back-end.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Voer de **`azure network route-table route set`** de opdracht voor het maken van een route in de tabel hierboven is gemaakt voor het verzenden van alle verkeer dat bestemd is voor het subnet-front-end (192.168.1.0/24) **FW1** VM (192.168.0.4).

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Voer het **`azure network vnet subnet route-table add`** opdracht aan de routetabel dat hierboven is gemaakt met de **back-end** -subnet koppelen.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

