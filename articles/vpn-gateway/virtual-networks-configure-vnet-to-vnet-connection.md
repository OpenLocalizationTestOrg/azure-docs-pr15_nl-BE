<properties
   pageTitle="Configureren van een verbinding met VNet met VNet voor het implementatiemodel klassiek | Microsoft Azure"
   description="Het Azure virtuele netwerken maken met behulp van PowerShell en de klassieke Azure portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Een VNet-VNet-verbinding voor het implementatiemodel klassiek configureren

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Classic - klassieke Portal](virtual-networks-configure-vnet-to-vnet-connection.md)



Dit artikel begeleidt u door de stappen voor het maken en verbinding maken met virtuele netwerken maken met behulp van het implementatiemodel klassiek (ook bekend als Service Management). De Azure klassieke portal de volgende stappen gebruiken om de VNets en gateways en PowerShell configureren van de VNet-VNet-verbinding te maken. U kunt de verbinding niet configureren in de portal.

![VNet VNet connectiviteit diagram](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Van implementatiemodellen en methoden voor VNet-VNet-verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

De volgende tabel ziet u de van de momenteel beschikbare implementatiemodellen en methoden voor VNet-VNet-configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Over VNet-VNet-verbindingen

Een virtueel netwerk verbinding te maken met een andere virtuele netwerk is (VNet VNet) vergelijkbaar met een virtueel netwerk verbinding te maken met een locatie in de ruimten. Een VPN-gateway beide typen verbindingen gebruiken om een beveiligde tunnel met IPsec/IKE. 

De VNets die u verbinding kunt maken kunnen in de verschillende abonnementen en de verschillende regio's worden. U kunt VNet combineren om VNet communicatie met configuraties met meerdere sites. Hiermee kunt u netwerktopologieën die meerdere ruimten connectiviteit met de netwerkverbinding tussen de virtuele combineren tot stand brengen.


### <a name="why-connect-virtual-networks"></a>Waarom virtuele netwerken verbinding?

U kunt virtuele netwerken met elkaar verbinden om de volgende redenen:

- **Cross-regio geo-redundantie en geo-aanwezigheid**
    - U kunt instellen uw eigen geo-replicatie of synchronisatie met beveiligde verbindingen via het Internet facing eindpunten zonder.
    - Met Azure Load Balancer en Microsoft of derden clusteringtechnologie, kunt u instellen maximaal beschikbare werkbelasting met geo-redundantie tussen meerdere Azure regio's. Een belangrijk voorbeeld is voor het instellen van SQL altijd op met beschikbaarheidsgroepen spreiden over meerdere Azure regio's.

- **Regionale meerlagige toepassingen met sterke isolatie grens**
    - In hetzelfde gebied, kunt u toepassingen met meerdere niveaus instellen met meerdere VNets en sterke isolatie en veilige communicatie van de laag tussen verbonden.

- **Cross-communicatie tussen organisatie in Azure-abonnement**
    - Als er meerdere Azure abonnementen, kunt u de werklast van verschillende abonnementen samen veilig tussen virtuele netwerken.
    - Voor ondernemingen of dienstverleners kunt u de gehele organisatie communicatie met beveiligde VPN-technologie in Azure.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>VNet-VNet-Veelgestelde vragen over het klassieke VNets

- De virtuele netwerken kunnen worden in dezelfde of verschillende abonnementen.

- De virtuele netwerken kunnen worden in dezelfde of een andere Azure regio's (locatie).

- Een cloud-service of een eindpunt voor taakverdeling niet kan overspannen virtuele netwerken, zelfs als ze met elkaar zijn verbonden.

- Meerdere virtuele netwerken aan elkaar koppelen, is het niet nodig alle VPN-apparaten.

- VNet-VNet-ondersteunt verbinding Azure virtuele netwerken. Het ondersteunt aansluitende virtuele machines of cloud services die niet worden geïmplementeerd op een virtueel netwerk.

- VNet-VNet-vereist dynamische routering gateways. Azure statische routering gateways worden niet ondersteund.

- Verbinding met het virtuele netwerk kunt tegelijkertijd met meerdere site VPN-verbindingen worden gebruikt. Er is een maximum van 10 VPN-tunnels voor een virtueel netwerk VPN-gateway verbinding maken met andere virtuele netwerken of sites op gebouwen.

- De adresruimte van de virtuele netwerken en op ruimten lokale netwerksites moeten niet overlappen. Overlappende adresruimten leidt ertoe dat het maken van virtuele netwerken of de bestanden zijn geüpload netcfg configuratie mislukt.

- Redundante tunnels tussen twee virtuele netwerken worden niet ondersteund.

- Alle VPN-tunnels voor de VNet, P2S VPN's, inclusief delen de beschikbare bandbreedte voor de VPN-gateway en de uptime van dezelfde VPN-gateway SLA in Azure.

- VNet-VNet-verkeer gaat via de backbone Azure.


## <a name="step1"></a>Stap 1: uw IP-adresbereiken plannen

Het is belangrijk om te bepalen van de bereiken die u gebruiken zult om uw virtuele netwerken configureren. Bij deze configuratie moet u ervoor zorgen dat geen van de VNet bereiken overlappen elkaar, of met een van de lokale netwerken waarmee ze verbinding met maken.

De volgende tabel ziet u een voorbeeld van het definiëren van uw VNets. Gebruik de bereiken als richtsnoer alleen. Noteer de bereiken voor uw virtuele netwerken. U hebt deze informatie nodig voor latere stappen.

**Van de voorbeeldinstellingen**

|Virtueel netwerk  |Adresruimte               |Regio      |Maakt verbinding met het lokale netwerksite|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |Verenigde Staten West     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |East Japan  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Stap 2: VNet1 maken

In deze stap maken gaan we VNet1. Als u een van de voorbeelden, moet u vervangen door uw eigen waarden. Als uw VNet al bestaat, moet u niet deze stap doen. Maar als u wilt controleren of de IP-adresbereiken niet overlappen met de bereiken voor de tweede VNet of met een van de andere VNets waarmee u verbinding wilt maken.

1. Log in op de [Azure klassieke portal](https://manage.windowsazure.com). In dit artikel gebruiken we de klassieke portal omdat sommige van de vereiste configuratie-instellingen nog niet beschikbaar in de portal Azure.

2. Klik op **Nieuw**in de linkerbenedenhoek van het scherm > **Network Services** > **Virtueel netwerk** > **Aangepaste maken** om te beginnen met de configuratiewizard. Als u de wizard hebt doorlopen, moet u de opgegeven waarden toevoegen aan elke pagina.

### <a name="virtual-network-details"></a>Virtueel netwerkdetails

Voer op de pagina virtuele netwerkdetails de volgende gegevens:

  ![Virtueel netwerkdetails](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Name** - de naam van het virtuele netwerk. Bijvoorbeeld VNet1.
  - **Locatie** : als u een virtueel netwerk maakt, koppelt u deze aan een Azure locatie (regio). Als u uw VMs die zijn geïmplementeerd op het virtuele netwerk wilt op fysiek bevindt zich in de VS West, selecteert u die locatie in. De locatie die is gekoppeld aan het virtuele netwerk nadat u deze hebt gemaakt kunt u niet wijzigen.

### <a name="dns-servers-and-vpn-connectivity"></a>DNS-Servers en VPN-verbindingen

Voer de volgende gegevens op de pagina DNS-Servers en VPN-verbindingen en klik vervolgens op de pijl volgende gemarkeerd in de rechterbenedenhoek.

  ![DNS-Servers en VPN-verbindingen](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **DNS-Servers** - Voer de naam van de DNS-server en IP-adres of Selecteer een eerder geregistreerde DNS-server in de vervolgkeuzelijst. Deze instelling maakt een DNS-server. U kunt de DNS-servers opgeven die u wilt gebruiken voor het herleiden van namen voor deze virtuele netwerk. Als u naamomzetting tussen uw virtuele netwerken wilt gebruiken, hebt u voor het configureren van DNS-server, in plaats van met behulp van de naamomzetting die wordt geleverd door Azure.
- Selecteert u een van de selectievakjes in voor de connectiviteit van P2S of S2S. Klik op de pijl rechts om naar het volgende scherm te gaan.

### <a name="virtual-network-address-spaces"></a>Virtueel netwerk adresruimten

Geef het bereik van adressen die u wilt gebruiken voor het virtuele netwerk op de pagina virtuele netwerk adresruimten. Dit zijn de dynamische IP-adressen (SPANNINGSDIPS) die wordt toegewezen aan de VMs en andere rol exemplaren die u met dit virtuele netwerk implementeert. 

Als u een VNet die ook een verbinding met het netwerk op gebouwen maakt, is het vooral belangrijk is om een bereik selecteren dat niet overlapt met een van de bereiken die worden gebruikt voor het netwerk op gebouwen. In dat geval moet u samenwerken met uw netwerkbeheerder. De netwerkbeheerder kan moet kunnen fungeren van een bereik van IP-adressen van uw space op ruimten netwerk adres kunt gebruiken voor uw VNet.

  ![Virtuele adresruimte netwerk pagina](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Address Space** - met inbegrip van begin-IP- en een aantal adressen. Controleer of dat de adresruimte die u opgeeft met een van de adresruimte die u in uw netwerk op gebouwen hebt in elkaar niet overlappen. In dit voorbeeld gebruiken we 10.1.0.0/16 voor VNet1.
  - **Subnet toevoegen** - met inbegrip van begin-IP- en een aantal adressen. Extra subnetten zijn niet vereist, maar u kunt een apart subnet maken voor VMs met statische SPANNINGSDIPS. Of u kunt uw VMs hebben in een subnet dat losstaat van de andere exemplaren van de rol.
 
**Klik op het vinkje** in de rechterbenedenhoek van de pagina en het virtuele netwerk begint te maken. Wanneer deze is voltooid, ziet u 'Gemaakt' weergegeven onder Status op de pagina netwerken.

## <a name="step-3---create-vnet2"></a>Stap 3: VNet2 maken

Vervolgens herhaalt u de voorgaande stappen om een andere VNet te maken. In de volgende stappen maakt u verbinding de twee VNets. U kunt verwijzen naar het [van de voorbeeldinstellingen](#step1) in stap 1. Als uw VNet al bestaat, moet u niet deze stap doen. Echter, moet u controleren of dat het IP-adresbereiken elkaar niet overlappen met een van de andere VNets of op gebouwen netwerken waarmee u verbinding wilt maken.

## <a name="step-4---add-the-local-network-sites"></a>Stap 4: het lokale netwerksites toevoegen

Wanneer u een VNet-VNet-configuratie maakt, moet u lokaal netwerksites, die worden weergegeven in de **Lokale netwerken** van de portal te configureren. Azure gebruikt de instellingen van elke site lokaal netwerk voor informatie over het routeren van netwerkverkeer tussen de VNets. U bepaalt de naam die u wilt gebruiken om te verwijzen naar elke locatie van het lokale netwerk. Het is raadzaam als u wilt gebruiken een beschrijvende, selecteert u de waarde in een vervolgkeuzelijst in de volgende stappen.

Bijvoorbeeld VNet1 maakt verbinding met een lokaal netwerksite die u maakt met de naam 'VNet2Local'. De instellingen voor VNet2Local bevatten de adresprefixen voor VNet2 en een openbaar IP-adres voor de gateway VNet2. VNet2 verbinding met een lokaal netwerksite die u maakt met de naam 'VNet1Local' met de adresprefixen voor VNet1 en het openbare IP-adres voor de gateway VNet1.

### <a name="localnet"></a>De VNet1Local van het lokale netwerk-site toevoegen

1. Klik op **Nieuw**in de linkerbenedenhoek van het scherm > **Network Services** > **Virtueel netwerk** > **Lokaal netwerk toevoegen**.

2. Voer op de pagina **Geef de gegevens van uw lokale netwerk** **naam**een naam die u wilt gebruiken voor het netwerk waarmee u verbinding wilt maken. In dit voorbeeld kunt u 'VNet1Local' gebruiken om te verwijzen naar de gateway en het IP-adresbereiken voor VNet1.

3. Geef een geldig openbaar IP-adres voor **VPN-apparaat-IP-adres (optioneel)**. Normaal gesproken zou u het externe IP-adres gebruiken voor een VPN-apparaat. Voor VNet-VNet-configuraties gebruikt u het openbare IP-adres dat is toegewezen aan de gateway voor uw VNet. Maar gezien het feit dat u nog niet de gateway hebt gemaakt, kunt u een geldig openbaar IP-adres als een tijdelijke aanduiding. Niet leeg laten - het is niet optioneel voor deze configuratie. In een latere stap teruggaan naar deze instellingen en met de desbetreffende gateway-IP-adressen configureren, nadat Azure genereert. Klik op de pijl om door te gaan naar het volgende scherm.

4. Voer het IP-adres bereik en adres aantal voor VNet1 op **de pagina adres opgeven**. Dit moet komen exact overeen met het bereik dat is geconfigureerd voor VNet1. Azure maakt gebruik van de IP-adresbereiken die u opgeeft voor het doorsturen van het verkeer voor VNet1. Klik op het selectievakje om het lokale netwerk te maken.

### <a name="add-the-local-network-site-vnet2local"></a>De VNet2Local van het lokale netwerk-site toevoegen

Gebruik de bovenstaande stappen voor het maken van het lokale netwerksite 'VNet2Local'. Indien nodig kunt u verwijzen naar de waarden in het [van de voorbeeldinstellingen](#step1) in stap 1.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Elke VNet om te verwijzen naar een lokaal netwerk configureren

Elke VNet moet verwijzen naar de respectieve lokale netwerk die u wilt routeren van verkeer. 

#### <a name="for-vnet1"></a>Voor VNet1

1. Ga naar de pagina **configureren** voor virtuele netwerk **VNet1**. 
2. Selecteer "Verbinden met het lokale netwerk" onder verbinding van site naar site, en selecteer **VNet2Local** als het lokale netwerk in de vervolgkeuzelijst. 
3. Sla uw instellingen.

#### <a name="for-vnet2"></a>Voor VNet2

1. Ga naar de pagina **configureren** voor virtuele netwerk **VNet2**. 
2. Onder verbinding van site naar site Selecteer selecteert u 'Verbinding maken met het lokale netwerk', **VNet1Local** in de vervolgkeuzelijst als het lokale netwerk. 
3. Sla uw instellingen.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Stap 5 - gateway configureren voor elke VNet

Een dynamische routering-gateway voor elke virtuele netwerk configureren. Deze configuratie biedt geen ondersteuning voor statische routering gateways. Als u de VNets die eerder zijn geconfigureerd en dynamische routering gateways al hebben, moet u niet deze stap doen. Als de gateways statische routering, moet u deze verwijderen en ze opnieuw te maken als dynamische routering gateways. Als u een gateway verwijdert, het openbare IP-adres toegewezen wordt vrijgegeven en u wilt teruggaan en het opnieuw configureren van uw lokale netwerken en VPN-apparaten met de nieuwe openbare IP-adres voor de nieuwe gateway.

1. Controleer of de statuskolom van het virtuele netwerk is **gemaakt**op de pagina **netwerken** .

2. Klik in de kolom **naam** op de naam van het virtuele netwerk. In dit voorbeeld gebruiken we de "VNet1".

3. U ziet dat deze VNet geen gateway nog geconfigureerd op de **Dashboard** -pagina. U ziet deze status wijzigen terwijl u door de stappen voor het configureren van uw gateway.

4. Klik onderaan de pagina **Dynamische routering**- en **Gateway maken** . Wanneer het systeem wordt u gevraagd te bevestigen dat u wilt dat de gateway is gemaakt, klikt u op Ja.

    ![Type gateway](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Als uw gateway maakt, ziet u de afbeelding van de gateway op de pagina wordt gewijzigd in geel en "Gateway maken", zegt. Het duurt meestal ongeveer 30 minuten voor de gateway te maken.

6. Herhaal dezelfde stappen voor VNet2. U hoeft niet de eerste gateway VNet voltooien voordat u begint met het maken van de gateway voor de andere VNet.

7. Wanneer de status van de gateway verandert in 'Verbinding maken', is het openbare IP-adres voor de gateway zichtbaar in het Dashboard. Noteer het IP-adres dat overeenkomt met voor elke VNet, om ze van elkaar niet te bekommeren. Dit zijn de IP-adressen die worden gebruikt wanneer u de IP-adressen van de tijdelijke aanduiding voor het VPN-apparaat voor elk lokaal netwerk kunt bewerken.

## <a name="step-6---edit-the-local-network"></a>Stap 6: het bewerken van het lokale netwerk

1. Op de pagina **Lokale netwerken** , klikt u op de naam van het lokale netwerk dat u wilt bewerken, klik op **bewerken** onder aan de pagina. Voor **VPN-apparaat IP-adres**, het IP-adres van de gateway die overeenkomt met de VNet worden ingevoerd. Bijvoorbeeld, VNet1Local, plaatst het gateway-IP-adres toegewezen aan VNet1. Klik vervolgens op de pijl onder aan de pagina.

2. Klik op het vinkje in de rechterbenedenhoek zonder wijzigingen op de pagina **Geef de adresruimte** .

## <a name="step-7---create-the-vpn-connection"></a>Stap 7: de VPN-verbinding maken

Wanneer u alle voorgaande stappen zijn voltooid, stelt u het IPsec IKE/vooraf-gedeelde sleutels en de verbinding te maken. Deze reeks stappen gebruikt PowerShell en kan niet worden geconfigureerd in de portal. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de Azure PowerShell-cmdlets. Zorg ervoor dat de nieuwste versie van de Service Management (SM)-cmdlets downloaden. 

1. Windows PowerShell te openen en zich aanmelden.

        Add-AzureAccount

2. Selecteer het abonnement die zich in de VNets bevinden.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. De verbindingen maken. In de voorbeelden ziet u de gedeelde sleutel is precies hetzelfde. De gedeelde sleutel moet dit altijd overeenkomen.


    VNet1 VNet2-verbinding

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1-verbinding

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Wacht tot de verbindingen te initialiseren. Zodra de gateway is geïnitialiseerd, wordt de gateway ziet eruit als in de volgende afbeelding.

    ![De Status van de gateway - verbinding](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Volgende stappen

U kunt virtuele machines toevoegen aan uw virtuele netwerken. Raadpleeg de [documentatie van de virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
