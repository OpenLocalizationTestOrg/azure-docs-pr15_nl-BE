<properties
    pageTitle="Maken en uploaden van een VHD Linux | Microsoft Azure"
    description="Maken en uploaden van een Azure virtuele harde schijf (VHD) met het klassieke implementatiemodel met Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Maken en uploaden van een virtuele vaste schijf waarop het besturingssysteem Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]U kunt ook [een aangepaste schijfkopie via Azure Resource Manager uploaden](virtual-machines-linux-upload-vhd.md).

In dit artikel wordt beschreven hoe u voor het maken en uploaden van een virtuele harde schijf (VHD), zodat u deze als uw eigen afbeelding gebruiken kunt te maken van virtuele machines in Azure. Informatie over het voorbereiden van het besturingssysteem, zodat u deze gebruiken kunt voor het maken van meerdere virtuele machines op basis van die afbeelding. 

>  [AZURE.NOTE] Als u enkele ogenblikken hebt, kunt u ons helpen de Azure Linux VM documentatie verbeteren door middel van deze [Snelle enquête](https://aka.ms/linuxdocsurvey) uw ervaringen. Elk antwoord helpt ons help u uw werk kunt doen krijgt.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de volgende items:

- **Linux-besturingssysteem is geïnstalleerd in het VHD-bestand** - u een [Azure geviseerd Linux distributie](virtual-machines-linux-endorsed-distros.md) hebt geïnstalleerd (of informatie [voor distributies niet bekrachtigd](virtual-machines-linux-create-upload-generic.md)) tot een virtuele schijf in de indeling van de VHD. Meerdere hulpprogramma's bestaan om een VM en VHD te maken:
    - Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), bekommeren VHD gebruiken als indeling voor uw afbeeldingen. Indien nodig, kunt u met behulp van [een afbeelding omzet in](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) `qemu-img convert`.
    - U kunt ook Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] De nieuwere VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een VM maakt, geeft u VHD als de indeling. Indien nodig, kunt u omzetten VHDX schijven VHD via [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure ondersteunt bovendien geen dynamische VHD's, uploaden, dus u deze schijven converteren naar statische VHD's moet voordat u uploadt. Dynamische schijven converteren tijdens het uploaden naar Azure kunt u hulpprogramma's zoals [Azure VHD-hulpprogramma's voor gaan](https://github.com/Microsoft/azure-vhd-utils-for-go) .

- **De opdrachtregelinterface azure** - Installeer de meest recente [Azure opdrachtregelinterface](../virtual-machines-command-line-tools.md) voor het uploaden van de VHD.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Stap 1: Voorbereiden van de afbeelding te uploaden

Azure ondersteunt diverse Linux-distributies (Zie [Geviseerd distributies](virtual-machines-linux-endorsed-distros.md)). De volgende artikelen begeleiden u bij het voorbereiden van de diverse Linux-distributies die worden ondersteund op Azure. Nadat u de stappen in de volgende handleidingen, komen hier terug zodra er een VHD-bestand dat klaar is om te uploaden naar Azure is:

- **[Op basis van centOS verdelingen](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - niet-geviseerd verdelingen](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] De Azure platform SLA is van toepassing op de virtuele machines met het Linux-besturingssysteem alleen wanneer een van de geviseerde verdelingen met configuratiegegevens van de opgegeven onder versies ondersteund in [Linux op de verdelingen van de Azure-Endorsed](virtual-machines-linux-endorsed-distros.md)wordt gebruikt. Alle Linux-distributies in de afbeeldinggalerie met Azure zijn geviseerd verdelingen met de vereiste configuratie.

Zie ook de **[Opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** voor meer algemene tips over het voorbereiden van images Linux van Azure.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Stap 2: De verbinding met Azure voorbereiden

Controleer of u de CLI Azure in het klassieke implementatiemodel (`azure config mode asm`), vervolgens aanmelden bij uw account:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Stap 3: De afbeelding uploaden naar Azure

U moet een account opslag de VHD-bestand te uploaden. U kunt ofwel een bestaande opslag account of [Maak een nieuwe](../storage/storage-create-storage-account.md)kiezen.

De Azure CLI gebruiken voor het uploaden van de afbeelding met de volgende opdracht:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

In het vorige voorbeeld:

- **BlobStorageURL** is de URL voor de opslag-account die u wilt gebruiken
- **YourImagesFolder** is de container in de blob-opslag waar u uw afbeeldingen opslaan
- **VHDName** is de naam die wordt weergegeven in de portal voor het identificeren van de virtuele harde schijf.
- **PathToVHDFile** is het volledige pad en de naam van het VHD-bestand op uw computer.

Het volgende voorbeeld toont een volledig voorbeeld:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Stap 4: Maak een VM uit de afbeelding
U maakt een VM met behulp van `azure vm create` op dezelfde manier als een gewone VM. Geef de naam die u voor uw afbeelding in de vorige stap hebt opgegeven. In het volgende voorbeeld gebruiken we de naam **UbuntuLTS** afbeelding in de vorige stap:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Als u wilt uw eigen VMs, bieden uw eigen gebruikersnaam + wachtwoord, locatie, DNS-naam, en de afbeelding.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure CLI-verwijzing voor het model Klassiek Azure-implementatie](../virtual-machines-command-line-tools.md)voor meer informatie.

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
