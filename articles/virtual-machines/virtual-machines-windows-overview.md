<properties
    pageTitle="Overzicht van de virtuele Machines Windows | Microsoft Azure"
    description="Informatie over het maken en beheren van Windows virtuele machines in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Overzicht van Windows virtuele machines in Azure

Azure virtuele Machines (VM) is een van de verschillende soorten [op aanvraag, schaalbare computerbronnen](../app-service-web/choose-web-site-cloud-service-vm.md) Azure biedt. Meestal kiest een VM als u meer controle over uw computeromgeving bieden dan de andere opties bieden. In dit artikel vindt u informatie over wat u moet overwegen voordat u een VM, hoe u het maken en beheren.

Een Azure VM biedt u de flexibiliteit van virtualisatie zonder te kopen en onderhouden van de fysieke hardware die wordt uitgevoerd. U moet echter nog steeds de VM onderhouden door het uitvoeren van taken, zoals het configureren en installeren van de software die compatibel is met deze patch toepassen.

Azure virtuele machines kunnen op verschillende manieren worden gebruikt. Enkele voorbeelden zijn:

- **Ontwikkel- en testomgeving** – Azure VMs bieden een snelle en gemakkelijke manier om een computer te maken met de specifieke configuraties vereist voor code en testen van een toepassing.
- **Toepassingen in de cloud** – omdat de vraag naar uw toepassing kan fluctueren, kan het zinvol economische uit te voeren op een VM in Azure. U betaalt voor extra VMs wanneer u ze nodig hebt en deze afsluiten wanneer u dit niet doet.
- **Uitgebreid datacenter** – virtuele machines in een virtueel netwerk van Azure kan eenvoudig worden aangesloten op het netwerk van uw organisatie.

Het aantal VMs die gebruikmaakt van uw toepassing kunt schalen omhoog en uit tot hetgeen nodig is om te voldoen aan uw behoeften.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Wat moet ik aan denken voordat u een VM

Er zijn altijd een groot aantal [overwegingen](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) bij het samenstellen van een infrastructuur voor toepassing in Azure. Deze aspecten van een VM zijn belangrijk om na te denken voordat u begint:

- De namen van de resources van uw toepassing
- De locatie waar de resources worden opgeslagen
- De grootte van de VM
- Het maximum aantal VMs die kunnen worden gemaakt
- Het besturingssysteem waarop de VM
- De configuratie van de VM na het starten
- De verwante bronnen waarvoor de VM

### <a name="naming"></a>Naamgeving

Een virtuele machine heeft een [naam](virtual-machines-windows-infrastructure-naming-guidelines.md) toegewezen en is de naam van een computer geconfigureerd als onderdeel van het besturingssysteem. De naam van een VM kan maximaal 15 tekens lang zijn.

Als u de schijf maken met Azure, zijn de computernaam en de naam van de virtuele machine hetzelfde. Als u [uploaden en uw eigen afbeelding gebruiken](virtual-machines-windows-upload-image.md) die een eerder geconfigureerde besturingssysteem bevat en deze gebruiken voor het maken van een virtuele machine, de namen kunnen afwijken. Wij raden aan dat wanneer u uw eigen afbeelding uploaden, u de computernaam in het besturingssysteem en de virtuele machine dezelfde naam.

### <a name="locations"></a>Locaties

