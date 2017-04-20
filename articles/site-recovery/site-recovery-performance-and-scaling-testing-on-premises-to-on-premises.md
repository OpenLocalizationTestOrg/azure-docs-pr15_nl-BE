<properties
    pageTitle="Test waarbij de prestaties en de schaal-resultaten voor op ruimten met Hyper-V-replicatie voor ruimten met terugwinning van de Site | Microsoft Azure"
    description="Dit artikel bevat informatie over de prestatietest voor op ruimten voor bedrijfsruimten replicatie met Azure Site herstellen."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Prestaties testen en de resultaten voor op-premises Hyper-V-replicatie voor ruimten met terugwinning van de Site aanpassen

U kunt Microsoft Azure Site herstellen goedkeuringen en het beheer van de replicatie van virtuele machines en fysieke servers Azure of een secundaire datacenter. Dit artikel bevat de resultaten van prestatietests Hyper-V virtuele machines te repliceren tussen twee toen op-locatie datacenters.



## <a name="overview"></a>Overzicht

Het doel van deze tests is te onderzoeken hoe Azure Site herstel uitvoert tijdens de replicatie van de stationaire toestand. Replicatie van de stationaire toestand vindt plaats wanneer virtuele machines eerste replicatie is voltooid en deltawijzigingen synchroniseert. Het is belangrijk voor het meten van prestaties met behulp van de stationaire toestand is de staat waarin de meeste virtuele machines blijven, tenzij u onverwachte storingen optreden.


De implementatie test bestond uit twee op ruimten sites met een VMM server in elke site. Deze testinstallatie is typisch voor de implementatie van een hoofd/kantoor office met het hoofdkantoor fungeert als de primaire site en het filiaal als de secundaire of herstel van de site.

### <a name="what-we-did"></a>Wat we hebben gedaan

Dit is wat we is in de test geslaagd:

1. Virtuele machines met behulp van VMM sjablonen gemaakt.

1. Virtuele machines en prestatiemetingen van opname basislijn gedurende 12 uur gestart.

1. Gemaakte wolken op primaire en herstel VMM-servers.

1. Bescherming van de geconfigureerde wolk in Azure Site terugwinning, met inbegrip van de toewijzing van bron- en herstel wolken.

1. Ingeschakeld voor virtuele machines en hen in staat stellen om de initiële replicatie is voltooid.

1. Een paar uur voor stabilisatie systeem gewacht.

1. Vastgelegde prestatiegegevens gedurende 12 uur ervoor te zorgen dat alle virtuele machines in een verwachte replicatiestatus bleef die 12 uur.

1. Meet de delta tussen de prestatiegegevens van de basislijn en de prestatiemetingen van replicatie.

## <a name="test-deployment-results"></a>Implementatie van testresultaten

### <a name="primary-server-performance"></a>Prestaties van de primaire server.

- Hyper-V-Replica asynchroon wijzigingen worden bijgehouden in een logboekbestand met minimale opslagoverhead op de primaire server.

- Hyper-V-Replica zelf onderhouden geheugencache te minimaliseren IOP's overhead voor tracering gebruikt. Deze worden opgeslagen naar de VHDX in het geheugen geschreven en ze leegmaken in het logboekbestand voor de tijd die het logboek wordt verzonden naar de site herstellen. Een schijf leegmaken gebeurt ook als een vooraf vastgestelde limiet bereikt, wordt het schrijven.

- Het volgende diagram ziet u de overhead van de stationaire toestand IOP's voor replicatie. U ziet het IOP's overhead als gevolg van replicatie is ongeveer 5% is zeer beperkt.

![Primaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Geheugen op de primaire server voor een optimale schijfprestaties wordt gebruikgemaakt van Hyper-V-Replica. Zoals in het volgende diagram wordt weergegeven, is het geheugen overhead op alle servers in de primaire cluster randnummer. De overhead geheugen is het percentage van het geheugen dat wordt gebruikt door middel van replicatie in vergelijking met de totale hoeveelheid geïnstalleerd geheugen op de Hyper-V server.

![Primaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V-Replica heeft minimale CPU-overhead. Zoals u in de grafiek, is de overhead bij replicatie in het bereik van 2-3%.

![Primaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Prestaties van de secundaire (herstellen)

Hyper-V-Replica maakt gebruik van een kleine hoeveelheid geheugen op de herstelserver optimaliseren het nummer van de opslag. De grafiek geeft een overzicht van het geheugengebruik op de herstelserver. De overhead geheugen is het percentage van het geheugen dat wordt gebruikt door middel van replicatie in vergelijking met de totale hoeveelheid geïnstalleerd geheugen op de Hyper-V server.

![Secundaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Het bedrag van i/o-bewerkingen op de site recovery is een functie van het aantal schrijfbewerkingen op de primaire site. We bekijken de totale i/o-bewerkingen op de site herstellen in vergelijking met de totale i/o-bewerkingen en schrijfbewerkingen op de primaire site. De grafieken blijkt dat de totale IOP's op de site herstellen

- Ongeveer 1,5 keer de schrijven IOP's op de primaire.

- Ongeveer 37% van de totale IOP's op de primaire site.

![Secundaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Secundaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Werking van replicatie voor netwerkgebruik

Gemiddeld 275 MB per seconde van de bandbreedte van het netwerk is ten opzichte van een bestaande bandbreedte van 5 GB per seconde gebruikt tussen de knooppunten van het primaire en het herstel (met gebruik van compressie).

![Resultaten van netwerk in gebruik](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Effect van de replicatie van de prestaties van de virtuele machine

Een belangrijke overweging is de invloed van de replicatie op productie werkbelasting op de virtuele machines. Als de primaire site goed is ingericht voor replicatie, hoeft er niet een impact op de werkbelasting. Mechanisme voor het bijhouden van de Replica van de Hyper-V-lightweight zorgt in de virtuele machines met werklasten ondervinden geen hinder tijdens de replicatie van de stationaire toestand. Dit wordt geïllustreerd in de volgende grafieken.

Deze grafiek toont IOP's uitgevoerd door virtuele machines met verschillende werkbelastingen voordat en nadat de replicatie is ingeschakeld. U kunt zien dat er geen verschil tussen de twee is.

![Replica effect resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

De volgende grafiek ziet u de doorvoer van virtuele machines met verschillende werkbelastingen voordat en nadat de replicatie is ingeschakeld. U kunt zien dat replicatie geen significant effect heeft.

![Resultaten replica effecten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Conclusie

De resultaten blijkt duidelijk dat Azure Site herstel, in combinatie met Hyper-V-Replica wordt geschaald en met minimale overhead voor een grote cluster.  Azure-Site-Recovery biedt eenvoudige implementatie, replicatie, beheer en controle. Replica van de Hyper-V biedt de noodzakelijke infrastructuur voor replicatie schalen. Wij stellen dat u de [Hyper-V Replica Capacity Planner](https://www.microsoft.com/download/details.aspx?id=39057)downloaden voor een optimale implementatie plannen.

## <a name="test-environment-details"></a>Details van de omgeving

### <a name="primary-site"></a>Primaire site

- De primaire site heeft een cluster met vijf Hyper-V-servers waarop de 470 virtuele machines.

- De virtuele machines voert verschillende werkbelastingen en alle Azure Site bescherming ingeschakeld hebben.

- Opslag voor het clusterknooppunt wordt geleverd door een iSCSI SAN. Model: Hitachi HUS130.

- Elke clusterserver heeft vier netwerkkaarten (NIC's) van een Gbps.

- Twee van de netwerkkaarten zijn verbonden met een particulier netwerk voor iSCSI- en twee zijn verbonden met een externe bedrijfsnetwerk. Een van de externe netwerken is gereserveerd voor alleen clustercommunicatie.

![Primaire hardwarevereisten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Server|RAM-GEHEUGEN|Model|Processor|Aantal processors|NIC|Software|
|---|---|---|---|---|---|---|
|Hyper-V-servers in het cluster: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25|128ESTLAB HOST25 is 256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5 4620 0 @ 2,20 GHz|4|Ik x 4 Gbps|Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol|
|VMM-Server|2|||2|1 Gbps|Windows Server-Database 2012 R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Secundaire (recovery) site

- De secundaire site heeft een zes-failover-cluster.

- Opslag voor het clusterknooppunt wordt geleverd door een iSCSI SAN. Model: Hitachi HUS130.

![Primaire hardwarespecificaties](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Server|RAM-GEHEUGEN|Model|Processor|Aantal processors|NIC|Software|
|---|---|---|---|---|---|---|
|Hyper-V-servers in het cluster: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10|96|Dell™ PowerEdge™ R720|Intel(R) Xeon(R) CPU E5-2630 0 @ 2.30GHz|2|Ik x 4 Gbps|Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol|
|ESTLAB HOST17|128|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5 4620 0 @ 2,20 GHz|4||Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol|
|ESTLAB HOST24|256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5 4620 0 @ 2,20 GHz|2||Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol|
|VMM-Server|2|||2|1 Gbps|Windows Server-Database 2012 R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Werklast van servers

- Wij opgenomen workloads die vaak worden gebruikt in het bedrijfsleven van de klant voor testdoeleinden.

- We gebruiken [IOMeter](http://www.iometer.org) met de werkbelasting kenmerk samengevat in de tabel voor de simulatie.

- Alle IOMeter profielen zijn ingesteld op willekeurige bytes voor het simuleren van slechtste patronen voor een standaardwerkbelasting schrijven schrijven.

|Werkbelasting|I/o-grootte (KB)|Toegang %|Lezen %|Uitstekende i/o 's|I/o-patroon|
|---|---|---|---|---|---|
|File Server|48163264|60% 20%-5 %5% 10%|80% 80% 80% 80% 80%|88888 verwijderd|Alle 100% willekeurig|
|(Volume 1) van SQL Server SQL Server (volume 2)|864|100% 100%|70 %0%|88|100% random100% sequentieel|
|Exchange|32|100%|67%|8|100%, willekeurige|
|Workstation/VDI|464|66% 34%|70%-95%|11|Beide willekeurige 100%|
|Webserver-bestand|4864|33% 34% 33%|95% 95%-95%|888|Alle 75% willekeurige|

### <a name="virtual-machine-configuration"></a>Configuratie van de virtuele machine

- 470 virtuele machines op de primaire cluster.

- Alle virtuele machines met schijf VHDX.

- Virtuele machines met werklasten samengevat in de tabel. Alle zijn met VMM sjablonen gemaakt.

|Werkbelasting|# VMs|Minimum RAM (GB)|Maximum RAM-geheugen (GB)|Logische schijfgrootte (GB) per VM|Maximale IOP 's|
|---|---|---|---|---|---|
|SQL Server|51|1|4|167|10|
|Exchange Server|71|1|4|552|10|
|File Server|50|1|2|552|22|
|VDI|149|.5|1|80|6|
|Webserver|149|.5|1|80|6|
|TOTAAL|470|||96.83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Azure Site herstelinstellingen

- Azure-Site-Recovery is geconfigureerd voor op ruimten bescherming voor gebouwen

- De VMM-server heeft vier wolken geconfigureerd, met de Hyper-V clusterservers en hun virtuele machines.

|Primaire VMM cloud|Beveiligde virtuele machines in de cloud|Frequentie van replicatie|Herstel met extra punten|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 minuten|Geen|
|PrimaryCloudRpo30s|47|30 sec.|Geen|
|PrimaryCloudRpo30sArp1|47|30 sec.|1|
|PrimaryCloudRpo5m|235|5 minuten|Geen|

### <a name="performance-metrics"></a>Prestatiegegevens

De tabel bevat een overzicht van de performance metrics en items die zijn gemeten in de implementatie.

|Metric|Teller|
|---|---|
|CPU|\Processor(_Total)\% processortijd|
|Beschikbaar geheugen|\Memory\Available megabytes (MB)|
|IOP 'S|\PhysicalDisk (_Totaal) \Disk-overdrachten per seconde|
|VM leesbewerkingen (IOP's) per seconde|Apparaat voor virtuele opslag van \Hyper-V (<VHD>) \Read per seconde|
|VM schrijfbewerkingen (IOP's) per seconde|Apparaat voor virtuele opslag van \Hyper-V (<VHD>) \Write Operations/S|
|VM doorvoer lezen|Apparaat voor virtuele opslag van \Hyper-V (<VHD>) \Read Bytes per seconde|
|VM-schrijfbewerkingen|Apparaat voor virtuele opslag van \Hyper-V (<VHD>) \Write Bytes per seconde|


## <a name="next-steps"></a>Volgende stappen

- [De beveiliging tussen twee op ruimten VMM sites instellen](site-recovery-vmm-to-vmm.md)
