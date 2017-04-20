<properties
    pageTitle="VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstellen in de Azure portal | Microsoft Azure"
    description="Hierin wordt beschreven hoe u Azure Site herstellen om replicatie, failover en terugvordering van in lokalen VMware virtuele machines en fysieke servers via de portal Azure Azure Windows/Linux evoluerende implementeren"
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
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>VMware virtuele machines en fysieke machines repliceren naar Azure met Azure Site herstellen met behulp van de portal Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Azure klassiek](site-recovery-vmware-to-azure-classic.md)
- [Azure klassieke (verouderd)](site-recovery-vmware-to-azure-classic-legacy.md)

Welkom bij herstel Azure Site! Gebruik dit artikel als u wilt repliceren op ruimten VMware virtuele machines of fysieke servers Windows/Linux naar Azure met Azure Site herstellen in de portal Azure.

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure Resource Manager (ARM) en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen.

Herstel van de site in de portal Azure biedt een aantal nieuwe functies:

- De Azure back-up en herstel van Azure Site services worden gecombineerd tot een enkel Recovery Services kluis zodat u kunt instellen en bedrijfscontinuïteit en noodherstel (BCDR) vanaf één locatie beheren. In het centrale dashboard kunt u controleren en beheren van bewerkingen in uw sites op gebouwen en de openbare Azure cloud.
- Gebruikers met Azure abonnementen met Cloud Solution Provider (CSP) wordt ingericht kunnen nu terugvorderingen Site in Azure portal beheren.
- Herstel van de site in de portal Azure kunt machines repliceren naar ARM opslag rekeningen. Failover wordt uitgevoerd maakt u Site herstellen ARM gebaseerde VMs in Azure.
- Herstel van de site nog steeds ondersteuning voor replicatie naar klassieke opslag rekeningen. Failover wordt uitgevoerd maakt u Site herstellen met het klassieke objectmodel VMs.

Na het lezen van dit artikel post opmerkingen onderaan in de Disqus opmerkingen. Technische vragen op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Overzicht

Organisaties hebben behoefte aan een strategie voor BCDR die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen. Uw strategie voor BCDR moet u bedrijfsgegevens veilig en terug te vorderen en dat werklasten continu beschikbaar blijven als ramp plaatsvindt.

Herstel van de site is een Azure service die deel uitmaakt van uw strategie voor BCDR door replicatie van fysieke servers op gebouwen en virtuele machines naar de cloud (Azure) of een secundaire datacenter slaagde. Wanneer er storingen optreden in de primaire locatie, failover u uitvoeren naar de secundaire locatie apps en werkbelasting beschikbaar te houden. U niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen. Meer informatie in [Wat is Azure Site terugzetten?](site-recovery-overview.md)

Dit artikel bevat alle informatie die u wilt repliceren op-lokalen VMs VMware en fysieke servers Windows/Linux met Azure. Het bevat een architectonisch overzicht, informatie en implementatiestappen voor het configureren van Azure op gebouwen-servers, replicatie-instellingen en capaciteitsplanning plannen. Nadat u hebt ingesteld met de infrastructuur kunt u replicatie op computers die u wilt beveiligen en controleren dat failover werkt.

## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Herstel van de site biedt off-site bescherming business werkbelasting en toepassingen die worden uitgevoerd op VMs VMware en fysieke servers.
- De portal Recovery Services biedt één locatie wilt instellen, beheren en controleren van replicatie, failover en herstel.
- Herstel van de site kan automatisch detecteren VMs van VMware vSphere hosts worden toegevoegd.
- In uw site op ruimten kunt u gemakkelijk failovers van uw infrastructuur op ruimten Azure en failback (terugzetten) van Azure VM VMware-servers uitvoeren.
- U kunt meerdere VM en replicatiegroepen maken zodat de werkbelasting van toepassingen op meerdere computers repliceren op hetzelfde moment doorverbonden. De machines in een replicatiegroep hebt crash consistent en app consistent herstel punten wanneer ze een failover. Voor failover, kunt u meerdere computers in de herstelplannen verzamelen zodat gelaagde werkbelasting van toepassingen aan elkaar failover.

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

Dit zijn de onderdelen:

- **Configuratieserver**: een machine op de ruimten die communicatie coördineert en beheert gegevens repliceren en herstel processen. U zult enkele setup-bestand om de configuratieserver en deze Aanvullende componenten te installeren uitvoeren op deze computer:
    - **Process-server**: fungeert als gateway voor replicatie. Deze replicatiegegevens worden ontvangen van beveiligde bron machines optimaliseert met caching, compressie en codering en verzendt deze naar Azure opslag. Ook push installatie van de service mobiliteit voor beveiligde computers worden verwerkt en voert de automatische detectie van VMs VMware. De standaard process-server is geïnstalleerd op de server configureren. U kunt implementeren meer zelfstandig proces-servers wilt schalen van uw implementatie.
    - **Master-doelserver**: replicatiegegevens verwerkt tijdens de failback van Azure.

- **Mobiliteit service**: dit onderdeel wordt geïnstalleerd op elke computer die u wilt repliceren naar Azure (VM VMware of fysieke server). Het schrijven van gegevens op de computer worden vastgelegd en ze doorstuurt naar de processerver.
- **Azure**: U hoeft niet te maken van een VMs Azure voor het afhandelen van replicatie en -failover naar Azure.  U hoeft een Azure-abonnement, een Azure opslag account voor het opslaan van de gerepliceerde gegevens en een Azure virtueel netwerk zodat Azure VMs na een failover worden verbonden met een netwerk. De opslag en het netwerkbeleid moeten zich in hetzelfde gebied, als de kluis Recovery Services.
- **Failback**: wanneer u aan uw site in de lokalen van Azure mislukken nadat failover is uitgevoerd, moet u voor het maken van een Azure VM als een van tijdelijke processerver. Nadat de failback-bewerking is voltooid, kunt u deze verwijderen. Voor failback moet u ook een VPN-(of ExpressRoute Azure) verbinding tussen uw site op gebouwen en de Azure netwerk waarin uw Azure VMs bevinden. Als failback wordt moet u mogelijk ook voor het instellen van een speciaal model doelserver machine op gebouwen. Voor lichtere verkeer kan de standaard master doelserver uitgevoerd op de configuratieserver worden gebruikt.


De afbeelding ziet u hoe deze onderdelen samenwerken.

