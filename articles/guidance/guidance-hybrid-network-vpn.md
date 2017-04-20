<properties
   pageTitle="Implementatie van een hybride netwerkarchitectuur met Azure en On-premises VPN | Microsoft Azure"
   description="Het implementeren van een beveiligde site naar site netwerkarchitectuur die omvat een Azure virtual network en een netwerk op ruimten is verbonden via een VPN."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Een hybride netwerkarchitectuur met Azure en On-premises-VPN implementeren

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel worden een aantal methoden voor het uitbreiden van een netwerk op ruimten naar Azure met behulp van een site naar site virtueel particulier netwerk (VPN). De verkeersstromen tussen het netwerk van lokalen en een Azure Virtual Network (VNet) via een IPSec VPN-tunnel. Deze architectuur is geschikt voor hybride toepassingen met de volgende kenmerken:

- Onderdelen van de toepassing uitgevoerd op gebouwen, terwijl anderen in Azure worden uitgevoerd.

- Het verkeer tussen op-premises hardware en de cloud is waarschijnlijk licht of is het nuttig om de handel, enigszins uitgebreid latentie voor de flexibiliteit en de verwerkingskracht van de wolk.

- Het uitgebreide netwerk vormt een gesloten systeem. Er is geen directe pad van het Internet naar de Azure-VNet.

- Gebruikers verbinding maken met het netwerk op ruimten met gebruik van de services die worden gehost in Azure. De brug tussen het netwerk van gebouwen en de services die worden uitgevoerd in Azure is transparant voor de gebruikers.

Voorbeelden van scenario's die aan dit profiel zijn:

- Line-of-business-toepassingen die worden gebruikt binnen een organisatie, waar het deel van de functionaliteit is gemigreerd naar de cloud.

- Ontwikkeling/testlab werkbelasting.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Azure Resource Manager] [ resource-manager-overview] en klassiek. Deze blauwdruk gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de onderdelen in deze architectuur:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de 'hybride netwerk - VPN' pagina.

![[0]][0]

- **Netwerk van gebouwen.** Dit is een netwerk van computers en apparaten, verbonden via een particuliere LAN-netwerk binnen een organisatie worden uitgevoerd.

- **VPN-toestel.** Dit is een apparaat of een service voor externe verbinding met het netwerk op gebouwen. Het VPN-apparaat is mogelijk een hardware-apparaat of kan een softwareoplossing zoals de Routing and Remote Access Service (RRAS) in Windows Server 2012.

> [AZURE.NOTE] Zie de instructies van het betreffende apparaat in de [lijst met VPN-apparaten worden ondersteund door Azure]voor een lijst met ondersteunde VPN-apparaten en informatie over het configureren van de geselecteerde VPN-apparaten voor een verbinding met een VPN-Gateway van Azure,[vpn-appliance].

- **Wolk van N-tier-toepassing.** Dit is de toepassing die wordt gehost in Azure. Het kan meerdere lagen, met meerdere subnetten zijn verbonden via Azure Netwerktaakverdeling bevatten. Het verkeer in elk subnet kan worden onderworpen aan de regels die zijn gedefinieerd met behulp van [Beveiligingsgroepen Azure-netwerk (NSGs)][azure-network-security-group]. Zie voor meer informatie, [aan de slag met Azure Microsoft security][getting-started-with-azure-security].

> [AZURE.NOTE] Dit artikel beschrijft de wolk toepassing als één entiteit. [Een N-lagige architectuur op Azure uitgevoerd] Zie[ implementing-a-multi-tier-architecture-on-Azure] voor meer informatie.

- **[Virtueel netwerk (VNet)][azure-virtual-network].** De wolk toepassing en de onderdelen voor de VPN-Gateway Azure bevinden zich in de dezelfde VNet.

