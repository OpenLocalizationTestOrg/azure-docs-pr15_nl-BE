<properties
   pageTitle="Technische begeleiding: herstel van lokale fouten in Azure | Microsoft Azure"
   description="Artikel over wat en ontwerpen van robuuste, hoge beschikbaarheid, fouttolerante toepassingen, evenals planning voor herstel na noodgevallen gericht op lokale fouten in Azure."
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

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Technische richtsnoeren Azure tolerantie: herstel van lokale fouten in Azure

Er zijn twee primaire bedreigingen voor de beschikbaarheid van toepassingen:

* Het mislukken van apparaten, zoals schijven en servers
* De uitputting van essentiële bronnen, zoals compute piek belasting voorwaarden

Azure biedt een combinatie van Resourcemanagement, elasticiteit, taakverdeling en partitioneren als u hoge beschikbaarheid onder deze omstandigheden. Sommige van deze functies worden automatisch voor alle Azure services uitgevoerd. Ontwikkelaar van de toepassing moet echter in sommige gevallen doen om te profiteren van deze extra werk.

##<a name="cloud-services"></a>Cloud Services

Azure Cloud Services bestaat uit modules van een of meer webonderdelen of de werknemer. Een of meer exemplaren van een rol kunnen tegelijkertijd worden uitgevoerd. De configuratie bepaalt het aantal exemplaren. Rol-instanties worden gecontroleerd en beheerd via een onderdeel dat de controller fabric genoemd. De controller fabric detecteert en automatisch reageert op software-en hardwarefouten.

Elk exemplaar van de rol wordt uitgevoerd in een eigen virtuele machine (VM) en communiceert met de controller fabric via een gast-agent. De Gast-agent verzamelt resource en knooppunt metrics, met inbegrip van de VM-gebruik, status, Logboeken, Resourcegebruik, uitzonderingen en storingen. De controller fabric de agent Gast configureerbare tussenpozen query's en de VM wordt gestart als de Gast-agent niet reageert. Bij een defect van de hardware, de controller gekoppelde fabric alle instanties van de betrokken functie verplaatst naar een nieuw hardware-knooppunt en het netwerk te routeren van netwerkverkeer er opnieuw geconfigureerd.

Als u wilt profiteren van deze functies, moeten ontwikkelaars zorgen dat alle functies van de service niet opslaan, staat op de exemplaren van de rol. In plaats daarvan moet alle permanente gegevens worden benaderd vanuit duurzame opslag, zoals Azure opslag of Azure SQL-Database. U kunt geen rollen om aanvragen te verwerken. Het betekent ook dat exemplaren van de rol kunnen omlaag gaan op elk gewenst moment zonder inconsistenties in de tijdelijke of permanente status van de service.

De eis om op te slaan staat extern aan de rollen heeft verschillende gevolgen. Dit betekent bijvoorbeeld dat alle gerelateerde wijzigingen in een tabel Azure opslag moeten worden gewijzigd in een enkele entiteit groep transactie indien mogelijk. Natuurlijk is het niet altijd mogelijk om alle wijzigingen in één transactie. U moet erop letten om ervoor te zorgen dat rol exemplaar storingen geen problemen veroorzaken wanneer ze langdurige bewerkingen, die zich uitstrekken over twee of meer updates voor de permanente status van de service onderbreken. Als wordt geprobeerd een andere rol die bewerking opnieuw uit te voeren, moet het anticiperen op en verwerkt het geval waarin het werk gedeeltelijk is voltooid.

Neem bijvoorbeeld een service waarmee gegevens over meerdere winkels partities. Als een werknemer rol uitvalt terwijl deze een shard wordt verplaatst, wordt de verplaatsing van de shard mogelijk niet voltooien. Of de verplaatsing vanaf het prille begin van een andere werknemer rol, mogelijk veroorzaakt door gegevens van zwevende of beschadiging van gegevens kan worden herhaald. Om problemen te voorkomen, moet langdurige bewerkingen een of beide van de volgende opties:

 * *Idempotency is ingeschakeld*: herhaalbaar zonder neveneffecten. Idempotency is ingeschakeld te, een langdurige bewerking hebben hetzelfde effect, ongeacht het aantal keren uitgevoerd, zelfs wanneer deze wordt onderbroken tijdens de uitvoering.
 * *Incrementeel restartable*: kunnen blijven van het laatste moment van de storing. Als u stapsgewijs opnieuw starten, bestaat een langdurige bewerking uit een reeks kleinere atomaire bewerkingen. Het moet ook de voortgang vastleggen in duurzame opslag, zodat elke volgende aanroep wordt opgehaald wanneer de voorafgaande taak is gestopt.

