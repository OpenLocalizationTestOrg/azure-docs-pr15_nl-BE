<properties
    pageTitle="Over schijven en VHD's voor Windows VMs | Microsoft Azure"
    description="Meer informatie over de basisbeginselen van schijven en VHD's voor Windows virtuele machines in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Over schijven en VHD's voor virtuele machines die Azure

Net als elke andere computer gebruik virtuele machines in Azure schijven als een locatie voor het opslaan van een besturingssysteem, toepassingen en gegevens. Alle Azure virtuele machines ten minste twee schijven hebt: een Windows-besturingssysteem schijf en een tijdelijke schijf. De schijf wordt gemaakt op basis van een afbeelding en de schijf en de afbeelding zijn beide virtuele vaste schijven opgeslagen in een account Azure opslag. Virtuele machines kunnen ook een of meer data schijven, die ook worden opgeslagen als VHD's hebben. Dit artikel is ook beschikbaar voor [Linux virtuele machines](virtual-machines-linux-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



## <a name="operating-system-disk"></a>Besturingssysteem schijf

Elke virtuele machine heeft een bijbehorende besturingssysteem schijf. Het is geregistreerd als een SATA harde schijf en aangeduid als standaard station C:. Deze schijf heeft een maximale capaciteit van 1023 gigabytes (GB). 

##<a name="temporary-disk"></a>Tijdelijke schijf

De tijdelijke schijf wordt automatisch voor u gemaakt. De tijdelijke schijf wordt aangeduid als het station D: standaard en het wordt gebruikt voor het opslaan van pagefile.sys. 

De grootte van de tijdelijke schijf is gebaseerd op de grootte van de virtuele machine. Zie [grootte voor Windows virtuele machines](virtual-machines-windows-sizes.md)voor meer informatie.

>[AZURE.WARNING] Geen gegevens opslaan op de tijdelijke schijf. Het biedt tijdelijke opslag voor toepassingen en processen en is bedoeld voor het opslaan van gegevens, zoals een pagina of swap-bestanden alleen. Zie voor deze schijf naar een andere stationsletter toewijzen, [wijzigen de stationsletter van de tijdelijke schijf met Windows](virtual-machines-windows-classic-change-drive-letter.md).

Zie voor meer informatie over de manier waarop de tijdelijke schijf wordt gebruikt in Azure, [Wat de tijdelijke schijf op Microsoft Azure virtuele Machines?](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Schijf met gegevens

Een schijf met gegevens is een VHD die gekoppeld aan een virtuele machine voor het opslaan van toepassingsgegevens of andere gegevens die u wilt bewaren. Gegevensschijven worden geregistreerd als SCSI-schijven en worden aangeduid met een letter die u kiest.  Elke gegevensschijf heeft een maximale capaciteit van 1023 GB. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven u en het type opslag koppelen kunt die u kunt gebruiken voor het hosten van de schijven.

>[AZURE.NOTE] Zie voor meer informatie over virtuele machines, capaciteit, [grootte voor Windows virtuele machines](virtual-machines-windows-sizes.md).

Azure maakt een diskette als u een virtuele machine van een afbeelding maakt. Als u een afbeelding van gegevensschijven, Azure gegevensschijven ook gemaakt wanneer de virtuele machine wordt gemaakt. Anders wordt toevoegen u data schijven nadat u de virtuele machine hebt gemaakt.

U kunt gegevensschijven met een virtuele machine te allen tijde toevoegen door het **koppelen van** de schijf met de virtuele machine. U kunt een VHD die u hebt geüpload of gekopieerd naar uw account opslag, of die Azure voor u gemaakt. Toevoegen van een schijf met gegevens worden gekoppeld aan het VHD-bestand VM door een "lease" op de VHD zodat deze niet uit de opslag verwijderen terwijl deze nog steeds gekoppeld.

## <a name="about-vhds"></a>Over VHD 's

De VHD's gebruikt in Azure worden opgeslagen als BLOB voor pagina's in een opslag standard of premium account in Azure VHD-bestanden. Zie voor meer informatie over de pagina BLOB's [Wat blok BLOB's en BLOB's pagina](https://msdn.microsoft.com/library/ee691964.aspx). Zie voor meer informatie over de premie opslag, [opslag Premium: krachtige opslag voor Azure VM werklasten](../storage/storage-premium-storage.md).

Azure ondersteunt de indeling van de VHD vaste schijf. Vaste indeling worden hierdoor de logische schijf lineair in het bestand, zodat die Verschuiving X van de schijf wordt opgeslagen op de X-verschuiving van de blob. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. Vaak ruimte de afvalstoffen vaste indeling omdat de meeste schijven grote ongebruikte bereiken in deze hebben. Azure slaat echter VHD-bestanden in een verspreide indeling, zodat u de voordelen van de vaste en dynamische schijven op hetzelfde moment ontvangen. Zie [aan de slag met virtuele harde schijven](https://technet.microsoft.com/library/dd979539.aspx)voor meer informatie.

Alle bestanden van de VHD in Azure die u gebruiken als bron wilt voor het maken van diskettes of afbeeldingen zijn alleen-lezen. Als u een schijf of een afbeelding maakt, kunt u Azure kopieën van de VHD-bestanden. Deze kopieën kunnen worden alleen-lezen of lezen-en-schrijven, afhankelijk van hoe u de VHD.

Wanneer u een virtuele machine van een afbeelding maakt, wordt een schijf voor de virtuele machine die een kopie van het bronbestand voor de VHD in Azure gemaakt. Ter beveiliging tegen het onbedoeld verwijderen, Azure een lease-overeenkomst geplaatst op een willekeurige bron VHD-bestand dat wordt gebruikt voor het maken van een afbeelding, een diskette of een schijf met gegevens.

Voordat u een bron VHD-bestand verwijderen kunt, moet u de lease-overeenkomst door het verwijderen van de schijf of de afbeelding verwijderen. U kunt de virtuele machine, het besturingssysteem schijf, verwijderen en het bronbestand VHD in één keer door de virtuele machine verwijderen en het verwijderen van alle gekoppelde schijven. Verwijderen van een bron voor een schijf met gegevens VHD-bestand vereist echter verschillende stappen in de volgorde van een set. Eerst u loskoppelen van de schijf van de virtuele machine, en vervolgens de schijf verwijderen en vervolgens het VHD-bestand verwijderen.

>[AZURE.WARNING] Als u een bron VHD-bestand uit de opslag verwijderen of uw opslag account verwijderen, niet kan Microsoft die gegevens voor u herstellen.



## <a name="next-steps"></a>Volgende stappen
-  [Een schijf toevoegen](virtual-machines-windows-attach-disk-portal.md) extra opslagruimte moet toevoegen voor uw VM.
-  [Upload een afbeelding met een VM Windows Azure](virtual-machines-windows-upload-image.md) te gebruiken bij het maken van een nieuwe VM.
-  [Wijziging van de stationsletter van de tijdelijke schijf Windows](virtual-machines-windows-classic-change-drive-letter.md) zodat toepassingen van het station D: voor gegevens gebruikmaken kunnen.