- **[Azure VPN-Gateway][azure-vpn-gateway].** De VPN-gatewayservice kunt u de VNet verbinding met het netwerk op locatie via een VPN-apparaat. Zie voor meer informatie [verbinding maken met een netwerk van gebouwen met een virtueel netwerk Microsoft Azure][connect-to-an-Azure-vnet]. De VPN-Gateway omvat de volgende elementen:

  - **Virtueel netwerkgateway.** Dit is een bron met een virtuele VPN-toestel voor de VNet. Is verantwoordelijk voor het routeren van verkeer van het netwerk op ruimten aan de VNet.

  - **LAN gateway.** Dit is een abstractie van het toestel op ruimten VPN. Netwerkverkeer van de wolk toepassing met het netwerk op locatie worden gerouteerd via deze gateway.

  - **De verbinding.** De verbinding heeft eigenschappen die het verbindingstype (IPSec) en de gedeeld met het toestel op ruimten VPN-sleutel voor het coderen van verkeer.

  - **Subnetmasker, gateway.** Het virtuele netwerkgateway wordt vastgehouden in een eigen subnet waarvoor verschillende vereisten, zoals beschreven in de onderstaande sectie voor aanbevelingen.

- **Interne taakverdeling.** Verkeer van de VPN-Gateway gerouteerd naar de cloud toepassing via een interne taakverdeling. De taakverdeling bevindt zich in het subnet front-end van de toepassing.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="vnet-and-gateway-subnet"></a>Subnetmasker en de Gateway VNet

Maak een Azure-VNet voor de opslag van de onderdelen in de cloud. De adresruimte van de Azure VNet moet groot genoeg voor de adressen die worden gebruikt door het VMs en subnetten in de VNet. Zorg ervoor dat de VNet-adresruimte voldoende ruimte om te groeien als extra VMs zijn waarschijnlijk in de toekomst nodig is. De adresruimte van de VNet moet niet met het netwerk op ruimten overlappen. Het bovenstaande diagram wordt het 10.20.0.0/16 adres ruimte gebruikt voor de VNet.

Een subnet met een adresbereik van /27 _GatewaySubnet_, de naam maken. Dit subnet wordt vereist door de gateway virtueel netwerk en het toewijzen van 32 adressen op dit subnet helpt om te voorkomen dat u in de toekomst zich verhouden tot de mogelijke gateway groottebeperkingen uitgevoerd. Plaats dit subnet in het midden van de adresruimte. Het wordt aangeraden de adresruimte voor de gateway-subnet aan de bovenkant van de adresruimte van het VNet instellen. 10.20.255.224/27 maakt gebruik van het voorbeeld in het diagram weergegeven.  Een snelle procedure voor het berekenen van de CIDR is als volgt:

1. Stel de variabele bits in de adresruimte van de VNet 1 tot en met de bits wordt gebruikt door het subnet van de gateway en vervolgens de overige bits ingesteld op 0.

2. De resulterende bits converteren naar een decimaal getal en druk een adresruimte met het voorvoegsel lengte is ingesteld op de grootte van het subnet van de gateway.

Voor een VNet met een IP-adresbereik van 10.20.0.0/16 wordt toe te passen stap #1 hierboven bijvoorbeeld 10.20.0b11111111.0b11100000.  Dat converteren naar een decimaal getal en uitdrukken, zoals een adresruimte 10.20.255.224/27 levert

> [AZURE.WARNING] Implementeer andere virtuele machines of rol exemplaren niet naar het subnet van de gateway. Ook geen toewijst een NSG op dit subnet, omdat de gateway niet meer werkt.

### <a name="virtual-network-gateway"></a>Virtueel netwerkgateway

Een openbaar IP-adres voor de gateway virtueel netwerk toewijzen.

De gateway virtueel netwerk maken in het subnet van de Gateway en de nieuw toegewezen openbare IP-adres toewijzen. Gebruik de gateway die het meest overeenkomt met uw vereisten en die door de VPN-toestel is ingeschakeld:

Maken van een [gateway op beleid gebaseerde] [ policy-based-routing] als u nauwkeurig bepalen hoe aanvragen worden doorgestuurd. op basis van criteria zoals adresprefixen beleid. Op beleid gebaseerde gateways met statische routering en alleen werken met verbindingen van site naar site.

