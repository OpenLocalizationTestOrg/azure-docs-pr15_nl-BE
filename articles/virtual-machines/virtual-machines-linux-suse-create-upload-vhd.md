<properties
    pageTitle="Maken en uploaden van een VHD SUSE Linux in Azure"
    description="Informatie over maken en uploaden van een Azure virtuele harde schijf (VHD) met een SUSE Linux-besturingssysteem."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Een virtuele machine van SLES of openSUSE voorbereiden voor Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten ##

In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd een SUSE of openSUSE Linux-besturingssysteem op een virtuele harde schijf. Meerdere hulpprogramma's bestaan voor het maken van VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie de [installatie van de functie Hyper-V en virtuele machines configureren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE Installatieopmerkingen

- Zie ook [Algemene opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar met Hyper-V-beheer of de cmdlet converteren vhd VHD-indeling.

- Bij de installatie van het Linux-systeem moet u ook standaard partities in plaats van LVM (vaak standaard voor vele installaties). Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere VM voor het oplossen van problemen. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven als de voorkeur.

- Configureer een swap partitie op de schijf voor het OS. Maakt een wisselbestand op de tijdelijke resource schijf kan de Linux-agent worden geconfigureerd.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.


## <a name="use-suse-studio"></a>SUSE Studio gebruiken
[SUSE Studio](http://www.susestudio.com) kunt gemakkelijk maken en beheren van uw afbeeldingen SLES en openSUSE voor Hyper-V en Azure. Dit is de aanbevolen methode voor het aanpassen van uw eigen afbeeldingen SLES en openSUSE.

Als alternatief voor het bouwen van uw eigen VHD publiceert SUSE ook afbeeldingen BYOS (uw eigen abonnement brengen) voor SLES op [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 voorbereiden ##

1. Selecteer in het middenvenster van Hyper-V-beheer de virtuele machine.

2. Klik op **verbinding maken** om het te openen voor de virtuele machine.

3. Registreer uw systeem SUSE Linux Enterprise als deze updates downloaden en installeren van pakketten.

4. Het systeem bijwerken met de nieuwste patches:

        # sudo zypper update

5. De Agent Azure Linux te installeren vanaf de opslagplaats SLES:

        # sudo zypper install WALinuxAgent

6. Chkconfig inchecken als waagent is ingesteld op 'aan' en zo niet, deze inschakelen voor automatisch starten:
               
        # sudo chkconfig waagent on

7. Controleer of de waagent-service wordt uitgevoerd en als dat niet het geval is, start: 

        # sudo service waagent start
                
8. De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Deze open doen "/ boot/grub/menu.lst" in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen.

9. Bevestig dat /boot/grub/menu.lst en /etc/fstab beide verwijzen naar de schijf met de UUID (door uuid) in plaats van de schijf-ID (door-id). 

    UUID schijf ophalen
    
        # ls /dev/disk/by-uuid/

    Als /dev/disk/by-id is zowel /boot/grub/menu.lst als/etc/fstab gebruikt, worden bijgewerkt met de juiste door uuid-waarde

    Voor wijziging
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Na wijziging
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Udev regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Het wordt aanbevolen het bestand '/ etc/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` parameter voor het volgende:

        DHCLIENT_SET_HOSTNAME="no"

12. In "/ etc/sudoers", uitschakelen of verwijderen de volgende regels als deze bestaan:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

14. Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Let op de schijf van de lokale resource is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klik op **actie -> sluiten af** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.


----------

## <a name="prepare-opensuse-131"></a>OpenSUSE 13,1 + voorbereiden ##

1. Selecteer in het middenvenster van Hyper-V-beheer de virtuele machine.

2. Klik op **verbinding maken** om het te openen voor de virtuele machine.

3. Op de shell, voert u de opdracht '`zypper lr`'. Als deze opdracht wordt uitvoer weergegeven als de volgende en vervolgens de opslagplaatsen worden geconfigureerd zoals verwacht--geen aanpassingen nodig zijn (Houd er rekening mee dat de versienummers kan verschillen):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Als de opdracht geeft "Geen opslagplaatsen gedefinieerd..." gebruikt u de volgende opdrachten toe te voegen deze repo's:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    U kunt controleren de opslagplaatsen zijn toegevoegd met de opdracht '`zypper lr`' opnieuw. In het geval een van de opslagplaatsen van de betreffende update niet is ingeschakeld, wordt dit inschakelen met de volgende opdracht:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. De kernel update naar de nieuwste versie:

        # sudo zypper up kernel-default

    Of het systeem wilt bijwerken met de nieuwste patches:

        # sudo zypper update

5.  De Azure Linux-Agent te installeren.

        # sudo zypper install WALinuxAgent

6.  De kernel boot lijn in uw configuratie wormgaten aanvullende kernel parameters opnemen voor Azure wijzigen. Open hiervoor ' / boot/grub/menu.lst ' in een teksteditor en controleer of de standaard kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen. Bovendien de volgende parameters van de kernel boot lijn verwijderen als deze bestaan:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Het wordt aanbevolen het bestand '/ etc/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` parameter voor het volgende:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Belangrijk:** In "/ etc/sudoers", uitschakelen of verwijderen de volgende regels als deze bestaan:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

10. Maak geen swap ruimte op de schijf voor het OS.

    De Agent Azure Linux kunt swap ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Let op de schijf van de lokale resource is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Voer de volgende opdrachten voor deprovision van de virtuele machine en voorbereiden op Azure-aanbod:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Controleer dat de Azure Linux-Agent wordt uitgevoerd bij het opstarten:

        # sudo systemctl enable waagent.service

13. Klik op **actie -> sluiten af** in Hyper-V-beheer. De VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U kunt nu uw SUSE Linux virtuele harde schijf gebruikt voor het maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u het VHD-bestand naar Azure uploadt, Zie stappen 2 en 3 in het [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
