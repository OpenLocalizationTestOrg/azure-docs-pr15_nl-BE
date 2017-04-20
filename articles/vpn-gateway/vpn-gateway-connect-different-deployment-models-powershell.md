<properties 
   pageTitle="Klassieke virtuele netwerken aansluiten op Resource Manager virtuele netwerken met PowerShell | Microsoft Azure"
   description="Informatie over het maken van een VPN-verbinding tussen het klassieke VNets en met behulp van VPN-Gateway en PowerShell Resource Manager-VNets"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Verbinding maken met virtuele netwerken van verschillende modellen met PowerShell

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure heeft momenteel twee modellen: klassieke en Resource Manager (RM). Als u eerder hebt gebruikt Azure gedurende een bepaalde periode, hebt u waarschijnlijk Azure VMs en exemplaar rollen uitgevoerd in een klassieke VNet. De nieuwere VMs en rol exemplaren mogelijk in een VNet gemaakt in Resource Manager wordt uitgevoerd. Dit artikel helpt u bij het klassieke VNets verbinding met VNets om de bronnen in de van afzonderlijke implementatiemodellen met elkaar communiceren via een gateway verbinding Resource Manager.

U kunt een verbinding maken tussen VNets die zich in verschillende abonnementen en in verschillende regio's. U kunt ook verbinding maken VNets met verbindingen met netwerken voor gebouwen, al zolang als de gateway die zijn geconfigureerd met dynamische of op basis van een route. Zie de [Veelgestelde vragen over het VNet op VNet](#faq) aan het einde van dit artikel voor meer informatie over VNet-VNet-verbindingen.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Van implementatiemodellen en methoden voor het aansluiten van VNets in verschillende modellen

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We werken in de volgende tabel als nieuwe artikelen en extra's beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks op uit de tabel.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>Peering VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Voordat u begint

De volgende stappen wordt u stapsgewijs door de instellingen die nodig zijn voor een dynamische of een route-gateway configureren voor elke VNet en een VPN-verbinding maken tussen de gateways. Deze configuratie biedt geen ondersteuning voor statische of op basis van beleid gateways.

