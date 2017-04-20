<properties
   pageTitle="Een virtueel netwerk maken via een VPN website met behulp van bronbeheer Azure en PowerShell | Microsoft Azure"
   description="Dit artikel helpt u bij het maken van een VNet met behulp van het implementatiemodel Resource Manager en verbonden met het lokale op gebouwen-netwerk via een gateway S2S VPN-verbinding."
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

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Een VNet maken met een Site naar Site verbinding met PowerShell

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Classic - klassieke Portal](vpn-gateway-site-to-site-create.md)

Dit artikel helpt u bij het maken van een virtueel netwerk en een Site naar Site VPN-gateway verbinding met het netwerk op gebouwen is het implementatiemodel Azure Resource Manager gebruiken. Verbindingen van site naar Site kunnen worden gebruikt voor cross-lokalen en hybride configuraties.

![Diagram van site naar Site] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "site naar site") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Van implementatiemodellen en methoden voor verbindingen van Site naar Site

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de van de momenteel beschikbare implementatiemodellen en methoden voor website configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties

Zie als u wilt VNets met elkaar verbinden, maar niet van een verbinding naar een locatie in de lokalen maken zijn, [een VNet-VNet - verbinding configureren](vpn-gateway-vnet-vnet-rm-ps.md). Zie [een S2S verbinding toevoegen met een VNet met een bestaande VPN-gateway verbinding](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)een verbinding van Site naar Site toevoegen aan een VNet die al verbonden is.

## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u de configuratie.

