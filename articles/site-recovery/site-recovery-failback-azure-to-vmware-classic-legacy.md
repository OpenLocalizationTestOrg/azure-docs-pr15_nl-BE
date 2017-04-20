<properties 
   pageTitle="Fouten terug VMware virtuele machines en fysieke servers in Azure, VMware (verouderd) | Microsoft Azure" 
   description="In dit artikel wordt beschreven hoe een VMware virtuele machine die wordt gerepliceerd naar Azure met Azure Site herstel failback." 
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
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Fouten terug VMware virtuele machines en fysieke servers in Azure, VMware met Azure Site herstel (verouderd)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure klassieke Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klassieke Portal (verouderd)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe terug VMware virtuele machines en fysieke servers Windows/Linux vanaf Azure naar uw site op gebouwen mislukken nadat u hebt gerepliceerd van uw site op ruimten naar Azure.

>[AZURE.NOTE] Dit artikel beschrijft een oudere scenario. U moet alleen de instructies in dit artikel gebruiken als u gerepliceerd naar [deze oude](site-recovery-vmware-to-azure-classic-legacy.md)instructies Azure. Als u een replicatie met behulp van de [verbeterde implementatie](site-recovery-vmware-to-azure-classic-legacy.md) Volg de instructies in [dit artikel](site-recovery-failback-azure-to-vmware-classic.md) voor de failback-bewerking. 


## <a name="architecture"></a>Architectuur

In dit diagram vertegenwoordigt het failover- en failback-scenario. De blauwe lijnen zijn de verbindingen gebruikt tijdens een overname. De rode lijnen zijn de verbindingen gebruikt tijdens een failback-bewerking. De lijnen met pijlen gaan via het internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Voordat u begint 

- U moet via de VMs VMware of fysieke servers zijn mislukt en moeten ze worden uitgevoerd in Azure.
- Houd er rekening mee dat u kunt alleen niet terug VMware virtuele machines en fysieke servers Windows/Linux vanaf Azure VMware virtuele machines in de primaire site op gebouwen.  Als u weer een fysieke machine mislukken bent, failover naar Azure zal converteren naar een Azure VM en failback VMware zal converteren naar een VM VMware.

Hier ziet u hoe u failback ingesteld:

1. **Onderdelen voor failback instellen**: moet u een vContinuum-server instellen op gebouwen en wijs de configuratieserver VM in Azure. U zult ook een processerver instellen als een Azure VM om gegevens te verzenden naar de server op locatie master doel. U registreren proces met de configuratieserver die failover worden verwerkt. U installeert een basispagina op gebouwen-doelserver. Als u een basispagina doelserver Windows wordt deze automatisch ingesteld tijdens de installatie van vContinuum. Als u Linux moet u deze functie handmatig instellen op een afzonderlijke server.
2. **Bescherming en failback inschakelen**: nadat u de onderdelen hebt ingesteld, in de vContinuum moet u beveiliging inschakelen voor failover Azure VMs. U zult uitgevoerd een gereedheidscontrole van het VMs en een failover van Azure naar uw site op gebouwen. Als failback is voltooid reprotect machines op gebouwen zodat deze repliceren naar Azure beginnen.



## <a name="step-1-install-vcontinuum-on-premises"></a>Stap 1: Installeer vContinuum op locatie

U moet een vContinuum-server installeren op locatie en wijs de configuratieserver.

