<properties
    pageTitle="Optimaliseren van uw Linux VM in Azure | Microsoft Azure"
    description="Meer informatie over enkele optimalisatietips om ervoor te zorgen dat u hebt ingesteld met de Linux VM voor optimale prestaties op Azure"
    keywords="Linux virtuele machine van virtuele machine linux ubuntu virtual machine" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Optimaliseer uw Linux VM in Azure

Maken van een Linux virtuele machine (VM) is het eenvoudig om uit te voeren vanaf de opdrachtregel of vanuit de portal. In deze zelfstudie wordt beschreven hoe u ervoor te zorgen u dit hebt ingesteld voor het optimaliseren van de prestaties op het platform Microsoft Azure. In dit onderwerp een Ubuntu Server VM gebruikt, maar u kunt ook Linux virtuele machine met behulp van [uw eigen afbeeldingen als sjablonen](virtual-machines-linux-create-upload-generic.md)maken.  

## <a name="prerequisites"></a>Vereisten

In dit onderwerp wordt ervan uitgegaan dat u al een abonnement Azure ([gratis proefversie aanmelden](https://azure.microsoft.com/pricing/free-trial/)) [de CLI Azure geïnstalleerd](../xplat-cli-install.md) hebt en hebt een VM al voorzien in uw abonnement Azure. Eerst alles met Azure - hebt u om uw abonnement te verifiëren. Dit kunt u doen met Azure CLI typt u `azure login` het interactieve proces te starten. 

## <a name="azure-os-disk"></a>Azure OS schijf

Als u een Linux Vm in Azure maakt, heeft twee schijven zijn gekoppeld. /dev/SDA is de schijf OS, /dev/sdb is de tijdelijke schijf.  Gebruik de belangrijkste OS schijf (/ dev/sda) niet voor iets anders dan het besturingssysteem, zoals die is geoptimaliseerd voor snelle opstarttijd van VM en uitstekende prestaties voor uw werkbelasting biedt. U wilt een of meer schijven koppelen aan uw VM om permanente en opslag van uw gegevens geoptimaliseerd. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Schijven toe te voegen voor de doelen en de prestaties 

Op basis van de grootte van de VM, kunt u maximaal 16 extra schijven op een A-serie, 32 schijven op een D-serie en koppelen 64 schijven op een G-serie machine - elk tot 1 TB in grootte. U toevoegt extra schijven desgewenst per uw ruimte en IOP's vereisten. Elke schijf heeft een prestatiedoel van 500 IOP's voor standaardopslag en maximaal 5000 IOP's per schijf voor de opslag van de premie.  Raadpleeg voor meer informatie over de schijven Premium [Premium-opslag: krachtige opslag voor Azure VMs](../storage/storage-premium-storage.md)

U bereikt de hoogste IOP's op Premium schijven waar de cache-instellingen zijn ingesteld op 'Alleen-lezen' of 'None', moet u "-barrières uitschakelen tijdens de montage van het bestandssysteem in Linux. U hoeft niet barrières omdat schrijfbewerkingen naar Premium back-schijven duurzaam voor de cache-instellingen zijn.

- Als u **reiserFS**, belemmeringen met de optie koppelen uitschakelen ' dam = geen ' (gebruiken voor het inschakelen van belemmeringen ' dam = flush ")
- Als u **ext3/ext4**, belemmeringen met de optie koppelen uitschakelen ' dam = 0 ' (gebruiken voor het inschakelen van barrières ' dam = 1')
- Als u **XFS**gebruikt, uitschakelen belemmeringen met de optie koppelen 'nobarrier' (voor het inschakelen van barrières, gebruik de optie 'dam')

## <a name="storage-account-considerations"></a>Overwegingen voor opslag Account

Wanneer u uw Linux VM in Azure maakt, moet u controleren of dat u schijven koppelen van opslag accounts die zich in hetzelfde gebied, als uw VM dicht en netwerklatentie minimaliseren.  Elke account standaard opslag heeft een maximum van 20 k IOP's en een capaciteit van 500 TB grootte.  Dit leidt tot ongeveer 40 intensief gebruikte schijven, met inbegrip van zowel de OS-schijf en gegevensschijven u. Er is geen limiet voor de maximale IOP's voor opslag van Premium accounts, maar er is een limiet van 32 TB. 

Bij het omgaan met hoge werkbelasting IOP's en u hebt ervoor gekozen standaardopslag voor uw schijven, moet u mogelijk de schijven splitsen in meerdere opslag rekeningen om te controleren of dat u hebt de limiet van 20.000 IOP's voor standaardopslag accounts niet raakt. Uw VM bevat een mix van schijven uit aan verschillende opslag rekeningen en rekeningsoorten opslag om de optimale configuratie. 

## <a name="your-vm-temporary-drive"></a>De VM tijdelijke schijf

Standaard bij het maken van een VM, biedt Azure u een OS schijf (/ dev/sda) en een tijdelijke schijf (/ dev/sdb).  Alle andere schijven die u toevoegt weergegeven als /dev/sdc, /dev/sdd, /dev/sde enzovoort. Alle gegevens op de tijdelijke schijf (/ dev/sdb) is niet duurzaam, en kunnen verloren gaan als u specifieke gebeurtenissen zoals VM vergroten/verkleinen, opnieuw installeren, of het onderhoud zorgt ervoor dat het opnieuw opstarten van de VM.  De grootte en het type van de tijdelijke schijf is gerelateerd aan de VM-grootte die u hebt gekozen bij de implementatie. In het geval van een van de premium-grootte VMs (DS, G- en DS_V2-serie) wordt tijdelijk station ondersteund door een lokale SSD voor extra prestaties van maximaal 48 kB IOP's. 

## <a name="linux-swap-file"></a>Linux Swap File

Als uw Azure VM afkomstig uit een afbeelding Ubuntu of CoreOS is, kunt u CustomData gebruiken voor het verzenden van een cloud-config naar cloud init. Als u [een aangepaste afbeelding voor Linux geüpload](virtual-machines-linux-upload-vhd.md) met cloud-init, ook u swap-partities met cloud init.

Op Ubuntu wolk afbeeldingen, moet u cloud init gebruiken voor het configureren van de swap-partitie. Zie de volgende pagina op de Ubuntu wiki voor meer informatie: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Afbeeldingen zonder ondersteuning voor cloud-init hebben VM-images van de Marketplace Azure geïmplementeerd een VM Linux Agent is geïntegreerd met het besturingssysteem. Deze agent kan de VM te communiceren met verschillende Azure services. Als dat u een standaard installatiekopie van Azure Marketplace hebt geïmplementeerd, moet u het volgende doen om te correct configureren van instellingen voor het wisselbestand Linux:

Zoek en twee vermeldingen in het bestand **/etc/waagent.conf** wijzigen. Ze bepalen het bestaan van een specifieke wisselbestand en de grootte van het wisselbestand. De parameters die u zoeken wilt om te wijzigen zijn `ResourceDisk.EnableSwap=N` en`ResourceDisk.SwapSizeMB=0` 

U moet deze als volgt wijzigen:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size in MB aan uw behoeften} 

Nadat u de wijziging hebt aangebracht, moet u de waagent opnieuw of start opnieuw op uw Linux VM aan deze wijzigingen.  U weet dat de wijzigingen zijn uitgevoerd en is een wisselbestand gemaakt wanneer u de `free` opdracht om vrije ruimte weer te geven. In het volgende voorbeeld is een wisselbestand van 512MB gemaakt als gevolg van het wijzigen van het bestand waagent.conf.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>I/o-planning algoritme voor de opslag van de premie

Met de 2.6.18 Linux kernel, de standaard I/O algoritme plannen vanaf de uiterste datum is gewijzigd in CFQ (volledig reële queuing algoritme). RAM-i/o-patronen is te verwaarlozen verschil in prestatieverschillen tussen CFQ en de Deadline.  Overschakelen naar de Nooperation of Deadline algoritme bereiken betere i/o-prestaties op basis van SSD schijven waarbij de schijf-i/o-patroon voornamelijk sequentieel is.

### <a name="view-the-current-io-scheduler"></a>De huidige i/o-scheduler bekijken

Gebruik de volgende opdracht:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Ziet u na uitvoer geeft de huidige scheduler.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Het huidige apparaat (/ dev/sda) van I/O algoritme planning wijzigen

Gebruik de volgende opdrachten:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Instelling voor /dev/sda alleen is niet handig. Deze moet worden ingesteld op alle gegevensschijven waar sequentiële I/O zomaar het i/o-patroon.  

U ziet de volgende uitvoer, die aangeeft dat de grub.cfg zijn opnieuw gemaakt en dat de standaard-scheduler is bijgewerkt naar Nooperation.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Voor de distributie Redhat-familie hoeft u alleen de volgende opdracht:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Softwarematige RAID gebruiken om te bereiken hoger ik / Ops

Als uw werkbelasting meer IOP's vereisen dan een enkele schijf, moet u een software-RAID-configuratie van meerdere schijven gebruiken. Omdat Azure al tolerantie op de laag weefsel lokale schijf voert, moet u het hoogste niveau van de prestaties van de configuratie van een RAID 0 striping bereiken.  U moet inrichten en nieuwe schijven maakt in de Azure-omgeving en deze koppelt aan uw Linux VM voor partitioneren, formatteren en koppelen van stations.  In het **[Configureren van Software RAID onder Linux](virtual-machines-linux-configure-raid.md)** document vindt u meer informatie over het configureren van een software RAID-installatie op uw Linux VM in azure.


## <a name="next-steps"></a>Volgende stappen

Vergeet niet, als met alle optimalisatie discussies die u nodig hebt voor het uitvoeren van tests voor en na elke wijziging aan het meten van de impact van de wijziging.  Optimalisatie is een stap voor stap proces dat verschillende resultaten op verschillende computers in uw omgeving zal hebben.  Wat werkt voor een configuratie werkt mogelijk niet voor anderen.

Enkele nuttige koppelingen naar aanvullende bronnen: 

- [Premium-opslagruimte: Krachtige opslag voor Azure VM werkbelasting](../storage/storage-premium-storage.md)
- [Azure Linux Agent-gebruikershandleiding](virtual-machines-linux-agent-user-guide.md)
- [Optimaliseren MySQL prestaties op Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md)
- [Software RAID onder Linux configureren](virtual-machines-linux-configure-raid.md)
