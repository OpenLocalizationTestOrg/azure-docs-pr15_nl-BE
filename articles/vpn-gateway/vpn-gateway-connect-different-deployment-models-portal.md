<properties 
   pageTitle="Klassieke virtuele netwerken verbinding met virtuele netwerken Resource Manager in de portal | Microsoft Azure"
   description="Informatie over het maken van een VPN-verbinding tussen het klassieke VNets en Resource Manager VNets met behulp van VPN-Gateway en de portal"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Verbinding maken met virtuele netwerken van verschillende modellen in de portal

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure heeft momenteel twee modellen: klassieke en Resource Manager (RM). Als u eerder hebt gebruikt Azure gedurende een bepaalde periode, hebt u waarschijnlijk Azure VMs en exemplaar rollen uitgevoerd in een klassieke VNet. De nieuwere VMs en rol exemplaren mogelijk in een VNet gemaakt in Resource Manager wordt uitgevoerd. Dit artikel helpt u bij het klassieke VNets verbinding met VNets om de bronnen in de van afzonderlijke implementatiemodellen met elkaar communiceren via een gateway verbinding Resource Manager. 

U kunt een verbinding maken tussen VNets die zich in verschillende abonnementen en in verschillende regio's. U kunt ook verbinding maken VNets met verbindingen met netwerken voor gebouwen, al zolang als de gateway die zijn geconfigureerd met dynamische of op basis van een route. Zie de [Veelgestelde vragen over het VNet op VNet](#faq) aan het einde van dit artikel voor meer informatie over VNet-VNet-verbindingen.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Van implementatiemodellen en methoden voor VNet-VNet-verbindingen

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We werken in de volgende tabel als nieuwe artikelen en extra's beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks op uit de tabel.<br><br>

**VNet-VNet-**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>Peering VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Voordat u begint

De volgende stappen wordt u stapsgewijs door de instellingen die nodig zijn voor een dynamische of een route-gateway configureren voor elke VNet en een VPN-verbinding maken tussen de gateways. Deze configuratie biedt geen ondersteuning voor statische of op basis van beleid gateways. 

In dit artikel gebruiken we de klassieke portal de Azure portal en PowerShell. Op dit moment is het niet mogelijk te maken van deze configuratie met alleen de Azure portal.

