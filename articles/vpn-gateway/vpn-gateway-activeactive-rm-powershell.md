<properties
   pageTitle="Actieve S2S VPN-verbindingen configureren met Azure VPN-Gateways met Azure Resource Manager en PowerShell | Microsoft Azure"
   description="Dit artikel begeleidt u bij het configureren van actieve verbindingen met Azure VPN-Gateways Azure Resource Manager en PowerShell gebruiken."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Actieve S2S VPN-verbindingen met Azure Azure Resource Manager en PowerShell gebruiken VPN-Gateways configureren

Dit artikel begeleidt u door de stappen voor het maken van actieve cross-ruimten en VNet-VNet-verbindingen met de bronnenbeheerder implementatiemodel en PowerShell.


**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Over verbindingen met hoge beschikbaarheid Cross-gebouwen

Voor maximale beschikbaarheid voor cross-ruimten en VNet-VNet-connectiviteit, moet u implementeert meerdere VPN-gateways en meerdere parallelle verbindingen maken tussen netwerken en Azure. Zie [uiterst beschikbare Cross-ruimten en VNet-VNet - connectiviteit](./vpn-gateway-highlyavailable.md) voor een overzicht van connectiviteitsopties en de topologie.

Dit artikel bevat de instructies voor het instellen van een VPN-verbinding van actieve cross-lokalen en actieve verbinding tussen twee virtuele netwerken:

