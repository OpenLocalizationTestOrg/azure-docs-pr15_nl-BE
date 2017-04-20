<properties
   pageTitle="Uitvoering van een beveiligde hybride netwerkarchitectuur in Azure | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur in Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Implementatie van een DMZ tussen Azure en uw datacenter in gebouwen

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dit artikel bevat aanbevolen procedures voor de uitvoering van een beveiligde hybride netwerk die een netwerk op ruimten naar Azure uitbreidt. Deze referentiearchitectuur implementeert een DMZ tussen een netwerk op gebouwen en een Azure virtueel netwerk met behulp van door de gebruiker gedefinieerd routes (UDRs). De DMZ bevat uiterst beschikbaar netwerk virtuele toestellen (NVAs) die beveiligingsfuncties zoals firewalls en packet inspectie implementeren. Al het uitgaande verkeer van de VNet is kracht tunnelverbinding met het Internet via het netwerk op ruimten kunnen worden gecontroleerd. 

Deze architectuur is vereist voor een verbinding met uw datacenter op ruimten is geïmplementeerd met behulp van een [VPN-gateway][ra-vpn], of een [ExpressRoute] [ ra-expressroute] verbinding.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze referentiearchitectuur gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

Typische use-cases voor deze architectuur zijn:

- Hybride toepassingen waar werklasten uitvoeren deels op-lokalen en deels in Azure.

- Azure infrastructuur waarmee binnenkomende verkeer van de lokale en het internet worden doorgestuurd.

- Toepassingen die zijn vereist voor het uitgaande verkeer te controleren. Dit is vaak een wettelijke eis van veel commerciële systemen en kan helpen om te voorkomen dat de openbaarmaking van persoonlijke informatie.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de belangrijke onderdelen van deze architectuur:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de pagina 'DMZ – privé'.

[! [0]][0]

- **Netwerk van gebouwen.** Dit is een netwerk van computers en apparaten die zijn verbonden via een particuliere LAN geïmplementeerd in een organisatie.

- **Azure virtueel netwerk (VNet).** De VNet fungeert als host voor de toepassing en andere bronnen in de cloud wordt uitgevoerd.

- **Gateway.** De gateway biedt connectiviteit tussen de routers in het netwerk op gebouwen en de VNet.

- **Virtueel netwerkapparaat (NVA).** NVA is een algemene term ter aanduiding van een VM taken uitvoeren zoals het toegang verlenen of weigeren als een firewall, optimaliseren WAN-bewerkingen (inclusief Netwerkcompressie), aangepaste routering of andere netwerkfunctionaliteit. 

- **Web tier en laag business data tier subnetten.** Dit zijn de subnetten waarop het VMs en services die een voorbeeld 3-tier-toepassing die wordt uitgevoerd in de cloud te implementeren. Zie [Windows VMs uitvoeren voor een N-lagige architectuur op Azure] [ ra-n-tier] voor meer informatie.

- **Door gebruiker gedefinieerde routes (UDR).** [Door de gebruiker gedefinieerde routes] [ udr-overview] de doorstroming van IP-verkeer in Azure VNets definiëren. 

> [AZURE.NOTE] Afhankelijk van de vereisten van uw VPN-verbinding, kunt u routes Border Gateway Protocol (BGP) als alternatief voor de back-UDRs gebruikt voor het implementeren van de forwarding regels die verkeer via het netwerk op gebouwen.

- **Beheer van subnet.** Dit subnet bevat VMs die beheer- en controlefuncties voor de onderdelen die worden uitgevoerd in de VNet te implementeren. 

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="rbac-recommendations"></a>RBAC aanbevelingen

