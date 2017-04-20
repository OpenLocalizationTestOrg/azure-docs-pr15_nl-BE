<properties
   pageTitle="Batch- en HPC-oplossingen in de cloud | Microsoft Azure"
   description="Informatie over partij en high performance computing (HPC en grote berekenen) scenario's en oplossingsopties in Azure"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="07/27/2016"
   ms.author="danlep"/>

# <a name="batch-and-hpc-solutions-in-the-azure-cloud"></a>Batch- en HPC-oplossingen in de cloud Azure

Azure biedt efficiënte, schaalbare cloud-oplossingen voor batch- en high performance computing (HPC) - ook wel *Grote berekenen*. Hier informatie over het berekenen van grote werkbelasting en de Azure services worden ondersteund, of rechtstreeks naar [Oplossingsscenario's](#scenarios) verderop in dit artikel gaan. Dit artikel is voornamelijk voor technische besluitvormers en IT-managers onafhankelijke softwareleveranciers, maar andere IT-professionals en ontwikkelaars kunnen gebruiken om te leren over deze oplossingen.

Organisaties hebben grote problemen met computergebruik: technisch ontwerp en analyse, beeldweergave complexe modellering, Monte Carlo-simulaties, berekeningen van de financiële risico's en anderen. Azure helpt organisaties deze problemen oplossen met de middelen, de schaal en de plannen die ze nodig hebben. Met Azure kunnen organisaties:

* Hybride oplossingen, het uitbreiden van een HPC-cluster op ruimten voor offload piek belasting naar de cloud

* Hulpmiddelen voor HPC-cluster en de werklast volledig in Azure uitvoeren

* Gebruik van beheerde en schaalbare Azure services zoals [Batch](https://azure.microsoft.com/documentation/services/batch/) computerintensieve werklasten uitvoeren zonder te hoeven implementeren en beheren van infrastructuur compute

Hoewel het buiten het bestek van dit artikel, Azure ook biedt ontwikkelaars en partners een volledige set mogelijkheden architectuur keuzen en ontwikkelingsprogramma's te maken van grootschalige, aangepaste werkstromen voor grote berekenen. En een groeiend ecosysteem van partners is klaar om te helpen u uw werkbelasting grote berekenen in de Azure cloud productief maken.


## <a name="batch-and-hpc-applications"></a>Batch- en HPC-toepassingen

Toepassingen en veel van branchespecifieke toepassingen, batch en HPC-toepassingen hebben een gedefinieerde begin en einde in tegenstelling tot het web en deze kunnen worden uitgevoerd op een schema of op verzoek, soms voor een uur of langer. De meeste worden onderverdeeld in twee hoofdcategorieën: *intrinsiek parallelle* (ook wel 'embarrassingly parallel', omdat de problemen die ze oplossen parallel uitgevoerd op meerdere computers of processors) en *nauw verbonden*. Zie de volgende tabel voor meer informatie over deze toepassingstypen. Sommige Azure oplossing benadert het werk beter voor een bepaald type of het andere.

>[AZURE.NOTE] Een *taak*wordt normaal gesproken aangeroepen door een actieve sessie van een toepassing in Batch- en HPC-oplossingen en elke taak kan krijgen onderverdeeld in *taken*. En de compute geclusterde bronnen voor de toepassing worden vaak genoemd *computerknooppunten*.

Type | Kenmerken | Voorbeelden
------------- | ----------- | ---------------
**Intrinsiek parallel**<br/><br/>![Intrinsiek parallel][parallel] |• Individuele computers toepassingslogica onafhankelijk uitvoeren<br/><br/> • Toevoegen computers kan de toepassing schalen en minder tijd berekening<br/><br/>• Toepassing bestaat uit afzonderlijke uitvoerbare bestanden of is verdeeld in een groep services die worden aangeroepen door een client (een service oriented architecture of SOA, toepassing) |• Financiële risico modellering<br/><br/>• Beeldweergave en verwerking van afbeeldingen<br/><br/>• Media-codering en transcodering<br/><br/>• Monte Carlo-simulaties<br/><br/>• Software testen
**Nauw verbonden**<br/><br/>![Nauw verbonden][coupled] |• Toepassing vereist computerknooppunten interactie of tussentijdse resultaten uitwisselen<br/><br/>• Compute nodes kunnen communiceren met het Message Passing Interface (MPI), een gemeenschappelijke communicatieprotocol voor parallelle computers<br/><br/>• De toepassing is gevoelig voor netwerk- en bandbreedte<br/><br/>• Prestaties kunnen worden verbeterd met behulp van snelle netwerktechnologieën, zoals InfiniBand en externe directe geheugentoegang (RDMA) |• Olie- en reservoirmodellen<br/><br/>• Engineering-ontwerp en analyse, zoals computational fluid dynamics<br/><br/>• Fysieke simulaties zoals auto crashes en nucleaire reacties<br/><br/>• Weersvoorspellingen

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Overwegingen voor het uitvoeren van batch- en HPC-toepassingen in de cloud

U kunt gemakkelijk veel toepassingen die zijn ontworpen in de HPC-clusters voor bedrijfsruimten voor Azure, of voor het milieu van een hybride (cross-ruimten) migreren. Echter, kunnen er bepaalde beperkingen of overwegingen, waaronder:


* **Beschikbaarheid van resources cloud** - afhankelijk van het type wolk compute resources die u gebruikt, mogelijk niet kunnen vertrouwen op de machine continue beschikbaarheid terwijl een taak wordt uitgevoerd. Verwerking status en voortgang te controleren aan te wijzen zijn veelgebruikte methoden voor het verwerken van mogelijke tijdelijke storingen en meer nodig bij het gebruik van cloud resources.


* **Data access** - gegevens toegang technieken vaak beschikbaar in de enterprise-clusters, zoals NFS, mogelijk speciale configuratie in de cloud. Of moet u mogelijk verschillende data access-procedures en -patronen voor de wolk vast te stellen.

* **Verplaatsing van gegevens** - zijn voor toepassingen die het verwerken van grote hoeveelheden gegevens, strategieën nodig verplaatst u de gegevens in de cloud opslag en resources te berekenen. Mogelijk moet u snelle toegang zoals [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)cross-ruimten. Ook rekening houden met wettelijke, regelgevende, of beperkingen van beleid wilt opslaan of toegang tot die gegevens.


* **Licentiëring** - selectievakje met de leverancier van een commerciële toepassing voor licenties of andere beperkingen voor het uitvoeren in de cloud. Niet alle leveranciers bieden "pay-as-you-go"-licentiëring. Mogelijk moet u van plan bent voor een server voor licentieverlening in de cloud voor uw oplossing, of verbinding maken met een licentieserver op gebouwen.


### <a name="big-compute-or-big-data"></a>Grote Compute of grote gegevens?

De scheidslijn tussen grote berekenen en Big Data toepassingen is niet altijd duidelijk en bepaalde toepassingen mogelijk kenmerken van beide. Beide dient uit te voeren berekeningen van grootschalige, meestal in clusters van computers. Maar de oplossing benaderingen en ondersteunende hulpmiddelen kunnen verschillen.

• **Grote berekenen** vaak betrekking hebben op toepassingen die afhankelijk zijn van de processorkracht en geheugen, zoals technische simulaties, financiële risico modellering en digitale weergave. De infrastructuur voor een grote berekenen oplossing mogelijk zijn computers met gespecialiseerde processors met meerdere kernen ruwe berekening uitvoeren en de gespecialiseerde, snelle netwerkhardware om de computers te verbinden.

• **Grote gegevens** is opgelost problemen met analyse van gegevens die betrekking hebben op grote hoeveelheden gegevens die niet worden beheerd met een enkele computer of database managementsysteem. Voorbeelden zijn grote hoeveelheden van weblogs of andere bedrijfsgegevens. Grote gegevens vaak meer vertrouwen op de schijf en i/o-prestaties dan op CPU-capaciteit. Er zijn ook speciale grote hulpmiddelen zoals Apache Hadoop voor het beheren van het cluster en de data-partitie. (Zie voor meer informatie over Azure, HDInsight en andere oplossingen Azure Hadoop [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## <a name="compute-management-and-job-scheduling"></a>Beheer- en taakplanning berekenen

Uitvoeren van Batch- en HPC-toepassingen vaak bestaat uit een *cluster manager* en een *job scheduler* compute geclusterde bronnen beheren en deze toewijzen aan de toepassingen die de taken worden uitgevoerd. Deze functies kunnen worden uitgevoerd door afzonderlijke hulpprogramma's, of een geïntegreerd hulpprogramma of service.

* **Cluster manager** - bepalingen, releases en beheert resources (of het berekenen van knooppunten). Een cluster manager automatiseert de installatie van installatiekopieën van besturingssystemen en toepassingen op de compute nodes kan eisen de compute resources schalen en bewaken van de prestaties van de knooppunten.

* **Job scheduler** - Hiermee geeft u de resources (zoals processors of geheugen) een toepassing behoeften en de omstandigheden wanneer deze wordt uitgevoerd. Een job scheduler onderhoudt een wachtrij van taken en resources toe op basis van een toegewezen prioriteit of andere kenmerken worden toegewezen.

Clustering en hulpprogramma's voor Windows- en Linux clusters voor taakplanning kunnen migreren naar Azure goed. Bijvoorbeeld, biedt [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), Microsoft vrije compute clusteroplossing voor Windows en Linux HPC werkbelasting, verschillende opties voor het uitvoeren van in Azure. U kunt ook Linux-clusters om uit te voeren open source hulpmiddelen als koppel en SLURM maken. U kunt raster commerciële oplossingen ook overbrengen naar Azure zoals [TIBCO DataSynapse GridServer](http://www.tibco.com/company/news/releases/2016/tibco-to-accelerate-cloud-adoption-of-banking-and-capital-markets-customers-via-microsoft-collaboration)en [IBM Platform Symphony](http://www-01.ibm.com/support/docview.wss?uid=isg3T1023592) [Univa raster Engine](http://www.univa.com/products/grid-engine).

Zoals in de volgende secties, kunt u ook profiteren van Azure services compute netwerkbronnen beheren en plannen van taken zonder (of naast) traditionele cluster management tools.


## <a name="scenarios"></a>Scenario 's

Hier zijn drie veelvoorkomende scenario's grote berekenen werklasten in Azure uitvoeren met behulp van bestaande oplossingen voor HPC-cluster, Azure services of een combinatie van de twee. Belangrijke aandachtspunten bij het kiezen van elk scenario worden weergegeven, maar niet limitatief. Meer over de beschikbare Azure services die kunt u in uw oplossing hiervoor wordt verderop in het artikel.

  | Scenario | Waarom kiezen?
------------- | ----------- | ---------------
**Een HPC-cluster met Azure burst**<br/><br/>[! [Cluster burst] [burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Meer informatie:<br/>• [Burst Azure werknemer exemplaren met HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Stel een hybride compute cluster met HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Burst Azure batch met HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/>|• Combineren uw [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) of andere cluster op ruimten met extra Azure bronnen in een hybride oplossing.<br/><br/>• Breid uw werkbelasting grote berekenen uit te voeren op een Platform als Service (PaaS) exemplaren van de virtuele machine (momenteel alleen Windows Server).<br/><br/>• Toegang tot een winkel op ruimten license server of gegevens met behulp van een optionele Azure virtueel netwerk|• U hebt een bestaand HPC-cluster en meer bronnen nodig hebben <br/><br/>• U niet wilt kopen, en meer HPC-cluster infrastructuur beheren<br/><br/>• U hebt tijdelijke piek vraag periodes of speciale projecten
**Een HPC-cluster maken geheel in Azure**<br/><br/>[! [Cluster in IaaS] [iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Meer informatie:<br/>• [HPC-clusteroplossingen in Azure](./big-compute-resources.md)<br/><br/>|• Snel en consistent implementeren uw toepassingen en hulpprogramma's cluster op standaard of aangepaste Windows- of Linux-infrastructuur als een service (IaaS) virtuele machines.<br/><br/>• Verschillende werkbelastingen grote berekenen uitvoeren met behulp van de oplossing van uw keuze voor de taakplanning.<br/><br/>• Gebruik extra Azure services, met inbegrip van netwerk- en volledige cloud-gebaseerde oplossingen te maken. |• U niet wilt kopen en meer Linux of Windows HPC-cluster infrastructuur beheren<br/><br/>• U hebt tijdelijke piek vraag periodes of speciale projecten<br/><br/>• U hebt een extra nodig voor een cluster maar niet hoeft te investeren in computers en ruimte te implementeren<br/><br/>• U wilt verplaatsten de intensieve toepassing kan worden uitgevoerd als een service volledig in de cloud
**Schaal van een parallelle toepassing Azure**<br/><br/>[! [Azure Batch] [batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Meer informatie:<br/>• [Basisprincipes van Azure Batch](./batch-technical-overview.md)<br/><br/>• [Aan de slag met de bibliotheek Azure Batch voor .NET](./batch-dotnet-get-started.md)|• Ontwikkelen met [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) voor het schalen van verschillende grote berekenen werklasten worden uitgevoerd op groepen van Windows of Linux virtuele machines.<br/><br/>• Een Azure platform-service gebruiken voor het beheren van implementatie en autoscaling van virtuele machines, taakplanning noodherstel, verplaatsing van gegevens, afhankelijkheid management en implementatie van toepassingen.|• U niet wilt dat voor het beheren van bronnen of een job scheduler; berekenen in plaats daarvan wilt u zich concentreren op uw toepassingen<br/><br/>• U wilt verplaatsten de intensieve toepassing kan worden uitgevoerd als een service in de cloud<br/><br/>• U automatisch schalen wilt zodat deze overeenkomen met de werkbelasting compute compute resources


## <a name="azure-services-for-big-compute"></a>Azure services voor grote berekenen

Hier volgt meer informatie over de compute, gegevens, netwerken en verwante services die kunt u combineren voor grote berekenen oplossingen en workflows. Zie de [documentatie](https://azure.microsoft.com/documentation/)van Azure services voor gedetailleerde richtlijnen over Azure services. De [scenario's](#scenarios) eerder in dit artikel bevatten slechts enkele manieren om deze services te gebruiken.

>[AZURE.NOTE] Azure introduceert regelmatig nieuwe services die nuttig voor uw scenario zijn kunnen. Als u vragen hebt, neem u contact op met een [partner Azure](https://pinpoint.microsoft.com/en-US/search?keyword=azure) of e- *bigcompute@microsoft.com*.

### <a name="compute-services"></a>Services berekenen

Compute Azure services zijn de kern van een oplossing voor grote berekenen en de verschillende compute services bieden voordelen voor verschillende scenario's. Deze services bieden op een basisniveau, verschillende modi voor toepassingen uit te voeren op de exemplaren berekenen op basis van een virtuele machine die Azure biedt Windows Server Hyper-V-technologie gebruiken. Deze instanties kunnen standaard- en aangepaste Linux en Windows-besturingssystemen en hulpprogramma's worden uitgevoerd. Azure biedt u een selectie van [instantie formaten](../virtual-machines/virtual-machines-windows-sizes.md) met verschillende configuraties van CPU cores, geheugen, schijf en andere kenmerken. Afhankelijk van uw behoeften, kunt u de exemplaren met duizenden cores te schalen en verminder de wanneer u minder bronnen nodig.

>[AZURE.NOTE] Voordeel van de Azure intensieve exemplaren voor het verbeteren van de prestaties en schaalbaarheid van HPC werkbelasting, met inbegrip van parallelle MPI toepassingen waarvoor een lage latentie en hoge doorvoersnelheden toepassingennetwerk nemen. Zie [informatie over het VMs van H-serie en intensieve A-serie](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).  

Service | Beschrijving
------------- | -----------
**[Virtuele machines](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Geef infrastructuur berekenen als een service (IaaS) met behulp van Microsoft Hyper-V-technologie<br/><br/>• Kunt u flexibel inrichten en permanente wolk computers beheren van standaard Windows Server- of Linux-afbeeldingen uit de [Marketplace Azure](https://azure.microsoft.com/marketplace/), of afbeeldingen en gegevensschijven die u opgeeft<br/><br/>• Kan worden geïmplementeerd en beheerd als [VM schaal Sets](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) grootschalige services van identieke virtuele machines met autoscaling vergroten of verkleinen capaciteit automatisch maken<br/><br/>• Uitgevoerd in lokalen compute cluster tools en toepassingen volledig in de cloud<br/><br/>
**[Cloud services](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Kunt berekenen voor grote toepassingen uitvoeren in werknemer rol-exemplaren die worden beheerd door Azure geheel en zijn virtuele machines met een versie van Windows Server<br/><br/>• Schaalbare en betrouwbare toepassingen met lage administratieve overhead, uitgevoerd in een platform als servicemodel (PaaS) inschakelen<br/><br/>• Moet u mogelijk extra hulpmiddelen of ontwikkeling te integreren met voor bedrijfsruimten HPC-clusteroplossingen
**[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Grootschalige parallel en batch werklasten in een volledig beheerde service wordt uitgevoerd<br/><br/>• Biedt taakplanning en autoscaling uit een beheerde groep van virtuele machines<br/><br/>• Kunnen ontwikkelaars bouwen en toepassingen worden uitgevoerd als een service of wolk bestaande toepassingen inschakelen<br/>

### <a name="storage-services"></a>Storage-services

Een oplossing voor het berekenen van grote meestal werkt op een set van invoergegevens en genereert gegevens voor de resultaten. Enkele van de Azure storage-services gebruikt in grote berekenen oplossingen zijn:

* [Blob, tabel, en de opslag van de wachtrij](https://azure.microsoft.com/documentation/services/storage/) - beheren van grote hoeveelheden ongestructureerde gegevens, NoSQL gegevens en berichten voor workflow en communicatie, respectievelijk. Zo kunt u blob-opslag voor grote verzamelingen van technische gegevens, of voor de invoerafbeeldingen of mediabestanden voor uw toepassing verwerkt. U kunt wachtrijen voor asynchrone communicatie in een oplossing. Zie [Inleiding tot Microsoft Azure opslag](../storage/storage-introduction.md).

* [Azure bestandsopslag](https://azure.microsoft.com/services/storage/files/) - aandelen algemene bestanden en gegevens in de standaard SMB-protocol, dat nodig voor bepaalde oplossingen HPC-cluster is met Azure.

* [Gegevensarchief Lake](https://azure.microsoft.com/services/data-lake-store/) - biedt een ' hyperscale ' Apache Hadoop Distributed File System voor de cloud voor batch, real-time, handig en interactieve analytics.

### <a name="data-and-analysis-services"></a>Gegevens en analyse

Sommige grote berekenen gevallen betrekking hebben op grootschalige gegevensstromen of verdere verwerking of analyse benodigde gegevens genereren. Azure biedt verschillende gegevens en analysis services, waaronder:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) - Builds gegevensgestuurde werkstromen pijpleidingen () die join, cumulatief en transformatie gegevens uit voor bedrijven, cloud-gebaseerde en Internet gegevensarchieven.

* [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/) - biedt de belangrijkste functies van Microsoft SQL Server relationeel database management systeem in een beheerde service.

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) - implementeert en voorzieningen van Windows Server of Apache Hadoop Linux gebaseerde clusters in de cloud te beheren, analyseren en rapporteren over big data.

* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) - helpt u bij het maken, testen, gebruiken en anticiperende analytische oplossingen in een volledig beheerde service beheren.

### <a name="additional-services"></a>Aanvullende services

Uw oplossing voor grote berekenen wellicht andere Azure services verbinding maken met bronnen op bedrijfsruimten of in andere omgevingen. Voorbeelden zijn:

* [Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) - een logisch geïsoleerde sectie gemaakt in Azure verbinding Azure resources op elkaar of op uw datacenter op gebouwen. Met een virtueel netwerk meerdere ruimten, grote berekenen toepassingen toegang tot gegevens in de lokalen, Active Directory-services en licentieservers

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) - maakt een persoonlijke verbinding tussen Microsoft datacenters en infrastructuur die op het bedrijf of in een omgeving met collocatie. ExpressRoute biedt hogere veiligheid, meer betrouwbaarheid hogere snelheden en lagere vertragingstijden dan normale verbindingen via het Internet.

* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) - biedt verschillende mechanismen voor toepassingen communiceren of uitwisseling van gegevens, of ze op Azure, bevinden zich op een andere cloud-platform of in een datacenter.

## <a name="next-steps"></a>Volgende stappen

* Zie de [Technische bronnen voor Batch- en HPC](big-compute-resources.md) aan technische richtlijnen voor het bouwen van uw oplossing.

* Bespreek uw Azure opties met partners, met inbegrip van de cyclus Computing en UberCloud.

* Meer informatie over grote berekenen Azure oplossingen geleverd door [Torens Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/) [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)en [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).

* Zie de [Microsoft HPC en Batch teamblog](http://blogs.technet.com/b/windowshpc/) en de [Azure blog](https://azure.microsoft.com/blog/tag/hpc/)voor de meest recente aankondigingen.

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
