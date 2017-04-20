<properties
    pageTitle="Maken en uploaden van een VHD van Red Hat Enterprise Linux voor gebruik in Azure"
    description="Informatie over maken en uploaden van een Azure virtuele harde schijf (VHD) met een Red Hat Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Voorbereiden op basis van Red Hat virtuele machines voor Azure

In dit artikel leert u hoe u een virtuele machine van Red Hat Enterprise Linux (RHEL) voorbereiden voor gebruik in Azure. Versies van RHEL die worden besproken in dit artikel zijn 6.7, 7.1 en 7.2. Hypervisors voor bereiding die worden besproken in dit artikel worden de Hyper-V, Kernel-gebaseerde Virtual Machine (KVM) en VMware. Zie voor meer informatie over de vereisten voor uw deelname aan de Red Hat's Cloud-programma in aanmerking komen, [Red Hat's Cloud toegang website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) en [RHEL uitgevoerd op Azure](https://access.redhat.com/articles/1989673).

[Een virtuele machine van 6,7 RHEL voorbereiden van Hyper-V-beheer](#rhel67hyperv)

[Voorbereiden van een virtuele machine van RHEL 7.1/7.2 van Hyper-V-beheer](#rhel7xhyperv)

[Voorbereiden van een virtuele machine van RHEL 6.7 van KVM](#rhel67kvm)

[Voorbereiden van een virtuele machine van RHEL 7.1/7.2 van KVM](#rhel7xkvm)

[Voorbereiden van een virtuele machine van RHEL 6.7 van VMware](#rhel67vmware)

[Voorbereiden van een virtuele machine van RHEL 7.1/7.2 van VMware](#rhel7xvmware)

[Een RHEL 7.1/7.2 virtuele machine uit een kickstart bestand voorbereiden](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Voorbereiden op basis van Red Hat virtuele machines van Hyper-V-beheer
### <a name="prerequisites"></a>Vereisten
In dit gedeelte wordt ervan uitgegaan dat u al hebt geïnstalleerd een afbeelding RHEL (van een ISO-bestand afkomstig is van de website van Red Hat) om een virtuele harde schijf (VHD). Zie [installeren van de functie Hyper-V en virtuele machines configureren](http://technet.microsoft.com/library/hh846766.aspx)voor meer informatie over het installeren van een installatiekopie van het besturingssysteem met Hyper-V-beheer.

**Opmerkingen bij de installatie RHEL**

- Zie ook [Algemene opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- De nieuwere VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de PowerShell-cmdlet **converteren vhd** .

- VHD's moeten worden gemaakt als de "vaste"--dynamische VHD's worden niet ondersteund.

- Wanneer u het Linux-systeem installeert, wordt u aangeraden standaard partities in plaats van LVM (vaak standaard voor vele installaties) te gebruiken. Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen. LVM of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven als de voorkeur.

- Configureer een swap partitie op de schijf voor het OS. U kunt de Linux-agent een wisselbestand op de tijdelijke resource-schijf maken. Meer informatie hierover is beschikbaar in de volgende stappen uit.

- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.

- Wanneer u met **qemu-img** schijfkopieën converteren naar VHD, u ziet dat een bekende fout in qemu-img versies 2.2.1 of hoger. Deze fout resulteert in een onjuiste indeling VHD. Het probleem is bedoeld om te worden opgelost in een toekomstige versie van qemu-img. Op dit moment wordt aangeraden dat u versie 2.2.0 qemu-img of eerder.

### <a id="rhel67hyperv"> </a>Bereiden een RHEL 6.7 virtuele machine vanuit Hyper-V-beheer###


1.  Selecteer de virtuele machine in Hyper-V-beheer.

2.  Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3.  NetworkManager verwijderen met de volgende opdracht:

        # sudo rpm -e --nodeps NetworkManager

    Houd er rekening mee dat als het pakket nog niet is geïnstalleerd, deze opdracht met een foutbericht weergegeven mislukt. Dit is normaal.

4.  Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` directory met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Maak een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Udev regels om te voorkomen dat statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

8.  Registreer uw abonnement op Red Hat om de installatie van pakketten uit de opslagplaats RHEL inschakelen door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Het pakket WALinuxAgent `WALinuxAgent-<version>` is de opslagplaats van Red Hat-extra's zijn geduwd. De extra's-bibliotheek inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiertoe `/boot/grub/menu.lst` in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dit zal er ook voor zorgen dat alle console berichten worden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Vanwege een fout in de kernelversie die wordt gebruikt door de 6 RHEL wordt NUMA uitgeschakeld.

    Naast de bovenstaande actie, raden we aan dat u de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De optie crashkernel kan worden geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op kleinere VM zijn.

11. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is gewoonlijk de standaardinstelling. /Etc/ssh/sshd_config als u wilt opnemen in de volgende regel wijzigen:

        ClientAliveInterval 180

12. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Houd er rekening mee dat de installatie van het pakket WALinuxAgent u de NetworkManager verwijdert en NetworkManager-gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 2.

13. Maak geen swap ruimte op de schijf voor het OS.
De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM nadat de VM in Azure is ingericht. Let op de schijf van de lokale resource is een tijdelijke schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie de vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Registratie (indien nodig) het abonnement met de volgende opdracht:

        # sudo subscription-manager unregister

15. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klik op **actie > afgesloten** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.
 

### <a id="rhel7xhyperv"> </a>Bereiden een RHEL 7.1/7.2 virtuele machine vanuit Hyper-V-beheer###

1.  Selecteer de virtuele machine in Hyper-V-beheer.

2.  Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3.  Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` directory met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Maak een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

6.  Registreer uw abonnement op Red Hat om de installatie van pakketten uit de opslagplaats RHEL inschakelen door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiertoe `/etc/default/grub` in een teksteditor en bewerken met de parameter **GRUB_CMDLINE_LINUX** . Bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dit zal er ook voor zorgen dat alle console berichten worden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Naast de bovenstaande actie, raden we aan dat u de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.
    De optie crashkernel kan worden geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op kleinere VM zijn.

8.  Nadat u klaar bent met bewerken `/etc/default/grub`, de volgende opdracht de configuratie wormgaten opnieuw samen te stellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is gewoonlijk de standaardinstelling. Wijzigen `/etc/ssh/sshd_config` naar de volgende regel bevatten:

        ClientAliveInterval 180

10. Het pakket WALinuxAgent `WALinuxAgent-<version>` is de opslagplaats van Red Hat-extra's zijn geduwd. De extra's-bibliotheek inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Maak geen swap ruimte op de schijf voor het OS. De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM nadat de VM in Azure is ingericht. Let op de schijf van de lokale resource is een tijdelijke schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie de vorige stap), wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste manier:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Als u wilt dat de registratie van het abonnement, kunt u de volgende opdracht uitvoeren:

        # sudo subscription-manager unregister

14. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klik op **actie > afgesloten** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Voorbereiden op basis van Red Hat virtuele machines van KVM

### <a id="rhel67kvm"> </a>Een RHEL 6.7 virtuele machine uit KVM voorbereiden###


1.  De KVM-afbeelding van 6,7 RHEL downloaden vanaf de website van Red Hat.

2.  Een root-wachtwoord instellen.

    Een gecodeerd wachtwoord genereren en de uitvoer van de opdracht kopiëren:

        # openssl passwd -1 changeme

    Wachtwoord instellen voor een hoofdmap met guestfish:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Wijzig het tweede veld van de hoofdgebruiker van "!!" met het gecodeerde wachtwoord.

3.  Een virtuele machine maken in KVM uit de afbeelding qcow2, het schijftype ingesteld op **qcow2**en het virtuele netwerk interface model ingesteld op **virtio**. Vervolgens start u de virtuele machine en log in als root.

4.  Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` directory met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Maak een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  De udev regels om te voorkomen dat statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # chkconfig network on

8.  Registreer uw abonnement op Red Hat om de installatie van pakketten uit de opslagplaats RHEL inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiertoe `/boot/grub/menu.lst` in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dit zal er ook voor zorgen dat alle console berichten worden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Vanwege een fout in de kernelversie die wordt gebruikt door de 6 RHEL wordt NUMA uitgeschakeld.

    Naast de bovenstaande actie, raden we aan dat u de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.
    De optie crashkernel is geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op kleinere VM zijn.

10. Hyper-V-modules toevoegen aan initramfs:  

    Bewerken `/etc/dracut.conf` en inhoud toevoegen: add_drivers += "hv_vmbus hv_netvsc hv_storvsc"

    Opnieuw initramfs: # dracut: f - v

11. Cloud-init verwijderen:

        # yum remove cloud-init

12. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten bij het opstarten:

        # chkconfig sshd on

    /Etc/ssh/sshd_config om op te nemen van de volgende regels wijzigen:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Sshd opnieuw:

        # service sshd restart

13. Het pakket WALinuxAgent `WALinuxAgent-<version>` is de opslagplaats van Red Hat-extra's zijn geduwd. De extra's-bibliotheek inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM nadat de VM in Azure is ingericht. Let op de schijf van de lokale resource is een tijdelijke schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie de vorige stap), de volgende parameters in de **/etc/waagent.conf** op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Registratie (indien nodig) het abonnement met de volgende opdracht:

        # subscription-manager unregister

17. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. De VM in KVM afgesloten.

19. Converteer de afbeelding qcow2 naar de VHD-indeling.
    De afbeelding eerst omzetten in raw-indeling:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd met 1 MB. Afronden anders de grootte aan te passen aan 1 MB: # MB = $((1024*1024)) # grootte = $(qemu-img info -f ruwe--json "rhel-6.7.raw" uitvoer | \ gawk ' overeenkomen met ($0, / 'virtuele grootte': ([0-9] +), /, val) {afdrukken val[1]}') # rounded_size = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    De ruwe schijf converteren naar een vaste grootte en VHD:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Een RHEL 7.1/7.2 virtuele machine uit KVM voorbereiden###


1.  De KVM-afbeelding of RHEL 7.1 (7.2) downloaden vanaf de website van Red Hat. Als in het voorbeeld hier gebruiken we RHEL 7.1.

2.  Een root-wachtwoord instellen.

    Een gecodeerd wachtwoord genereren en de uitvoer van de opdracht kopiëren:

        # openssl passwd -1 changeme

    Stel een root wachtwoord met guestfish.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Wijzig het tweede veld van de hoofdgebruiker van "!!" met het gecodeerde wachtwoord.

3.  Een virtuele machine maken in KVM uit de afbeelding qcow2, het schijftype ingesteld op **qcow2**en het virtuele netwerk interface model ingesteld op **virtio**. Vervolgens start u de virtuele machine en log in als root.

4.  Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` directory met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Maak een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # chkconfig network on

7.  Registreer uw abonnement op Red Hat om de installatie van pakketten uit de opslagplaats RHEL inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiertoe `/etc/default/grub` in een teksteditor en bewerken met de parameter **GRUB_CMDLINE_LINUX** . Bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dit zal er ook voor zorgen dat alle console berichten worden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Naast de bovenstaande actie, raden we aan dat u de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.
    De optie crashkernel kan worden geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op kleinere VM zijn.

9.  Nadat u klaar bent met bewerken `/etc/default/grub`, de volgende opdracht de configuratie wormgaten opnieuw samen te stellen:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Hyper-V-modules toevoegen aan initramfs:

    Bewerken `/etc/dracut.conf` en inhoud toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Initramfs opnieuw:

        # dracut –f -v

11. Cloud-init verwijderen:

        # yum remove cloud-init

12. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten bij het opstarten:

        # systemctl enable sshd

    /Etc/ssh/sshd_config om op te nemen van de volgende regels wijzigen:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Sshd opnieuw:

        systemctl restart sshd

13. Het pakket WALinuxAgent `WALinuxAgent-<version>` is de opslagplaats van Red Hat-extra's zijn geduwd. De extra's-bibliotheek inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # yum install WALinuxAgent

    De waagent-service inschakelen:

        # systemctl enable waagent.service

15. Maak geen swap ruimte op de schijf voor het OS. De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM nadat de VM in Azure is ingericht. Let op de schijf van de lokale resource is een tijdelijke schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie de vorige stap), wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste manier:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Registratie (indien nodig) het abonnement met de volgende opdracht:

        # subscription-manager unregister

17. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. De virtuele machine in KVM afgesloten.

19. Converteer de afbeelding qcow2 naar de VHD-indeling.

    De afbeelding eerst omzetten in raw-indeling:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd met 1 MB. Anders wordt de grootte aan te passen aan 1 MB afronden:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    De ruwe schijf converteren naar een vaste grootte en VHD:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Voorbereiden op basis van Red Hat virtuele machines van VMware
### <a name="prerequisites"></a>Vereisten
In dit gedeelte wordt ervan uitgegaan dat u al een RHEL virtual machine in VMware hebt geïnstalleerd. Zie voor meer informatie over het installeren van een besturingssysteem in VMware [VMware Gast Operating System Installation Guide](http://partnerweb.vmware.com/GOSIG/home.html).

- Wanneer u het besturingssysteem Linux installeert, wordt u aangeraden standaard partities in plaats van LVM (vaak standaard voor vele installaties) te gebruiken. Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen. LVM of RAID kan worden gebruikt op gegevensschijven als de voorkeur.

- Configureer een swap partitie op de schijf voor het OS. U kunt de Linux-agent een wisselbestand op de tijdelijke resource-schijf maken. Meer informatie hierover vindt u in de onderstaande stappen.

- Selecteer **de virtuele schijf opslaan als één bestand**bij het maken van de virtuele harde schijf.



### <a id="rhel67vmware"> </a>Een RHEL 6.7 virtuele machine voorbereiden van VMware###

1.  NetworkManager verwijderen met de volgende opdracht:

         # sudo rpm -e --nodeps NetworkManager

    Houd er rekening mee dat als het pakket nog niet is geïnstalleerd, deze opdracht met een foutbericht weergegeven mislukt. Dit is normaal.

2.  Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Maak een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  De udev regels om te voorkomen dat statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

6.  Registreer uw abonnement op Red Hat om de installatie van pakketten uit de opslagplaats RHEL inschakelen door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Het pakket WALinuxAgent `WALinuxAgent-<version>` is de opslagplaats van Red Hat-extra's zijn geduwd. De extra's-bibliotheek inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiervoor ' / boot/grub/menu.lst ' in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dit zal er ook voor zorgen dat alle console berichten worden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Vanwege een fout in de kernelversie die wordt gebruikt door de 6 RHEL wordt NUMA uitgeschakeld.
    Naast de bovenstaande actie, raden we aan dat u de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.
    De optie crashkernel kan worden geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op kleinere VM zijn.

9.  Hyper-V-modules toevoegen aan initramfs:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is gewoonlijk de standaardinstelling. Wijzigen `/etc/ssh/sshd_config` naar de volgende regel bevatten:

        ClientAliveInterval 180

11. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Maak geen swap ruimte op de schijf OS:

    De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM nadat de VM in Azure is ingericht. Let op de schijf van de lokale resource is een tijdelijke schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie de vorige stap), wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste manier:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Registratie (indien nodig) het abonnement met de volgende opdracht:

        # sudo subscription-manager unregister

14. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. De VM afgesloten en het VMDK bestand converteren naar een VHD-bestand.

    De afbeelding eerst omzetten in raw-indeling:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd met 1 MB. Anders wordt de grootte aan te passen aan 1 MB afronden:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    De ruwe schijf converteren naar een vaste grootte en VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Een RHEL 7.1/7.2 virtuele machine voorbereiden van VMware###

1.  Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Maak een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

4.  Registreer uw abonnement op Red Hat om de installatie van pakketten uit de opslagplaats RHEL inschakelen door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiertoe `/etc/default/grub` in een teksteditor en bewerken met de parameter **GRUB_CMDLINE_LINUX** . Bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dit zal er ook voor zorgen dat alle console berichten worden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Naast de bovenstaande actie, raden we aan dat u de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.
    De optie crashkernel kan worden geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op kleinere VM zijn.

6.  Nadat u klaar bent met bewerken `/etc/default/grub`, de volgende opdracht de configuratie wormgaten opnieuw samen te stellen:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Hyper-V-modules toevoegen aan initramfs:

    Bewerken `/etc/dracut.conf`, inhoud toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Initramfs opnieuw:

        # dracut –f -v

8.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is gewoonlijk de standaardinstelling. Wijzigen `/etc/ssh/sshd_config` naar de volgende regel bevatten:

        ClientAliveInterval 180

9.  Het pakket WALinuxAgent `WALinuxAgent-<version>` is de opslagplaats van Red Hat-extra's zijn geduwd. De extra's-bibliotheek inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Maak geen swap ruimte op de schijf voor het OS. De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM nadat de VM in Azure is ingericht. Let op de schijf van de lokale resource is een tijdelijke schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie de vorige stap), wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste manier:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Als u wilt dat de registratie van het abonnement, kunt u de volgende opdracht uitvoeren:

        # sudo subscription-manager unregister

13. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. De VM afgesloten en de VMDK bestand converteren naar de VHD-indeling.

    De afbeelding eerst omzetten in raw-indeling:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd met 1 MB. Anders wordt de grootte aan te passen aan 1 MB afronden:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    De ruwe schijf converteren naar een vaste grootte en VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Op basis van Red Hat virtuele machines van een ISO met behulp van een kickstart bestand automatisch voorbereiden


### <a id="rhel7xkickstart"> </a>Een RHEL 7.1/7.2 virtuele machine uit een kickstart bestand voorbereiden###


1.  Maak een kickstart met de inhoud hieronder en sla het bestand. Zie de [Installatiehandleiding Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html)voor meer informatie over de installatie van de kickstart.



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Plaats het kickstart bestand op een locatie die toegankelijk is vanaf de installatie-systeem.

3.  Maak een nieuwe VM in Hyper-V-beheer. Selecteer **later een virtuele harde schijf toevoegen**op de pagina **verbinding maken met virtuele harde schijf** en voltooien van de Wizard Nieuwe virtuele Machine.

4.  Open de VM instellingen:

    een.  Een nieuwe virtuele harde schijf voor de VM koppelen. Zorg ervoor dat het **VHD-indeling** en een **Vaste grootte**selecteren.

    b.  De ISO-installatie toevoegen aan het DVD-station.

    c.  Stel het BIOS vanaf cd-rom wilt opstarten.

5.  Start de VM. Druk in de installatiehandleiding op het **tabblad** voor het configureren van opties voor het opstarten.

6.  ENTER `inst.ks=<the location of the kickstart file>` aan het einde van de opties voor opstarten en druk op **Enter**.

7.  Wacht totdat de installatie te voltooien. Wanneer deze voltooid, wordt de VM automatisch afgesloten. De VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="known-issues"></a>Bekende problemen
Er zijn bekende problemen bij gebruik van RHEL 7.1 in Hyper-V en Azure.

### <a name="disk-io-freeze"></a>Schijf-i/o-blokkering

Dit probleem kan optreden tijdens de frequente opslag schijf-i/o-activiteiten met RHEL 7.1 in Hyper-V en Azure.   

Repro-tarief:

Dit probleem wordt onderbroken. Dit gebeurt echter vaak meer tijdens de frequente schijf i/o-bewerkingen in Hyper-V en Azure.   


[AZURE.NOTE] Dit bekende probleem is reeds behandeld door Red Hat. Installeert de bijbehorende correcties te voeren met de volgende opdracht:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>De Hyper-V-stuurprogramma kan niet worden opgenomen in de oorspronkelijke RAM-schijf wanneer u een niet-Hyper-V-hypervisor

In sommige gevallen kunnen Linux installers niet bevatten de stuurprogramma's voor Hyper-V in de oorspronkelijke RAM-schijf (initrd of initramfs) tenzij wordt vastgesteld dat wordt uitgevoerd in een Hyper-V-omgeving.

Wanneer u een andere virtualisatie systeem (dat wil zeggen Virtualbox, Xen, enz.) voor het voorbereiden van uw image Linux gebruikt, moet u mogelijk opnieuw initrd ervoor te zorgen dat ten minste de kernel-modules hv_vmbus en hv_storvsc zijn beschikbaar op de oorspronkelijke RAM-schijf. Dit is een bekend probleem ten minste op systemen die zijn gebaseerd op de upstream Red Hat-distributie.

Dit probleem op te lossen moet u Hyper-V-modules toevoegen aan initramfs en opnieuw maken:

Bewerken `/etc/dracut.conf` en inhoud toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Initramfs opnieuw:

        # dracut –f -v

Zie de informatie over het [initramfs opnieuw maken](https://access.redhat.com/solutions/1958)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
U kunt nu uw virtuele harde schijf voor Red Hat Enterprise Linux gebruiken voor het maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u het VHD-bestand naar Azure uploadt, Zie stappen 2 en 3 in het [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).

Voor meer informatie over de hypervisors die zijn gecertificeerd voor het uitvoeren van Red Hat Enterprise Linux, Zie [de website van Red Hat](https://access.redhat.com/certified-hypervisors).