Een [gateway op basis van een route] maken[ route-based-routing] als u verbinding met het netwerk op ruimten is met RRAS maakt, ondersteuning voor verbindingen met meerdere locaties of cross-regio of VNet-VNet-verbindingen (met inbegrip van de routes die door meerdere VNets) implementeert. Op basis van route gateways gebruiken dynamische routering naar directe netwerkverkeer tussen netwerken. Zij kunnen storingen in het netwerkpad beter dan statische routes tolereren omdat ze alternatieve routes proberen kunnen. Gateways op basis van een route kunnen ook de overhead, verminderd omdat routes niet wellicht handmatig moeten worden bijgewerkt wanneer netwerkadressen wijzigen.

Zie voor een lijst met ondersteunde VPN-apparaten, [over VPN-apparaten voor verbindingen van Site naar Site VPN-Gateway][vpn-appliances].

> [AZURE.NOTE] Nadat u de gateway hebt gemaakt, kunt u niet wijzigen tussen gateway typen zonder te verwijderen en opnieuw maken van de gateway.

Selecteer de SKU Azure VPN-Gateway die het meest overeenkomt met uw vereisten doorvoer. Azure VPN-Gateway is beschikbaar in drie SKU's weergegeven in de volgende tabel. Elke SKU biedt verschillende doorvoer, [toeslagen worden geheven] [ azure-gateway-charges] op basis van de hoeveelheid tijd die de gateway is ingericht en beschikbaar.

| SKU | VPN-doorvoer | Max IPSec-Tunnels|
|-----|----------------|------------------|
| Basic | 100 Mbps | 10 |
| Standaard | 100 Mbps | 10 |
| Hoge prestaties | 200 Mbps | 30 |

> [AZURE.NOTE] De Basic SKU is niet compatibel met Azure ExpressRoute. U kunt [de SKU wijzigen] [ changing-SKUs] nadat u de gateway hebt gemaakt.

Routeringsregels voor het subnet van de gateway die directe binnenkomend verkeer voor toepassing van de gateway naar de interne taakverdeling in plaats van aanvragen die worden doorgegeven aan de VMs die de toepassing implementeren direct toestaan maken.

### <a name="on-premises-network-connection"></a>Netwerkverbinding in gebouwen

De gateway van een lokale netwerk maken. Geef het openbare IP-adres van het toestel op ruimten VPN en de adresruimte van het netwerk op gebouwen. Houd er rekening mee dat het toestel op ruimten VPN moet een openbaar IP-adres dat toegankelijk is voor het lokale netwerkgateway in Azure VPN-Gateway. Het VPN-apparaat kan niet zich bevinden achter een NAT-apparaat.

Maak een verbinding van site naar site voor virtueel netwerkgateway en de gateway voor het lokale netwerk. Selecteer het verbindingstype van Site naar site (IPSec) en de gedeelde sleutel opgeven. Codering met de Azure VPN-Gateway-website is gebaseerd op het IPSec-protocol, met behulp van vooraf-gedeelde sleutels voor de verificatie. U kunt de sleutel opgeven wanneer u de Azure VPN-Gateway maakt. Moet u de VPN-toestel op locatie uitgevoerd met dezelfde sleutel. Andere verificatiemechanismen worden momenteel niet ondersteund.

Ervoor te zorgen dat de routeringsinfrastructuur is geconfigureerd om aanvragen doorsturen die bestemd zijn voor adressen in de Azure-VNet voor het VPN-apparaat op gebouwen.

Open poorten vereist door de toepassing van de wolk in het netwerk op gebouwen.

Test de verbinding om te controleren:

- Het toestel op ruimten VPN goed routeert verkeer naar de cloud toepassing via de Azure VPN-Gateway.

- De VNet goed routeert verkeer naar het netwerk van gebouwen.

- Verboden verkeer in beide richtingen wordt juist geblokkeerd.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

Beperkte verticale schaalbaarheid kunt u bereiken door het verplaatsen van de Basic- of VPN-Gateway-SKU's standaard naar de hoge prestaties van VPN-SKU.

Voor VNets die een groot volume van VPN-verkeer wordt verwacht, kunt u de distributie van de verschillende werkbelastingen in verschillende kleinere VNets en een VPN-Gateway configureren voor elk van hen.

