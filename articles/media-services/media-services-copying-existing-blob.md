<properties 
    pageTitle="Een bestaande Blob kopiëren naar een Media-Services actief | Microsoft Azure" 
    description="In dit onderwerp ziet u hoe u een bestaande blob kopiëren naar een actief Media-Services." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/13/2016" 
    ms.author="juliako"/>

#<a name="copying-an-existing-blob-into-a-media-services-asset"></a>Een bestaande Blob kopiëren naar een actief Media-Services

In dit onderwerp ziet u hoe u BLOB's vanuit een opslag account kopiëren naar een nieuwe Microsoft Azure Media Services actief.

De BLOB's kunnen bestaan in een opslag-account die is gekoppeld aan Media Services-account, of opslag-account is niet gekoppeld aan Media Services-account. In dit onderwerp wordt beschreven hoe BLOB's kopiëren van een account voor opslag naar een actief Media-Services. Opmerking u kunt ook kopiëren tussen datacenters. Echter, kunnen er kosten door dit te doen. Zie voor meer informatie over prijzen, [Gegevensoverdracht](https://azure.microsoft.com/pricing/#header-11).

>[AZURE.NOTE] U moet niet proberen de inhoud van de blob-containers die zijn gegenereerd door de Media Services zonder Media Service API's wijzigen.

##<a name="download-sample"></a>Voorbeeld downloaden

Ophalen en uitvoeren van een steekproef van [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/).

##<a name="prerequisites"></a>Vereisten

- Twee Media Services accounts in een nieuw of bestaand Azure abonnement. Zie het onderwerp [een Media-Services-Account te maken](media-services-portal-create-account.md).
- Besturingssystemen: Windows 10, Windows 7, Windows 2008 R2 of Windows 8.
- .NET framework 4.5.
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express) of hoger.

##<a name="set-up-your-project"></a>Uw project instellen

In deze sectie maakt en een consoletoepassing in C#-project instellen.

1. Visual Studio gebruiken voor het maken van een nieuwe oplossing die met het project consoletoepassing in C#. 
2. Voer CopyExistingBlobsIntoAsset voor de naam en klik vervolgens op OK.
1. Gebruik Nuget toevoegen van verwijzingen naar verwante Media Services-dll's. In Visual Studio in het hoofdmenu, selecteer Extra -> bibliotheek Package Manager -> Package Manager-Console. Typ in het console venster installatie pakket windowsazure.mediaservices en druk op enter.
1. Andere referenties die vereist voor dit project zijn toevoegen: System.Configuration.
1. Vervangen met behulp van instructies die zijn toegevoegd aan het bestand Programs.cs met de volgende standaard:
        
        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using System.Collections.Generic;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure;
        using System.Web;
        using Microsoft.WindowsAzure.Storage.Blob;
        using Microsoft.WindowsAzure.Storage.Auth;

1. De sectie appSettings naar het .config-bestand toevoegen en bijwerken van de waarden op basis van uw sleutel Media Services en de opslag en waarden. 

        <appSettings>
          <add key="MediaServicesAccountName" value="Media-Services-Account-Name"/>
          <add key="MediaServicesAccountKey" value="Media-Services-Account-Key"/>
          <add key="MediaServicesStorageAccountName" value="Media-Services-Storage-Account-Name"/>
          <add key="MediaServicesStorageAccountKey" value="Media-Services-Storage-Account-Key"/>
          <add key="ExternalStorageAccountName" value="External-Storage-Account-Name"/>
          <add key="ExternalStorageAccountKey" value="External-Storage-Account-Key"/>
        </appSettings>


##<a name="copy-blobs-from-a-storage-account-into-a-media-services-asset"></a>BLOB's kopiëren vanaf een Account voor opslag in een actief Media-Services

In het onderstaande voorbeeld worden de volgende taken uitgevoerd:

1. Hiermee maakt u het exemplaar van de CloudMediaContext. 
1. CloudStorageAccount exemplaren gemaakt: _sourceStorageAccount en _destinationStorageAccount.
1. Goede Streaming-bestanden van een lokale map uploads naar een blob-container bevindt zich in _sourceStorageAccount. 
1. Hiermee maakt u een nieuw activum. De blob-container die wordt gemaakt voor dit activum bevindt zich in _destinationStorageAccount. 
1. Azure opslag SDK wordt gekopieerd van het opgegeven aantal BLOB's in de container die is gekoppeld aan het activum.

    >[AZURE.NOTE]De kopieerbewerking is niet genereert een uitzondering als de locator is verlopen.