- [Deel 1 - maken en de Azure VPN-gateway configureren in de actieve modus](#aagateway)

- [Deel 2 – actieve cross-premises verbindingen tot stand brengen](#aacrossprem)

- [Deel 3 – actieve VNet-VNet-verbindingen tot stand brengen](#aav2v)

- [Deel 4 - bestaande gateway tussen actieve en actieve stand-by-Update](#aaupdate)

U kunt deze bij elkaar om samen te stellen een meer complexe, hoge beschikbaarheid netwerktopologie die voldoet aan uw wensen kunt combineren.

>[AZURE.IMPORTANT] Houd er rekening mee dat de actieve alleen in HighPerformance SKU werkt


## <a name ="aagateway"></a>Deel 1 - maken en actieve VPN-gateways configureren

De volgende stappen configureert de Azure VPN-gateway in actieve modus. De belangrijkste verschillen tussen de actieve en actieve stand-by-gateways:

- U moet twee IP-Gateway-configuraties met twee openbare IP-adressen maken
- U moet de vlag EnableActiveActiveFeature instellen
- De gateway SKU moet worden HighPerformance

De overige eigenschappen zijn hetzelfde als de niet-actieve gateways. 

### <a name="before-you-begin"></a>Voordat u begint

- Controleer of u een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
    
- U moet de Azure Resource Manager PowerShell cmdlets installeert. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Stap 1 - maken en VNet1 configureren

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Voor deze oefening beginnen we met onze variabelen declareren. In het onderstaande voorbeeld verklaart de variabelen met behulp van de waarden voor deze oefening. Zorg ervoor dat de waarden vervangen door uw eigen wanneer configureren voor productie. U kunt deze variabelen gebruiken als u door de stappen om u vertrouwd bent met dit type configuratie uitvoert. Wijzig de variabelen, en kopieer en plak in uw console PowerShell.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Maak verbinding met uw abonnement en een nieuwe resourcegroep maken

Zorg ervoor dat u overschakelen naar de modus PowerShell naar de Resource Manager-cmdlets gebruiken. Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.

De PowerShell-console openen en verbinding maken met uw account. Met het volgende voorbeeld kunt u verbinding maken:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. TestVNet1 maken

Het onderstaande voorbeeld wordt een virtueel netwerk met de naam TestVNet1 en drie subnetten, één genaamd GatewaySubnet één genoemd FrontEnd en Backend voor één genoemd. Bij het vervangen van waarden, is het belangrijk dat u altijd uw gateway-subnet naam speciaal GatewaySubnet. Als u naam iets anders, mislukt uw gateway maken. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Stap 2: de VPN-gateway maken voor TestVNet1 met actieve modus

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. het openbare IP-adressen en gateway IP-configuraties maken

Aanvraag twee openbare IP-adressen worden toegewezen aan de gateway die u voor uw VNet maakt. U kunt ook definiëren subnet en IP-configuraties vereist. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Maak de VPN-gateway met actieve configuratie

De gateway virtueel netwerk maken voor TestVNet1. Houd er rekening mee dat er twee GatewayIpConfig posten, en de vlag EnableActiveActiveFeature is ingesteld. Actieve modus is een Route-gebaseerde VPN-gateway van HighPerformance SKU. Een gateway maken kost tijd (30 minuten of langer).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. de gateway openbare IP-adressen en het BGP Peer-IP-adres verkrijgen

Zodra de gateway is gemaakt, moet u het BGP Peer-IP-adres van de VPN-Gateway van Azure. Dit adres is nodig om de Azure VPN-Gateway configureert als een Peer BGP voor VPN-apparaten op gebouwen.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Gebruik de volgende cmdlets twee openbare IP-adressen die zijn toegewezen voor de VPN-gateway en de bijbehorende BGP Peer-IP-adressen voor elk exemplaar van de gateway weergeven:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

De volgorde van de openbare IP-adressen voor de gateway-exemplaren en de bijbehorende BGP Peering adressen hetzelfde zijn. In dit voorbeeld wordt de gateway VM met openbare IP-40.112.190.5 van de 10.12.255.4 wordt gebruikt als het adres BGP Peering en de gateway met 138.91.156.129 wordt 10.12.255.5 gebruikt. Deze informatie is nodig wanneer u op ruimten VPN-apparaten verbinding te maken met de actieve gateway instellen. De gateway wordt weergegeven in het diagram hieronder met alle adressen:

![actieve gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Zodra de gateway is gemaakt, kunt u deze gateway om actieve cross-lokalen of VNet-VNet-verbinding. In de volgende secties doorlopen de stappen voor het voltooien van de oefening.


## <a name ="aacrossprem"></a>Deel 2 - verbinding van een actieve cross-gebouwen

Een cross-lokalen om verbinding te maken, moet u de lokale Gateway voor uw apparaat op ruimten VPN-netwerk en een verbinding met de Azure VPN-gateway met het lokale netwerkgateway verbinding maken. In dit voorbeeld wordt de Azure VPN-gateway in actieve modus. Als gevolg hiervan zelfs als er slechts één op-lokalen VPN-apparaat (LAN gateway) en een verbinding-bron, maken beide exemplaren Azure VPN-gateway met S2S VPN-tunnels met het apparaat op gebouwen.

Voordat u verdergaat, Controleer of dat u [deel 1](#aagateway) van deze oefening hebt voltooid.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Stap 1 - Maak en configureer de LAN gateway

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

In deze oefening zal blijven maken van de configuratie in het diagram weergegeven. Zorg ervoor dat vervangen door de waarden die u wilt gebruiken voor uw configuratie.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Een paar dingen te weten over het lokale netwerk gateway-parameters:

- De gateway voor het lokale netwerk kan zich in dezelfde of een andere locatie en resourcegroep als VPN-gateway. In dit voorbeeld ziet u ze in verschillende resourcegroepen, maar in dezelfde locatie Azure.

- Als er slechts één VPN-apparaat van op ruimten zoals hierboven is weergegeven, wordt de actieve verbinding kunt werken met of zonder BGP-protocol. BGP wordt in dit voorbeeld voor de verbinding tussen bedrijven.

- Als BGP is ingeschakeld, wordt het voorvoegsel dat u nodig hebt om aan te geven voor de gateway voor het lokale netwerk het adres van de host van uw BGP Peer-IP-adres op het VPN-apparaat. In dit geval een /32 is het voorvoegsel '10.52.255.253/32'.

- Ter herinnering, moet u verschillende BGP ASN's tussen de netwerken op gebouwen en Azure VNet. Als ze hetzelfde zijn, moet u de ASN VNet wijzigen als uw apparaat op ruimten VPN-al de ASN wordt gebruikt om met andere buren BGP van peer.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Maak de gateway voor het lokale netwerk voor Site5
    
Controleer of dat u bent nog steeds verbonden met abonnement 1 voordat u doorgaat. De resourcegroep te maken als deze nog niet is gemaakt.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: verbinding maken met de VNet-gateway en de LAN gateway

#### <a name="1-get-the-two-gateways"></a>1. de twee gateways ophalen

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. de TestVNet1 met Site5 verbinding maken

In deze stap maakt u de verbinding van TestVNet1 naar Site5_1 met 'EnableBGP' is ingesteld op $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN en BGP parameters voor uw apparaat op ruimten VPN

In het volgende voorbeeld worden de parameters u in de sectie BGP-configuratie op uw apparaat op ruimten VPN voor deze oefening voert:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Voorvoegsels aan te kondigen: (bijvoorbeeld) 10.51.0.0/16 en 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 voor 40.112.190.5-tunnel
    - Azure VNet BGP IP 2: 10.12.255.5 voor 138.91.156.129-tunnel
    - Statische routes: bestemming 10.12.255.4/32, nexthop de VPN-tunnel in de interface 40.112.190.5 10.12.255.5/32 bestemming, de VPN-tunnel in de interface 138.91.156.129 nexthop
    - Multihop eBGP: de optie 'multihop' zorgen voor eBGP is ingeschakeld op het apparaat, indien nodig

De verbinding moet worden gemaakt na een paar minuten en de peering BGP-sessie gestart nadat de IPSec-verbinding tot stand is gebracht. In dit voorbeeld heeft tot nu toe geconfigureerd dat slechts één op ruimten VPN-apparaat, waardoor het diagram hieronder weergegeven:

![actief-actief-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Stap 3: twee op ruimten VPN-apparaten aansluiten op de actieve VPN-gateway

Hebt u twee VPN-apparaten op hetzelfde netwerk op lokalen, kunt u dubbele redundantie bereiken door de Azure VPN-gateway naar het tweede apparaat van de VPN-verbinding te maken.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. de tweede LAN gateway voor Site5 maken

Noteer het gateway-IP-adres, adresprefix en BGP peering adres voor de tweede LAN gateway niet met de vorige LAN gateway voor hetzelfde netwerk op gebouwen overlappen moeten. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Sluit de gateway VNet en de tweede LAN gateway

De verbinding van TestVNet1 naar Site5_2 met 'EnableBGP' is ingesteld op $True maken

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN en BGP-parameters voor het tweede op ruimten VPN-apparaat

Op dezelfde manier onder lijsten de parameters voert u in het tweede VPN-apparaat:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Voorvoegsels aan te kondigen: (bijvoorbeeld) 10.51.0.0/16 en 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 voor 40.112.190.5-tunnel
    - Azure VNet BGP IP 2: 10.12.255.5 voor 138.91.156.129-tunnel
    - Statische routes: bestemming 10.12.255.4/32, nexthop de VPN-tunnel in de interface 40.112.190.5 10.12.255.5/32 bestemming, de VPN-tunnel in de interface 138.91.156.129 nexthop
    - Multihop eBGP: de optie 'multihop' zorgen voor eBGP is ingeschakeld op het apparaat, indien nodig

Zodra de verbinding (tunnels) zijn gemaakt, hebt u twee redundante VPN-apparaten en verbinding maken met uw netwerk op gebouwen en Azure tunnels:

![Dual-redundantie-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Deel 3 – een actieve VNet-VNet-verbinding tot stand brengen

In deze sectie wordt een actieve VNet-VNet-verbinding gemaakt met BGP. 

De onderstaande instructies uit de voorgaande stappen hierboven vermeld worden voortgezet. [Deel 1](#aagateway) te maken en de TestVNet1 en de VPN-Gateway configureert met BGP, moet u voltooien. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Stap 1 - TestVNet2 en de VPN-gateway maken

Het is belangrijk om ervoor te zorgen dat de IP-adresruimte van het nieuwe virtuele netwerk TestVNet2, niet met een VNet bereiken overlapt.

In dit voorbeeld wordt behoren de virtuele netwerken tot het hetzelfde abonnement. U kunt instellen van VNet-VNet-verbindingen tussen verschillende abonnementen. Raadpleeg [een VNet-VNet - verbinding configureren](./vpn-gateway-vnet-vnet-rm-ps.md) voor meer informatie. Zorg ervoor dat u toevoegt de "-EnableBgp $True ' bij het maken van de verbindingen zodat BGP.

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Zorg ervoor dat vervangen door de waarden die u wilt gebruiken voor uw configuratie.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. TestVNet2 in de nieuwe resourcegroep maken

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. de actieve VPN-gateway maken voor TestVNet2

Aanvraag twee openbare IP-adressen worden toegewezen aan de gateway die u voor uw VNet maakt. U kunt ook definiëren subnet en IP-configuraties vereist. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

De VPN-gateway maken met de AS-nummer en de vlag 'EnableActiveActiveFeature'. Houd er rekening mee moet u de standaard ASN overschrijven op uw VPN-Azure gateways. De ASN's voor de VNets verbonden moeten zijn verschillende BGP en doorvoer routering inschakelen.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Stap 2: verbinding maken met de TestVNet1 en TestVNet2-gateways

In dit voorbeeld worden beide gateways in het hetzelfde abonnement. U kunt deze stap voltooien in dezelfde sessie PowerShell.

#### <a name="1-get-both-gateways"></a>1. krijgen beide gateways

Zorg ervoor dat u zich aanmeldt en verbinding maken met 1 abonnement.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. beide verbindingen maken

In deze stap maakt u de verbinding tussen TestVNet1 en TestVNet2 en de aansluiting van TestVNet2 naar TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Zorg ervoor dat BGP voor beide verbindingen inschakelen.

Na het voltooien van deze stappen, de verbinding wordt tot stand brengen in een paar minuten en het BGP peering sessie worden van zodra de verbinding VNet naar VNet met dubbele redundantie is voltooid:

![Active-active-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Deel 4 - bestaande gateway tussen actieve en actieve stand-by-Update

De laatste sectie wordt beschreven hoe u een bestaande Azure VPN-gateway van de actieve stand-by-modus actief-actief of vice versa kunt configureren.

>[AZURE.IMPORTANT] Houd er rekening mee dat de actieve alleen in HighPerformance SKU werkt

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Een actieve stand-by-gateway naar actieve gateway configureren

#### <a name="1-gateway-parameters"></a>1. de gateway-parameters

In het volgende voorbeeld wordt een actieve stand-by-gateway converteert naar een actieve gateway. U moet een andere openbare IP-adres maken en vervolgens een tweede Gateway IP-configuratie toevoegen. Hieronder ziet u de parameters die worden gebruikt:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Maak het openbare IP-adres en vervolgens de tweede gateway IP-configuratie toevoegen

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. actieve modus inschakelen en bijwerken van de gateway

In PowerShell voor het starten van de werkelijke update, moet u het object gateway instellen. De SKU van het object gateway moet ook worden gewijzigd in HighPerformance omdat het eerder is gemaakt als standaard.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Deze update kan 30 tot 45 minuten duren.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Een actieve gateway naar de actieve stand-by-gateway configureren

#### <a name="1-gateway-parameters"></a>1. de gateway-parameters

Gebruik dezelfde parameters zoals hierboven, de naam van de IP-configuratie die u wilt verwijderen.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. Verwijder het gateway-IP-configuratie en de actieve modus uitschakelen

U moet de gateway-object stelt in PowerShell voor het starten van de werkelijke update.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Deze update kan maximaal 30 tot 45 minuten duren.


## <a name="next-steps"></a>Volgende stappen

Zodra de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

