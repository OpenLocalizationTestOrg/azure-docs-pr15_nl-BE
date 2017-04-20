<properties
    pageTitle="Maken en uploaden van een Oracle Linux VHD | Microsoft Azure"
    description="Informatie over maken en uploaden van een Azure virtuele harde schijf (VHD) met een Oracle Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Voorbereiden van een Oracle Linux virtuele machine voor Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten ##

In dit artikel wordt ervan uitgegaan dat u al een Oracle Linux-besturingssysteem hebt geïnstalleerd op een virtuele harde schijf. Meerdere hulpprogramma's bestaan voor het maken van VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie de [installatie van de functie Hyper-V en virtuele machines configureren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.


### <a name="oracle-linux-installation-notes"></a>Opmerkingen bij de installatie Oracle Linux

- Zie ook [Algemene opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- Oracle van Red Hat compatibel kernel en hun UEK3 (Unbreakable Enterprise Kernel) worden beide ondersteund in Hyper-V en Azure. Voor de beste resultaten moet je bij te werken naar de nieuwste kernel tijdens het voorbereiden van uw Oracle Linux VHD.

- UEK2 van Oracle wordt niet ondersteund in Hyper-V en Azure als niet de vereiste stuurprogramma's bevat.

- De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar met Hyper-V-beheer of de cmdlet converteren vhd VHD-indeling.

- Bij de installatie van het Linux-systeem moet u ook standaard partities in plaats van LVM (vaak standaard voor vele installaties). Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven als de voorkeur.

- NUMA wordt niet ondersteund voor grotere VM vanwege een fout in de versies van de Linux kernel onder 2.6.37. Dit probleem heeft voornamelijk gevolgen voor distributies met de upstream Red Hat 2.6.32 kernel. Handmatige installatie van Linux Azure agent (waagent) uitschakelt automatisch NUMA in de GRUB-configuratie voor de Linux-kernel. Meer informatie hierover vindt u in de onderstaande stappen.

- Configureer een swap partitie op de schijf voor het OS. Maakt een wisselbestand op de tijdelijke resource schijf kan de Linux-agent worden geconfigureerd.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.

- Zorg ervoor dat de `Addons` bibliotheek is ingeschakeld. Bewerk het bestand `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), en wijzig de regel `enabled=0` te `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 + ##

U moet stappen specifieke configuratie van het besturingssysteem voor de virtuele machine uit te voeren in Azure.

1. Selecteer in het middenvenster van Hyper-V-beheer de virtuele machine.

2. Klik op **verbinding maken** om het te openen voor de virtuele machine.

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

        # chkconfig network on

8. Python-pyasn1 installeren door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

9.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Deze open doen "/ boot/grub/menu.lst" in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ook Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Vanwege een fout in Oracle van Red Hat compatibel kernel wordt NUMA uitgeschakeld.

    Naast het bovenstaande wordt aangeraden voor het *verwijderen van* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De `crashkernel` de optie is geconfigureerd als links, maar de opmerking dat deze parameter zal verminderen de hoeveelheid beschikbaar geheugen in de VM 128MB of meer, die mogelijk problemen op de kleinere VM.


10. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

11. De Agent Azure Linux installeren door de volgende opdracht uit te voeren. De meest recente versie is 2.0.15.

        # sudo yum install WALinuxAgent

    Houd er rekening mee dat de installatie van het pakket WALinuxAgent u de NetworkManager verwijdert en NetworkManager-gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 2.

12. Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Let op de schijf van de lokale resource is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klik op **actie -> sluiten af** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.


----------


## <a name="oracle-linux-70"></a>Oracle Linux 7.0 + ##

**Wijzigingen in de Oracle Linux 7**

Voorbereiden van een Oracle Linux 7 virtuele machine voor Azure is vergelijkbaar met Oracle Linux 6, maar er zijn echter enkele belangrijke verschillen opgemerkt:

 - Zowel de compatibel kernel Red Hat en Oracle van UEK3 worden ondersteund in Azure.  De kernel UEK3 wordt aanbevolen.
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

5.  Udev regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Controleer dat de netwerkservice wordt gestart tijdens het opstarten door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

7. Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

8.  Voer de volgende opdracht schakelt u de huidige metagegevens van yum en eventuele updates installeren:

        # sudo yum clean all
        # sudo yum -y update

9.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Om dit te doen open "/ etc/standaard/grub" in een teksteditor en bewerken de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Ook Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Ook blijkt uit de nieuwe naamconventies van de indicatie 7 voor netwerkkaarten. Naast het bovenstaande wordt aangeraden voor het *verwijderen van* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De `crashkernel` de optie is geconfigureerd als links, maar de opmerking dat deze parameter zal verminderen de hoeveelheid beschikbaar geheugen in de VM 128MB of meer, die mogelijk problemen op de kleinere VM.


10. Zodra u klaar bent met bewerken "/ etc/standaard/wormgaten" per hierboven, voer de volgende opdracht de configuratie wormgaten opnieuw samen te stellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

12. De Agent Azure Linux installeren door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Let op de schijf van de lokale resource is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie de vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klik op **actie -> sluiten af** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.


## <a name="next-steps"></a>Volgende stappen
U kunt nu uw VHD Oracle Linux gebruiken voor het maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u het VHD-bestand naar Azure uploadt, Zie stappen 2 en 3 in het [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