Verschillende RBAC-rollen voor het beheer van de resources in uw toepassing maken. Houd rekening met het maken van een [aangepaste rol] van DevOps[ rbac-custom-roles] met machtigingen voor het beheren van de infrastructuur voor de toepassing. Rekening houden met het maken van een gecentraliseerde IT-beheerder [aangepaste rol] [ rbac-custom-roles] voor het beheer van netwerkbronnen en een afzonderlijke beveiliging van IT-beheerder [aangepaste rol] [ rbac-custom-roles] voor het beheren van bronnen zoals het NVAs beveiligd netwerk. 

De rol van DevOps moet machtigingen voor het implementeren van de onderdelen, alsmede de monitor en VMs opnieuw opnemen. De centrale rol van de IT-beheerder moet de machtigingen voor het controleren van netwerkbronnen bevatten. Geen van deze rollen moeten toegang hebben tot de NVA bronnen zoals dit beperkt tot de rol van IT-beheerder van de beveiliging worden moet.

### <a name="resource-group-recommendations"></a>Aanbevelingen voor resource-groep

Azure bronnen zoals VMs, VNets en Netwerktaakverdeling kunnen gemakkelijk worden beheerd door het groeperen in resourcegroepen. U kunt vervolgens de rollen boven aan elke resource om toegang te beperken.

Wij raden aan het maken van de volgende opties:

- Een groep met de subnetten (met uitzondering van de VMs), NSGs en de bronnen van de gateway voor de verbinding met het netwerk op gebouwen. De centrale rol van de IT-beheerder toewijzen aan deze resourcegroep.

- Een resourcegroep met het VMs voor de NVAs (met inbegrip van de verdeling van de belasting), de sprong en andere VMs management en de UDR voor het subnet gateway, waardoor al het verkeer via de NVAs. De beveiliging van IT-beheerder toewijzen aan deze resourcegroep.

- Aparte bronnengroepen voor elke toepassingslaag van de die de taakverdeling en VMs bevatten. Rekening mee dat deze resourcegroep moet niet de subnetten die voor elke laag. De rol van DevOps toewijzen aan deze resourcegroep.

### <a name="virtual-network-gateway-recommendations"></a>Virtueel netwerk gateway aanbevelingen

Op gebouwen-verkeer wordt doorgegeven aan de VNet via een virtueel netwerkgateway. Wij raden aan een [Azure VPN-gateway] [ guidance-vpn-gateway] of een [gateway Azure ExpressRoute][guidance-expressroute]. 

### <a name="nva-recommendations"></a>NVA-aanbevelingen

NVAs biedt verschillende services voor het beheren en controleren van netwerkverkeer. De markt Azure biedt verschillende leveranciers NVAs, met inbegrip van:

- [Barracuda Web Application Firewall] [ barracuda-waf] en [Barracuda NextGen Firewall][barracuda-nf]

- [Samenhangende netwerken VNS3 Firewall/Router/VPN][vns3]

- [Fortinet FortiGate-VM][fortinet]

- [SecureSphere Web Application Firewall][securesphere]

- [DenyAll Web Application Firewall][denyall]

- [Check Point vSEC][checkpoint]

Als geen van deze derde partij NVAs voldoen, kunt u een aangepaste NVA met VMs. Zie voor een voorbeeld van het maken van aangepaste NVAs, de DMZ in deze referentiearchitectuur die de volgende functies implementeert:

- Verkeer wordt omgeleid via [doorsturen via IP] [ ip-forwarding] op de NVA-netwerkkaarten.

- Verkeer mag passeren de NVA als het juiste te doen. Elke NVA VM in de referentiearchitectuur is een eenvoudige Linux router voor binnenkomend verkeer die op het netwerk interface *eth0*en uitgaand verkeer overeenkomende regels gedefinieerd door aangepaste scripts die worden verzonden via een netwerk interface *eth1*aankomen. 

- Verkeer doorgestuurd naar het beheer van subnet niet langs de NVAs en de NVAs kunnen alleen uit het beheer van subnet worden geconfigureerd. Als het beheer van subnet-verkeer worden gerouteerd via de NVAs moet, is er geen route naar het subnet management op te lossen de NVAs als ze zou moeten mislukken.  

