<properties
   pageTitle="SAP NetWeaver op Microsoft Azure SUSE Linux VMs testen | Microsoft Azure"
   description="SAP NetWeaver op Microsoft Azure SUSE Linux VMs testen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Met SAP NetWeaver op Microsoft Azure SUSE Linux VMs


Dit artikel beschrijft verschillende overwegingen wanneer u SAP NetWeaver op Microsoft Azure SUSE Linux virtuele machines (VMs uitvoert). Op 19 mei-2016 wordt SAP NetWeaver officieel ondersteund op SUSE Linux VMs op Azure. Alle details over de versies van Linux, SAP kernel versies, enzovoort kunnen u vinden in het 1928533 van SAP notitie "SAP-toepassingen op Azure: ondersteunde producten en typen Azure VM".
Aanvullende documentatie over SAP op Linux VMs vindt u hier: [Met behulp van SAP op Linux virtuele machines (VMs)](virtual-machines-linux-sap-get-started.md).


De volgende informatie kunt u een aantal potentiële valkuilen vermijden.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE afbeeldingen op Azure voor het uitvoeren van SAP

Alleen SUSE Linux Enterprise Server SLES 12 (SPx) gebruiken voor het uitvoeren van SAP NetWeaver op Azure, - Zie ook SAP notitie 1928533. Een speciale afbeelding voor SUSE is in de markt Azure ("SLES 11 SP3 voor SAP-CAL"), maar dit is niet bedoeld voor algemeen gebruik. Gebruik deze afbeelding niet omdat deze gereserveerd voor de oplossing met [SAP Cloud toestel bibliotheek](https://cal.sap.com/) .  

U moet Azure Resource Manager gebruiken voor alle nieuwe tests en installaties op Azure. Om te zoeken naar afbeeldingen SUSE SLES en versies met Azure PowerShell of Azure opdrachtregelinterface (CLI), gebruikt u de volgende opdrachten. Vervolgens kunt u de output, bijvoorbeeld voor het definiëren van de installatiekopie van het besturingssysteem in een JSON-sjabloon voor de implementatie van een nieuwe SUSE Linux VM.
Deze PowerShell-opdrachten zijn geldig voor Azure PowerShell versie 1.0.1 en hoger.

* Zoeken naar bestaande uitgevers, met inbegrip van SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Zoek naar het bestaande aanbod van SUSE:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Zoek naar SUSE SLES aanbiedingen:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Zoek naar een specifieke versie van een SKU SLES:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Installeren van WALinuxAgent in een VM SUSE

De agent genaamd WALinuxAgent maakt deel uit van de afbeeldingen SLES in de markt Azure. Zie voor informatie over het installeren van het handmatig (bijvoorbeeld tijdens het uploaden van een OS SLES virtuele harde schijf (VHD) op ruimten):

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (virtuele-machines-linux-geviseerd-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "verbeterde monitoring"

SAP "enhanced aan toezicht' is een verplicht vereiste voor het uitvoeren van SAP op Azure. Controleer de details in SAP Opmerking 2191498 'SAP op Linux met Azure: verbeterde Monitoring".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Azure gegevensschijven koppelen aan een Linux Azure VM

U moet nooit Azure mount data schijven naar een Linux Azure VM met behulp van de apparaat-ID. Gebruik in plaats daarvan de universally unique identifier (UUID). Wees voorzichtig wanneer u grafische hulpprogramma's voor het koppelpunt Azure gegevensschijven, bijvoorbeeld. Controleer de vermeldingen in /etc/fstab.

Het probleem met de apparaat-ID is die kan worden gewijzigd en vervolgens de Azure VM tijdens het opstartproces vastlopen kan. Als het probleem verminderen, zou u de parameter nofail in /etc/fstab. Maar wees voorzichtig met nofail omdat toepassingen het koppelpunt als vóór gebruiken en in het root-bestandssysteem schrijft voor het geval een schijf externe Azure gegevens tijdens het opstarten niet is gekoppeld.

De enige uitzondering koppelen via UUID is een OS schijf voor het oplossen van problemen, koppelen, zoals beschreven in de volgende sectie.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Het oplossen van een VM SUSE die niet meer toegankelijk is

Mogelijk zijn er situaties waar een SUSE VM in Azure loopt tijdens het opstarten (bijvoorbeeld met een fout opgetreden vast met het koppelen van schijven). U kunt dit controleren met de functie van de diagnose opstarten voor v2 Azure virtuele Machines in de portal Azure. Zie voor meer informatie [Boot diagnostics] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

De OS-schijf uit de beschadigde VM koppelen aan een andere SUSE VM in Azure is één manier om het probleem te verhelpen. Vervolgens Breng gewenste wijzigingen zoals /etc/fstab bewerken of verwijderen van netwerk udev regels, zoals beschreven in de volgende sectie.

Er is een belangrijke factor. Implementatie van verschillende SUSE VMs uit dezelfde Azure Marketplace-afbeelding (bijvoorbeeld SLES 11 SP4) zorgt ervoor dat de schijf OS moet altijd worden gemonteerd door dezelfde UUID. Daarom resulteert een OS schijf uit een andere VM die is geïmplementeerd met behulp van dezelfde Azure Marketplace-afbeelding toevoegen met behulp van de UUID in twee identieke UUID's. Deze problemen veroorzaakt en kan dit betekenen dat de VM bedoeld voor het oplossen van problemen in feite wordt opgestart vanaf de schijf aangesloten en beschadigde OS in plaats van de oorspronkelijke.

Er zijn twee manieren om dit te voorkomen:

* Een andere Azure Marketplace-afbeelding gebruiken voor het oplossen van problemen VM (bijvoorbeeld SLES 11 SPx in plaats van SLES 12).
* De beschadigde OS schijf niet koppelen van een andere VM via UUID--gebruik iets anders.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Een VM SUSE van on-premises tot Azure uploaden

Zie voor een beschrijving van de stappen voor het uploaden van een VM SUSE op ruimten naar Azure [voorbereiden een SLES of openSUSE virtuele machine voor Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Als u wilt dat een VM zonder de identiteitsgegevens stap aan het eind (bijvoorbeeld om te houden van een bestaande installatie van SAP, alsmede de hostnaam van de) uploaden, kunt u het volgende controleren:

* Zorg ervoor dat de OS-schijf is gekoppeld met behulp van UUID en niet de apparaat-ID. Wijzigen van UUID in /etc/fstab is niet genoeg voor de OS-schijf. Ook, vergeet niet om aan te passen de opstartlader via YaST of door /boot/grub/menu.lst te bewerken.
* Als u de VHDX-indeling gebruiken voor het SUSE OS schijf converteren naar een VHD voor het uploaden naar Azure, is het zeer waarschijnlijk dat het netwerkapparaat van eth0 eth1 verandert. Om problemen te voorkomen wanneer u later op Azure starten bent, terug te zetten naar eth0 zoals beschreven in [tot vaststelling van eth0 in VMware voor gekloonde SLES 11] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Behalve dat wordt beschreven in het artikel, raden wij u aan deze te verwijderen:

   /lib/udev/Rules.d/75-persistent-NET-generator.Rules

U kunt ook de Azure Linux-Agent (waagent) om u te helpen potentiële problemen te voorkomen, zolang er meerdere NIC's zijn installeren.

## <a name="deploying-a-suse-vm-on-azure"></a>Implementatie van een SUSE VM in Azure

U moet nieuwe SUSE VMs maken met behulp van JSON-sjabloonbestanden in het nieuwe model van Azure Resource Manager. Nadat het sjabloonbestand JSON is gemaakt, kunt u de VM kunt implementeren met behulp van de volgende CLI-opdracht als alternatief voor PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Zie voor meer informatie over de sjabloonbestanden JSON [sjablonen ontwerpen Azure Resource Manager] (resource-groep-authoring-templates.md) en [sjablonen Azure quickstart] (https://azure.microsoft.com/documentation/templates/).

Zie voor meer informatie over de CLI en Azure Resource Manager [gebruiken de CLI Azure voor Mac, Linux en Windows Azure Resource Manager] (xplat-cli-azure-bron-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-licentie- en sleutel

Voor de officiële certificering van de SAP-Azure is een nieuw mechanisme geïntroduceerd voor het berekenen van de hardwaresleutel SAP dat wordt gebruikt voor de SAP-licentie. De SAP-kernel moest worden aangepast aan het maken van dit gebruik. Voormalige SAP kernel versies voor Linux, bevatten deze codewijziging. Daarom, in bepaalde situaties (bijvoorbeeld Azure VM formaat), de hardwaresleutel SAP gewijzigd en de SAP-licentie is niet langer geldig. Dit wordt opgelost in de nieuwste SAP-Linux kernels. Controleer of SAP notitie 1928533 voor meer informatie.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf pakket / afgestemd adm

SUSE biedt een pakket genaamd "sapconf", die een set van SAP-specifieke instellingen worden beheerd. Voor meer informatie over de betekenis van dit pakket en het installeren en gebruiken, Zie [sapconf bij het voorbereiden van SUSE Linux Enterprise Server voor het uitvoeren van SAP-systemen] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) en [wat sapconf is of het voorbereiden van een SUSE Linux Enterprise Server voor het uitvoeren van SAP-systemen?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

In de tussentijd is er een nieuw hulpprogramma dat sapconf - afgestemd adm. vervangt Een vindt meer informatie over dit hulpprogramma de onderstaande twee koppelingen te volgen.

Documentatie over afgestemd adm profiel sap-hana SLES vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Tuning-systemen voor SAP werkbelasting met afgestemd adm - vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) in hoofdstuk 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>NFS delen in gedistribueerde installaties van SAP

Als u een gedistribueerde installatie--bijvoorbeeld hebt, waar u de database en de SAP-toepassingsservers in afzonderlijke VMs--installeren kunt u de map /sapmnt via het Network File System (NFS) delen. Als u problemen hebt met de installatie stappen uit nadat u de NFS-share voor /sapmnt maken, Controleer of 'no_root_squash' is ingesteld voor de share.

## <a name="logical-volumes"></a>Logische volumes

In het verleden als een een groot logisch volume nodig op meerdere diskettes van Azure gegevens (bijvoorbeeld voor de SAP-database), is het raadzaam te gebruiken mdadm lvm volledig nog niet is gevalideerd op Azure. Meer informatie over het instellen van RAID op Azure Linux met behulp van mdadm, Zie [configureren software RAID onder Linux](virtual-machines-linux-configure-raid.md). In de tussentijd vanaf het begin van mei 2016 ook lvm wordt volledig ondersteund in Azure en kan worden gebruikt als alternatief voor mdadm. Zie voor meer informatie over lvm op Azure [LVM configureren op een Linux VM in Azure](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE opslagplaats

Als er een probleem met toegang naar de standaard Azure SUSE-bibliotheek, kunt u een eenvoudige opdracht dit opnieuw instellen. Dit kan gebeuren als u een persoonlijke installatiekopie van het besturingssysteem in een regio Azure maakt en kopieert u de afbeelding naar een andere regio waar u nieuwe VMs die zijn gebaseerd op deze persoonlijke installatiekopie implementeren. Alleen de volgende opdracht in de VM uitvoeren:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome desktop

Als u wilt installeren, een compleet systeem van SAP demo binnen een enkele VM--waaronder een SAP-GUI met de Gnome desktop gebruik browser en SAP-beheerconsole--deze aanwijzing op de Azure SLES afbeeldingen installeren:

   Voor SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Voor SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-ondersteuning voor Oracle voor Linux in de cloud

Er is een beperking van de ondersteuning van Oracle voor Linux in gevirtualiseerde omgevingen. Dit is niet een Azure-specifieke onderwerp, is het belangrijk dat u begrijpt. SAP biedt geen ondersteuning voor Oracle op SUSE of Red Hat in een openbare cloud zoals Azure. Om te discussiëren over dit onderwerp, rechtstreeks contact opnemen met Oracle.
