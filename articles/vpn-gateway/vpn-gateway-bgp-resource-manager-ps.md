<properties
   pageTitle="Het BGP configureren op Azure VPN-Gateways met Azure Resource Manager en PowerShell | Microsoft Azure"
   description="In dit artikel helpt u bij het configureren van BGP met Azure VPN-Gateways met Azure Resource Manager en PowerShell."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>BGP op Azure Azure Resource Manager en PowerShell gebruiken VPN-Gateways configureren

Dit artikel begeleidt u door de stappen BGP inschakelen voor een verbinding tussen lokalen VPN van Site naar Site (S2S) en een VNet-VNet-verbinding met de bronnenbeheerder implementatiemodel en PowerShell.


**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Over BGP

BGP is de standaard routeringsprotocol vaak gebruikt op het Internet voor het uitwisselen van informatie voor Routering en bereikbaarheid tussen twee of meer netwerken. BGP kunt Azure VPN-Gateways en op ruimten VPN-apparaten, BGP collega's of buren uit te wisselen "routes" die beide gateways op de beschikbaarheid en bereikbaarheid van de voorvoegsels te gaan via de betrokken routers of gateways informeert genoemd. BGP kunt transit routering tussen meerdere netwerken doorgeven van een gateway BGP van een BGP peer aan alle andere BGP peers hoort routes verder.

Zie [Overzicht van BGP met Azure VPN-Gateways](./vpn-gateway-bgp-overview.md) voor meer discussie over de voordelen van BGP en begrijpen van de technische vereisten en overwegingen van het gebruik van BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Aan de slag met BGP op Azure VPN-gateways

Dit artikel begeleidt u door de stappen om de volgende taken uitvoeren:

