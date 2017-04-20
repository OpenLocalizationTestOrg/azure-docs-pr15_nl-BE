<properties
    pageTitle="Maken en uploaden van een Linux VHD in Azure"
    description="Informatie over maken en uploaden van een Azure virtuele harde schijf (VHD) met een Linux-besturingssysteem."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Informatie voor niet-geviseerd verdelingen #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Belangrijk**: de Azure platform SLA is van toepassing op de virtuele machines met het Linux-besturingssysteem alleen als een van de [verdelingen geviseerd](virtual-machines-linux-endorsed-distros.md) wordt gebruikt. Alle Linux-distributies die beschikbaar zijn in de afbeeldinggalerie met Azure zijn geviseerd verdelingen met de vereiste configuratie.

- [Linux op Azure - geviseerd verdelingen](virtual-machines-linux-endorsed-distros.md)
- [Ondersteuning voor afbeeldingen in Microsoft Azure Linux](https://support.microsoft.com/kb/2941892)

Alle distributies op Azure moet voldoen aan een aantal vereisten voor een kans voor een correcte werking van het platform.  Dit artikel is in geen geval uitgebreid zoals elke distributie verschilt; en het is goed mogelijk dat zelfs als u aan de onderstaande criteria voldoen wel u aanzienlijk tweak uw Linux-systeem moet om ervoor te zorgen dat deze correct wordt uitgevoerd op het platform.

Het is daarom raadzaam dat u begint met een van onze [Linux op Azure geviseerd uitkeringen](virtual-machines-linux-endorsed-distros.md) indien mogelijk. De volgende artikelen begeleidt u bij het voorbereiden van de verschillende geviseerd Linux distributies die op Azure worden ondersteund:

- **[Op basis van centOS verdelingen](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

De rest van dit artikel zal zich richten op algemene richtlijnen voor het uitvoeren van de Linux-distributie op Azure.


## <a name="general-linux-installation-notes"></a>Linux algemene opmerkingen bij de installatie ##

- De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar met Hyper-V-beheer of de cmdlet converteren vhd VHD-indeling. Als u met behulp van VirtualBox betekent dit een **vaste grootte** selecteren in plaats van de standaard dynamisch toegewezen bij het maken van de schijf.

- Bij de installatie van het Linux-systeem is het *aanbevolen* standaard partities in plaats van LVM (vaak standaard voor vele installaties) te gebruiken. Dit vermijdt LVM conflicteert met gekloonde VMs, vooral als u een schijf OS ooit moet worden gekoppeld aan een andere identieke VM voor het oplossen van problemen. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) kan worden gebruikt op gegevensschijven.

- Kernel-ondersteuning voor de montage van UDF-bestandssysteem is vereist. Bij de eerste keer op Azure is de provisioning configuratie voor Linux VM doorgegeven via media UDF-indeling die is gekoppeld aan de Gast. De agent Azure Linux moet koppelen, het UDF-bestandssysteem lezen van de configuratie en de VM inrichten.

- NUMA ondersteund Linux kernel versies onder 2.6.37 niet in Hyper-V met grotere VM. Dit probleem voornamelijk effecten oudere distributies met de upstream kernel 2.6.32 van Red Hat, en is opgelost in RHEL 6.6 (kernel 2.6.32 504). Systemen met aangepaste kernels ouder dan 2.6.37, of op basis van RHEL kernels ouder dan 2.6.32-504 moet de parameter boot instellen `numa=off` op de opdrachtregel in grub.conf kernel. Zie Red Hat [KB 436883](https://access.redhat.com/solutions/436883)voor meer informatie.

- Configureer een swap partitie op de schijf voor het OS. Maakt een wisselbestand op de tijdelijke resource schijf kan de Linux-agent worden geconfigureerd.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle van de VHD moet grootten die veelvouden van 1 MB zijn hebben.


### <a name="installing-linux-without-hyper-v"></a>Linux zonder Hyper-V installeren ###

In sommige gevallen Linux installers eventueel de stuurprogramma's voor Hyper-V in de initiële ramdisk (initrd of initramfs) tenzij wordt vastgesteld dat deze wordt uitgevoerd een een Hyper-V-omgeving.  Wanneer u een andere virtualisatie systeem (dat wil zeggen Virtualbox, KVM, enz.) voor het voorbereiden van uw image Linux, moet u mogelijk opnieuw de initrd om ervoor te zorgen dat ten minste de `hv_vmbus` en `hv_storvsc` kernelmodules zijn beschikbaar op de initiële ramdisk.  Dit is een bekend probleem ten minste op systemen die zijn gebaseerd op de upstream Red Hat-distributie.

Het mechanisme voor het opnieuw samenstellen van de afbeelding initrd of initramfs kan variëren afhankelijk van de verdeling. Raadpleeg de documentatie van uw distributie of ondersteuning voor de juiste procedure.  Hier volgt een voorbeeld voor het opnieuw opbouwen van de initrd met behulp van de `mkinitrd` hulpprogramma:

Eerst een back-up van de bestaande initrd image:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Vervolgens opnieuw de initrd met de `hv_vmbus` en `hv_storvsc` kernelmodules:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>VHD's vergroten/verkleinen ###

VHD-installatiekopieën op Azure moet een virtuele grootte uitgelijnd op 1MB.  VHD's die zijn gemaakt met behulp van Hyper-V moeten normaal gesproken al correct zijn uitgelijnd.  Als u de VHD niet correct wordt uitgelijnd vervolgens verschijnt een foutbericht van de volgende strekking wanneer u probeert een *afbeelding* maken van de VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

U lost dit probleem kunt u het formaat van de VM met behulp van de Hyper-V Manager-console of de [Grootte van VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell-cmdlet.  Als u niet in een Windows-omgeving wordt aanbevolen qemu-img te converteren (indien nodig) en het formaat van de VHD.

> [AZURE.NOTE] Er is een bekende bug in qemu-img versies > = 2.2.1 die tot een onjuiste indeling VHD leidt. Het probleem is opgelost in QEMU 2.6. Het wordt qemu-img 2.2.0 of lager gebruiken, of bijwerken naar 2.6 of hoger aanbevolen. Referentie: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Formaat van de VHD rechtstreeks gebruik van hulpprogramma's zoals `qemu-img` of `vbox-manage` kan leiden tot een VHD opgestart.  Daarom wordt het aanbevolen om eerst converteren de VHD naar een RAW-schijfkopie.  Als de afbeelding VM al is gemaakt als RAW-schijfkopie (de standaardinstelling voor sommige Hypervisors zoals KVM) kunt u deze stap overslaan:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. De vereiste grootte van de schijfkopie om ervoor te zorgen dat de virtuele grootte wordt uitgelijnd op 1MB berekenen.  De volgende bash shell-script kan met dit helpen.  Het script maakt gebruik van "`qemu-img info`" om te bepalen van de virtuele grootte van de schijfkopie en berekent de grootte op de volgende 1MB:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Het formaat van de ruwe schijf met behulp van $rounded_size zoals in het bovenstaande script:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Nu de ruwe schijf weer converteren naar een vaste grootte VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Vereisten voor Linux-Kernel ##

De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure worden rechtstreeks op de upstream-Linux kernel bijgedragen. Veel distributies met een recente versie van Linux kernel (dat wil zeggen 3.x) al deze stuurprogramma's beschikbaar zijn, of anders backported versies van deze stuurprogramma's bieden hun kernels.  Deze stuurprogramma's worden voortdurend bijgewerkt in de upstream kernel met nieuwe oplossingen en functies, zodat u indien mogelijk verdient het uitvoeren van een [distributie onderschreven](virtual-machines-linux-endorsed-distros.md) waarin deze correcties en updates.

Als u werkt met een variant van Red Hat Enterprise Linux versies **6.0 6.3**, moet u de nieuwste stuurprogramma's voor LIS voor Hyper-V installeren. De stuurprogramma's vindt u [op deze locatie](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL **6.4 +** (en afgeleiden) de LIS-stuurprogramma's zijn al opgenomen in de kernel en dus geen extra installatiepakketten nodig zijn voor het uitvoeren van deze systemen op Azure.

Als u een aangepaste kernel vereist is, verdient het gebruik van een meer recente kernelversie (dat wil zeggen **3,8 +**). Voor deze verdelingen of leveranciers die hun eigen kernel onderhouden moeite worden regelmatig backport moet met de LIS-stuurprogramma's van de upstream kernel aan uw aangepaste kernel.  Zelfs als u al een relatief recente kernelversie uitvoert, is het nadrukkelijk aanbevolen om bij te houden in de LIS-stuurprogramma's en backport upstream fixes die zo nodig. De locatie van de bronbestanden van de LIS-stuurprogramma is beschikbaar in het bestand [zullen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) in de Linux kernel source tree:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Het ontbreken van de volgende patches om problemen te veroorzaken op Azure bekend is bij een zeer minimale en dat deze moeten worden opgenomen in de kernel. Deze lijst is geenszins limitatief of voltooid voor alle distributies:

- [ata_piix: schijven de Hyper-V-stuurprogramma's standaard uitstellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: Account voor pakketten in transit in het pad opnieuw instellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: gebruik van WRITE_SAME voorkomen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: hetzelfde schrijven voor RAID en virtuele host netwerkadapterstuurprogramma's uitschakelen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: NULL-pointer dereferentie fix](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: ring buffer storingen kunnen resulteren in een i/o-blokkering](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: bescherming tegen dubbele uitvoering van __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>De Azure Linux-Agent ##

[Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) (waagent) is een virtuele Linux machine in Azure goed inrichten vereist. U kunt de meest recente versie, problemen met bestanden opvragen of pull-aanvragen op de [Linux-Agent GitHub repo](https://github.com/Azure/WALinuxAgent)indienen.

- De Linux-agent is vrijgegeven onder de Apache 2.0 licentie. Veel distributies al bieden RPM of deb pakketten voor de agent, en dus in sommige gevallen dit kan worden geïnstalleerd en bijgewerkt met weinig moeite.

- De Agent Azure Linux moet Python v2.6 +.

- De agent moet ook de python-pyasn1-module. De meeste distributies bieden dit als een afzonderlijk pakket dat kan worden geïnstalleerd.

- In sommige gevallen de Agent Azure Linux mogelijk niet compatibel met de NetworkManager. Veel van de RPM/Deb-pakketten verstrekt door verdelingen NetworkManager configureren als een conflict op het waagent-pakket en dus verwijdert NetworkManager tijdens de installatie van het pakket Linux agent.


## <a name="general-linux-system-requirements"></a>Algemene Linux-systeemvereisten ##

- De kernel boot lijn in GRUB of GRUB2 op te nemen van de volgende parameters wijzigen. Ook Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Ook Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het debuggen van problemen.

    Naast het bovenstaande wordt aangeraden voor het *verwijderen van* de volgende parameters indien aanwezig:

        rhgb quiet crashkernel=auto

    Grafische en stille boot komen niet in een cloud omgeving waar we de logboeken worden verzonden naar de seriële poort.

    De `crashkernel` de optie is geconfigureerd als links, maar de opmerking dat deze parameter zal verminderen de hoeveelheid beschikbaar geheugen in de VM 128MB of meer, die mogelijk problemen op de kleinere VM.

- De Azure Linux Agent installeren

    De Agent Azure Linux is vereist voor het inrichten van een Linux-installatiekopie op Azure.  Veel distributies bieden de agent als een RPM of Deb pakket (het pakket wordt meestal genoemd 'WALinuxAgent' of 'walinuxagent').  De agent kan ook handmatig worden geïnstalleerd door de stappen in de [Handleiding van Linux-Agent](virtual-machines-linux-agent-user-guide.md).

- Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is gewoonlijk de standaardinstelling.

- Maak geen swap ruimte op de schijf OS

    De Agent Azure Linux kunt swap ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Let op de schijf van de lokale resource is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned. Na het installeren van de Agent Azure Linux (Zie vorige stap), de volgende parameters in de /etc/waagent.conf op de juiste wijze te wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Voer de volgende opdrachten op de virtuele machine deprovision een laatste stap:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Op Virtualbox ziet u het volgende foutbericht nadat u ' waagent-force - identiteitsgegevens ': `[Errno 5] Input/output error`. Dit foutbericht is niet essentieel en kan worden genegeerd.

- Vervolgens moet u de virtuele machine afgesloten en uploaden van de VHD naar Azure.
