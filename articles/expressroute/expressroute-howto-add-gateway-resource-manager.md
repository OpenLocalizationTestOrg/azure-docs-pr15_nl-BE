<properties
   pageTitle="Een gateway VNet met een virtueel netwerk toe te voegen voor ExpressRoute met behulp van bronbeheer en PowerShell | Microsoft Azure"
   description="Dit artikel begeleidt u bij het toevoegen van een Vnet-gateway naar een reeds gemaakte Resource Manager VNet voor ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Een virtueel netwerk-gateway configureren voor gebruik van Resource Manager en PowerShell ExpressRoute


> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - klassiek](expressroute-howto-add-gateway-classic.md)


Dit artikel begeleidt u door de stappen voor het toevoegen en verwijderen van een virtueel netwerk (VNet)-gateway voor een bestaande VNet formaat. De stappen worden voor deze configuratie zijn specifiek voor de VNets die zijn gemaakt met behulp van het **implementatiemodel Resource Manager** en die zal gebruikt in een configuratie ExpressRoute. 

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Voordat u begint

Controleer of u de Azure PowerShell cmdlets die nodig zijn voor deze configuratie hebt geïnstalleerd (1.0.2 of hoger). Als u de cmdlets nog niet hebt geïnstalleerd, moet u dit doen voordat u begint met de configuratiestappen. Zie voor meer informatie over het installeren van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Volgende stappen

Nadat u de VNet-gateway hebt gemaakt, kunt u uw VNet kunt koppelen aan een ExpressRoute circuit. Zie de [koppeling een virtueel netwerk naar een ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).
