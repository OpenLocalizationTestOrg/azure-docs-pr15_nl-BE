<properties
    pageTitle="VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstellen | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe Azure Site herstellen om replicatie, failover en herstel op ruimten VMware virtuele machines en fysieke servers Windows/Linux met Azure evoluerende implementeren."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstellen

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Klassieke Portal](site-recovery-vmware-to-azure-classic.md)
- [Klassieke Portal (verouderd)](site-recovery-vmware-to-azure-classic-legacy.md)


De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md).

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u kunt:

- **Repliceren VMware virtuele machines, Azure**-Site herstellen implementeren voor het coördineren van replicatie, failover en herstel voor bedrijfsruimten VMware virtuele machines naar Azure opslag.
- **Azure-fysieke servers te repliceren**, Azure Site herstel te coördineren, replicatie, failover en herstel voor bedrijfsruimten fysieke Windows en Linux servers Azure implementeren.

>[AZURE.NOTE] In dit artikel wordt beschreven hoe om te repliceren naar Azure. Als u wilt dat VMs VMware of fysieke Windows/Linux-servers repliceren naar een secundaire datacenter, volg de instructies in [dit artikel](site-recovery-vmware-to-vmware.md).

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="enhanced-deployment"></a>Verbeterde distributie

Dit artikel bevat instructies voor een verbeterde implementatie in de klassieke Azure portal bevat. We raden dat u deze versie gebruiken voor alle nieuwe installaties. Als u al hebt geïmplementeerd met behulp van de eerdere versie van de oude raadzaam te migreren naar de nieuwe versie. Lees [meer](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) over migratie.

De uitgebreide implementatie is een belangrijke update. Hier volgt een overzicht van de verbeteringen die we hebben aangebracht:

- **Geen infrastructuur VMs in Azure**: gegevens rechtstreeks naar een Azure opslag repliceert. Ook voor replicatie en failover-is er niet nodig voor het instellen van een infrastructuur VMs (configuratieserver, master doelserver) als we in de oude installatie nodig is.  
- **Centrale installatie**: een enkele installatie biedt eenvoudige installatie en schaalbaarheid voor componenten voor gebouwen.
- **Implementatie beveiligen**: alle gegevensverkeer wordt gecodeerd en replication management communicatie worden verzonden via HTTPS-443.
- **Herstel punten**: ondersteuning voor crash en consistente toepassing herstel voor Windows en Linux-omgevingen verwijst en ondersteunt zowel single VM en multi-VM consistente configuraties.
- **Failover testen**: ondersteuning voor failover-test ononderbroken naar Azure, zonder invloed op productie of replicatie te onderbreken.
- **Niet-geplande failover**: ondersteuning voor niet-geplande failover naar Azure met een verbeterde optie voor het VMs automatisch afgesloten voordat failover wordt uitgevoerd.
- **Failback**: geïntegreerde failback die alleen deltawijzigingen terug naar de website van lokalen repliceert.
- **vSphere 6.0**: beperkte ondersteuning voor VMware Vsphere 6.0-implementaties.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Hoe helpt herstel Site beschermen virtuele machines en fysieke servers?


- VMware-beheerders kunnen Azure off-site bescherming van zakelijke werkbelasting en toepassingen op VMware virtuele machines configureren. Server managers kunnen fysieke op vestigingen Windows en Linux-servers repliceren naar Azure.
- De herstelconsole Azure-Site biedt één enkele locatie voor eenvoudige installatie en beheer van replicatie, failover en processen voor herstel.
- Als u VMware virtuele machines die worden beheerd door een vCenter server repliceert, kan Site herstellen die VMs automatisch ontdekken. Als computers zich in een ESXi host ontdekt Site herstel VMs op de host.
- Eenvoudig failover uitvoeren vanaf uw infrastructuur op gebouwen Azure en failback (terugzetten) van Azure VM VMware-servers op de site van gebouwen.
- Configureer de herstelplannen die de werkbelasting van toepassingen die trapsgewijs zijn geschakeld via meerdere computers te groeperen. Kunt u niet via deze plannen en Recovery Site consistentie multi-VM zodat computers waarop de werklast dezelfde samen naar een consistente gegevens kunnen worden hersteld.


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

### <a name="windows64-bit-only"></a>(Alleen voor 64 bits) voor Windows
- Windows Server 2008 R2 SP1 +
- Windows Server 2012
- Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (alleen 64 bits)
- Red Hat Enterprise Linux 6.7, 7.1, 7.2
- CentOS 6.5 6.6, 6.7, 7.0, 7.1, 7.2
- Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise versie 3 voor de Kernel (UEK3)
- SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Scenario-architectuur

Onderdelen:

- **Een server op locatie management**: de server Recovery Site onderdelen wordt uitgevoerd:
    - **Configuratieserver**: communicatie coördineert en beheert gegevens repliceren en herstel processen.
    - **Process-server**: fungeert als gateway voor replicatie. Deze gegevens worden ontvangen van een beveiligde bron machines optimaliseert met caching, compressie en codering en replicatie verzendt naar Azure opslag. Ook push installatie van mobiliteit service beveiligde computers worden verwerkt en automatische detectie van VMs VMware uitvoert.
    - **Master-doelserver**: replicatiegegevens verwerkt tijdens de failback van Azure.
    U kunt ook een server voor documentbeheer die als een processerver alleen, fungeert om het schalen van uw implementatie implementeren.
- **De mobiliteit service**: dit onderdeel wordt geïnstalleerd op elke machine (VM VMware of fysieke server) die u wilt repliceren naar Azure. Het schrijven van gegevens op de computer worden vastgelegd en ze doorstuurt naar de processerver.
- **Azure**: U hoeft niet te maken van een VMs Azure voor het afhandelen van replicatie en -failover. Beheer van de Site Recovery-service worden verwerkt en de gegevens rechtstreeks naar Azure opslag repliceert. Gerepliceerde Azure VMs worden automatisch omhoog of alleen bij een storing op Azure. Echter als u wilt niet de site in de lokalen van Azure moet u een Azure VM om te fungeren als een processerver instellen.


De afbeelding ziet u hoe deze onderdelen samenwerken.

![architectuur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Afbeelding 1: VMware/fysieke naar Azure** (gemaakt door Henry Robalino)


## <a name="capacity-planning"></a>Capaciteitsplanning

Wanneer u van plan bent capaciteit, hier van wat u nodig hebt om na te denken over:

- **De bronomgeving**, capaciteitsplanning of de vereisten voor VMware infrastructure en bron machine.
- **De server voor documentbeheer**, Planning van de beheerservers in ruimten die onderdelen van de Site herstel uitvoeren.
- **Bandbreedte van het netwerk van de bron naar het doel**-plannen voor de bandbreedte die nodig is voor replicatie tussen bron en Azure

### <a name="source-environment-considerations"></a>Bron milieu-overwegingen

- **Dagelijks maximum snelheid wijzigen**, een beveiligde computer één processerver kan alleen worden gebruikt en tot 2 TB aan gegevens wijzigen per dag kan worden verwerkt door een server met een enkel proces bekijken. 2 TB is dus dat de maximale dagelijkse gegevens wijzigen die wordt ondersteund voor een beveiligde machine.
- **Maximale doorvoer**— een gerepliceerde machine kan deel uitmaken van één rekening voor opslag in Azure. Een account standaardopslag maximaal 20.000 aanvragen per seconde kan verwerken en wordt aangeraden dat u het aantal IOP's via een broncomputer aan 20.000 behouden. Voor het voorbeeld hebt u een broncomputer met 5 schijven en elke schijf 120 IOP's (8K grootte) van de bron genereert vervolgens worden binnen de Azure per schijf IOP's limiet van 500. Het nummer van opslag rekeningen vereist = totale bron IOP's / 20000.


### <a name="management-server-considerations"></a>Overwegingen bij server

De server wordt uitgevoerd Recovery Site onderdelen beheer, replicatie en optimalisatie van gegevens verwerken. Het moet mogelijk zijn voor het verwerken van de dagelijkse capaciteit van wijziging tarief in alle workloads die worden uitgevoerd op beveiligde computers en heeft voldoende bandbreedte doorlopend gegevens worden gerepliceerd naar Azure opslag. Met name:

- Process-server ontvangt van replicatiegegevens van beveiligde computers en optimaliseert met caching, compressie en codering voordat u naar Azure. De server moet voldoende middelen voor het uitvoeren van deze taken hebben.
- De processerver gebruikt de schijfcache. Het is raadzaam een afzonderlijke cache schijf van 600 GB of meer voor het verwerken van wijzigingen in gegevens opgeslagen bij netwerk knelpunt of stroomstoring. Tijdens de implementatie kunt u de cache op een station met ten minste 5 GB opslagruimte beschikbaar maar 600 GB is de minimale aanbeveling.
- Beste is het raadzaam dat de server bevindt zich op de LAN-segment en hetzelfde netwerk als de computers die u wilt beveiligen. Het kan worden gevonden op een ander netwerk maar machines die u wilt beveiligen L3 netwerk zichtbaarheid te moeten hebben.

Aanbevelingen voor de server voor documentbeheer worden in de volgende tabel samengevat.

**Management server CPU** | **Geheugen** | **Cachegrootte voor schijf** | **De snelheid wijzigen gegevens** | **Beveiligde computers**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 cores @ 2.5 GHz) | 16 GB | 300 GB | 500 GB of minder | Een server voor documentbeheer met deze instellingen voor het repliceren van minder dan 100 computers implementeren.
12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) | 18 GB | 600 GB | 500 GB tot 1 TB | Implementatie van een server voor documentbeheer met deze instellingen te repliceren tussen 100-150 machines.
16 vCPUs (2 sockets * 8 cores @ 2.5 GHz) | 32 GB | 1 TB | 1 TB tot 2 TB | Implementatie van een server voor documentbeheer met deze instellingen te repliceren tussen 150-200 machines.
Een ander processerver implementeren | | | > 2 TB | Extra proces-servers implementeren als u bij het repliceren van meer dan 200 machines of als dagelijkse gegevens wijzigen tarief hoger is dan 2 TB.

