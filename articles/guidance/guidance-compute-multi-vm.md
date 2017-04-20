<properties
   pageTitle="Met meerdere VMs | Verwijzen naar architectuur | Microsoft Azure"
   description="Het uitvoeren van meerdere exemplaren van de VM in Azure voor schaalbaarheid, tolerantie, beheerbaarheid en beveiliging."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Waarop meerdere VMs Azure voor schaalbaarheid en beschikbaarheid 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel worden een aantal beproefde procedures voor het uitvoeren van meerdere exemplaren van de virtuele machines (VM) voor een betere schaalbaarheid, beschikbaarheid, beheerbaarheid en beveiliging.   

In deze architectuur, wordt de werklast verdeeld over de VM-exemplaren. Er is één openbaar IP-adres en Internet-verkeer wordt verdeeld over het gebruik van een load balancer VMs. Deze architectuur kan worden gebruikt voor een enkele-tier-app, zoals een cluster dat stateless web app of opslag. Het is ook een bouwsteen voor N-tier toepassingen. 

Dit artikel is gebaseerd op [een enkele VM in Azure uitgevoerd][single vm]. De aanbevelingen in dit artikel gelden ook voor deze architectuur.

## <a name="architecture-diagram"></a>Architectuurdiagram

VMs in Azure vereist ondersteuning van netwerk-en opslagbronnen.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de "Compute - multi VM tabblad." 

![[0]][0]

De architectuur, heeft de volgende onderdelen: 

- **Beschikbaarheid is ingesteld.** Het [instellen van beschikbaarheid] [ availability set] bevat de VMs en is noodzakelijk voor de ondersteuning van de [beschikbaarheid van SLA voor Azure VMs][vm-sla].

- **VNet**. Elke VM in Azure is geïmplementeerd in een virtueel netwerk (VNet) wordt onderverdeeld in **subnetten**.