- Een VPN-apparaat en iemand kan configureren. Zie [VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw VPN-apparaat of niet vertrouwd bent met het IP-adres bereiken zich in uw locatie op het netwerk configuratie, moet u samenwerken met iemand die de details voor u kan bieden.

- Een extern gerichte openbare IP-adres voor het VPN-apparaat. Dit IP-adres kan niet zich bevinden achter een NAT bevindt.
    
- Een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
    
- De nieuwste versie van de Azure Resource Manager PowerShell-cmdlets. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="Login"></a>1. Maak verbinding met uw abonnement 

Zorg ervoor dat u overschakelen naar de modus PowerShell naar de Resource Manager-cmdlets gebruiken. Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.

De PowerShell-console openen en verbinding maken met uw account. Met het volgende voorbeeld kunt u verbinding maken:

    Login-AzureRmAccount

Controleer de abonnementen voor de account.

    Get-AzureRmSubscription 

Geef het abonnement dat u wilt gebruiken.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. een virtueel netwerk en een gateway-subnet maken

De voorbeelden gebruiken een subnet gateway van /28. Tijdens het maken van een subnet gateway /29 zo klein mogelijk is, wordt aangeraden dat u maakt een grotere subnet met meer adressen door ten minste /28 of /27 te selecteren. Hierdoor wordt voor voldoende adressen voor eventuele extra configuraties kunt u in de toekomst.

Als u al een virtueel netwerk met een gateway-subnet dat is/29 of groter, u kunt gaan naar [uw lokale netwerkgateway toevoegt](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Een virtueel netwerk en een gateway-subnet maken

In het volgende voorbeeld gebruiken om een virtueel netwerk en een subnet gateway te maken. Vervang de waarden voor uw eigen. 

Maak eerst een groep:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Maak vervolgens het virtuele netwerk. Controleer of de adresruimte die u niet een van de adresruimte die u in uw netwerk op ruimten hebt overlappen.

In het volgende voorbeeld wordt een virtueel netwerk met de naam *testvnet* en twee subnetten, één genaamd *GatewaySubnet* gemaakt en de andere met de naam *Subnet1*. Het is belangrijk dat u één subnet genaamd speciaal *GatewaySubnet*maken. Als noem maar iets anders, mislukt de configuratie van de verbinding. 

Stel de variabelen.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

De VNet maken.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Een gateway subnet toevoegen aan een virtueel netwerk die u al hebt gemaakt

Deze stap is alleen vereist als u wilt een gateway subnet toevoegen aan een VNet die u eerder hebt gemaakt.

U kunt uw gateway-subnet maken met behulp van het volgende voorbeeld. Zorg ervoor dat de naam van het subnet gateway 'GatewaySubnet'. Als u naam iets anders, u een subnet maken, maar Azure won't worden behandeld als een gateway-subnet.

Stel de variabelen.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

De gateway-subnet maken.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

De configuratie instellen. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>de gateway van uw lokale netwerk toevoegen

In een virtueel netwerk doorgaans het lokale netwerkgateway op uw locatie op gebouwen. U geeft de site een naam waarmee Azure kunt raadplegen en ook het adresprefix ruimte voor het lokale netwerkgateway opgeven. 

Azure gebruikt het IP-adresprefix dat u om te bepalen welk verkeer te verzenden naar uw locatie op de locatie opgeven. Dit betekent dat u moet elke adresprefix die u wilt koppelen aan uw lokale netwerkgateway opgeven. Als uw netwerk op de locatie wijzigt, kunt u gemakkelijk deze voorvoegsels bijwerken. 

Wanneer u de voorbeelden PowerShell, Let op het volgende:
    
- De *GatewayIPAddress* is het IP-adres van uw apparaat op ruimten VPN. Het VPN-apparaat kan niet zich bevinden achter een NAT bevindt. 
- De *AddressPrefix* is de adresruimte op gebouwen.

Een lokaal netwerkgateway met een voorvoegsel één adres toevoegen:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Een lokaal netwerkgateway met meerdere adresprefixen toevoegen:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>IP-adresprefixen voor de gateway van uw lokale netwerk wijzigen

Soms wordt de netwerkprefixen voor lokale gateway wijzigen. De stappen voor het wijzigen van uw IP-adresprefixen is afhankelijk van of u een VPN-gateway verbinding hebt gemaakt. Zie de sectie [wijzigen van IP-adresprefixen voor een lokaal netwerkgateway](#modify) van dit artikel.


## <a name="PublicIP"></a>4. verzoek een openbaar IP-adres voor de VPN-gateway

Vervolgens vraagt u een openbaar IP-adres moet worden toegewezen aan de Azure VNet VPN-gateway. Dit is niet hetzelfde IP-adres dat is toegewezen aan de VPN-apparaat; in plaats daarvan toegewezen aan de Azure VPN-gateway zelf. U kunt het IP-adres dat u wilt gebruiken niet opgeven. Het is dynamisch toegewezen aan uw gateway. U kunt dit IP-adres bij het configureren van het apparaat op ruimten VPN-verbinding maken met de gateway.

De Azure VPN-gateway voor de Resource Manager implementatiemodel momenteel ondersteunt alleen de openbare IP-adressen met behulp van de methode voor dynamische toewijzing. Maar betekent dit niet dat het IP-adres wordt gewijzigd. De enige keer dat de Azure VPN-IP-adres van standaardgateway is wanneer de gateway is verwijderd en opnieuw gemaakt. Het openbare IP-adres voor gateway niet gewijzigd op vergroten/verkleinen, opnieuw in te stellen of een andere interne onderhoud/upgrades van de Azure VPN-gateway.

Gebruik de volgende PowerShell voorbeeld:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. Maak de configuratie van gateway-IP-adressering

De configuratie van de standaardgateway definieert het subnet en het openbare IP-adres te gebruiken. In het volgende voorbeeld gebruik te maken van de configuratie van de standaardgateway.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. de gateway virtueel netwerk maken

In deze stap maakt u de gateway virtueel netwerk. Een gateway maken kan lang duren om te voltooien. Vaak 45 minuten of meer. 

De volgende waarden gebruiken:

- De *-GatewayType* voor een Site naar Site-configuratie is de *Vpn*. Het type gateway heeft altijd betrekking op de configuratie die u implementeert. Andere gateway-configuraties mogelijk bijvoorbeeld - GatewayType ExpressRoute. 

- De *-VpnType* kan worden *RouteBased* (hierna aangeduid als een dynamische Gateway in sommige documentatie) of *PolicyBased* (hierna aangeduid als een statische Gateway in sommige documentatie). Zie voor meer informatie over VPN-gateway typen [Over VPN-Gateways](vpn-gateway-about-vpngateways.md#vpntype).
- De *-GatewaySku* is *standaard*, *standaard*of *HighPerformance*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. het VPN-apparaat configureren

Op dit punt moet u het openbare IP-adres van de gateway virtueel netwerk voor het configureren van uw op ruimten VPN-apparaat. Werken met de fabrikant van het apparaat voor specifieke configuratiegegevens. U kunt verwijzen naar de [VPN-apparaten](vpn-gateway-about-vpn-devices.md) voor meer informatie.

Het openbare IP-adres van uw gateway virtueel netwerk zoekt, gebruikt u in het volgende voorbeeld:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. de VPN-verbinding maken

Vervolgens maakt u de Site naar Site VPN-verbinding tussen uw van virtuele netwerkgateway en het VPN-apparaat. Zorg ervoor dat de waarden vervangen door uw eigen. De gedeelde sleutel moet overeenkomen met de waarde die u voor uw VPN-configuratie gebruikt. Merk op dat de `-ConnectionType` voor *IPsec*-website. 

Stel de variabelen.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Maak de verbinding.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Na een korte tijdens de verbinding wordt tot stand worden gebracht. 

## <a name="toverify"></a>Om te controleren of een VPN-verbinding

Er zijn een paar manieren om te controleren of uw VPN-verbinding.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>IP-adresprefixen voor de gateway van een lokale netwerk wijzigen

Als u wijzigen de voorvoegsels voor de gateway van uw lokale netwerk wilt, gebruik de volgende instructies. Twee sets van instructies worden geleverd. De instructies die u kiest, is afhankelijk van of u al uw gateway verbinding hebt gemaakt. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Het gateway-IP-adres voor de gateway van een lokale netwerk wijzigen

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

- U kunt virtuele machines toevoegen aan uw virtuele netwerken. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

- Zie voor informatie over BGP, het [BGP-overzicht](vpn-gateway-bgp-overview.md) en [BGP configureren](vpn-gateway-bgp-resource-manager-ps.md).