Tot slot moeten alle langdurige bewerkingen worden ingeroepen herhaaldelijk totdat ze worden voltooid. Bijvoorbeeld kan een provisioning bewerking worden geplaatst in een wachtrij Azure en vervolgens uit de wachtrij verwijderd door de rol van een werknemer alleen als dit lukt. Garbage collector mogelijk opschonen van gegevens bewerkingen onderbroken maken.

###<a name="elasticity"></a>Elasticiteit

Het aantal exemplaren die worden uitgevoerd voor elke rol wordt bepaald in de configuratie van elke rol. Beheerders moeten in eerste instantie configureren voor elke rol met twee of meer exemplaren op basis van de verwachte werklast uit te voeren. Maar kunt u gemakkelijk de schaal rol exemplaren omhoog of omlaag als verbruik patronen wijzigen. U kunt dit handmatig doen in Azure portal of kunt u het proces automatiseren met behulp van Windows PowerShell, de Service Management API of de hulpprogramma's van derden. Zie voor meer informatie [het automatisch schalen van een toepassing](../cloud-services/cloud-services-how-to-scale.md).

###<a name="partitioning"></a>Partitioneren

De Azure fabric-controller gebruikt twee typen partities:

* Een *domein bijwerken* wordt gebruikt om de upgrade van de service rol exemplaren in groepen. Azure implementeert-exemplaren in meerdere domeinen voor update. Deze worden bijgewerkt en wordt opnieuw opgestart voordat u naar de volgende update-domein voor een update in plaats de controller fabric brengt alle exemplaren in één update domein. Deze aanpak voorkomt u dat de volledige service tijdelijk niet beschikbaar zijn tijdens het bijwerken.
* Een *fout met betrekking tot domein* definieert mogelijke punten van hardware-of netwerkfout. Voor elke rol die meer dan één instantie heeft, de controller fabric zorgt u ervoor dat de exemplaren worden verdeeld over meerdere domeinen veroorzaakt, om te voorkomen dat geïsoleerde hardwarefouten service te onderbreken. Fout met betrekking tot domeinen bepalen alle blootstelling aan server en clusterstoringen.

