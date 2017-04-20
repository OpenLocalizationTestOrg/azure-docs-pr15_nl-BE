<properties
    pageTitle="Incrementele momentopnamen gebruiken voor back-up en herstel van Azure virtual machines | Microsoft Azure"
    description="Een aangepaste oplossing voor back-up en herstellen van schijven met incrementele momentopnamen Azure virtuele machine maken."
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
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Back-up van schijven met incrementele momentopnamen Azure virtuele machine

## <a name="overview"></a>Overzicht

Azure opslag biedt de mogelijkheid om momentopnamen van BLOB's. De blob staat vastgelegd op dat moment in de tijd. In dit artikel wordt een scenario van hoe u back-ups van de virtuele machine schijven met momentopnamen kunt onderhouden worden beschreven. Kunt u deze methode wanneer u niet wilt gebruiken voor Azure back-up en Recovery-Service en een aangepaste back-upstrategie voor uw virtuele machine schijven maken.

Azure VM schijven worden opgeslagen als BLOB's pagina in Azure opslag. Omdat we back-upstrategie voor virtuele machine schijven in dit artikel bedoelen, zullen we worden verwijzen naar momentopnamen in de context van BLOB's pagina. Raadpleeg voor meer informatie over momentopnamen, voor het [maken van een momentopname van een Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Wat is een momentopname?

Een momentopname van een blob is een alleen-lezen versie van een blob die wordt vastgelegd op een punt in de tijd. Wanneer u een momentopname hebt gemaakt, kan deze worden gelezen, gekopieerd of verwijderd, maar niet gewijzigd. Snapshots bieden een manier om een back-up een blob zoals deze wordt weergegeven op een moment. U had de mogelijkheid om te kopiëren van volledige momentopnamen tot REST versie 2015-04-05. Met de REST versie 2015-07-08 en boven u kunt u ook incrementele momentopnamen kopiëren.

## <a name="full-snapshot-copy"></a>Volledige momentopname kopiëren

Momentopnamen kunnen worden gekopieerd naar een andere opslag-account als een blob om back-ups van de basis-blob. U kunt ook een momentopname kopiëren via de blob basis, net als in de blob terugzetten naar een eerdere versie. Wanneer u een momentopname van een opslag account naar de andere wordt gekopieerd, wordt deze beslaan dezelfde ruimte als de blob basis pagina. Hele momentopnamen kopiëren van de ene account voor opslag naar de andere traag zijn en daarom ook veel ruimte in de doelaccount opslag verbruiken.

>[AZURE.NOTE] Als u de basis blob naar een andere bestemming kopieert, worden de momentopnamen van de blob niet mee gekopieerd. Als u een basis blob met een kopie overschrijven, momentopnamen die is gekoppeld aan de basis blob niet worden beïnvloed en onder de naam base blob intact blijven.

### <a name="back-up-disks-using-snapshots"></a>Back-up van schijven met momentopnamen

Als een back-up strategie voor uw virtuele machine-schijven kunt u periodieke momentopnamen van de schijf- of blob en deze kopiëren naar een andere opslag-account met behulp van hulpprogramma's zoals [Kopie Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) bewerking of [AzCopy](storage-use-azcopy.md). U kunt een momentopname kopiëren naar een bestemming pagina blob met een andere naam. De uiteindelijke bestemming pagina blob is een blob beschrijfbare pagina en niet een momentopname. Verderop in dit artikel worden stappen voor het maken van back-ups van de virtuele machine schijven met momentopnamen wordt beschreven.

### <a name="restore-disks-using-snapshots"></a>Herstellen van schijven met momentopnamen

Wanneer is het tijd om uw schijf te herstellen naar een vorige stabiele versie, vastgelegd in een van de momentopnamen voor back-up, kunt u een momentopname van de blob basis pagina kopiëren. Nadat de momentopname wordt gepromoveerd tot de basis pagina blob, momentopname, blijft maar de bron wordt overschreven door een kopie die kan worden gelezen en geschreven. Verderop in dit artikel worden stappen voor het terugzetten van een vorige versie van de schijf van de momentopname wordt beschreven.

### <a name="implementing-full-snapshot-copy"></a>Uitvoering van de volledige momentopname kopiëren

Op de volgende manieren kunt u een kopie van de volledige momentopname implementeren

-   Eerst een momentopname van de basis-blob met de [Momentopname Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) -bewerking.
-   Kopieer vervolgens de momentopname naar opslag doelaccount [Blob kopiëren](https://msdn.microsoft.com/library/azure/dd894037.aspx)gebruiken.
-   Herhaal deze procedure voor het onderhouden van back-ups van uw basis blob.

## <a name="incremental-snapshot-copy"></a>Incrementele momentopname kopiëren

De nieuwe functie in de [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API biedt een veel betere manier om een back-up van de momentopnamen van uw pagina BLOB's of schijven. De API wordt de lijst met wijzigingen tussen de blob basis en de momentopnamen. Dit vermindert de hoeveelheid opslagruimte die wordt gebruikt voor de back-account. De API ondersteunt BLOB's pagina op Premium opslag en standaardopslag. Met deze API kunt u sneller en efficiënte back-up oplossingen bouwen voor VMs Azure. Dit is beschikbaar met de REST versie 2015-07-08 en hoger.

Incrementele momentopname kopiëren kunt u kopiëren van de ene account voor opslag naar een andere het verschil tussen,

-   Basis blob en de momentopname of
-   De twee momentopnamen van de basis-blob

Geleverd met dat de volgende voorwaarden wordt voldaan,

- De blob is gemaakt op 1-Jan-2016 of later.
- De blob is niet overschreven met [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) of [Blob kopiëren](https://msdn.microsoft.com/library/azure/dd894037.aspx) tussen twee momentopnamen.


**Opmerking**: deze functie is beschikbaar voor Premium en Standard Azure pagina BLOB's.

Wanneer er een aangepaste back-upstrategie met momentopnamen, de momentopnamen kopiëren van de ene account voor opslag naar de andere kunnen erg traag en verbruikt veel ruimte. In plaats van de volledige momentopname kopiëren naar een back-up opslaan, kunt u het verschil tussen opeenvolgende momentopnamen om een blob pagina back-up te schrijven. Op deze manier wordt de tijd voor het kopiëren en ruimte voor het opslaan van back-ups aanzienlijk verminderd.

### <a name="implementing-incremental-snapshot-copy"></a>Implementeren van incrementele momentopname kopiëren

Op de volgende manieren kunt u incrementele snapshotkopieën implementeren

-   Een momentopname van de basis blob [Blob momentopname](https://msdn.microsoft.com/library/azure/ee691971.aspx)gebruiken.
-   De momentopname kopiëren naar de doelaccount opslagmedium met [Kopie Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Dit is de pagina back-up-blob. Een momentopname van de blob voor deze pagina back-up en worden opgeslagen in back-account.
-   Een andere momentopname van de basis blob Blob momentopname gebruiken.
-   Het verschil tussen de eerste en tweede momentopnamen van base blob met [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx)krijgen. Met de nieuwe parameter **prevsnapshot** geeft u de datum/tijd-waarde van de momentopname die u wilt ophalen, het verschil met. Wanneer deze parameter aanwezig is, wordt het antwoord van de REST alleen de pagina's die zijn gewijzigd tussen doel momentopname en de vorige momentopname met inbegrip van normale pagina's opnemen.
-   Gebruik [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) om deze wijzigingen toepassen op de pagina back-up-blob.
-   Ten slotte, een momentopname van de blob pagina back-up en opslaan in de opslag van back-account.

In het volgende gedeelte wordt beschreven in meer detail hoe u back-ups van schijven met incrementele snapshotkopieën kunt onderhouden

## <a name="scenario"></a>Scenario

In deze sectie wordt een scenario waarbij een aangepaste back-upstrategie voor virtuele machine schijven met momentopnamen worden beschreven.

U kunt een DS serie Azure VM met een premie opslag P30 schijf die is gekoppeld. De P30 schijf genaamd *mypremiumdisk* wordt opgeslagen in een premium opslag account *mypremiumaccount*genoemd. Een standaard opslag met de naam van *mybackupstdaccount* wordt gebruikt voor het opslaan van de back-up van *mypremiumdisk*. We willen graag een momentopname van een *mypremiumdisk* houden om 12 uur.

Raadpleeg voor meer informatie over het maken van account voor opslag en schijven [over Azure opslag](storage-create-storage-account.md)rekeningen.

Raadpleeg voor meer informatie over back-ups van VMs Azure [Plan Azure VM](../backup/backup-azure-vms-introduction.md)back-ups.

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Stappen voor het beheren van back-ups van een schijf met een incrementele momentopnamen

De hieronder beschreven stappen worden momentopnamen van de *mypremiumdisk* en de back-ups in *mybackupstdaccount*onderhouden. De back-up is een standaard pagina blob *mybackupstdpageblob*genoemd. De pagina backup label weerspiegelt altijd dezelfde staat als de laatste momentopname van de *mypremiumdisk*.

1.  Maak eerst de blob pagina back-up van de premie opslagschijf. Hiertoe een momentopname van een *mypremiumdisk* genaamd *mypremiumdisk_ss1*.
2.  Deze momentopname kopiëren naar mybackupstdaccount als een pagina blob *mybackupstdpageblob*genoemd.
3.  Een momentopname maken van *mybackupstdpageblob* , *mybackupstdpageblob_ss1*, [Momentopname Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) met de naam en sla het op in *mybackupstdaccount*.
4.  Een andere momentopname maken van *mypremiumdisk*tijdens de back-uptijd, zegt *mypremiumdisk_ss2*en opslaan in *mypremiumaccount*.
5.  De incrementele wijzigingen tussen de twee momentopnamen, *mypremiumdisk_ss2* en *mypremiumdisk_ss1*, met [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) op *mypremiumdisk_ss2* met **prevsnapshot** -parameter ingesteld op de tijdstempel van de *mypremiumdisk_ss1*krijgen. Schrijf deze incrementele wijzigingen op de pagina back-up blob- *mybackupstdpageblob* in *mybackupstdaccount*. Als er in de incrementele wijzigingen verwijderde bereiken, moeten ze worden gewist uit de blob pagina back-up. Gebruik [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) om incrementele wijzigingen op de pagina back-up blob schrijven.
6.  Een momentopname van de pagina back-up blob *mybackupstdpageblob*, *mybackupstdpageblob_ss2*genoemd. De vorige momentopname *mypremiumdisk_ss1* uit premium opslag account verwijderen.
7.  Herhaal stap 4-6 elk back-venster. Op deze manier kunt u back-ups van *mypremiumdisk* in een standaard opslag account behouden.

![Met behulp van momentopnamen van incrementele back-up van schijf](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Terugzetten van een schijf van momentopnamen

Premium schijf, *mypremiumdisk* naar een eerdere momentopname van de back-up opslaan account *mybackupstdaccount*, herstelt u de stappen uit die hieronder worden beschreven.

1.  Het punt in de tijd die u wilt terugzetten de premium-schijf te identificeren. Stel dat de momentopname *mybackupstdpageblob_ss2*, dat is opgeslagen in de back-up opslaan account *mybackupstdaccount*is.
2.  In mybackupstdaccount, bevordering van de momentopname *mybackupstdpageblob_ss2* als de nieuwe back-basis pagina blob *mybackupstdpageblobrestored*.
3.  Een momentopname van deze teruggezette back-pagina blob, *mybackupstdpageblobrestored_ss1*genoemd.
4.  Kopieer de herstelde pagina blob *mybackupstdpageblobrestored* van *mybackupstdaccount* naar *mypremiumaccount* als de nieuwe schijf premium- *mypremiumdiskrestored*.
5.  Een momentopname van de *mypremiumdiskrestored*, *mypremiumdiskrestored_ss1* aangeroepen voor het maken van toekomstige incrementele back-ups.
6.  Punt de DS serie VM aan de herstelde *mypremiumdiskrestored* schijf en loskoppelen van de oude *mypremiumdisk* van de VM.
7.  De back-up-proces dat wordt beschreven in de vorige sectie voor de herstelde schijf *mypremiumdiskrestored*, met behulp van de *mybackupstdpageblobrestored* als de blob pagina back-up beginnen.

![Schijf herstellen van momentopnamen](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het maken van momentopnamen van een blob en planning van uw VM back-infrastructuur met behulp van de onderstaande koppelingen.

- [Maken van een momentopname van een Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [Plan uw back-upinfrastructuur VM](../backup/backup-azure-vms-introduction.md)
