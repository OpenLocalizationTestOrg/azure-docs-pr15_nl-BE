<properties
   pageTitle="Controlelijst voor hoge beschikbaarheid | Microsoft Azure"
   description="Een snelle checklist van instellingen en de acties die u nemen kunt om ervoor te zorgen u de beschikbaarheid van uw toepassingen met Azure verbeteren."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-checklist"></a>Controlelijst voor hoge beschikbaarheid
Een van de grote voordelen van het gebruik van Azure is de mogelijkheid om te vergroten de beschikbaarheid (en de schaalbaarheid) van uw toepassingen met behulp van de cloud. Onderstaande controlelijst is bedoeld om te zorgen dat u de meeste van deze opties wilt maken, om u te helpen met enkele van de basisprincipes van belangrijke infrastructuur om ervoor te zorgen dat uw toepassingen robuuste. 

>[AZURE.NOTE] De meeste van de volgende suggesties zijn dingen die kunnen worden geïmplementeerd op elk gewenst moment in uw toepassing en dus zijn uitermate geschikt voor "snelle correcties". De beste oplossing is vaak het ontwerp van een toepassing dat is gebouwd voor de cloud.  Voor een controlelijst op deze (meer georiënteerd ontwerp gebieden, Lees onze [beschikbaarheid controlelijst](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Gebruikt u beheer van verkeer voor uw resources?
Met verkeer Manager helpt routeren u internet-verkeer via Azure regio of locatie Azure en in ruimten. U kunt hiervoor een aantal redenen, met inbegrip van latentie en beschikbaarheid. Als u meer informatie over het gebruik van beheer van verkeer wilt te verhogen uw tolerantie en uw verkeer naar meerdere regio's verspreid lezen [VMs uitgevoerd in meerdere datacenters op Azure voor hoge beschikbaarheid](../guidance/guidance-compute-multiple-datacenters.md).

__Wat gebeurt er als u geen verkeer Manager?__ Als u niet de Manager van verkeer voor uw toepassing gebruikt, bent u beperkt tot één gebied voor uw resources. Beperkt de schaal, de latentie voor gebruikers die zich niet dicht bij uw gekozen regio verhoogt en verlaagt uw bescherming in geval van een hele regio onderbrekingen.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Hebt u vermeden met behulp van een enkele virtuele machine voor een rol?
Een goed ontwerp wordt voorkomen dat een potentieel risico. Dit is belangrijk bij alle service-ontwerp (in ruimten of in de cloud), maar is vooral handig in de cloud als u verhoogt de schaalbaarheid en tolerantie maar schalen (virtuele machines toe) in plaats van verticaal schalen (met behulp van een krachtige virtuele machine). Als u wilt weten meer informatie over het ontwerp van schaalbare toepassingen lezen [hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](resiliency-high-availability-azure-applications.md).

__Wat gebeurt er als er een enkele virtuele machine voor een rol?__ Een enkele machine is een potentieel risico en is niet beschikbaar voor de [Azure Virtual Machine Service Level Agreement](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). In het beste geval moet uw toepassing wordt uitgevoerd dan grote, maar dit is niet een robuust ontwerp, wordt niet gedekt door de virtuele Machine Azure SLA en aanspreekpunt of storing verhoogt de kans op uitvaltijd als er iets mislukt.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Gebruikt u een load balancer voor van uw toepassing internetgerichte VMs?
Netwerktaakverdeling, kunt u het binnenkomende verkeer naar uw toepassing verspreid over een willekeurig aantal computers. U kunt toevoegen of verwijderen machines van de verdeling van de belasting op elk gewenst moment, werkt goed met virtuele Machines (en ook met automatische schaling schaal met Virtual Machine) kunt u gemakkelijk verhogingen van verkeer of VM fouten afhandelen. Als u meer weten over Netwerktaakverdeling wilt, lees dan de [Azure Load Balancer-overzicht](../load-balancer/load-balancer-overview.md) en het [uitvoeren van meerdere VMs op Azure voor schaalbaarheid en beschikbaarheid](../guidance/guidance-compute-multi-vm.md).

__Wat gebeurt er als u niet een load balancer voor uw internetverbinding VMs?__ Zonder een load balancer u wordt mogelijk niet schaalt (meer virtuele machines toevoegen) en de enige optie is om uit te breiden (vergroten van uw web gerichte virtuele machine). U wordt ook een potentieel risico met die virtuele machine wordt geconfronteerd. Ook moet u DNS-code schrijven om te merken als u een computer met internetverbinding verbroken en opnieuw toewijzen van uw DNS-vermelding voor de nieuwe computer starten op de plaats.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Weet u met beschikbaarheid wordt ingesteld voor uw stateless toepassingen en de web-servers?
Zet uw computers op hetzelfde niveau in een set van de beschikbaarheid van toepassing kunt uw VMs u in aanmerking voor de SLA Azure VM. Deel van een beschikbaarheid ook zorgt ervoor dat uw machines in verschillende update-domeinen (dat wil zeggen andere host machines die is geïnstalleerd op verschillende tijdstippen) zijn gebracht en fault-domeinen (dat wil zeggen overschakelen van host-computers die een gemeenschappelijke energiebron en netwerk delen). Het VMs kunnen zich bevinden op dezelfde host machine zonder in een set van beschikbaarheid, en dus is er mogelijk een potentieel risico die niet zichtbaar is voor u. Wilt u meer informatie over het verhogen van de beschikbaarheid van uw beschikbaarheid sets met VMs, lees dan [de beschikbaarheid van virtuele machines beheren](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Wat gebeurt er als u niet een beschikbaarheid met webservers en stateless toepassingen instellen?__ Niet met een beschikbaarheid van instellen middelen die u niet kunt profiteren van de SLA Azure VM. Het betekent ook dat computers in die laag van de toepassing kunnen alle off line gaan als er een update op de hostcomputer (de computer waarop het VMs die u gebruikt) of een algemene hardwarefout.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Gebruikt u virtuele Machine schaal Sets (VMSS) voor uw stateless toepassingen of web-servers?
Een goed schaalbaar en robuust ontwerp maakt gebruik van VMSS om ervoor te zorgen dat u kunt vergroten/verkleinen het aantal computers in een laag van de toepassing (bijvoorbeeld uw weblaag). VMSS kunt u definiëren hoe de toepassingslaag wordt geschaald (servers toevoegen of verwijderen op basis van criteria die u kiest). Als u meer informatie over het gebruik van Azure Virtual Machine schaal Sets wilt te verhogen uw herstellingsvermogen voor verkeer spikes, leest u de [Virtuele Machine schaal Sets overzicht](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Wat gebeurt er als u ons een virtuele Machine schaal instellen van mijn stateless toepassing van de webserver niet?__ Zonder een VMSS beperkt u de mogelijkheid om te schalen zonder grenzen en het gebruik van bronnen optimaliseren. Een ontwerp zonder VMSS schaal bovengrens is die moet worden afgehandeld met aanvullende code (of handmatig). Dit gebrek aan een VMSS betekent ook dat uw toepassing niet gemakkelijk kunt toevoegen en verwijderen van machines (ongeacht de schaal) waarmee u grote pieken van verkeer verwerken (die als tijdens een promotie of als uw site/app/product virale gaat).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Gebruikt u premie opslag en opslag van afzonderlijke accounts voor elk van uw virtuele machines?
Is het beste premium opslag gebruiken voor uw productie virtuele machines. Bovendien moet u ervoor zorgen dat u een aparte opslag-account voor elke virtuele machine gebruiken (dit geldt voor kleinschalige implementaties. Voor grotere implementaties kunt u opnieuw gebruiken opslag rekeningen voor meerdere machines maar er is een afweging die moet worden gedaan om ervoor te zorgen u in evenwicht zijn tussen update domeinen en lagen van de toepassing). Als u wilt weten meer informatie over de opslag van Azure prestaties en schaalbaarheid lezen [Microsoft Azure opslag prestaties en schaalbaarheid controlelijst](../storage/storage-performance-checklist.md).

__Wat gebeurt er als u geen aparte opslag accounts voor elke virtuele machine gebruikt?__ Een opslag, net als veel andere bronnen is een potentieel risico. Er zijn veel bescherming en tolerantiefuncties van opslag in Azure, is een potentieel risico nooit een goed ontwerp. Bijvoorbeeld, als u toegangsrechten beschadigd raken op een opslaglimiet is bereikt, rekening, of een [IOP's beperken](../azure-subscription-service-limits.md#virtual-machine-disk-limits) is bereikt, alle virtuele machines met die account opslag die erdoor worden beïnvloed. Als er een verstoring van de service die van invloed op een stempel opslag met deze account bepaalde opslag kan u ook meerdere virtuele machines die invloed hebben.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Gebruikt u een load balancer of een wachtrij tussen elke laag van de toepassing?
Met behulp van netwerktaakverdeling of wachtrijen tussen elke laag van uw toepassing, kunt u gemakkelijk de schaal van elke laag van de toepassing gemakkelijk en onafhankelijk van elkaar. U moet kiezen tussen deze technologieën op basis van de latentie, complexiteit, en distributie (dat wil zeggen hoe ver u verdeelt uw app) nodig heeft. In het algemeen wachtrijen zijn over het algemeen hebben hogere latentie en complexer maar voordelen bij veerkrachtiger wordt en zodat u uw toepassing distribueert via grotere gebieden (zoals tussen de regio's). Als u graag meer informatie over het gebruik van interne netwerktaakverdeling of Lees [interne taakverdeling overzicht](../load-balancer/load-balancer-internal-overview.md) en [Azure wachtrijen en Service Bus wachtrijen - vergeleken en tegenstelling tot](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)wachtrijen.

__Wat gebeurt er als u niet een taakverdeling of een wachtrij tussen elke laag van de toepassing?__ Zonder een taakverdeling of wachtrij tussen elke laag van de toepassing is het moeilijk om te schalen van uw toepassing omhoog of omlaag en de werklast verdelen over meerdere computers. Hierdoor niet kan leiden tot boven of onder het inrichten van een risico van downtime of slechte gebruikerservaring, en uw resources als u onverwachte wijzigingen in het verkeer of het systeem fouten.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>SQL-Databases active geo-replicatie gebruikt? 
Actieve Geo-replicatie kunt u maximaal 4 leesbaar secundaire databases configureren in dezelfde of verschillende regio's. Secundaire databases zijn beschikbaar in het geval van een onderbreking van de service of het onvermogen om verbinding met de primaire database. Als u meer weten over SQL-Database actief geo-replicatie wilt, Lees [Overzicht: SQL Database actief Geo-replicatie](../sql-database/sql-database-geo-replication-overview.md).
 
 __Wat gebeurt er als u geen actieve geo-replicatie met SQL-databases?__ Zonder actieve geo-replicatie als de primaire database ooit off line (planmatig onderhoud, service-onderbrekingen, hardwarestoringen, enz.) de database van uw toepassing worden off line totdat u de primaire database weer on line in orde zijn brengen kunt. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Gebruikt u een cache (Azure bestand Vgx. Cache) voor uw databases?
Als uw toepassing een database met hoge belasting waar de meeste database-aanroepen worden gelezen, kunt u verhogen de snelheid van uw toepassing en de belasting van uw database verkleinen door het implementeren van een laag in de cache voor de database te dragen deze leesbewerkingen. U kunt verhogen de snelheid van uw toepassing en uw database laden (dus verhoging van de schaal die kan verwerken) door het plaatsen van een cache laag voor uw database verkleinen. Als u wilt weten over de cache Azure bestand Vgx. Lees de [richtlijnen voor opslaan in cache](../best-practices-caching.md).
 
 __Wat gebeurt er als u niet een cache voor de database?__ Als uw database machine krachtig genoeg is voor de belasting plaatsen u op deze vervolgens uw aanvraag normaal reageren zal, al dit betekent dat bij lagere belasting u betaalt voor een database machine die duurder zijn dan nodig is. Als uw database machine niet krachtig is ondervinden genoeg om de belasting te verwerken en vervolgens gaat u slechte gebruiker ervaring met uw toepassing (latentie, time-outs en eventueel service downtime).
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>U contact opgenomen met Microsoft Azure ondersteuning als u een hoge schaal gebeurtenis verwacht?
Azure-ondersteuning kunt u uw service verruimen omgaan met veel verkeer geplande gebeurtenissen (zoals nieuwe producten op de markt of speciale feestdagen). Ondersteuning van Azure ook mogelijk om te verbinden met experts die u helpen uw ontwerp controleren met uw accountteam en helpen u vindt de beste oplossing voor uw behoeften hoge schaal gebeurtenis. Wilt u meer informatie over hoe u contact kunt opnemen met Azure, lees de [Veelgestelde vragen Azure ondersteunen](https://azure.microsoft.com/support/faq/).

__Wat gebeurt er als u geen contact op met ondersteuning voor Azure voor een hoge schaalbaarheid gebeurtenis?__ Als u niet communiceren of plannen, een gebeurtenis met veel verkeer, kunnen bepaalde [Azure services beperkt](../azure-subscription-service-limits.md) raken en waardoor er een slechte gebruikerservaring (of nog erger, uitvaltijd) tijdens het evenement. Architecturale beoordelingen en communicatie voor piekspanningen kunt u deze risico's te verminderen.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Gebruikt u een Content Delivery Network (CDN Azure) voor uw web gerichte storage blobs en statische activa?
Met behulp van een CDN, kunt u de werklast van servers door de cache van de inhoud van de CDN POP/edge locaties die zich over de hele wereld te nemen. U kunt hiervoor latentie te verlagen, verhoogt de schaalbaarheid, minder belasting van de server, en als onderdeel van een strategie voor de bescherming tegen denial-of-service(DOS)-aanvallen. Als u meer informatie over het gebruik van Azure CDN uw tolerantie verhogen en verlagen van uw klant latentie wilt, [overzicht van de Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md)lezen.

__Wat gebeurt er als u niet een CDN?__ Als u niet een CDN gebruikt vervolgens al het klantverkeer van uw komt rechtstreeks naar uw resources. Dit betekent dat u ziet hogere belasting op de servers die hun schaalbaarheid afneemt. Bovendien ondervinden uw klanten hogere vertragingstijden zoals CDN locaties over de hele wereld die waarschijnlijk dichter aan uw klanten aanbieden.

##<a name="next-steps"></a>Volgende stappen:
Als u meer wilt lezen over het ontwerpen van uw toepassingen voor hoge beschikbaarheid, Lees [hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](resiliency-high-availability-azure-applications.md).
