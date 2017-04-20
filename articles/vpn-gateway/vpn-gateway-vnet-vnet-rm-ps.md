<properties
   pageTitle="Azure VNets verbinding met een VPN-Gateway en PowerShell | Microsoft Azure"
   description="Dit artikel begeleidt u bij virtuele netwerken aan elkaar koppelen met behulp van bronbeheer Azure en PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>VNet-VNet-verbinding configureren voor bronbeheer met PowerShell

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Classic - klassieke Portal](virtual-networks-configure-vnet-to-vnet-connection.md)

Dit artikel begeleidt u door de stappen voor het maken van een verbinding tussen de VNets in het implementatiemodel Resource Manager met behulp van VPN-Gateway. De virtuele netwerken zijn in dezelfde of een andere regio's en van dezelfde of verschillende abonnementen.


![V2V-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Van implementatiemodellen en methoden voor VNet-VNet-verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

De volgende tabel ziet u de van de momenteel beschikbare implementatiemodellen en methoden voor VNet-VNet-configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>Peering VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>Over VNet-VNet-verbindingen

Een virtueel netwerk verbinding te maken met een andere virtuele netwerk is (VNet VNet) vergelijkbaar met een VNet verbinding te maken met een locatie in de ruimten. Een Azure VPN-gateway beide typen verbindingen gebruiken om een beveiligde tunnel met IPsec/IKE. De VNets die u verbinding kunt maken kunnen in verschillende regio's worden. Of in verschillende abonnementen. U kunt zelfs combineren VNet-VNet-communicatie met configuraties met meerdere sites. Hiermee kunt u tot stand brengen netwerktopologieën met een combinatie van cross-premises connectiviteit met connectiviteit tussen virtueel netwerk, zoals in het volgende diagram:


![Over verbindingen](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Waarom virtuele netwerken verbinding?

U kunt virtuele netwerken met elkaar verbinden om de volgende redenen:

- **Cross-regio geo-redundantie en geo-aanwezigheid**
    - U kunt instellen uw eigen geo-replicatie of synchronisatie met beveiligde verbindingen via het Internet facing eindpunten zonder.
    - Met Azure verkeer Manager en taakverdeling, kunt u maximaal beschikbare werkbelasting met geo-redundantie instellen tussen meerdere Azure regio's. Een belangrijk voorbeeld is voor het instellen van SQL altijd op met beschikbaarheidsgroepen spreiden over meerdere Azure regio's.

- **Regionale meerlagige toepassingen met isolatie of begrenzing**
    - In hetzelfde gebied, kunt u toepassingen met meerdere niveaus instellen met meerdere virtuele netwerken met elkaar verbonden door de isolatie- of beheervereisten.


### <a name="vnet-to-vnet-faq"></a>VNet-VNet-Veelgestelde vragen

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Welke stappen moet ik gebruiken?

In dit artikel ziet u twee verschillende sets van stappen. Een reeks stappen voor [die zich in het abonnement op dezelfde bevinden VNets](#samesub)en een andere voor de [VNets die zich in verschillende abonnementen bevinden](#difsub). Het belangrijkste verschil tussen de sets is of u kunt maken en configureren van alle virtuele netwerk- en gateway-bronnen binnen dezelfde PowerShell-sessie.

De stappen in dit artikel met de variabelen die zijn gedeclareerd aan het begin van elke sectie. Als u al met bestaande VNets werkt, wijzig de variabelen aan de instellingen in uw eigen omgeving. 

![Beide verbindingen](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>VNets die in het abonnement op dezelfde verbinding maken

![V2V-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Voordat u begint
    
Voordat u begint moet u de Azure Resource Manager PowerShell cmdlets installeert. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="Step1"></a>Stap 1: uw IP-adresbereiken plannen


In de volgende stappen maken we twee virtuele netwerken en hun respectieve gateway subnetten en configuraties. Vervolgens maken we een VPN-verbinding tussen de twee VNets. Het is belangrijk voor het plannen van de IP-adresbereiken voor uw netwerkconfiguratie. Houd er rekening mee dat u moet ervoor zorgen dat geen van de VNet bereiken of lokale netwerkbereiken op enigerlei wijze overlappen.

We gebruiken de volgende waarden in de voorbeelden:

**Waarden voor TestVNet1:**

- VNet naam: TestVNet1
- Resourcegroep: TestRG1
- Locatie: Oost-VS
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Back-end: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet1GW
- Openbare IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**Waarden voor TestVNet4:**

- VNet naam: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Back-end: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Resourcegroep: TestRG4
- Locatie: West VS
- DNS-Server: 8.8.8.8
- GatewayName: VNet4GW
- Openbare IP: VNet4GWIP
- VPNType: RouteBased
- Verbinding: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Stap 2 - maken en TestVNet1 configureren

1. De variabelen declareren

    Begin met het declareren van variabelen. In dit voorbeeld verklaart de variabelen met behulp van de waarden voor deze oefening. In de meeste gevallen moet u de waarden vervangen door uw eigen. U kunt deze variabelen echter zijn dat u door de stappen om u vertrouwd bent met dit type configuratie. Wijzig de variabelen indien nodig, en vervolgens kopiëren en plakken in de console met PowerShell.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Verbinding maken met uw abonnement

    Overschakelen naar de modus PowerShell naar de Resource Manager-cmdlets gebruiken. De PowerShell-console openen en verbinding maken met uw account. In het volgende voorbeeld gebruiken waarmee u verbinding kunt maken:

        Login-AzureRmAccount

    Controleer de abonnementen voor de account.

        Get-AzureRmSubscription 

    Geef het abonnement dat u wilt gebruiken.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Een nieuwe resourcegroep maken

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. De subnet-configuraties voor TestVNet1 maken

    In dit voorbeeld wordt een virtueel netwerk met de naam TestVNet1 en drie subnetten, één genaamd GatewaySubnet één genoemd FrontEnd en Backend voor één genoemd. Bij het vervangen van waarden, is het belangrijk dat u altijd uw gateway-subnet naam speciaal GatewaySubnet. Als u naam iets anders, mislukt uw gateway maken. 

    Het volgende voorbeeld worden de variabelen die u eerder hebt ingesteld. In dit voorbeeld wordt maakt het subnet gateway gebruik van een /27. Tijdens het maken van een subnet gateway /29 zo klein mogelijk is, wordt aangeraden dat u maakt een grotere subnet met meer adressen door ten minste /28 of /27 te selecteren. Hierdoor wordt voor voldoende adressen voor eventuele extra configuraties kunt u in de toekomst. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. TestVNet1 maken

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Een openbaar IP-adres aanvragen

    Aanvragen van een openbaar IP-adres moet worden toegewezen aan de gateway die u voor uw VNet maakt. Merk op dat de AllocationMethod dynamisch is. U kunt het IP-adres dat u wilt gebruiken niet opgeven. Het uw gateway dynamisch toegewezen. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Maken van de configuratie van de standaardgateway

    De configuratie van de standaardgateway definieert het subnet en het openbare IP-adres te gebruiken. In het voorbeeld gebruik te maken van de configuratie van de standaardgateway. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. De gateway voor TestVNet1 maken

    In deze stap maakt u de gateway virtueel netwerk voor uw TestVNet1. VNet-VNet-configuraties vereisen een RouteBased VpnType. Een gateway maken kan even duren (45 minuten of langer).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Stap 3 - maken en TestVNet4 configureren

Wanneer u TestVNet1 hebt geconfigureerd, maakt de TestVNet4. Volg de stappen onder de waarden te vervangen door uw eigen wanneer dat nodig is. Deze stap kan worden gedaan in dezelfde sessie PowerShell omdat het in het hetzelfde abonnement.

1. De variabelen declareren

    Zorg ervoor dat vervangen door de waarden die u wilt gebruiken voor uw configuratie.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Voordat u verdergaat, moet dat u nog bent verbonden met 1 abonnement.

2. Een nieuwe resourcegroep maken

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. De subnet-configuraties voor TestVNet4 maken

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. TestVNet4 maken

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Een openbaar IP-adres aanvragen

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Maken van de configuratie van de standaardgateway

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. De gateway TestVNet4 maken

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Stap 4 - gateways verbinden

1. Beide gateways virtueel netwerk ophalen

    In dit voorbeeld omdat beide gateways in het hetzelfde abonnement, kan deze stap worden uitgevoerd in dezelfde sessie PowerShell.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. De TestVNet1 op TestVNet4-verbinding maken

    In deze stap maakt u de verbinding van TestVNet1 naar TestVNet4. Hier ziet u een gedeelde sleutel waarnaar wordt verwezen in de voorbeelden. U kunt uw eigen waarden voor de gedeelde sleutel gebruiken. Het belangrijkste is dat de gedeelde sleutel voor beide verbindingen moet overeenkomen. Maken van een verbinding kan duren kort te voltooien.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. De TestVNet4 op TestVNet1-verbinding maken

    Deze stap lijkt een hierboven, maar u de verbinding van TestVNet4 naar TestVNet1 maakt. Zorg ervoor dat de gedeelde sleutels overeenkomen.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    De verbinding moet worden gemaakt na een paar minuten.

4. Controleer of de verbinding. Zie de sectie [controleren of uw verbinding](#verify).


## <a name="difsub"></a>Het aansluiten van de VNets die in de verschillende abonnementen


![V2V-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

In dit scenario wordt de verbinding TestVNet1 en TestVNet5. TestVNet1 en TestVNet5 bevinden zich in een ander abonnement. De stappen voor deze configuratie toevoegen een extra verbinding van de VNet-VNet-TestVNet1 verbinding met TestVNet5. 

Het verschil hier is dat sommige configuratiestappen moeten worden uitgevoerd in een aparte PowerShell-sessie in het kader van het tweede abonnement. Met name wanneer de twee abonnementen deel uitmaken van verschillende organisaties. 

De instructies van de voorgaande stappen hierboven vermeld worden voortgezet. U kunt [stap 1](#Step1) en [stap 2](#Step2) als u wilt maken en configureren van TestVNet1 en de VPN-Gateway voor TestVNet1 moet voltooien. Nadat u stap 1 en stap 2 hebt voltooid, gaat u verder met stap 5 voor het maken van TestVNet5.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Stap 5: Controleer of de overige IP-adresbereiken

Het is belangrijk om ervoor te zorgen dat de IP-adresruimte van het nieuwe virtuele netwerk TestVNet5, niet met een van de VNet bereiken of lokale netwerk gateway bereiken overlapt. 

In dit voorbeeld wordt de virtuele netwerken kunnen deel uitmaken van verschillende organisaties. Voor deze oefening, kunt u de volgende waarden voor de TestVNet5:

**Waarden voor TestVNet5:**

- VNet naam: TestVNet5
- Resourcegroep: TestRG5
- Locatie: Japan Oost
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Back-end: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet5GW
- Openbare IP: VNet5GWIP
- VPNType: RouteBased
- Verbinding: VNet5toVNet1
- ConnectionType: VNet2VNet

**Extra waarden voor TestVNet1:**

- Verbinding: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Stap 6 - maken en TestVNet5 configureren

Deze stap moet worden uitgevoerd in het kader van het nieuwe abonnement. Dit deel kan worden uitgevoerd door de beheerder in een andere organisatie die eigenaar is van het abonnement.

1. De variabelen declareren

    Zorg ervoor dat vervangen door de waarden die u wilt gebruiken voor uw configuratie.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Verbinding maken met een abonnement 5

    De PowerShell-console openen en verbinding maken met uw account. Met het volgende voorbeeld kunt u verbinding maken:

        Login-AzureRmAccount

    Controleer de abonnementen voor de account.

        Get-AzureRmSubscription 

    Geef het abonnement dat u wilt gebruiken.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Een nieuwe resourcegroep maken

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. De subnet-configuraties voor TestVNet4 maken
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. TestVNet5 maken

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Een openbaar IP-adres aanvragen

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Maken van de configuratie van de standaardgateway

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. De gateway TestVNet5 maken

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Stap 7 - gateways verbinden

In dit voorbeeld omdat de gateways in de verschillende abonnementen hebt we opgesplitst in deze stap twee PowerShell-sessies die zijn gemarkeerd als [abonnement 1] en [abonnement 5].

1. **[1 abonnement]** De gateway virtueel netwerk voor abonnement 1 ophalen

    Zorg ervoor dat u zich aanmeldt en verbinding maken met 1 abonnement.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    De uitvoer van de volgende elementen kopiëren en verzenden deze naar de beheerder van 5 abonnement per e-mail of een andere methode.

        $vnet1gw.Name
        $vnet1gw.Id

    Deze twee elementen heeft een vergelijkbaar met het volgende voorbeeld van de uitvoer:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Abonnement 5]** De gateway virtueel netwerk voor abonnement 5 ophalen

    Zorg ervoor dat u zich aanmeldt en verbinding maken met een abonnement op 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    De uitvoer van de volgende elementen kopiëren en verzenden deze naar de beheerder van 1 abonnement per e-mail of een andere methode.

        $vnet5gw.Name
        $vnet5gw.Id

    Deze twee elementen heeft een vergelijkbaar met het volgende voorbeeld van de uitvoer:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[1 abonnement]** De TestVNet1 op TestVNet5-verbinding maken

    In deze stap maakt u de verbinding van TestVNet1 naar TestVNet5. Het verschil hier is $vnet5gw kan niet rechtstreeks worden verkregen omdat het in een ander abonnement. U moet een nieuw PowerShell-object te maken met de waarden van 1 abonnement meegedeeld in de bovenstaande stappen. Gebruik het onderstaande voorbeeld. De naam, de Id en de gedeelde sleutel vervangen door uw eigen waarden. Het belangrijkste is dat de gedeelde sleutel voor beide verbindingen moet overeenkomen. Maken van een verbinding kan duren kort te voltooien.

    Zorg ervoor dat u verbinding maakt met 1 abonnement. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Abonnement 5]** De TestVNet5 op TestVNet1-verbinding maken

    Deze stap lijkt een hierboven, maar u de verbinding van TestVNet5 naar TestVNet1 maakt. Hetzelfde proces van het maken van een PowerShell-object op basis van de waarden die zijn verkregen uit abonnement 1 geldt ook hier. In deze stap worden de gedeelde sleutels moeten overeenkomen met.

    Zorg ervoor dat u verbinding maakt met een abonnement op 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Een verbinding controleren


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Volgende stappen

- Zodra de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.
- Zie voor informatie over BGP, het [BGP-overzicht](vpn-gateway-bgp-overview.md) en [BGP configureren](vpn-gateway-bgp-resource-manager-ps.md). 

