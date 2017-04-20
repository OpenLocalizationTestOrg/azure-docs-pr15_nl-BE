<properties
    pageTitle="Debian Linux VHD voorbereiden | Microsoft Azure"
    description="Informatie over het maken van Debian 7 & 8 VHD-bestanden voor implementatie in Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Voorbereiden op een Debian VHD Azure

## <a name="prerequisites"></a>Vereisten
In dit gedeelte wordt ervan uitgegaan dat u al een Debian Linux-besturingssysteem hebt geïnstalleerd vanaf een ISO-bestand gedownload vanaf de [website van Debian](https://www.debian.org/distrib/) op een virtuele harde schijf. Er zijn meerdere hulpprogramma's Maak VHD-bestanden; Hyper-V is slechts één voorbeeld. Zie [installeren van de functie Hyper-V en virtuele machines configureren](https://technet.microsoft.com/library/hh846766.aspx)voor instructies over het gebruik van Hyper-V.


## <a name="installation-notes"></a>Opmerkingen bij de installatie

- Zie ook [Algemene opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.
- De nieuwere VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar met Hyper-V-beheer of de cmdlet **converteren vhd** VHD-indeling.
- Bij de installatie van het Linux-systeem moet u ook standaard partities in plaats van LVM (vaak standaard voor vele installaties). Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven als de voorkeur.
- Configureer een swap partitie op de schijf voor het OS. Maakt een wisselbestand op de schijf van de tijdelijke resource kan de Azure Linux-agent worden geconfigureerd. Meer informatie hierover vindt u in de onderstaande stappen.
- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Debian VHD's maken gebruik van Azure beheren

Er zijn hulpprogramma's beschikbaar voor het genereren van VHD Azure, Debian's, zoals de [azure-beheren](https://github.com/credativ/azure-manage) scripts van [credativ](http://www.credativ.com/). Dit is de aanbevolen werkwijze versus een afbeelding helemaal zelf maken. Bijvoorbeeld voor het maken van een Debian 8 VHD Voer de volgende opdrachten voor het downloaden van azure-beheren (en afhankelijkheden) en voer het script azure_build_image:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Handmatig een Debian VHD voorbereiden

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3. De regel voor **deb cdrom** in commentaar `/etc/apt/source.list` als u de VM tegen een ISO-bestand ingesteld.

4. Bewerkt de `/etc/default/grub` -bestand en wijzig de **GRUB_CMDLINE_LINUX** parameter als volgt aanvullende kernel parameters bevatten voor Azure.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. De wormgaten bouwen en uitvoeren:

        # sudo update-grub

6. Azure opslagplaatsen van Debian toevoegen aan /etc/apt/sources.list voor Debian 7 of 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. De Azure Linux-Agent te installeren:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 7 is vereist voor het uitvoeren van de kernel op basis van 3,16 uit de opslagplaats wheezy backports. Maak eerst een bestand met de naam /etc/apt/preferences.d/linux.pref met de volgende inhoud:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Voer "sudo apt-get install linux-image-amd64" de nieuwe kernel te installeren.

8. Deprovision van de virtuele machine en voorbereiden op Azure inrichten en uitvoeren:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Klik op **actie** -afsluiten > omlaag in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.


## <a name="next-steps"></a>Volgende stappen

U kunt nu uw Debian virtuele harde schijf gebruikt voor het maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u het VHD-bestand naar Azure uploadt, Zie stappen 2 en 3 in het [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