- [Deel 1 - BGP op de Azure VPN-gateway inschakelen](#enablebgp)

- [Deel 2 - verbinding tussen ruimten met BGP](#crossprembgp)

- [Deel 3 - VNet-VNet-verbinding maken met BGP](#v2vbgp)

Elk deel van de instructies vormt een fundamentele bouwsteen voor BGP inschakelen in uw verbinding met het netwerk. Als u alle drie delen hebt voltooid, kunt u de topologie wordt bouwen, zoals in het volgende diagram:

![BGP-topologie](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

U kunt deze bij elkaar om samen te stellen een meer complexe, meervoudige hoop, netwerk die voldoen aan uw wensen kunt combineren.

## <a name ="enablebgp"></a>Deel 1 - BGP op de Azure VPN-Gateway configureren

De volgende configuratiestappen worden BGP de parameters van de Azure VPN-gateway instellen zoals in het volgende diagram:

![BGP Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Voordat u begint

- Controleer of u een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
    
- U moet de Azure Resource Manager PowerShell cmdlets installeert. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Stap 1 - maken en VNet1 configureren 

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Voor deze oefening beginnen we met onze variabelen declareren. In het onderstaande voorbeeld verklaart de variabelen met behulp van de waarden voor deze oefening. Zorg ervoor dat de waarden vervangen door uw eigen wanneer configureren voor productie. U kunt deze variabelen gebruiken als u door de stappen om u vertrouwd bent met dit type configuratie uitvoert. Wijzig de variabelen, en kopieer en plak in uw console PowerShell.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Stap 2: de VPN-Gateway maken voor TestVNet1 met BGP-parameters

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. configuratie van de IP- en subnet maken

Aanvragen van een openbaar IP-adres moet worden toegewezen aan de gateway die u voor uw VNet maakt. U kunt ook definiëren subnet en IP-configuraties vereist. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. de VPN-gateway maken met de AS-nummer

De gateway virtueel netwerk maken voor TestVNet1. Houd er rekening mee dat BGP vereist een Route gebaseerde VPN-gateway en de parameter toevoeging - Asn, de ASN (AS-nummer) instellen voor TestVNet1. Een gateway maken kost tijd (30 minuten of langer).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. het Azure BGP Peer-IP-adres verkrijgen

Zodra de gateway is gemaakt, moet u het BGP Peer-IP-adres van de VPN-Gateway van Azure. Dit adres is nodig om de Azure VPN-Gateway configureert als een Peer BGP voor VPN-apparaten op gebouwen.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

De laatste opdracht geeft de overeenkomstige BGP-configuraties op de Azure VPN-Gateway; bijvoorbeeld:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Zodra de gateway is gemaakt, kunt u deze gateway tot stand brengen tussen lokalen of VNet-VNet-verbinding met BGP. In de volgende secties doorlopen de stappen voor het voltooien van de oefening.

## <a name ="crossprembbgp"></a>Deel 2 - verbinding tussen ruimten met BGP

Een cross-lokalen om verbinding te maken, moet u de lokale Gateway voor uw apparaat op ruimten VPN-netwerk en een verbinding met de Azure VPN-gateway met het lokale netwerkgateway verbinding maken. Het verschil tussen de instructies in dit artikel wordt de extra eigenschappen opgeven het BGP-configuratieparameters.

![BGP voor meerdere gebouwen](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Voordat u verdergaat, Controleer of dat u [deel 1](#enablebgp) van deze oefening hebt voltooid.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Stap 1 - Maak en configureer de LAN gateway

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

In deze oefening zal blijven maken van de configuratie in het diagram weergegeven. Zorg ervoor dat vervangen door de waarden die u wilt gebruiken voor uw configuratie.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Een paar dingen te weten over het lokale netwerk gateway-parameters:

- De gateway voor het lokale netwerk kan zich in dezelfde of een andere locatie en resourcegroep als VPN-gateway. In dit voorbeeld ziet u ze in verschillende resourcegroepen in verschillende locaties.

- De minimale prefix die op te geven voor de gateway voor het lokale netwerk moet u is het adres van de host van uw BGP Peer-IP-adres op het VPN-apparaat. In dit geval een /32 is het voorvoegsel '10.52.255.254/32'.

- Ter herinnering, moet u verschillende BGP ASN's tussen de netwerken op gebouwen en Azure VNet. Als ze hetzelfde zijn, moet u de ASN VNet wijzigen als uw apparaat op ruimten VPN al met de ASN peer met andere buren BGP.
    
Controleer of dat u bent nog steeds verbonden met abonnement 1 voordat u doorgaat.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Maak de gateway voor het lokale netwerk voor Site5

Zorg ervoor dat de resourcegroep te maken als deze is gemaakt, voordat u de gateway voor het lokale netwerk. U ziet de twee extra parameters voor het lokale netwerkgateway: Asn en BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: verbinding maken met de VNet-gateway en de LAN gateway

#### <a name="1-get-the-two-gateways"></a>1. de twee gateways ophalen

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. de TestVNet1 met Site5 verbinding maken

In deze stap maakt u de verbinding van TestVNet1 voor Site5. U moet opgeven "-EnableBGP $True" BGP inschakelen voor deze verbinding. Zoals eerder is besproken, is het mogelijk om zowel BGP niet BGP verbindingen voor het VPN-Gateway van dezelfde Azure. Tenzij BGP is ingeschakeld in de eigenschap connection, Azure niet mogelijk BGP voor deze verbinding, hoewel BGP parameters al zijn geconfigureerd op beide gateways.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


In het volgende voorbeeld worden de parameters u in de sectie BGP-configuratie op uw apparaat op ruimten VPN voor deze oefening voert:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Voorvoegsels aan te kondigen: (bijvoorbeeld) 10.51.0.0/16 en 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP: 10.12.255.30
    - Statische route: een route voor 10.12.255.30/32, met nexthop wordt de interface van de VPN-tunnel op uw apparaat toevoegen
    - Multihop eBGP: de optie 'multihop' zorgen voor eBGP is ingeschakeld op het apparaat, indien nodig

De verbinding moet worden gemaakt na een paar minuten en de peering BGP-sessie gestart nadat de IPSec-verbinding tot stand is gebracht.
 
## <a name ="v2vbgp"></a>Deel 3 - VNet-VNet-verbinding maken met BGP

In deze sectie voegt een verbinding VNet naar VNet met BGP, zoals in het onderstaande diagram wordt weergegeven. 

![BGP voor VNet voor VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

De onderstaande instructies uit de voorgaande stappen hierboven vermeld worden voortgezet. U vult [deel I](#enablebgp) te maken en de TestVNet1 en de VPN-Gateway configureert met BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Stap 1 - TestVNet2 en de VPN-gateway maken

Het is belangrijk om ervoor te zorgen dat de IP-adresruimte van het nieuwe virtuele netwerk TestVNet2, niet met een VNet bereiken overlapt.

In dit voorbeeld wordt behoren de virtuele netwerken tot het hetzelfde abonnement. U kunt setup-VNet-VNet-verbindingen tussen verschillende abonnementen. Raadpleeg [een VNet-VNet - verbinding configureren](./vpn-gateway-vnet-vnet-rm-ps.md) voor meer informatie. Zorg ervoor dat u toevoegt de "-EnableBgp $True ' bij het maken van de verbindingen zodat BGP.

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Zorg ervoor dat vervangen door de waarden die u wilt gebruiken voor uw configuratie.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. TestVNet2 in de nieuwe resourcegroep maken

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. het maken van de VPN-gateway voor TestVNet2 met BGP-parameters

Aanvragen van een openbaar IP-adres moet worden toegewezen aan de gateway die u voor uw VNet maakt. U kunt ook definiëren subnet en IP-configuraties vereist. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

De VPN-gateway maken met de AS-nummer. Houd er rekening mee moet u de standaard ASN overschrijven op uw VPN-Azure gateways. De ASN's voor de VNets verbonden moeten zijn verschillende BGP en doorvoer routering inschakelen.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Stap 2: verbinding maken met de TestVNet1 en TestVNet2-gateways

In dit voorbeeld worden beide gateways in het hetzelfde abonnement. U kunt deze stap voltooien in dezelfde sessie PowerShell.

#### <a name="1-get-both-gateways"></a>1. krijgen beide gateways

Zorg ervoor dat u zich aanmelden en verbinden met 1 abonnement.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. beide verbindingen maken

In deze stap maakt u de verbinding tussen TestVNet1 en TestVNet2 en de aansluiting van TestVNet2 naar TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Zorg ervoor dat BGP voor beide verbindingen inschakelen.

Nadat u deze stappen uitvoert, worden de verbindingen in een paar minuten vast en wordt de peering BGP-sessie maar één keer is de VNet-VNet-verbinding voltooid.

Als u alle drie delen van deze oefening hebt voltooid, tot u een netwerktopologie zoals hieronder wordt weergegeven:

![BGP voor VNet voor VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Volgende stappen

Zodra de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