- Het VMs voor de NVA zijn opgenomen in een beschikbaarheid van achter een taakverdeling instellen. De UDR in het subnet gateway afdrukopdrachten NVA voor de taakverdeling.

Een andere aanbeveling rekening te houden met maakt meerdere NVAs in reeks verbinding met elke NVA is een gespecialiseerde SSP taak uit te voeren. Hierdoor kan elke beveiligingsfunctie kan worden beheerd op basis van per NVA. Bijvoorbeeld een NVA implementeren van een firewall kan worden in serie zijn geplaatst met een NVA identiteit services uitgevoerd. De verhouding voor gemakkelijk beheer is de toevoeging van extra netwerk hops die kunnen verhogen latentieperiode, dus zorg ervoor dat dit niet van invloed op de prestaties van uw toepassing.

### <a name="nsg-recommendations"></a>NSG-aanbevelingen

De VPN-gateway stelt een openbaar IP-adres voor de verbinding met het netwerk op gebouwen. Beste groep (NSG) met een netwerk maken voor de inkomende NVA subnet uitvoeringsbepalingen voor het blokkeren van al het verkeer niet afkomstig zijn van het netwerk op gebouwen.

Ook aanbevolen NSGs voor elk subnet om een tweede niveau van bescherming tegen inkomend verkeer door een onjuist geconfigureerde of uitgeschakelde NVA te implementeren. Bijvoorbeeld, implementeert het web tier subnet in de referentiearchitectuur een NSG met een regel voor het negeren van alle aanvragen die worden ontvangen van het netwerk op gebouwen (192.168.0.0/16) of de VNet en een andere regel alle verzoeken niet op poort 80 negeert. 

### <a name="internet-access-recommendations"></a>Aanbevelingen voor Internet-toegang

[Force-tunnel] [ azure-forced-tunneling] alle uitgaande internet-verkeer via het netwerk op ruimten is met behulp van de site naar site VPN-tunnel en de route met het internet via het netwerk adres tranlation (NAT). Dit voorkomt het per ongeluk weglekken van vertrouwelijke informatie opgeslagen in de gegevenslaag en inspectie en controle van alle uitgaande verkeer toestaan.

> [AZURE.NOTE] Geen Internet-verkeer van het web, zakelijke- en lagen volledig blokkeren. Als deze lagen PaaS Azure services afhankelijk van openbare IP-adressen voor VM diagnostics logging, het downloaden van de VM-extensies en andere functionaliteit. Azure diagnostiek vereist ook dat onderdelen kunnen lezen en naar een Azure opslag afhankelijk van de internet-account schrijven.

Verder het beste controleren of uitgaand internetverkeer is kracht tunnel correct is. Als u een VPN-verbinding met de [service voor Routering en RAS toegang] [ routing-and-remote-access-service] op een server op locatie met een hulpprogramma zoals [WireShark] [ wireshark] of [Microsoft Message Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Beheer van subnet-aanbevelingen

Het beheer van subnet bevat het sprong die management en monitoring functionaliteit uitgevoerd. De volgende aanbevelingen voor het vak snel implementeren:
- Maak een openbaar IP-adres voor de sprong geen. 
- Een route om het vak gaan via de gateway voor binnenkomende en implementeren van een NSG in het beheer van subnet alleen reageren op aanvragen van de toegestane route maken.
- Uitvoering van alle beveiligde taken beperken tot het vak koppeling.

### <a name="nva-recommendations"></a>NVA-aanbevelingen

Opnemen van een laag 7 NVA beëindigen van verbindingen voor toepassing op het niveau van de NVA en onderhouden van affiniteit met de back-end-lagen. Dit garandeert een symmetrische verbinding waarin antwoord verkeer vanaf de back-end lagen als resultaat door de NVA gegeven.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

De referentiearchitectuur implementeert een load balancer op ruimten netwerkverkeer naar een groep van NVA apparaten sturen. Zoals eerder besproken, de NVA apparaten VMs uitvoeren van regels voor routing netwerkverkeer zijn en worden ingezet in een [beschikbaar][availability-set]. Dit ontwerp kunt u de doorvoer van de NVAs controleren na verloop van tijd en NVA apparaten toevoegen in antwoord op de stijgingen van de belasting.

De standaard SKU VPN-gateway ondersteunt duurzame doorvoer van maximaal 100 Mbps. De hoge prestaties SKU biedt maximaal 200 Mbps. Overweeg een upgrade naar een gateway ExpressRoute voor hogere bandbreedte. ExpressRoute biedt maximaal 10 Gbps bandbreedte met lagere latentie dan een VPN-verbinding.

> [AZURE.NOTE] De [uitvoering van een hybride netwerkarchitectuur met Azure en VPN-On-premises] artikelen[ guidance-vpn-gateway] en de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute] [ guidance-expressroute] beschreven problemen rond de schaalbaarheid van Azure gateways.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

