<properties
    pageTitle="Een alleen-lezen momentopname maken van een blob | Microsoft Azure"
    description="Informatie over het maken van een momentopname van een blob back-up blob-gegevens op een bepaald moment in de tijd. Begrijpen hoe momentopnamen worden gefactureerd en het gebruik ervan te minimaliseren capaciteit toeslagen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="create-a-blob-snapshot"></a>Een blob-momentopname maken

## <a name="overview"></a>Overzicht

Een momentopname is een alleen-lezen versie van een blob die op een punt in de tijd wordt genomen. Momentopnamen zijn handig voor back-ups van BLOB's. Nadat u een momentopname maken, lezen, kopiëren of verwijderen, maar kan niet worden gewijzigd.

Een momentopname van een blob is identiek aan de blob basis, met dien verstande dat de blob-URI is een **DateTime** -waarde toegevoegd aan de blob URI om aan te geven, is de tijd waarop de momentopname. Als een pagina blob-URI is bijvoorbeeld `http://storagesample.core.blob.windows.net/mydrives/myvhd`, de momentopname URI vergelijkbaar met is `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Alle momentopnamen delen de basis blob-URI. Het enige onderscheid tussen de blob basis en de momentopname is de toegevoegde waarde voor **datum/tijd** .

Een blob kan een willekeurig aantal momentopnamen hebben. Momentopnamen blijft bestaan totdat ze expliciet worden verwijderd. Een momentopname kan niet de basis blob outlive. U kunt de momentopnamen die is gekoppeld aan de basis blob voor het bijhouden van uw huidige momentopnamen opsommen.

Wanneer u een momentopname van een blob maken, worden eigenschappen van de blob gekopieerd naar de momentopname met dezelfde waarden. De basis blob metagegevens wordt ook gekopieerd naar de momentopname, tenzij u afzonderlijke metagegevens voor de opname opgeven wanneer u deze maakt.

Een lease-overeenkomsten die zijn gekoppeld aan de basis blob is niet van invloed op de momentopname. U kan op een momentopname van een lease-overeenkomst niet verkrijgen.

## <a name="create-a-snapshot"></a>Een momentopname maken

In het volgende voorbeeld ziet u een momentopname maken in .NET. In dit voorbeeld geeft de afzonderlijke metagegevens voor de momentopname wanneer deze wordt gemaakt.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Momentopnamen kopiëren

Kopieer de veredeling BLOB's en momentopnamen volgen deze regels:

- De blob basis kunt u een momentopname kopiëren. Door het bevorderen van een momentopname van de positie van de blob base, kunt u een eerdere versie van een blob te herstellen. De momentopname blijft, maar de base blob met een beschrijfbare kopie van de momentopname wordt overschreven.

- U kunt een momentopname kopiëren naar een bestemming blob met een andere naam. De uiteindelijke bestemming blob is een blob beschrijfbare en niet een momentopname.

- Als een blob bron wordt gekopieerd, worden de momentopnamen van de blob bron worden niet gekopieerd naar de bestemming. Als een blob bestemming wordt overschreven door een exemplaar, blijven de momentopnamen die is gekoppeld aan de oorspronkelijke bestemming blob intact.

- Wanneer u een momentopname van een blok blob maken, wordt van de blob toegezegde blokkeringslijst ook gekopieerd naar de momentopname. Alle niet-toegewezen blokken worden niet gekopieerd.

## <a name="specify-an-access-condition"></a>Een access-voorwaarde opgeven

Een voorwaarde voor toegang kunt u opgeven dat de momentopname alleen gemaakt wordt als een voorwaarde is voldaan. Als u een access-voorwaarde, gebruikt u de eigenschap **AccessCondition** . Niet aan de opgegeven voorwaarde wordt voldaan, de momentopname wordt niet gemaakt als de Blob-service wordt de statuscode HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Momentopnamen verwijderen

U kunt een blob met momentopnamen niet verwijderen, tenzij de momentopnamen worden ook verwijderd. U kunt een momentopname van een afzonderlijk verwijderen of opgeven dat alle momentopnamen worden verwijderd wanneer de bron blob wordt verwijderd. Als u een blob die nog momentopnamen is verwijderd probeert, wordt een fout weergegeven.

In het volgende voorbeeld ziet u hoe een blob en de momentopnamen in .NET verwijdert waarbij `blockBlob` is een variabele van het type **CloudBlockBlob**:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Momentopnamen met Azure Premium opslag

Met behulp van momentopnamen met Premium-opslagruimte volgen deze regels:

- Het maximum aantal momentopnamen per pagina blob in een opslag premium account is 100. Als deze limiet wordt overschreden, wordt de momentopname Blob-bewerking foutcode 409 (**SnapshotCountExceeded**).

- U kunt een momentopname van een pagina blob in een opslag premium account om de 10 minuten. Als dat percentage wordt overschreden, wordt de momentopname Blob-bewerking foutcode 409 (**SnaphotOperationRateExceeded**).

- Blob ophalen om te lezen van een momentopname van een pagina blob in een opslag premium account kan niet worden aangeroepen. Aanroepen van Blob ophalen voor een opname in een opslag premium account retourneert foutcode 400 (**InvalidOperation**). U kunt echter Blob eigenschappen en Blob-metagegevens ophalen tegen een momentopname in een opslag premium account aanroepen.

- Om te lezen een momentopname, kunt u de kopie Blob-bewerking een momentopname kopiëren naar een andere pagina blob in de rekening. De blob bestemming voor de kopieerbewerking mag geen eventuele bestaande momentopnamen. Als de bestemming blob momentopnamen, retourneert de bewerking kopiëren Blob foutcode 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>De absolute URI terug naar een momentopname

In dit voorbeeld C#-code maakt een momentopname en schrijft de absolute URI voor de primaire locatie.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Begrijpen hoe momentopnamen kosten toenemen

Maken van een opname, een alleen-lezen kopie van een blob is, kan resulteren in extra gegevens opslag kosten op uw account. Bij het ontwerpen van uw toepassing, is het belangrijk te weten hoe deze kosten kunnen toenemen, zodat u kunt onnodige kosten minimaliseren.

### <a name="important-billing-considerations"></a>Belangrijke overwegingen voor facturering

De volgende lijst bevat belangrijke punten bij het maken van een opname.

- U account opslag maakt toeslagen voor unieke blokken of pagina's, of ze nu in de blob of in de momentopname. Uw account is niet tot extra kosten voor momentopnamen die is gekoppeld aan een blob totdat u de label waarop ze zijn gebaseerd bijwerken. Na het bijwerken van de basis blob is deze afwijkt van de momentopnamen. Als dit gebeurt, wordt u in rekening gebracht voor de unieke blokken of pagina's in elke blob of momentopname.

- Wanneer u een blok in een blob blok vervangt, dat blok wordt later in rekening gebracht als een unieke blok. Dit geldt zelfs als het blok heeft dezelfde ID blokkeren en dezelfde gegevens in de momentopname is. Nadat het blok vastgelegd wordt, dat afwijkt van het equivalent van een momentopname en brengt voor de gegevens. Hetzelfde geldt voor een pagina in een blob pagina die wordt bijgewerkt met dezelfde gegevens.

- Een blob blok vervangen door het aanroepen van de methode **UploadFile**, **UploadText**, **UploadStream**of **UploadByteArray** vervangt alle blokken in de blob. Als u een momentopname die is gekoppeld aan die blob, alle blokken in de basis blob en momentopname nu luidsprekers en brengt voor alle van de blokken in beide BLOB's. Dit geldt ook als de gegevens in de blob basis en de momentopname identiek.

- De Azure Blob-service beschikt niet over een manier om te bepalen of twee blokken identieke gegevens bevatten. Elk blok dat is geüpload en gepleegd wordt behandeld als uniek, zelfs als er al dezelfde gegevens en hetzelfde blok-ID. Omdat de kosten toenemen voor unieke blokken, is het belangrijk rekening te houden met een blob die een momentopname, worden extra unieke blokken en extra kosten heeft bijwerken.

> [AZURE.NOTE] Aanbevolen procedures voor dicteren beheren van momentopnamen zorgvuldig om te voorkomen dat de extra kosten. Wij raden aan het beheren van momentopnamen op de volgende wijze:

> - Verwijderen en opnieuw maken van momentopnamen die is gekoppeld aan een blob als de blob, zelfs als u met gegevens identiek zijn, bijwerken wilt tenzij het ontwerp van uw toepassing is vereist dat u momentopnamen onderhouden. Door te verwijderen en opnieuw maken van momentopnamen van de blob, kunt u ervoor zorgen dat de blob en momentopnamen niet luidsprekers.

> - Als u momentopnamen van een blob, Vermijd het aanroepen van **UploadFile**, **UploadText**, **UploadStream**of **UploadByteArray** om te werken de blob. Deze methoden vervangen alle van de blokken in de blob, zodat uw basis blob en momentopnamen signficantly luidsprekers. In plaats daarvan het kleinste aantal blokken bijwerken met behulp van de methoden **PutBlock** en **PutBlockList** .


### <a name="snapshot-billing-scenarios"></a>Momentopname van scenario's voor facturering


De volgende scenario's laten zien hoe toeslagen voor een blob blok en de momentopnamen laten aangroeien.

In scenario 1 is de basis blob niet bijgewerkt nadat u de momentopname is gemaakt, zodat er kosten verbonden zijn alleen unieke blokken 1, 2 en 3.

![Azure opslagbronnen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

In scenario 2 wordt de basis blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 is bijgewerkt en hoewel dezelfde gegevens en hetzelfde-ID bevat, het is niet hetzelfde als het blokkeren van 3 in de momentopname. Hierdoor wordt het account belast met vier blokken.

![Azure opslagbronnen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

In scenario 3 wordt de basis blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 is vervangen door blok 4 in de blob basis, maar wordt de momentopname van het blok 3. Hierdoor wordt het account belast met vier blokken.

![Azure opslagbronnen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

In scenario 4 wordt de basis blob is volledig bijgewerkt en bevat geen van de originele blokken. Daardoor is de rekening voor alle blokken van acht unieke betalen. In dit scenario kan optreden als u een update-methode zoals **UploadFile**, **UploadText**, **UploadFromStream**of **UploadByteArray**, omdat deze methoden vervangen door de inhoud van een blob.

![Azure opslagbronnen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer voorbeelden van Blob-opslag met [Azure codevoorbeelden](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Een voorbeeldtoepassing te downloaden en uit te voeren, of de code op GitHub te bladeren. 
