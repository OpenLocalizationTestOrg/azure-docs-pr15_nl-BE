<properties
   pageTitle="Verbinding maken met uw netwerk op gebouwen naar Azure | Microsoft Azure"
   description="Uitgelegd en vergelijkt de verschillende methoden voor de verbinding met Microsoft cloud services zoals Azure, Office 365 en Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Uw netwerk op ruimten verbinden met Azure

Microsoft biedt verschillende soorten cloud services. Terwijl u verbinding met alle services via het openbare Internet maakt, kunt u ook verbinding maken met enkele van de services met een virtueel particulier netwerk (VPN)-tunnel via Internet of via een directe, particuliere verbinding naar Microsoft. In dit artikel kunt u beter bepalen welke optie connectiviteit wordt best uw behoeften op basis van de soorten Microsoft cloud services die u verbruikt. De meeste organisaties maken gebruik van verschillende verbindingstypen die hieronder worden beschreven.

## <a name="connecting-over-the-public-internet"></a>Verbinding maken via het openbare Internet

Dit verbindingstype biedt toegang tot Microsoft cloud services rechtstreeks via Internet, zoals hieronder wordt weergegeven.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Deze verbinding is meestal het eerste dat wordt gebruikt voor de verbinding met de cloud services van Microsoft. De onderstaande tabel worden de voor- en nadelen van dit verbindingstype.



| **Voordelen**| **Overwegingen met betrekking tot**|
|---------|---------|
|Vereist geen wijziging van uw netwerk op ruimten als alle clientapparaten hebt onbeperkte toegang tot alle IP-adressen en poorten op het Internet.|Al het verkeer gecodeerd is vaak via HTTPS, kan deze worden onderschept in doorvoer omdat het transport via het openbare Internet.|
|Kan verbinding maken met alle Microsoft-cloud services met het openbare Internet wordt blootgesteld.|Onverwachte vertraging omdat de verbinding web surft.|
|Gebruikt uw bestaande Internet-verbinding.||
|Beheer van apparaten verbinding nodig.||

Deze verbinding heeft geen verbinding of de kosten voor bandbreedte, omdat u uw bestaande Internet-verbinding gebruiken. 

## <a name="connecting-with-a-point-to-site-connection"></a>Verbinding maken met een punt-naar-site verbinding

Dit verbindingstype biedt toegang tot sommige Microsoft cloud-services via een tunnel Secure Socket Tunneling Protocol (SSTP) via het Internet, zoals hieronder wordt weergegeven.

![P2S] (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "Punt-naar-site verbinding")

De verbinding wordt gemaakt via uw bestaande Internet-verbinding, maar vereist het gebruik van een VPN-Gateway van Azure. De onderstaande tabel worden de voor- en nadelen van dit verbindingstype.

| **Voordelen**| **Overwegingen met betrekking tot**|
|---------|---------|
|Vereist geen wijziging van uw netwerk op ruimten als alle clientapparaten hebt onbeperkte toegang tot alle IP-adressen en poorten op het Internet.|Al het gegevensverkeer wordt gecodeerd met IPSec, kan deze worden onderschept omdat het transport via het openbare Internet.|
|Gebruikt uw bestaande Internet-verbinding.|Onverwachte vertraging omdat de verbinding web surft.|
|De doorvoer van maximaal 200 Mb/s per gateway.|Maken en beheren van afzonderlijke verbindingen tussen elk apparaat op uw netwerk op gebouwen en elke gateway verbinding maken moet met elk apparaat vereist.|
|Kan worden gebruikt voor verbinding met Azure services die kunnen worden aangesloten op een Azure virtuele netwerken (VNet) zoals Azure virtuele Machines en Azure Cloud Services.|Minimale doorlopende beheer van een Azure VPN-Gateway vereist.|
||Verbinding maken met Microsoft Office 365 of Dynamics CRM Online kan niet worden gebruikt.
||Verbinding maken met Azure services die niet worden verbonden met een VNet kan niet worden gebruikt.|

