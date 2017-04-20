<properties
    pageTitle="Aan de slag met blob storage en Visual Studio verbonden diensten (cloud) | Microsoft Azure"
    description="Hoe aan de slag met Azure Blob-opslag in een cloud-service-project in Visual Studio nadat services verbinding maken met een opslag-account met behulp van Visual Studio worden verbonden"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure Blob-opslag en Visual Studio verbonden services (cloud services-projecten)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure Blob-opslag nadat u hebt gemaakt of een Azure opslag account waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** in een Visual Studio-project cloud services. U ziet en containers blob maken en het uitvoeren van veelvoorkomende taken, zoals het uploaden en downloaden van BLOB's aanbieden. De monsters zijn geschreven in C\# en de [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)gebruiken.

Azure Blob Storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die kan worden geopend vanuit overal in de wereld via HTTP of HTTPS. Een enkele blob kan groot zijn. BLOB's kunnen worden dingen zoals afbeeldingen, audio-en videobestanden, onbewerkte gegevens en bestanden.

Net als bestanden in mappen woont, live storage blobs in containers. Nadat u een opslag hebt gemaakt, maakt u een of meer recipiënten in de opslag. Bijvoorbeeld in een opslag 'Plakboek' genoemd, kunt u containers maken in de naam "afbeeldingen" voor het opslaan van afbeeldingen en andere zogenaamde "audio" audio-bestanden op te slaan. Nadat u de containers gemaakt, kunt u bestanden afzonderlijk blob om ze te uploaden.

- Zie voor meer informatie over het programmatisch manipuleren BLOB's [aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md).
- Zie voor algemene informatie over Azure opslag [opslag documentatie](https://azure.microsoft.com/documentation/services/storage/).
- Zie de [documentatie van Cloud-Services](https://azure.microsoft.com/documentation/services/cloud-services/)voor algemene informatie over Azure Cloud Services.
- Zie [ASP.NET](http://www.asp.net)voor meer informatie over het programmeren van ASP.NET-toepassingen.

## <a name="access-blob-containers-in-code"></a>Access blob containers in code

Als u via programmacode BLOB's in de cloud serviceprojecten, moet u de volgende items toevoegen als ze nog niet aanwezig.

1. De volgende code naamruimtedeclaraties aan de bovenkant van de C#-bestand waarin u via programmering toegang tot opslag Azure wilt toevoegen.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag. De volgende code gebruiken om de de verbindingsreeks voor opslag en opslag-accountgegevens van de configuratie van Azure service.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Krijg een **CloudBlobClient** -object verwijst naar een bestaande container in uw account voor opslag.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Krijg een **CloudBlobContainer** -object te verwijzen naar een specifieke blob-container.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Alle van de code die wordt weergegeven in de vorige procedure voor de code die wordt weergegeven in de volgende secties gebruiken.

## <a name="create-a-container-in-code"></a>Het maken van een container in code

> [AZURE.NOTE] Sommige API's die aanroepen van Azure opslag in ASP.NET uitvoeren zijn asynchroon. Zie [asynchroon asynchrone en Await programmeren](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De code in het volgende voorbeeld wordt ervan uitgegaan dat u asynchrone programmeermethoden.

Is maken van een container in uw account voor de opslag, hoeft u een aanroep van **CreateIfNotExistsAsync** zoals in de volgende code toevoegen:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Als u de bestanden in de container voor iedereen, kunt u de container als public met de volgende code instellen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste sleutel hebt.

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Azure opslag ondersteunt BLOB's blokkeren en BLOB's pagina. Blok blob is in de meeste gevallen het aanbevolen type te gebruiken.

Als u wilt een bestand uploadt naar een blob blok, u container en gebruikt het blok blob verwijst. Nadat u een verwijzing blob hebt, kunt u kunt de stroom van gegevens door het aanroepen van de methode **UploadFromStream** uploaden. Hiermee maakt u de blob als deze eerder niet bestond, of wordt deze overschreven als deze bestaat. In het volgende voorbeeld ziet u hoe een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

Als u het aantal BLOB's in een container, de eerste keer een verwijzing van de container. Vervolgens kunt u de container **ListBlobs** methode voor het ophalen van de BLOB's en/of mappen binnen het. Als u de uitgebreide verzameling eigenschappen en methoden voor een geretourneerde **IListBlobItem**, moet u het casten naar een object **CloudBlockBlob**, **CloudPageBlob**of **CloudBlobDirectory** . Als het type onbekend is, kunt u een typecontrole om te bepalen welke te casten. De volgende code laat zien hoe op te halen en de uitvoer van de URI van elk item in de container van de **foto's** :

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

Zoals in het vorige voorbeeld, is de blob echter het concept van mappen binnen ook containers. Dit is zodat u uw BLOB's in een map achtige structuur kunt. Neem bijvoorbeeld de volgende set van BLOB's blokkeren in een container met de naam **foto's**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Als u **ListBlobs** aanroept op de container (zoals in het vorige voorbeeld), bevat de geretourneerde collectie **CloudBlobDirectory** en **CloudBlockBlob** -objecten die de mappen en BLOB's die zich op het hoogste niveau. Dit is het eindresultaat:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Desgewenst kunt u de **UseFlatBlobListing** -parameter van de methode **ListBlobs** op **true**instellen. Dit resulteert in elke blob wordt geretourneerd als een **CloudBlockBlob**, ongeacht de map. Hier is het aanroepen van **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

en hier zijn de resultaten:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Zie [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx)voor meer informatie.

## <a name="download-blobs"></a>BLOB's downloaden

BLOB's downloaden, eerst een blob-objectverwijzing opgehaald en vervolgens de methode **DownloadToStream** aanroepen. In het volgende voorbeeld wordt de methode **DownloadToStream** gebruikt de blob inhoud overbrengen naar een streamobject dat u vervolgens naar een lokaal bestand kunt aanhouden.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

U kunt ook de methode **DownloadToStream** gebruiken voor het downloaden van de inhoud van een blob als een tekenreeks.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>BLOB's verwijderen

Verwijderen van een blob, de eerste keer een blob-referentie en roept vervolgens de methode **Delete** .

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchroon BLOB's in pagina's weergeven

Als u een groot aantal BLOB's aanbiedt, of wilt u het aantal resultaten dat u terug in één aanbieding bewerking bepalen, kunt u vermelden BLOB's in pagina's met resultaten. In dit voorbeeld ziet u hoe zoekresultaten in pagina's asynchroon, zodat kan worden uitgevoerd terwijl u wacht op een grote reeks resultaten retourneren niet wordt geblokkeerd.

In dit voorbeeld wordt een platte blob weergeven, maar u kunt ook een hiërarchische lijst uitvoeren door de **useFlatBlobListing** -parameter van de methode **ListBlobsSegmentedAsync** in te stellen op **false**.

Omdat de methode van het monster een asynchrone methode wordt aangeroepen, moet het worden voorafgegaan door het sleutelwoord **asynchrone** en het moet een **Task** -object als resultaat geven. Het sleutelwoord await is opgegeven voor de methode **ListBlobsSegmentedAsync** opschort van de methode monster totdat de lijst taak is voltooid.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
