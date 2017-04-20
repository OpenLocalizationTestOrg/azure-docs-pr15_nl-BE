<properties
   pageTitle="Een virtueel netwerk maken via een VPN website met behulp van bronbeheer Azure en de Azure Portal | Microsoft Azure"
   description="VNet met het implementatiemodel Resource Manager maken en verbinding maken met uw lokale op-premises netwerk via een gateway S2S VPN-verbinding."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Een VNet maken met een Site naar Site verbinding met de Azure portal

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Classic - klassieke Portal](vpn-gateway-site-to-site-create.md)


Dit artikel helpt u bij het maken van een virtueel netwerk en een Site naar Site VPN-gateway verbinding met uw netwerk op ruimten is met het implementatiemodel Azure Resource Manager en de Azure portal. Verbindingen van site naar Site kunnen worden gebruikt voor cross-lokalen en hybride configuraties.

![Diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Van implementatiemodellen en methoden voor verbindingen van Site naar Site

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de van de momenteel beschikbare implementatiemodellen en methoden voor website configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties 

Zie als u wilt VNets met elkaar verbinden, maar niet van een verbinding naar een locatie in de lokalen maken zijn, [een VNet-VNet - verbinding configureren](vpn-gateway-vnet-vnet-rm-ps.md). Zie [een S2S verbinding toevoegen met een VNet met een bestaande VPN-gateway verbinding](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)een verbinding van Site naar Site toevoegen aan een VNet die al verbonden is.

## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met uw configuratie:

- Een VPN-apparaat en iemand kan configureren. Zie [VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw VPN-apparaat of niet vertrouwd bent met het IP-adres bereiken zich in uw locatie op het netwerk configuratie, moet u samenwerken met iemand die de details voor u kan bieden.

- Een extern gerichte openbare IP-adres voor het VPN-apparaat. Dit IP-adres kan niet zich bevinden achter een NAT bevindt.
    
- Een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Voorbeeldwaarden configuratie voor deze oefening


Wanneer u de volgende stappen uit als een oefening, kunt u de configuratiewaarden van het monster:

- **VNet naam:** TestVNet1
- **Adresruimte:** 10.11.0.0/16 en 10.12.0.0/16
- **Subnetten:**
    - FrontEnd: 10.11.0.0/24
    - Back-end: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Groep:** TestRG1
- **Locatie:** Oost-Verenigde Staten
- **DNS-Server:** 8.8.8.8
- **Gateway-naam:** VNet1GW
- **Openbare IP:** VNet1GWIP
- **VPN-Type:** Op basis van een route
- **Verbindingstype:** Site naar site (IPsec)
- **Gateway Type:** VPN
- **Lokaal netwerk Gateway naam:** Site2
- **Naam:** VNet1toSite2


## <a name="CreatVNet"></a>1. een virtueel netwerk maken 

Als u al een VNet hebt, moet u controleren of de instellingen compatibel met het ontwerp van de VPN-gateway zijn. Speciale aandacht besteden aan alle subnetten die met andere netwerken overlappen kan. Als u overlappende subnetten, werkt de verbinding niet goed. Als uw VNet is geconfigureerd met de juiste instellingen, kunt u beginnen met de stappen in de sectie [een DNS-server opgeven](#dns) .

### <a name="to-create-a-virtual-network"></a>Een virtueel netwerk maken

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Voeg extra adresruimte en subnetten

U kunt extra adresruimte en subnetten toevoegen aan uw VNet eenmaal is gemaakt.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Geef een DNS-server

### <a name="to-specify-a-dns-server"></a>Een DNS-server opgeven

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. een gateway-subnet maken

Voordat u het virtuele netwerk verbinding te maken met een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. Indien mogelijk is het meest geschikt voor het maken van een gateway-subnet met een CIDR-blok van /28 of /27 om voldoende IP-adressen ten behoeve van extra configuratievereisten voor toekomstige.

Als u deze configuratie als oefening maakt, raadpleegt u deze [waarden](#values) bij het maken van uw subnet gateway.

### <a name="to-create-a-gateway-subnet"></a>Een gateway-subnet maken


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Maak een virtueel netwerkgateway

Als u deze configuratie als oefening maakt, kunt u verwijzen naar de [voorbeeldwaarden voor de configuratie](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Voor het maken van een virtueel netwerkgateway

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. een lokale netwerk-gateway maken

De gateway van het lokale netwerk verwijst naar de locatie van gebouwen. Geef de gateway voor het lokale netwerk een naam die Azure kan verwijzen. 

Als u deze configuratie als oefening maakt, kunt u verwijzen naar de [voorbeeldwaarden voor de configuratie](#values).

### <a name="to-create-a-local-network-gateway"></a>Voor het maken van een lokaal netwerkgateway

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. het VPN-apparaat configureren

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Maak een Site naar Site VPN-verbinding

Maak de Site naar Site VPN-verbinding tussen uw van virtuele netwerkgateway en het VPN-apparaat. Zorg ervoor dat de waarden vervangen door uw eigen. De gedeelde sleutel moet overeenkomen met de waarde die u voor uw VPN-configuratie gebruikt. 

Voordat u dit gedeelte begint, controleert u of dat uw gateway virtueel netwerk en het lokale netwerkgateways gemaakt hebt. Als u deze configuratie als oefening maakt, raadpleegt u deze [waarden](#values) bij het maken van uw verbinding.

### <a name="to-create-the-vpn-connection"></a>De VPN-verbinding maken


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Controleer of de VPN-verbinding

U kunt controleren of uw VPN-verbinding in de portal of via PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

- Zodra de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie de virtuele machines [pad leren](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) voor meer informatie.

- Zie voor informatie over BGP, het [BGP-overzicht](vpn-gateway-bgp-overview.md) en [BGP configureren](vpn-gateway-bgp-resource-manager-ps.md).