Alle bronnen die zijn gemaakt in Azure zijn verdeeld over verschillende [geografische regio's](https://azure.microsoft.com/regions/) over de hele wereld. Het gebied heet gewoonlijk **locatie** bij het maken van een VM. Voor een VM, geeft de locatie aan waar de virtuele harde schijven zijn opgeslagen.

Deze tabel bevat enkele van de manieren om een lijst met beschikbare locaties.

| Methode | Beschrijving |
| ------ | ----------- |
| Azure portal | Selecteer een locatie uit de lijst bij het maken van een VM. |
| Azure PowerShell | Gebruik de opdracht [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| REST-API | Gebruik de [lijst met locaties](https://msdn.microsoft.com/library/dn790540.aspx) -bewerking. |

### <a name="vm-size"></a>VM-grootte

De [grootte](virtual-machines-windows-sizes.md) van de VM die u gebruikt, wordt bepaald door de werklast die u wilt uitvoeren. De grootte die u vervolgens kiest bepaalt factoren zoals energie-, geheugen- en processorcapaciteit. Azure biedt een grote verscheidenheid van formaten voor de ondersteuning van vele soorten worden gebruikt.

Azure brengt een [prijs per uur](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) , op basis van de grootte en het besturingssysteem van de VM. Gedeeltelijke uur Azure toeslagen alleen voor de minuten. Opslag is geprijsd en afzonderlijk in rekening gebracht.

### <a name="vm-limits"></a>VM-limieten

Uw abonnement heeft standaard [quotalimieten](../azure-subscription-service-limits.md) die invloed op de implementatie van veel VMs voor uw project hebben kan. De huidige limiet op basis van per abonnement is 20 VMs per regio. Beperkingen kunnen worden verhoogd door het indienen van een support ticket aanvragen van een verhoging.

### <a name="operating-system-disks-and-images"></a>Operating system-schijven en afbeeldingen

Virtuele machines met [virtuele vaste schijven](virtual-machines-windows-about-disks-vhds.md) kunt hun besturingssysteem (OS) en de gegevens opslaan. VHD's worden ook gebruikt voor de afbeeldingen die u kiezen kunt uit om een besturingssysteem te installeren. 

Azure biedt veel [afbeeldingen op marketplace](https://azure.microsoft.com/marketplace/virtual-machines/) te gebruiken met verschillende versies en typen Windows-besturingssystemen. Afbeeldingen op Marketplace worden geïdentificeerd door publisher afbeelding, aanbieding, sku en versie (versie is meestal opgegeven als uiterlijk). 

In deze tabel ziet enkele manieren dat u de gegevens voor een afbeelding kunt vinden.

| Methode | Beschrijving |
| ------ | ----------- |
| Azure portal | De waarden worden automatisch opgegeven voor u wanneer u een afbeelding wilt gebruiken. |
| Azure PowerShell | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -locatie 'locatie'<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -locatie 'locatie'-Publisher "publisherName"<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -locatie 'locatie'-'publisherName' Publisher-bieden "offerName" |
| REST API 's | [Lijst afbeelding uitgevers](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Afbeelding van de lijst biedt](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Lijst afbeelding SKU 's](https://msdn.microsoft.com/library/mt743701.aspx) |

U kunt [uploaden en uw eigen afbeelding gebruiken](virtual-machines-windows-upload-image.md) en wanneer u de naam van de uitgever, aanbod en sku niet gebruikt.

### <a name="extensions"></a>Extensies

VM- [extensies](virtual-machines-windows-extensions-features.md) geven uw VM extra mogelijkheden via post implementatieconfiguratie en geautomatiseerde taken.

Deze gemeenschappelijke taken kunnen worden uitgevoerd met behulp van extensies:

- **Aangepaste scripts uitvoeren** – het [Aangepaste Script extensie](virtual-machines-windows-extensions-customscript.md) kunt u werkbelasting op de VM configureren door het script wordt uitgevoerd wanneer de VM is ingericht.
- **Implementeren en beheren van configuraties** – de [extensie PowerShell gewenst staat configuratie (DSC)](virtual-machines-windows-extensions-dsc-overview.md) kunt u instellen DSC op een VM-configuraties en omgevingen te beheren.
- **Diagnostische gegevens verzamelen** – de [Azure Diagnostics extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) , kunt u de VM voor het verzamelen van diagnostische gegevens die kunnen worden gebruikt voor het controleren van de gezondheid van uw toepassing configureren.

### <a name="related-resources"></a>Verwante bronnen

De bronnen in deze tabel worden gebruikt door de VM en moeten bestaan of worden gemaakt wanneer de VM is gemaakt.

| Resource | Vereist | Beschrijving |
| -------- | -------- | ----------- |
| [Resourcegroep](../azure-resource-manager/resource-group-overview.md) | Ja | De VM moet worden opgenomen in een resourcegroep. |
| [Opslag account](../storage/storage-create-storage-account.md) | Ja | De VM moet de account van de opslagruimte voor het opslaan van de virtuele harde schijven. |
| [Virtueel netwerk](../virtual-network/virtual-networks-overview.md) | Ja | De VM moet lid zijn van een virtueel netwerk. |
| [Openbare IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Nee | VM kan een openbaar IP-adres toegewezen extern toegang tot hebben. |
| [Netwerk-interface](../virtual-network/virtual-network-network-interface-overview.md) | Ja | De VM moet de netwerkinterface in het netwerk communiceren. |
| [Gegevensschijven](virtual-machines-windows-attach-disk-portal.md) | Nee | VM kan gegevensschijven opslagmogelijkheden Vouw bevatten. |

## <a name="how-do-i-create-my-first-vm"></a>Hoe maak ik mijn eerste VM

Hebt u verschillende opties voor het maken van uw VM. De keuze die u maakt, hangt af van de omgeving in. 

Deze tabel bevat informatie om u te helpen uw VM maken is gestart.

| Methode | Artikel |
| ------ | ------- |
| Azure portal | [Maak een virtuele machine met Windows met behulp van de portal](virtual-machines-windows-hero-tutorial.md) |
| Sjablonen | [Maak een virtuele Windows-computer met een sjabloon Resource Manager](virtual-machines-windows-ps-template.md) |
| Azure PowerShell | [Maak een Windows VM met PowerShell](virtual-machines-windows-ps-create.md) |
| Client SDK 's | [Implementeren van Azure bronnen met C#](virtual-machines-windows-csharp.md) |
| REST API 's | [Maken of bijwerken van een VM](https://msdn.microsoft.com/library/mt163591.aspx) |

U hoopt dat dit nooit gebeurt, maar soms iets misgaat. Als deze situatie aan u gebeurt, de gegevens bekijken in [Bronbeheer oplossen van problemen bij de implementatie met het maken van een virtuele Windows-computer in Azure](virtual-machines-windows-troubleshoot-deployment-new-vm.md).

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Hoe beheer ik de VM die ik heb gemaakt?

VMs kunnen worden beheerd met behulp van een browser gebaseerde portal opdrachtregelprogramma's met ondersteuning voor scripts, of rechtstreeks via de API's. Enkele typische taken die u kunt uitvoeren zijn informatie ophalen over een VM, aanmelden bij een VM, back-ups maken en beheren van de beschikbaarheid.

### <a name="get-information-about-a-vm"></a>Informatie opvragen over een VM

Deze tabel ziet u enkele van de manieren waarop u informatie over een VM opvragen kunt.

| Methode | Beschrijving |
| ------ | ----------- |
| Azure portal | In het menu hub op **virtuele Machines** en selecteer vervolgens de VM uit de lijst. Op de blade voor VM hebt u toegang tot informatie, waarden in te stellen en parameters controleren. |
| Azure PowerShell | Voor informatie over het beheren van VMs met PowerShell, Zie [beheren van Azure virtuele Machines met behulp van bronbeheer en PowerShell](virtual-machines-windows-ps-manage.md). |
| REST-API | De bewerking [informatie ophalen VM](https://msdn.microsoft.com/library/mt163682.aspx) gebruiken om informatie over een VM. |
| Client SDK 's | Voor meer informatie over het gebruik van C# VMs beheren Zie [Azure virtuele Machines beheren met behulp van bronbeheer Azure en C#](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Meld u aan bij de VM

U gebruikt de knop verbinding maken in de portal Azure een extern bureaublad-sessie te [starten](virtual-machines-windows-connect-logon.md). Dingen kunnen soms fout gaan bij het gebruik van een externe verbinding. Als deze situatie met u gebeurt, Raadpleeg de help-informatie in [problemen met extern bureaublad-verbindingen met een Azure virtuele machine met Windows](virtual-machines-windows-troubleshoot-rdp-connection.md).

### <a name="manage-availability"></a>Beschikbaarheid beheren

Het is belangrijk dat u begrijpt hoe [hoge beschikbaarheid garanderen](virtual-machines-windows-manage-availability.md) voor uw toepassing. Deze configuratie moet maken van meerdere VMs om ervoor te zorgen dat ten minste één actief is.

Voor de implementatie in aanmerking komt voor onze 99.95 VM Service Level Agreement moet, u twee of meer VMs met uw werkbelasting binnen een [set beschikbaarheid](virtual-machines-windows-infrastructure-availability-sets-guidelines.md)implementeren. Deze configuratie zorgt ervoor uw VMs zijn verdeeld over meerdere domeinen voor fouttolerantie en naar hosts met verschillende onderhoud windows zijn geïmplementeerd. De volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wordt uitgelegd dat de gegarandeerde beschikbaarheid van Azure als geheel.
 
### <a name="back-up-the-vm"></a>Back-up van de VM
 
Een [kluis Recovery Services](../backup/backup-introduction-to-azure-backup.md) wordt gebruikt voor de bescherming van gegevens en de activa in Azure back-up- en Azure-Site herstellen. U kunt een kluis Recovery Services te [implementeren en beheren van back-ups voor de bronnenbeheerder geïmplementeerd VMs met PowerShell](../backup/backup-azure-vms-automation.md). 

## <a name="next-steps"></a>Volgende stappen

- Als het uw bedoeling is om te werken met Linux VMs, bekijkt u [Azure en Linux](virtual-machines-linux-azure-overview.md).
- Meer informatie over de richtlijnen over het instellen van de infrastructuur in het [voorbeeld Azure infrastructuur scenario](virtual-machines-windows-infrastructure-example.md).
- Zorg ervoor dat u de [Aanbevolen procedures voor het uitvoeren van een Windows VM in Azure](virtual-machines-windows-guidance-compute-single-vm.md)volgen.


