<properties
    pageTitle="Maken en uploaden van een Linux CentOS gebaseerde VHD in Azure"
    description="Informatie over maken en uploaden van een Azure virtuele harde schijf (VHD) met een op basis van CentOS Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
        tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Voorbereiden op een virtuele machine onder CentOS Azure

- [Voorbereiden op een virtuele machine voor CentOS 6.x Azure](#centos-6x)
- [Voorbereiden op een virtuele machine voor CentOS 7.0 + Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten ##

In dit artikel wordt ervan uitgegaan dat u al een CentOS hebt geïnstalleerd (of soortgelijke afgeleide) Linux-besturingssysteem op een virtuele harde schijf. Meerdere hulpprogramma's bestaan voor het maken van VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie de [installatie van de functie Hyper-V en virtuele machines configureren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.


**Opmerkingen bij de installatie centOS**

- Zie ook [Algemene opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar met Hyper-V-beheer of de cmdlet converteren vhd VHD-indeling.

- Bij de installatie van het Linux-systeem moet u ook standaard partities in plaats van LVM (vaak standaard voor vele installaties). Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen.  [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven als de voorkeur.

- NUMA wordt niet ondersteund voor grotere VM vanwege een fout in de versies van de Linux kernel onder 2.6.37. Dit probleem heeft voornamelijk gevolgen voor distributies met de upstream Red Hat 2.6.32 kernel. Handmatige installatie van Linux Azure agent (waagent) uitschakelt automatisch NUMA in de GRUB-configuratie voor de Linux-kernel. Meer informatie hierover vindt u in de onderstaande stappen.

- Configureer een swap partitie op de schijf voor het OS. Maakt een wisselbestand op de tijdelijke resource schijf kan de Linux-agent worden geconfigureerd.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3. NetworkManager verwijderen met de volgende opdracht:

        # sudo rpm -e --nodeps NetworkManager

    **Opmerking:** Als het pakket nog niet is geïnstalleerd, mislukt deze opdracht met een foutbericht weergegeven. Dit is normaal.

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

6.  Udev regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules


7. Controleer dat de netwerkservice wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on


8. **Alleen centOS 6.3**: Installeer de stuurprogramma's voor Linux Integration Services (LIS).

    **Belangrijk: De stap is alleen geldig voor CentOS 6.3 en eerdere versies.**  De integratieservices Linux zijn CentOS 6.4 + *al beschikbaar in de standard-kernel*.

    - Installatie-instructies op de [downloadpagina van de LIS](https://www.microsoft.com/en-us/download/details.aspx?id=46842) en de RPM naar uw image te installeren.  


9. Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

10. Als u gebruiken de OpenLogic mirrors die worden gehost binnen de Azure datacenters wilt, vervolgens vervangen door het bestand /etc/yum.repos.d/CentOS-Base.repo de volgende opslagplaatsen.  Dit wordt ook de **[openlogic]** -opslagplaats waarin de pakketten voor de agent Azure Linux toevoegen:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Opmerking:** De rest van deze handleiding wordt uitgegaan dat u gebruikmaakt van ten minste de repo [openlogic], die wordt gebruikt voor de onderstaande Azure Linux-agent installeren.


11. Voeg de volgende regel aan /etc/yum.conf:

        http_caching=packages

    En voeg de volgende regel **op CentOS 6.3 alleen** :

        exclude=kernel*

12. De module yum "fastestmirror" uitschakelen door het bewerken van het bestand "/ etc/yum/pluginconf.d/fastestmirror.conf", en onder `[main]`, typt u het volgende:

        set enabled=0

13. Voer de volgende opdracht om de metagegevens van het huidige yum wissen:

        # yum clean all

14. **Op CentOS 6.3 alleen**bijwerken van de kernel met de volgende opdracht:

        # sudo yum --disableexcludes=all install kernel

15. De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiervoor ' / boot/grub/menu.lst ' in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ook Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Vanwege een fout in de kernelversie die wordt gebruikt door CentOS 6 wordt NUMA uitgeschakeld.

    Naast het bovenstaande wordt aangeraden voor het *verwijderen van* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De `crashkernel` de optie is geconfigureerd als links, maar de opmerking dat deze parameter zal verminderen de hoeveelheid beschikbaar geheugen in de VM 128MB of meer, die mogelijk problemen op de kleinere VM.


16. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

17. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent

    Houd er rekening mee dat de installatie van het pakket WALinuxAgent u de NetworkManager verwijdert en NetworkManager-gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 2.

18. Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Let op de schijf van de lokale resource is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Klik op **actie -> sluiten af** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Wijzigingen in CentOS 7 (en soortgelijke producten)**

Voorbereiden van een CentOS 7 virtuele machine voor Azure is zeer vergelijkbaar zijn met CentOS 6, maar er zijn echter enkele belangrijke verschillen opgemerkt:

 - De NetworkManager pakket geen conflicten meer veroorzaakt met de agent Azure Linux. Dit pakket wordt standaard geïnstalleerd en wordt aangeraden niet is verwijderd.
 - GRUB2 wordt nu gebruikt als de standaard bootloader, zodat de procedure voor het bewerken van kernel parameters is gewijzigd (Zie hieronder).
 - XFS is nu het standaardbestandssysteem. De ext4-bestandssysteem kan nog steeds worden gebruikt als gewenst.


**Configuratiestappen**

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

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

6.  Udev regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Controleer dat de netwerkservice wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

7. Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

8. Als u gebruiken de OpenLogic mirrors die worden gehost binnen de Azure datacenters wilt, vervolgens vervangen door het bestand /etc/yum.repos.d/CentOS-Base.repo de volgende opslagplaatsen.  Dit wordt ook de **[openlogic]** -opslagplaats waarin de pakketten voor de agent Azure Linux toevoegen:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Opmerking:** De rest van deze handleiding wordt uitgegaan dat u gebruikmaakt van ten minste de repo [openlogic], die wordt gebruikt voor de onderstaande Azure Linux-agent installeren.

9.  Voer de volgende opdracht schakelt u de huidige metagegevens van yum en eventuele updates installeren:

        # sudo yum clean all
        # sudo yum -y update

10. De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Hiervoor open "/ etc/standaard/grub" in een teksteditor en bewerken de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Ook Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Ook blijkt uit de nieuwe naamconventies CentOS 7 voor netwerkkaarten. Naast het bovenstaande wordt aangeraden voor het *verwijderen van* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De `crashkernel` de optie is geconfigureerd als links, maar de opmerking dat deze parameter zal verminderen de hoeveelheid beschikbaar geheugen in de VM 128MB of meer, die mogelijk problemen op de kleinere VM.

11. Zodra u klaar bent met bewerken "/ etc/standaard/wormgaten" per hierboven, voer de volgende opdracht de configuratie wormgaten opnieuw samen te stellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

13. **Alleen als de afbeelding van VMWare, VirtualBox of KVM-:** Hyper-V-modules toevoegen aan initramfs:

    Bewerken `/etc/dracut.conf`, inhoud toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    De initramfs opnieuw:

        # dracut –f -v

14. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Let op de schijf van de lokale resource is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klik op **actie -> sluiten af** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U kunt nu uw CentOS Linux virtuele harde schijf gebruikt voor het maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u het VHD-bestand naar Azure uploadt, Zie stappen 2 en 3 in het [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