1. Aangezien in dit voorbeeld vloeiend streaming-bestanden worden gekopieerd in het voorbeeld ziet hoe u het bestand .ism naar het primaire bestand zijn. Als bijvoorbeeld we een .mp4 bestand gekopieerd, zou het mp4-bestand worden ingesteld om het primaire bestand.
1. De soepele Streaming URL maakt voor de OnDemandOrigin-locator die aan het activum is gekoppeld. 
            
        class Program
        {
            // Read values from the App.config file. 
            static string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
            static string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            static string _storageAccountName = ConfigurationManager.AppSettings["MediaServicesStorageAccountName"];
            static string _storageAccountKey = ConfigurationManager.AppSettings["MediaServicesStorageAccountKey"];
            static string _externalStorageAccountName = ConfigurationManager.AppSettings["ExternalStorageAccountName"];
            static string _externalStorageAccountKey = ConfigurationManager.AppSettings["ExternalStorageAccountKey"];

            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            private static CloudStorageAccount _sourceStorageAccount = null;
            private static CloudStorageAccount _destinationStorageAccount = null;

            static void Main(string[] args)
            {
            _cachedCredentials = new MediaServicesCredentials(
                    _accountName,
                    _accountKey);
            // Use the cached credentials to create CloudMediaContext.
            _context = new CloudMediaContext(_cachedCredentials);

            // In this example the storage account from which we copy blobs is not 
            // associated with the Media Services account into which we copy blobs.
            // But the same code will work for coping blobs from a storage account that is 
            // associated with the Media Services account.
            //
            // Get a reference to a storage account that is not associated with a Media Services account
            // (an external account).  
            StorageCredentials externalStorageCredentials =
                new StorageCredentials(_externalStorageAccountName, _externalStorageAccountKey);
            _sourceStorageAccount = new CloudStorageAccount(externalStorageCredentials, true);

            //Get a reference to the storage account that is associated with a Media Services account. 
            StorageCredentials mediaServicesStorageCredentials =
                new StorageCredentials(_storageAccountName, _storageAccountKey);
            _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);

            // Upload Smooth Streaming files into a storage account.
            string localMediaDir = @"C:\supportFiles\streamingfiles";
            CloudBlobContainer blobContainer =
                UploadContentToStorageAccount(localMediaDir);

            // Create a new asset and copy the smooth streaming files into 
            // the container that is associated with the asset.
            IAsset asset = CreateAssetFromExistingBlobs(blobContainer);

            // Get the streaming URL for the smooth streaming files 
            // that were copied into the asset.   
            string urlForClientStreaming = CreateStreamingLocator(asset);
            Console.WriteLine("Smooth Streaming URL: " + urlForClientStreaming);

            Console.ReadLine();
            }

            /// <summary>
            /// Uploads content from a local directory into the specified storage account.
            /// In this example the storage account is not associated with the Media Services account.
            /// </summary>
            /// <param name="localPath">The path from which to upload the files.</param>
            /// <returns>The container that contains the uploaded files.</returns>
            static public CloudBlobContainer UploadContentToStorageAccount(string localPath)
            {
            CloudBlobClient externalCloudBlobClient = _sourceStorageAccount.CreateCloudBlobClient();

            CloudBlobContainer externalMediaBlobContainer = externalCloudBlobClient.GetContainerReference("streamingfiles");

            externalMediaBlobContainer.CreateIfNotExists();

            // Upload files to the blob container.  
            DirectoryInfo uploadDirectory = new DirectoryInfo(localPath);
            foreach (var file in uploadDirectory.EnumerateFiles())
            {
                CloudBlockBlob blob = externalMediaBlobContainer.GetBlockBlobReference(file.Name);

                blob.UploadFromFile(file.FullName, FileMode.Open);
            }

            return externalMediaBlobContainer;
            }

            /// <summary>
            /// Creates a new asset and copies blobs from the specifed storage account.
            /// </summary>
            /// <param name="mediaBlobContainer">The specified blob container.</param>
            /// <returns>The new asset.</returns>
            static public IAsset CreateAssetFromExistingBlobs(CloudBlobContainer mediaBlobContainer)
            {
            // Create a new asset. 
            IAsset asset = _context.Assets.Create("NewAsset_" + Guid.NewGuid(), AssetCreationOptions.None);

            IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
                TimeSpan.FromHours(24), AccessPermissions.Write);
            ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);

            CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

            // Get the asset container URI and Blob copy from mediaContainer to assetContainer. 
            string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];

            CloudBlobContainer assetContainer =
                destBlobStorage.GetContainerReference(destinationContainerName);

            if (assetContainer.CreateIfNotExists())
            {
                assetContainer.SetPermissions(new BlobContainerPermissions
                {
                PublicAccess = BlobContainerPublicAccessType.Blob
                });
            }

            var blobList = mediaBlobContainer.ListBlobs();
            foreach (var sourceBlob in blobList)
            {
                var assetFile = asset.AssetFiles.Create((sourceBlob as ICloudBlob).Name);
                CopyBlob(sourceBlob as ICloudBlob, assetContainer);
                assetFile.ContentFileSize = (sourceBlob as ICloudBlob).Properties.Length;
                assetFile.Update();
            }

            asset.Update();

            destinationLocator.Delete();
            writePolicy.Delete();

            // Since we copied a set of Smooth Streaming files, 
            // set the .ism file to be the primary file. 
            // If we, for example, copied an .mp4, then the mp4 would be the primary file. 
            SetISMFileAsPrimary(asset);

            return asset;
            }

            /// <summary>
            /// Creates the OnDemandOrigin locator in order to get the streaming URL.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            /// <returns>The streaming URL.</returns>
            static public string CreateStreamingLocator(IAsset asset)
            {
            var ismAssetFile = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).First();

            // Create a 30-day readonly access policy. 
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                TimeSpan.FromDays(30),
                AccessPermissions.Read);

            // Create a locator to the streaming content on an origin. 
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                policy,
                DateTime.UtcNow.AddMinutes(-5));

            return originLocator.Path + ismAssetFile.Name + "/manifest";
            }

            /// <summary>
            /// Copies the specified blob into the specified container.
            /// </summary>
            /// <param name="sourceBlob">The source container.</param>
            /// <param name="destinationContainer">The destination container.</param>
            static private void CopyBlob(ICloudBlob sourceBlob, CloudBlobContainer destinationContainer)
            {
            var signature = sourceBlob.GetSharedAccessSignature(new SharedAccessBlobPolicy
            {
                Permissions = SharedAccessBlobPermissions.Read,
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
            });

            ICloudBlob destinationBlob = destinationContainer.GetBlockBlobReference(sourceBlob.Name);

            if (destinationBlob.Exists())
            {
                Console.WriteLine(string.Format("Destination blob '{0}' already exists. Skipping.", destinationBlob.Uri));
            }
            else
            {

                // Display the size of the source blob.
                Console.WriteLine(sourceBlob.Properties.Length);

                Console.WriteLine(string.Format("Copy blob '{0}' to '{1}'", sourceBlob.Uri, destinationBlob.Uri));
                destinationBlob.StartCopyFromBlob(new Uri(sourceBlob.Uri.AbsoluteUri + signature));

                while (true)
                {
                // The StartCopyFromBlob is an async operation, 
                // so we want to check if the copy operation is completed before proceeding. 
                // To do that, we call FetchAttributes on the blob and check the CopyStatus. 
                destinationBlob.FetchAttributes();
                if (destinationBlob.CopyState.Status != CopyStatus.Pending)
                {
                    break;
                }
                //It's still not completed. So wait for some time.
                System.Threading.Thread.Sleep(1000);
                }


                // Display the size of the destination blob.
                Console.WriteLine(destinationBlob.Properties.Length);

            }
            }

            /// <summary>
            /// Sets a file with the .ism extension as a primary file.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            static private void SetISMFileAsPrimary(IAsset asset)
            {

            //If you expect the asset to contain the .ism asset file, set the .ism file as the primary file.
            var ismAssetFiles = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();

            // The following code assigns the first .ism file as the primary file in the asset.
            // An asset should have one .ism file.  
            ismAssetFiles.First().IsPrimary = true;
            ismAssetFiles.First().Update();
            }
        }

 

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
