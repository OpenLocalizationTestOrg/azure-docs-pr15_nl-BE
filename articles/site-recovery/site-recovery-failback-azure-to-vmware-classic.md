<properties 
   pageTitle="Fouten terug VMware virtuele machines en fysieke servers op de site van gebouwen | Microsoft Azure"
   description="Informatie over mislukte terug naar de website van gebouwen na de overname van VMs VMware en Azure fysieke servers." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Fail back VMware virtuele machines en fysieke servers op de site van gebouwen

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure klassieke Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klassieke Portal (verouderd)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



In dit artikel wordt beschreven hoe u niet terug Azure virtuele machines van Azure naar de site van gebouwen. Volg de instructies in dit artikel wanneer u klaar bent om uw VMware virtuele machines failback of fysieke servers Windows/Linux nadat u ze hebt overgenomen uit de ruimten van site naar Azure deze [zelfstudie](site-recovery-vmware-to-azure-classic.md)gebruiken.



## <a name="overview"></a>Overzicht

In dit diagram ziet u de failback-architectuur voor dit scenario.

Deze architectuur gebruiken wanneer de processerver op gebouwen en u een ExpressRoute gebruikt.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Deze architectuur gebruiken wanneer de processerver op Azure en er een VPN of een ExpressRoute-verbinding.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

De volledige lijst van havens en de failback diagram architechture raadpleegt u de onderstaande afbeelding

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Hier ziet u de werking van failback-bewerking:

- Nadat u hebt niet via Azure, u niet terug naar uw site op de ruimten in een paar stappen:
    - **Fase 1**: U het VMs Azure reprotect zodat zij weer worden gerepliceerd naar VMware VMs uitgevoerd in uw site op gebouwen. Reprotection inschakelen de VM worden verplaatst naar een groep failback bescherming automatisch gemaakt is wanneer u de groep failover-beveiliging gemaakt. Als u uw groep failover-bescherming een herstelplan vervolgens de groep failback bescherming is ook automatisch toegevoegd aan het plan hebt toegevoegd.  Tijdens de reprotect u failback plannen.
    - **Fase 2**: nadat de VMs Azure naar uw site op gebouwen repliceert, voert u een fail over mislukken van Azure.
    - **Stap 3**: nadat u uw gegevens niet terug, reprotect het VMs in ruimten die u niet terug naar, zodat deze beginnen met repliceren naar Azure.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Failback naar de oorspronkelijke of alternatieve locatie

Als u niet over een VM VMware kunt u niet terug naar dezelfde bron VM als nog steeds in gebouwen bestaat. In dit geval alleen de deltawijzigingen weer mislukt. Houd rekening met:

- Als u fysieke servers failover is failback altijd voor een nieuwe VMware VM.
    - Alvorens terug een fysieke machine Opmerking
    - Fysieke machine beschermd zal terugkomen als een virtuele machine als failover van Azure, VMware
    - Zorg ervoor dat u ten minste een Master Target server samen met de nodige ESX/ESXi hosts waarop moet u failback ontdekt.
- Als u niet terug naar de oorspronkelijke VM is de volgende vereist:
    - Als de VM wordt beheerd door een vCenter server moet het doel van de Master ESX host toegang tot het gegevensarchief VMs hebben.
    - Als de VM op een host ESX maar wordt niet beheerd door vCenter moet de vaste schijf van de VM worden in een gegevensarchief toegankelijk door van de MT-host.
    - Als uw VM op een host ESX en maakt geen gebruik van vCenter moet u de detectie van de ESX-host van de MT voltooien voordat u reprotect. Dit is van toepassing als u bent terug fysieke servers te mislukken.
    - Een andere optie is (als de VM op ruimten bestaat) te verwijderen voordat u een failback-bewerking. Failback vervolgens maakt een nieuwe VM op dezelfde host als de master ESX doelhost.
    
- Wanneer wordt het gegevensarchief dezelfde en dezelfde host als door de server op de locatie master doel ESX failback naar een andere locatie van de gegevens hersteld. 


## <a name="prerequisites"></a>Vereisten