1.  [VContinuum downloaden](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Download de versie [vContinuum bijwerken](http://go.microsoft.com/fwlink/?LinkID=533813) .
3. Installeer de nieuwste versie van vContinuum. Klik op **de welkomstpagina** op **volgende**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Geef op de eerste pagina van de wizard het IP-adres van de CX-server en poort van de CX. Selecteer **HTTPS gebruiken**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  De server configureren IP-adres vinden op het tabblad **Dashboard** van de configuratieserver VM in Azure.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  De configuratieserver openbare HTTPS-poort op het tabblad **eindpunten** van de configuratieserver VM in Azure vinden.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Geef op de pagina **Details van CS wachtwoordzin** de wachtwoordzin die u hebt genoteerd omlaag als u de van configuratieserver geregistreerd. Als u niet meer weet het inchecken **C:\\Program Files (x86)\\InMage-systemen\\particuliere\\connection.passphrase** op de configuratieserver VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  Op de pagina **Selecteer doellocatie** opgeven waar u de vContinuum-server installeren en klik op **installeren**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Nadat de installatie is voltooid, kunt u vContinuum starten.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Stap 2: Installeer een processerver in Azure 

U moet een werkende server installeren in Azure zodat het VMs in Azure de gegevens naar een server op locatie master doel verzenden kunnen. 

1.  Selecteer op de pagina **Configuratie van Servers** in Azure, een nieuw processerver toevoegen.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Geef een processerver naam, en een gebruikersnaam en wachtwoord verbinding maakt met de virtuele machine als beheerder. Selecteer de configuratieserver die u wilt de processerver registreren. Dit moet dezelfde server die u gebruikt voor het beschermen en niet via de virtuele machines.
3.  Geef de Azure netwerk waarin de processerver moet worden geïmplementeerd. Hetzelfde netwerk als de configuratieserver moet worden. Geef een uniek IP-adres geselecteerd subnet en implementatie begint.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Een taak wordt geactiveerd voor de implementatie van de processerver.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Nadat de processerver wordt geïmplementeerd in Azure kunt u zich aanmelden bij de server met de referenties die u hebt opgegeven. De processerver registreren op dezelfde manier als die u de server op locatie proces geregistreerd. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] De servers die zijn geregistreerd tijdens de failback-bewerking niet zichtbaar onder VM-eigenschappen bij het herstellen van de Site. Ze zijn alleen zichtbaar op het tabblad **Servers** van de van configuratieserver waarop ze zijn geregistreerd. Het kost ongeveer 10-15 minuten, totdat ze de processerver wordt weergegeven op het tabblad.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Stap 3: Installeer een master doelserver in gebouwen

Afhankelijk van uw bronbesturingssysteem voor virtuele machines, moet u een Linux installeren of een master doelserver Windows op gebouwen.

### <a name="deploy-a-windows-master-target-server"></a>Een doel master Windows server implementeren

Een master doel van Windows is al gebundeld met de installatie van vContinuum. Tijdens de installatie van de vContinuum is ook een masterserver op dezelfde computer zijn geïnstalleerd en geregistreerd op de server configureren.

1.  Als u wilt implementeren, maakt u een lege machine op gebouwen op de ESX-host die u wilt herstellen het VMs Azure.

2.  Zorg ervoor dat er ten minste twee schijven die zijn aangesloten op de VM: wordt gebruikt voor het besturingssysteem en de andere wordt gebruikt voor het behoud station.

3.  Het besturingssysteem installeren.

4.  De vContinuum op de server installeren. Hiermee voltooit u ook de installatie van de master-doelserver.

### <a name="deploy-a-linux-master-target-server"></a>Een master doel Linux server implementeren

1.  Als u wilt implementeren, maakt u een lege machine op gebouwen op de ESX-host die u wilt herstellen het VMs Azure.

2.  Zorg ervoor dat er ten minste twee schijven die zijn aangesloten op de VM: wordt gebruikt voor het besturingssysteem en de andere wordt gebruikt voor het behoud station.

3.  Het Linux-besturingssysteem installeren. De master doelsysteem Linux gebruik niet LVM voor opslagruimtes root of niet. Een master-doelserver van Linux is standaard geconfigureerd om te voorkomen dat de LVM partities/schijven detecteren.
4.  De partities die u kunt maken:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Verrichten de installatiestappen hieronder boeken voordat u begint met het doel master-installatie.


#### <a name="post-os-installation-steps"></a>OS installatiestappen boeken

Inschakelen als u de SCSI-id's voor elke SCSI-vaste schijf in een virtuele machine van Linux, de parameter 'schijf. EnableUUID = TRUE ' als volgt:

1. Sluit de virtuele machine.
2. Klik met de rechtermuisknop op de VM-vermelding in het deelvenster links > **Instellingen bewerken**.
3. Klik op het tabblad **Opties** . Selecteer **Geavanceerd\>algemene** > **-Configuratieparameters**. De **Parameters voor de configuratie** -optie is alleen beschikbaar wanneer de computer wordt afgesloten.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Controleert of een rij met **schijf. EnableUUID** bestaat. Als deze is ingesteld op **False** en vervolgens deze ingesteld op **True** (niet hoofdlettergevoelig). Als bestaat en is ingesteld op true, klikt u op **Annuleren** en testen van de SCSI-opdracht in het gastbesturingssysteem nadat deze opgestart omhoog. Als bestaat niet klikt u op de **Rij toevoegen**.
5. Schijf toevoegen. EnableUUID in de kolom **naam** . Hiermee stelt u de waarde waar. Voeg niet de bovenstaande waarden met dubbele aanhalingstekens.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>De extra pakketten downloaden en installeren

Opmerking: Zorg ervoor dat het systeem verbinding heeft met internet voordat u downloaden en installeren van de aanvullende pakketten.

\#YUM -y xfsprogs perl lsscsi rsync wget kexec-hulpprogramma's installeren

Met deze opdracht deze 15 pakketten van CentOS 6.6 opslagplaats gedownload en geïnstalleerd:

BC-1.06.95-1.el6.x86\_64. rpm

busybox-1.15.1-20.el6.x86\_64. rpm

elfutils libs-0.158 3.2.el6.x86\_64. rpm

kexec-tools-2.0.0-280.el6.x86\_64. rpm

lsscsi-0.23-2.el6.x86\_64. rpm

lzo-2.03-3.1.el6\_5.1.x86\_64. rpm

Perl-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-Module-Pluggable-3.90-136.el6\_6.1.x86\_64. rpm

Perl-Pod-Hiermee heft-trilling 1,04-136.el6\_6.1.x86\_64. rpm
    
Perl-Pod-eenvoudig-3.13-136.el6\_6.1.x86\_64. rpm

Perl-libs-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl versie-0,77 136.el6\_6.1.x86\_64. rpm

rsync-3.0.6-12.el6.x86\_64. rpm

treffende 1.1.0-1.el6.x86\_64. rpm

wget-1.12-5.el6\_6.1.x86\_64. rpm

Opmerking: Als de broncomputer Reiser of XFS-bestandssysteem voor de hoofdmap of boot-apparaat gebruikt, vervolgens volgende pakketten moeten worden gedownload en geïnstalleerd op de master doel Linux voor bescherming.

\#cd /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#rpm - ivh kmod-reiserfs-0,0-1.el6.elrepo.x86\_64. rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. rpm

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#rpm - ivh xfsprogs-3.1.1-16.el6.x86\_64. rpm

#### <a name="apply-custom-configuration-changes"></a>Wijzigingen in de aangepaste configuratie toepassen

Zorg ervoor dat u de vorige sectie hebt uitgevoerd voordat u deze wijzigingen toepast en vervolgens als volgt te werk:


1. De 6-64 RHEL Unified Agent binaire kopiëren naar het nieuwe besturingssysteem.

2. Deze opdracht uitvoeren om het binaire bestand untar: **tar - zxvf \<bestandsnaam\> **

3. Uitvoeren van deze opdracht machtigingen te geven: \# **type chmod 755-./ApplyCustomChanges.sh**

4. Voer het script: ** \# ./ApplyCustomChanges.sh**. Slechts één keer het script op de server uitvoeren. Start de server opnieuw op nadat het script wordt uitgevoerd.



### <a name="install-the-linux-server"></a>De Linux-server installeren


1. [Download](http://go.microsoft.com/fwlink/?LinkID=529757) het installatiebestand.
2. Kopieer het bestand naar de doel Master Linux virtuele machine met een sftp client-hulpprogramma van uw keuze. Ook melden bij de virtuele machine Linux master doel en wget downloaden van het installatiepakket van de onderstaande koppeling te gebruiken
3. Zich aanmeldt bij de Linux target master server virtuele machine met behulp van een ssh client van uw keuze
4. Als u verbonden bent met het Azure netwerk waarin u de doelserver van uw Linux via een VPN-verbinding geïmplementeerd gebruikt u het interne IP-adres voor de server die u kunt vinden in het **Dashboard** -tabblad virtuele machine en poort 22 verbinding maken met de master doel Linux Server met behulp van Secure Shell.
5. Als u verbinding met de doelserver voor Linux master via een openbare internetverbinding gebruikt de Linux master doelserver van openbare virtueel IP-adres (uit het tabblad **Dashboard** van virtuele machines) en het eindpunt openbaar gemaakt voor ssh aanmelden bij de Linux-servder.
6. De bestanden extraheren uit het tar-archief voor gzipped Linux target master Server installer door te voeren: *' tar-xvzf Microsoft ASR\_UA\_8.2.0.0\_RHEL6 64\ "* uit de map met het installatiebestand.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Als u het installatieprogramma bestanden naar een andere map wijzigen in de map uitgepakt die de inhoud van de tar archiveren zijn geëxtraheerd. In dit pad worden uitgevoerd "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Wanneer u wordt gevraagd te kiezen Selecteer een primaire rol **2 (model doel)**. De interactieve installatie-opties op hun standaardwaarden laten staan.
9. Wachten op de installatie voortgezet en de Host-configuratie-Interface moet worden weergegeven. Het hulpprogramma Configuratie van de Host voor de master sarget Linux Server is een hulpprogramma voor de opdrachtregel. Niet het formaat van de ssh hulpprogramma clientvenster. Gebruik de pijltoetsen om de **globale** optie en druk op ENTER op uw toetsenbord.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. Voer in het veld **IP** het interne IP-adres van de configuratieserver (u vindt deze op het tabblad **Dashboard** van de VM-configuratieserver) en druk op ENTER. In de **haven** **22** en op ENTER drukt. 
11.  Laat u **HTTPS gebruikt** als **Ja**en druk op ENTER.
12.  De wachtwoordzin invoeren die is gegenereerd op de server configureren. Als u een client STOPVERF van een Windows-computer met ssh naar de virtuele machine van Linux kunt u Shift + Insert om de inhoud van het Klembord plakken. De wachtwoordzin kopiëren naar het lokale Klembord met Ctrl + C en plakken met Shift + Insert. Druk op ENTER.
13.  Pijl-rechts gebruiken om te navigeren om te sluiten en druk op ENTER. Wachten op de installatie te voltooien.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Als om welke reden u kan niet registreren de doelserver van uw Linux op de server configureren kunt u dit doen door de host hulpprogramma voor systeemconfiguratie uit /usr/local/ASR/Vx/bin/hostconfigcli (u moet eerst toegangsmachtigingen naar deze map type chmod als een super-gebruiker uitvoert).


#### <a name="validate-master-target-registration"></a>Target master inschrijving te valideren

U kunt valideren dat de master doelserver geregistreerd op de configuratieserver in Azure Site herstel kluis > **Server Configuration** > **-Gegevens**.

>[AZURE.NOTE] Na de registratie van de master-doelserver als er fouten in de configuratie die de virtuele machine is mogelijk verwijderd uit de Azure of eindpunten zijn niet goed geconfigureerd, is dit omdat Hoewel de master doelconfiguratie wordt gedetecteerd door de Azure dndpoints wanneer het doel van de master in Azure is geïmplementeerd, dit geldt niet voor een in de lokalen target master server op-ruimten. Dit geen invloed op de failback-bewerking en u kunt deze fouten negeren. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Stap 4: Bescherming van de virtuele machines naar de site van gebouwen

U moet de VMs naar de site van lokalen beschermen voordat u failback.

### <a name="before-you-begin"></a>Voordat u begint

Een VM met Azure storing is opgetreden, wordt een extra tijdelijke station voor paginabestand toegevoegd. Dit is een extra station dat is meestal niet vereist door de mislukte via VM aangezien mogelijk al een station dat is toegewezen voor het wisselbestand. Voordat u reverse-bescherming van de virtuele machines, moet u ervoor zorgen dat deze schijf off line wordt genomen, zodat deze wordt niet beveiligd. Dit als volgt:

1.  Open Computerbeheer en selecteer opslagbeheer zodat er een lijst met de schijven on line en op de computer aangesloten.
2.  Selecteer de tijdelijke schijf is aangesloten op de computer en kies off line brengt. 

### <a name="protect-the-vms"></a>Het VMs beschermen

1. Controleer de Staten van de virtuele machine om ervoor te zorgen dat ze nu niet via in het portal voor Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. VContinuum op de computer wordt gestart.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. Klik op **Nieuwe bescherming** en selecteer het type bewerking systeem de 

4.  In het nieuwe venster dat open, het **type besturingssysteem**selecteren > **Details ophalen** voor de gewenste failback VMs. In de **primaire Serverdetails**, identificeren en selecteren van de virtuele machines die u wilt beveiligen. VMs worden vermeld onder de hostnaam van vCenter die ze op voordat failover wordt uitgevoerd zijn.
5.  Wanneer u een virtuele machine te beschermen (en dat al niet is via Azure) biedt een pop-upvenster twee vermeldingen voor de virtuele machine. Dit komt omdat de configuratieserver twee exemplaren van de virtuele machines geregistreerd detecteert. U moet de vermelding voor de VM op ruimten verwijderen zodat u de juiste VM kunt beveiligen. U kunt zich aanmelden bij de Azure VM en Ga naar C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc voor de juiste Azure VM-post hier. In het bestand drscout.conf, identificatie van de host-ID. Houd de post waarvoor u de host-ID op de VM gevonden in het dialoogvenster vContinuum. Alle andere items verwijderen. Selecteer de juiste VM u naar het IP-adres verwijzen kunt. Het IP-adres bereik op-ruimten worden de VM op gebouwen.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Stop de virtuele machine op de vCenter server. U kunt ook de VMs op ruimten verwijderen.
7. Geef vervolgens de op ruimten MT-server die u wilt beschermen het VMs. U doet dit door verbinding maken met de vCenter server waarop u failback.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Selecteer de basispagina doelserver die is gebaseerd op de host die u wilt herstellen van de VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Bieden de replicatie-optie voor elk van de virtuele machines. Hiervoor moet u selecteren het herstel kant gegevensarchief waarop het VMs wordt hersteld. De tabel bevat een overzicht van de verschillende opties die u moet voor elke VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Optie** | **Optie waarde aanbevolen**
    ---|---
    IP-adres van het proces | Selecteer het processerver geïmplementeerd in Azure
    Behoud grootte in MB| 
    Waarde | 1
    Dagen/uur | Dagen
    Interval voor consistentie | 1
    Selecteer doel datastore | Het gegevensarchief beschikbaar op de site van herstel. De data store moet voldoende ruimte hebben en beschikbaar zijn voor de ESX-host die u wilt herstellen van de virtuele machine.


10. Configureer de eigenschappen die de virtuele machine na een failover naar de site voor gebouwen krijgen. De eigenschappen zijn samengevat in de volgende tabel.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Eigenschap** | **Details**
    ---|---
    Netwerkconfiguratie| Selecteren en klik op **wijzigen** om het failback-IP-adres voor de virtuele machine configureren voor elke gevonden netwerkadapter. 
    Configuratie van hardware| Geef de CPU en het geheugen voor de VM. Instellingen kunnen worden toegepast op alle VMs die u wilt beveiligen. U kunt verwijzen naar de grootte van de rol van VMs IAAS en Zie het aantal cores en geheugen toegewezen voor de juiste waarden voor de CPU en het geheugen.
    Weergavenaam | Na de failback van gebouwen naam kunt u de virtuele machines in de voorraad vCenter zullen zien. De standaardnaam is de naam van de virtuele machine computer host. Voor de naam van de VM kunt u verwijzen naar de VM-lijst in de groep bescherming.
    NAT-configuratie | Hieronder uitvoerig besproken

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>NAT-instellingen configureren

1. Als u wilt dat de bescherming van de virtuele machines, moeten twee communicatiekanalen tot stand worden gebracht. Het eerste kanaal is tussen de virtuele machine en de processerver. Dit kanaal worden de gegevens van de VM verzameld en verzonden naar de processerver de gegevens naar de doelserver master stuurt. Als de processerver en de virtuele machine worden beschermd in Azure hetzelfde virtuele netwerk dan hoeft u niet te NAT-instellingen gebruiken. Anders NAT-instellingen opgeven. Het openbare IP-adres van de processerver bekijken in Azure. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Het tweede kanaal is tussen de processerver en de doelserver master. De optie voor het gebruik van NAT of niet hangt af van of u met behulp van een op basis van VPN-verbinding of via het internet communiceren. Selecteer niet NAt als u gebruikmaakt van VPN, maar alleen als u een internet-verbinding.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Als u de op het bedrijf virtuele machines zoals is opgegeven niet hebt verwijderd en als het lukt u niet terug op niet-gegevensarchief de oude VMDK bevat moet u ervoor zorgen dat de failback-bewerking die VM wordt gemaakt op een nieuwe plaats. Hiervoor selecteert u de instellingen voor **Geavanceerd** en geef een andere map in de **Map de naam instellingen**herstellen. Laat de andere opties met de standaardinstellingen. De map naam instellingen toepassen op alle servers.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Voer een Readiness controleren om ervoor te zorgen dat de virtuele machines klaar om te worden beschermd zijn op ruimten naar. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Wacht om te voltooien. Als het bestand met succes op alle VMs kunt u een naam voor het plan bescherming. Klik op **beveiligen** om te beginnen.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. U kunt de voortgang in vContinuum kunt controleren.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. Na het **Activeren van bescherming van Plan** is voltooid stap kunt u controleren VM bescherming in de portal-Site terugzetten.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Hier ziet u de exacte status op de VM te klikken en controleren van de voortgang.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Het failback-plan opstellen

U kunt een failback-plan met vContinuum, zodat de toepassing kan worden niet terug naar de website van gebouwen op elk gewenst moment voorbereiden. Deze herstelplannen zijn vergelijkbaar met de herstelplannen bij het herstellen van de Site.

1.  VContinuum starten en selecteer **beheren plannen** > **herstellen.** U kunt zien van de lijst van de plannen waarmee VMs failover. U kunt de dezelfde plannen te herstellen.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Selecteer het plan voor de bescherming en alle van de VMs die u wilt herstellen in deze. Als u elke VM ziet u meer details met inbegrip van het doel ESX-server en de brondiskette VM. Klik op **volgende** om te beginnen met de Wizard herstellen en selecteer het VMs die u wilt herstellen.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Kunt u herstellen op basis van meerdere opties, maar het is raadzaam u **Nieuwste Tag** en selecteer **toepassen voor alle VMs** om ervoor te zorgen dat de meest recente gegevens van de virtuele machine wordt gebruikt.
4. Voer de **gereedheidscontrole.** Dit wordt gecontroleerd als de juiste parameters worden geconfigureerd VM herstellen. Klik op **volgende** als u alle controles worden uitgevoerd. Als dat niet het logbestand controleren en de fouten op te lossen.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  Zorg ervoor dat de Systeemherstel-instellingen juist zijn ingesteld in **VM configuratie** . Als u wilt, kunt u de VM-instellingen wijzigen.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Bekijk de lijst met virtuele machines die wordt hersteld en geeft een invorderingsopdracht. Opmerking VMs worden weergegeven met behulp van de hostnaam van de computer. Het kan lastig zijn om de hostnaam van de computer worden toegewezen aan de virtuele machine.
De namen toewijzen, gaat u naar de virtuele machines **Dashboard** in Azure en controleert u de hostnaam van de VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Geef een naam en selecteer **later herstellen**. We raden je aan het later herstellen nadat de eerste bescherming mogelijk niet voltooid. 
11. Klik op **herstellen** op te slaan van het plan of het herstel worden geactiveerd als u nu en niet later herstellen hebt geselecteerd. U kunt controleren de status herstellen als u wilt zien als het programma is opgeslagen.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Herstellen van virtuele machines

Nadat de het plan hebt gemaakt, kunt u de virtuele machines te herstellen. Voordat u inschakelt dat de virtuele machines synchronisatie hebt voltooid. Als de replicatiestatus OK bescherming is voltooid en de drempel voor de vrijgegeven Productieorder is voldaan. U kunt controleren of de volksgezondheid in de VM-eigenschappen.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

De Azure virtuele machines uitschakelen voordat u de herstelconsole starten. Hierdoor is er geen hersenen splitsen en dat gebruikers alleen toegang hebben tot één exemplaar van de toepassing. 


1.  Start het programma opgeslagen. Selecteer **Monitor** de plannen in de vContinuum. Hier worden de plannen die zijn uitgevoerd.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  Bij **het herstellen van** het plan en klik op **starten**. U kunt Systeemherstel kunt controleren. Nadat het VMs is ingeschakeld op kunt u ze in vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Bescherming van Azure na opnieuw failback

Nadat de failback-bewerking is voltooid zult u waarschijnlijk opnieuw de virtuele machines beveiligen. Dit als volgt:

1.  Controleer of de virtuele machines op ruimten werkt en die toepassingen kunnen worden bereikt.
2.  Selecteer de virtuele machines in de portal Azure Site herstellen en verwijder deze. Selecteer uitschakelen van de bescherming van de virtuele machines. Dit zorgt ervoor dat de VMs niet meer beschermd zijn.
3.  In Azure verwijderen de mislukte over Azure virtuele machines
4.  Verwijder de oude virtuele machine op vSpehere. Dit zijn de VMs die u eerder niet via Azure konden.
5.  Beschermen de VMs die onlangs overgenomen heeft in de portal-Site terugzetten. Nadat zij beschermd bent kunt u ze toevoegen aan een herstelplan.
 
## <a name="next-steps"></a>Volgende stappen



- [Meer informatie over](site-recovery-vmware-to-azure-classic.md) VMware virtuele machines en fysieke servers worden gerepliceerd naar Azure met behulp van de verbeterde implementatie.

 
