<properties 
   pageTitle="Over ExpressRoute virtueel netwerkgateways | Microsoft Azure"
   description="Meer informatie over virtueel netwerkgateways voor ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Over het virtuele netwerkgateways voor ExpressRoute


Een virtueel netwerkgateway wordt gebruikt voor het verzenden van netwerkverkeer tussen netwerken met Azure virtual en locaties ruimten. Wanneer u een ExpressRoute verbinding configureert, moet u maken en configureren van een virtueel netwerkgateway en een virtueel netwerk gateway verbinding.

Als u een virtueel netwerkgateway maakt, kunt u verschillende instellingen opgeven. Een van de vereiste instellingen geeft aan of de gateway voor ExpressRoute of van Site naar Site VPN-verkeer worden gebruikt. In het implementatiemodel Resource Manager de instelling is '-GatewayType'.

Wanneer netwerkverkeer op een specifieke persoonlijke verbinding worden verzonden, gebruikt u het type gateway 'ExpressRoute'. Dit is ook een gateway ExpressRoute genoemd. Wanneer netwerkverkeer gecodeerd via het openbare Internet wordt verzonden, gebruikt u het type gateway 'Vpn'. Dit is een VPN-gateway genoemd. Site naar Site punt-naar-Site en VNet-VNet-verbindingen gebruiken voor een VPN-gateway. 

Elke virtuele netwerk kan slechts één virtuele netwerkgateway per type gateway hebben. U kunt bijvoorbeeld een virtueel netwerkgateway die wordt gebruikt - GatewayType Vpn en met - GatewayType ExpressRoute hebben. Dit artikel richt zich op de gateway ExpressRoute virtueel netwerk.

## <a name="gwsku"></a>Gateway-SKU 's

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Als u upgraden, vindt u een krachtiger gateway SKU wilt, in de meeste gevallen kunt u de 'Grootte AzureRmVirtualNetworkGateway' PowerShell-cmdlet. Deze procedure werkt voor upgrades naar standaard- en HighPerformance-SKU's. Echter, om te upgraden naar de SKU UltraPerformance, moet u opnieuw de gateway.

###  <a name="aggthroughput"></a>Geschatte totale doorvoer door gateway SKU


De volgende tabel ziet u de typen gateway en de geschatte totale doorvoer. Deze tabel geldt voor de resourcemanager en de van klassieke implementatiemodellen.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>REST API's en PowerShell cmdlets

Zie de volgende pagina's voor meer technische bronnen en de eisen van de specifieke syntaxis wanneer u REST API's en PowerShell-cmdlets voor virtueel netwerk gateway configuraties:

|**Klassiek** | **Resource Manager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Volgende stappen

[ExpressRoute](expressroute-introduction.md) overzicht voor meer informatie over beschikbare verbinding configuraties. 







 
