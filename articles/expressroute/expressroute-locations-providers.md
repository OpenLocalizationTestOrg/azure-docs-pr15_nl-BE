<properties
   pageTitle="ExpressRoute locaties | Microsoft Azure"
   description="Dit artikel bevat een gedetailleerd overzicht van de locaties waar de diensten worden aangeboden en verbinding maken met Azure regio's."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute partners en peering locaties

De tabellen in dit artikel bevatten informatie over ExpressRoute connectiviteit providers, ExpressRoute geografische dekking, Microsoft cloud services ondersteund via ExpressRoute en ExpressRoute, systeemintegrators (SI).

## <a name="partners"></a>ExpressRoute connectiviteit providers

ExpressRoute wordt ondersteund in alle regio's van Azure locaties. Het volgende overzicht bevat een lijst van Azure regio's en locaties ExpressRoute. ExpressRoute locaties verwijzen naar die waar Microsoft peers met diverse serviceproviders.

![Kaart met locatie][0]

Hebt u toegang tot Azure services in alle regio's binnen een geopolitieke regio als u met ten minste één ExpressRoute locatie binnen het geopolitieke gebied verbonden. De volgende tabel geeft een overzicht van Azure regio ExpressRoute locaties binnen een geopolitieke regio.

|**Geopolitieke regio**|**Azure regio 's**|**ExpressRoute locaties**|
|---|---|---|
|**Noord-Amerika**|Oost-Verenigde Staten, West VS, Oost-VS 2, centrale VS, Zuid-centraal-Amerikaanse, centrale Noord-Amerikaanse, Oost-Canada-centrale, Canada|Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal +, Quebec City +, Toronto|
|**Zuid-Amerika**|Brazilië-Zuid|Sao Paulo|
|**Europa**|West Noord-Europa, West-Europa, VK, Zuid-Verenigd Koninkrijk|Amsterdam, Dublin, Londen, Newport(Wales) +, Parijs|
|**Azië**|Oost-Azië, Zuidoost-Azië|Hong Kong, Singapore|
|**Japan**|Japanse West Japan Oost|Osaka, Tokio|
|**Australië**|Zuidoost Australië, Oost-Australië|Melbourne, Sydney|
|**India**|West India, Zuid-India-centrale, India|Chennai, Mumbai|



In de volgende tabel bevat informatie over regio's en de geopolitieke grenzen voor nationale wolken.

|**Geopolitieke regio**|**Azure regio 's**|**ExpressRoute locaties**|
|---|---|---|---|
|**Amerikaanse overheid cloud**|Amerikaanse beurs Iowa, VS finan-Virginia|Chicago, Dallas, New York, Washington DC|
|**China**|China Noord, Oost China|Beijing, Shanghai|
|**Duitsland**|Centrale, Duitsland-Oost Duitsland|Berlijn, Frankfurt|


Verbinding tussen de geopolitieke regio's wordt niet ondersteund in de standaard ExpressRoute SKU. U moet de invoegtoepassing ExpressRoute premie ter ondersteuning van algemene verbindingen inschakelen. Verbinding met nationale cloud-omgevingen wordt niet ondersteund. U kunt werken met uw provider verbinding als deze nodig.


## <a name="connectivity-provider-locations"></a>Connectiviteit-provider locaties

