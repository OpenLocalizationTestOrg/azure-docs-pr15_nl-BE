<properties
   pageTitle="Verbinding maken met het implementatiemodel Resource Manager en de Azure portal VNets | Microsoft Azure"
   description="Een VPN-gateway verbinding maken tussen VNets met behulp van bronbeheer en de Azure portal."
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
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Een VNet-VNet-verbinding configureren met de Azure portal

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Classic - klassieke Portal](virtual-networks-configure-vnet-to-vnet-connection.md)


Dit artikel begeleidt u door de stappen voor het maken van een verbinding tussen de VNets in het implementatiemodel Resource Manager met behulp van VPN-Gateway en de Azure portal.

Wanneer u de portal Azure virtuele netwerken gebruikt, moet de VNets van het abonnement op hetzelfde. Als uw virtuele netwerken in verschillende abonnementen zijn, kunt u ze nog steeds aansluiten via [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) stappen.

![V2V-diagram](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Van implementatiemodellen en methoden voor VNet-VNet-verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

De volgende tabel ziet u de van de momenteel beschikbare implementatiemodellen en methoden voor VNet-VNet-configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>Peering VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Over VNet-VNet-verbindingen

Een virtueel netwerk verbinding te maken met een andere virtuele netwerk is (VNet VNet) vergelijkbaar met een VNet verbinding te maken met een locatie in de ruimten. Een Azure VPN-gateway beide typen verbindingen gebruiken om een beveiligde tunnel met IPsec/IKE. De VNets die u kan zijn in verschillende regio's of in verschillende abonnementen.

U kunt zelfs combineren VNet-VNet-communicatie met configuraties met meerdere sites. Hiermee kunt u tot stand brengen netwerktopologieën met een combinatie van cross-premises connectiviteit met connectiviteit tussen virtueel netwerk, zoals in het volgende diagram:

![Over verbindingen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Over verbindingen")

### <a name="why-connect-virtual-networks"></a>Waarom virtuele netwerken verbinding?

U kunt virtuele netwerken met elkaar verbinden om de volgende redenen:

- **Cross-regio geo-redundantie en geo-aanwezigheid**
    - U kunt instellen uw eigen geo-replicatie of synchronisatie met beveiligde verbindingen via het Internet facing eindpunten zonder.
    - Met Azure verkeer Manager en taakverdeling, kunt u maximaal beschikbare werkbelasting met geo-redundantie instellen tussen meerdere Azure regio's. Een belangrijk voorbeeld is voor het instellen van SQL altijd op met beschikbaarheidsgroepen spreiden over meerdere Azure regio's.

- **Regionale meerlagige toepassingen met isolatie of begrenzing**
    - In hetzelfde gebied, kunt u toepassingen met meerdere niveaus instellen met meerdere virtuele netwerken met elkaar verbonden door de isolatie- of beheervereisten.

Zie de [Veelgestelde vragen over het VNet op VNet](#faq) aan het einde van dit artikel voor meer informatie over VNet-VNet-verbindingen.

### <a name="values"></a>Van de voorbeeldinstellingen

Wanneer u de volgende stappen uit als een oefening, kunt u de configuratiewaarden van het monster. Ter voorbeeld gebruiken we meerdere adresruimten voor elke VNet. VNet-VNet-configuraties vereisen echter niet meerdere adresruimten.

**Waarden voor TestVNet1:**

- VNet naam: TestVNet1
- Adresruimte: 10.11.0.0/16
    - Subnetnaam: FrontEnd
    - Subnet-adresbereik: 10.11.0.0/24
- Resourcegroep: TestRG1
- Locatie: Oost-VS
- Adresruimte: 10.12.0.0/16
    - Subnetnaam: back-end
    - Subnet-adresbereik: 10.12.0.0/24
- Gateway subnetnaam: GatewaySubnet (wordt automatisch doorvoeren in de portal)
    - Gateway-subnetbereik adres: 10.11.255.0/27
- DNS-Server: Het IP-adres van uw DNS-Server gebruiken
- Virtueel netwerk Gateway naam: TestVNet1GW
- Gateway-Type: VPN
- VPN-type: op basis van een Route
- SKU: Selecteer de Gateway SKU die u wilt gebruiken
- Openbare IP-adres de naam: TestVNet1GWIP
- Waarden van de verbinding:
    - Naam: TestVNet1toTestVNet4
    - Gedeelde sleutel: kunt u de gedeelde sleutel zelf maken. In dit voorbeeld gebruiken we abc123. Het belangrijkste is dat bij het maken van de verbinding tussen de VNets de waarde moet overeenkomen.

**Waarden voor TestVNet4:**

- VNet naam: TestVNet4
- Adresruimte: 10.41.0.0/16
    - Subnetnaam: FrontEnd
    - Subnet-adresbereik: 10.41.0.0/24
- Resourcegroep: TestRG1
- Locatie: West VS
- Adresruimte: 10.42.0.0/16
    - Subnetnaam: back-end
    - Subnet-adresbereik: 10.42.0.0/24
- GatewaySubnet naam: GatewaySubnet (wordt automatisch doorvoeren in de portal)
    - GatewaySubnet-adresbereik: 10.41.255.0/27
- DNS-Server: Het IP-adres van uw DNS-Server gebruiken
- Virtueel netwerk Gateway naam: TestVNet4GW
- Gateway-Type: VPN
- VPN-type: op basis van een Route
- SKU: Selecteer de Gateway SKU die u wilt gebruiken
- Openbare IP-adres de naam: TestVNet4GWIP
- Waarden van de verbinding:
    - Naam: TestVNet4toTestVNet1
    - Gedeelde sleutel: kunt u de gedeelde sleutel zelf maken. In dit voorbeeld gebruiken we abc123. Het belangrijkste is dat bij het maken van de verbinding tussen de VNets de waarde moet overeenkomen.


## <a name="CreatVNet"></a>1. het maken en configureren van TestVNet1

Als u al een VNet hebt, moet u controleren of de instellingen compatibel met het ontwerp van de VPN-gateway zijn. Speciale aandacht besteden aan alle subnetten die met andere netwerken overlappen kan. Als u overlappende subnetten, werkt de verbinding niet goed. Als uw VNet is geconfigureerd met de juiste instellingen, kunt u beginnen met de stappen in de sectie [een DNS-server opgeven](#dns) .

### <a name="to-create-a-virtual-network"></a>Een virtueel netwerk maken

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Voeg extra adresruimte en subnetten maken

Als u extra ruimte toevoegen en subnetten maken zodra de VNet is gemaakt.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. een gateway-subnet maken

Voordat u het virtuele netwerk verbinding te maken met een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. Indien mogelijk is het meest geschikt voor het maken van een gateway-subnet met een CIDR-blok van /28 of /27 om voldoende IP-adressen ten behoeve van extra configuratievereisten voor toekomstige.

Als u deze configuratie als oefening maakt, verwijzen naar deze [Instellingen voorbeeld](#values) bij het maken van uw subnet gateway.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Een gateway-subnet maken

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Geef een DNS-server (optioneel)

Als u wilt het omzetten van namen voor virtuele machines die worden gebruikt voor uw VNets hebt, moet u een DNS-server.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. Maak een virtueel netwerkgateway

In deze stap maakt u de gateway virtueel netwerk voor uw VNet. Deze stap duurt maximaal 45 minuten. Als u deze configuratie als oefening maakt, kunt u verwijzen naar het [voorbeeld van de instellingen](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Voor het maken van een virtueel netwerkgateway

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. maken en TestVNet4 configureren

Wanneer u TestVNet1 hebt geconfigureerd, maken TestVNet4 herhaalt u de voorgaande stappen, vervangen door de waarden die van TestVNet4. U hoeft niet te wachten tot het virtuele netwerkgateway voor TestVNet1 maken voor het configureren van de TestVNet4 is voltooid. Als u uw eigen waarden, zorg ervoor dat de adresruimten elkaar niet overlappen met een van de VNets waarmee u verbinding wilt maken.


## <a name="TestVNet1Connection"></a>7. de TestVNet1-verbinding configureren

Als het virtuele netwerkgateways voor zowel TestVNet1 als TestVNet4 hebt voltooid, kunt u het virtuele netwerk gateway-verbindingen. In dit gedeelte maakt u een verbinding van VNet1 naar VNet4.

1. **Alle resources**, Ga naar de gateway virtueel netwerk voor uw VNet. Bijvoorbeeld **TestVNet1GW**. Klik op **TestVNet1GW** om het virtuele netwerk gateway blade openen.

    ![Verbindingen blade] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Verbindingen blade")

2. Klik op **+ toevoegen** om de bladeserver **verbinding toevoegen** te openen.

3. Typ in het naamveld op de bladeserver **verbinding toevoegen** een naam voor de verbinding. Bijvoorbeeld **TestVNet1toTestVNet4**.

    ![Naam van de verbinding] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Naam van de verbinding")

4. Voor het **type verbinding**. **VNet-VNet -** Selecteer in de vervolgkeuzelijst.

5. De **eerste virtuele netwerkgateway** veldwaarde wordt automatisch ingevuld omdat u deze verbinding vanaf de opgegeven virtuele netwerkgateway maakt.

6. Het **tweede van virtuele netwerkgateway** -veld is de gateway virtueel netwerk van de VNet die u wilt een verbinding te maken. Klik op **Kies een andere virtuele netwerkgateway** om te openen de blade **virtuele netwerkgateway kiezen** .

    ![Verbinding toevoegen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Een verbinding toevoegen")

7. Virtueel netwerkgateways die worden vermeld op deze blade weergeven. U ziet dat slechts virtueel netwerkgateways die in uw abonnement worden vermeld. Als u verbinding maken met een virtueel netwerk-gateway bevindt zich niet in uw abonnement wilt, gebruikt u [PowerShell artikel](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Klik op de netwerkgateway virtuele waarmee u verbinding wilt maken.
 
9. Typ in het veld **gedeelde sleutel** , een gedeelde sleutel voor uw verbinding. U kunt genereren of deze sleutel zelf maken. In een verbinding van site naar site, zou de sleutel precies hetzelfde zijn voor uw apparaat op gebouwen en de virtuele gateway netwerkverbinding. Het concept is vergelijkbaar, behalve dat in plaats van een verbinding met een VPN-apparaat, u verbinding maakt met een ander virtueel netwerkgateway.

    ![Gedeelde sleutel] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Gedeelde sleutel")

10. Klik op **OK** onder aan het blad als uw wijzigingen wilt opslaan.

## <a name="TestVNet4Connection"></a>8. de TestVNet4-verbinding configureren

Maak vervolgens een verbinding van TestVNet4 naar TestVNet1. Gebruik dezelfde methode die u gebruikt voor het maken van de verbinding van TestVNet1 naar TestVNet4. Zorg ervoor dat u dezelfde gedeelde sleutel gebruiken.

## <a name="VerifyConnection"></a>9. Controleer uw verbinding

Controleer de verbinding. Voor elke gateway virtueel netwerk het volgende doen:

1. Zoek het blad voor de gateway virtueel netwerk. Bijvoorbeeld **TestVNet4GW**. 
2. Klik op het netwerk van virtuele gateway-blade **verbindingen** om de bladeserver verbindingen voor de gateway virtueel netwerk weer te geven.

De verbindingen weergeven en controleer of de status. Zodra de verbinding is gemaakt, ziet u **geslaagd** en **verbonden** als de waarden van de Status.

![Geslaagd] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Geslaagd")

U kunt dubbelklikken op elke verbinding afzonderlijk voor meer informatie over de verbinding.

![Essentials] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Essentials")

## <a name="faq"></a>VNet-VNet-Veelgestelde vragen

De details van de veelgestelde vragen voor meer informatie over VNet-VNet-verbindingen weergeven.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen

Zodra de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.
