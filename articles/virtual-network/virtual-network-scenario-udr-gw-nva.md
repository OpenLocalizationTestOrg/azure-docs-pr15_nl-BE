<properties 
   pageTitle="Hybride verbinding met 2-tier toepassing | Microsoft Azure"
   description="Informatie over het implementeren van virtuele apparaten en UDR een omgeving met meerdere niveaus maken in Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Virtueel toestel scenario

Een algemeen scenario tussen grotere Azure klant is aan een toepassing twee lagen blootgesteld aan Internet, terwijl toegang tot de vorige laag van een datacenter op ruimten moeten. Dit document helpt u bij een scenario waarbij de gebruiker gedefinieerd Routes (UDR), een VPN-Gateway en virtuele toestellen voor netwerken implementeren een tweelagige omgeving die voldoet aan de volgende vereisten:

- Webtoepassing moet toegankelijk zijn via openbare Internet.
- Webserver die als host fungeert voor de toepassing moet toegang kunnen krijgen tot een back-end application server.
- Alle verkeer vanaf het Internet naar de webtoepassing moet gaan via een firewall virtueel toestel. Deze virtuele toestel wordt gebruikt voor Internet-verkeer.
- De application server zal al het verkeer moet via een firewall virtueel toestel gaan. Deze virtuele toestel wordt gebruikt voor toegang tot de back-endserver einde, en afkomstig van het netwerk op gebouwen via een VPN-Gateway.
- Beheerders kunnen de firewall virtuele apparaten beheren vanaf hun computers op het bedrijf moeten zijn, met behulp van een derde firewall virtueel toestel alleen voor beheerdoeleinden gebruikt.

Dit is een standaard DMZ scenario met een DMZ en een beveiligd netwerk. Dit scenario kan worden samengesteld in Azure met NSGs, virtuele firewalltoepassingen of een combinatie van beide. In de volgende tabel bevat enkele van de voor- en nadelen tussen NSGs en virtuele firewalltoepassingen.

||Pros|Nadelen|
|---|---|---|
|NSG|Geen kosten. <br/>Geïntegreerd in Azure RBAC. <br/>Regels kunnen worden gemaakt in de ARM sjablonen.|Omgevingen met grotere kan complexiteit variëren. |
|Firewall|Volledige controle over data vlak. <br/>Centraal beheer via de console van de firewall.|De kosten van het firewall-apparaat. <br/>Niet geïntegreerd met Azure RBAC.|

De onderstaande oplossing gebruikt virtuele firewalltoepassingen een DMZ/beveiligde netwerk te implementeren.

## <a name="considerations"></a>Overwegingen met betrekking tot

U kunt de omgeving uiteengezet in Azure met verschillende functies die beschikbaar zijn vandaag, als volgt implementeren.