- Je hebt een VMware omgeving te mislukken back VMs VMware en fysieke servers. Storing op een fysieke server wordt niet ondersteund.
- Om het failback-moet u hebt gemaakt een Azure netwerk wanneer u eerst de beveiliging instellen. Failback-bewerking moet een VPN- of ExpressRoute verbinding vanaf de Azure netwerk waarin de VMs Azure bevinden zich op de site van gebouwen.
- Als de VMs u failback naar worden beheerd door een vCenter server moet u controleren of hebben u de vereiste machtigingen voor de detectie van VMs op vCenter servers. [Lees meer](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Als de momentopnamen aanwezig zijn op een VM mislukt reprotection. U kunt de momentopnamen of de schijven verwijderen. 
- Voordat u failback-bewerking moet u voor het maken van een aantal onderdelen:
    - **Een processerver in Azure maken**. Dit is een Azure VM u nodig hebt om te maken en te houden tijdens de failback-bewerking wordt uitgevoerd. Nadat de failback-bewerking is voltooid, kunt u de computer verwijderen.
    - **Maken van een master-doelserver**: de master doelserver gegevens verzendt en ontvangt failback. De server voor u gemaakt op de locatie heeft een master doelserver standaard geïnstalleerd. Het volume van het verkeer van mislukte back moet u mogelijk echter een aparte master doelserver voor failback maken.
    - Als u een extra master target server op Linux te maken wilt, moet u voor het instellen van de Linux VM voordat u de master-doelserver installeren zoals hieronder beschreven.
- Configuratieserver is vereist in ruimten wanneer u een failback-bewerking. Tijdens de failback-bewerking, moet de virtuele machine aanwezig zijn in de configuratiedatabase server, bij gebreke daarvan welke failback niet lukken. Daarom zorgen ervoor dat u regelmatig geplande back-up van uw server. In het geval van een ramp moet u herstellen met hetzelfde IP-adres dat failback werken.

## <a name="set-up-the-process-server-in-azure"></a>De server verwerken in Azure instellen

U moet een processerver installeren in Azure zodat de Azure VMs gegevens terug naar de master doelserver voor gebouwen kunt verzenden. 

1.  In de portal-Site herstellen > **Configuration Servers** selecteren om een nieuw processerver toevoegen.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Geef een servernaam proces en voer een naam en wachtwoord die kunt u verbinding maken met de Azure VM als beheerder. In **Server Configuration** selecteert u de server op locatie en geef de Azure netwerk waarin de processerver moet worden geïmplementeerd. Dit moet het netwerk waarop het VMs Azure. Geef een uniek IP-adres van het geselecteerde subnet en implementatie begint.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Een taak voor het implementeren van de processerver wordt geactiveerd

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Nadat de processerver wordt geïmplementeerd in Azure kunt u zich aanmelden op met behulp van de referenties die u hebt opgegeven. De eerste keer dat u zich in het dialoogvenster proces-server wordt uitgevoerd. Typ het IP-adres van de server in de ruimten en de wachtwoordzin. Laat de standaardinstelling voor poort 443. U kunt ook laten de standaard 9443 poort voor gegevensreplicatie tenzij u deze instelling expliciet gewijzigd tijdens het instellen van de server op locatie. 

    >[AZURE.NOTE] De server is niet zichtbaar onder **Eigenschappen voor VM**. Het is alleen zichtbaar op het tabblad **Servers** in de server waarop het is ingeschreven. Het kan duren over 10-15 minuten voor de processerver worden weergegeven.


## <a name="set-up-the-master-target-server-on-premises"></a>De master target server op-ruimten instellen

De master doelserver ontvangt de failback-gegevens. Een doel master-server wordt automatisch geïnstalleerd op de server op locatie, maar als u bent anders weer veel gegevens moet u mogelijk een extra master doelserver instellen. Dit als volgt:

>[AZURE.NOTE] Als u een doel master-server installeren onder Linux wilt, volg de instructies in de volgende procedure.

1. Als u de master-doelserver op Windows installeert, opent de pagina Quick Start van de VM waarop u de basispagina doelserver installeert en download het installatiebestand voor de Azure Site herstel Unified Setup wizard.
2. Setup en selecteer **proces extra servers toevoegen voor het schalen van implementatie**in **voordat u begint** .
3. Voltooi de wizard in de dezelfde manier als wanneer u [de server instellen](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Geef het IP-adres van deze doelserver master en een wachtwoordzin toegang te krijgen tot de VM op de pagina **Configuratie-gegevens** .

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Een Linux VM ingesteld als de doelserver master
Voor het instellen van de server met de master doelserver als een Linux VM moet u het installeren van de Cent) S 6.6 minimaal besturingssysteem ophalen van de SCSI-id's voor elke SCSI-vaste schijf sommige extra pakketten installeren en sommige aangepaste wijzigingen toe te passen.

#### <a name="install-centos-66"></a>6.6 CentOS installeren

1.  De minimale 6.6 CentOS-besturingssysteem installeren op de server voor documentbeheer VM. De ISO in een DVD-station en start het systeem. Overslaan het testen van media, Amerikaans Engels op de taal selecteren, selecteert u **Basic opslagapparaten**, controleren of de vaste schijf niet hebben van alle belangrijke gegevens en klikt u op **Ja**gegevens Voer de hostnaam van de server en selecteer de netwerkadapter van de server.  In het dialoogvenster **Bewerken systeem** selecteert** automatisch een verbinding maken** en toevoegen van een statische IP-adres, netwerk- en DNS-instellingen. Geef een tijdzone en een root wachtwoord voor toegang tot de server. 
2.  U wordt gevraagd het type installatie wilt u **Maakt aangepaste lay-out** selecteren als de partitie. Nadat u op **volgende** **vrije** selecteren en klik op maken. Maak **/**, **/var/crash** en **-partities thuis** met **FS Type:** **ext4**. Maken van de swap partion als **FS Type: swap**.
3.  Als bestaande apparaten zijn gevonden een waarschuwingsbericht wordt weergegeven. Klik op **Opmaak** om de schijf met de partitie-instellingen te maken. Klik op **wijzigen naar schijf schrijven** om de wijzigingen van de partitie.
4.  Selecteer **installatie boot loader** > **volgende** de opstartlader op de root-partitie te installeren.
5.  Klik op **opnieuw opstarten**als installatie voltooid is.


#### <a name="retrieve-the-scsi-ids"></a>De SCSI-id's ophalen

1. Na de installatie de SCSI-id's voor de SCSI-vaste schijven in de VM te halen. Deze afsluiten van de server VM doet in de VM-eigenschappen in VMware met de rechtermuisknop op de vermelding VM > **Instellingen bewerken** > **Opties**.
2. Selecteer **Geavanceerd** > **algemene** en **Configuratieparameters**op. Met deze optie worden de-active wanneer de machine wordt uitgevoerd. Om deze te activeren moet u de computer afsluiten.
3. Als de rij **schijf. EnableUUID** bestaat Zorg ervoor dat de waarde is ingesteld op **True** (hoofdlettergevoelig). Als het al kunt u annuleren en testen van de SCSI-opdracht binnen een gastbesturingssysteem nadat deze opgestart. 
4.  Als de rij niet bestaande **Rij toevoegen** : klik op en toe te voegen met de waarde **True** . Gebruik geen dubbele aanhalingstekens.

#### <a name="install-additional-packages"></a>Extra pakketten installeren

U moet enkele aanvullende pakketten downloaden en installeren. 

1.  Controleer of dat de master doelserver is verbonden met het internet.
2.  Deze opdracht uitvoeren om te downloaden en installeren van 15 pakketten uit de opslagplaats CentOS: **# yum-y xfsprogs perl lsscsi rsync wget kexec-hulpprogramma's installeren**.
3.  Als de bron machines u beveiligen met Linux wit Reiser of XFS-bestandssysteem voor de hoofdmap of boot-apparaat en vervolgens downloaden en installeren van extra pakketten als volgt:

    - # <a name="cd-usrlocal"></a>cd /usr/local
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>reiserfs in de rpm-ivh-kmod-reiserfs-0,0-1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>rpm-ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Aangepaste wijzigingen toepassen

Voer de volgende wijzigingen toepassen nadat u hebt de stappen na de installatie en de pakketten geïnstalleerd:

1.  De 6-64 RHEL Unified Agent binaire kopiëren naar de VM. Deze opdracht uitvoeren om het binaire bestand untar: **tar-zxvf <file name> **
2.  Uitvoeren van deze opdracht machtigingen geven: **# type chmod 755-./ApplyCustomChanges.sh**
3.  Voer het script: **./ApplyCustomChanges.sh #**. U moet het script slechts één keer uitvoeren. Start de server opnieuw op nadat het script met succes is uitgevoerd.


## <a name="run-the-failback"></a>De failback-bewerking uitvoeren

### <a name="reprotect-the-azure-vms"></a>De Azure VMs reprotect

1.  In de portal-Site herstellen > tabblad **computers** selecteren de VM die storing opgetreden en klik op **opnieuw te beveiligen**.
2.  Selecteer de basispagina doelserver op gebouwen en de server Azure VM proces in **Master doelserver** en **Process Server** .
3.  Selecteer de account die u hebt geconfigureerd voor verbinding met de VM.
4.  Selecteer de versie van de failback van de groep bescherming. Voor het voorbeeld als de VM is beveiligd in PG1 en u moet selecteren PG1_Failback.
5.  Als u herstellen naar een andere locatie wilt, selecteert u het behoud station en geconfigureerd voor de doelserver master datastore. Wanneer u niet terug naar de website van gebouwen de VMs VMware in het plan failback bescherming wordt het gegevensarchief dezelfde als de doelserver master gebruikt. Als u wilt herstellen van de replica Azure VM naar dezelfde op-premises VM dan de VM op ruimten moet al in het gegevensarchief hetzelfde als de master-doelserver. Als er geen VM op ruimten wordt een nieuw gemaakt tijdens de reprotection.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Nadat u op begint **OK** om te beginnen met reprotection een project Azure VM gerepliceerd op de site van gebouwen. Klik op het tabblad **taken** kunt u de voortgang bijhouden.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Een failover uitvoeren naar de site van gebouwen

Na reprotection de VM wordt verplaatst naar de versie voor failback van de groep voor de bescherming en wordt automatisch toegevoegd aan het herstelplan dat u voor de failover naar Azure gebruikt als er een.

1.  In de pagina **Herstel plannen** het herstelplan dat de groep failback bevat selecteert en klikt u op **Failover** > **Niet-geplande Failover**.
2.  Controleer of de failover-richting (naar Azure) in **Failover bevestigen** en selecteer het herstelpunt dat u wilt gebruiken voor de failover (meest recente). Als u **Meerdere VM** hebt ingeschakeld tijdens het configureren van replicatie-eigenschappen kunt u met de nieuwste app of crash consistent herstelpunt herstellen. Klik op het vinkje om te beginnen de failover-functie.
3.  Tijdens een overname Site herstel het VMs Azure afgesloten. Nadat u hebt gecontroleerd dat failback is voltooid zoals verwacht u kunt kunt u controleren dat het VMs Azure afgesloten zoals verwacht.

### <a name="reprotect-the--on-premises-site"></a>De site van lokalen reprotect

Nadat de failback-bewerking is voltooid uw gegevens terug op de site van gebouwen worden echter won't worden beschermd. Repliceren naar Azure starten opnieuw het volgende doen:

1.  In de portal-Site herstellen > **Machines** tabblad selecteren het VMs die zijn mislukt back en klik op **opnieuw te beveiligen**. 
2.  Na het controleren van replicatie naar werkt Azure zoals verwacht in Azure kunt u de Azure VMs (niet actief) verwijderen die zijn niet terug.


### <a name="common-issues-in-failback"></a>Veelvoorkomende problemen in failback

1. Als u gebruikers alleen-lezen vCenter-discovery uitvoeren en beveiligen van virtuele machines, dit lukt en failover werkt. Op het moment van Reprotect zal mislukken omdat de datastores kan niet worden gedetecteerd. Als een probleem ziet u niet de datastores en beschermt opnieuw weergegeven. U kunt dit oplossen door kunt u de vCenter referentie bijwerken met de juiste account die machtigingen heeft en probeer opnieuw de taak. [Lees meer](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Wanneer u failback-bewerking een Linux VM en uit te voeren op prem, ziet u dat het pakket Network Manager is verwijderd van de computer. Dit is omdat wanneer de VM in Azure is hersteld, de Network Manager-pakket moet worden verwijderd.
3. Wanneer een VM is geconfigureerd met statische IP-adres en Azure is overgenomen, wordt het IP-adres verkregen via DHCP. Wanneer u weer in op prem failover, blijft de VM via DHCP het IP-adres verkrijgen. U moet handmatig de aanmelding in de computer en het IP-adres terug naar statisch adres indien nodig.
4. Als u gratis editie ESXi 5.5 of 6 vSphere Hypervisor gratis editie failover zou slagen, maar failback-bewerking mislukt. U zal ned te upgraden naar een evaluatielicentie failback inschakelen.

## <a name="failing-back-with-expressroute"></a>Beschadigde back met ExpressRoute

U kunt niet terug via een VPN-verbinding of Azure ExpressRoute. Als u ExpressRoute opmerking het volgende gebruiken wilt:

- ExpressRoute moet worden ingesteld op de Azure virtueel netwerk aan welke bron computers mislukken via en in welke VMs Azure staan nadat de failover plaatsvindt.
- Gegevens worden gerepliceerd naar een account Azure opslag op een openbare eindpunt. U moet een openbare peering in ExpressRoute met het doel Datacenter voor replicatie van Site-Recovery ExpressRoute gebruiken instellen.