Meer informatie over de [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) service, de [prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway)en de gegevens van uitgaande transfer [prijzen](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Verbinding maken met een verbinding van site naar site

Dit verbindingstype biedt toegang tot sommige Microsoft cloud-services via een IPSec-tunnel via het Internet, zoals hieronder wordt weergegeven.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Verbinding van site naar site")

De verbinding wordt gemaakt via uw bestaande Internet-verbinding, maar vereist het gebruik van een VPN-Gateway Azure, met bijbehorende prijzen en een uitgaande gegevensoverdracht prijzen. De onderstaande tabel worden de voor- en nadelen van dit verbindingstype.

| **Voordelen**| **Overwegingen met betrekking tot**|
|---------|---------|
|Alle apparaten op uw netwerk op gebouwen kunnen communiceren met Azure services verbonden met een VNet is niet nodig om afzonderlijke verbindingen voor elk apparaat te configureren.|Al het gegevensverkeer wordt gecodeerd met IPSec, kan deze worden onderschept omdat het transport via het openbare Internet.|
|Gebruikt uw bestaande Internet-verbinding.|Onverwachte vertraging omdat de verbinding web surft.|
|Verbinding maken met Azure services die kunnen worden verbonden met een VNet zoals virtuele Machines en Cloud Services kan worden gebruikt.|Moet configureren en beheren van een gevalideerde VPN-apparaat * op gebouwen.|
|De doorvoer van maximaal 200 Mb/s per gateway.|Minimale doorlopende beheer van een Azure VPN-Gateway vereist.|
|Uitgaand verkeer geïnitieerd vanuit de cloud virtuele machines via het netwerk op voorzieningen voor inspectie en registratie door de gebruiker gedefinieerde routes of Border Gateway Protocol (BGP) kunt dwingen **.|Verbinding maken met Microsoft Office 365 of Dynamics CRM Online kan niet worden gebruikt.|
||Verbinding maken met Azure services die niet worden verbonden met een VNet kan niet worden gebruikt.|
||Als u services die verbindingen met apparaten voor lokalen weer starten en uw beveiligingsbeleid is vereist, moet u een firewall tussen het netwerk van lokalen en Azure.|

- * Een lijst van [gevalideerde VPN-apparaten](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices)weergeven.
- ** Meer weten over het gebruik van [door de gebruiker gedefinieerde routes](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) of [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) dwingen routering van Azure VNets met een apparaat dat op het bedrijf.

## <a name="connecting-with-a-dedicated-private-connection"></a>Verbinding maken met een specifieke persoonlijke verbinding

Dit verbindingstype biedt toegang tot alle Microsoft cloud-services via een specifieke persoonlijke verbinding naar Microsoft wordt niet via Internet, verloopt als volgt.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute verbinding")

De verbinding is vereist voor gebruik van de service ExpressRoute en een verbinding met de provider voor een verbinding. De onderstaande tabel worden de voor- en nadelen van dit verbindingstype.

| **Voordelen**| **Overwegingen met betrekking tot**|
|---------|---------|
|Verkeer kan niet worden onderschept op het openbare Internet omdat een speciale verbinding via een Internet-provider wordt gebruikt.|Beheer van de router op locatie vereist.|
|Bandbreedte tot 10 Gb/s per ExpressRoute circuit en een doorvoer van maximaal 2 Gb/s met elke gateway.|Een speciale verbinding aan een provider verbinding vereist.|
|Voorspelbare latentie omdat het is een speciale verbinding aan Microsoft die niet via Internet verloopt.|Mogelijk minimale doorlopende beheer van een of meer Azure VPN-Gateways (als de verbinding het circuit met VNets).|
|Gecodeerde communicatie vereist niet als u, het verkeer, coderen kunt indien gewenst.| Als u cloud-services die verbindingen terug naar apparaten voor bedrijven te maken, moet u een firewall tussen het netwerk van lokalen en Azure.|
|Kan rechtstreeks verbinding maken met alle Microsoft cloud-diensten, met een paar uitzonderingen *.|Netwerkadresomzetting (NAT) is vereist voor bedrijfsruimten IP-adressen invoeren van de Microsoft datacenters voor services die niet worden verbonden met een VNet.* *|
|Uitgaand verkeer geïnitieerd vanuit de cloud virtuele machines via het netwerk op voorzieningen voor inspectie en BGP-Logboeken kunt u dwingen.|

- * Een [lijst met services](../expressroute/expressroute-faqs.md#supported-services) die kunnen worden gebruikt met ExpressRoute weergeven. Uw abonnement op Azure moet worden goedgekeurd voor de verbinding met Office 365.  Zie het artikel [Azure ExpressRoute voor Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) voor meer informatie.
- ** Meer informatie over [NAT](../expressroute/expressroute-nat.md) ExpressRoute eisen.

Meer informatie over [ExpressRoute](../expressroute/expressroute-introduction.md), de bijbehorende [prijzen](https://azure.microsoft.com/pricing/details/expressroute)en [providers connectiviteit](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Aanvullende overwegingen

- Verschillende van de bovenstaande opties hebben verschillende maximumgehalten die ze voor VNet verbindingen, gateway-verbindingen en andere criteria kunnen ondersteunen. Het wordt aangeraden de Azure [netwerken beperkt](../azure-subscription-service-limits.md#networking-limits) om te begrijpen als deze van invloed zijn op de verbindingen typen die u wilt gebruiken. 
- Als u een gateway van een VPN-verbinding van site naar site het dezelfde VNet als een gateway ExpressRoute verbinding wilt maken, moet u vertrouwd raken met belangrijke beperkingen eerst. Zie het artikel [ExpressRoute configureren en naast elkaar bestaande verbindingen van Site naar Site](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

De volgende bronnen wordt uitgelegd hoe de verbindingstypen die in dit artikel te implementeren.

-   [Een punt-naar-site verbinding implementeren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Een verbinding van site naar site implementeren](guidance-hybrid-network-vpn.md)
-   [Een specifieke persoonlijke verbinding implementeren](guidance-hybrid-network-expressroute.md)
-   [Een speciale particuliere verbinding met een verbinding van site naar site voor hoge beschikbaarheid implementeren](guidance-hybrid-network-expressroute-vpn-failover.md)
