<properties
    pageTitle="Aan de slag met blob storage en Visual Studio verbonden services (ASP.NET 5) | Microsoft Azure"
    description="Hoe aan de slag met Azure Blob-opslag in een project van Visual Studio ASP.NET 5 nadat u hebt gemaakt met behulp van Visual Studio een opslag-account verbonden services"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure Blob-opslag in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een ASP.NET-5-project waarnaar wordt verwezen in het dialoogvenster Visual Studio verbonden Services toevoegen.

Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die kan worden geopend vanuit overal in de wereld via HTTP of HTTPS. Een enkele blob kan groot zijn. BLOB's kunnen worden dingen zoals afbeeldingen, audio-en videobestanden, onbewerkte gegevens en bestanden. In dit artikel wordt beschreven hoe aan de slag met blob-opslag nadat u een account Azure opslag maken met behulp van het dialoogvenster Visual Studio **Verbonden Services toevoegen** in een project voor ASP.NET-5.

Net als bestanden in mappen woont, live storage blobs in containers. Nadat u een opslag hebt gemaakt, maakt u een of meer recipiënten in de opslag. Bijvoorbeeld in een opslag 'Plakboek' genoemd, kunt u containers maken in de naam "afbeeldingen" voor het opslaan van afbeeldingen en andere zogenaamde "audio" audio-bestanden op te slaan. Nadat u de containers gemaakt, kunt u bestanden afzonderlijk blob om ze te uploaden. Zie [aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md) voor meer informatie over het programmatisch manipuleren BLOB's.

##<a name="access-blob-containers-in-code"></a>Access blob containers in code

Als u via programmacode BLOB's in ASP.NET 5 projecten, moet u de volgende items toevoegen als ze nog niet aanwezig.

1. De volgende code naamruimtedeclaraties aan de bovenkant van een C#-bestand waarin u programmatisch toegang verkrijgen tot Azure opslag wilt toevoegen.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag. De volgende code gebruiken om de de verbindingsreeks voor opslag en opslag-accountgegevens van de configuratie van Azure service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Opmerking:** Alle van de bovenstaande code voor de code gebruiken in de volgende secties.


3. Met een **CloudBlobClient** -object kunt u **CloudBlobContainer** naar een bestaande container in uw account voor opslag.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Het maken van een container in code

U kunt ook de **CloudBlobClient** voor het maken van een container in uw account voor opslag. Hoeft te doen is een aanroep van **CreateIfNotExistsAsync** zoals in de volgende code toevoegen:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Opmerking:** De API's die aanroepen naar Azure opslag in ASP.NET 5 uitvoeren zijn asynchroon. Zie [asynchroon asynchrone en Await programmeren](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De volgende code wordt ervan uitgegaan dat asynchrone programmering methoden worden gebruikt.

Als u de bestanden in de container voor iedereen, kunt u de container als public met de volgende code instellen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Een blob-bestand uploaden naar een container, u container en gebruikt het blob verwijst. Nadat u een verwijzing blob hebt, kunt u kunt de stroom van gegevens door het aanroepen van de methode **UploadFromStreamAsync** uploaden. Hiermee maakt u de blob als deze nog niet is ingevuld, of het overschreven als deze bestaat. In het volgende voorbeeld ziet u hoe een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container
Als u het aantal BLOB's in een container, de eerste keer een verwijzing van de container. Vervolgens kunt u de **ListBlobsSegmentedAsync** -methode van de container voor het ophalen van de BLOB's en/of mappen binnen het aanroepen. Als u de uitgebreide verzameling eigenschappen en methoden voor een geretourneerde **IListBlobItem**, moet u het casten naar een object **CloudBlockBlob**, **CloudPageBlob**of **CloudBlobDirectory** . Als u het type blob niet weet, kunt u een typecontrole om te bepalen welke cast deze naar. De volgende code laat zien hoe op te halen en de uitvoer van de URI van elk item in een container.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Er zijn nog andere manieren om de inhoud van een container blob. Zie [aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) voor meer informatie.

##<a name="download-a-blob"></a>Een blob downloaden
Downloaden van een blob, eerst een verwijzing naar de blob ophalen en vervolgens de methode **DownloadToStreamAsync** aanroepen. In het volgende voorbeeld wordt de methode **DownloadToStreamAsync** gebruikt om de blob inhoud overbrengen naar een streamobject dat u kunt opslaan als een lokaal bestand.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Er zijn andere manieren om het opslaan van BLOB's als bestanden. Zie [aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) voor meer informatie.

##<a name="delete-a-blob"></a>Een blob verwijderen
Een blob verwijderen en vervolgens de methode **DeleteAsync** aanroepen op het krijgt eerst een verwijzing naar de blob.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