U kunt de VNet partitioneren horizontaal of verticaal. Verplaatsen om horizontaal partitioneren sommige VM exemplaren van elke laag in subnetten van de nieuwe VNet. Het resultaat is dat elke VNet dezelfde structuur en functionaliteit heeft. Ontwerpen om verticaal verdelen, elke laag om de functionaliteit te verdelen in verschillende logische gebieden (zoals het verwerken van orders, facturering, klant accountmanagement, enzovoort). Elk functioneel gebied kan vervolgens worden geplaatst in een eigen VNet.

Een Active Directory-domeincontroller op de locatie in de VNet te repliceren en het implementeren van DNS in de VNet, kunt u minder van de beveiligings- en administratieve verkeer die voortvloeien uit de op het bedrijf naar de cloud.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Als u nodig hebt om ervoor te zorgen dat het netwerk op lokalen beschikbaar voor de Azure VPN-Gateway blijven, voert u een failover-cluster voor de VPN-Gateway op gebouwen.

Als uw organisatie meerdere sites voor op gebouwen, [verbindingen met meerdere locaties] maken[ vpn-gateway-multi-site] voor een of meer Azure-VNets. Hiervoor zijn dynamische routering (route gebaseerd), dus zorg ervoor dat deze functie wordt ondersteund door de VPN-Gateway voor op gebouwen.

[SLA voor de VPN-Gateway] Zie[ sla-for-vpn-gateway] voor de details over de VPN-Gateway SLA.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

Diagnostische gegevens van voor bedrijfsruimten VPN-apparaten controleren. Dit proces is afhankelijk van de functies van het VPN-apparaat. Bijvoorbeeld, als u de Routing and Remote Access-Service op Windows Server 2012 gebruikt, u moet [RRAS logboekregistratie]inschakelen[rras-logging].

De [Diagnostische gegevens van Azure VPN-Gateway] gebruiken[ gateway-diagnostic-logs] voor het vastleggen van informatie over verbindingsproblemen. Deze logboeken kunnen worden gebruikt voor het bijhouden van informatie over de bron en het doel van een verbinding aanvragen, welk protocol is gebruikt, en hoe de verbinding tot stand is gebracht (of waarom de poging is mislukt).

De operationele logboeken van de VPN-Gateway Azure controleren met behulp van de controlelogboeken die beschikbaar zijn in de portal Azure. Afzonderlijke logboeken zijn beschikbaar voor de gateway voor het lokale netwerk, de netwerkgateway Azure en de verbinding. Deze informatie kan worden gebruikt voor het bijhouden van wijzigingen die de gateway en is handig als u een eerder werkende gateway werkt niet om een bepaalde reden.

![[2]][2]

Controleren, connectiviteit en connectiviteit mislukte gebeurtenissen bijhouden. U kunt een pakket controle zoals [Nagios] [ nagios] vastleggen en rapporteren deze informatie.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Genereer een andere gedeelde sleutel voor elke VPN-gateway. Met een sterke gedeelde sleutel te verzetten brute-force aanvallen.

> [AZURE.NOTE] U kunt Azure sleutel kluis op dit moment niet gebruiken voor vooraf-gedeelde sleutels Azure VPN-gateway.

Ervoor te zorgen dat een coderingsmethode die [compatibel]is met de Azure VPN-Gateway maakt gebruik van het toestel op ruimten VPN-[vpn-appliance-ipsec]. De Azure VPN-Gateway ondersteunt voor routering op basis van beleid, AES256, AES128 en 3DES-coderingsalgoritmen. Gateways op basis van een route ondersteunen AES256 en 3DES.

Als uw VPN op ruimten toestel in een perimeternetwerk met een firewall tussen het perimeternetwerk en het Internet, u wellicht [Extra firewallregels] configureren[ additional-firewall-rules] zodat de VPN-verbinding van site naar site.

Als de toepassing in de VNet gegevens naar het Internet verzendt, kunt u [uitvoering van gedwongen tunneling] [ forced-tunneling] voor het routeren van alle Internet-verkeer dat via het netwerk op gebouwen. Deze benadering kunt u uitgaande aanvragen die zijn gedaan door de toepassing van de infrastructuur op het bedrijf controleren.