### <a name="prerequisites"></a>Vereisten

 - Beide VNets zijn al gemaakt.
 - De adresbereiken weer voor de VNets niet met elkaar overlappen of overlappen met een van de bereiken voor de gateways kunnen worden verbonden met andere verbindingen.
 - U kunt de meest recente PowerShell-cmdlets hebt geïnstalleerd (1.0.2 of hoger). [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie. Zorg ervoor dat u de Service Management (SM) en de cmdlets Resource Manager (RM) installeren. 

### <a name="exampleref"></a>Van de voorbeeldinstellingen

U kunt de voorbeeldinstellingen als referentie.

**Klassieke VNet-instellingen**

VNet Name = ClassicVNet <br>
Locatie = US West <br>
Virtueel netwerk adresruimten 10.0.0.0/24 = <br>
Subnet 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Lokale netwerknaam = RMVNetLocal <br>
GatewayType = DynamicRouting

**Resource Manager VNet instellingen**

VNet Name = RMVNet <br>
Resourcegroep RG1 = <br>
Virtueel netwerk-IP-adresruimten = 192.168.0.0/16 <br>
Subnet 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Locatie = Oost-VS <br>
Naam van openbare IP-gateway = gwpip <br>
LAN Gateway ClassicVNetLocal = <br>
Naam van de virtuele netwerkgateway = RMGateway <br>
IP-adressering configuratie van gateway = gwipconfig


## <a name="createsmgw"></a>Afdeling 1 - de klassieke VNet configureren

### <a name="part-1---download-your-network-configuration-file"></a>Deel 1 - het configuratiebestand van uw netwerk downloaden

1. Log in op uw account Azure in de console PowerShell met verhoogde rechten. De volgende cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Account Azure. Na het aanmelden, wordt het gedownload uw accountinstellingen zodat ze beschikbaar voor Azure PowerShell zijn. U gebruikt de SM PowerShell-cmdlets voor het voltooien van dit deel van de configuratie.

        Add-AzureAccount

2. Exporteer uw configuratiebestand Azure netwerk door de volgende opdracht uit te voeren. U kunt de locatie van het bestand te exporteren naar een andere locatie indien nodig wijzigen. U het bestand bewerken en vervolgens importeren in Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Open het .XML-bestand dat u hebt gedownload om deze te bewerken. Zie voor een voorbeeld van het configuratiebestand van het netwerk, het [Netwerk configuratieschema](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Deel 2 - Controleer het subnet gateway

Toevoegen in het element **VirtualNetworkSites** een subnet gateway aan uw VNet als een nog niet is gemaakt. Als u werkt met het configuratiebestand van het netwerk, de gateway subnet moet de naam 'GatewaySubnet' of Azure kan herkennen en gebruiken als een gateway-subnet.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Voorbeeld:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Deel 3 - het lokale netwerksite toevoegen

Het lokale netwerksite die u toevoegt, vertegenwoordigt de RM-VNet waarmee u verbinding wilt maken. U moet toevoegen van een **LocalNetworkSites** -element aan het bestand als een nog niet bestaat. Op dit moment in de configuratie is de VPNGatewayAddress geldige openbare IP-adres omdat we nog de gateway voor de Resource Manager VNet nog niet gemaakt. Wanneer we de gateway hebt gemaakt, vervangen we dit tijdelijke IP-adres met de juiste openbare IP-adres dat is toegewezen aan de RM-gateway.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Deel 4 – het VNet aan het lokale netwerksite koppelen

In deze sectie geeft u het lokale netwerksite die u wilt verbinding maken met de VNet aan. In dit geval is de Resource Manager-VNet die u eerder naar verwezen. Zorg ervoor dat de namen overeen. Deze stap maakt een gateway. Deze geeft het lokale netwerk waarbij de verbinding met de gateway.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Deel 5 - het bestand opslaan en uploaden

Sla het bestand op en klik vervolgens op Azure importeren door met de volgende opdracht uit te voeren. Zorg ervoor dat u het pad als dat nodig is voor uw omgeving wijzigen.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Ziet u iets dat vergelijkbaar is met dit resultaat tonen dat het importeren is voltooid.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Deel 6 – de gateway maken 

U kunt de gateway VNet maken met behulp van de klassieke portal of via PowerShell.

In het volgende voorbeeld gebruiken voor het maken van een dynamische routering gateway:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

U kunt de status van de gateway controleren met behulp van de `Get-AzureVNetGateway` cmdlet.


## <a name="creatermgw"></a>Deel 2: De RM-VNet-gateway configureren

Volg de onderstaande instructies voor informatie over het maken van een VPN-gateway voor de RM-VNet. De stappen tot niet worden gestart nadat u het openbare IP-adres voor de klassieke VNet gateway hebt opgehaald. 

1. **Log in op uw account Azure** in de console met PowerShell. De volgende cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Account Azure. Na het aanmelden, worden de instellingen van uw account zodat ze beschikbaar voor Azure PowerShell zijn gedownload.

        Login-AzureRmAccount 

    Een lijst van uw abonnementen Azure ophalen als u meer dan één abonnement hebt.

        Get-AzureRmSubscription

    Geef het abonnement dat u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Een gateway LAN maken**. In een virtueel netwerk doorgaans het lokale netwerkgateway op uw locatie op gebouwen. In dit geval verwijst de gateway voor het lokale netwerk naar de klassieke VNet. Geef deze een naam waarmee Azure kunt raadplegen en ook het adresprefix ruimte opgeven. Azure gebruikt het IP-adresprefix dat u om te bepalen welk verkeer te verzenden naar uw locatie op de locatie opgeven. Als u hier de informatie later aanpassen voordat u de gateway, kunt u de waarden wijzigen en het monster opnieuw uit te voeren.<br><br>
    - `-Name`is de naam die u wilt toewijzen om te verwijzen naar de gateway voor het lokale netwerk.<br>
    - `-AddressPrefix`is de adresruimte voor de klassieke VNet.<br>
    - `-GatewayIpAddress`het openbare IP-adres van de gateway van de klassieke VNet is. Zorg ervoor dat in het volgende voorbeeld, zodat het juiste IP-adres wijzigen.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Aanvragen van een openbaar IP-adres** worden toegewezen aan de virtuele netwerkgateway voor de Resource Manager VNet. U kunt het IP-adres dat u wilt gebruiken niet opgeven. Het IP-adres dynamisch toegewezen aan de gateway virtueel netwerk. Maar betekent dit niet dat het IP-adres wordt gewijzigd. De enige keer dat het virtuele netwerk IP-adres van standaardgateway is als de gateway is verwijderd en opnieuw gemaakt. Het wordt niet gewijzigd op vergroten/verkleinen, opnieuw in te stellen of een andere interne onderhoud/upgrades van de gateway.<br>In deze stap stellen we ook een variabele die wordt gebruikt in een latere stap.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Controleren dat het virtuele netwerk een subnet gateway heeft**. Als er geen gateway-subnet bestaat, Voeg een. Zorg ervoor dat het subnet gateway met de naam *GatewaySubnet*.

5. **Ophalen van het subnet** voor de gateway wordt gebruikt door de volgende opdracht uit te voeren. In deze stap stellen we ook een variabele moet worden gebruikt in de volgende stap.
    - `-Name`is de naam van de Resource Manager VNet.
    - `-ResourceGroupName`de resourcegroep die betrekking heeft op de VNet is. Het gateway-subnet moet al bestaan voor deze VNet en *GatewaySubnet* goed te laten werken moet worden genoemd.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Het gateway-IP-adressering configuratie maken**. De configuratie van de standaardgateway definieert het subnet en het openbare IP-adres te gebruiken. In het volgende voorbeeld gebruik te maken van de configuratie van de standaardgateway.<br>In deze stap de `-SubnetId` en `-PublicIpAddressId` -parameters moeten worden doorgegeven de eigenschap id van het subnet en het IP-adres-objecten, respectievelijk. U kunt een eenvoudige tekenreeks niet gebruiken. Deze variabelen worden ingesteld in de stap voor het aanvragen van een openbaar IP-adres en de stap voor het ophalen van het subnet.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **De bronnenbeheerder virtueel netwerkgateway maken** door de volgende opdracht uit te voeren. De `-VpnType` *RouteBased*moet zijn. Het duurt 45 minuten of meer voor deze taak is voltooid.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Het openbare IP-adres kopiëren** nadat de VPN-gateway is gemaakt. U bij het configureren van de LAN-instellingen voor de klassieke VNet. U kunt de volgende cmdlet het openbare IP-adres ophalen. Het openbare IP-adres wordt vermeld in de retour als *IP-adres*.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Deel 3: Het lokale netwerk wijzigen voor de klassieke VNet

U kunt deze stap door het exporteren van het netwerk configuratiebestand, bewerken, opslaan en importeren van het bestand terug naar Azure doen. Of u kunt deze instelling wijzigen op de klassieke portal. 

### <a name="to-modify-in-the-portal"></a>In de portal wijzigen

1. Open de [klassieke portal](https://manage.windowsazure.com).

2. Schuif naar beneden aan de linkerkant in de klassieke portal en op **netwerken**. Klik op **Lokale netwerken** boven aan de pagina op de pagina **netwerken** . 

3. Klik op de pagina **Lokale netwerken** op het lokale netwerk dat u in deel 1, geconfigureerd en Ga naar de onderkant van de pagina en klik op **bewerken**.

4. Op de pagina **Geef de gegevens van uw lokale netwerk** vervangen de tijdelijke aanduiding voor IP-adres het openbare IP-adres voor de gateway Resource Manager die u in de vorige sectie hebt gemaakt. Klik op de pijl te verplaatsen naar de volgende sectie. Controleer of het **Adres** juist is en klik op het vinkje om de wijzigingen te accepteren.

### <a name="to-modify-using-the-network-configuration-file"></a>Het configuratiebestand van netwerk wijzigen

1. Het netwerk configuratiebestand exporteren.
2. Wijzig het IP-adres van de VPN-gateway in een teksteditor.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Uw wijzigingen opslaan en het bewerkte bestand importeren in Azure.


## <a name="connect"></a>Afdeling 4: Een verbinding maken tussen de gateways

Maken van een verbinding tussen de gateways vereist PowerShell. Wellicht moet u de klassieke PowerShell-cmdlets gebruiken uw Azure-Account toevoegen. U kunt hiervoor de volgende cmdlet gebruiken: 
        
    Add-AzureAccount

1. **Uw gedeelde sleutel instellen** door de volgende opdracht uit te voeren. In dit voorbeeld wordt `-VNetName` is de naam van de klassieke VNet en `-LocalNetworkSiteName` is de naam die u hebt opgegeven voor het lokale netwerk wanneer u deze hebt geconfigureerd in de klassieke portal. De `-SharedKey` is een waarde die u kunt genereren en opgeven. De waarde die u hier opgeeft, moeten dezelfde waarde die u in de volgende stap opgeeft bij het maken van uw verbinding. Het rendement voor dit voorbeeld moet worden weergegeven **Status: geslaagde**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. De VPN-verbinding maken door de volgende opdrachten uit te voeren.

    **Stel de variabelen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Maak de verbinding**<br> Houd er rekening mee dat de `-ConnectionType` is IPsec, niet Vnet2Vnet.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. U kunt de verbinding weergeven in een portal of via de `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>VNet-VNet-Veelgestelde vragen

De details van de veelgestelde vragen voor meer informatie over VNet-VNet-verbindingen weergeven.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


