<properties
    pageTitle="Richtlijnen voor opslag oplossingen | Microsoft Azure"
    description="Informatie over de belangrijkste richtlijnen voor ontwerp en implementatie voor de implementatie van opslagoplossingen in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Richtlijnen voor opslag infrastructuur

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dit artikel richt zich op het inzicht van de opslagbehoeften en overwegingen voor het bereiken van optimale virtuele machine (VM) prestaties.


## <a name="implementation-guidelines-for-storage"></a>Implementatie van richtlijnen voor opslag

Besluiten:

- Moet u gebruik Standard of Premium opslag voor uw werkbelasting?
- Moet u het maken van striped schijven groter is dan 1023 GB maken?
- Moet u een schijf-striping met optimale i/o-prestaties voor uw werkbelasting?
- Welke set van opslag accounts moet u voor het hosten van uw infrastructuur of de werkbelasting van IT?

Taken:

- Bekijk de i/o-behoeften van de toepassingen die u implementeert en van plan bent het juiste aantal en soort opslag rekeningen.
- Maak de opslag rekeningen met behulp van de naamgevingsregel vormt. U kunt de Azure PowerShell of de portal.


## <a name="storage"></a>Opslag

Azure opslag is een belangrijk onderdeel van de implementatie en het beheer van virtuele machines (VMs) en toepassingen. Azure opslag biedt services voor het opslaan van gegevens in een bestand, ongestructureerde gegevens en berichten, en is ook onderdeel van de infrastructuur voor ondersteuning van VMs.

Er zijn twee soorten accounts opslag beschikbaar ter ondersteuning van VMs:

- Standaardopslag accounts u toegang krijgt tot blob-opslag (gebruikt voor het opslaan van Azure VM schijven),, tabelopslag, opslag van de wachtrij en opslag van bestanden.
- Rekeningen van de [premie opslag](../storage/storage-premium-storage.md) leveren met hoge prestaties, lage latentie schijfondersteuning voor i/o-intensieve belasting, zoals SQL-Servers in een cluster AlwaysOn. Premium opslag ondersteunt momenteel alleen schijven Azure VM.

VMs maakt Azure met een diskette, een tijdelijke schijf en schijven van nul of meer optionele gegevens. Het besturingssysteem schijf- en gegevensschijven zijn pagina Azure BLOB's, dat de tijdelijke schijf lokaal is opgeslagen op het knooppunt waar de machine zich. Wees voorzichtig bij het ontwerpen van toepassingen alleen deze tijdelijke schijf gebruiken voor niet-permanente gegevens zoals de VM kan worden gemigreerd tussen hosts tijdens een gebeurtenis voor onderhoud. Gegevens die zijn opgeslagen op de tijdelijke schijf wordt, gaat verloren.

Duurzaamheid en hoge beschikbaarheid wordt geleverd door de onderliggende opslag Azure-omgeving om ervoor te zorgen dat uw gegevens beschermd tegen niet-geplande onderhoud of hardware-fouten blijft. Bij het ontwerpen van uw opslagomgeving Azure kunt u kiezen voor het repliceren van VM-opslagruimte:

- lokaal binnen een bepaalde Azure datacenter
- via Azure datacenters binnen een bepaald gebied
- via Azure datacenters tussen de verschillende regio 's

U vindt [meer informatie over de replicatieopties voor hoge beschikbaarheid](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operating system-schijven en gegevensschijven hebben een maximale grootte van 1023 gigabytes (GB). De maximale grootte van een blob is 1024 GB en moet de metagegevens (voettekst) van het VHD-bestand (een GB is 1024<sup>3</sup> bytes) bevat. Opslagruimtes in Windows Server 2012 kunt u deze limiet groter zijn dan door het groeperen van gegevensschijven voor het weergeven van logische volumes die groter is dan 1023GB voor uw VM.

Er zijn enkele beperkingen schaalbaarheid bij het ontwerpen van uw implementaties Azure opslag - Zie [Microsoft Azure abonnement en service, quota's en beperkingen](azure-subscription-service-limits.md#storage-limits) voor meer informatie. Zie ook [Azure opslag schaalbaarheid en prestaties doelen](../storage/storage-scalability-targets.md).

Voor de opslag van de toepassing, kunt u opslaan ongestructureerde objectgegevens zoals documenten, afbeeldingen, back-ups, configuratiegegevens, Logboeken, enz. met behulp van blob-opslag. De toepassing kan in plaats van uw toepassing schrijven naar een virtuele schijf is aangesloten op de VM, schrijven rechtstreeks naar Azure blob-opslag. BLOB-opslag biedt ook de optie [lagen hot en cool opslag](../storage/storage-blob-storage-tiers.md) afhankelijk van de beschikbaarheid van behoeften en kostenbeperkingen.


## <a name="striped-disks"></a>Striped schijven
Naast waardoor u schijven groter is dan 1023 GB, maken in veel gevallen verbetert met striping voor gegevensschijven de prestaties doordat meerdere BLOB's back van de opslag voor één volume. Met striping opbrengsten nodig is om te schrijven en lezen van gegevens uit een enkele logische schijf I/O parallel.

Azure legt beperkingen op het aantal gegevensschijven en de hoeveelheid bandbreedte die beschikbaar zijn, afhankelijk van de grootte van de VM. Zie voor meer informatie [voor virtuele machines](virtual-machines-windows-sizes.md).

Als u van schijf-striping voor gegevensschijven Azure gebruikmaakt, kunt u de volgende richtlijnen:

- Gegevensschijven moet altijd de maximale grootte (1023 GB).
- Koppel de maximale schijven voor de VM-grootte is toegestaan.
- Opslag spaties gebruiken.
- Vermijd het gebruik van opties in de schijfcache Azure gegevens (cachebeleid = geen).

Voor meer informatie Zie [opslagruimtes - ontwerpen voor de prestaties](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).


## <a name="multiple-storage-accounts"></a>Meerdere accounts voor opslag

Als u uw opslagomgeving Azure ontwerpt, kunt u meerdere accounts voor opslag als het aantal VMs implementeert u toeneemt. Dit helpt om de I/O verdelen over de onderliggende Azure opslaginfrastructuur te handhaven optimale prestaties voor uw VMs en toepassingen. Bij het ontwerpen van de toepassingen die u wilt implementeren, kunt u de i/o-eisen elke VM heeft en het saldo van de VMs in Azure opslag rekeningen. Probeer te vermijden de hoge I/O VMs in veeleisende maar één of twee opslag rekeningen te groeperen.

Voor meer informatie over de i/o-mogelijkheden van de verschillende opties voor opslag van Azure en enkele aanbevolen maximumwaarden, Zie [Azure opslag schaalbaarheid en prestaties doelen](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 