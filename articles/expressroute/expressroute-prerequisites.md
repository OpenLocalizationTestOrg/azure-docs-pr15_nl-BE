<properties
   pageTitle="Vereisten voor de vaststelling van ExpressRoute | Microsoft Azure"
   description="Deze pagina bevat een lijst met vereisten moet worden voldaan voordat u een circuit Azure ExpressRoute kunt bestellen."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute vereisten & Controlelijst  

Als u wilt verbinding maken met Microsoft cloud services met behulp van ExpressRoute, moet u controleren of voldaan aan de volgende vereisten die worden vermeld in de onderstaande secties.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-account

- Een geldige en actieve Microsoft Azure-account. Dit is nodig om het circuit ExpressRoute in te stellen. ExpressRoute circuits zijn bronnen binnen Azure abonnementen. Een Azure-abonnement is vereist, zelfs als de verbinding is beperkt tot Azure Microsoft cloud services, zoals Office 365 diensten en CRM online.
- Een actieve Office 365-abonnement (als services voor Office 365). Zie de sectie [Office 365 specifieke voorschriften](#office-365-specific-requirements) van dit artikel voor meer informatie.

## <a name="connectivity-provider"></a>Connectiviteit-provider
- U kunt werken met een [partner van ExpressRoute verbindingen](expressroute-locations.md#partners) verbinding maken met de Microsoft-cloud. U kunt een verbinding tussen uw netwerk op gebouwen en van Microsoft op [drie manieren](expressroute-introduction.md#howtoconnect)instellen. 
- Als uw provider een verbinding ExpressRoute partner is, kunt u nog steeds verbinding maken naar de cloud met Microsoft via een [cloud exchange provider](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Netwerkvereisten
- **Redundante verbindingen**: Er is geen redundantievereiste op de fysieke verbinding tussen u en uw provider. Microsoft vereist redundante BGP sessies ingesteld tussen Microsoft routers en routers peering, zelfs wanneer er slechts [één fysieke verbinding met een exchange cloud](expressroute-faqs.md#onep2plink). 
- **Routering**: afhankelijk van hoe u verbinding met de Microsoft-Cloud, u of uw provider moet instellen en beheren van de BGP sessies voor [Routering domeinen](expressroute-circuit-peerings.md). Sommige Ethernet-connectiviteit provider of cloud exchange provider bieden BGP management als een service waarde toevoegen.
- **NAT**: Microsoft accepteert alleen openbare IP-adressen via Microsoft peering. Als u van particuliere IP-adressen in het netwerk op gebouwen gebruikmaakt, adressen u of uw provider moet vertalen de particuliere IP-adressen aan het openbare IP-, [met het NAT-apparaat](expressroute-nat.md).
- **QoS**: Skype voor Business heeft diverse services (bijvoorbeeld spraak, video, tekst), waarvoor gedifferentieerde QoS-behandeling. U en uw provider moeten voldoen aan de [vereisten voor QoS](expressroute-qos.md).
- **Netwerkbeveiliging**: u moet rekening houden met [beveiliging van het netwerk](../best-practices-network-security.md) verbinding met de Microsoft Cloud via ExpressRoute.
 
## <a name="office-365"></a>Office 365

Als u van plan bent om Office 365 op ExpressRoute, raadpleegt u de volgende documenten voor meer informatie over de vereisten voor Office 365.


- [Overzicht van ExpressRoute voor Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Routering met ExpressRoute voor Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365-URL's en IP-adresbereiken](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Planning van het netwerk en het afstemmen van prestaties voor Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Netwerk bandbreedte rekenmachines en hulpprogramma 's](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Integratie van Office 365 met omgevingen voor gebouwen](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Als u van plan bent om CRM Online op ExpressRoute, raadpleegt u de volgende documenten voor meer informatie over CRM Online

- [CRM Online URL's](https://support.microsoft.com/kb/2655102) en [IP-adresbereiken](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg voor meer informatie over ExpressRoute, [ExpressRoute Veelgestelde vragen](expressroute-faqs.md).
- Zoek een provider ExpressRoute connectiviteit. Zie [ExpressRoute partners en peering locaties](expressroute-locations.md).
- Zie vereisten voor [Routering](expressroute-routing.md), [NAT](expressroute-nat.md) en [QoS](expressroute-qos.md).
- Configureer uw verbinding ExpressRoute.
    - [ExpressRoute circuits maken](expressroute-howto-circuit-classic.md)
    - [Routering configureren](expressroute-howto-routing-classic.md)
    - [Een VNet koppelen aan een ExpressRoute circuit](expressroute-howto-linkvnet-classic.md)

