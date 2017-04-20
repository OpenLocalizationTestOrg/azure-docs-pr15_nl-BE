<properties
    pageTitle="Maken en uploaden van een Ubuntu Linux VHD in Azure"
    description="Informatie over maken en uploaden van een Azure virtuele harde schijf (VHD) met een Ubuntu Linux-besturingssysteem."
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
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Voorbereiden op een virtuele machine van Ubuntu Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Officiële Ubuntu wolk afbeeldingen
Ubuntu publiceert nu officiële Azure VHD's downloaden vanaf [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Als u uw eigen gespecialiseerde Ubuntu-image samenstellen voor Azure, veeleer dan handmatig de volgende procedure gebruiken verdient om te starten met deze bekende VHD's werken en zo nodig aanpassen. De meest recente versies van de afbeelding kunnen altijd worden gevonden op de volgende locaties:

 - Ubuntu 12.04/nauwkeurig: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/betrouwbare: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al een Ubuntu Linux-besturingssysteem hebt geïnstalleerd op een virtuele harde schijf. Meerdere hulpprogramma's bestaan voor het maken van VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie de [installatie van de functie Hyper-V en virtuele machines configureren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.

**Opmerkingen bij de installatie Ubuntu**

- Zie ook [Algemene opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.
- De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar met Hyper-V-beheer of de cmdlet converteren vhd VHD-indeling.
- Bij de installatie van het Linux-systeem moet u ook standaard partities in plaats van LVM (vaak standaard voor vele installaties). Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven als de voorkeur.
- Configureer een swap partitie op de schijf voor het OS. Maakt een wisselbestand op de tijdelijke resource schijf kan de Linux-agent worden geconfigureerd.  Meer informatie hierover vindt u in de onderstaande stappen.
- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.


## <a name="manual-steps"></a>Handmatige stappen

> [AZURE.NOTE] Voordat u uw eigen aangepaste afbeelding van Ubuntu voor Azure, Overweeg de beelden van [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) in plaats daarvan.


1. Selecteer in het middenvenster van Hyper-V-beheer de virtuele machine.

2. Klik op **verbinding maken** om het te openen voor de virtuele machine.

3.  Vervang de huidige opslaglocaties in de afbeelding om het gebruik van Ubuntu Azure repo's. De stappen verschillen afhankelijk van de versie van Ubuntu.

    Voordat u /etc/apt/sources.list bewerkt, wordt aangeraden een back-up:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. De afbeeldingen Ubuntu Azure volgt nu de kernel *hardware medewerkers* (HWE). Het besturingssysteem bijwerken naar de meest recente kernel door de volgende opdrachten:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. De kernel boot lijn voor wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Om dit te doen open "/ etc/standaard/grub" in een teksteditor, vindt u de variabele met de naam `GRUB_CMDLINE_LINUX_DEFAULT` (of toe te voegen indien nodig) en bewerken om aan te nemen van de volgende parameters:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Opslaan en sluiten van dit bestand en voer '`sudo update-grub`'. Hierdoor worden dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure technische ondersteuning helpen kan bij het debuggen van problemen.

6.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

7.  De Azure Linux-Agent te installeren:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Houd er rekening mee dat u installeert de `walinuxagent` pakket verwijdert de `NetworkManager` en `NetworkManager-gnome` pakketten als ze zijn geïnstalleerd.

8.  Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klik op **actie -> sluiten af** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.


## <a name="next-steps"></a>Volgende stappen
U kunt nu uw Ubuntu Linux virtuele harde schijf gebruikt voor het maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u het VHD-bestand naar Azure uploadt, Zie stappen 2 en 3 in het [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Verwijzingen ##

Ubuntu hardware medewerkers (HWE) kernel:

- [http://blog.utlemming.org/2015/01/Ubuntu-1404-Azure-images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-Using-Hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