Waar:

- Elke broncomputer wordt geconfigureerd met 3 schijven van 100 GB.
- Wij benchmarking opslag van 8 SAS harde schijven van 10 K RPM met RAID 10 voor cache schijf metingen gebruikt.

### <a name="network-bandwidth-from-source-to-target"></a>Bandbreedte van het netwerk van bron naar het doel
Zorg ervoor dat u de bandbreedte die nodig is voor de eerste replicatie en delta-replicatie met behulp van de [Capaciteitsplanner](site-recovery-capacity-planner.md) berekenen

#### <a name="throttling-bandwidth-used-for-replication"></a>De bandbreedte voor replicatie regelen

VMware verkeer gerepliceerd naar Azure gaat via de server van een bepaald proces. U kunt de bandbreedte die beschikbaar is voor replicatie van Site terugzetten op die server als volgt beperken:

1. Open de module MMC van Microsoft Azure back-up op de belangrijkste server of op een server voor documentbeheer met extra ingericht proces-servers. Standaard een snelkoppeling voor Microsoft Azure back-up wordt gemaakt op het bureaublad, of kunt u vinden in: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.

    ![De bandbreedte beperken](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Geef op het tabblad **Throttling** de bandbreedte die kan worden gebruikt voor replicatie van Site-herstel en de planning van toepassing.

    ![De bandbreedte beperken](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

U kunt desgewenst ook instellen met PowerShell beperken. Hier volgt een voorbeeld:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Bandbreedtegebruik optimaliseren
Verhogen van de bandbreedte die wordt gebruikt voor replicatie door Azure Site herstellen, moet u een registersleutel wijzigen.

De volgende sleutel bepaalt u het aantal threads per schijf te repliceren die worden gebruikt bij het repliceren

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 In een 'overprovisioned' netwerk moet deze registersleutel te wijzigen van de standaardwaarden. Ondersteuning van maximaal 32.  


[Meer informatie](site-recovery-capacity-planner.md) over capaciteitsplanning gedetailleerde.

### <a name="additional-process-servers"></a>Extra proces-servers

Als u wilt beschermen, meer dan 200 machines of dagtarief wijzigen groter dat 2 TB kunt u toevoegen extra servers voor het verwerken van de belasting. Als u wilt schalen die u kunt doen:

- Verhoog het aantal servers voor het beheer. Zo kunt u maximaal 400 machines met twee management servers beveiligen.
- Proces van extra servers toevoegen en deze gebruiken voor het verwerken van verkeer in plaats van (of naast) de server.

Deze tabel ziet u een scenario waarin:

- U instellen de oorspronkelijke server als een configuratieserver te gebruiken.
- U instellen een extra processerver.
- Configureren van beveiligde virtuele machines voor het gebruik van de processerver extra.
- Elke machine beveiligde bron is geconfigureerd met drie schijven van 100 GB.

**Oorspronkelijke server voor documentbeheer**<br/><br/>(configuratieserver) | **Extra process-server**| **Cachegrootte voor schijf** | **De snelheid wijzigen gegevens** | **Beveiligde computers**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 16 GB geheugen | 4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8 GB geheugen | 300 GB | 250 GB of minder | 85 of minder computers kan worden gerepliceerd.
8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 16 GB geheugen | 8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12 GB geheugen | 600 GB | 250 GB tot 1 TB | U kunt repliceren tussen 85-150 machines.
12 vCPUs (2 sockets * 6 cores @ 2.5 GHz), 18 GB geheugen | 12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24 GB geheugen | 1 TB | 1 TB tot 2 TB | U kunt repliceren tussen 150-225 machines.


De manier waarop u uw servers op schaal wordt is afhankelijk van uw voorkeur voor een schaal van of schaal van model.  U laat een paar geavanceerde management en proces-servers implementeren of geschaald uit door het gebruik van meer servers met minder middelen. Bijvoorbeeld: als u 220 machines beveiligen moet u een van de volgende kan doen:

- De oorspronkelijke server met 12vCPU, 18 GB geheugen, een extra processerver met 12vCPU, 24 GB geheugen, en beveiligde computers alleen de aanvullende CMYK-server configureren.
- Of ook kan twee servers voor het beheer (2 x 8vCPU, 16 GB RAM) en twee extra proces-servers (1 x 8vCPU) en 4vCPU x 1 voor het verwerken van 135 + 85 (220) machines configureren en beveiligde computers zodanig alleen de extra proces-servers configureren.


[Volg deze instructies](#deploy-additional-process-servers) voor het instellen van een extra processerver.

## <a name="before-you-start-deployment"></a>Voordat u de implementatie

De tabellen geven een overzicht van de vereisten voor het implementeren van dit scenario.


### <a name="azure-prerequisites"></a>Azure-vereisten

**Vereiste** | **Details**
--- | ---
**Azure-account**| U moet een account van [Microsoft Azure](https://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**Azure opslag** | U moet een account Azure opslag gerepliceerde gegevens wilt opslaan. Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gesponnen omhoog wanneer failover plaatsvindt. <br/><br/>U moet een [account standaard geo-redundante opslag](../storage/storage-redundancy.md#geo-redundant-storage). De account moet zich in hetzelfde gebied, als de Site-Recovery-service en worden gekoppeld aan het abonnement hetzelfde. Opmerking replicatie naar premium opslag accounts die momenteel niet wordt ondersteund en niet moet worden gebruikt.<br/><br/>We ondersteunen niet het verplaatsen van de opslag-accounts die zijn gemaakt met de [nieuwe Azure portal](../storage/storage-create-storage-account.md) over resourcegroepen. [Meer informatie over](../storage/storage-introduction.md) Azure opslag.<br/><br/>
**Azure netwerk** | Je hebt een Azure virtueel netwerk die Azure VMs wordt verbonden wanneer failover plaatsvindt. De Azure virtueel netwerk moet in hetzelfde gebied, als de Site herstel kluis.<br/><br/>Opmerking dat niet terug na een failover naar Azure u een VPN moet-verbinding (of ExpressRoute Azure) instellen vanaf het netwerk Azure naar de site van gebouwen.


### <a name="on-premises-prerequisites"></a>Vereisten voor gebouwen

**Vereiste** | **Details**
--- | ---
**Server voor documentbeheer** | U moet een op ruimten Windows 2012 R2 server op een virtuele machine of fysieke server. Alle onderdelen op ruimten Recovery-Site zijn geïnstalleerd op deze server beheren<br/><br/> Het is raadzaam om de implementatie van de server als een maximaal beschikbare VMware VM. Failback naar de site in de lokalen van Azure is altijd naar VMs VMware ongeacht of u failover VMs of fysieke servers. Als de server niet is geconfigureerd als een VM VMware moet u een afzonderlijke master doelserver instellen als een VM VMware failback-verkeer wordt ontvangen.<br/><br/>De server mag geen domeincontroller.<br/><br/>De server moet een statisch IP-adres hebben.<br/><br/>De hostnaam van de server moet 15 tekens of minder.<br/><br/>De landinstelling van het besturingssysteem worden alleen Engels.<br/><br/>De server is vereist voor toegang tot het internet.<br/><br/>Moet u uitgaande toegang vanaf de server als volgt: tijdelijke toegang tot HTTP-80 tijdens de installatie van de onderdelen van de Site herstellen (voor het downloaden van MySQL); Doorlopende uitgaande toegang via HTTPS-443 voor het replicatiebeheer van; Doorlopende uitgaande toegang via HTTPS 9443 voor replicatieverkeer (deze poort kan worden gewijzigd)<br/><br/> Zorg ervoor dat deze URL's zijn toegankelijk vanaf de server: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Als u IP-adres gebaseerde firewall-regels op de server hebt, Controleer de regels dat communicatie met Azure. U moet de [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653) en de poort HTTPS (443). U moet ook witte lijst IP-adresbereiken voor de Azure regio van uw abonnement en West VS. De URL [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") is voor het downloaden van MySQL.
**VMware vCenter/ESXi host**: | U moet een of meer vMware vSphere ESX/ESXi hypervisors beheren van uw VMware virtuele machines, versie 6.0, 5.5 of 5.1 ESX/ESXi uitgevoerd met de nieuwste updates.<br/><br/> Wij raden u implementeert een server VMware vCenter ESXi hosts beheren aan. Maar moet vCenter versie 6.0 of 5.5 actief met de nieuwste updates.<br/><br/>Overigens vSphere 6.0 functies zoals cross-vCenter vMotion, virtuele volumes en DRS opslag en herstel van de Site ondersteunt geen nieuwe vCenter. Site herstel ondersteuning is beperkt tot de functies die ook beschikbaar in versie 5.5 zijn.
**Beveiligde computers**: | **AZURE**<br/><br/>Machines die u wilt beveiligen, moeten voldoen met [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements) voor het maken van VMs Azure.<br><br/>Als u wilt verbinding maken met het VMs Azure nadat failover moet u zult de lokale firewall voor extern bureaublad-verbindingen inschakelen.<br/><br/>Afzonderlijke schijfcapaciteit op beveiligde computers, hoeft niet meer dan 1023 GB. Een VM kan maximaal 64 schijven hebt (dus tot 64 TB). Als u overwegen schijven groter dan 1 TB databasereplicatie zoals SQL Server altijd op of Oracle Data Guard.<br/><br/>Minimaal 2 GB vrije ruimte op het installatiestation voor de installatie van onderdelen.<br/><br/>Gedeelde schijf Gast clusters worden niet ondersteund. Als u een geclusterde implementatie databasereplicatie zoals SQL Server altijd op of Oracle Data Guard overwegen.<br/><br/>Unified Extensible Firmware Interface (UEFI) / opstarten met Extensible Firmware Interface(EFI) wordt niet ondersteund.<br/><br/>Namen van de machine moeten tussen 1 en 63 tekens (letters, cijfers en streepjes) bevatten. De naam moet beginnen met een letter of cijfer en eindigen met een letter of cijfer. U kunt de naam van de Azure wijzigen nadat een computer is beveiligd.<br/><br/>**VMs VMware**<br/><br>U moet installeren VMware vSphere PowerCLI 6.0. op de server (configuratieserver).<br/><br/>VMware tools geïnstalleerd en uitgevoerd hebt VMware VMs die u wilt beveiligen.<br/><br/>Als de bron VM NIC-koppeling wordt deze geconverteerd naar een enkele NIC na een failover naar Azure.<br/><br/>Als beveiligde VMs een iSCSI-schijf hebben vervolgens zet Site herstel de beveiligde VM iSCSI-schijf naar een VHD-bestand wanneer de VM Azure overgenomen wordt. Als iSCSI-doel kan worden bereikt door de Azure VM wordt deze verbinding met het iSCSI-doel en Zie in wezen twee schijven – de VHD-schijf op de Azure VM en de brondiskette iSCSI. In dit geval moet u verbinding met het iSCSI-doel dat wordt weergegeven op de mislukte via Azure VM.<br/><br/>[Meer informatie](#vmware-permissions-for-vcenter-access) over de VMware gebruikersmachtigingen die nodig zijn voor het herstel van de Site.<br/><br/> **WINDOWS SERVER-computers (op VMware VM of fysieke server)**<br/><br/>De server moet worden uitgevoerd op een ondersteunde 64-bits besturingssysteem: Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 met op minimaal SP1.<br/><br/>Het besturingssysteem moet worden geïnstalleerd op station C:\ en de OS-schijf moet een standaard Windows-schijf (OS niet moet worden geïnstalleerd op een dynamische schijf voor Windows).<br/><br/>Voor computers met Windows Server 2008 R2 moet u .NET Framework 3.5.1 geïnstalleerd hebben.<br/><br/>U moet een administrator-account opgeven (dit moet een lokale beheerder op de computer Windows zijn) voor de push installatie de mobiliteit Service op Windows-servers. Als de opgegeven account een niet-domein-account moet u uitschakelen gebruiker RAS-beheer op de lokale computer is. [Meer informatie](#install-the-mobility-service-with-push-installation).<br/><br/>Herstel van de site ondersteunt VMs bij RDM schijf.  Tijdens de failback hergebruiken Site herstel van de RDM-schijf als de oorspronkelijke bron VM en RDM schijf beschikbaar is. Als ze niet beschikbaar zijn, tijdens de failback maakt Recovery Site een nieuw VMDK bestand voor elke schijf.<br/><br/>**LINUX-MACHINES**<br/><br/>Moet u een ondersteunde 64-bits besturingssysteem: Red Hat Enterprise Linux 6.7; 6.5 centos, 6.6,6.7; Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3), SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts-bestanden op een beveiligde computers moeten bevatten die de naam van de lokale host worden toegewezen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters. <br/><br/>Als u wilt verbinding maken met een Azure virtual machine waarop Linux na een failover met een Secure Shell client (ssh), ervoor te zorgen dat de service Secure Shell op de beveiligde computer start automatisch op opstarten van het systeem is ingesteld en dat de firewallregels toestaan een ssh verbinding mee.<br/><br/>Beveiliging kan alleen worden ingeschakeld voor Linux machines met de volgende opslag: File system (EXT3, ETX4, ReiserFS, XFS); Multipath-software-apparaat Mapper (MPIO)); Volume manager: (LVM2). Fysieke servers met HP CCISS-controller opslag worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.<br/><br/>Herstel van de site ondersteunt VMs bij RDM schijf.  Tijdens de failback-bewerking voor Linux niet Recovery Site opnieuw de RDM-schijf gebruiken. In plaats daarvan wordt een nieuw VMDK bestand voor elke overeenkomstige RDM schijf gemaakt.

Alleen voor Linux VM - Zorg ervoor dat u de instelling van de disk.enableUUID=true ingesteld in de Parameters voor de configuratie van de VM in VMware. Als deze rij bestaat, toe te voegen. Dit is nodig om te geven met een consistente UUID de VMDK zodat deze correct koppelt. Merk op dat ook zonder deze instelling, failback, zal een volledige download zelfs als de VM is beschikbaar op prem. Deze instelling toe te voegen zorgt u ervoor dat alleen de deltawijzigingen tijdens het failback worden overgebracht.

## <a name="step-1-create-a-vault"></a>Stap 1: Maak een kluis

1. Aanmelden bij de [Portal beheren](https://manage.windowsazure.com/).
2. Vouw **Services Data** > **Recovery Services** en klik op **Site-Recovery kluis**.
3. Klik op **Nieuw** > **snel tabellen maken**.
4. Voer in het vak **naam**een beschrijvende naam ter identificatie van de kluis.
5. Selecteer de regio voor de kluis in de **regio**. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klik op **de kluis maken**.
    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Controleer de statusbalk om te bevestigen dat de kluis is gemaakt. De kluis wordt aangeboden als **actief** op de hoofdpagina **Recovery Services** .

## <a name="step-2-set-up-an-azure-network"></a>Stap 2: Een Azure netwerk instellen

Een Azure netwerk instellen zodat Azure VMs verbonden met een netwerk na een failover en failback naar de site van ruimten kunt werken zoals verwacht.

1. In de Azure portal > **virtueel netwerk maken** de netwerknaam opgeven. IP-adres en het subnetmasker bereik naam.
2. Moet u VPN/ExpressRoute aan het netwerk toevoegen als u hoeft alleen failback. VPN/ExpressRoute kan zelfs na een failover worden toegevoegd aan het netwerk.

[Lees meer](../virtual-network/virtual-networks-overview.md) over Azure netwerken.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die worden gebruikt voor het herstel van de Site te implementeren.

## <a name="step-3-install-the-vmware-components"></a>Stap 3: De VMware-onderdelen installeren

Als u wilt repliceren VMware virtuele installeren machines de volgende onderdelen van de VMware op de server:

1. [Download](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) en installeer VMware vSphere PowerCLI 6.0.
2. Start de server opnieuw.


## <a name="step-4-download-a-vault-registration-key"></a>Stap 4: Een sleutel van de registratie vault downloaden

1. Vanuit het management server de herstelconsole op Site openen in Azure. Klik op de kluis om de pagina Quick Start te openen in de **Recovery Services** pagina. Quick Start kan ook worden geopend via het pictogram.

    ![Quick Start-pictogram](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Klik op de pagina **Quick Start** **Voorbereiden Target Resources** > **downloaden van een registratie-sleutel**. Het registratiebestand wordt automatisch gegenereerd. Het is geldig voor 5 dagen nadat het gegenereerd.


## <a name="step-5-install-the-management-server"></a>Stap 5: De server installeren
> [AZURE.TIP] Zorg ervoor dat deze URL's zijn toegankelijk vanaf de server:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. Download de gemeenschappelijke installatie op de server op de pagina **Quick Start** .

2. Het installatiebestand om setup te starten in de wizard website herstel Unified Setup uitvoeren.

3.  Selecteer in **voordat u** **de configuratieserver and process-server installeren**.

    ![Voordat u begint](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. Klik op **ik ga akkoord** als u wilt downloaden en installeren van MySQL in **Licentie voor Software van derden** . 

    ![= Derde partij software](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. Blader in **registratie** en selecteer de registratie sleutel die u hebt gedownload van de kluis.

    ![Registratie](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. Opgeven hoe de Provider die wordt uitgevoerd op de configuratieserver maakt verbinding met Azure Site herstellen via het internet in **Internet-opties** .

    - Selecteer **verbinding maken met bestaande proxy-instellingen**als u wilt verbinden met de proxy die momenteel op de computer ingesteld.
    - Selecteer **Connect rechtstreeks zonder proxy**als u wilt dat de Provider om rechtstreeks verbinding te maken.
    - Als de bestaande proxy verificatie vereist is, of u wilt een aangepaste proxy gebruiken voor de verbinding van de Provider, selecteert u **verbinding maken met aangepaste proxy-instellingen**.
        - Als u een aangepaste proxy gebruiken moet u het adres, poort en referenties
        - Als u een proxy gebruikt hebt u al mogen de volgende URL's:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Firewall](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. In de **Controle van vereisten** setup wordt uitgevoerd een controle om ervoor te zorgen dat de installatie kunt uitvoeren. 

    
    ![Vereisten](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Controleer of dat de tijd op de klok (**datum en tijd** instellingen) is hetzelfde als de tijdzone als een waarschuwing wordt weergegeven over de **globale tijdsynchronisatie controleren** .

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. Maak in de **MySQL configuratie** referenties voor aanmelding bij de MySQL-server-exemplaar dat wordt geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. In **Omgeving Details** selecteren of u gaat voor het repliceren van VMs VMware. Als u bent, controleert setup of PowerCLI 6.0 is geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. In een **Locatie** selecteren waar u de binaire bestanden installeren en de cache worden opgeslagen. Een cache-station met ten minste 600 GB wordt aangeraden, maar kunt u een station met ten minste 5 GB opslagruimte beschikbaar.

    ![Installatielocatie](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. Geef de listener (netwerkadapter en SSL-poort) waarop de configuratieserver zal verzenden en ontvangen van replicatiegegevens in **Netwerk selecteren** . U kunt de standaardinstelling wijzigen (9443) poort. Naast deze poort, wordt poort 443 gebruikt door een webserver die replicatiebewerkingen orchestrates. 443 niet moet worden gebruikt voor het ontvangen van replicatieverkeer.


    ![Netwerk selecteren](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  **Samenvatting van** de informatie en klik op **installeren**. Een wachtwoordzin is gegenereerd wanneer de installatie is voltooid. U hebt deze nodig wanneer u replicatie inschakelt dus kopiëren en op een veilige plaats bewaren.

    ![Samenvatting](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  Lees de informatie in de **Samenvatting** .

    ![Samenvatting](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure Service herstelagent proxy moet instellen.
>Start een toepassing met de naam 'Microsoft Azure Recovery Services Shell' in het menu Start van Windows nadat de installatie voltooid is. Voer de volgende reeks opdrachten voor het instellen van de proxy-instellingen in het venster dat wordt geopend.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Voer setup uit vanaf de opdrachtregel

U kunt ook de uniforme wizard uitvoeren vanaf de opdrachtregel als volgt:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Waar:

- / ServerMode: verplicht. Hiermee geeft u aan of de installatie de configuratie- en -servers of alleen de processerver moet worden geïnstalleerd (gebruikt voor het proces van extra servers installeren). Waarden invoeren: CS, PS worden opgehaald.
- InstallDrive: verplicht. Hiermee geeft u de map waarin de onderdelen zijn geïnstalleerd.
- / MySQLCredFilePath. Verplicht. Hiermee geeft u het pad naar een bestand waarin de MySQL-server-referenties verhaal zijn. De sjabloon om het bestand te maken krijgen.
- / VaultCredFilePath. Verplicht. Locatie van het bestand kluis referenties
- / EnvType. Verplicht. Type installatie. Waarden: VMware, NonVMware
- / PSIP en /CSIP. Verplicht. IP-adres van het proces en de configuratieserver.
- / PassphraseFilePath. Verplicht. De locatie van het bestand wachtwoordzin.
- / ByPassProxy. Dit is optioneel. Hiermee geeft u dat de server verbindt met Azure zonder proxy.
- / ProxySettingsFilePath. Dit is optioneel. Hiermee geeft u de instellingen voor een aangepaste proxy (standaardproxy op de server die verificatie vereist) of aangepaste proxy




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Stap 6: Instellen van referenties voor de vCenter server

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

De processerver kan VMs VMware die worden beheerd door een vCenter server automatisch detecteren. Voor automatische detectie moet herstel van de Site een account en referenties die toegang heeft tot de vCenter server. Dit is niet relevant als u bent alleen fysieke servers te repliceren.

Dit als volgt:

1. Op de vCenter server een rol (**Azure_Site_Recovery**) maken op het niveau van vCenter met de [vereiste machtigingen beschikt](#vmware-permissions-for-vcenter-access).
2. De **Azure_Site_Recovery** rol toewijst aan een gebruiker vCenter.

    >[AZURE.NOTE] Een vCenter-gebruikersaccount die de rol van alleen-lezen heeft failover kan worden uitgevoerd zonder bron beveiligde computers wordt afgesloten. Als u wilt dat deze computers afsluiten moet u de functie Azure_Site_Recovery. Houd er rekening mee dat als u bent alleen VMs migratie van VMware naar Azure en failback-bewerking moet niet de rol van alleen-lezen voldoende is.

3. Om toe te voegen opent de account **cspsconfigtool**. Het is beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map [locatie] \home\svsystems\bin.
2. Klik op het tabblad **Accounts beheren** op **Account toevoegen**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. Voeg **Details van** referenties die kunnen worden gebruikt voor toegang tot de vCenter server. Houd er rekening mee dat het voor de accountnaam wordt weergegeven in de portal voor meer dan 15 minuten kan duren. Klik op Vernieuwen op het tabblad **Configuratie van Servers** onmiddellijk bijwerken.

    ![Details](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Stap 7: VCenter-servers en hosts ESXi toevoegen

Als u bij het repliceren van VMs VMware moet u een vCenter server (of ESXi host) toe te voegen.

1. Op de **Servers** > **Servers configuratie** tab, selecteer de configuratieserver > **vCenter-server toevoegen**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. De vCenter server of ESXi host details, de naam van de account die u hebt opgegeven voor toegang tot de vCenter server in de vorige stap en de processerver die wordt gebruikt voor het ontdekken van VMs VMware die worden beheerd door de vCenter server toevoegen. Houd er rekening mee dat de vCenter server of ESXi host bevindt zich in hetzelfde netwerk als de server waarop het processerver is geïnstalleerd.

    >[AZURE.NOTE] Als u de vCenter server of ESXi host met een account die geen administrator-bevoegdheden op de host of vCenter server toevoegt, zorg ervoor dat de vCenter of ESXi accounts hebben deze bevoegdheden ingeschakeld: Datacenter, Datastore, map, Jost, netwerk, Resource, virtuele machine, vSphere Distributed Switch. De vCenter server moet ook de bevoegdheid opslag weergaven.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Nadat de detectie is voltooid worden, de vCenter server weergegeven op het tabblad **Configuratie van Servers** .

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Stap 8: Maak een groep voor de bescherming

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Bescherming groepen zijn logische groeperingen van virtuele machines of fysieke servers die u wilt beveiligen met dezelfde beveiligingsinstellingen. U beveiligingsinstellingen toepast op een groep voor de bescherming en de instellingen worden toegepast op alle virtuele machines/fysieke machines die u aan de groep toevoegt.

1. **Beveiligde Items**openen > **Bescherming groep** en klik op een groep bescherming toe te voegen.

    ![Bescherming-groep maken](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. Op de pagina **Instellingen voor gegevensbeveiliging groep opgeven** een naam opgeven voor de groep en selecteer de configuratieserver waarop u wilt maken van de groep in **uit** . **Het doel** is Azure.

    ![Instellingen voor beveiliging](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. Configureer de replicatie-instellingen die worden gebruikt voor alle computers in de groep op de pagina **De replicatie-instellingen opgeven** .

    ![Bescherming groep Replicatie](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Consistentie van meerdere VM**: als u deze op gedeelde toepassingen consistente herstel punten wordt gemaakt op de computers in de groep bescherming. Deze instelling is het meest relevante wanneer alle computers in de groep voor de bescherming van de werkbelasting dezelfde worden uitgevoerd. Alle machines worden op dezelfde plaats worden hersteld. Deze optie is beschikbaar of bent u VMs VMware of fysieke Windows/Linux-servers repliceren.
    - **Vrijgegeven Productieorder drempel**: Hiermee stelt u de vrijgegeven Productieorder. Meldingen worden gegenereerd wanneer de doorlopende bescherming gegevensreplicatie de geconfigureerde drempelwaarde voor de vrijgegeven Productieorder overschrijdt.
    - **Herstel punt bewaren**: Hiermee geeft u het venster bewaren. Beveiligde computers kunnen naar een willekeurig punt in dit venster worden hersteld.
    - **Toepassing consistent momentopname frequentie**: Hiermee geeft u aan hoe vaak herstel punten met consistente toepassing momentopnamen worden gemaakt.

Als u het selectievakje op wordt een groep voor de bescherming gemaakt met de opgegeven naam. In Addition een tweede groep van bescherming is gemaakt met de naam < bescherming, groep, naam, Failback). Deze groep bescherming wordt gebruikt als u niet terug naar de website van gebouwen na een failover naar Azure. Als ze op de pagina **Items beveiligd** zijn gemaakt, kunt u de groepen bescherming controleren.

## <a name="step-9-install-the-mobility-service"></a>Stap 9: De mobiliteit-service installeren

De eerste stap bij het inschakelen van beveiliging voor virtuele machines en fysieke servers is de mobiliteit service installeren. U kunt dit op twee manieren doen:

- Automatisch push en de service te installeren op elke machine van de processerver. Houd er rekening mee dat wanneer u computers aan een groep voor de bescherming toevoegen en ze al zijn een juiste versie van de installatie van het push mobiliteit zich niet voor.
- Automatisch de service installeren via het enterprise push-methode zoals WSUS of System Center Configuration Manager. Zorg ervoor dat u hebt de server instellen voordat u dit kunt doen.
- Handmatig installeren op elke computer die u wilt beveiligen. maken er zeker van dat u hebt ingesteld de server voordat u dit kunt doen.


### <a name="install-the-mobility-service-with-push-installation"></a>De service mobiliteit met push installatie installeren

Wanneer u computers aan een groep voor de bescherming van toevoegen wordt de mobiliteit-service automatisch geduwd en op elke computer geïnstalleerd door de processerver.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Voorbereiden voor automatische push op Windows-computers

Hier is het voorbereiden van Windows machines zodat de mobiliteit service automatisch door de processerver kan worden geïnstalleerd.

1.  Een account maken die door de processerver kan worden gebruikt voor toegang tot de computer. De account moet beheerdersrechten (lokaal of domein). Houd er rekening mee dat deze gegevens alleen worden gebruikt voor push-installatie van de service mobiliteit.

    >[AZURE.NOTE] Als u niet een domeinaccount gebruikt moet u uitschakelen gebruiker RAS-beheer op de lokale computer. U doet dit door in het register van de HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen de DWORD-vermelding LocalAccountTokenFilterPolicy met een waarde van 1 in. Aan het register toevoegen post uit een CLI opdracht openen of met PowerShell invoeren **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  De Windows Firewall van de computer die u wilt beschermen, selecteert u **toestaan een app of onderdeel toegang geven via de Firewall** en schakel **Bestands- en printerdeling** en **Windows Management Instrumentation**. Voor computers die deel uitmaken van een domein kunt u het firewallbeleid configureert met een groepsbeleidsobject.

    ![Firewall-instellingen](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Voeg de account die u hebt gemaakt:

    - Open **cspsconfigtool**. Het is beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map [locatie] \home\svsystems\bin.
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Voeg de account die u hebt gemaakt. Na het toevoegen van de account moet u de referenties opgeven wanneer u een computer aan een groep bescherming toevoegt.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Automatische push op Linux servers voorbereiden

1.  Zorg ervoor dat de Linux-computer die u wilt beveiligen zoals wordt beschreven in de [On-premises vereisten](#on-premises-prerequisites)wordt ondersteund. Controleer of er netwerkverbinding is tussen de computer die u wilt beveiligen en de server voor die processerver wordt uitgevoerd het.

2.  Een account maken die door de processerver kan worden gebruikt voor toegang tot de computer. De account moet een Hoofdgebruiker op de bronserver voor Linux. Houd er rekening mee dat deze gegevens alleen worden gebruikt voor push-installatie van de service mobiliteit.

    - Open **cspsconfigtool**. Het is beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map [locatie] \home\svsystems\bin.
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Voeg de account die u hebt gemaakt. Na het toevoegen van de account moet u de referenties opgeven wanneer u een computer aan een groep bescherming toevoegt.

3.  Controleer dat het bestand/etc/hosts op de bronserver Linux bevat items die de naam van de lokale host worden toegewezen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
4.  Installeer de meest recente openssh, openssh-server, openssl-pakketten op de computer die u wilt beveiligen.
5.  Zorg ervoor dat SSH is ingeschakeld en wordt uitgevoerd op poort 22.
6.  SFTP subsysteem wachtwoord verificatie en in het bestand sshd_config als volgt inschakelen:

    - Meld u aan als root.
    - Zoek de regel die met PasswordAuthentication begint in het bestand /etc/ssh/sshd_config.
    - Verwijder de opmerking van de regel en wijzig de waarde van **niet** op **Ja**.
    - Zoek de regel die met het **subsysteem begint** en verwijder de opmerkingen van de regel.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>De mobiliteit service handmatig installeren

Het installatieprogramma's zijn beschikbaar in C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

Source besturingssysteem | Mobiliteit service-installatiebestand
--- | ---
Windows Server (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Handmatig installeren op een Windows server


1. Downloaden en uitvoeren van de relevante installer.
2. Selecteer in **voordat u begint met ** **mobiliteit service**.

    ![Mobiliteit service](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. Geef het IP-adres van de server en de coderingssleutel is gegenereerd tijdens de installatie van de onderdelen van de server management in de **Configuratiegegevens van de Server** . U kunt de wachtwoordzin ophalen door te voeren: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** op de server voor documentbeheer.

    ![Mobiliteit service](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. Laat de standaardlocatie op **Locatie installeren** en klik op **volgende** om te beginnen met de installatie.
5. In de **Voortgang van de installatie** controleren installatie en start de computer desgevraagd opnieuw op.

Ook kunt u installeren vanaf de opdrachtregel:

UnifiedAgent.exe [/ rol < Agent/MasterTarget >] [/ InstallLocation <Installation Directory>] [/ CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Waar:

- / Rol: verplicht. Hiermee geeft u aan of de service mobiliteit moet worden geïnstalleerd.
- / InstallLocation: verplicht. Geeft aan waar de service installeren.
- / PassphraseFilePath: verplicht. Hiermee geeft u de configuratie van server-wachtwoordzin.
- / LogFilePath: verplicht. Hiermee geeft u de locatie van logboek voor setup

#### <a name="uninstall-mobility-service-manually"></a>Mobiliteit-service handmatig verwijderen

Mobiliteit Service kan worden verwijderd met behulp van het programma toevoegen verwijderen uit het Configuratiescherm of met behulp van de opdrachtregel.

Is de opdracht voor het verwijderen van de mobiliteit Service met behulp van de opdrachtregel

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Het IP-adres van de server wijzigen

Na het uitvoeren van de wizard kunt u het IP-adres van de server als volgt wijzigen:

1. Open het bestand hostconfig.exe (te vinden op het bureaublad).
2. Klik op het tabblad **Algemeen** kunt u het IP-adres van de server.

    >[AZURE.NOTE] U moet alleen het IP-adres van de server wijzigen. Het poortnummer voor de communicatie server management moet 443 en gebruik HTTPS links moet worden ingeschakeld. De wachtwoordzin niet moet worden gewijzigd.

    ![Beheer van IP-adres](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Handmatig installeren op een Linux-server:

1. Kopieer de juiste tar-archief op basis van de bovenstaande tabel met de Linux-machine die u wilt beveiligen.
2. Open een shellprogramma en pak het gecomprimeerde tar-archief naar een lokaal pad door te voeren:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Maak een bestand passphrase.txt in de lokale map waarin u de inhoud van de tar-archief uitgepakt. Doen dit kopiëren de wachtwoordzin C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase op de server voor documentbeheer en opslaan in passphrase.txt door *`echo <passphrase> >passphrase.txt`* in de shell.
4. De mobiliteit service installeren door te voeren *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Geef het interne IP-adres van de server en controleer of poort 443 is geselecteerd.

**U kunt ook installeren vanaf de opdrachtregel**:

1. De wachtwoordzin van C:\Program Files (x86) \InMage Systems\private\connection op de server kopiëren en opslaan als 'passphrase.txt' op de server voor documentbeheer. Vervolgens deze opdrachten uitvoeren. In ons voorbeeld is het beheer van server-IP-adres 104.40.75.37 en de HTTPS-poort 443 moet zijn:

Op een productieserver installeert:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Installeren op de doelserver master:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Stap 10: Beveiliging voor een computer inschakelen

Beveiliging inschakelen als u wilt u een virtuele machines en fysieke servers toevoegen aan een groep voor de bescherming. Voordat u begint, houd rekening met het volgende als u bij het beveiligen van VMware virtuele machines:

- VMware VMs elke 15 minuten worden ontdekt en het kan meer dan 15 minuten duren voordat ze worden weergegeven in de portal-Site herstellen na detectie.
- De wijzigingen op de virtuele machine (zoals de installatie van VMware's) kunnen ook worden bijgewerkt bij het herstellen van de Site meer dan 15 minuten duren.
- De laatste keer ontdekt kunt u controleren voor VMs VMware in het veld **Laatste Contact op** voor de vCenter server/ESXi host op het tabblad **Configuratie van Servers** .
- Als u een groep bescherming is al gemaakt en een vCenter Server of ESXi host toevoegen, het duurt langer dan 15 minuten voor de portal Azure Site herstel te vernieuwen en voor virtuele machines die worden weergegeven in het dialoogvenster voor het **toevoegen van computers aan een groep voor de bescherming** .
- Als u gaan direct wilt met machines toe te voegen aan de groep bescherming zonder te wachten op de geplande discovery, markeer de configuratieserver (Klik niet op deze) en klik op de knop **vernieuwen** .

Houd ook rekening met:

- Wij raden u aan uw groepen bescherming te bouwen, zodat ze de werklast. Bijvoorbeeld computers met een bepaalde toepassing in dezelfde groep toevoegen.
- Als u computers aan een groep voor de bescherming toevoegen, wordt de processerver automatisch duwt en de mobiliteit service wordt geïnstalleerd als dit niet is gebeurd. Houd er rekening mee dat u over de push-mechanisme bereiden beschikken moet zoals beschreven in de vorige stap.


Computers toevoegen aan een groep voor de bescherming:

1. Klik op **Items beveiligd** > **Bescherming groep** > **Machines** > Machines toevoegen. Beste \As
2. In **virtuele Machines selecteren** als u VMware virtuele machines beveiligen bent, selecteer een vCenter server die u beheert uw virtuele machines (of de EXSi-host waarop ze werken) en selecteert u de machines.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  In **virtuele Machines selecteren** als u de fysieke servers beschermen bent, in de wizard **Toevoegen fysieke Machines** bieden het IP-adres en de beschrijvende naam. Selecteer vervolgens de besturingssysteem-familie.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. Selecteer de opslag account u gebruikt voor replicatie en geef aan of de instellingen voor alle werklast moeten worden gebruikt in **Target Resources opgeven** . Houd er rekening mee dat op dit moment premium opslag accounts worden niet ondersteund.

    >[AZURE.NOTE] 1. Wij ondersteunen niet het verplaatsen van de opslag-accounts die zijn gemaakt met de [nieuwe Azure portal](../storage/storage-create-storage-account.md) over resourcegroepen.                           2.[de migratie van accounts voor opslag](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag gebruikt voor het herstel van de Site te implementeren.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. Selecteer de account in **Accounts opgeven** [geconfigureerd](#install-the-mobility-service-with-push-installation) voor automatische installatie van de service voor mobiliteit.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Klik op het vinkje aan computers toe te voegen aan de groep voor de bescherming en het starten van de eerste replicatie voor elke computer.

    >[AZURE.NOTE] Als push installatie is de service wordt automatisch geïnstalleerd op computers die niet laten zoals ze zijn toegevoegd aan de groep bescherming mobiliteit voorbereid. Nadat de service is de installatie een bescherming wordt gestart en mislukt. Na de storing moet u handmatig opnieuw op elke computer waarop de mobiliteit service is geïnstalleerd. Na het opnieuw opstarten de taak bescherming opnieuw begint en de initiële replicatie plaatsvindt.

U kunt controleren status op de pagina **taken** .

![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Bovendien bescherming kan worden gecontroleerd in **Beveiligde Items** > <protection group name> > **virtuele Machines**. Nadat de eerste replicatie is voltooid en worden de gegevens gesynchroniseerd, verandert status van de computer** beschermd**.

![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>Stap 11: Set beveiligde eigenschappen machine

1. Nadat u een machine heeft een **beveiligde** status kunt u de failover-eigenschappen. Selecteer de machine in details over de bescherming en open het tabblad **configureren** .
2. Herstel van de site automatisch eigenschappen voorgesteld voor Azure VM en detecteert dat de gebouwen op het netwerk instellingen.

    ![Eigenschappen van de virtuele machine instellen](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. U kunt deze instellingen wijzigen:

    -  **Naam Azure VM**: dit is de naam die wordt gegeven aan de machine in Azure na een failover. De naam moet voldoen aan eisen van Azure.

    -  **Azure VM-grootte**: het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele machine van doel opgeeft. [Lees meer](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) over formaten en adapters. Houd rekening met:

        - Wanneer u de grootte van een virtuele machine wijzigen en de instellingen opslaat, wordt het nummer van de netwerkadapter wijzigen als u het tabblad **configureren** volgende keer opent. Het aantal netwerkadapters van target virtuele machines is een minimum van het aantal netwerkadapters op bron virtuele machine en het maximum aantal netwerkadapters die worden ondersteund door de grootte van de virtuele machine gekozen.
            - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters die zijn toegestaan voor de grootte van de doel machine is, vervolgens heeft het doel hetzelfde aantal adapters als de bron.
            - Als het aantal adapters voor de virtuele machine van bron het aantal dat is toegestaan overschrijdt voor de doelgrootte en het doel maximaal worden gebruikt.
            - Als bijvoorbeeld een broncomputer twee netwerkadapters heeft en de grootte van target machine ondersteunt vier, moet de doelcomputer twee adapters. Als de broncomputer twee adapters heeft maar de ondersteunde doelgrootte slechts één ondersteunt hebben de doelcomputer slechts één adapter.
        - Als de virtuele machine is verbonden meerdere netwerkadapters moeten voor alle adapters op hetzelfde netwerk als Azure.
    - **Azure netwerk**: moet u een Azure netwerk die Azure VMs met na een failover verbonden. Als u niet opgeeft wordt niet de VMs Azure aangesloten op een netwerk. Bovendien moet u een Azure netwerk opgeven als u failback van Azure naar de site van gebouwen. Failback-bewerking moet een VPN-verbinding tussen een Azure-netwerk en een netwerk op gebouwen.
    - **Azure-IP-adres/subnet**: voor elke netwerkadapter die u selecteert het subnet waaraan de Azure VM verbinding moet maken. Houd rekening met:
        - Als de netwerkadapter van de broncomputer is geconfigureerd voor het gebruik van een statisch IP-adres kunt u een statisch IP-adres opgeven voor Azure VM. Als u een statisch IP-adres niet opgeeft worden, een beschikbaar IP-adres toegewezen. Als het doel-IP-adres is opgegeven, maar al in gebruik door een andere VM in Azure is mislukt failover. Als de netwerkadapter van de broncomputer is geconfigureerd voor gebruik van DHCP zult u DHCP hebben als de instelling voor Azure.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Stap 12: Maak een herstelplan en een failover uitvoeren

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

U kunt een failover voor een enkele computer uitvoeren of meerdere virtuele machines die dezelfde taak uitvoeren of het uitvoeren van de dezelfde werkbelasting failover. Failover meerdere computers op het moment dat u ze aan een herstelplan toevoegen.

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Op de pagina **Herstel plannen** op **Herstelplan toevoegen** en een herstelplan toevoegen. Geef de details voor het plan en selecteer het doel **Azure** .

    ![Herstelplan configureren](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. Selecteer een groep voor de bescherming en machines selecteert in de groep toe te voegen aan het herstelplan in **virtuele Machine selecteren** .

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

U kunt het plan voor het maken van groepen en reeks de volgorde waarin computers in het herstelplan worden overgenomen. U kunt ook scripts en aanwijzingen voor handmatige acties toevoegen. Scripts kunnen worden gemaakt, handmatig of via [Azure automatisering Runbooks](site-recovery-runbook-automation.md). [Meer informatie](site-recovery-create-recovery-plans.md) over het aanpassen van de herstelplannen.

## <a name="run-a-failover"></a>Een failover uitvoeren

Merk op dat voordat u een failover uitvoert:


- Zorg ervoor dat de server wordt uitgevoerd en beschikbaar - anders failover mislukken.
- Als u een niet-geplande failover opmerking dat:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoeren. Dit zorgt ervoor dat u hoeft niet de bron- en replica-computers op hetzelfde moment. Als u bij het repliceren van VMs VMware kunt vervolgens tijdens het uitvoeren van een niet-geplande failover u sites hersteld moet Qualitative afsluiten van de bron-machines. Afhankelijk van de status van de primaire site kan of niet kan werken. Als u bij het repliceren van fysieke servers biedt niet herstel van de Site deze optie.
    - Wanneer u een niet-geplande failover uitvoeren stopt gegevensreplicatie uit primaire machines zodat alle gegevens delta won't worden overgedragen wanneer een niet-geplande failover.

- Als u verbinding maken met de replica virtuele machine in Azure nadat failover wilt, inschakelen verbinding met extern bureaublad op de broncomputer voordat u de failover uitvoeren en RDP verbindingen toestaan via de firewall. U moet ook RDP toestaan op het openbare eindpunt van Azure virtual machine na een failover. Volg deze [richtlijnen](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) om ervoor te zorgen dat de RDP werken nadat failover is uitgevoerd.

>[AZURE.NOTE] Als u de beste prestaties wanneer u een failover naar Azure uitvoeren, ervoor te zorgen dat de Agent Azure in de beveiligde computer is geïnstalleerd. Dit helpt in sneller opstarten en helpt ook bij het stellen van een diagnose bij problemen. Linux-agent kan worden gevonden [hier](https://github.com/Azure/WALinuxAgent) - en Windows agent vindt u [hier](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Een failover test uitvoeren

Uitvoeren van een test failover om het failover- en herstel processen in een geïsoleerde netwerk dat heeft geen invloed op uw productieomgeving te simuleren en normale replicatie kan worden voortgezet als normaal. Failover van de test wordt gestart op de bron en kan in een aantal manieren worden uitgevoerd:

- **Geeft u een netwerk Azure**: als u een failover testen zonder de test eenvoudig controleren wordt of virtuele machines starten en goed worden weergegeven in Azure netwerk uitvoeren. Virtuele machines wordt niet aangesloten op een netwerk Azure na een failover.
- **Geef een Azure netwerk**: dit type van failover controleert de volledige replicatieomgeving afkomstig van verwacht en dat Azure virtuele machines zijn verbonden met het opgegeven netwerk.


1. De pagina **Herstel plannen** het plan selecteren en klik op **Failover testen**.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. Selecteer **geen** om aan te geven niet aan een Azure netwerk gebruiken voor de test failover of Selecteer het netwerk waarmee de test VMs zijn na een failover verbonden in **Failover testen-bevestigen** . Klik op het vinkje om te beginnen de failover-functie.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. Failover-voortgang op het tabblad **taken** .

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Na voltooiing van de overname ook moet u kunnen zien van de replica Azure machine weergegeven in Azure portal > **virtuele Machines**. Als u wilt initiëren van een RDP-verbinding voor Azure VM moet u poort 3389 op het eindpunt van de VM te openen.

5. Nadat u hebt voltooid wanneer failover bereikt de Complete testfase, klikt u op volledige Test te voltooien. In notities vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de overname van de test.

6. Klik op **de test failover is uitgevoerd** voor het automatisch opschonen van de testomgeving. Nadat dit is gebeurd wordt failover test de status **voltooid** weergegeven. Alle elementen of VMs automatisch gemaakt tijdens de overname van de test worden verwijderd. Houd er rekening mee dat als een failover test langer dan twee weken blijft deze voltooid door force.



### <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

Niet-geplande failover wordt geïnitieerd vanuit Azure en kan worden uitgevoerd, zelfs als de primaire site is niet beschikbaar.


1. Selecteer het plan in de pagina **Herstel plannen** en klik op **Failover** > **Niet-geplande Failover**.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Als u bij het repliceren van VMware virtuele machines kunt u selecteren om te proberen en afgesloten voor bedrijfsruimten VMs. Dit is best effort en failover of de poging of niet slaagt wordt voortgezet. Als het niet lukt foutgegevens weergegeven op het tabblad **taken **> **Ongeplande failover-taken**.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Deze optie is niet beschikbaar als u bij het repliceren van fysieke servers. U moet proberen en afgesloten die handmatig indien mogelijk.

3. Controleer of de failover-richting (naar Azure) in **Failover bevestigen** en selecteer het herstelpunt dat u wilt gebruiken voor de failover. Als u meerdere VM hebt ingeschakeld tijdens het configureren van replicatie-eigenschappen kunt u naar de meest recente toepassing of crash consistent herstelpunt herstellen. U kunt ook een **aangepast herstel punt** terugzetten op een eerder tijdstip selecteren. Klik op het vinkje om te beginnen de failover-functie.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Wacht tot de niet-geplande failover taak te voltooien. U kunt de failover uitgevoerd op het tabblad **taken** controleren. Houd er rekening mee dat zelfs als er fouten optreden tijdens een niet-geplande overname het herstelplan wordt uitgevoerd totdat deze voltooid is. U moet ook de replica Azure Zie machine in virtuele Machines worden weergegeven in de portal Azure.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Verbinding maken met Azure virtuele machines gerepliceerd na een failover

De verbinding met virtuele machines in Azure te gerepliceerd nadat failover hier is wat u nodig hebt:

1. Verbinding met extern bureaublad moet zijn ingeschakeld op de primaire computer.
2. De Windows Firewall op de primaire computer laat RDP.
3. Na een failover moet u RDP toevoegen aan het openbare eindpunt voor Azure VM.

[Lees meer](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) over het instellen van dit.


## <a name="deploy-additional-process-servers"></a>Extra proces-servers implementeren

Als u wilt schalen uit de implementatie van meer dan 200 machines van de bron of de snelheid van de totale dagelijkse lospeuteren van meer dan 2 TB, moet u extra proces-servers voor het verwerken van het netwerkverkeer. Voor het instellen van een extra processerver controleert de vereisten in het [proces van extra servers](#additional-process-servers) en volg de instructies voor het instellen van de processerver. Nadat u de server hebt ingesteld, kunt u machines voor het gebruik van deze bron configureren.

### <a name="set-up-an-additional-process-server"></a>Een extra processerver instellen

U instellen een extra processerver als volgt:

- De uniforme wizard een management-server configureren als een processerver uitvoeren.
- Als u wilt repliceren met behulp van het nieuwe processerver beheren moet u uw beveiligde machines hiervoor migreren.

### <a name="install-the-process-server"></a>De processerver installeren

1. Download de gemeenschappelijke installatie voor de installatie van het onderdeel Site terugzetten op de pagina Quick Start. Voer setup uit.
2. Selecteer in **voordat u begint met het** **proces van extra servers toevoegen voor het schalen van de implementatie**.

    ![Process-server toevoegen](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Voltooi de wizard in de dezelfde manier als wanneer u [instellen van](#step-5-install-the-management-server) de eerste server.

4. Geef het IP-adres van de oorspronkelijke server waarop u de server configureren en de wachtwoordzin geïnstalleerd in de **Server-configuratiegegevens** . Op de oorspronkelijke server uitvoeren ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** de wachtwoordzin te verkrijgen.

    ![Process-server toevoegen](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Machines voor het gebruik van het nieuwe processerver migreren

1. **Configuratie van Servers**Open > **Server** > naam van de oorspronkelijke server > **Server-gegevens**.

    ![Process-server bijwerken](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. Klik in de lijst met **Servers proces** **Proces-Server wijzigen** naast de server die u wilt wijzigen.

    ![Process-server bijwerken](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. In het **Proces-Server wijzigen** > **Proces doelserver** de nieuwe server selecteren en selecteer vervolgens de virtuele machines die het nieuwe processerver worden verwerkt. Klik op het pictogram voor informatie voor meer informatie over de server. De gemiddelde ruimte die nodig is voor het repliceren van elke geselecteerde virtuele machine naar het nieuwe processerver wordt weergegeven zodat u de werklast van beslissingen. Klik op het vinkje om te repliceren naar de nieuwe processerver start.

    ![Process-server bijwerken](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>Machtigingen voor toegang tot vCenter VMware

De processerver kan VMs automatisch op een vCenter server vinden. Voor het uitvoeren van automatische detectie moet u een functie (Azure_Site_Recovery) op het niveau van vCenter tot herstel van de Site toegang te krijgen tot de vCenter server definieert. Merk op dat als u alleen hoeft te VMware machines migreren naar Azure en geen failback van Azure wilt, kunt u de rol van een alleen-lezen dat groot genoeg is. U de machtigingen hebt ingesteld, zoals wordt beschreven in [stap 6: instellen van referenties voor de vCenter server](#step-6-set-up-credentials-for-the-vcenter-server) de machtigingen in de volgende tabel worden samengevat.

**Rol** | **Details** | **Machtigingen**
--- | --- | ---
Azure_Site_Recovery rol | VM VMware detectie |Deze bevoegdheden voor de server v Center toewijzen:<br/><br/>Datastore -> ruimte toewijzen, bladeren datastore, lage niveau bestand bewerkingen., bestand verwijderen, virtuele machine updatebestanden<br/><br/>Netwerk -> netwerk toewijzen<br/><br/>Resource -> virtuele machine toewijzen aan resourcegroep migreren uitgeschakeld virtuele machine, migreren is ingeschakeld op de virtuele machine<br/><br/>Taken maken, taak, taak -><br/><br/>Virtuele machine configuratie -><br/><br/>Virtuele machine beheren -> -> vraag beantwoorden, apparaat verbinding., configureren van CD's, floppy's configureren, uit kracht van, VMware tools installeren<br/><br/>Virtuele machine voorraad -> -> maken, journaal en registratie<br/><br/>Virtuele machine Provisioning -> -> toestaan virtuele machine downloaden, uploaden van bestanden van de virtuele machine toestaan<br/><br/>VM-Snapshots > -> momentopnamen verwijderen
de rol van de gebruiker vCenter | VM VMware detectie/Failover zonder het afsluiten van de bron VM | Deze bevoegdheden voor de server v Center toewijzen:<br/><br/>Data Center-object –> doorgeven aan onderliggende Object, rol = alleen lezen <br/><br/>De gebruiker is toegewezen op het niveau van datacenter en dus toegang heeft tot alle objecten in het datacenter.  Als u wilt om de toegang te beperken, kunt u de rol **geen toegang** met het object **doorgeven aan onderliggende** toewijzen aan de onderliggende objecten (ESX hosts, datastores, VMs en netwerken).
de rol van de gebruiker vCenter | Failover en failback | Deze bevoegdheden voor de server v Center toewijzen:<br/><br/>Datacenter-object doorgeven aan onderliggende object, rol = Azure_Site_Recovery<br/><br/>De gebruiker is toegewezen op het niveau van datacenter en dus toegang heeft tot alle objecten in het datacenter.  Als u wilt om de toegang te beperken, kunt u de rol **geen toegang **met het **doorgeven aan een onderliggend object** toewijzen aan het onderliggende object (ESX hosts, datastores, VMs en netwerken).  



## <a name="third-party-software-notices-and-information"></a>Software van derden mededelingen en bekendmakingen

Niet vertalen of lokaliseren

De software en firmware in het Microsoft-product of de service wordt uitgevoerd op basis van of uitgerust met materiaal uit de onderstaande projecten (gezamenlijk "derden Code").  Microsoft is niet de oorspronkelijke auteur van de Code van derden.  De oorspronkelijke auteursrechtaanduiding en licentie, waaronder Microsoft dergelijke Code derden ontvangen ingesteld hierna anders wordt bepaald.

De informatie in de sectie A is met betrekking tot onderdelen van derden Code uit de onderstaande projecten. Deze licenties en informatie dienen alleen ter informatie.  Deze Code van derden, is aan u door Microsoft onder de licentievoorwaarden voor Microsoft software voor het Microsoft-product of de service wordt relicensed.  

De informatie in de sectie B wordt met betrekking tot onderdelen van derden Code die beschikbaar worden gesteld aan u door Microsoft onder de oorspronkelijke licentievoorwaarden.

Het volledige dossier kan worden gevonden op het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behoudt zich alle rechten voor die niet uitdrukkelijk in deze overeenkomst worden verleend door implicatie, ' estoppel ' of anderszins.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de failback-bewerking](site-recovery-failback-azure-to-vmware-classic.md) om de mislukte via computers in Azure terug naar uw omgeving op gebouwen.