De [Azure serviceniveau overeenkomst (SLA)](https://azure.microsoft.com/support/legal/sla/) zorgt ervoor dat als twee of meer webonderdelen rol exemplaren worden gedistribueerd naar verschillende veroorzaakt domeinen upgraden, ze beschikt over een externe verbinding ten minste 99.95 procent van de tijd. In tegenstelling tot de update-domeinen is er geen manier om te bepalen van het aantal domeinen veroorzaakt. Azure veroorzaakt domeinen wijst automatisch en rol exemplaren verdeeld over hen. AT ten minste de eerste twee exemplaren van elke rol worden in verschillende fout en upgraden van domeinen om ervoor te zorgen dat de SLA wordt voldoen aan elke rol met ten minste twee exemplaren. Dit wordt weergegeven in het volgende diagram.

![Vereenvoudigde weergave van domeinisolatie veroorzaakt](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Taakverdeling

Al het binnenkomende verkeer aan een web-functie wordt doorgegeven via een stateless taakverdeling die clientaanvragen tussen de exemplaren van de rol verdeelt. Rol van afzonderlijke instanties geen openbare IP-adressen en zijn niet rechtstreeks kan vanaf het Internet. Web-rollen zijn ' stateless ' zodat de aanvraag van een client kan worden doorgestuurd naar een exemplaar van de rol. Een [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) -gebeurtenis treedt op elke 15 seconden. U kunt dit gebruiken om aan te geven of de rol is geschikt voor het ontvangen van verkeer of bezet is en moet worden gehouden bij de rotatie van de verdeling van de belasting.

##<a name="virtual-machines"></a>Virtuele Machines

Azure virtuele Machines wijkt af van platform als een service (PaaS) rollen in verschillende opzichten in hoge mate van beschikbaarheid berekenen. In sommige gevallen moet u doen meer werk om een hoge beschikbaarheid garanderen.

###<a name="disk-durability"></a>Duurzaamheid van de schijf

Gegevens die zijn opgeslagen op de stations van de virtuele machine is in tegenstelling tot PaaS rol gevallen permanent zelfs wanneer de virtuele machine wordt overgeplaatst. Azure virtuele machines VM-schijven bestaan als BLOB's in Azure opslag gebruiken. Vanwege de kenmerken van de beschikbaarheid van opslag in Azure is ook de gegevens die zijn opgeslagen op de schijven van een virtuele machine maximaal beschikbaar.

Houd er rekening mee dat station D (in Windows VMs) de uitzondering op deze regel is. Station D is feitelijk fysieke opslag op de rackserver waarop de VM en de gegevens niet verloren als de VM gerecycled wordt. Station D is bedoeld voor tijdelijke opslag alleen. In Linux stelt Azure 'abonnee' (maar niet altijd) de lokale tijdelijke schijf als /dev/sdb block-apparaat. Het is vaak gekoppeld door de Agent Azure Linux als /mnt/resource of mnt koppelpunten (te configureren in /etc/waagent.conf).

###<a name="partitioning"></a>Partitioneren

Azure native begrijpt de lagen in een PaaS-toepassing (Webrol en functie van de werknemer) en dus kan goed verdelen tussen probleem- en domeinen. Daarentegen moeten de lagen in een infrastructuur als (IaaS)-servicetoepassing worden handmatig gedefinieerd door beschikbaarheid sets. Beschikbaarheid sets zijn vereist voor een SLA onder IaaS.

![Beschikbaarheid wordt ingesteld voor Azure virtuele machines](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

In het voorgaande diagram worden de Internet Information Services (IIS)-laag (die werkt als een web app laag) en de SQL-laag (die werkt als een gegevenslaag) toegewezen aan de beschikbaarheid van verschillende sets. Dit zorgt ervoor dat alle exemplaren van elke laag hardware redundantie door virtuele machines te verdelen in verschillende domeinen veroorzaakt en dat hele lagen niet worden genomen omlaag tijdens een update.

###<a name="load-balancing"></a>Taakverdeling

Als de VMs verkeer verdeeld over hen hebben moeten, moet u het VMs in een toepassing en belasting evenwicht groeperen op een specifieke TCP- of UDP-eindpunt. Zie [virtuele machines voor taakverdeling](../virtual-machines/virtual-machines-linux-load-balance.md)voor meer informatie. Als de VMs informatie ontvangt van een andere bron (bijvoorbeeld een wachtrijmechanisme), is een taakverdeling niet vereist. De taakverdeling gebruikt een eenvoudige health check om te bepalen of verkeer moet worden verzonden naar het knooppunt. Het is ook mogelijk te maken van uw eigen sondes implementeren toepassingsspecifieke gezondheid statistieken die bepalen of de VM verkeer moet ontvangen.

##<a name="storage"></a>Opslag

Azure opslag is de basislijn duurzame gegevens voor Azure. Het biedt blob, tabel, wachtrij en VM schijfopslag. Beschikbaarheid binnen een datacenter enkel wordt een combinatie van replicatie en resourcebeheer. De beschikbaarheid van Azure opslag SLA zorgt u ervoor dat ten minste 99,9% van de gevallen:

* Correct opgemaakte aanvragen voor het toevoegen, bijwerken, lezen en verwijderen van gegevens is en correct verwerkt.
* Opslag-accounts hebt verbinding met de Internet-gateway.

###<a name="replication"></a>Replicatie

Azure opslag vergemakkelijkt de duurzaamheid van de gegevens door het onderhouden van meerdere exemplaren van alle gegevens op verschillende schijven op fysieke opslag volledig onafhankelijke subsystemen in het gebied. Gegevens synchroon worden gerepliceerd en alle kopieën worden doorgevoerd voordat het schrijven is bevestigd. Azure opslag sterk consistent is, wat betekent dat leest aangepast aan de meest recente schrijfbewerkingen worden gegarandeerd. Bovendien kopieën van gegevens voortdurend gescand op analyse en herstel bits rot, een vaak onderbelicht bedreiging voor de integriteit van opgeslagen gegevens.

Voordeel voor services van replicatie alleen via Azure opslag. De ontwikkelaar hoeft niet extra werk aan een lokale herstellen.

###<a name="resource-management"></a>Bronbeheer

Opslag-accounts die zijn gemaakt na mei 2014 uitgroeien tot maximaal 500 TB (het vorige maximum is 200 TB). Als extra ruimte vereist is, moeten toepassingen zijn ontworpen voor het gebruik van meerdere accounts voor opslag.

###<a name="virtual-machine-disks"></a>Virtuele machine schijven

De schijf van een virtuele machine wordt opgeslagen als een blob pagina in Azure opslag geven dezelfde eigenschappen voor duurzaamheid en schaalbaarheid als Blob-opslag. Dit ontwerp is de gegevens op de schijf een virtuele machine van permanente, zelfs als de server met de VM mislukt en de VM moet opnieuw worden gestart op een andere server.

##<a name="database"></a>Database

###<a name="sql-database"></a>SQL-Database

Azure SQL-Database biedt een database als een service. Er kan toepassingen snel inrichten, gegevens en relationele databases query invoegen. Biedt veel van de vertrouwde SQL Server-functies en functionaliteit, terwijl de belasting van hardware, configuratie, patches en tolerantie samenvatten.

>[AZURE.NOTE] Azure SQL-Database beschikt niet over een-op-functie pariteit met SQL Server. Het is bedoeld om te voldoen aan een andere reeks eisen--één die uniek is geschikt voor de cloud-toepassingen (elastische schaal, de database als service onderhoudskosten te verlagen, enzovoort). Zie voor meer informatie [kiest u de optie SQL-Server van een wolk: Azure SQL (PaaS)-Database of SQL Server op Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Replicatie

Azure SQL-Database biedt ingebouwde tolerantie bij storing niveau. Alle schrijfbewerkingen naar een database worden automatisch gerepliceerd naar twee of meer knooppunten achtergrond via een quorum commit-techniek. (De primaire en secundaire ten minste één moeten bevestigen dat de activiteit naar het transactielogbestand wordt geschreven voordat de transactie geslaagd geacht is en als resultaat gegeven.) In het geval van storingen in knooppunten overgenomen de database automatisch door een van de secundaire replica's. Hierdoor ontstaat een onderbreking tijdelijke verbinding voor client-toepassingen. Daarom moeten alle clients met Azure SQL-Database een vorm van verwerking van tijdelijke verbinding implementeren. Zie voor meer informatie, [richtlijnen voor specifieke services opnieuw](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Bronbeheer

Elke database bij het maken, is geconfigureerd met een limiet. De beschikbare maximale grootte is 1 TB (grootte grenzen variëren op basis van uw service-tier, Zie [service niveaus en prestaties van Azure SQL-Databases](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Wanneer een database de limiet raakt, weigert het extra opdrachten voor invoegen of bijwerken. (Zoeken en verwijderen van gegevens is nog steeds mogelijk.)

Azure SQL-Database wordt een stof in een database gebruikt om bronnen te beheren. Echter, in plaats van een controller weefsel wordt een ringtopologie fouten opsporen. Elke replica in een cluster heeft twee neighbors en is verantwoordelijk voor het detecteren van wanneer ze naar beneden gaan. Wanneer u een replica uitvalt, trigger de buren een agent opnieuw configureren om het opnieuw te maken op een andere computer. Motor beperken wordt om ervoor te zorgen dat een logische server niet te veel resources op een computer gebruiken of groter zijn dan de fysieke grenzen van de machine geleverd.

###<a name="elasticity"></a>Elasticiteit

Als de toepassing meer dan de limiet van 1 TB-database, moet een aanpak schalen worden geïmplementeerd. U de schaal die met Azure SQL-Database door handmatig partitionering, ook bekend als sharding, gegevens over meerdere SQL-databases. Deze aanpak schalen biedt de mogelijkheid om bijna lineaire groei met schaal. Elastische groei of capaciteit op verzoek kan worden uitgebreid met incrementele kosten indien nodig omdat databases worden gefactureerd op basis van de gemiddelde werkelijke grootte gebruikt per dag, niet op basis van de maximale grootte.

##<a name="sql-server-on-virtual-machines"></a>SQL Server op de virtuele Machines

Door het installeren van SQL Server (versie 2014 of later) op Azure virtuele Machines, kunt u profiteren van de van de traditionele beschikbaarheidfuncties van SQL Server. Deze functies omvatten AlwaysOn beschikbaarheidsgroepen en database-mirroring. Houd er rekening mee dat Azure VMs, opslag en netwerken andere operationele kenmerken dan een op-lokalen, niet-gevirtualiseerde IT-infrastructuur hebben. Een succesvolle implementatie van een hoge beschikbaarheid en herstel in noodgevallen (HA/DR) SQL Server-oplossing in Azure, moet u weten over deze verschillen en ontwerpen van uw oplossing voor hen.

###<a name="high-availability-nodes-in-an-availability-set"></a>Hoge beschikbaarheid-knooppunten in een set van beschikbaarheid

Wanneer u een oplossing met hoge beschikbaarheid in Azure implementeren, kunt u de beschikbaarheid in de Azure plaatst u de knooppunten met hoge beschikbaarheid in een fout met betrekking tot afzonderlijke domeinen en domeinen upgraden. Als u wilt wissen, beschikbaarheid, is de een Azure concept. Het is raadzaam dat u volgen moet om ervoor te zorgen dat uw databases inderdaad beschikbaar zijn, zijn of u AlwaysOn beschikbaarheidsgroepen, database-mirroring of iets anders. Als u deze het beste niet volgt, kunt u mogelijk in de false veronderstelling dat uw systeem maximaal beschikbaar is. Maar in werkelijkheid de knooppunten kunnen mislukken tegelijk omdat ze per ongeluk worden geplaatst in hetzelfde domein in de regio Azure veroorzaakt.

Deze aanbeveling geldt niet als met logboekbestanden. Als een disaster recovery functie, moet u ervoor zorgen dat de servers worden uitgevoerd in afzonderlijke Azure regio's. Deze gebieden zijn per definitie fout met betrekking tot afzonderlijke domeinen.

Voor Azure Cloud Services VMs geïmplementeerd via de klassieke portal in dezelfde beschikbaarheid set, moet u ze implementeren in dezelfde Cloud-Service. VMs geïmplementeerd via Azure Resource Manager (huidige portal) beschikt niet over deze beperking. Voor klassieke portal VMs in Azure Cloud-Service geïmplementeerd, deel alleen knooppunten in dezelfde Service Cloud uitmaken van dezelfde set beschikbaarheid. Bovendien moet de Cloud Services VMs in hetzelfde virtuele netwerk om ervoor te zorgen dat ze hun IP-adressen na het Retoucheerpenseel service behouden. Dit voorkomt verstoring van de DNS-update.

###<a name="azure-only-high-availability-solutions"></a>Alleen Azure: oplossingen met hoge beschikbaarheid

Hebt u een oplossing met hoge beschikbaarheid voor SQL Server-databases in Azure met AlwaysOn beschikbaarheidsgroepen of database-mirroring.

In het volgende diagram ziet u de architectuur van AlwaysOn beschikbaarheidsgroepen met Azure virtuele Machines. Dit diagram is overgenomen uit het uitvoerige artikel daarover, [hoge beschikbaarheid en noodherstel voor SQL Server Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![AlwaysOn beschikbaarheidsgroepen in Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

U kunt ook automatisch een AlwaysOn beschikbaarheidsgroepen implementatie end-to-end op Azure VMs inrichten met behulp van de sjabloon AlwaysOn in Azure portal. Zie [SQL Server AlwaysOn bieden in Microsoft Azure Portal Gallery](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/)voor meer informatie.

In het volgende diagram illustreert het gebruik van database-mirroring Azure virtuele Machines. Het is ook overgenomen van de diepgaande onderwerp [voor hoge beschikbaarheid en noodherstel voor SQL Server Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Database-mirroring in Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Beide architecturen vereisen een domeincontroller. Met database-mirroring is het echter mogelijk met behulp van servercertificaten te elimineren de noodzaak voor een domeincontroller.

##<a name="other-azure-platform-services"></a>Andere services Azure platform

Toepassingen die zijn gebaseerd op Azure profiteren van de platformmogelijkheden van lokale fouten herstellen. In sommige gevallen kunt u specifieke acties te verhogen van de beschikbaarheid van uw specifieke scenario uitvoeren.

###<a name="service-bus"></a>Bus service

Te beperken ten opzichte van een tijdelijke storing van Azure Service Bus, kunt u overwegen een duurzame client-side wachtrij maken. Dit wordt tijdelijk een opslagmechanisme voor alternatieve, lokale gebruikt voor het opslaan van berichten die naar de wachtrij van de Bus-Service kunnen niet worden toegevoegd. De toepassing kunt bepalen hoe berichten worden verwerkt de tijdelijk opgeslagen nadat de service is hersteld. Zie [Aanbevolen procedures voor een Service Bus met prestatieverbeteringen brokered messaging](../service-bus-messaging/service-bus-performance-improvements.md) en [Service Bus (noodherstel)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services)voor meer informatie.

###<a name="hdinsight"></a>HDInsight

De gegevens die is gekoppeld aan de Azure HDInsight wordt standaard opgeslagen in Azure Blob-opslag. Azure opslag geeft eigenschappen voor hoge beschikbaarheid en duurzaamheid voor Blob-opslag. De verwerking van meerdere knooppunten dat is gekoppeld aan taken Hadoop MapReduce vindt plaats op een tijdelijke Hadoop Distributed bestand System (HDFS) die is ingericht als een HDInsight nodig heeft. Resultaten van een project MapReduce worden ook standaard opgeslagen in Azure Blob-opslag, zodat de verwerkte gegevens duurzaam en uiterst beschikbaar blijft nadat het cluster Hadoop is deprovisioned. Zie [HDInsight (noodherstel)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services)voor meer informatie.

##<a name="checklists-for-local-failures"></a>Controlelijsten voor lokale fouten

###<a name="cloud-services"></a>Cloud Services

  1. Raadpleeg de sectie Cloud Services van dit document.
  2. Ten minste twee exemplaren voor elke rol configureren.
  3. Persist-status in duurzame opslag, niet op de rol van instanties.
  4. De gebeurtenis StatusCheck goed verwerkt.
  5. Wijzigingen in transacties mogelijk laten teruglopen.
  6. Controleer of de werknemer rol taken zijn idempotency is ingeschakeld en opnieuw starten.
  7. Verder bewerkingen aanroepen totdat ze worden voltooid.
  8. Overweeg autoscaling strategieën.

###<a name="virtual-machines"></a>Virtuele Machines

  1. Raadpleeg de sectie virtuele Machines van dit document.
  2. Gebruik geen station D voor permanente opslag.
  3. Groep computers in een bepaalde servicelaag in een set beschikbaarheid.
  4. Taakverdeling en optionele sondes configureren.

###<a name="storage"></a>Opslag

  1. Raadpleeg de sectie opslag van dit document.
  2. Gebruik verschillende accounts voor opslag wanneer gegevens of bandbreedte quota overschrijdt.

###<a name="sql-database"></a>SQL-Database

  1. Raadpleeg de sectie SQL-Database van dit document.
  2. Implementeer een beleid voor nieuwe pogingen voor het afhandelen van fouten van voorbijgaande aard.
  3. Partitioneren/sharding gebruiken als een scale-out strategie.

###<a name="sql-server-on-virtual-machines"></a>SQL Server op de virtuele Machines

  1. Controleer de SQL-Server op de virtuele Machines gedeelte van dit document.
  2. Volg de bovenstaande aanbevelingen voor virtuele Machines.
  3. Gebruik SQL Server hoge beschikbaarheid, zoals AlwaysOn.

###<a name="service-bus"></a>Bus service

  1. Raadpleeg de sectie Service Bus van dit document.
  2. Kunt u een duurzame client-side wachtrij als een back-up te maken.

###<a name="hdinsight"></a>HDInsight

  1. Raadpleeg de sectie HDInsight van dit document.
  2. Er zijn geen stappen meer beschikbaarheid vereist voor lokale fouten.

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie gericht op [Azure resiliency technische richtsnoeren](./resiliency-technical-guidance.md). Het volgende artikel in deze serie is [herstel na een onderbreking van de service van de hele regio](./resiliency-technical-guidance-recovery-loss-azure-region.md).