- **Virtueel netwerk (VNet)**. Een Azure-VNet werkt op vergelijkbare wijze met een netwerk van lokalen en kan worden opgesplitst in een of meer subnetten voor verkeer, isolatie en afscheiding van de problemen.
- **Virtueel toestel**. Verschillende partners bieden virtuele toestellen in Azure Marktonderzoek dat kan worden gebruikt voor de drie firewalls die hierboven is beschreven. 
- **Door de gebruiker gedefinieerde Routes (UDR)**. Routetabellen kunnen UDRs door Azure netwerken gebruikt voor de gegevensoverdracht van pakketten binnen een VNet bevatten. Deze routetabellen kunnen worden toegepast op subnetten. Een van de nieuwste functies in Azure is de mogelijkheid om een tabel van toepassing op de GatewaySubnet, de mogelijkheid voor het doorsturen van al het verkeer in de Azure VNet afkomstig van een hybride-verbinding op een virtueel toestel.
- **Doorsturen via IP**. Standaard de Azure netwerken engine doorsturen van pakketten naar virtuele netwerkinterfacekaarten (NIC's) alleen als het IP-doeladres van het pakket overeenkomt met de NIC IP-adres. Als een UDR definieert dat een pakket moet worden verzonden naar een bepaald virtueel toestel, zou de Azure netwerken engine dus dat pakket weghalen. Het pakket wordt bezorgd in een VM (in dit geval een virtueel toestel) die niet de werkelijke bestemming van het pakket, zodat moet u IP-doorsturen inschakelen voor het virtuele toestel.
- **Netwerk-beveiligingsgroepen (NSGs)**. In het volgende voorbeeld maakt geen gebruik van NSGs, maar u kunt NSGs toegepast op de subnetten en/of NIC's in deze oplossing voor het filteren van het verkeer in en uit deze subnetten en NIC's verder.


![IPv6-connectiviteit](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

In dit voorbeeld is er een abonnement dat het volgende bevat:

- 2 resourcegroepen niet weergegeven in het diagram. 
    - **ONPREMRG**. Bevat alle resources die nodig zijn voor het simuleren van een netwerk op gebouwen.
    - **AZURERG**. Bevat alle resources die nodig zijn voor de Azure virtuele netwerkomgeving. 
- Een VNet met de naam **onpremvnet** gebruikt om na te bootsen van een in de lokalen datacenter onderstaande gesegmenteerd.
    - **onpremsn1**. Subnet met een virtuele machine (VM) met Ubuntu om na te bootsen van een server op locatie.
    - **onpremsn2**. Subnet met een VM met Ubuntu om na te bootsen van een computer op ruimten is gebruikt door een beheerder.
- Er is een firewall virtueel toestel met de naam **OPFW** op **onpremvnet** gebruikt voor het onderhouden van een tunnel naar **azurevnet**.
- Een VNet met de naam **azurevnet** gesegmenteerd zoals hieronder vermeld.
    - **azsn1**. Externe firewall subnet wordt uitsluitend gebruikt voor de externe firewall. Alle Internet-verkeer wordt ontvangen in dit subnet. Dit subnet bevat alleen een NIC die zijn gekoppeld aan de externe firewall.
    - **azsn2**. Front-end subnet die als host fungeert voor een VM wordt uitgevoerd als een webserver die wordt benaderd via het Internet.
    - **azsn3**. Back-end subnet in een VM met een back-end-toepassingsserver wordt benaderd door de front-end-webserver die als host fungeert.
    - **azsn4**. Beheer van subnet worden uitsluitend gebruikt voor beheertoegang bieden tot alle firewall virtuele toestellen. Dit subnet bevat alleen een NIC voor elke firewall virtueel toestel gebruikt om in de oplossing.
    - **GatewaySubnet**. Azure hybride verbinding subnet nodig voor ExpressRoute en VPN-Gateway voor de connectiviteit tussen Azure, VNets en andere netwerken. 
- Er zijn 3 virtuele firewall-apparaten in het netwerk **azurevnet** . 
    - **AZF1**. Externe firewall blootgesteld aan het openbare Internet via een openbaar IP-adresbron in Azure. U moet ervoor zorgen dat u een sjabloon hebt vanuit de markt of rechtstreeks van de leverancier van uw toestel, deze bepalingen een 3-NIC virtueel toestel.
    - **AZF2**. Interne firewall gebruikt om verkeer tussen **azsn2** en **azsn3**. Dit is ook een 3-NIC virtueel toestel.
    - **AZF3**. Firewall beheer voor beheerders toegankelijk vanuit het datacenter van lokalen en verbonden is met subnet management gebruikt voor het beheren van alle firewall-toestellen. U kunt 2 NIC virtueel toestel sjablonen zoeken op de markt of de aanvragen rechtstreeks bij de leverancier van uw toestel.

## <a name="user-defined-routing-udr"></a>Door de gebruiker gedefinieerde routering (UDR)

Elk subnet in Azure kan worden gekoppeld aan een tabel UDR wordt gebruikt om te definiëren hoe verkeer wordt gestart dat subnet wordt gerouteerd. Als er geen UDRs zijn gedefinieerd, Azure standaardroutes gebruikt om verkeer van een subnet naar een ander. Voor een beter begrip van UDRs, Ga naar [Wat zijn gebruiker gedefinieerd Routes en doorsturen via IP](./virtual-networks-udr-overview.md#ip-forwarding).

Zodat communicatie vindt plaats via de juiste firewall toestel, op basis van de laatste eis die hierboven, moet u de volgende routetabel met UDRs in **azurevnet**maken.

### <a name="azgwudr"></a>azgwudr

In dit scenario, alleen verkeer die voortvloeien uit in de lokalen voor Azure wordt gebruikt voor het beheren van de firewalls door verbinding te maken met **AZF3**en dat verkeer door de interne firewall, **AZF2**moet gaan. Slechts één route is daarom noodzakelijk in **GatewaySubnet** zoals hieronder wordt weergegeven.

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Op gebouwen gegevensverkeer te bereiken management firewall **AZF3**|

### <a name="azsn2udr"></a>azsn2udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Gegevensverkeer op het back-end-subnet die als host fungeert voor de toepassingsserver via **AZF2**|
|0.0.0.0/0|10.0.2.10|Hiermee kunt al het andere verkeer moet worden doorgestuurd via **AZF1**|

### <a name="azsn3udr"></a>azsn3udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Gegevensverkeer naar **azsn2** van app server overlopen naar de webserver via **AZF2**|

U moet ook de om routetabellen te maken voor de subnetten in **onpremvnet** om na te bootsen het datacenter op gebouwen.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Gegevensverkeer op **onpremsn2** via **OPFW**|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Gegevensverkeer op het subnet van de back-ups in Azure via **OPFW**|
|192.168.1.0/24|192.168.2.4|Gegevensverkeer op **onpremsn1** via **OPFW**|

## <a name="ip-forwarding"></a>Doorsturen via IP 

UDR en doorsturen via IP zijn functies die u kunt gebruiken in combinatie om virtuele toestellen worden gebruikt voor het besturen van de verkeersstroom in een VNet Azure.  Een virtuele toestel is niets meer dan een VM met een toepassing die wordt gebruikt voor het verwerken van netwerkverkeer op een bepaalde manier, zoals een firewall of een NAT-apparaat.

Deze virtuele VM moet zich kunnen ontvangen van binnenkomend verkeer dat niet aan zichzelf wordt behandeld. Als u wilt toestaan dat een VM naar andere bestemmingen worden geadresseerd verkeer wordt ontvangen, moet u doorsturen via IP inschakelen voor VM. Dit is een instelling, niet een instelling in het gastbesturingssysteem Azure. Het virtuele toestel nog moet sommige type van toepassing voor het binnenkomende verkeer verwerken, en stuurt deze op de juiste wijze uitvoeren.

Voor meer informatie over doorsturen via IP, Ga naar [Wat zijn gebruiker gedefinieerd Routes en doorsturen via IP](./virtual-networks-udr-overview.md#ip-forwarding).

Stel, dat u hebt de volgende instellingen in een Azure vnet bijvoorbeeld:

- Subnet **onpremsn1** bevat een VM met de naam **onpremvm1**.
- Subnet **onpremsn2** bevat een VM met de naam **onpremvm2**.
- Een virtueel toestel met de naam **OPFW** is verbonden met **onpremsn1** en **onpremsn2**.
- Een route door de gebruiker gedefinieerd is gekoppeld aan de **onpremsn1** geeft aan dat alle verkeer voor **onpremsn2** moet worden verzonden naar **OPFW**.

Op dit moment **onpremvm1** probeert een verbinding maken met **onpremvm2**, de UDR wordt gebruikt als verkeer wordt verzonden naar de **OPFW** als de volgende hop. Houd er rekening mee dat de werkelijke pakketdoel niet wordt gewijzigd, wordt nog steeds aangegeven **onpremvm2** is de bestemming. 

Zonder doorsturen via IP ingeschakeld voor **OPFW**, drop de Azure virtuele netwerken logica van de pakketten, aangezien deze alleen pakketten worden verzonden naar een VM als van de VM-IP-adres de bestemming voor het pakket is.

Met het doorsturen van IP-stuurt de logica van Azure virtual network de pakketten naar OPFW, zonder de oorspronkelijke doeladres. **OPFW** moet de pakketten verwerken en bepalen wat u ermee kunt doen.

Voor het bovenstaande scenario wilt werken, u moet IP-doorsturen inschakelen op de NIC's voor **OPFW**, **AZF1**, **AZF2**en **AZF3** die worden gebruikt voor routering (alle NIC's, behalve die gekoppeld zijn aan het beheer van subnet). 

## <a name="firewall-rules"></a>Firewall-regels

Zoals hierboven beschreven, zorgt doorsturen via IP alleen voor pakketten worden verzonden naar de virtuele apparaten. Uw toestel moet nog beslissen wat te doen met de pakketten. In het bovenstaande scenario moet u de volgende regels maken in uw toestellen:

### <a name="opfw"></a>OPFW

OPFW geeft een apparaat aan op de gebouwen met de volgende regels:

- **Route**: alle verkeer naar 10.0.0.0/16 (**azurevnet**) via **ONPREMAZURE**tunnel moet worden verzonden.
- **Beleid**: alle bidirectionele verkeer tussen **port2** en **ONPREMAZURE**.
 
### <a name="azf1"></a>AZF1

AZF1 geeft een Azure virtueel toestel met de volgende regels:

- **Beleid**: alle bidirectionele verkeer tussen **port1** en **port2**.

### <a name="azf2"></a>AZF2

AZF2 geeft een Azure virtueel toestel met de volgende regels:

- **Route**: alle verkeer naar 10.0.0.0/16 (**onpremvnet**) moet worden verzonden naar de Azure gateway IP-adres (dat wil zeggen 10.0.0.1) via **port1**.
- **Beleid**: alle bidirectionele verkeer tussen **port1** en **port2**.

## <a name="network-security-groups-nsgs"></a>Netwerk-beveiligingsgroepen (NSGs)

In dit scenario worden niet NSGs gebruikt. U kan echter NSGs toepassen op elk subnet binnenkomende en uitgaande verkeer te beperken. Bijvoorbeeld, kunt u de volgende NSG-regels naar het externe subnet met FW toepassen.

**Binnenkomend**

- Alle TCP-verkeer vanaf het Internet toestaan op poort 80 op een VM in het subnet.
- Al het andere verkeer vanaf het Internet weigeren.

**Uitgaande**
- Al het verkeer naar het Internet ontzeggen.

## <a name="high-level-steps"></a>Hoog niveau stappen

Voor de implementatie van dit scenario volgt hoog niveau.

1.  Aanmelden bij uw abonnement Azure.
2.  Als u implementeren, een VNet die zich voordoet als het netwerk op gebouwen wilt, inrichten van de resources die deel van de **ONPREMRG uitmaken**.
3.  De bronnen die deel van de **AZURERG uitmaken**ter beschikking stellen.
4.  Inrichten van de tunnel van **onpremvnet** naar **azurevnet**.
5.  Nadat alle bronnen zijn ingericht, aanmelden bij **onpremvm2** en 10.0.3.101 om te testen van de connectiviteit tussen **onpremsn2** en **azsn3**de opdracht ping.
