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

| **Serviceprovider**  |**Microsoft Azure** | **Office 365 en CRM Online** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **AARNet** | Ondersteund | Ondersteund | Melbourne, Sydney |
| **[Aryaka netwerken]( http://www.aryaka.com/)** | Ondersteund | Ondersteund | Amsterdam, Silicon Valley, Singapore, Tokio, Washington DC |
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Ondersteund | Ondersteund | Amsterdam, Arnhem, Rotterdam, Amsterdam, Silicon Valley, Singapore, Sydney, Washington DC |
| **[British Telecom]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | Ondersteund | Ondersteund | Amsterdam, Hong Kong, Londen, Silicon Valley, Singapore, Sydney, Tokio, Washington DC |
|**CenturyLink** | Binnenkort beschikbaar | Binnenkort beschikbaar| Silicon Valley |
|**China Telecom globale** | Ondersteund | Niet ondersteund | Hong Kong |
|**[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** | Ondersteund | Binnenkort beschikbaar | Rotterdam, Montreal + Toronto |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  Ondersteund | Ondersteund | Amsterdam, Dublin, Londen, Tokio |
| **Comcast** | Ondersteund | Ondersteund | Chicago, Silicon Valley, Washington DC |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** | Ondersteund | Ondersteund | Los Angeles | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Ondersteund | Ondersteund | Amsterdam, Atlanta, Chicago, Dallas, Hong Kong, Londen, Los Angeles, Melbourne, New York, Osaka, Parijs +, Sao Paulo, Seattle, Silicon Valley, Singapore, Sydney, Tokio, Toronto, Washington DC |
| **euNetworks** |  Ondersteund | Ondersteund | Amsterdam |
| **GÉANT** | Ondersteund | Ondersteund | Amsterdam |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |  Ondersteund | Ondersteund | Osaka, Tokio |
| **[InterCloud]( https://www.intercloud.com/)** | Ondersteund | Ondersteund | Amsterdam, Londen, Singapore, Washington D.C. |
| **Sluit Internet - Cloud-oplossingen** | Ondersteund | Ondersteund | Amsterdam, Londen |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)**  | Ondersteund | Ondersteund | Amsterdam, Londen, Parijs |
| **Jisc** | Ondersteund | Ondersteund | Londen | 
| **[Level 3 Communications]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Ondersteund | Ondersteund | Amsterdam, Arnhem, Groningen, Las Vegas +, Amsterdam, Rotterdam, Silicon Valley, Washington DC |
| **Megaport** | Ondersteund | Ondersteund | Dallas, Hong-Kong, Las Vegas, Los Angeles, Melbourne, New York, Seattle, Singapore, Sydney, Washington DC |
| **MTN** | Ondersteund | Ondersteund | Londen |
| **Volgende generatie gegevens** | Binnenkort beschikbaar | Binnenkort beschikbaar | Newport(Wales) + |
| **NEXTDC** | Ondersteund | Ondersteund | Melbourne, Sydney |
| **NTT Communications** | Ondersteund | Ondersteund | Osaka in Los Angeles, Londen, Tokio |
| **[Oranje]( http://www.orange-business.com/en/products/business-vpn-galerie)** | Ondersteund | Ondersteund | Amsterdam, Hong Kong, Londen, Silicon Valley, Singapore, Sydney, Washington DC |
| **Globale PCCW beperkt** | Ondersteund | Ondersteund | Hong Kong |
| **[SingTel]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  Ondersteund | Ondersteund | Singapore |
| **Softbank** | Ondersteund | Ondersteund | Osaka, Tokio | 
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | Ondersteund | Ondersteund | Amsterdam, Chennai, Hongkong, Londen, Mumbai, Silicon Valley, Singapore, Washington D.C. |
| **[Groep TeleCity]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | Ondersteund | Ondersteund | Amsterdam, Dublin, Londen |
| **Telefonica** | Ondersteund | Ondersteund | Sao Paulo |
| **Telenor** | Ondersteund | Ondersteund | Amsterdam, Londen |
| **[Telstra Corporation]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | Ondersteund | Ondersteund | Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | Ondersteund | Ondersteund | Amsterdam, Hong Kong, Londen, Silicon Valley, Singapore, Sydney, Tokio, Washington DC |
| **Vodafone** | Ondersteund | Niet ondersteund | Londen | 
| **[Zayo groep]( http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** | Ondersteund | Ondersteund | Chicago, Los Angeles, New York, Silicon Valley, Toronto, Washington DC |

 **+**binnenkort duidt

### <a name="national-cloud-environments"></a>Nationale cloud-omgevingen

#### <a name="us-government-cloud"></a>Amerikaanse overheid cloud

| **Serviceprovider**  |**Microsoft Azure** | **Office 365** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Ondersteund | Ondersteund | Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Ondersteund | Ondersteund | Chicago, Dallas, New York, Washington DC |
| **[Level 3 Communications]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Ondersteund | Ondersteund | Chicago, New York +, Washington DC |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | Ondersteund | Ondersteund | Chicago, Dallas +, New York, Washington DC |

#### <a name="china"></a>China

| **Serviceprovider**  |**Microsoft Azure** | **Office 365** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **China Telecom** | Ondersteund | Niet ondersteund | Beijing, Shanghai|
Voor meer informatie, Zie [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/).

#### <a name="germany"></a>Duitsland

| **Serviceprovider**  |**Microsoft Azure** | **Office 365** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** | Ondersteund | Niet ondersteund | Berlijn + Frankfurt|
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Ondersteund | Niet ondersteund | Frankfurt|
| **e beschutting** | Ondersteund | Niet ondersteund | Berlijn|
| **Interxion** | Ondersteund | Niet ondersteund | Frankfurt|

## <a name="nonpartners"></a>Verbindingen via dienstverleners niet wordt weergegeven

Als uw provider verbinding niet wordt vermeld in de voorgaande secties, kunt u nog steeds een verbinding maken.

- Neem contact op met uw provider verbinding om te zien of ze zijn aangesloten op een van de uitwisseling in de bovenstaande tabel. U kunt de volgende koppelingen voor meer informatie verzamelen over diensten die worden aangeboden door providers van exchange controleren. Verschillende aanbieders van connectiviteit zijn al verbonden met Ethernet-uitwisselingen.

    - [Equinix wolk Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Hebben uw connectiviteit provider uitbreiden van uw netwerk aan de peering gewenste locatie.
    - Controleer uw provider verbinding breidt de verbinding op een wijze die maximaal beschikbaar, zodat er geen potentiële risico.
- Bestel een ExpressRoute circuit met de exchange-als uw provider verbinding met Microsoft.
    - Volg de stappen in [een circuit ExpressRoute maken](expressroute-howto-circuit-classic.md) voor het instellen van de verbinding.

|**Connectiviteit-provider**|**Exchange**|**Locaties**|
|---|---|---|
|**[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)**|Equinix|Singapore|
|**Alaska communicatie**|Equinix|Seattle|
|**[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure )**|Equinix|New York, Washington DC|
|**[XO-communicatie](http://www.xo.com/)**|Equinix|Silicon Valley|


## <a name="expressroute-system-integrators"></a>ExpressRoute systeemintegrators

Inschakelen van de particuliere verbinding aanpassen aan uw wensen kan lastig zijn, op basis van de omvang van het netwerk. U kunt werken met een van de systeemintegrators die worden vermeld in de volgende tabel om u te helpen met onboarding naar ExpressRoute.

|**Systeem integrator**|**Werelddeel**|
|---|---|
|**[Avanade Inc.](http://www.avanade.com/)**| Azië, Europa, Verenigde Staten |
|**[DotNet oplossingen](http://www.dotnetsolutions.co.uk/)**| Europa |
|**[Equinix Professional Services](http://www.equinix.com/services/consulting/)**|ONS|
|**[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Azië |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | ONS |
|**[Project leiderschap](http://www.projectleadership.net/azure)** | ONS |

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg voor meer informatie over ExpressRoute, [ExpressRoute Veelgestelde vragen](expressroute-faqs.md).
- Zorg ervoor dat aan alle vereisten is voldaan. Zie [ExpressRoute vereisten](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Kaart met locatie"
