<properties
pageTitle="Voorbereiden van een Oracle Linux virtuele Machine voor Azure | Microsoft Azure"
description="Stap voor stap de configuratie van een Oracle-virtuele machine waarop Linux wordt uitgevoerd in Microsoft Azure."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Voorbereiden van een Oracle Linux virtuele machine voor Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Voorbereiden op een virtuele machine van Oracle Linux 6.4 + Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Voorbereiden op een virtuele machine van Oracle Linux 7.0 + Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al een Oracle Linux-besturingssysteem hebt geïnstalleerd op een virtuele harde schijf. Meerdere hulpprogramma's bestaan voor het maken van VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie voor meer informatie [Hyper-V installeren en het maken van een virtuele machine](http://technet.microsoft.com/library/hh846766.aspx).

**Opmerkingen bij de installatie Oracle Linux**

- Oracle van Red Hat compatibel kernel en de UEK3 (Unbreakable Enterprise Kernel) worden beide ondersteund in Hyper-V en Azure. Voor de beste resultaten moet u aan de nieuwste kernel tijdens het voorbereiden van uw Oracle Linux VHD.

- UEK2 van Oracle wordt niet ondersteund in Hyper-V en Azure als niet de vereiste stuurprogramma's bevat.

- De nieuwere VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet converteren vhd.

- Wanneer u het Linux-systeem installeert, wordt u aangeraden standaard partities in plaats van LVM (vaak standaard voor vele installaties) te gebruiken. Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen. LVM of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven als de voorkeur.

- NUMA wordt niet ondersteund voor grotere VM vanwege een fout in de versies van de Linux kernel onder 2.6.37. Dit probleem heeft voornamelijk gevolgen voor distributies die gebruikmaken van de upstream Red Hat 2.6.32 kernel. Handmatige installatie van Linux Azure agent (waagent) uitschakelt automatisch NUMA in de GRUB-configuratie voor de Linux-kernel. Meer informatie hierover vindt u in de onderstaande stappen.

- Configureer een swap partitie op de schijf voor het OS. Maakt een wisselbestand op de tijdelijke resource schijf kan de Linux-agent worden geconfigureerd. Meer informatie hierover vindt u in de onderstaande stappen.

- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.

- Zorg ervoor dat de `Addons` bibliotheek is ingeschakeld. Kiest u voor het bewerken van het bestand `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), en wijzig de regel `enabled=0` te `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
U moet stappen specifieke configuratie van het besturingssysteem voor de virtuele machine uit te voeren in Azure.

1. Selecteer in het middenvenster van Hyper-V-beheer de virtuele machine.

2. Klik op **verbinding maken** om het te openen voor de virtuele machine.

3. NetworkManager verwijderen met de volgende opdracht:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Als het pakket nog niet is geïnstalleerd, mislukt deze opdracht met een foutbericht weergegeven. Dit is normaal.

4. Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Maak een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Udev regels om te voorkomen dat statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Azure of Hyper-V: kloont

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # chkconfig network on

8.  Python-pyasn1 installeren door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

9.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiervoor ' / boot/grub/menu.lst ' in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dit zal er ook voor zorgen dat alle console berichten worden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Vanwege een fout in Oracle van Red Hat compatibel kernel wordt NUMA uitgeschakeld.

    Behalve het bovenstaande, raden wij aan dat u *verwijdert* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De `crashkernel` de optie is geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op de kleinere VM zijn.

10.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is gewoonlijk de standaardinstelling.

11.  De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum installeren WALinuxAgent

    Houd er rekening mee dat de installatie van het pakket WALinuxAgent u de NetworkManager verwijdert en NetworkManager-gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 2.

12.  Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM na het inrichten op Azure. Houd er rekening mee dat de schijf van de lokale resource een *tijdelijke* schijf is en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Opmerking: deze optie instellen op wat u nodig hebt om te worden.

13.  Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - identiteitsgegevens
        # <a name="export-histsize0"></a>exporteren van HISTSIZE = 0
        # <a name="logout"></a>logout

14.  Klik op **actie -\> afgesloten** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Wijzigingen in de Oracle Linux 7**

Voorbereiden van een Oracle Linux 7 virtuele machine voor Azure is vergelijkbaar met het proces voor Oracle Linux 6. Er zijn echter enkele belangrijke verschillen opgemerkt:

-   Zowel de compatibel kernel Red Hat en Oracle van UEK3 worden ondersteund in Azure. Wij raden aan de kernel UEK3.

-   De NetworkManager pakket geen conflicten meer veroorzaakt met de agent Azure Linux. Dit pakket wordt standaard geïnstalleerd en wordt aangeraden niet is verwijderd.

-   GRUB2 wordt nu gebruikt als de standaard bootloader, zodat de procedure voor het bewerken van kernel parameters is gewijzigd (Zie hieronder).

-   XFS is nu het standaardbestandssysteem. De ext4-bestandssysteem kan nog steeds worden gebruikt als gewenst.

**Configuratiestappen**

1.  Selecteer de virtuele machine in Hyper-V-beheer.

2.  Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3.  Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Maak een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Udev regels om te voorkomen dat statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Microsoft Azure of Hyper-V kloont.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Zorg ervoor dat de NetworkService wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

7.  Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

8.  Voer de volgende opdracht schakelt u de huidige metagegevens van yum en eventuele updates installeren:

        # sudo yum clean all
        # sudo yum -y update

9.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Hiervoor open "/ etc/standaard/grub" in een teksteditor en bewerken van de GRUB\_CMDLINE\_LINUX-parameter, bijvoorbeeld:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Ook Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Behalve het bovenstaande, raden wij aan dat u *verwijdert* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De `crashkernel` de optie is geconfigureerd als gewenst links, maar de opmerking dat de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt beperkt door deze parameter. Dit kan problemen veroorzaken op de kleinere VM zijn.

10.  Zodra u klaar bent met bewerken "/ etc/standaard/grub", de volgende opdracht de configuratie wormgaten opnieuw samen te stellen:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2 mkconfig - o /boot/grub2/grub.cfg

11.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is gewoonlijk de standaardinstelling.

12.  De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum installeren WALinuxAgent

13.  Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte automatisch configureren met behulp van de lokale resource schijf die is gekoppeld aan de VM na het inrichten op Azure. Houd er rekening mee dat de schijf van de lokale resource een *tijdelijke* schijf is en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Opmerking: deze optie instellen op wat u nodig hebt om te worden.

14.  Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - identiteitsgegevens
        # <a name="export-histsize0"></a>exporteren van HISTSIZE = 0
        # <a name="logout"></a>logout

15.  Klik op **actie -\> afgesloten** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.