De referentiearchitectuur implementeert een load balancer aanvragen op ruimten naar een groep met apparaten in Azure NVA distribueren. De NVA-apparaten zijn VMs routeringsregels voor netwerk-verkeer wordt uitgevoerd en worden geïmplementeerd in een [beschikbaar][availability-set]. Verdeling van de belasting regelmatig een sonde met gezondheid geïmplementeerd op elke NVA een query en een niet-reagerende NVAs wordt verwijderd uit de groep. 

Als u voor de connectiviteit tussen het netwerk VNet en op de locatie [configureren van een VPN-gateway voor failover] Azure ExpressRoute[ guidance-vpn-failover] als de ExpressRoute verbinding niet beschikbaar is.

Zie de artikelen [een hybride netwerkarchitectuur met Azure en On-premises VPN-implementatie] voor specifieke informatie over het beheren van de beschikbaarheid van ExpressRoute- en VPN-verbindingen,[ guidance-vpn-gateway] en de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

Alle toepassingen en monitoring van de resource moeten worden uitgevoerd door het vak sprong in het beheer van subnet. Afhankelijk van de toepassingsvereisten van uw, moet u extra controle resources in het beheer van subnet toevoegen, maar opnieuw een van de volgende extra hulpbronnen moeten worden benaderd via het vak koppeling.

Als gateway verbinding hebben met het netwerk op ruimten naar Azure niet actief is, nog steeds bereikbaar het vak sprong door het implementeren van een PIP, toe te voegen aan het vak sprong en externe communicatie in van het internet.

Subnet van elke laag in de referentiearchitectuur is beveiligd door NSG regels en kan het nodig zijn voor het maken van een regel voor het openen van poort 3389 voor RDP-toegang in Windows VMs of poort 22 voor SSH toegang op Linux VMs zijn. Andere beheer- en controlehulpprogramma's mogelijk regels aanvullende poorten openen.

Als u de connectiviteit tussen uw datacenter op gebouwen en Azure in ExpressRoute, gebruiken de [Azure connectiviteit Toolkit (AzureCT)] [ azurect] te controleren en problemen met de verbinding.

> [AZURE.NOTE] Aanvullende informatie die specifiek is gericht op het controleren en beheren van verbindingen van VPN- en ExpressRoute in de [uitvoering van een hybride netwerkarchitectuur met Azure en VPN-On-premises] -artikelen vindt u[ guidance-vpn-gateway] en de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Deze referentiearchitectuur implementeert meerdere niveaus van beveiliging: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Alle verzoeken op ruimten door de NVA-routering

