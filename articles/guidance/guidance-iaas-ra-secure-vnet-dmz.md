<properties
   pageTitle="Referentie architectuur Azure - IaaS: Implementatie van een DMZ tussen Azure en Internet | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met toegang tot het Internet in Azure."
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

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Implementatie van een DMZ tussen Azure en het Internet

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Aanbevolen procedures voor de uitvoering van een beveiligde hybride netwerk waarmee uw netwerk op de ruimten die verkeer vanaf het internet netwerk naar Azure accepteert wordt uitgebreid beschreven. Deze referentiearchitectuur breidt de architectuur die is beschreven in het artikel [een DMZ Azure en uw datacenter op ruimten tussen de uitvoering van][implementing-a-secure-hybrid-network-architecture]. Het verdient u dat document te lezen en begrijpen die verwijzing architectuur voor het lezen van dit document.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze referentiearchitectuur gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties. 

Typische use-cases voor deze architectuur zijn:

- Hybride toepassingen waar werklasten uitvoeren deels op-lokalen en deels in Azure.

- Azure infrastructuur waarmee binnenkomende verkeer van de lokale en het internet worden doorgestuurd.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de belangrijke onderdelen van deze architectuur:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de pagina 'DMZ – Public'.

[! [0]][0]

- **Openbare IP-adres (PIP).** Dit is het IP-adres van de openbare eindpunt. Externe gebruikers zijn verbonden met het Internet kunnen toegang krijgen tot het systeem via dit adres.

- **Virtueel netwerkapparaat (NVA).**  NVA is een algemene term ter aanduiding van een VM taken uitvoeren zoals het toegang verlenen of weigeren als een firewall, optimaliseren WAN-bewerkingen (inclusief Netwerkcompressie), aangepaste routering of andere netwerkfunctionaliteit.

- **Azure taakverdeling.** Alle binnenkomende aanvragen via het internet via deze taakverdeling en worden gedistribueerd naar NVAs in de openbare DMZ inkomende subnet.

- **Openbare DMZ inkomende subnet.** Dit subnet ontvangt de Azure taakverdeling. Binnenkomende verzoeken worden doorgegeven aan een van de NVAs in de DMZ.

- **Openbare DMZ uitgaande subnet.** Aanvragen die zijn goedgekeurd door de NVA doorgegeven aan de interne taakverdeling voor de weblaag dit subnet.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="nva-recommendations"></a>NVA-aanbevelingen

Één set NVAs voor verkeer dat afkomstig is van het internet en een andere voor verkeer op het bedrijf van oorsprong worden geïmplementeerd. Het is een beveiligingsrisico voor slechts één reeks NVAs voor beide gebruiken omdat dit ontwerp geen beveiliging omtrek tussen de twee sets van netwerkverkeer biedt. Het is een voordeel voor dit ontwerp gebruiken, omdat het daardoor de complexiteit van de beveiligingsregels controleren en maakt het duidelijker welke regels overeenkomen met een binnenkomende netwerkaanvraag. Één set NVAs implementeert bijvoorbeeld regels voor internet-verkeer alleen bij een andere reeks regels die NVAs implementeren voor ruimten alleen voor verkeer.

Opnemen van een laag 7 NVA beëindigen van verbindingen voor toepassing op het niveau van de NVA en onderhouden van affiniteit met de back-end-lagen. Dit garandeert een symmetrische verbinding waarin antwoord verkeer vanaf de back-end lagen als resultaat door de NVA gegeven.  

### <a name="public-load-balancer-recommendations"></a>Openbare load balancer aanbevelingen ###

Implementeren zodat schaalbaarheid en beschikbaarheid van de openbare DMZ inkomende NVAs in een [beschikbaar] [ availability-set] en een [internet facing taakverdeling] met[ load-balancer] voor het distribueren van internet-aanvragen via de NVAs in de beschikbaarheid.  

De verdeling van de belasting voor het accepteren van aanvragen alleen op de poorten die nodig zijn voor internet-verkeer configureren. Stel binnenkomende HTTP-verzoeken voor poort 80 en HTTPS-verzoeken naar poort 443 inkomende beperken.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