- **Azure taakverdeling.** De [belasting voor documentconversies] verdeelt binnenkomende Internet-verzoeken naar de VM-exemplaren in een set van beschikbaarheid. De taakverdeling omvat sommige verwante bronnen:

  - **Openbare IP-adres.** Een openbaar IP-adres is nodig voor de verdeling van de belasting voor het ontvangen van Internet-verkeer.

  - **Front-configuratie.** Hiermee koppelt het openbare IP-adres aan de taakverdeling.

  - **Back-end-adresgroep.** Bevat de netwerkinterfaces (NIC's) voor de VMs dat binnenkomend verkeer ontvangt.

- **Regels voor de verdeling van laden.** Gebruikt voor het verdelen van netwerkverkeer wordt verdeeld over alle VMs in de back-end-adresgroep. 

- **NAT regels.** Gebruikt voor het routeren van netwerkverkeer naar een bepaalde VM. Bijvoorbeeld, zodat het remote desktop protocol (RDP) naar de VMs een afzonderlijk netwerk adres netwerkadresomzetting (NAT) regel maken voor elke VM. 

- **Netwerkinterfaces (NIC's)**. Elke VM heeft een Netwerkkaart om verbinding met het netwerk.

- **Opslag.** Opslag rekeningen houdt de VM-afbeeldingen en andere bronnen bestand gerelateerd, zoals VM diagnostische gegevens vastgelegd met Azure.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op de onderstaande aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet ondersteunt. 

### <a name="availability-set-recommendations"></a>Beschikbaarheid instellen aanbevelingen

U moet ten minste twee VMs maken in de beschikbaarheid van ondersteuning voor de [beschikbaarheid van SLA voor Azure VMs]ingesteld[vm-sla]. Houd er rekening mee dat de Azure taakverdeling ook vereist dat VMs taakverdeling tot dezelfde set beschikbaarheid behoren.

### <a name="network-recommendations"></a>Aanbevelingen voor netwerken

Het VMs achter de taakverdeling moeten alle geplaatst in hetzelfde subnet. Het VMs rechtstreeks met het Internet niet zichtbaar, maar in plaats daarvan een particulier IP-adres geven elke VM. Clients, verbinding maken met het openbare IP-adres van de taakverdeling.

### <a name="load-balancer-recommendations"></a>Load balancer aanbevelingen

Voeg alle VMs in de beschikbaarheid van de back-end-adresgroep van de taakverdeling.

Load balancer regels definiëren voor directe netwerkverkeer naar het VMs. Bijvoorbeeld als u HTTP-verkeer, een regel maken die poort 80 van de configuratie van de front-poort 80 op de back-end-adresgroep toegewezen. Wanneer de verdeling van de belasting een aanvraag op poort 80 van het openbare IP-adres ontvangt, wordt het verzoek doorsturen naar poort 80 op een van de netwerkkaarten in de back-end-adresgroep.

NAT-regels voor het routeren van netwerkverkeer naar een bepaalde VM definiëren. Bijvoorbeeld om te schakelen RDP naar de VMs een aparte NAT regel maken voor elke VM. Elke regel een afzonderlijke poortnummer moet worden toegewezen aan poort 3389, de standaardpoort voor RDP. (Bijvoorbeeld gebruik van poort 50001 voor 'VM1', poort 50002 voor "VM2", enzovoort.) De NAT regels toewijzen aan de netwerkadapters op de VMs. 

### <a name="storage-account-recommendations"></a>Aanbevelingen voor opslag account

Aparte opslag van Azure-accounts maken voor elke VM voor virtuele harde schijven (VHD's) om te voorkomen dat de invoer/uitvoer-bewerkingen per tweede [grenzen (IOP's)] kunt u door[ vm-disk-limits] voor opslag. 

Maak een opslag account voor diagnostische logboeken. Deze opslag account kan worden gedeeld door alle VMs.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

Er zijn twee opties voor het horizontaal schalen VMs in Azure: 

- Gebruik een load balancer netwerkverkeer over een set van VMs verdelen. Schaalbare, creëren extra VMs en hen plaatsen achter de taakverdeling. 

- [Virtuele Machine schaal]worden[vmss]. Een set schaal bevat een opgegeven aantal identieke VMs achter een taakverdeling. VM-schaal wordt ondersteuning autoscaling op basis van prestatiegegevens. Als de belasting op het VMs toeneemt, worden extra VMs automatisch toegevoegd aan de taakverdeling. 

De volgende secties vergelijken deze twee opties.

### <a name="load-balancer-without-vm-scale-sets"></a>De belasting voor documentconversies zonder VM schaal sets

Een load balancer wordt binnenkomende netwerkaanvragen en verdeelt ze over de netwerkkaarten in de back-end-adresgroep. Horizontaal schalen, meer VM exemplaren toevoegen aan de set beschikbaarheid (of VMs in te perken deallocate). 

Stel bijvoorbeeld dat u bij het uitvoeren van een webserver. Voegt u een regel load balancer voor poort 80 en/of poort 443 (voor SSL). Wanneer een client een HTTP-aanvraag verzendt, de verdeling van de belasting een back-end-IP-adres met behulp van een [hash-algoritme] picks[ load balancer hashing] die het bron-IP-adres bevat. Op die manier worden clientaanvragen verdeeld over alle VMs. 

> [AZURE.TIP] Wanneer u een nieuwe VM toevoegt aan een beschikbaarheid instellen, zorg ervoor dat u een NIC voor VM en de NIC toevoegen aan de back-end-adresgroep voor de taakverdeling. Anders won't Internet-verkeer doorgestuurd naar de nieuwe VM.

Elk abonnement Azure heeft standaardlimieten in plaats, inclusief een maximum aantal VMs per regio. U kunt deze limiet verhogen door het indienen van een verzoek om ondersteuning. Zie voor meer informatie, [Azure abonnement service grenzen, quota's en beperkingen][subscription-limits].  

### <a name="vm-scale-sets"></a>VM schaal sets 

VM schaal sets helpt u bij het implementeren en beheren van een reeks identiek VMs. Met alle VMs geconfigureerd dezelfde, VM schaal sets ondersteuning waar automatisch schalen, zonder vooraf VMs, waardoor het gemakkelijker te maken van grootschalige services gericht op grote compute, big data en beperkte werklast wordt ingericht. 

Voor meer informatie over VM schaal sets overzicht [virtuele Machine schaal stelt][vmss].

Overwegingen bij het gebruik van VM schaal sets:

- Overweeg schaal instellen als u nodig hebt om snel te schalen van VMs, of moet automatisch schalen. 

- Op dit moment schaal wordt niet ondersteund door gegevensschijven. De opties voor het opslaan van gegevens zijn Azure bestandsopslag, het station van het besturingssysteem, het Temp-station of een extern archief, zoals opslag Azure. 

- Deel uitmaken van VM overal binnen een schaal automatisch ingesteld op dezelfde beschikbaarheid, met 5 fout met betrekking tot domeinen en domeinen met 5 update.

- Standaard gebruik schaal "overprovisioning", wat betekent dat de set schaal aanvankelijk bepalingen meer VMs dan u voor vragen en vervolgens verwijdert u de extra VMs. Dit verbetert de algehele slagingspercentage tijdens het inrichten van het VMs. 

- En vervolgens wordt aangeraden niet meer dan 20 VMs per opslag account met overprovisioning ingeschakeld of niet meer dan 40 VMs met overprovisioning uitgeschakeld.  

- Vindt u Resource Manager templates voor distributie schaal wordt ingesteld in de [Sjablonen van Azure Quickstart][vmss-quickstart].

- Er zijn twee basismethoden voor het configureren van VMs geïmplementeerd in een schaal: een aangepaste installatiekopie maken of extensies gebruiken voor het configureren van de VM nadat deze is ingericht.

    - Een set schaal is gebaseerd op een aangepaste afbeelding moet alle OS schijf VHD in één opslag account maken. 

    - U moet de afbeelding te houden met aangepaste images.

    - Serverextensies kunt duurt het langer voor een nieuw ingerichte VM op toeren moet komen.

Zie voor aanvullende overwegingen [Ontwerpen VM schaal ingesteld voor schaal][vmss-design].

> [AZURE.TIP]  Wanneer u een oplossing met automatisch schalen, getest met productie-niveau werkbelastingen ruim. 

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

De beschikbaarheid Set kunt u uw app beter bestand tegen zowel geplande als niet-gepland onderhoud gebeurtenissen.

- _Gepland onderhoud_ vindt plaats wanneer Microsoft het onderliggende platform werkt soms veroorzaakt door VMs opnieuw worden gestart. Azure is zeker het VMs in een set beschikbaarheid zijn niet allemaal opnieuw gestart op hetzelfde moment, ten minste één blijft uitgevoerd terwijl anderen opnieuw te starten.

- _Niet-gepland onderhoud_ gebeurt er als er een hardwarefout opgetreden. Azure zorgt ervoor dat VMs in een set beschikbaarheid over meer dan één server rack zijn ingericht. Hierdoor vermindert de impact van de problemen met de hardware, netwerk storingen, onderbrekingen voeding, enzovoort.

Voor meer informatie, Zie [beheren van de beschikbaarheid van virtuele machines][availability set]. De volgende video is ook een goed overzicht van beschikbaarheid sets: [Hoe kan ik een beschikbaarheid ingesteld op schaal VMs configureren][availability set ch9]. 

> [AZURE.WARNING]  Zorg ervoor dat de beschikbaarheid van instellen bij het inrichten van de VM configureren. Er is momenteel geen manier om een VM Resource Manager toevoegen aan een beschikbaarheid ingesteld nadat de VM is ingericht.

Verdeling van de belasting wordt [gezondheid sondes] voor het controleren van de beschikbaarheid van VM exemplaren. Als een sonde niet kan van een exemplaar een time-out is verstreken bereiken, reageert de taakverdelingsvoorziening verzenden van verkeer naar dat VM. Echter, blijven de taakverdelingsvoorziening probe en als de VM weer beschikbaar is, de taakverdeling hervat verzenden van verkeer naar dat VM.

Hier volgen enkele aanbevelingen op load balancer gezondheid sondes:

- Sondes kunnen HTTP- of TCP-test. Als een HTTP-server (IIS, nginx, Node.js app, enzovoort), werken VMs een HTTP-sonde maken. Een TCP-sonde anders maken.

- Geef het pad naar het eindpunt van de HTTP-voor een HTTP-sonde. De sonde wordt gecontroleerd op een HTTP-200 reactie van dit pad. Dit is het pad naar de hoofdmap (/') of een controle eindpunt dat sommige aangepaste logica om te controleren van de status van de toepassing wordt geïmplementeerd. Het eindpunt moet anonieme HTTP-aanvragen toestaan.

- De sonde wordt verzonden door een [bekende] [ health-probe-ip] IP-adres, 168.63.129.16. Zorg ervoor dat u geen verkeer naar of van deze IP-netwerk security group (NSG) regels of firewallbeleid van blokkeren.

- Met behulp van [Logboeken voor gezondheid sonde] [ health probe log] om de status van de sondes gezondheid weer te geven. Logboekregistratie inschakelen in de Azure portal voor de taakverdeling. Logboeken geschreven naar Azure Blob-opslag. De logboeken weergeven hoeveel VMs in de back-end netwerkverkeer als gevolg van defecte sonde antwoorden niet ontvangt.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

Met meerdere VMs wordt het automatiseren van processen, zodat ze een betrouwbare en herhaalbare zijn belangrijk. U kunt [Automatisering Azure] [ azure-automation] implementatie OS patches en andere taken te automatiseren. Azure automatisering is een Automatiseringsservice die wordt uitgevoerd op Azure en is gebaseerd op Windows PowerShell. Voorbeeld van automatische scripts zijn beschikbaar in de [Galerie met Runbook] op TechNet.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Virtuele netwerken zijn een grens verkeer isolatie in Azure. VMs in een die vnet kan niet rechtstreeks met VMs in een andere VNet communiceren. VMs in dezelfde VNet kunnen communiceren, tenzij u [beveiligingsgroepen netwerk] [ nsg] (NSGs) voor het beperken van verkeer. Zie voor meer informatie [Microsoft cloud services en beveiliging van het netwerk][network-security].

Voor binnenkomend verkeer van Internet definiëren de load balancer regels wat voor verkeer door de back-end kunt bereiken. Load balancer regels ondersteunen echter geen IP-whitelisting, dus als u een NSG wilt "witte" lijst bepaalde openbare IP-adressen toevoegen aan het subnet.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie voor een referentiearchitectuur die deze aanbevelingen implementeert, is beschikbaar op GitHub. Deze referentiearchitectuur bevat een virtueel netwerk (VNet), network security group (NSG), taakverdeling en twee virtuele machines (VMs).

De referentiearchitectuur kan worden geïmplementeerd met Windows of Linux VMs door de onderstaande aanwijzingen: 

1. Klik met de rechtermuisknop op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal Azure, moet u waarden opgeven voor enkele van de instellingen: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus selecteer **Bestaande gebruiken** en voer `ra-multi-vm-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Het **Type besturingssysteem** selecteren uit de vervolgkeuzelijst vak, **windows** of **linux**. 
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

4. De parameterbestanden bevatten een hard gecodeerde gebruikersnaam en wachtwoord en het is raadzaam dat u onmiddellijk beide wijzigen. Klik op de VM met de naam `ra-multi-vm1` in het portal voor Azure. Klik vervolgens op **wachtwoord opnieuw instellen** in de bladeserver **ondersteuning + het oplossen van problemen** . Selecteer **wachtwoord opnieuw instellen** in het dropdown **-modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om de nieuwe gebruikersnaam en wachtwoord opslaan. Herhaal dit voor de VM met de naam `ra-multi-vm2`.

Zie voor informatie over manieren waarop u deze referentiearchitectuur implementeren, het Leesmij-bestand in de [richtlijnen voor-één-vm] [ github-folder] map GitHub. 

## <a name="next-steps"></a>Volgende stappen

Verschillende VMs achter een load balancer plaatsen is een bouwsteen voor een architectuur met meerdere niveaus maken. Zie voor meer informatie [Windows VMs uitvoeren voor een N-lagige architectuur op Azure] [ n-tier-windows] en [Linux VMs uitvoeren voor een N-lagige architectuur op Azure][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[gezondheid sondes]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[taakverdeling]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Galerie Runbook]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Architectuur van een oplossing voor meervoudige VM in Azure, bestaande uit een set met twee VMs en een load balancer beschikbaarheid"
