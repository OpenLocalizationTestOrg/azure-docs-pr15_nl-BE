<properties
   pageTitle="Een VNet-gateway configureren voor gebruik van PowerShell ExpressRoute | Microsoft Azure"
   description="Configureer een VNet-gateway voor de implementatie van een klassieke model VNet met PowerShell voor de configuratie van een ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-classic-deployment-model-and-powershell"></a>Een virtueel netwerk-gateway configureren voor de klassieke implementatiemodel en PowerShell ExpressRoute

> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - klassiek](expressroute-howto-add-gateway-classic.md)

Dit artikel begeleidt u door de stappen voor het toevoegen en verwijderen van een virtueel netwerk (VNet)-gateway voor een bestaande VNet formaat. De stappen worden voor deze configuratie zijn specifiek voor de VNets die zijn gemaakt met behulp van het **klassieke implementatiemodel** en die zal gebruikt in een configuratie ExpressRoute. 

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Voordat u begint

Controleer of u de Azure PowerShell cmdlets die nodig zijn voor deze configuratie hebt geïnstalleerd (1.0.2 of hoger). Als u de cmdlets nog niet hebt geïnstalleerd, moet u dit doen voordat u begint met de configuratiestappen. Zie voor meer informatie over het installeren van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

    
## <a name="next-steps"></a>Volgende stappen

Nadat u de VNet-gateway hebt gemaakt, kunt u uw VNet kunt koppelen aan een ExpressRoute circuit. Zie de [koppeling een virtueel netwerk naar een ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).