> [AZURE.NOTE] Geforceerde tunneling kan gevolgen hebben voor connectiviteit met Azure services (bijvoorbeeld de opslagservice) en de Windows License Manager.

## <a name="troubleshooting-considerations"></a>Overwegingen bij het oplossen van problemen

> [AZURE.NOTE] Ga naar de Routing and Remote Access-Blog voor algemene informatie over [het oplossen van veelvoorkomende fouten met betrekking tot VPN][troubleshooting-vpn-errors].

Als verkeer kan via de VPN-verbinding, controleert u het volgende:

### <a name="on-premises-vpn-appliance"></a>Over lokalen VPN-toestel:

**Het toestel op ruimten VPN goed functioneert?**

Selectievakje alle logboekbestanden die worden gegenereerd door de VPN-apparaat voor fouten en storingen. De locatie van deze gegevens zijn afhankelijk van uw toestel. Bijvoorbeeld, als u RRAS op Windows Server 2012 gebruikt, kunt u de volgende PowerShell-opdracht weer te geven informatie over de fout gebeurtenis voor de RRAS-service:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

De eigenschap _bericht_ van elke post bevat een beschrijving van de fout. Gangbare voorbeelden hiervan zijn:

- Onvermogen om verbinding worden gemaakt, mogelijk vanwege een onjuist IP-adres dat is opgegeven voor de Azure VPN-Gateway in de configuratie van de netwerkinterface RRAS VPN:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- De verkeerde gedeelde sleutel wordt opgegeven in de configuratie van de netwerkinterface RRAS VPN:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

U kunt ook gebeurtenislogboeken informatie verkrijgen over pogingen via de RRAS-service verbinding te maken met behulp van de volgende PowerShell-opdracht:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

Dit logboek bevat fouten die op het volgende lijkt bij een storing om verbinding te maken:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**De VPN-apparaat correct routeren van verkeer via de VPN-Gateway van Azure is?**

Met een hulpprogramma zoals [PsPing] [ psping] om te controleren of de verbindingen en routering via de VPN-gateway. Bijvoorbeeld, als u wilt testen van de connectiviteit van een computer op de locatie op een webserver op de VNet, de volgende opdracht uitvoeren (Vervang `<<web-server-address>>` met het adres van de webserver):

```
PsPing -t <<web-server-address>>:80
```

Als de machine op locatie verkeer naar de webserver doorsturen kunt, ziet u uitvoer van de volgende strekking:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Als de machine op locatie kan niet met de opgegeven bestemming communiceren, ziet u berichten als volgt:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Kan de firewall op ruimten VPN-verkeer via De juiste poorten zijn geopend?**

Controleer of dat een coderingsmethode die [compatibel]is met de Azure VPN-Gateway maakt gebruik van het toestel op ruimten VPN-[vpn-appliance].

De Azure VPN-Gateway ondersteunt voor routering op basis van beleid, AES256, AES128 en 3DES-coderingsalgoritmen. Gateways op basis van een route ondersteunen AES256 en 3DES.

### <a name="azure-vnet-gateway"></a>Azure VNet Gateway:

[Azure VPN-Gateway diagnostische logboeken] bekijken[ gateway-diagnostic-logs] voor mogelijke problemen.

**Zijn de Azure VPN-Gateway en op ruimten VPN-toestel met dezelfde verificatie met een gedeelde sleutel geconfigureerd?**

U kunt de gedeelde sleutel die is opgeslagen door de Azure VPN-Gateway met behulp van de volgende Azure CLI-opdracht weergeven:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Gebruik de opdracht geschikt is voor uw VPN op ruimten toestel weer te geven van de gedeelde sleutel die is geconfigureerd voor dat toestel.

Controleer of het bedrijf dat de Azure VPN-Gateway is niet gekoppeld aan een NSG _GatewaySubnet_ -subnet.

U kunt de details van het subnet bekijken met behulp van de volgende Azure CLI-opdracht:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Zorg er geen veld met de naam _beveiligingsgroep netwerk-id_. In het volgende voorbeeld ziet u de resultaten bijvoorbeeld van _GatewaySubnet_ met een toegewezen NSG (_VPN-Gateway-groep_). Dit kan ertoe leiden dat de gateway naar behoren werken als er regels gedefinieerd voor deze NSG voorkomen:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**De virtuele machines in de VNet Azure zodanig zijn geconfigureerd dat verkeer afkomstig van buiten de VNet? Controleer de NSG-regels die zijn gekoppeld aan deze virtuele machines met subnetten.**

