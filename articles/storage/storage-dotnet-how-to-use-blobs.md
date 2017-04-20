<properties
    pageTitle="Aan de slag met Azure Blob-opslag (opslag van objecten) met behulp van .NET | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure Blob-opslag (opslag van objecten)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Aan de slag met Azure Blob-opslag met behulp van .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure Blob storage is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. BLOB-opslag kan tekst of binaire gegevens, zoals een document, bestand of installatieprogramma voor elk type worden opgeslagen. BLOB-opslag is ook opslag van objecten genoemd.

### <a name="about-this-tutorial"></a>Over deze handleiding

Deze zelfstudie laat zien hoe .NET code schrijven voor enkele algemene scenario's met Azure Blob-opslag. Scenario's gedekt zijn geüpload, aanbieding, downloaden en verwijderen van BLOB's.

**Geschatte tijd:** 45 minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure opslag Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Een [Azure opslag account](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Meer voorbeelden

Zie voor meer voorbeelden van Blob-opslag met [Aan de slag met Azure Blob-opslag in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). De voorbeeldtoepassing te downloaden en uit te voeren, of de code op GitHub te bladeren.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

Voeg de volgende `using` instructies aan de bovenkant van de `program.cs` bestand:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>De Blob-client maken

De klasse **CloudBlobClient** kunt u ophalen van containers en BLOB's opgeslagen in de Blob-opslag. Hier is één manier om de client te maken:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

U bent nu gereed om code te schrijven die de gegevens leest uit en schrijft gegevens naar een Blob-opslag.

## <a name="create-a-container"></a>Maken van een container

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

In dit voorbeeld ziet u hoe een container maken als deze nog niet bestaat:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Nieuwe container is standaard persoonlijk, wat betekent dat u de toegangstoets opslag voor het BLOB's downloaden van deze container moet opgeven. Als u wilt dat de bestanden in de container beschikbaar maken voor iedereen, kunt u instellen dat de container als public met de volgende code:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u de toegangstoets rekening of een handtekening gedeelde toegang hebt.

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Azure Blob-opslag ondersteunt BLOB's blokkeren en BLOB's pagina.  Blok blob is in de meeste gevallen het aanbevolen type te gebruiken.

Als u wilt een bestand uploadt naar een blob blok, u container en gebruikt het blok blob verwijst. Nadat u een verwijzing blob hebt, kunt u kunt de stroom van gegevens door het aanroepen van de methode **UploadFromStream** uploaden. Deze bewerking wordt de blob maken als deze niet eerder bestaan of deze overschrijven als deze bestaat.

In het volgende voorbeeld ziet u hoe een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

Als u het aantal BLOB's in een container, de eerste keer een verwijzing van de container. Vervolgens kunt u de container **ListBlobs** methode voor het ophalen van de BLOB's en/of mappen binnen het. Als u de uitgebreide verzameling eigenschappen en methoden voor een geretourneerde **IListBlobItem**, moet u het casten naar een object **CloudBlockBlob**, **CloudPageBlob**of **CloudBlobDirectory** .  Als het type onbekend is, kunt u een typecontrole om te bepalen welke te casten.  De volgende code laat zien hoe op te halen en de uitvoer van de URI van elk item in de `photos` container:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Zoals hierboven vermeld, kunt u de naam BLOB's met informatie over het pad in hun naam. Hiermee maakt u een virtuele mapstructuur die u kunt ordenen en bladeren zoals u zou doen met een traditioneel systeem. Houd er rekening mee dat de directory-structuur alleen virtueel is-de enige beschikbare bronnen in Blob-opslag containers en BLOB's zijn. De client-bibliotheek van opslag biedt echter een **CloudBlobDirectory** -object als u wilt verwijzen naar een virtuele map en het vereenvoudigen van het proces van het werken met BLOB's die op deze manier zijn gerangschikt.

Neem bijvoorbeeld de volgende set van BLOB's blokkeren in een container met de naam `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Als u **ListBlobs** aanroept op de container 'foto's ' (zoals in het bovenstaande voorbeeld), wordt een hiërarchische lijst geretourneerd. Het bevat zowel **CloudBlobDirectory** als **CloudBlockBlob** -objecten, mappen en BLOB's in de container, respectievelijk. De resulterende uitvoer ziet:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Desgewenst kunt u de **UseFlatBlobListing** -parameter van de methode **ListBlobs** op **true**instellen. In dit geval wordt elke blob in de container geretourneerd als een **CloudBlockBlob** -object. De aanroep van **ListBlobs** om terug te keren een platte lijst ziet er zo uit:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

en de resultaten zien er als volgt:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>BLOB's downloaden

BLOB's downloaden, eerst een blob-objectverwijzing opgehaald en vervolgens de methode **DownloadToStream** aanroepen. In het volgende voorbeeld wordt de methode **DownloadToStream** gebruikt de blob inhoud overbrengen naar een streamobject dat u vervolgens naar een lokaal bestand kunt aanhouden.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

U kunt ook de methode **DownloadToStream** gebruiken voor het downloaden van de inhoud van een blob als een tekenreeks.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>BLOB's verwijderen

Verwijderen van een blob, de eerste keer een blob-referentie en roept vervolgens de methode **Delete** op het.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchroon BLOB's in pagina's weergeven

Als u een groot aantal BLOB's aanbiedt, of wilt u het aantal resultaten dat u terug in één aanbieding bewerking bepalen, kunt u vermelden BLOB's in pagina's met resultaten. In dit voorbeeld ziet u hoe zoekresultaten in pagina's asynchroon, zodat kan worden uitgevoerd terwijl u wacht op een grote reeks resultaten retourneren niet wordt geblokkeerd.

In dit voorbeeld wordt een platte blob weergeven, maar u kunt ook een hiërarchische lijst uitvoeren door het instellen van de `useFlatBlobListing` -parameter van de methode **ListBlobsSegmentedAsync** `false`.

Omdat de methode van het monster een asynchrone methode wordt aangeroepen, moet deze voorafgegaan door de `async` en het trefwoord moet een **Task** -object retourneren. Het sleutelwoord await is opgegeven voor de methode **ListBlobsSegmentedAsync** opschort van de methode monster totdat de lijst taak is voltooid.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>Schrijven naar een blob toevoegen

Een toevoegquery blob is een nieuw type blob, geïntroduceerd in versie 5.x van de clientbibliotheek Azure opslag voor .NET. Een toevoegquery blob is geoptimaliseerd voor append bewerkingen, zoals het aanmelden. Net als een blob blok een blob append bestaat uit blokken, maar als u een nieuw blok aan een blob toevoegen toevoegen, altijd wordt toegevoegd aan het einde van de blob. U kan bijwerken of verwijderen van een bestaand notitieblok in een blob toevoegen. De blokkering van id's voor een blob toevoegen niet beschikbaar zijn als voor een blob blokkeren.

Elk blok in een toevoegquery blob kan een ander formaat, met een maximum van 4 MB en een blob append een maximum van 50.000 blokken kunt opnemen. De maximale grootte van een toevoegquery blob is dus iets meer dan 195 GB (4 MB X 50.000 blokken).

In het volgende voorbeeld wordt een nieuwe append blob gemaakt en sommige worden gegevens toegevoegd aan het simuleren van een eenvoudige registratie.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Zie [Wat blok BLOB's, pagina-Blobs en BLOB's toevoegen](https://msdn.microsoft.com/library/azure/ee691964.aspx) voor meer informatie over de verschillen tussen de drie soorten BLOB's.

## <a name="managing-security-for-blobs"></a>Beveiliging voor BLOB's beheren

Standaard beveiligt Azure opslag uw gegevens door het beperken van toegang tot de rekeninghouder, die in het bezit van de toegangstoetsen account. Wanneer u voor het delen van blob-gegevens in uw account voor de opslag, is het belangrijk om dit te doen zonder de beveiliging van uw account toegangstoetsen. U kunt bovendien coderen zodat is het veilig via de kabel en in de opslag van Azure blob-gegevens.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Toegang tot de blob-gegevens beheren

De blob-gegevens in uw account opslag is standaard alleen toegankelijk voor de rekeninghouder opslag. Verificatie aanvragen tegen de Blob-opslag is de toegangstoets account standaard vereist. U kunt echter bepaalde blob-gegevens beschikbaar stellen aan andere gebruikers. U hebt twee opties:

- **Anonieme toegang:** U kunt een container of het aantal BLOB's openbaar beschikbaar maken voor anonieme toegang. Zie [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md) voor meer informatie.
- **Gedeelde toegang handtekeningen:** U kunt clients voorzien van een handtekening van gedeelde toegang (SAS), die de gedelegeerde toegang tot een bron in uw account opslag biedt met de machtigingen die u opgeeft en op een interval dat u opgeeft. [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md) Zie voor meer informatie.

### <a name="encrypting-blob-data"></a>Blob-gegevens coderen

Azure opslag ondersteunt blob-gegevens op de client en de server te coderen:

- **Client-side codering:** Client-bibliotheek van de opslag voor .NET biedt ondersteuning voor het coderen van gegevens binnen toepassingen voordat u gaat uploaden naar Azure opslag en decoderen van gegevens tijdens het downloaden van de client. De bibliotheek ondersteunt ook integratie met Azure sleutel kluis voor opslag account managers. Zie [Client-Side-codering met .NET voor Microsoft Azure opslag](storage-client-side-encryption.md) voor meer informatie. Zie ook [Zelfstudie: coderen en decoderen van BLOB's in Microsoft Azure opslag Azure sleutel kluis met](storage-encrypt-decrypt-blobs-key-vault.md).
- **Server-side codering**: Azure opslag biedt nu ondersteuning voor server-side-codering. Zie [opslag Azure Service codering](storage-service-encryption.md)voor gegevens in rust (voorbeeld).

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Blob-opslag hebt geleerd, volg deze koppelingen voor meer informatie.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Opslagverkenner
- [Azure opslag Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis standalone app van Microsoft waarmee u visueel werken met gegevens in Azure opslag op Windows, OS X en Linux.

### <a name="blob-storage-samples"></a>Voorbeelden van BLOB-opslag

- [Aan de slag met Azure Blob-opslag in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Verwijzing naar BLOB-opslag

- [Client-bibliotheek van opslag voor referentie .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST API: naslag](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Conceptuele hulplijnen

- [Gegevens overbrengen met het opdrachtregelprogramma AzCopy](storage-use-azcopy.md)
- [Aan de slag met de opslag van bestanden voor .NET](storage-dotnet-how-to-use-files.md)
- [Azure blob-opslag gebruiken met de WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
