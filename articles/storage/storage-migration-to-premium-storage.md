<properties
    pageTitle="Migreren naar Azure Premium opslag | Microsoft Azure"
    description="Uw bestaande virtuele machines migreren naar Azure Premium opslag. Premium-opslagruimte biedt schijfondersteuning voor I/O-intensieve werkbelasting Azure virtuele Machines met hoge prestaties, lage latentie."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Migreren naar Azure Premium opslag

## <a name="overview"></a>Overzicht

Azure Premium opslag biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines met I/O-intensieve belasting. Migratie van uw toepassing VM schijven naar Azure Premium opslag kunt u profiteren van de snelheid en prestaties van deze schijven halen.

Het doel van deze handleiding is waarmee nieuwe gebruikers van Azure Premium opslag beter voorbereiden op een vloeiende overgang van hun huidige systeem naar de opslag van de premie. De handleiding biedt drie van de belangrijkste onderdelen van dit proces: 

  - [Plan voor de migratie naar Premium-opslag](#plan-the-migration-to-premium-storage)
  - [Voorbereiden en virtuele vaste schijven kopiëren naar Premium-opslag](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Azure Virtual Machine met behulp van de premie opslag maken](#create-azure-virtual-machine-using-premium-storage)

Andere platforms VMs migreren naar Azure Premium opslag of bestaande Azure VMs standaardopslag migreren naar Premium-opslag. In deze gids vindt de stappen voor beide twee scenario's. Volg de stappen in de desbetreffende sectie afhankelijk van het scenario wordt opgegeven.

>[AZURE.NOTE] U vindt een Functieoverzicht en de prijzen van de premie opslag in de premie opslag: [Krachtige opslag voor Azure VM werkbelasting](storage-premium-storage.md). Wij raden u aan elke virtuele machine schijf vereisen hoge IOP's naar Azure premie opslag voor de beste prestaties voor uw toepassing te migreren. Als de schijf geen hoge IOP's vereist, kunt u kosten beperken door deze in de standaardopslag die virtuele machine schijfgegevens worden opgeslagen op de vaste-schijfstations (harde schijven) in plaats van SSD.

De migratie voltooien in zijn geheel mogelijk aanvullende acties zowel vóór als na de stappen in deze handleiding. Voorbeelden zijn virtuele netwerken of eindpunten configureren of u codewijzigingen in de toepassing zelf kunnen vereisen sommige uitvaltijd in uw toepassing. Deze acties zijn uniek voor elke toepassing en u ze samen met de stappen in deze handleiding voor de volledige overgang naar Premium opslag zo weinig mogelijk moet voltooien.


## <a name="plan-the-migration-to-premium-storage"></a>Plan voor de migratie naar Premium-opslag

In deze sectie zorgt ervoor dat u klaar bent voor de migratiestappen in dit artikel helpt u de beste beslissing moet op de VM en schijf.

### <a name="prerequisites"></a>Vereisten
- U moet een Azure-abonnement. Als u niet hebt, kunt u een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/) van een maand abonnement maken of Ga naar [Azure prijzen](https://azure.microsoft.com/pricing/) voor meer opties.
- PowerShell-cmdlets wordt uitgevoerd, moet u de module Microsoft Azure PowerShell. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor de installatie punt en installatie-instructies.
- Wanneer u Azure VMs op Premium-opslagruimte gebruiken plant, moet u de premie opslag kan VMs gebruiken. Met de Premium-opslag kan VMs kunt u standaard- en Premium-opslagruimte schijven. Premium schijven worden in de toekomst meer VM typen beschikbaar. Zie voor meer informatie over alle beschikbare Azure VM schijf types, afmetingen, [formaten voor virtuele machines](../virtual-machines/virtual-machines-windows-sizes.md) en [grootten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Overwegingen met betrekking tot

Een Azure VM ondersteunt verschillende Premium schijven koppelen, zodat uw toepassingen maximaal 64 TB aan opslag per VM. Met Premium-opslagruimte kunnen uw toepassingen per tweede schijf, doorvoersnelheid per VM met zeer lage vertragingstijden voor leesbewerkingen 80.000 IOP's (input/output-bewerkingen per seconde) per VM en 2000 MB bereiken. U hebt opties op tal van VMs en schijven. Deze sectie is om te zoeken naar een optie die het beste aansluit bij uw werkbelasting.

#### <a name="vm-sizes"></a>VM-formaten
De grootte van Azure VM specificaties staan in de [formaten voor virtuele machines](../virtual-machines/virtual-machines-windows-sizes.md). Bekijk de prestatiekenmerken van virtuele machines die werken met Premium-opslagruimte en kies de meest geschikte VM-grootte die het beste aansluit bij uw werkbelasting. Zorg ervoor dat er voldoende bandbreedte beschikbaar op uw VM is om het verkeer van de schijf te sturen.


#### <a name="disk-sizes"></a>Schijfgrootten
Er zijn drie soorten schijven die kunnen worden gebruikt met uw VM en elk heeft specifieke IOP's en doorvoer grenzen. In overweging nemen deze limieten bij het kiezen van het schijftype voor de VM op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek wordt geladen.

|Premium schijf opslagtype|P10|P20|P30|
|:---:|:---:|:---:|:---:|
|Grootte van de schijf|128 GB|512 GB|1024 GB (1 TB)|
|IOP's per schijf|500|2300|5000|
|Doorvoer per schijf|100 MB per seconde|150 MB per seconde|200 MB per seconde|

Afhankelijk van uw werkbelasting, bepalen als gegevensschijven meer nodig voor uw VM zijn. U kunt meerdere schijven van permanente gegevens koppelen aan uw VM. Indien nodig, kunt u op de schijven te vergroten en de prestaties van het volume stripe. (Zie Wat is schijf-Striping [hier](storage-premium-storage-performance.md#disk-striping).) Als u een Premium data schijven voor [Opslag]spaties stripe[4], moet u deze configureren met een kolom voor elke schijf die wordt gebruikt. Anders wordt de algehele prestaties van het striped volume mogelijk lager dan verwacht als gevolg van ongelijkmatige spreiding van het verkeer op de schijven. Voor Linux VMs kunt u het hulpprogramma *mdadm* gebruiken om hetzelfde te bereiken. Zie artikel [Softwarematige RAID op Linux configureren](../virtual-machines/virtual-machines-linux-configure-raid.md) voor meer informatie.

#### <a name="storage-account-scalability-targets"></a>Opslag account schaalbaarheid doelen
Premium zijn opslag de volgende streefcijfers schaalbaarheid naast de [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md). Als de toepassingsvereisten van uw de doelen van de schaalbaarheid van een opslag voor één account overschrijden, uw toepassing gebruik van meerdere accounts voor opslag maken en partitioneren van uw gegevens in de rekeningen van de opslag.

|Capaciteit van de totale rekening|Totale bandbreedte voor een lokaal redundante opslag|
|:--|:---|
|Schijf capaciteit: 35TB<br />Snapshot-capaciteit: 10 TB|Maximaal 50 gigabits per seconde voor Inkomend + uitgaand|

Bekijk voor meer informatie op de specificaties van de premie opslag [schaalbaarheid en prestaties doelen bij het gebruik van de premie opslag](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Beleid in de schijfcache
Beleid in de schijfcache is standaard *Alleen-lezen* voor alle gegevens van de premie schijven en *Lees-en schrijfrechten* voor de Premium-schijf aangesloten op de VM. Deze configuratie-instelling wordt aanbevolen om de optimale prestaties voor IOs van uw toepassing. Schijven met veel schrijven of alleen-schrijven gegevens (bijvoorbeeld SQL Server-logboekbestanden) uitschakelen schijfcaching zodat u betere prestaties kunt bereiken. De cache-instellingen voor gegevensschijven met bestaande kunnen worden bijgewerkt via [Azure Portal](https://portal.azure.com) of de parameter *- HostCaching* van de cmdlet *Set-AzureDataDisk* .

#### <a name="location"></a>Locatie
Kies een locatie waar Azure Premium-opslagruimte beschikbaar is. [Azure Services per regio](https://azure.microsoft.com/regions/#services) Zie voor actuele informatie over de beschikbare locaties. VMs zich in hetzelfde gebied, als de opslag account dat winkels de schijven voor VM veel betere prestaties krijgt dan wanneer ze in afzonderlijke regio's.

#### <a name="other-azure-vm-configuration-settings"></a>Andere Azure VM configuratie-instellingen
Wanneer u een Azure VM maakt, wordt u gevraagd bepaalde VM-instellingen configureren. Vergeet niet dat enkele instellingen voor de levensduur van de VM, zijn vastgesteld tijdens het wijzigen of anderen later toevoegen. Bekijk deze Azure VM configuratie-instellingen en zorg ervoor dat deze overeenkomt met de vereisten van uw werkbelasting op de juiste wijze zijn geconfigureerd.

### <a name="optimization"></a>Optimalisatie

[Azure Premium opslag: ontwerp voor een krachtige](storage-premium-storage-performance.md) vindt u richtlijnen voor het bouwen van krachtige toepassingen met Azure Premium opslag. Volg de richtlijnen in combinatie met de beste praktijken van prestaties van toepassing op technologieën die worden gebruikt door de toepassing.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Voorbereiden en virtuele vaste schijven kopiëren naar Premium-opslag

De volgende sectie bevat richtlijnen voor het voorbereiden van VHD's van uw VM en VHD's kopiëren naar Azure-opslag.

- [Scenario 1: "Ik ben migreren van bestaande Azure VMs naar Azure Premium opslag."](#scenario1)
- [Scenario 2: 'Ik ben migratie VMs van andere platforms naar Azure Premium opslag."](#scenario2)

### <a name="prerequisites"></a>Vereisten

Als u wilt de VHD's voorbereiden voor migratie, hebt u het volgende nodig:

- Een abonnement op Azure een opslag-account en een container in opslag account waaraan u de VHD kunt kopiëren. Houd er rekening mee dat de bestemming opslag account een Standard of Premium opslag account afhankelijk van uw behoefte kan worden.
- Een hulpprogramma dat de VHD generaliseren als u meerdere exemplaren van de VM uit. Bijvoorbeeld sysprep voor Windows of Ubuntu virt-sysprep.
- Een hulpprogramma voor het uploaden van het VHD-bestand naar de rekening voor de opslag. [Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md) Zie of gebruik een [Azure Opslagverkenner](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Deze handleiding beschrijft de VHD met het hulpprogramma AzCopy kopiëren.

> [AZURE.NOTE] Als u de optie synchroon kopiëren met AzCopy, de VHD voor optimale prestaties kopiëren door een van deze hulpprogramma's uitvoeren vanaf een Azure VM in hetzelfde gebied, als de bestemming opslag account. Als u een VHD uit een Azure VM in een andere regio kopieert, de prestaties van uw trager worden.
>
> Voor het kopiëren van een grote hoeveelheid gegevens die via beperkte bandbreedte, overweeg het [gebruik van de service Azure importeren/exporteren om gegevens naar een Blob-opslag](storage-import-export-service.md); Hiermee kunt u uw gegevens door verzending vasteschijfstations overbrengen naar een datacenter Azure. De service Azure importeren/exporteren kunt u gegevens kopieert naar een standaard opslag-account alleen. Als de gegevens in uw account standaardopslag, kunt u de [Kopie Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) - of AzCopy de gegevens overbrengen naar uw opslag premium account.
>
> Opmerking Microsoft Azure ondersteunt alleen vaste grootte VHD-bestanden. VHDX bestanden of dynamische VHD's worden niet ondersteund. Als u een dynamische VHD hebt, kunt u deze converteren naar vaste grootte met de cmdlet [Converteren VHD](http://technet.microsoft.com/library/hh848454.aspx) .

### <a name="scenario1"></a>Scenario 1: "Ik ben migreren van bestaande Azure VMs naar Azure Premium opslag."

Als u bestaande Azure VMs migreert, stop de VM VHD's per type de gewenste VHD voorbereiden en kopieer de VHD met AzCopy of PowerShell.

De VM moet volledig omlaag voor het migreren van een schone staat. Er is een tot de migratie is voltooid.

#### <a name="step-1-prepare-vhds-for-migration"></a>Stap 1. VHD's voorbereiden voor migratie

Als u bestaande Azure VMs naar de opslag van de premie migreert, kan de VHD zijn:

- Gegeneraliseerde besturingssysteemkopie
- Een unieke besturingssysteem schijf
- Een schijf met gegevens

Hieronder bekijken we deze 3 scenario's voor het voorbereiden van de VHD.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Een algemene besturingssysteem VHD gebruiken voor het maken van meerdere exemplaren van de VM

Als u een VHD die wordt gebruikt voor het maken van meerdere exemplaren van generieke Azure VM uploaden wilt, moet u eerst met behulp van een hulpprogramma sysprep VHD generaliseren. Dit is van toepassing op een VHD dat in ruimten of in de cloud. Machine-specifieke informatie wordt verwijderd uit de VHD.

>[AZURE.IMPORTANT] Een momentopname of back-up van uw VM voordat het noemt. Sysprep uitgevoerd zal stoppen en toewijzing van de VM-sessie. Volg onderstaande stappen voor sysprep een VHD Windows OS. Houd er rekening mee dat de opdracht Sysprep uitvoert, moet u de virtuele machine wordt afgesloten. Voor meer informatie over Sysprep Zie [Sysprep overzicht](http://technet.microsoft.com/library/hh825209.aspx) of [Technische naslaginformatie](http://technet.microsoft.com/library/cc766049.aspx).

1. Open een opdrachtvenster als beheerder.
2. Voer de volgende opdracht voor het openen van Sysprep:

        %windir%\system32\sysprep\sysprep.exe

3. Schakel het selectievakje generaliseren, selecteer **Afsluiten**in het hulpprogramma voor systeemvoorbereiding, selecteer systeem invoeren Out-of-Box Experience (OOBE), en klikt u op **OK**, zoals in de onderstaande afbeelding. Sysprep wordt generaliseren van het besturingssysteem en het systeem afsluiten.

    ![][1]

Voor een VM Ubuntu virt-sysprep te gebruiken om hetzelfde te bereiken. Zie [virt sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) voor meer informatie. Zie ook enkele van de open source [software Linux Server ingericht](http://www.cyberciti.biz/tips/server-provisioning-software.html) voor andere Linux-systemen.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Een unieke besturingssysteem VHD gebruiken voor het maken van een enkel exemplaar van de VM

Als er een toepassing wordt uitgevoerd op de VM waarvoor u de machine-specifieke gegevens, niet generaliseren de VHD. Een niet-generalized VHD kan worden gebruikt voor het maken van een uniek exemplaar voor Azure VM. Bijvoorbeeld, als u op de VHD-domeincontroller, kan uitvoeren van sysprep we niet effectief als een domeincontroller. Bekijk de toepassingen op uw VM en de impact van sysprep uitvoert voordat u de VHD noemt.

##### <a name="register-data-disk-vhd"></a>VHD schijf met gegevens registreren

Als u gegevensschijven in Azure worden gemigreerd, moet u controleren of de VMs die gebruikmaken van deze gegevensschijven zijn afgesloten.

Volg de stappen zoals hieronder beschreven VHD naar Azure Premium opslag te kopiëren en als een gegevensschijf ingerichte te registreren.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. De bestemming voor de VHD maken

Maak een account opslag voor het beheren van de VHD. Bij het plannen van de VHD's opslaan, moet u rekening houden met de volgende punten:

- Het doel van opslag-Premium-account.
- De locatie van het account moet hetzelfde als opslag Premium capable Azure VMs in de laatste fase u maakt zijn. U kan kopiëren naar een nieuwe opslag of een plan te gebruiken dezelfde opslag account op basis van uw behoeften.
- Kopiëren en opslaan van de sleutel van de account opslag van de bestemming opslag account voor de volgende fase.

Voor gegevensschijven, kunt u sommige gegevensschijven bewaren in een standaard opslag-account (bijvoorbeeld schijven met koeler opslag), maar wij raden u het verplaatsen van alle gegevens voor de productie van werkbelasting premium opslag gebruiken.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Stap 3. VHD met AzCopy of PowerShell kopiëren

Moet u naar de container en de opslag account sleutel voor het verwerken van een van deze twee opties. Sleutel voor container en de opslag-account kunt u vinden in **Azure Portal** > **opslag**. De URL van de container worden als 'https://myaccount.blob.core.windows.net/mycontainer/'.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Optie 1: Een VHD met AzCopy (kopie asynchroon) kopiëren

AzCopy gebruikt, kunt u gemakkelijk uploaden de VHD via het Internet. Dit kan een tijd duren, afhankelijk van de grootte van de VHD. Vergeet niet de limieten van ingress/egress opslag controleren wanneer u deze optie. [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md) Zie voor meer informatie.

1. Download en installeer AzCopy hier: [meest recente versie van AzCopy](http://aka.ms/downloadazcopy)
2. Azure PowerShell openen en Ga naar de map waarin AzCopy is geïnstalleerd.
3. Gebruik de volgende opdracht om te kopiëren van het VHD-bestand van de "Bron" naar "Bestemming".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Voorbeeld:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Hier vindt u beschrijvingen van de parameters die worden gebruikt in de opdracht AzCopy:

 - * */Bron: * &lt;bron&gt;:*** locatie van de map of URL van een opslag container waarin de VHD.
 - * */SourceKey: * &lt;bron-account-sleutel&gt;:*** opslag account sleutel van de bronaccount opslag.
 - * */Dest: * &lt;doel&gt;:*** opslag container URL de VHD te kopiëren.
 - * */DestKey: * &lt;doel-account-sleutel&gt;:*** opslag account sleutel van de account van de opslag bestemming.
 - * */Patroon: * &lt;-bestandsnaam&gt;:*** geeft u de bestandsnaam van de VHD te kopiëren.

Zie [gegevens met het hulpprogramma AzCopy voor Command-Line](storage-use-azcopy.md)voor meer informatie over het hulpprogramma voor AzCopy.

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Optie 2: Een VHD met PowerShell (gesynchroniseerde kopie) kopiëren

U kunt ook het VHD-bestand met de PowerShell-cmdlet Start AzureStorageBlobCopy kopiëren. Gebruik de volgende opdracht op Azure PowerShell VHD kopiëren. Vervang de waarden in <> met de bijbehorende waarden van de bron- en doeladressen opslag account. Als u deze opdracht gebruikt, moet u een zogenaamd VHD's in uw account bestemming opslag hebben. Als de container niet bestaat, maken voordat de opdracht wordt uitgevoerd.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Voorbeeld:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Scenario 2: 'Ik ben migratie VMs van andere platforms naar Azure Premium opslag."

Als u VHD vanuit niet - Azure Cloud opslag naar Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Het pad van de volledige broncode van de lokale map waarin VHD is opgeslagen bij de hand hebt, en vervolgens AzCopy te uploaden naar Azure opslag.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Stap 1. VHD naar een lokale map exporteren

##### <a name="copy-a-vhd-from-aws"></a>Een VHD van AWS kopiëren

1. Als u (AWS), moet u de EC2 instance exporteren naar een VHD in een Amazon S3 bucket. Volg de stappen die worden beschreven in de documentatie van Amazon voor Amazon EC2 gevallen het hulpprogramma Amazon EC2 opdrachtregelinterface (CLI) installeren en uitvoeren van de opdracht create-exemplaar-export-taak de EC2 instance exporteren naar een VHD-bestand exporteren. Zorg ervoor dat u de **VHD** gebruiken voor de schijf & #95; afbeelding & #95; Variabele indeling bij het uitvoeren van de opdracht **exemplaar-export-taak maken** . Het geëxporteerde VHD-bestand wordt opgeslagen in de Amazon S3 bucket die u tijdens dat proces opgeeft.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. Download het VHD-bestand uit de S3 bucket. Selecteer het VHD-bestand vervolgens **Acties** > **downloaden**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Een VHD kopiëren uit andere niet-Azure cloud

Als u VHD vanuit niet - Azure Cloud opslag naar Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Kopieer het pad van de volledige broncode van de lokale map waarin VHD is opgeslagen.

##### <a name="copy-a-vhd-from-on-premise"></a>Een VHD van on-premise kopiëren

Als u VHD vanuit een lokale omgeving migreert, moet u de volledige broncode pad waar VHD is opgeslagen. Het bronpad kan zijn van een server of bestandsshare.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. De bestemming voor de VHD maken

Maak een account opslag voor het beheren van de VHD. Bij het plannen van de VHD's opslaan, moet u rekening houden met de volgende punten:

- De doelaccount opslag kan worden standard of premium opslag afhankelijk van de vereisten van uw toepassing.
- Het gebied van opslag account moet hetzelfde als opslag Premium capable Azure VMs in de laatste fase u maakt zijn. U kan kopiëren naar een nieuwe opslag of een plan te gebruiken dezelfde opslag account op basis van uw behoeften.
- Kopiëren en opslaan van de sleutel van de account opslag van de bestemming opslag account voor de volgende fase.

Wij raden u het verplaatsen van alle gegevens voor de productie van werkbelasting premium opslag gebruiken.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Stap 3. De VHD naar Azure opslag uploaden

Nu dat u de VHD in de lokale map hebt, kunt u AzCopy of AzurePowerShell het VHD-bestand uploaden naar Azure opslag. Beide opties zijn hier beschikbaar:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Optie 1: Met behulp van Azure PowerShell Add-AzureVhd het VHD-bestand te uploaden

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Een voorbeeld van de <Uri> **_'https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd'_**kan zijn. Een voorbeeld van de <FileInfo> **_'C:\path\to\upload.vhd'_**kan zijn.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Optie 2: Gebruik van AzCopy voor het VHD-bestand uploaden

AzCopy gebruikt, kunt u gemakkelijk uploaden de VHD via het Internet. Dit kan een tijd duren, afhankelijk van de grootte van de VHD. Vergeet niet de limieten van ingress/egress opslag controleren wanneer u deze optie. [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md) Zie voor meer informatie.

1. Download en installeer AzCopy hier: [meest recente versie van AzCopy](http://aka.ms/downloadazcopy)
2. Azure PowerShell openen en Ga naar de map waarin AzCopy is geïnstalleerd.
3. Gebruik de volgende opdracht om te kopiëren van het VHD-bestand van de "Bron" naar "Bestemming".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Voorbeeld:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Hier vindt u beschrijvingen van de parameters die worden gebruikt in de opdracht AzCopy:

 - * */Bron: * &lt;bron&gt;:*** locatie van de map of URL van een opslag container waarin de VHD.
 - * */SourceKey: * &lt;bron-account-sleutel&gt;:*** opslag account sleutel van de bronaccount opslag.
 - * */Dest: * &lt;doel&gt;:*** opslag container URL de VHD te kopiëren.
 - * */DestKey: * &lt;doel-account-sleutel&gt;:*** opslag account sleutel van de account van de opslag bestemming.
 - **/BlobType: pagina:** Geeft aan dat de bestemming een pagina blob.
 - * */Patroon: * &lt;-bestandsnaam&gt;:*** geeft u de bestandsnaam van de VHD te kopiëren.

Zie [gegevens met het hulpprogramma AzCopy voor Command-Line](storage-use-azcopy.md)voor meer informatie over het hulpprogramma voor AzCopy.

##### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD

U kunt ook een VHD uploaden naar uw opslag-account met behulp van een van de volgende wijzen:

- [Azure opslag kopie Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure opslag Explorer uploaden BLOB 's](https://azurestorageexplorer.codeplex.com/)
- [Opslag Import/Export Service REST API: naslag](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Beste Service voor importeren/exporteren gebruiken als geschatte tijd uploaden langer is dan 7 dagen. [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) kunt u de tijd van de gegevens en de overdracht eenheid te schatten.
>
> Import/Export kan worden gebruikt om te kopiëren naar een standaard opslag-account. U wilt kopiëren uit de standaard opslag naar premium opslag-account met behulp van een hulpprogramma zoals AzCopy.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Azure VMs Premium opslag maken

Nadat de VHD is geüpload of gekopieerd naar de gewenste opslag account, volg de instructies in deze sectie de VHD registreren als een installatiekopie van het besturingssysteem of OS schijf afhankelijk van uw scenario en maak vervolgens een VM-sessie uit. De gegevensschijf VHD kan worden gekoppeld aan de VM zodra deze wordt gemaakt. Een voorbeeldscript van de migratie wordt geleverd aan het einde van deze sectie. Dit eenvoudige script komt niet overeen met alle scenario's. Wellicht moet u het script wilt afstemmen op uw specifieke scenario bijwerken. Als dit script is van toepassing op het scenario, Zie hieronder [Een voorbeeld van een migratie Script](#a-sample-migration-script).

### <a name="checklist"></a>Controlelijst

1.  Wacht totdat alle VHD schijven kopiëren is voltooid.
2.  Zorg ervoor dat de dat premie opslag is beschikbaar in de regio die u naar migreert.
3.  Bepaal de nieuwe VM-reeks die u wilt gebruiken. Een premie opslag staat dient te worden en de grootte moet worden afhankelijk van de beschikbaarheid in de regio en op basis van uw behoeften.
4.  Bepaal de exacte VM-grootte die u wilt gebruiken. VM-grootte moet groot genoeg zijn voor de ondersteuning van het aantal gegevensschijven die u hebt. Bv. Als u met 4 gegevensschijven, moet de VM 2 of meer cores hebben. Bedenk ook verwerkingskracht, geheugen en bandbreedte nodig.
5.  Maak een account premie opslag in de doelregio. Dit is de account die u voor de nieuwe VM gebruiken wilt.
6.  De huidige VM details bij de hand, met inbegrip van de lijst met schijven en bijbehorende VHD BLOB's hebben.

Bereid uw toepassing voor downtime. U hebt hiervoor een schone migratie stoppen van de verwerking in het huidige systeem. Alleen dan krijgt u deze naar een consistente status die u naar het nieuwe platform migreren kunt. Duur van de uitvaltijd is afhankelijk van de hoeveelheid gegevens in de schijven die u wilt migreren.

>[AZURE.NOTE] Als u een Resource Manager Azure VM vanaf een speciale VHD schijf maakt, raadpleegt u [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) voor het implementeren van Resource Manager VM met behulp van bestaande schijf.

### <a name="register-your-vhd"></a>Registreer uw VHD

Een VM OS VHD maken of een gegevensschijf koppelen aan een nieuwe VM, moet u ze eerst registreren. Volg de stappen hieronder afhankelijk van het scenario van de VHD.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized VHD maken van meerdere exemplaren van Azure VM Operating System

Nadat een gegeneraliseerde installatiekopie van het besturingssysteem VHD is geüpload naar de rekening voor de opslag, registreren als een **Afbeelding voor Azure VM** zodat kunt u een of meer exemplaren van de VM uit. De volgende PowerShell-cmdlets gebruiken voor het registreren van de VHD als afbeelding Azure VM OS. Geef de URL van de volledige container waar VHD is gekopieerd naar.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Kopiëren en de naam van deze nieuwe Azure VM afbeelding opslaan. In het bovenstaande voorbeeld is *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unieke besturingssysteem VHD maken van één exemplaar Azure VM

Nadat de unieke OS VHD is geüpload naar de rekening voor de opslag, registreren als een **Azure OS schijf** zodat kunt u een VM-sessie uit. Deze PowerShell-cmdlets gebruiken de VHD registreren als een schijf Azure OS. Geef de URL van de volledige container waar VHD is gekopieerd naar.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Kopiëren en de naam van deze nieuwe Azure OS schijf opslaan. In het bovenstaande voorbeeld is *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Gegevens VHD moet worden gekoppeld aan nieuwe stijlen voor Azure VM schijf

Nadat de gegevensschijf VHD is geüpload naar opslag rekening, registreren als een Azure gegevensschijf zodat deze kan worden gekoppeld aan de nieuwe DS-serie, DSv2-serie of het exemplaar van de GS serie Azure VM.

Deze PowerShell-cmdlets gebruiken de VHD registreren als een schijf met gegevens van Azure. Geef de URL van de volledige container waar VHD is gekopieerd naar.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Kopiëren en de naam van deze nieuwe schijf met Azure gegevens opgeslagen. In het bovenstaande voorbeeld is *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Maak een premie opslag kan VM

Nadat de installatiekopie van het besturingssysteem of OS schijf worden geregistreerd, maakt u een nieuwe DS-serie, de DSv2-serie of VM GS-serie. U gebruikt de installatiekopie of besturingssysteem schijf die u hebt geregistreerd. Selecteer het type VM van de premie opslag laag. In het volgende voorbeeld gebruiken we de grootte van de VM *Standard_DS2* .

>[AZURE.NOTE] De grootte van de schijf om ervoor te zorgen dat deze overeenkomt met de capaciteit en prestatie-eisen en de beschikbare Azure schijfgrootte bijwerken.

Volg de stap voor stap PowerShell-cmdlets voor het maken van de nieuwe VM hieronder. Stel eerst de algemene parameters:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Maak eerst een cloud-service waarmee u uw VMs nieuwe hosting.

    New-AzureService -ServiceName $serviceName -Location $location

Vervolgens, afhankelijk van het scenario, de Azure VM exemplaar maken van de installatiekopie van het besturingssysteem of het OS schijf die u hebt geregistreerd.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized VHD maken van meerdere exemplaren van Azure VM Operating System

De een of meer nieuwe DS-serie Azure VM instanties maken met de **Azure installatiekopie van het besturingssysteem** die u hebt geregistreerd. Geef de naam van deze installatiekopie van het besturingssysteem in de VM-configuratie bij het maken van nieuwe VM zoals hieronder wordt weergegeven.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unieke besturingssysteem VHD maken van één exemplaar Azure VM

Nieuwe DS-serie Azure VM exemplaren maken met **Azure OS schijf** die u hebt geregistreerd. Geef de naam van deze OS schijf in de VM-configuratie bij het maken van de nieuwe VM zoals hieronder wordt weergegeven.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Andere Azure VM-informatie, zoals een cloud service, regio opslag account, beschikbaarheid, set en cachebeleid opgeven. Zorg ervoor dat de VM-sessie met het besturingssysteem of van gegevensschijven, zodat de geselecteerde cloud-service, regio en opslag account op dezelfde locatie als de onderliggende VHD's van deze schijven zijn moet.

### <a name="attach-data-disk"></a>De gegevensschijf koppelen

Ten slotte, als u de gegevensschijf VHD's hebt geregistreerd, deze koppelt aan nieuwe opslag Premium capable Azure VM.

Gebruikt u de volgende PowerShell-cmdlet gegevensschijf koppelen aan de nieuwe VM en het cachebeleid opgeven. In het volgende voorbeeld wordt het cachebeleid ingesteld op *alleen-lezen*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Mogelijk zijn er extra stappen die nodig zijn voor de ondersteuning van de toepassing die niet vallen in deze handleiding.

### <a name="checking-and-plan-backup"></a>Controle en back-up plannen

Zodra de nieuwe VM actief is, toegang tot het gebruik van dezelfde aanmeldings-id en wachtwoord als de oorspronkelijke VM en controleren dat alles werkt zoals verwacht. Alle instellingen, met inbegrip van de striped volumes zou aanwezig zijn in de nieuwe VM.

De laatste stap is het plannen van back-up en planning voor de nieuwe VM onderhoud op basis van de behoeften van de toepassing.

### <a name="a-sample-migration-script"></a>Een voorbeeldscript van de migratie

Als er meerdere VMs te migreren, wordt automatisering via PowerShell scripts handig zijn. Hier volgt een voorbeeldscript waarmee u de migratie van een VM. Opmerking hieronder script is slechts een voorbeeld en er zijn enkele veronderstellingen de huidige VM-schijven. Wellicht moet u het script wilt afstemmen op uw specifieke scenario bijwerken.

De hypothesen zijn:

- Klassieke Azure VMs die u maakt.
- De bron OS schijven en de bron Data schijven zijn in dezelfde opslag account en dezelfde container. Als de OS-schijven en schijven gegevens niet op dezelfde plaats, kunt u AzCopy of Azure PowerShell VHD's kopiëren via rekeningen voor opslag en containers. Verwijzen naar de vorige stap: [Kopie VHD met AzCopy of PowerShell](#copy-vhd-with-azcopy-or-powershell). Dit script om te voldoen aan uw scenario bewerken is een andere keuze, maar wij raden aan via AzCopy of PowerShell nadat het is eenvoudiger en sneller.

Hieronder vindt u het automatiseringsscript. Tekst vervangen door uw gegevens en het script wilt afstemmen op uw specifieke scenario bijwerken.

>[AZURE.NOTE] Met het bestaande script behoudt niet de configuratie van uw bron VM. U moet re-config de netwerkinstellingen op de gemigreerde VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimalisatie

De huidige VM-configuratie kan worden aangepast specifiek goed werken met vaste schijven. Bijvoorbeeld, de prestaties te verhogen met behulp van veel schijven in een striped volume. Bijvoorbeeld, in plaats van 4 schijven afzonderlijk op de premie opslag, u mogelijk de kosten te optimaliseren door een enkele schijf. Optimalisaties zoals deze moeten worden verwerkt op basis van geval tot geval en aangepaste stappen nodig na de migratie. Let er ook op dat dit proces mogelijk niet goed werken voor databases en toepassingen die afhankelijk van de schijfindeling gedefinieerd in de instellingen zijn.

##### <a name="preparation"></a>Voorbereiding

1.  De eenvoudige migratie voltooien als beschreven in de vorige sectie. Optimalisaties worden uitgevoerd op de nieuwe VM na de migratie.
2.  Definieer de schijfgrootte van de nieuwe nodig is voor de optimale configuratie.
3.  Toewijzing van de huidige schijven/volumes aan de specificaties van de nieuwe schijf bepalen.

##### <a name="execution-steps"></a>Werk in uitvoering

1.  De nieuwe schijven maken met de juiste afmetingen op de premie opslag VM.
2.  Meld u aan bij de VM en een kopie van de gegevens van het huidige volume met de nieuwe schijf die is toegewezen aan dat volume. Doe dit voor de huidige volumes die u wilt toewijzen aan een nieuwe schijf.
3.  Vervolgens wijzigt u de toepassingsinstellingen om te schakelen naar de nieuwe schijven en de oude volumes ontkoppelen.

Raadpleeg [Optimaliseren van prestaties](storage-premium-storage-performance.md#optimizing-application-performance)voor het afstemmen van de toepassing voor betere schijfprestaties.

### <a name="application-migrations"></a>Migraties van toepassing

Databases en andere complexe toepassingen mogelijk speciale stappen die door de aanbieder van de toepassing voor de migratie. Raadpleeg de documentatie van de desbetreffende toepassing. Bv. meestal databases kunnen worden gemigreerd door back-up en terugzetten.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor specifieke scenario's voor migratie van virtuele machines:

- [Azure virtuele Machines tussen opslag Accounts migreren](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Maken en uploaden van een Windows Server-VHD naar Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Maken en uploaden van een virtuele vaste schijf waarop het besturingssysteem Linux](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migreren voor virtuele Machines van Amazon AWS Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Zie ook de volgende bronnen voor meer informatie over de opslag van Azure en Azure virtuele Machines:

- [Azure opslag](https://azure.microsoft.com/documentation/services/storage/)
- [Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Premium-opslagruimte: Krachtige opslag voor Azure VM werkbelasting](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