U kunt alle regels van de NSG weergeven met behulp van de volgende Azure CLI-opdracht:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**De Azure VPN-Gateway verwijderd is?**

De volgende Azure PowerShell-opdracht kunt u de huidige status van de Azure VPN-verbinding. De `<<connection-name>>` parameter is de naam van de Azure VPN-verbinding die het virtuele netwerkgateway en de lokale gateway is gekoppeld.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

De volgende codefragmenten markeren de uitvoer gegenereerd als de gateway is (in het eerste voorbeeld) verbonden en verbroken (in het tweede voorbeeld):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>VM configuratie, netwerk bandbreedtegebruik en de prestaties van toepassingen:

Controleer of de firewall in het gastbesturingssysteem actief op de Azure VMs in het subnet correct zijn geconfigureerd voor het toegestane verkeer van de IP-adresbereiken op gebouwen.

**Het volume van verkeer dicht bij de grens van de bandbreedte beschikbaar is voor de Azure VPN-Gateway?**

Tooling is afhankelijk van de faciliteiten beschikbaar voor uw VPN-toestel op locatie uitgevoerd. Bijvoorbeeld als u RRAS op Windows Server 2012 gebruikt, kunt u Prestatiemeter om bij te houden van de omvang van de gegevens worden ontvangen en verzonden via de VPN-verbinding; Selecteer de items _Ontvangen Bytes per seconde_ en _Verzonden Bytes per seconde_ met de _Totale RAS-_ object:

![[3]][3]

U moet de resultaten vergelijken met de bandbreedte die beschikbaar is op de VPN-gateway (100 Mbps voor Basic en Standard SKU's en 200 Mbps voor de hoge prestaties SKU):

![[4]][4]

**Een van de virtuele machines in de VNet Azure langzaam worden uitgevoerd? Ze overbelast zijn, zijn er genoeg van hen voor het verwerken van de belasting, alle Netwerktaakverdeling juist geconfigureerd zijn?**

U moet hiervoor [het vastleggen en analyseren van diagnostische gegevens][azure-vm-diagnostics]. Bekijk de resultaten met behulp van de portal Azure, maar zijn ook vele hulpprogramma's van derden beschikbaar die krijgt gedetailleerde inzichten in de prestatiegegevens.

**Is de toepassing het efficiënt maken gebruik van de cloud?**

Instrument toepassingscode uitgevoerd op elke VM om te bepalen of toepassingen optimaal gebruik van middelen maakt. Kunt u hulpprogramma's zoals de [Toepassing inzichten] [ application-insights] om deze taken uit te voeren.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Als u een bestaande op gebouwen infrastructuur reeds geconfigureerd met een VPN-apparaat hebt, kunt u de referentiearchitectuur implementeren door de volgende stappen:

1. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Wachten op de koppeling in de Azure portal te openen en vervolgens als volgt te werk: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-hybrid-vpn-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

## <a name="next-steps"></a>Volgende stappen

- [Installatie van extra domeincontrollers voor op ruimten Active Directory-domein met VMs in een Azure-VNet][installing-ad].

- [Richtlijnen voor de implementatie van Windows Server Active Directory op Azure VMs][deploying-ad].

- [Een DNS-server maken in een VNet][creating-dns].

- [Configureren en beheren van DNS-server in een VNet][configuring-dns].

- [Met behulp van lokalen Stormshield netwerk beveiligingsapparaten via een VPN][stormshield].

- [Implementatie van een hybride netwerkarchitectuur met Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Structuur van een hybride netwerk spanning op gebouwen en cloud-infrastructuren"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Partitionering van een VNet voor een betere schaalbaarheid"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Controlelogboeken in Azure portal"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Prestatiemeteritems voor VPN-netwerkverkeer controleren"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Voorbeeld van de VPN-netwerk Prestatiegrafiek"