Ontwerp uw infrastructuur met taakverdeling voor het binnenkomende openbare DMZ subnet vanaf het begin van een internetverbinding. Zelfs als uw architectuur initally een enkele NVA vereist, wordt het makkelijker moet worden geschaald naar meerdere NVAs in de toekomst als de internetverbinding taakverdeling is al geïmplementeerd zijn.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Taakverdeling van de internetverbinding is vereist voor elke NVA in de openbare DMZ inkomende subnet voor het implementeren van een [sonde gezondheid][lb-probe]. Een sonde gezondheid die niet reageert op dit eindpunt wordt beschouwd als niet beschikbaar en de verdeling van de belasting wordt aanvragen doorsturen naar andere NVAs in bepaalde beschikbaarheid. Houd er rekening mee dat als alle NVAs niet reageren, uw toepassing niet dus het is belangrijk dat toezicht geconfigureerd voor DevOps waarschuwing wanneer het aantal exemplaren van gezonde NVA kleiner is dan een drempelwaarde.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

De functionaliteit voor beheer en bewaking voor de binnenkomende openbare DMZ NVA van reageren op verzoeken van het vak sprong in het beheer van subnet alleen beperken. Zoals besproken in de [uitvoering van een DMZ tussen Azure en uw datacenter op gebouwen] [ implementing-a-secure-hybrid-network-architecture] document, definieert u een enkel netwerkroute via het netwerk op locatie via de gateway in het vak koppeling in het beheer van subnet toegang te beperken.

Als gateway verbinding hebben met het netwerk op ruimten naar Azure niet actief is, nog steeds bereikbaar het vak sprong door het implementeren van een PIP, toe te voegen aan het vak sprong en externe communicatie in van het internet.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Deze referentiearchitectuur implementeert meerdere niveaus van beveiliging:

- Internet facing taakverdeling afdrukopdrachten naar de NVAs in het binnenkomende openbare DMZ subnet uitsluitend en alleen op de poorten die nodig zijn voor de toepassing.

- De NSG-regels voor het binnenkomende en uitgaande openbare DMZ subnet voorkomen dat de NVAs wordt aangetast door het blokkeren van aanvragen die buiten de NSG-regels vallen.

- De NAT-configuratie voor routering voor de NVAs zorgt ervoor dat binnenkomende verzoeken op poort 80 en poort 443 met de taakverdelingsvoorziening web laag, maar aanvragen voor alle andere poorten worden genegeerd.

Houd er rekening mee dat u zich alle inkomende aanvragen op alle poorten aanmelden moet. Regelmatige controle van de logboekbestanden, met aandacht voor de aanvragen die buiten de verwachte parameters vallen zoals deze indringingspogingen kunnen aangeven.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Gebruik NSGs voor pass/blokkeren verkeer tussen lagen

Elk van de web-, business- en lagen onderling verkeer beperken met behulp van NSGs. Dat wil zeggen, een NSG voor het blokkeren van al het verkeer dat niet van oorsprong uit de weblaag zijn maakt gebruik van de business-laag en de gegevenslaag een NSG gebruikt voor het blokkeren van al het verkeer dat niet van oorsprong uit de business-laag zijn. Als er een behoefte om uit te breiden de NSG-regels voor bredere toegang tot deze lagen, wegen deze vereisten ten opzichte van de beveiligingsrisico's. Elke nieuwe inkomende pad vertegenwoordigt een verkoopkans per ongeluk of purposeful gegevens lekken of toepassingen zijn beschadigd.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie voor een referentiearchitectuur die deze aanbevelingen implementeert, is beschikbaar op Github. Deze referentiearchitectuur bevat een virtueel netwerk (VNet), network security group (NSG), taakverdeling en twee virtuele machines (VMs).

De referentiearchitectuur kan worden geïmplementeerd met Windows of Linux VMs door de onderstaande aanwijzingen: 

1. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Nadat de koppeling is geopend in de portal Azure, moet u waarden opgeven voor enkele van de instellingen: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-public-dmz-network-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Het **Type besturingssysteem** selecteren uit de vervolgkeuzelijst vak, **windows** of **linux**.
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

4. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Nadat de koppeling is geopend in de portal Azure, moet u waarden opgeven voor enkele van de instellingen: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-public-dmz-wl-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

6. Wacht totdat de installatie te voltooien.

7. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Nadat de koppeling is geopend in de portal Azure, moet u waarden opgeven voor enkele van de instellingen: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus selecteer **Bestaande gebruiken** en voer `ra-public-dmz-network-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

9. Wacht totdat de installatie te voltooien.

10. De parameterbestanden zijn hard gecodeerde gebruikersnaam en wachtwoord voor alle VMs en het is raadzaam dat u onmiddellijk beide wijzigen. Voor elke VM in de installatie, selecteert u deze in de Azure portal en klik vervolgens op **wachtwoord opnieuw instellen** in de bladeserver **ondersteuning + het oplossen van problemen** . Selecteer **wachtwoord opnieuw instellen** in het dropdown **-modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Architectuur van hybride netwerk beveiligen"