> [AZURE.SELECTOR]
[Locaties door Provider](expressroute-locations.md#connectivity-provider-locations)
[Providers per locatie](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Productie Azure
| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka netwerken, AT & T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, oplossingen voor Internet - verbinding wolk, Interxion, Level 3 Communications, oranje, Tata Communications, TeleCity groep Telenor, Verizon |
| **Atlanta** | Equinix |
| **Chennai** | Tata Communications |
| **Chicago** | AT & T NetBond, Comcast, Equinix, Level 3 Communications, Zayo-groep |
| **Groningen** | AT & T NetBond, Cologix, Equinix, Level 3 Communications, Megaport |
| **Dublin** | Colt Telecity groep |
| **Hong Kong** | British Telecom, China Telecom globale, Equinix, Megaport, oranje, globale PCCW beperkt, Tata Communications, Verizon |
| **Londen** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, oplossingen voor Internet - verbinding wolk, Interxion, Jisc, Level 3 Communications, MTN, NTT Communications, oranje, Tata Communications, Telecity groep Telenor, Verizon, Vodafone |
| **Las Vegas** | Level 3 Communications + Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo groep |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo groep |
| **Newport(Wales) +** | Volgende generatie gegevens + |
| **Montreal** | Cologix + |
| **Mumbai** | Tata Communications |
| **Osaka** | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank |
| **Parijs** | Interxion, Equinix + |
| **Sao Paulo** | Equinix, Telefonica |
| **Seattle** | Equinix, communicatie, niveau 3, Megaport |
| **Silicon Valley** | Netwerken, Aryaka, AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, Level 3 Communications, oranje, Tata Communications, Verizon, Zayo groep |
| **Singapore** | Netwerken, Aryaka, AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, oranje, SingTel, Tata Communications, Verizon |
| **Sydney** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, oranje, Telstra Corporation, Verizon |
| **Tokio** | Aryaka netwerken, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** | Cologix, Equinix, Zayo groep |
| **Washington D.C.** | Netwerken, Aryaka, AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, oranje, Tata Communications, Verizon, Zayo groep |

 **+**binnenkort duidt

### <a name="national-cloud-environments"></a>Nationale cloud-omgevingen

#### <a name="us-government-cloud"></a>Amerikaanse overheid cloud

| **Locatie**  |**Serviceproviders** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond, Equinix, Level 3 Communications, Verizon |
| **Groningen** |  Equinix, Verizon + |
| **New York** | Equinix, Level 3 Communications + Verizon |
| **Washington D.C.** | AT & T NetBond, Equinix, Level 3 Communications, Verizon |

#### <a name="china"></a>China

| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Peking** | China Telecom |
| **Shanghai** |  China Telecom |
Voor meer informatie, Zie [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Duitsland

| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Berlijn** | Colt + e beschutting |
| **Frankfurt** | Colt, Equinix, Interxion |

## <a name="nonpartners"></a>Verbindingen via dienstverleners niet wordt weergegeven

Als uw provider verbinding niet wordt vermeld in de voorgaande secties, kunt u nog steeds een verbinding maken.

- Neem contact op met uw provider verbinding om te zien of ze zijn aangesloten op een van de uitwisseling in de bovenstaande tabel. U kunt de volgende koppelingen voor meer informatie verzamelen over diensten die worden aangeboden door providers van exchange controleren. Verschillende aanbieders van connectiviteit zijn al verbonden met Ethernet-uitwisselingen.

    - [Equinix wolk Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Hebben uw connectiviteit provider uitbreiden van uw netwerk aan de peering gewenste locatie.
    - Controleer uw provider verbinding breidt de verbinding op een wijze die maximaal beschikbaar, zodat er geen potentiële risico.
- Bestel een ExpressRoute circuit met de exchange-als uw provider verbinding met Microsoft.
    - Volg de stappen in [een circuit ExpressRoute maken](expressroute-howto-circuit-classic.md) voor het instellen van de verbinding.

|**Locatie**|**Exchange**|**Providers connectiviteit**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Seattle** | Equinix | Alaska communicatie |
| **Silicon Valley** | Equinix | XO-communicatie |
| **Singapore** | Equinix | 1CLOUDSTAR |
| **Washington D.C.** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute systeemintegrators

Inschakelen van de particuliere verbinding aanpassen aan uw wensen kan lastig zijn, op basis van de omvang van het netwerk. U kunt werken met een van de systeemintegrators die worden vermeld in de volgende tabel om u te helpen met onboarding naar ExpressRoute.

|**Werelddeel**|**Systeemintegrators**|
|-------------|---------------------|
| **Azië** | Avanade Inc., OneAs1a|
| **Europa** | Avanade Inc., Dotnet oplossingen|
| **ONS** | Avanade Inc., Equinix Professional Services, Perficient, leiderschap Project|

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg voor meer informatie over ExpressRoute, [ExpressRoute Veelgestelde vragen](expressroute-faqs.md).
- Zorg ervoor dat aan alle vereisten is voldaan. Zie [ExpressRoute vereisten](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Kaart met locatie"