### <a name="prerequisites"></a>Vereisten

 - Beide VNets zijn al gemaakt.
 - De adresbereiken weer voor de VNets niet met elkaar overlappen of overlappen met een van de bereiken voor de gateways kunnen worden verbonden met andere verbindingen.
 - U kunt de meest recente PowerShell-cmdlets hebt geïnstalleerd (1.0.2 of hoger). [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie. Zorg ervoor dat u de Service Management (SM) en de cmdlets Resource Manager (RM) installeren. 

### <a name="values"></a>Van de voorbeeldinstellingen

U kunt de voorbeeldinstellingen als referentie.

**Klassieke VNet-instellingen**

VNet Name = ClassicVNet <br>
Locatie = US West <br>
Virtueel netwerk adresruimten 10.0.0.0/24 = <br>
Subnet 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Lokale netwerknaam = RMVNetLocal <br>

**Resource Manager VNet instellingen**

VNet Name = RMVNet <br>
Resourcegroep RG1 = <br>
Virtueel netwerk-IP-adresruimten = 192.168.0.0/16 <br>
Subnet 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Locatie = Oost-VS <br>
Naam van het virtuele netwerk gateway = RMGateway <br>
Naam van openbare IP-gateway = gwpip <br>
Type gateway VPN = <br>
VPN-type = op basis van een Route <br>
LAN gateway ClassicVNetLocal = <br>

## <a name="createsmgw"></a>Afdeling 1: Klassieke VNet-instellingen configureren

In deze sectie maken we het lokale netwerk en de gateway voor de klassieke VNet. De instructies in deze sectie worden de klassieke portal gebruiken. De Azure portal biedt op dit moment niet de instellingen die betrekking op een klassieke VNet hebben.

### <a name="part-1---create-a-new-local-network"></a>Deel 1: een nieuwe lokale netwerk maken

Open de [klassieke portal](https://manage.windowsazure.com) en meld u aan met uw account Azure.

1. Klik op **Nieuw**op de linkerbenedenhoek van het scherm, > **Network Services** > **Virtueel netwerk** > **lokaal netwerk toevoegen**.

2. Typ een naam voor de RM-VNet u verbinding wilt maken in het venster **geeft u de details van uw lokale netwerk** . Typ een geldig openbaar IP-adres in het vak **VPN-IP-adres (optioneel)** . Dit is een tijdelijke plaatsaanduiding. U kunt dit IP-adres later wijzigen. Klik op de pijl in de rechterbenedenhoek van het venster.
 
3. Typ de netwerkprefix en CIDR-blok op de pagina **Geef de adresruimte** in het tekstvak **IP starten** voor de Resource Manager VNet u verbinding wilt maken. Deze instelling wordt gebruikt om de adresruimte te routeren naar de RM-VNet opgeven.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Deel 2 - het lokale netwerk aan de VNet koppelen

1. **Virtuele netwerken** aan de bovenkant van de pagina om over te schakelen naar het scherm virtuele netwerken, klik op als u wilt uw klassieke VNet. Klik op **configureren** om te navigeren naar de configuratiepagina op de pagina voor de VNet.

2. Schakel het selectievakje **verbinding maken met het lokale netwerk** onder het gedeelte **verbinding van site naar site** verbinding. Selecteer het lokale netwerk die u hebt gemaakt. Als er meerdere lokale netwerken die u hebt gemaakt, moet u selecteren die u voor uw VNet Resource Manager in de vervolgkeuzelijst gemaakt.

3. Klik op **Opslaan** onder aan de pagina.

### <a name="part-3---create-the-gateway"></a>Deel 3 - de gateway maken

1. Na het opslaan van de instellingen, klikt u op het **Dashboard** aan de bovenkant van de pagina naar de pagina van het Dashboard. Aan de onderkant van de dashboardpagina **Gateway maken**Klik **Dynamische routering**. Klik op **Ja** om te beginnen met het maken van uw gateway. Een dynamische routering-gateway is vereist voor deze configuratie.

2. Wachten op de gateway moet worden gemaakt. Dit kan soms 45 minuten of langer duren.

### <a name="ip"></a>Deel 4 - weergave van het openbare IP-adres voor gateway

Nadat de gateway is gemaakt, kunt u het IP-adres van de gateway op de **Dashboard** -pagina bekijken. Dit is het openbare IP-adres van de gateway. Noteer of kopieer het openbare IP-adres. U in de volgende stappen als u het lokale netwerk voor de configuratie van de Resource Manager VNet.


## <a name="creatermgw"></a>Deel 2: Resource Manager VNet-instellingen configureren

In deze sectie maken we de gateway virtueel netwerk en het lokale netwerk voor de Resource Manager VNet. De volgende stappen pas niet worden gestart nadat u het openbare IP-adres voor de klassieke VNet gateway hebt opgehaald.

De screenshots zijn beschikbaar als voorbeelden. Zorg ervoor dat de waarden vervangen door uw eigen. Als u deze configuratie als oefening maakt, raadpleegt u deze [waarden](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Deel 1: een gateway-subnet maken

Voordat u het virtuele netwerk verbinding te maken met een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. Een gateway-subnet met CIDR-count van /28 of groter maken (/ 27/26, enz.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

Vanuit een browser, Ga naar de [Azure portal](http://portal.azure.com) en meld u aan met uw account Azure.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Deel 2: een virtueel netwerkgateway maken


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Deel 3: het maken van een lokaal netwerkgateway

De gateway van het lokale netwerk staat doorgaans op uw locatie op gebouwen. Krijgt Azure welk IP-adres om de route naar de locatie en het openbare IP-adres van het apparaat voor die locatie te bereiken. Echter, in dit geval het verwijst naar het adresbereik en openbare IP-adres dat is gekoppeld aan de klassieke VNet en virtueel netwerkgateway.

Geef de gateway voor het lokale netwerk een naam die Azure kan verwijzen. Kunt u uw lokale netwerkgateway terwijl uw gateway virtueel netwerk wordt gemaakt. Voor deze configuratie gebruikt u het openbare IP-adres dat is toegewezen aan de klassieke VNet vindt u in de [vorige sectie](#ip).

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Deel 4 – het openbare IP-adres kopiëren

Als de gateway virtueel netwerk maakt, is voltooid, kopieert u het openbare IP-adres dat is gekoppeld aan de gateway. U bij het configureren van de LAN-instellingen voor de klassieke VNet. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Deel 3: Het lokale netwerk wijzigen voor de klassieke VNet

Open de [klassieke portal](https://manage.windowsazure.com).

2. Schuif naar beneden aan de linkerkant in de klassieke portal en op **netwerken**. Klik op **Lokale netwerken** boven aan de pagina op de pagina **netwerken** . 

3. Klik op het lokale netwerk die u in deel 1 hebt geconfigureerd. Onderaan op de pagina, klikt u op **bewerken**.

4. Op de pagina **Geef de gegevens van uw lokale netwerk** vervangen de tijdelijke aanduiding voor IP-adres het openbare IP-adres voor de gateway Resource Manager die u in de vorige sectie hebt gemaakt. Klik op de pijl te verplaatsen naar de volgende sectie. Controleer of het **Adres** juist is en klik op het vinkje om de wijzigingen te accepteren.

## <a name="connect"></a>Afdeling 4: Maak de verbinding

In deze sectie maken we de verbinding tussen de VNets. De stappen voor deze vereist PowerShell. U kunt deze verbinding maken in een van de portals. Zorg ervoor dat u hebt gedownload en geïnstalleerd van de klassieke (SM) en de Resource Manager (RM) PowerShell-cmdlets.


1. Log in op uw account Azure in de console met PowerShell. De volgende cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Account Azure. Na het aanmelden, worden de instellingen van uw account zodat ze beschikbaar voor Azure PowerShell zijn gedownload.

        Login-AzureRmAccount 

    Een lijst van uw abonnementen Azure ophalen als u meer dan één abonnement hebt.

        Get-AzureRmSubscription

    Geef het abonnement dat u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. De klassieke PowerShell-cmdlets gebruiken uw Azure-Account toevoegen. U kunt hiervoor de volgende opdracht:

        Add-AzureAccount

3. Stel uw gedeelde sleutel door te voeren in het volgende voorbeeld. In dit voorbeeld wordt `-VNetName` is de naam van de klassieke VNet en `-LocalNetworkSiteName` is de naam die u hebt opgegeven voor het lokale netwerk wanneer u deze hebt geconfigureerd in de klassieke portal. De `-SharedKey` is een waarde die u kunt genereren en opgeven. De waarde die u hier opgeeft, moeten dezelfde waarde die u in de volgende stap opgeeft bij het maken van uw verbinding.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. De VPN-verbinding maken door de volgende opdrachten:
    
    **Stel de variabelen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Maak de verbinding**<br> Houd er rekening mee dat de `-ConnectionType` is IPsec, niet Vnet2Vnet. In dit voorbeeld wordt `-Name` is de naam die u wilt bellen uw verbinding. In het volgende voorbeeld maakt een verbinding met de naam '*rm naar klassieke verbinding*'.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Controleer uw verbinding

U kunt controleren of uw verbinding met behulp van de klassieke portal, de Azure portal of PowerShell. U kunt de volgende stappen uit om te controleren of uw verbinding. Vervangen door de waarden zelf.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>VNet-VNet-Veelgestelde vragen

De details van de veelgestelde vragen voor meer informatie over VNet-VNet-verbindingen weergeven.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