De UDR in het subnet gateway blokkeert alle aanvragen van gebruikers die van op het bedrijf ontvangen. De gangen UDR aanvragen naar de NVAs toegestaan in het particuliere DMZ subnet en deze verzoeken worden doorgegeven aan de toepassing als deze zijn toegestaan door de regels van de NVA. Andere routes kunnen worden toegevoegd aan de UDR, maar zorgen dat zij per ongeluk de NVAs of de administratieve verkeer blokkeren die bestemd zijn voor het beheer van subnet niet overslaan.

Verdeling van de belasting voor de NVAs fungeert ook als een beveiligingsapparaat door verkeer op poorten die niet zijn geopend in de regels voor de taakverdeling. De netwerktaakverdeling in de referentiearchitectuur luistert alleen voor HTTP-verzoeken op poort 80 en HTTPS-verzoeken op poort 443. Alle aanvullende regels toegevoegd aan de netwerktaakverdeling moeten worden gedocumenteerd en het verkeer moet worden gecontroleerd om ervoor te zorgen dat geen beveiligingsrisico's.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Gebruik NSGs voor pass/blokkeren verkeer tussen lagen

Elk van de web-, business- en lagen onderling verkeer beperken met behulp van NSGs. Dat wil zeggen, een NSG voor het blokkeren van al het verkeer dat niet van oorsprong uit de weblaag zijn maakt gebruik van de business-laag en de gegevenslaag een NSG gebruikt voor het blokkeren van al het verkeer dat niet van oorsprong uit de business-laag zijn. Als er een behoefte om uit te breiden de NSG-regels voor bredere toegang tot deze lagen, wegen deze vereisten ten opzichte van de beveiligingsrisico's. Elke nieuwe inkomende pad vertegenwoordigt een verkoopkans per ongeluk of purposeful gegevens lekken of toepassingen zijn beschadigd.

### <a name="devops-access"></a>DevOps toegang

De bewerkingen die DevOps kunt uitvoeren op elke laag met [RBAC] beperken[ rbac] om machtigingen te beheren. Bij het verlenen van machtigingen, het [principe van minimale bevoegdheden]gebruiken[security-principle-of-least-privilege]. Meld alle administratieve bewerkingen en voeren regelmatig controles om dat eventuele wijzigingen in de configuratie zijn gepland.

> [AZURE.NOTE] Voor meer uitgebreide informatie, voorbeelden en scenario's over het beheren van beveiliging van netwerken met Azure, raadpleegt u [Microsoft cloud services en beveiliging van het netwerk][cloud-services-network-security]. Zie voor gedetailleerde informatie over het beveiligen van bronnen in de cloud [aan de slag met Azure Microsoft security][getting-started-with-azure-security]. Voor meer informatie over de beveiligingsaspecten adressering via een verbinding Azure gateway Zie [een hybride netwerkarchitectuur met Azure en On-premises VPN-implementatie van] [ guidance-vpn-gateway] en de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie voor een referentiearchitectuur die deze aanbevelingen implementeert, is beschikbaar op Github. Deze referentiearchitectuur bevat een virtueel netwerk (VNet), network security group (NSG), taakverdeling en twee virtuele machines (VMs).

De referentiearchitectuur kan worden geïmplementeerd met Windows of Linux VMs door de onderstaande aanwijzingen: 

1. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal Azure, moet u waarden opgeven voor enkele van de instellingen: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-private-dmz-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

4. De parameterbestanden zijn hard gecodeerde gebruikersnaam en wachtwoord voor alle VMs en het is raadzaam dat u onmiddellijk beide wijzigen. Voor elke VM in de installatie, selecteert u deze in de Azure portal en klik vervolgens op **wachtwoord opnieuw instellen** in de bladeserver **ondersteuning + het oplossen van problemen** . Selecteer **wachtwoord opnieuw instellen** in het dropdown **-modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het implementeren van een [DMZ tussen Azure en het Internet](./guidance-iaas-ra-secure-vnet-dmz.md).
- Informatie over het implementeren van een [maximaal beschikbare hybride netwerkarchitectuur](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Architectuur van hybride netwerk beveiligen"