![architectuur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Figuur 1: VMware/fysieke naar Azure**

## <a name="azure-prerequisites"></a>Azure-vereisten

Hier is wat u nodig hebt in Azure implementeren van dit scenario.

**Vereiste** | **Details**
--- | ---
**Azure-account**| U moet een account van [Microsoft Azure](http://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**Azure opslag** | Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gemaakt wanneer failover plaatsvindt. <br/><br/>Opslaan van gegevens moet u een opslag standard of premium-account in dezelfde regio als de kluis Recovery Services.<br/><br/>U kunt een LRS of GRS opslag-account gebruiken. Het is raadzaam GRS zodat gegevens veerkrachtig als een regionale is uitgevallen of de primaire regio kan niet worden hersteld. [Meer informatie](../storage/storage-redundancy.md).<br/><br/> [Premium-opslagruimte](../storage/storage-premium-storage.md) wordt meestal gebruikt voor virtuele machines die een consistent hoge i/o-prestaties en lage latentie voor intensieve i/o-belasting host nodig.<br/><br/> Als u een premium account gebruik gerepliceerde gegevens wilt opslaan wilt, moet u ook een account standaardopslag voor het opslaan van Logboeken van bestandsreplicatie die voortdurende wijzigingen in de gegevens voor de lokalen vastleggen.<br/><br/> Houd er rekening mee dat opslag accounts die zijn gemaakt in de Azure portal over resourcegroepen kunnen niet worden verplaatst. Ook wordt de bescherming van de rekeningen van de premie opslag in Centraal India en Zuid-India niet ondersteund.<br/><br/> [Meer informatie over](../storage/storage-introduction.md) Azure opslag.
**Azure netwerk** | Je hebt een Azure virtueel netwerk die Azure VMs wordt verbonden wanneer failover plaatsvindt. De Azure virtueel netwerk moet zich in hetzelfde gebied, als de kluis Recovery Services.
**Failback van Azure** | Je hebt een tijdelijk processerver instellen als een Azure VM. Wanneer u nu failback en na fouten weer te verwijderen, kunt u deze maken.<br/><br/> Doen mislukken moet weer u een VPN-verbinding (of Azure ExpressRoute) van het netwerk Azure naar de site van gebouwen.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Configuratieserver / schaal van proces-vereisten

U zult een machine op locatie als de configuratieserver instellen / scale-out processerver.

**Vereiste** | **Details**
--- | ---
**Configuratieserver**| U moet een op ruimten fysieke of virtuele machine met Windows Server 2012 R2. Alle onderdelen op ruimten Recovery-Site zijn geïnstalleerd op deze computer.<br/><br/>VMware VM replicatie, wordt u aangeraden implementatie van de server als een maximaal beschikbare VMware VM. Als u bij het repliceren van fysieke machines kan de machine een fysieke server zijn.<br/><br/> Failback naar de site in de lokalen van Azure is altijd VMs VMware ongeacht of u failover VMs of fysieke servers. Als u de server configureren als een VM VMware niet implementeren moet u een afzonderlijke master doelserver instellen als een VM VMware failback-verkeer wordt ontvangen.<br/><br/>Als de server een VM VMware, moet het type netwerkadapter VMXNET3. Als u een ander type netwerkadapter gebruikt moet u een [VMware bijwerken](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) op de server vSphere 5.5 te installeren.<br/><br/>De server moet een statisch IP-adres hebben.<br/><br/>De server mag geen domeincontroller.<br/><br/>De hostnaam van de server moet 15 tekens of minder.<br/><br/>Het besturingssysteem moet alleen Engels zijn.<br/><br/> U moet installeren VMware vSphere PowerCLI 6.0. op de server configureren.<br/><br/>De configuratieserver moet toegang tot het internet. Uitgaande toegang is vereist:<br/><br/>Tijdelijke toegang tot HTTP-80 tijdens de installatie van de onderdelen van het herstel van de Site (voor het downloaden van MySQL)<br/><br/>Doorlopende uitgaande toegang via HTTPS-443 voor het replicatiebeheer van<br/><br/>Doorlopende uitgaande toegang via HTTPS 9443 voor replicatieverkeer (deze poort kan worden gewijzigd)<br/><br/>De server moet ook de toegang tot de volgende URL's zodat deze verbinding met Azure maken kan: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net<br/><br/>Als u IP-adres gebaseerde firewall-regels op de server hebt, Controleer de regels dat communicatie met Azure. U moet de [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en het protocol HTTPS (443).<br/><br/>IP-adresbereiken voor de Azure regio van uw abonnement en West VS toestaan.<br/><br/>Toestaan dat deze URL voor de MySQL-download:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>Vereisten voor VMware vCenter/vSphere host

**Vereiste** | **Details**
--- | ---
**vSphere**| U moet een of meer VMware vSphere hypervisors.<br/><br/>VSphere versie 6.0, 5.5 of 5.1 moet hypervisors uitgevoerd met de nieuwste updates.<br/><br/>Wij raden aan dat de hosts van vSphere en vCenter servers bevinden zich in hetzelfde netwerk als de processerver (dit is het netwerk waarin de configuratieserver zich bevindt als u een specifiek processerver hebt ingesteld).
**vCenter** | Wij raden aan dat u implementeert een server VMware vCenter vSphere hosts beheren. Maar moet vCenter versie 6.0 of 5.5 actief met de nieuwste updates.<br/><br/>Overigens vSphere 6.0 functies zoals cross-vCenter vMotion, virtuele volumes en DRS opslag en herstel van de Site ondersteunt geen nieuwe vCenter. Site herstel ondersteuning is beperkt tot de functies die ook beschikbaar in versie 5.5 zijn.


## <a name="protected-machine-prerequisites"></a>Beveiligde computer vereisten

**Vereiste** | **Details**
--- | ---
**On-premises (VMs VMware)** | VMware tools geïnstalleerd en uitgevoerd hebt VMware VMs die u wilt beveiligen.<br/><br/> Machines die u wilt beveiligen, moeten voldoen met [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements) voor het maken van VMs Azure.<br/><br/>Afzonderlijke schijfcapaciteit op beveiligde computers, hoeft niet meer dan 1023 GB. Een VM kan maximaal 64 schijven hebt (dus tot 64 TB). <br/><br/>Minimaal 2 GB vrije ruimte op het installatiestation voor de installatie van onderdelen.<br/><br/>Bescherming van VMs met versleutelde schijven wordt niet ondersteund.<br/><br/>Gedeelde schijf Gast clusters worden niet ondersteund.<br/><br/>**Poort 20004** moet worden geopend in de beveiligde virtuele machine van lokale firewall, als u wilt schakelen **toepassing consistentie**.<br/><br/>Machines met Unified Extensible Firmware Interface (UEFI) / opstarten met Extensible Firmware Interface(EFI) wordt niet ondersteund.<br/><br/>Namen van de machine moeten tussen 1 en 63 tekens (letters, cijfers en streepjes) bevatten. De naam moet beginnen met een letter of cijfer en eindigen met een letter of cijfer. Nadat u de replicatie voor een machine hebt ingeschakeld, kunt u de naam van de Azure wijzigen.<br/><br/>Als de bron VM NIC-koppeling wordt deze geconverteerd naar een enkele NIC na een failover naar Azure.<br/><br/>Als beveiligde virtuele machines een iSCSI-schijf hebben vervolgens zet Site herstel de beveiligde VM iSCSI-schijf naar een VHD-bestand wanneer de VM Azure overgenomen wordt. Als het iSCSI-doel kan worden bereikt door de Azure VM wordt het verbinding maken en in feite twee schijven – de VHD Azure VM schijf en de brondiskette iSCSI-Zie. In dit geval moet u verbinding met het iSCSI-doel dat wordt weergegeven op de Azure VM.
**Windows-computers (fysieke of VMware)** | De machine moet worden uitgevoerd op een ondersteunde 64-bits besturingssysteem: Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 met op minimaal SP1.<br/><br/> Het besturingssysteem moet worden geïnstalleerd op het station C:\. De OS-schijf een standaardschijf Windows moet worden en niet als dynamische. De gegevensschijf kan dynamisch worden toegepast.<br/><br/>VMs ondersteunt Recovery site met een schijf RDM. Tijdens de failback hergebruiken Site herstel van de RDM-schijf als de oorspronkelijke bron VM en RDM schijf beschikbaar is. Als ze niet beschikbaar zijn, tijdens de failback maakt Recovery Site een nieuw VMDK bestand voor elke schijf.
**Linux-machines** (phyical of VMware)|  U moet een ondersteund besturingssysteem voor de 64-bits: 6.7,7.1,7.2, Red Hat Enterprise Linux; 6.5 centos, 6.6,6.7,7.0,7.1,7.2; Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3), SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts-bestanden op een beveiligde computers moeten bevatten die de naam van de lokale host worden toegewezen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.<br/><br/>Als u wilt verbinding maken met een Azure virtual machine waarop Linux na een failover met een Secure Shell client (ssh), ervoor te zorgen dat de service Secure Shell op de beveiligde computer start automatisch op opstarten van het systeem is ingesteld en dat de firewallregels toestaan een ssh verbinding mee.<br/><br/>De hostnaam, koppelpunten, apparaat, en Linux systeempaden en bestandsnamen (bijvoorbeeld/etc /; /usr) dient alleen in het Engels zijn.<br/><br/>Beveiliging kan alleen worden ingeschakeld voor Linux machines met de volgende opslag: File system (EXT3, ETX4, ReiserFS, XFS); Multipath-software-apparaat Mapper (MPIO)); Volume manager: (LVM2). Fysieke servers met HP CCISS-controller opslag worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.<br/><br/>VMs ondersteunt Recovery site met een schijf RDM.  Tijdens de failback-bewerking voor Linux niet Recovery Site opnieuw de RDM-schijf gebruiken. In plaats daarvan wordt een nieuw VMDK bestand voor elke overeenkomstige RDM schijf gemaakt.<br/><br/>Stel dat u de instelling van de disk.enableUUID=true in de parameters voor de configuratie van de VM in VMware. De vermelding maken als deze niet bestaat. Nodig is om te geven met een consistente UUID de VMDK zodat deze correct koppelt. Deze instelling toe te voegen zorgt er tevens voor dat alleen deltawijzigingen aan in de lokalen worden overgebracht tijdens failback en niet een volledige replicatie.
**Mobiliteit Service** |  **Windows**: U zult u automatisch de mobiliteit service push naar VMs met Windows, u een administrator-account (de lokale beheerder op de Windows-computer) opgeven moet, zodat de processerver een push installatie kunt doen.<br/><br/>**Linux**: automatisch naar te zenden de mobiliteit service uitvoert, u een account maken die kan worden gebruikt door de processerver moet voor een push installatie Linux VMs.<br/><br/> Standaard worden alle schijven op een computer gerepliceerd. [Een schijf van replicatie uitsluiten](#exclude-disks-from-replication), moet de mobiliteit service handmatig worden geïnstalleerd op de computer voordat u replicatie inschakelt.<br/>

## <a name="prepare-for-deployment"></a>Voorbereiden voor distributie

Voorbereiden voor distributie moet je:

1. [Een Azure netwerk instellen](#set-up-an-azure-network) waarin Azure VMs geplaatst worden zal wanneer ze van bent of na een failover. Bovendien voor failback moet u voor het instellen van een VPN-verbinding (of Azure ExpressRoute) van het netwerk Azure naar uw site op gebouwen.
2. [Een account Azure opslag instellen](#set-up-an-azure-storage-account) voor gerepliceerde gegevens.
3. [Voorbereiden van een account](#prepare-an-account-for-automatic-discovery) op de vCenter server of vSphere host zodat Site herstel VMs VMware die worden toegevoegd automatisch kan detecteren.
4. [Voorbereiden van de server configureren](#prepare-the-configuration-server) om ervoor te zorgen dat toegang tot de gewenste URL's en vSphere PowerCLI 6.0 installeren.


### <a name="set-up-an-azure-network"></a>Een Azure netwerk instellen

- Het netwerk moet in hetzelfde gebied, Azure als die waar u de kluis Recovery Services gaat implementeren.
- Afhankelijk van de resource-model die u gebruiken wilt voor failover Azure VMs, zult u de Azure netwerk in de [ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [klassieke modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)ingesteld.
- Mislukken van Azure terug naar uw site op ruimten VMware moet u een VPN-verbinding (of een verbinding Azure ExpressRoute) van het Azure netwerk waarin de gerepliceerde Azure VMs bevinden, met het in lokalen netwerk waarin de configuratieserver zich bevindt.
- [Meer informatie over](../vpn-gateway/vpn-gateway-site-to-site-create.md) de ondersteunde implementatie voor VPN-verbindingen van site naar site, modellen en het [instellen van een verbinding](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- U kunt ook [Azure ExpressRoute](../expressroute/expressroute-introduction.md)instellen. [Meer informatie](../expressroute/expressroute-howto-vnet-portal-classic.md) over het instellen van een netwerk met ExpressRoute in Azure.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die worden gebruikt voor het herstel van de Site te implementeren.

### <a name="set-up-an-azure-storage-account"></a>Een opslag Azure-account instellen

- Je hebt een standaard of een account premie Azure opslag voor gegevens die zijn gerepliceerd naar Azure. De account moet zich in hetzelfde gebied, als de kluis Recovery Services. Afhankelijk van de resource-model die u gebruiken wilt voor failover Azure VMs, zult u een account in de [ARM](../storage/storage-create-storage-account.md) of [klassieke modus](../storage/storage-create-storage-account-classic-portal.md)instellen.
- Als u een premium-account gebruikt voor gerepliceerde gegevens die u nodig hebt voor het maken van een extra standaard account voor het opslaan van Logboeken van bestandsreplicatie die voortdurende wijzigingen in de gegevens voor de lokalen vastleggen.  

> [AZURE.NOTE] [Migratie van accounts opslag](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag gebruikt voor het herstel van de Site te implementeren.

### <a name="prepare-an-account-for-automatic-discovery"></a>Voorbereiden van een rekening voor automatische detectie

De Site herstelserver proces kan VMs VMware automatisch detecteren op vSphere hosts of een vCenter server die hosts worden beheerd. Voor het uitvoeren van automatische detectie Recovery Site referenties die toegang tot de server van VMware. Dit is niet relevant als u bent alleen fysieke machines te repliceren.

1. Voor het gebruik van een speciale rekening voor automatische detectie een rol (bijvoorbeeld Azure_Site_Recovery) maken op het niveau van vCenter met de [vereiste machtigingen beschikt](#vmware-account-permissions).
2. Een nieuwe gebruiker maken op de vSphere host of vCenter server en de rol toewijzen aan de gebruiker. U zult later laten Site herstel deze referenties weten dat er automatische detectie kan worden uitgevoerd.

    >[AZURE.NOTE] Een vCenter-gebruikersaccount met een alleen-lezen rol failover kan worden uitgevoerd maar machines beveiligde bron kan niet worden afgesloten. Als u wilt dat deze computers afsluiten moet u de functie [Azure_Site_Recovery](#vmware-account-permissions) . Als u bent alleen VMs migratie van VMware naar Azure en failback-bewerking moet niet is de rol alleen-lezen voldoende.

### <a name="prepare-the-configuration-server"></a>Voorbereiding van de configuratieserver

1.  Zorg ervoor dat de computer die u voor de configuratieserver gebruikt aan de [vereisten voldoet](#configuration-server-prerequisites). In het bijzonder zorg ervoor dat de computer is verbonden met het internet met deze instellingen:

    - Toegang tot deze URL's: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net
    - Toegang verlenen tot de [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) voor het downloaden van MySQL.
    - Communicatie met Azure met de [IP-adresbereiken Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en het protocol HTTPS (443) firewall toestaan.

2.  Download en installeer [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) op de server configureren. (Momenteel andere versies van de PowerCLI worden niet ondersteund, met inbegrip van R-releases van versie 6.0.)


## <a name="create-a-recovery-services-vault"></a>Maak een kluis Recovery Services

1. Log in om de [Azure portal](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **back-up en herstel van de Site (OMS)**. U kunt ook op **Bladeren klikken** > **Recovery Services Vault** > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. Geef in het vak **naam** een beschrijvende naam ter identificatie van de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een nieuwe resourcegroep maken](../resource-group-template-deploy-portal.md) of een bestaande selecteren. Geef een regio Azure. Machines worden gerepliceerd naar deze regio. Houd er rekening mee dat Azure opslag en netwerken die worden gebruikt voor herstel van de Site moet worden in hetzelfde gebied. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Als u wilt snel toegang tot de kluis van het Dashboard op **aan het dashboard vastmaken** , en klik op **maken**.

    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle bronnen**, en in het hoofdmenu **Recovery Services kluizen** blade.

## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een introductie-ervaring is ontworpen om u en zo snel mogelijk uitgevoerd. Het controleert de vereisten en doorloopt u de stappen die u moet Site Recovery geïmplementeerd.

Selecteert u het type waar u wilt repliceren naar en van computers die u wilt repliceren. U instellen kunt de infrastructuur, met inbegrip van servers op lokalen, Azure instellingen beleid voor replicatie en capaciteitsplanning. Nadat u uw infrastructuur schakelt u replicatie voor VMs en fysieke servers. Vervolgens kunt u uitvoeren van failover voor specifieke machines, of herstelplannen failover op meerdere computers maken.

Beginnen aan de slag door te kiezen hoe u wilt implementeren Site herstellen. De stroom aan de slag verandert enigszins afhankelijk van uw vereisten voor replicatie.


## <a name="step-1-choose-your-protection-goals"></a>Stap 1: Kies uw doelstellingen bescherming

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Selecteer uw kluis in de **Recovery Services kluizen** blade en klikt u op **Instellingen**.
2. In de **Instellingen** > **Aan de slag** Klik op **Herstel van de Site** > **stap 1: voorbereiden infrastructuur** > **doelstelling van bescherming**.

    ![Doelstellingen kiezen](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. In de **doelstelling van bescherming** **Azure te**selecteren en selecteert u **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving instellen

Instellen van de server configureren en registreren in de kluis Recovery Services. Als u bij het repliceren van VMs VMware geven de VMware-account die u gebruikt voor automatische detectie.

1. Klik op **stap 1: voorbereiden van infrastructuur** > **bron**. In **bron voorbereiden** als u niet over een configuratieserver klikt u op **+ server configuratie** een toe te voegen.

    ![Bron instellen](./media/site-recovery-vmware-to-azure/set-source1.png)

2. In de controle van de blade **Server toevoegen** weergegeven die **Server Configuration** in **Server typen**.
3. Controleer de [vereisten](#configuration-server-prerequisites)voordat u de configuratieserver instellen. In bepaalde controle dat de computer toegang krijgen de gewenste URL's tot.
4.  Download het installatiebestand Site herstel Unified Setup.
5.  De sleutel van de registratie vault downloaden. Dit hebt u nodig wanneer u Unified Setup uitvoert. De code is geldig tot 5 dagen nadat u het hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Op de computer die u als de configuratieserver gebruikt, Unified Setup uitvoeren om te installeren van de configuratieserver, de processerver en de doelserver master.


### <a name="run-site-recovery-unified-setup"></a>Herstel locatie Unified Setup

1.  Voer het installatiebestand Unified Setup.
2.  Selecteer in **voordat u** **de configuratieserver and process-server installeren**.

    ![Voordat u begint](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. Klik op **ik ga akkoord** als u wilt downloaden en installeren van MySQL in **Licentie voor Software van derden** .

    ![= Derde partij software](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. Blader in **registratie** en selecteer de registratie sleutel die u hebt gedownload van de kluis.

    ![Registratie](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. Opgeven hoe de Provider die wordt uitgevoerd op de configuratieserver maakt verbinding met Azure Site herstellen via het internet in **Internet-opties** .

    - Selecteer **verbinding maken met bestaande proxy-instellingen**als u wilt verbinden met de proxy die momenteel op de computer ingesteld.
    - Selecteer **Connect rechtstreeks zonder proxy**als u wilt dat de Provider om rechtstreeks verbinding te maken.
    - Als de bestaande proxy verificatie vereist is, of u wilt een aangepaste proxy gebruiken voor de verbinding van de Provider, selecteert u **verbinding maken met aangepaste proxy-instellingen**.
        - Als u een aangepaste proxy gebruiken moet u het adres, poort en referenties
        - Als u een proxy gebruikt hebt u al mag de URL's die worden beschreven in de [voorwaarden](#configuration-server-prerequisites).

    ![Firewall](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. In de **Controle van vereisten** setup wordt uitgevoerd een controle om ervoor te zorgen dat de installatie kunt uitvoeren. Controleer of dat de tijd op de klok (**datum en tijd** instellingen) is hetzelfde als de tijdzone als een waarschuwing wordt weergegeven over de **globale tijdsynchronisatie controleren** .

    ![Vereisten](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. Maak in de **MySQL configuratie** referenties voor aanmelding bij de MySQL-server-exemplaar dat wordt geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. In **Omgeving Details** selecteren of u gaat voor het repliceren van VMs VMware. Als u bent, controleert setup of PowerCLI 6.0 is geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. In een **Locatie** selecteren waar u de binaire bestanden installeren en de cache worden opgeslagen. Een cache-station met ten minste 600 GB wordt aangeraden, maar kunt u een station met ten minste 5 GB opslagruimte beschikbaar.

    ![Installatielocatie](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. Geef de listener (netwerkadapter en SSL-poort) waarop de configuratieserver zal verzenden en ontvangen van replicatiegegevens in **Netwerk selecteren** . U kunt de standaardinstelling wijzigen (9443) poort. Naast deze poort, wordt poort 443 gebruikt door een webserver die replicatiebewerkingen orchestrates. 443 niet moet worden gebruikt voor het ontvangen van replicatieverkeer.


    ![Netwerk selecteren](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  **Samenvatting van** de informatie en klik op **installeren**. Een wachtwoordzin is gegenereerd wanneer de installatie is voltooid. U hebt deze nodig wanneer u replicatie inschakelt dus kopiëren en op een veilige plaats bewaren.

    ![Samenvatting](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Nadat de registratie is voltooid voor de server wordt weergegeven in de **Instellingen** > blade**Servers** in de kluis.



#### <a name="run-setup-from-the-command-line"></a>Voer setup uit vanaf de opdrachtregel

U kunt de server configureren vanaf de opdrachtregel instellen:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parameters:

- / ServerMode: verplicht. Geeft aan of de configuratie en de proces-servers moeten worden geïnstalleerd of alleen de processerver. Waarden invoeren: CS, PS worden opgehaald.
- InstallLocation: verplicht. De map waarin de onderdelen worden geïnstalleerd.
- / MySQLCredsFilePath. Verplicht. Pad naar het bestand waarin de MySQL-server-referenties zijn opgeslagen. Het bestand moet in de volgende notatie:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Verplicht. De locatie van het bestand kluis referenties
- / EnvType. Verplicht. Het type installatie. Waarden: VMware, NonVMware
- / PSIP en /CSIP. Verplicht. Het IP-adres van het proces en de configuratieserver.
- / PassphraseFilePath. Verplicht. De locatie van het bestand wachtwoordzin.
- / BypassProxy. Dit is optioneel. Hiermee geeft u aan dat de configuratieserver zonder proxy verbinding met Azure maakt.
- / ProxySettingsFilePath. Dit is optioneel. Proxy-instellingen (de standaardproxy vereist verificatie of een aangepaste proxy). Het bestand moet in de volgende notatie:
    - [ProxySettings]
    - ProxyAuthentication = "Ja/Nee"
    - Proxy IP = "IP-adres >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Dit is optioneel. Het nummer van de poort moet worden gebruikt voor replicatie.
- SkipSpaceCheck. Dit is optioneel. Controle van de ruimte voor cache overslaan.
- AcceptThirdpartyEULA. Verplicht. Vlag impliceert de aanvaarding van de overeenkomst van derden.
- ShowThirdpartyEULA. Verplicht. Overeenkomst van derden weergegeven. Als de opgegeven als invoer worden alle andere parameters genegeerd.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>De VMware-account die wordt gebruikt voor automatische detectie toevoegen

 Wanneer u voorbereid voor distributie hebt u [een account op VMware gemaakt](#prepare-an-account-for-automatic-discovery) die Site herstel voor automatische detectie gebruiken kunt. Deze account als volgt toevoegen:

1. Open **CSPSConfigtool.exe**. Het is beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map [locatie] \home\svsystems\bin.
2. Klik op **Accounts beheren** > **Account toevoegen**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure/credentials1.png)

3. Voeg de account die wordt gebruikt voor automatische detectie in **De Details** . Houd er rekening mee kunt nemen 15 minuten of langer voor de accountnaam wordt weergegeven in de portal. Klik op **Configuratie van Servers** onmiddellijk bijwerken > naam van de server > **Server vernieuwen**.

    ![Details](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Verbinding maken met de vSphere hosts en servers vCenter

Als u bij het repliceren van VMs VMware vSphere hosts en servers van vCenter.

1. Controleer of de configuratieserver heeft toegang tot het netwerk met de vSphere hosts en servers van vCenter.
2. Klik op **infrastructuur voorbereiden** > **bron**. Selecteer de configuratieserver in **bron voorbereiden** en op **+ vCenter** als een vSphere host of vCenter server wilt toevoegen.
3. Geef een beschrijvende naam voor de host of vCenter server voor vSphere in **vCenter toevoegen** en geef het IP-adres of de FQDN-naam van de server. Laat de poort 443 tenzij de VMware-servers zijn geconfigureerd om te luisteren naar aanvragen op een andere poort. Selecteer de account die wordt gebruikt om verbinding met de server van VMware. Klik op **OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Als u de host met een account met administrator-bevoegdheden op de server vCenter of host geen vSphere of vCenter server toevoegt, moet u ervoor zorgen dat de account deze ingeschakeld heeft: Datacenter, Datastore, map, Host, netwerk, Resource, virtuele machine, vSphere Distributed Switch. De vCenter server moet ook de bevoegdheid opslag weergaven.

Herstel van de site verbindt met VMware-servers met de instellingen die u hebt opgegeven en VMs ontdekt.

## <a name="step-3-set-up-the-target-environment"></a>Stap 3: Instellen van de doelomgeving.

Controleer of dat u opslag voor replicatie en een Azure netwerk waarmee Azure VMs verbinding maakt na een failover-account hebt.

1.  Klik op **infrastructuur voorbereiden** > **doel** en selecteer de Azure abonnement dat u wilt gebruiken.
2.  Geef het implementatiemodel dat u wilt gebruiken voor VMs na een failover.
3.  Herstel van de site controleert dat er een of meer accounts compatibel Azure opslag en netwerken.

    ![Doel](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Als u een opslag-account nog niet hebt gemaakt en u wilt maken een ARM Klik op **+ opslag account** die inline doen.  Geef een naam, type, abonnement en locatie op het blad voor de **opslag-account maken** . De account moet in hetzelfde gebied, als de kluis Recovery Services.

    ![Opslag](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Houd rekening met:

    - Als u wilt een opslag account maken met het klassieke model zult u dit doen in de portal Azure. [Meer informatie](../storage/storage-create-storage-account-classic-portal.md)
    - Als u een account premie opslag voor gerepliceerde gegevens moet u een extra standaardopslag account instellen voor replicatie winkel gelogd die opname voortdurend gegevens voor gebouwen.

    > [AZURE.NOTE] Bescherming van de rekeningen van de premie opslag in Centraal India en Zuid-India wordt momenteel niet ondersteund.

4.  Selecteer een netwerk Azure. Als u een netwerk nog niet hebt gemaakt en u wilt die ARM Klik op **+ netwerk** die inline doen. Geef een netwerknaam, adresbereik subnet details, abonnement en locatie op de bladeserver **virtueel netwerk maken** . Het netwerk moet zich in dezelfde locatie als de kluis Recovery Services.

    ![Netwerk](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Als u wilt maken van een netwerk met behulp van het klassieke model zult u dit doen in de portal Azure. [Meer informatie](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Stap 4: Instellen van de replicatie-instellingen

1. Voor het maken van een nieuwe replicatie beleid **voorbereiden infrastructuur**klikt u op > **De replicatie-instellingen** > **+ maken en koppelen**.
2. Geef de naam van een beleid **maken** en koppelen beleid.
3. In de **vrijgegeven Productieorder drempel**: de limiet van de vrijgegeven Productieorder. Meldingen worden gegenereerd wanneer het voortdurende replicatie deze limiet wordt overschreden.
5. In **herstel punt vasthouden**, opgeven in uren hoe lang het venster inhoudingen worden voor elk herstelpunt. Beveiligde computers kunnen worden hersteld naar een willekeurig punt in een venster. Maximaal 24 uur bewaren wordt voor machines die worden gerepliceerd naar de premie opslag ondersteund.
6. In **App consistent momentopname frequentie**, opgeven hoe vaak (in minuten) herstel punten met consistente toepassing momentopnamen worden gemaakt.
7. Bij het maken van een replicatiebeleid voor standaard wordt een overeenkomstige beleid automatisch gemaakt voor failback. Voor het voorbeeld als het replicatiebeleid **rep beleid** wordt het failback-beleid zijn **rep-beleid-failback**. Dit beleid wordt niet gebruikt totdat u een failback-bewerking starten.  
8. Klik op **OK** om het beleid te maken.

    ![Replicatiebeleid](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Wanneer u een nieuw beleid maakt is deze automatisch gekoppeld aan de configuratieserver. Klik op **OK**.

    ![Replicatiebeleid](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteitsplanning

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over capaciteitsplanning en uitvinden of u aanvullende bronnen nodig hebt.

Herstel van de site biedt een planner capaciteit zodat u de juiste resources toewijzen voor uw bronomgeving, site herstel onderdelen, netwerk en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddeld aantal VMs, schijven en opslag, of in de gedetailleerde modus waarin figuren op het niveau van de belasting moet worden ingevoerd. Voordat u begint moet u:

- Informatie verzamelen over uw replicatieomgeving, met inbegrip van VMs, schijven per VMs en opslag per schijf.
- De snelheid van de dagelijkse wijzigen (lospeuteren) u voor gerepliceerde gegevens hebt te schatten. U kunt het [toestel voor capaciteitsplanning vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) doet.

1.  Klik op **downloaden** om het hulpprogramma te downloaden en uitvoeren. [Lees het artikel](site-recovery-capacity-planner.md) die bij het hulpprogramma hoort.
2.  Wanneer u klaar bent Selecteer **Ja** **hebt u voltooid capaciteitsplanning?**

    ![Capaciteitsplanning](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

In de onderstaande tabel worden een aantal punten om u te helpen bij de capaciteitsplanning voor dit scenario vastgelegd.


**Onderdeel** | **Details**
--- | --- | ---
**Replicatie** | **Dagelijks maximum snelheid wijzigen**, een beveiligde computer één processerver kan alleen worden gebruikt en een enkel processerver kunt verwerken, een dagelijkse wijzigingen beoordelen tot 2 TB. 2 TB is dus dat de maximale dagelijkse gegevens wijzigen die wordt ondersteund voor een beveiligde machine.<br/><br/> **Maximale doorvoer**— een gerepliceerde machine kan deel uitmaken van één rekening voor opslag in Azure. Een account standaardopslag maximaal 20.000 aanvragen per seconde kan verwerken en wordt aangeraden dat u het aantal IOP's via een broncomputer aan 20.000 behouden. Voor het voorbeeld hebt u een broncomputer met 5 schijven en elke schijf 120 IOP's (8K grootte) van de bron genereert vervolgens worden binnen de Azure per schijf IOP's limiet van 500. Het nummer van opslag rekeningen vereist = totale bron IOP's / 20000.
**Configuratieserver** | De server configureren voor het verwerken van de dagelijkse capaciteit van wijziging tarief in alle workloads die worden uitgevoerd op beveiligde computers kunnen en moet voldoende bandbreedte doorlopend gegevens worden gerepliceerd naar Azure opslag.<br/><br/> Beste is het raadzaam dat de server configureren bevindt zich op de LAN-segment en hetzelfde netwerk als de computers die u wilt beveiligen. Het kan worden gevonden op een ander netwerk maar machines die u wilt beveiligen L3 netwerk zichtbaarheid te moeten hebben.<br/><br/> Aanbevelingen voor de grootte van de configuratieserver worden samengevat in de volgende tabel.
**Process-server** | Het eerste processerver is standaard geïnstalleerd op de configuratieserver. U kunt extra proces-servers wilt schalen van uw omgeving kunt implementeren. Houd rekening met:<br/><br/> Process-server ontvangt van replicatiegegevens van beveiligde computers en optimaliseert met caching, compressie en codering voordat u naar Azure. Het proces server-computer moet beschikken over voldoende middelen voor het uitvoeren van deze taken.<br/><br/> De processerver gebruikt de schijfcache. Het is raadzaam een afzonderlijke cache schijf van 600 GB of meer voor het verwerken van wijzigingen in gegevens opgeslagen bij netwerk knelpunt of stroomstoring.

### <a name="size-recommendations-for-the-configuration-server"></a>Aanbevelingen voor de grootte van de configuratieserver

**CPU** | **Geheugen** | **Cachegrootte voor schijf** | **De snelheid wijzigen gegevens** | **Beveiligde computers**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 cores @ 2.5 GHz) | 16 GB | 300 GB | 500 GB of minder | Minder dan 100 machines worden gerepliceerd.
12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) | 18 GB | 600 GB | 500 GB tot 1 TB | Tussen 100-150 machines worden gerepliceerd.
16 vCPUs (2 sockets * 8 cores @ 2.5 GHz) | 32 GB | 1 TB | 1 TB tot 2 TB | Tussen 150-200 machines worden gerepliceerd.
Een ander processerver implementeren | | | > 2 TB | Extra proces-servers implementeren als u bij het repliceren van meer dan 200 machines of als dagelijkse gegevens wijzigen tarief hoger is dan 2 TB.

Waar:

- Elke broncomputer wordt geconfigureerd met 3 schijven van 100 GB.
- Wij benchmarking opslag van 8 SAS harde schijven van 10 K RPM met RAID 10 voor cache schijf metingen gebruikt.

### <a name="size-recommendations-for-the-process-server"></a>Aanbevelingen voor de grootte van de server verwerken

Als u wilt beschermen, meer dan 200 machines of wijziging dagtarief groter dan 2 TB is kunt u servers aanvullende processen voor het afhandelen van de belasting voor de replicatie kunt toevoegen. Als u wilt schalen die u kunt doen:

- Verhoog het aantal servers configureren. Zo kunt u maximaal 400 machines met twee configuratieservers beveiligen.
- Proces van extra servers toevoegen en deze gebruiken voor het verwerken van verkeer in plaats van (of naast) de configuratieserver.

Deze tabel ziet u een scenario waarin:

- U bent niet van plan de server configureren als een processerver gebruiken.
- U hebt een extra processerver instellen.
- U hebt de server extra proces voor beveiligde virtuele machines configureren.
- Elke machine beveiligde bron is geconfigureerd met drie schijven van 100 GB.

**Configuratieserver** | **Extra process-server**| **Cachegrootte voor schijf** | **De snelheid wijzigen gegevens** | **Beveiligde computers**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 16 GB geheugen | 4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8 GB geheugen | 300 GB | 250 GB of minder | 85 of minder computers repliceren.
8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 16 GB geheugen | 8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12 GB geheugen | 600 GB | 250 GB tot 1 TB | Repliceren tussen 85-150 machines.
12 vCPUs (2 sockets * 6 cores @ 2.5 GHz), 18 GB geheugen | 12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24 GB geheugen | 1 TB | 1 TB tot 2 TB | Tussen 150-225 machines worden gerepliceerd.


De manier waarop u uw servers op schaal wordt is afhankelijk van uw voorkeur voor een schaal van of schaal van model.  U laat een paar geavanceerde configuratie- en proces-servers implementeren of geschaald uit door het gebruik van meer servers met minder middelen. Bijvoorbeeld: als u 220 machines beveiligen moet u een van de volgende kan doen:

- De server configureren met 12vCPU, 18 GB geheugen, een extra processerver met 12vCPU, 24 GB geheugen, instellen en configureren van beveiligde computers voor het gebruik van alleen de processerver extra.
- U kan ook twee configuratie (2 x 8vCPU, 16 GB RAM) en twee extra proces servers (1 x 8vCPU) en 4vCPU x 1 voor het verwerken van 135 + 85 (220) machines configureren en beveiligde computers zodanig alleen de extra proces-servers configureren.

[Volg deze instructies](#deploy-additional-process-servers) voor het instellen van een extra processerver.

### <a name="network-bandwidth-considerations"></a>Netwerk bandbreedte overwegingen

U kunt de Capaciteitsplanner voor het berekenen van de bandbreedte die u nodig hebt voor replicatie (de eerste replicatie en vervolgens delta). U hebt een paar opties om te bepalen van de hoeveelheid bandbreedte te gebruiken voor replicatie:

- **De bandbreedte beperken**: VMware-verkeer dat wordt gerepliceerd naar Azure gaat via de server van een bepaald proces. U kunt de bandbreedte op de computers die als servers proces uitgevoerd.
- **Bandbreedte van invloed zijn op**: kunt u bepalen dat de gebruikte bandbreedte voor replicatie met behulp van een paar van registersleutels:
    - De waarde voor de registersleutel **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** geeft het aantal threads die worden gebruikt voor de overdracht van gegevens (initiële of delta-replicatie) van een schijf. Een hogere waarde verhoogt de netwerkbandbreedte gebruikt voor replicatie.
    - De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** geeft het aantal threads gebruikt voor de overdracht van gegevens tijdens de failback-bewerking.

#### <a name="throttle-bandwidth"></a>De bandbreedte beperken

1. Open de module MMC van Microsoft Azure back-up op de computer die fungeert als de processerver. Een snelkoppeling voor Microsoft Azure back-up is standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.

    ![De bandbreedte beperken](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Klik op het tabblad **Throttling** Selecteer **internet bandbreedtegebruik voor back-upbewerkingen bandbreedtebeperking inschakelen**, en de beperkingen voor het werk en niet-werk uren. Geldig bereik zijn van 512 Kbps tot 102 Mbps per seconde.

    ![De bandbreedte beperken](./media/site-recovery-vmware-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) voor het instellen van procesbeperking. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set OBMachineSetting NoThrottle** geeft aan dat er geen beperking vereist is.


#### <a name="influence-network-bandwidth"></a>Invloed op de netwerkbandbreedte

1. Ga naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**in het register.
    - Beïnvloeden van het verkeer van bandbreedte op een schijf vermenigvuldigt de waarde van de **UploadThreadsPerVM**wijzigen of de sleutel maken als deze niet bestaat.
    - Wijzig de waarde **DownloadThreadsPerVM**voor het beïnvloeden van de bandbreedte voor failback verkeer van Azure.
2. De standaardwaarde is 4. In een 'overprovisioned' netwerk moeten deze registersleutels van de standaardwaarden worden gewijzigd. Het maximumaantal is 32. Controleer het netwerkverkeer voor het optimaliseren van de waarde.

## <a name="step-6-replicate-applications"></a>Stap 6: Herhaalde toepassingen

Zorg ervoor dat computers die u wilt repliceren voor mobiliteit service installatie worden bereid, en schakelt u replicatie.

### <a name="install-the-mobility-service"></a>De mobiliteit-service installeren

De eerste stap bij het inschakelen van beveiliging voor virtuele machines en fysieke servers is de mobiliteit service installeren. U kunt dit op twee manieren doen:

- **Proces server push**: als u de replicatie op een computer inschakelt, push- en mobiliteit serviceonderdeel van de processerver installeren. Houd er rekening mee dat push installatie zich niet als machines een up-todate-versie van het onderdeel al wordt uitgevoerd.
- **Enterprise push**: het onderdeel met behulp van uw enterprise push proces zoals WSUS of System Center Configuration Manager of [Azure automatisering en status van de gewenste configuratie](./site-recovery-automate-mobility-service-install.md)automatisch worden geïnstalleerd. De van configuratieserver instellen voordat u dit kunt doen.
- **Handmatige installatie**: Installeer het onderdeel handmatig op elke computer die u wilt repliceren. De van configuratieserver instellen voordat u dit kunt doen.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Voorbereiden voor automatische push op Windows-computers

Hier is het voorbereiden van Windows machines zodat de mobiliteit service automatisch door de processerver kan worden geïnstalleerd.

1.  Een account maken die door de processerver kan worden gebruikt voor toegang tot de computer. De account moet beheerdersrechten (lokaal of domein) en het wordt alleen gebruikt voor de push installatie.

    >[AZURE.NOTE] Als u niet een domeinaccount gebruikt moet u uitschakelen gebruiker RAS-beheer op de lokale computer. U doet dit door in het register van de HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen de DWORD-vermelding LocalAccountTokenFilterPolicy met een waarde van 1. De registervermelding toevoegen vanuit een CLI-type **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  De Windows Firewall van de computer die u wilt beschermen, selecteert u **toestaan een app of onderdeel toegang geven via de Firewall**. Inschakelen van **Bestands- en printerdeling** en **Windows Management Instrumentation**. U kunt de firewall-instellingen voor computers die deel uitmaken van een domein configureren met een groepsbeleidsobject.

    ![Firewall-instellingen](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Voeg de account die u hebt gemaakt:

    - Open **cspsconfigtool**. Het is beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map [locatie] \home\svsystems\bin.
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Voeg de account die u hebt gemaakt. Nadat u de account toe te voegen moet u de referenties verstrekken wanneer u replicatie voor een machine.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Automatische push op Linux servers voorbereiden

1.  Zorg ervoor dat de Linux-computer die u wilt beveiligen zoals wordt beschreven in de [beveiligde computer vereisten](#protected-machine-prerequisites)wordt ondersteund. Controleer of er verbinding tussen de machine Linux en processerver-netwerk.

2.  Een account maken die door de processerver kan worden gebruikt voor toegang tot de computer. De account moet een Hoofdgebruiker op de bronserver Linux en het wordt alleen gebruikt voor de push installatie.

    - Open **cspsconfigtool**. Het is beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map [locatie] \home\svsystems\bin.
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Voeg de account die u hebt gemaakt. Nadat u de account toe te voegen moet u de referenties verstrekken wanneer u replicatie voor een machine.

3.  Controleer dat het bestand/etc/hosts op de bronserver Linux bevat items die de naam van de lokale host worden toegewezen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
4.  Installeer de meest recente openssh, openssh-server, openssl-pakketten op de computer die u wilt repliceren.
5.  Zorg ervoor dat SSH is ingeschakeld en wordt uitgevoerd op poort 22.
6.  SFTP subsysteem wachtwoord verificatie en in het bestand sshd_config als volgt inschakelen:

    - Meld u aan als root.
    - Zoek de regel die met **PasswordAuthentication begint**in het bestand /etc/ssh/sshd_config.
    - Verwijder de opmerking van de regel en wijzig de waarde van **niet** op **Ja**.
    - Zoek de regel die met het **subsysteem begint** en verwijder de opmerkingen van de regel.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>De mobiliteit Service handmatig installeren

Het installatieprogramma's zijn beschikbaar op de server Configuration in **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

Source besturingssysteem | Mobiliteit service-installatiebestand
--- | ---
Windows Server (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Mobiliteit-Service installeren op een Windows-Server


1. Downloaden en uitvoeren van de relevante installer.
2. Selecteer in **voordat u begint met** **mobiliteit service**.

    ![Mobiliteit service](./media/site-recovery-vmware-to-azure/mobility3.png)

3. Geef in de **Configuratiegegevens van de Server** het IP-adres van de server configureren en de coderingssleutel is gegenereerd toen u Unified Setup hebt uitgevoerd. U kunt de wachtwoordzin ophalen door te voeren: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe v-** op de server configureren.

    ![Mobiliteit service](./media/site-recovery-vmware-to-azure/mobility6.png)

4. Laat de standaardinstelling op **Locatie installeren** en klik op **volgende** om te beginnen met de installatie.
5. In de **Voortgang van de installatie** controleren installatie en start de computer desgevraagd opnieuw op. Het kost ongeveer 15 minuten voor de status bij te werken in de portal voor na de installatie van de service.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Mobiliteit-Service installeren op een Windows-server via de opdrachtprompt

1. Het installatieprogramma kopieert naar een lokale map (bijvoorbeeld C:\Temp) op de server die u wilt beveiligen. Het installatieprogramma vindt u op de Server configuratie onder de **\home\svsystems\pushinstallsvc\repository [locatie]**. Het pakket voor Windows-besturingssystemen hebben een naam die lijkt op de Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe
2. **Wijzig de naam van** dit bestand in MobilitySvcInstaller.exe
3. De volgende opdracht uit te pakken van de MSI installer </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>Volledige opdrachtregelsyntaxis

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parameters**

- **/Role:** Verplicht. Hiermee geeft u aan of de service mobiliteit moet worden geïnstalleerd. Invoerwaarden Agent | MasterTarget
- **/InstallLocation:** Verplicht. Geeft aan waar de service installeren.
- **/PassphraseFilePath:** Verplicht. De configuratie van server-wachtwoordzin.
- **/LogFilePath:** Verplicht. De locatie waar de logboekbestanden van de installatie moeten worden gemaakt.



#### <a name="uninstall-mobility-service-manually"></a>Mobiliteit-service handmatig verwijderen

Mobiliteit Service kan worden verwijderd met behulp van het programma toevoegen verwijderen uit het Configuratiescherm of met behulp van de opdrachtregel.

Is de opdracht voor het verwijderen van de mobiliteit Service met behulp van de opdrachtregel

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Mobiliteit-Service installeren op een Linux server met de opdrachtregel

1. Kopieer de juiste tar-archief op basis van de bovenstaande tabel met de Linux-computer die u wilt repliceren.
2. Open een shellprogramma en pak het gecomprimeerde tar-archief naar een lokaal pad door te voeren:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Maak een bestand passphrase.txt in de lokale map waarin u de inhoud van de tar-archief uitgepakt. Doen dit kopiëren de wachtwoordzin C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase op de server configureren en opslaan in passphrase.txt door *`echo <passphrase> >passphrase.txt`* in de shell.
4. De mobiliteit service installeren door het uitvoeren van *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Geef het interne IP-adres van de server configureren en controleren of poort 443 is geselecteerd. Het kost ongeveer 15 minuten voor de status bij te werken in de portal voor na de installatie van de service.

**U kunt ook installeren vanaf de opdrachtregel**:

1. De wachtwoordzin van C:\Program Files (x86) \InMage Systems\private\connection op de configuratieserver kopiëren en opslaan als 'passphrase.txt' op de server configureren. Vervolgens deze opdrachten uitvoeren. In dit voorbeeld wordt de configuratie van IP-adres 104.40.75.37 en de HTTPS-poort 443 moet zijn:

Op een productieserver installeert:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Installeren op de doelserver master:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Replicatie inschakelen

#### <a name="before-you-start"></a>Voordat u begint

Als u bij het repliceren van VMware virtual machines Houd rekening met het volgende:

- VMware VMs elke 15 minuten worden ontdekt en het duurt 15 minuten of langer voordat ze worden weergegeven in de portal na detectie. Ook discovery kan 15 minuten of langer duren wanneer u een nieuwe vCenter server of vSphere host toevoegen.
- De wijzigingen op de virtuele machine (zoals de installatie van VMware's) kunnen ook duren 15 minuten of langer moet worden bijgewerkt in de portal.
- U kunt controleren voor het ontdekte laatst VMs VMware in het veld **Laatste Contact op** voor de host vCenter server/vSphere op de blade **-Servers configureren** .
- Machines voor replicatie zonder te wachten op de geplande discovery toevoegen, markeert de configuratieserver (Klik niet op deze) en klik op de knop **vernieuwen** .
- Wanneer u replicatie inschakelt als de computer automatisch de processerver is opgesteld, wordt de service mobiliteit erop geïnstalleerd.

#### <a name="exclude-disks-from-replication"></a>Schijven uitsluiten van replicatie

Wanneer u replicatie inschakelt, standaard worden alle schijven op een computer gerepliceerd. U kunt schijven uitsluiten van replicatie. Zoals u wellicht niet wilt repliceren, schijven met tijdelijke gegevens of gegevens die telkens wanneer een machine is vernieuwd of toepassing opnieuw wordt gestart (bijvoorbeeld pagefile.sys of SQL Server tempdb). Als u wilt uitsluiten schijven Let wel:

- U kunt alleen schijven die al de mobiliteit service geïnstalleerd hebben uitsluiten. U moet [de mobiliteit-service handmatig installeren](#install-the-mobility-service-manually) omdat de mobiliteit service wordt alleen geïnstalleerd met push-mechanisme na replicatie is ingeschakeld.
- Alleen standaardschijven kunnen worden uitgesloten van replicatie. U kunt geen OS of dynamische schijven uitsluiten.
- Nadat de replicatie is ingeschakeld kan u toevoegen of verwijderen van schijven voor replicatie. Als u wilt toevoegen aan of uitsluiten van een schijf die u wilt uitschakelen voor de computer en deze vervolgens opnieuw inschakelen.
- Als u een schijf die nodig is voor het functioneren van een toepassing uitsluiten, na een failover naar Azure moet u deze handmatig maken in Azure zodat de gerepliceerde toepassing kunt uitvoeren. U kunt ook kan u integreren met Azure Automatisering een herstelplan voor het maken van de schijf tijdens een overname van de machine.
- Schijven die u handmatig in Azure maakt weer mislukt. Bijvoorbeeld als u meer dan drie schijven mislukken en twee rechtstreeks in Azure, alle vijf mislukt weer. Schijven handmatig gemaakt van failback-bewerking kan niet worden uitgesloten.

**Replicatie nu als volgt inschakelen**:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Nadat u replicatie voor het eerst hebt ingeschakeld zult u **+ repliceren** in de kluis replicatie voor extra computers inschakelen.
2. In de **bron** -blade > **bron** selecteert u de server configureren.
3. Selecteer de **virtuele Machines** of **Fysieke Machines**in **type Machine** .
4. Selecteer de vCenter server die de host vSphere beheert, of Selecteer de host in **vCenter/vSphere Hypervisor** . Deze instelling is niet van belang als u bij het repliceren van fysieke machines.
5. Selecteer de server verwerken. Dit is de naam van de server configureren als u een extra proces-servers hebt gemaakt. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. In **doel** selecteert u het abonnement kluis en selecteer het model dat u wilt gebruiken in Azure na een failover (klassiek of resource management) in **implementatiemodel boeken failover** .
7. Selecteer de Azure opslag u gebruikt voor het repliceren van gegevens. Houd rekening met:

    - U kunt een premium of standard opslag account selecteren. Als u een premium-account moet u een account extra standaardopslag voor voortdurende replicatie logboekbestanden opgeven. Rekeningen moeten zich in hetzelfde gebied, als de kluis Recovery Services.
    - Als u wilt gebruiken een andere opslag-account dan de hebt u u kunt [maken](#set-up-an-azure-storage-account). Klik op **Nieuw**om een opslag account met behulp van het model van de ARM. Als u wilt een opslag account maken met het klassieke objectmodel doet u dat [in de portal Azure](../storage/storage-create-storage-account-classic-portal.md).

8. Selecteer de Azure netwerk en subnet die Azure VMs verbinding maakt wanneer u ze hebt gesponnen van na een failover. Het netwerk moet zich in hetzelfde gebied, als de kluis Recovery Services. Selecteer **Configure nu voor de geselecteerde computers** het netwerkinstelling toepassen op alle computers die u selecteert voor de bescherming. Selecteert u **later configureren** selecteren de Azure netwerk per computer. Als u een netwerk geen moet u [er een maken](#set-up-an-azure-network). Klik op **Nieuw**om een netwerk met behulp van het model van de ARM. Als u wilt maken van een netwerk met behulp van het model Klassiek doet u dat [in de portal Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer een subnet, indien van toepassing. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. In de **virtuele Machines** > **virtuele machines te selecteren** klikt u op en selecteert u elke computer die u wilt repliceren. U kunt alleen selecteren machines waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. In het dialoogvenster **Eigenschappen** > **eigenschappen configureren**, selecteer de account die wordt gebruikt door de processerver automatisch de mobiliteit service op de computer installeren. Standaard worden alle schijven worden gerepliceerd. Klik op **Alle schijven** en schakelt u alle schijven die u niet wilt repliceren. Klik vervolgens op **OK**. U kunt later extra eigenschappen instellen.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. In **de replicatie-instellingen** > **-replicatie-instellingen configureren** Controleer of het juiste replicatiebeleid is geselecteerd. Instellingen voor replicatie in de **Instellingen**kunt u > **replicatie beleid** > beleidsnaam > **Instellingen bewerken**. Wijzigingen die u op een beleid toepast worden toegepast op repliceren en nieuwe machines.

12. **Multi-VM consistentie** inschakelen als u wilt dat computers in een replicatiegroep verzamelen en geef een naam op voor de groep. Klik vervolgens op **OK**. Houd rekening met:

    - Machines in de replicatie replicatie te groeperen en crash consistent en app consistent herstel punten hebt gedeeld, wanneer zij een failover.
    - Wij raden aan dat u verzamelen VMs en fysieke servers zodat ze de werklast. Inschakelen meerdere VM consistentie kan invloed op de prestaties van de werkbelasting en mag alleen worden gebruikt als machines de dezelfde werklast worden uitgevoerd en moet u de consistentie.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Klik op **Replicatie inschakelen**. U kunt de voortgang van het project **Beveiliging inschakelen** in de **Instellingen**volgen > **taken** > **Site herstel taken**. Nadat de taak **Voltooien bescherming** wordt uitgevoerd de computer is gereed voor failover.

> [AZURE.NOTE] Als de computer is voorbereid voor de push installatie die het onderdeel mobiliteit service wordt geïnstalleerd als beveiliging is ingeschakeld. Nadat het onderdeel is geïnstalleerd op de machine die bescherming taak begint en mislukt. Na de storing moet u elke computer handmatig opnieuw te starten. Na het opnieuw opstarten de taak bescherming opnieuw begint en de initiële replicatie plaatsvindt.

### <a name="view-and-manage-vm-properties"></a>Weergeven en beheren van de eigenschappen van de VM

Wij raden aan dat u de eigenschappen van de broncomputer controleren. Houd er rekening mee dat de naam Azure VM met [Azure VM eisen](site-recovery-best-practices.md#azure-virtual-machine-requirements)dienen te voldoen.

1. Klik op **Instellingen** > **gerepliceerde objecten** > en selecteer de machine. De blade **Essentials** bevat informatie over instellingen voor machines en status.

2. In het dialoogvenster **Eigenschappen** kunt u replicatie en -failover-informatie voor de VM weergeven.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. In het **netwerk en de rekenkracht** > **eigenschappen berekenen** kunt u de grootte van de naam en het doel Azure VM. De naam om te voldoen aan de eisen van Azure als u wilt wijzigen.
U kunt ook weergeven en informatie toevoegen over het doelnetwerk, subnetmasker en IP-adres dat wordt toegewezen aan de Azure VM. Let op het volgende:

    - U kunt het doel-IP-adres instellen. Als u een adres, de mislukte niet via de computer opgeeft DHCP gebruikt. Als u een adres dat niet beschikbaar failover wordt uitgevoerd, werkt de failover-functie niet. Hetzelfde doel-IP-adres kan worden gebruikt voor failover-test als het adres in het failover-testnetwerk beschikbaar is.
    - Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele machine van doel, als volgt opgeeft:

        - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters die zijn toegestaan voor de grootte van de doel machine is, vervolgens heeft het doel hetzelfde aantal adapters als de bron.
        - Als het aantal adapters voor de virtuele machine van bron het aantal dat is toegestaan overschrijdt voor de doelgrootte en het doel maximaal worden gebruikt.
        - Als bijvoorbeeld een broncomputer twee netwerkadapters heeft en de grootte van target machine ondersteunt vier, moet de doelcomputer twee adapters. Als de broncomputer twee adapters heeft maar de ondersteunde doelgrootte slechts één ondersteunt hebben de doelcomputer slechts één adapter.     
    - Als de VM meerdere netwerkadapters heeft verbinding ze al met hetzelfde netwerk.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. In **schijven** kunt u het besturingssysteem en de gegevensschijven bekijken op de VM die wordt gerepliceerd.


## <a name="step-7-test-the-deployment"></a>Stap 7: De implementatie testen

Om te testen van de implementatie kunt u een failover testen voor een enkele virtuele machine of een herstelplan met een of meer virtuele machines uitvoeren.


### <a name="prepare-for-failover"></a>Voorbereiden voor failover

- Voor het uitvoeren van een test failover die wordt aangeraden dat u een nieuw Azure netwerk die is geïsoleerd uit het Azure productienetwerk (dit is standaard bij het maken van een nieuw netwerk in Azure) maken. [Meer informatie](site-recovery-failover.md#run-a-test-failover) over het uitvoeren van failover van de test.
- Als u de beste prestaties wanneer u failover uitvoeren naar Azure, de Azure-Agent op de beveiligde computer te installeren. Het wordt sneller opgestart en helpt bij het oplossen van problemen. De agent [Linux](https://github.com/Azure/WALinuxAgent) of [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) te installeren.
- Om een volledige test de implementatie moet u een infrastructuur voor de gerepliceerde machine werken zoals verwacht. U kunt een virtuele machine maken als een domeincontroller met DNS en deze repliceren naar Azure met Azure Site herstellen als u wilt testen van Active Directory en DNS. Lees meer in de [overwegingen van failover-test voor Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Zorg ervoor dat de configuratieserver wordt uitgevoerd. Failover, anders mislukt.
- Als u schijven hebt uitgesloten van replicatie mogelijk moet u deze diskettes maken handmatig in Azure na een failover zodat de toepassing wordt uitgevoerd zoals verwacht.
- Let op het volgende als u een niet-geplande failover in plaats van een test failover uitvoeren:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoeren. Dit zorgt ervoor dat u hoeft niet de bron- en replica-computers op hetzelfde moment. Als u bij het repliceren van VMs VMware kunt vervolgens u herstel van de Site moet een Qualitative afsluiten van de bron-machines. Afhankelijk van de status van de primaire site kan of niet kan werken. Als u bij het repliceren van fysieke servers biedt niet herstel van de Site deze optie.
    - Wanneer u een niet-geplande failover uitgevoerd stopt gegevensreplicatie uit primaire machines zodat alle gegevens delta won't worden overgedragen wanneer een niet-geplande failover. Ook als u een niet-geplande failover op een herstelplan uitvoert wordt deze uitgevoerd totdat volledige, zelfs als er een fout optreedt.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Verbinding maken met Azure VMs na een failover voorbereiden

Als u wilt verbinding maken met Azure VMs met RDP na een failover, moet dat u het volgende doen:

**Op de computer op gebouwen voordat failover wordt uitgevoerd**:

- RDP inschakelen voor toegang via het internet, zorg ervoor dat TCP- en UDP-regels worden toegevoegd voor de **openbare**en zodat RDP in **Windows Firewall**wordt toegestaan -> **toegestaan apps en functies** voor alle profielen.
- RDP inschakelen op de computer om toegang te krijgen via een verbinding van site naar site en ervoor te zorgen dat de RDP is toegestaan in **Windows Firewall** -> **toegestaan apps en functies** voor het **domein** en **particuliere** netwerken.
- De [Azure VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) installeren op de computer op gebouwen.
- [De mobiliteit-service handmatig te installeren](#install-the-mobility-service-manually) op computers in plaats van de processerver gebruiken om de service automatisch. Dit komt omdat de push installatie alleen gebeurt als de computer is ingeschakeld voor replicatie.
- Zorg ervoor dat het besturingssysteem SAN-beleid is ingesteld op OnlineAll. [Meer informatie]( https://support.microsoft.com/kb/3031135)
- De IPSec-service uitschakelen voordat u de failover-functie uitvoeren.

**Op de Azure VM na een failover**:

- Een openbare eindpunt voor het RDP-protocol (poort 3389) toevoegen en geef de referenties voor aanmelding.
- Controleer of er geen domeinbeleid om te voorkomen dat u geen verbinding maken met een virtuele machine met behulp van een openbaar adres.
- Probeer verbinding te maken. Controleer of de VM wordt uitgevoerd als u geen verbinding maken. Lees dit [artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)voor meer tips voor het oplossen van problemen.


Als u toegang wilt tot een Azure VM waarop Linux wordt uitgevoerd na een failover met een Secure Shell client (ssh), het volgende doen:

**Op de computer op gebouwen voordat failover wordt uitgevoerd**:

- Zorg ervoor dat de service Secure Shell op de Azure VM is ingesteld op automatisch starten op het systeem wordt opgestart.
- Controleer de firewall-regels dat een SSH-verbinding met het.

**Op de Azure VM na een failover**:

- De groep beveiligingsregels voor netwerken op de mislukte via VM en de Azure subnet waaraan deze is aangesloten moeten toestaan binnenkomende verbindingen naar de SSH-poort.
- Een eindpunt openbaar moet worden gemaakt zodat binnenkomende verbindingen op de SSH-poort (standaard TCP-poort 22).
- Als de VM toegankelijk is via een VPN-verbinding (Route Express of site naar site VPN) kan de client rechtstreeks verbinding maken met de VM via SSH worden gebruikt.

**Op de Azure Windows/Linux VM na een failover**:

Als u een netwerk-beveiligingsgroep die is gekoppeld aan de virtuele Machine of het subnet waartoe de machine te behoort hebben, zorg dat de groep netwerk heeft een uitgaande regel zodat HTTP/HTTPS. Zorg ook dat de DNS-server van het netwerk met welke virtuele machine is opvragen van niet meer dan juist is geconfigureerd. Anders de failover-functie kan een time-out met fout-'PreFailoverWorkflow taak WaitForScriptExecutionTask timed out'. Om dit te begrijpen in detail, raadpleegt u de sectie op het herstel van de [controle en de handleiding voor het oplossen van problemen](site-recovery-monitoring-and-troubleshooting.md#recovery).

## <a name="run-a-test-failover"></a>Een failover test uitvoeren

1. Failover van een enkele computer, in de **Instellingen** > **Items gerepliceerd**, klikt u op de VM > pictogram **+ failover-Test** .

    ![Test-failover](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Failover van een herstelplan in **Instellingen** > **Recovery plan**, met de rechtermuisknop op het plan > **Test Failover**. U maakt een herstelplan [voert u deze instructies](site-recovery-create-recovery-plans.md).

3. Selecteer de Azure netwerk waarop Azure VMs verbonden nadat failover plaatsvindt in **Failover-Test** .
4. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM de eigenschappen ervan te openen of op de taak **Testen Failover** in naam van de kluis > **Instellingen** > **taken** > **Site herstel taken**.
5. Als de failover-functie de status **voltooid testen bereikt** , het volgende doen:

    1. De replica virtuele machine bekijken in het portal voor Azure. Controleer of de virtuele machine gestart.
    2. Als u ingesteld voor toegang tot virtuele machines vanaf het netwerk op ruimten bent kunt u een verbinding met extern bureaublad op de virtuele machine te starten.
    3. Klik op **test voltooid** wilt voltooien.

        ![Test-failover](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Klik op **notities** als u wilt opnemen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de overname van de test.
    5. Klik op **de test failover is uitgevoerd** voor het automatisch opschonen van de testomgeving. Nadat dit is gebeurd wordt failover test de status **voltooid** weergegeven.
    6.  In dit stadium worden geen elementen of VMs automatisch gemaakt door sites worden hersteld tijdens de test failover verwijderd. Eventuele aanvullende elementen die u hebt gemaakt voor failover-test niet worden verwijderd.

    > [AZURE.NOTE] Als een failover test langer dan twee weken wordt het uitvoeren van kracht blijft.


6. Na voltooiing van de overname ook moet u kunnen zien van de replica Azure machine worden weergegeven in de portal Azure > **virtuele Machines**. U moet ervoor zorgen dat de VM de juiste grootte waarmee het juiste netwerk is verbonden en wordt uitgevoerd.
7. Als u [voorbereid voor na een failover-verbindingen](#prepare-to-connect-to-azure-vms-after-failover) u verbinding kunnen moet maken met de Azure VM.

## <a name="monitor-your-deployment"></a>De installatie controleren

Hier is het controleren van de configuratie-instellingen, status en gezondheid voor de implementatie van het herstel van de Site:

1. Klik op de naam van de kluis voor toegang tot de dashboard **Essentials** . In dit dashboard kunt u Site-Recovery projecten, replicatiestatus herstelplannen, serverstatus en gebeurtenissen.  U kunt de Essentials tegels en -indelingen die vooral handig voor u zijn, met inbegrip van de status van andere kluizen Site herstel- en back-up weergegeven.<br>
![Essentials](./media/site-recovery-vmware-to-azure/essentials.png)

2. In de tegel **gezondheid** kunt u site-servers (VMM of configuratie van servers) ondervonden probleem, en de gebeurtenissen die door de Site herstellen in de afgelopen 24 uur controleren.
3. U kunt beheren en controleren van replicatie in de **Gerepliceerde objecten**, **Recovery plan**, en de **Site herstel taken** naast elkaar. U kunt inzoomen op de taken in de **Instellingen** -> **taken** -> **Site herstel taken**.


## <a name="deploy-additional-process-servers"></a>Extra proces-servers implementeren

Als u hebt uw implementatie meer dan 200 machines van de bron of een totale dagelijkse lospeuteren snelheid van meer dan 2 TB schaalt, moet u extra proces-servers voor het verwerken van het netwerkverkeer.

De [aanbevelingen voor proces-servers](#size-recommendations-for-the-process-server) controleren en volg deze instructies voor het instellen van de processerver. Na het instellen van de server hebt u machines voor het gebruik van deze bron migreren.

### <a name="install-an-additional-process-server"></a>Een extra processerver installeren

1. In de **Instellingen** > **-servers herstel van de Site** klikt u op de configuratieserver > **Process-server**.

    ![Process-server toevoegen](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. Klik in **Type Server** **processerver (op locatie)**.

    ![Process-server toevoegen](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. De Site herstel Unified Setup-bestand downloaden en uitvoeren om de processerver installeren en registreren in de kluis.
4. Selecteer in **voordat u begint met het** **proces van extra servers toevoegen voor het schalen van de implementatie**.
5. Voltooi de wizard in de dezelfde manier als wanneer u [instellen van](#step-2-set-up-the-source-environment) de configuratieserver.

    ![Process-server toevoegen](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. Geef het IP-adres van de configuratieserver en de wachtwoordzin in **Server-configuratiegegevens** . Verkrijgen van de wachtwoordzin uitvoeren ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** op de server configureren.

    ![Process-server toevoegen](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Machines voor het gebruik van het nieuwe processerver migreren

1. In de **Instellingen** > **-servers herstel van de Site** klikt u op de server configureren en klik op **proces-servers**.

    ![Process-server bijwerken](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Klik met de rechtermuisknop op de server verwerken die momenteel in gebruik en klik op **activeren**.

    ![Process-server bijwerken](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. Selecteer in **Selecteer doelserver proces**, het nieuwe processerver die u wilt gebruiken en selecteer vervolgens de virtuele machines die het nieuwe processerver worden verwerkt. Klik op het pictogram voor informatie voor meer informatie over de server. Om u te helpen u de werklast van beslissingen, wordt de gemiddelde ruimte die nodig is voor het repliceren van elke geselecteerde virtuele machine naar het nieuwe processerver weergegeven. Klik op het vinkje om te repliceren naar de nieuwe processerver start.

## <a name="vmware-account-permissions"></a>VMware accountmachtigingen

De processerver kan VMs automatisch op een vCenter server vinden. Voor het uitvoeren van automatische detectie moet u voor het [definiëren van een rol (Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) herstel van de Site toegang te krijgen tot de server van VMware. Merk op dat als u alleen hoeft te VMware machines migreren naar Azure en geen failback van Azure wilt, kunt u de rol van een alleen-lezen dat groot genoeg is. De vereiste machtigingen zijn samengevat in de volgende tabel.

**Rol** | **Details** | **Machtigingen**
--- | --- | ---
Azure_Site_Recovery rol | VM VMware detectie |Deze bevoegdheden voor de server v Center toewijzen:<br/><br/>Datastore -> ruimte toewijzen, bladeren datastore, lage niveau bestand bewerkingen., bestand verwijderen, virtuele machine updatebestanden<br/><br/>Netwerk -> netwerk toewijzen<br/><br/>Resource -> virtuele machine toewijzen aan resourcegroep migreren uitgeschakeld virtuele machine, migreren is ingeschakeld op de virtuele machine<br/><br/>Taken maken, taak, taak -><br/><br/>Virtuele machine configuratie -><br/><br/>Virtuele machine beheren -> -> vraag beantwoorden, apparaat verbinding., configureren van CD's, floppy's configureren, uit kracht van, VMware tools installeren<br/><br/>Virtuele machine voorraad -> -> maken, journaal en registratie<br/><br/>Virtuele machine Provisioning -> -> toestaan virtuele machine downloaden, uploaden van bestanden van de virtuele machine toestaan<br/><br/>VM-Snapshots > -> momentopnamen verwijderen
de rol van de gebruiker vCenter | VM VMware detectie/Failover zonder het afsluiten van de bron VM | Deze bevoegdheden voor de server v Center toewijzen:<br/><br/>Data Center-object –> doorgeven aan onderliggende Object, rol = alleen lezen <br/><br/>De gebruiker is toegewezen op het niveau van datacenter en dus toegang heeft tot alle objecten in het datacenter.  Als u wilt om de toegang te beperken, kunt u de rol **geen toegang** met het object **doorgeven aan onderliggende** toewijzen aan de onderliggende objecten (vSphere hosts, datastores, VMs en netwerken).
de rol van de gebruiker vCenter | Failover en failback | Deze bevoegdheden voor de server v Center toewijzen:<br/><br/>Datacenter-object doorgeven aan onderliggende object, rol = Azure_Site_Recovery<br/><br/>De gebruiker is toegewezen op het niveau van datacenter en dus toegang heeft tot alle objecten in het datacenter.  Als u wilt om de toegang te beperken, kunt u de rol **geen toegang** met het **doorgeven aan een onderliggend object** toewijzen aan het onderliggende object (vSphere hosts, datastores, VMs en netwerken).  
## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](site-recovery-failover.md) over de verschillende typen van failover.
- [Meer informatie over de failback-bewerking](site-recovery-failback-azure-to-vmware.md) om de mislukte via computers in Azure terug naar uw omgeving op gebouwen.

## <a name="third-party-software-notices-and-information"></a>Software van derden mededelingen en bekendmakingen

Niet vertalen of lokaliseren

De software en firmware in het Microsoft-product of de service wordt uitgevoerd op basis van of uitgerust met materiaal uit de onderstaande projecten (gezamenlijk "derden Code").  Microsoft is niet de oorspronkelijke auteur van de Code van derden.  De oorspronkelijke auteursrechtaanduiding en licentie, waaronder Microsoft dergelijke Code derden ontvangen ingesteld hierna anders wordt bepaald.

De informatie in de sectie A is met betrekking tot onderdelen van derden Code uit de onderstaande projecten. Deze licenties en informatie dienen alleen ter informatie.  Deze Code van derden, is aan u door Microsoft onder de licentievoorwaarden voor Microsoft software voor het Microsoft-product of de service wordt relicensed.  

De informatie in de sectie B wordt met betrekking tot onderdelen van derden Code die beschikbaar worden gesteld aan u door Microsoft onder de oorspronkelijke licentievoorwaarden.

Het volledige dossier kan worden gevonden op het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behoudt zich alle rechten voor die niet uitdrukkelijk in deze overeenkomst worden verleend door implicatie, ' estoppel ' of anderszins.
