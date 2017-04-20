<properties 
    pageTitle="Bestanden uploaden naar een Media-Services-account met behulp van .NET | Microsoft Azure" 
    description="Informatie over hoe u media-inhoud in de Media Services op maken en uploaden van activa." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Bestanden uploaden naar een Media-Services-account met behulp van .NET

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

In Media-Services u uploaden of consumptie van uw digitale bestanden in een actief. De entiteit **activa** kan bevatten, video, audio, afbeeldingen, miniaturen, verzamelingen, tekst tracks en ondertiteling bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden zijn geüpload, wordt de inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

De bestanden in de activa zijn **Activa bestanden**genoemd. Het exemplaar van de **AssetFile** en de werkelijke mediabestand zijn twee afzonderlijke objecten. De AssetFile instantie bevat metagegevens over het mediabestand terwijl het mediabestand dat de werkelijke media-inhoud bevat.

>[AZURE.NOTE]De volgende overwegingen zijn van toepassing bij het kiezen van de naam van een activum:
>
>- De waarde van de eigenschap IAssetFile.Name van Media Services wordt gebruikt bij het maken van URL's voor de stroomsgewijze inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Om deze reden is procent codering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens voor percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsextensie.
>
>- De lengte van de naam mag niet langer zijn dan 260 tekens.

Als u activa maakt, kunt u de volgende coderingsopties. 

- **Geen** - geen codering wordt gebruikt. Dit is de standaardwaarde. Er rekening mee dat wanneer u deze optie uw inhoud niet in doorvoer of opslag.
Als u van plan bent om een MP4 met progressieve download deze optie gebruiken. 
- **CommonEncryption** - Gebruik deze optie als u inhoud die al zijn gecodeerd en beveiligd met een gemeenschappelijke codering of PlayReady DRM (bijvoorbeeld goede Streaming beveiligd met DRM PlayReady) wilt uploaden.
- **EnvelopeEncrypted** : Gebruik deze optie als u HLS versleuteld met AES uploaden wilt. Houd er rekening mee dat de bestanden moeten zijn gecodeerd en gecodeerd door Manager transformeren.
- **StorageEncrypted** - codeert uw duidelijke inhoud lokaal met behulp van AES-256-bits codering en vervolgens uploaden naar Azure opslag waar het is opgeslagen gecodeerd in rust. Activa met opslag codering beveiligd worden ongecodeerd automatisch geplaatst in een gecodeerde-bestandssysteem voor de codering en eventueel opnieuw gecodeerd voordat u het uploadt terug als een nieuwe uitvoer actief. Het primaire gebruik geval voor opslag codering is als u wilt uw hoge kwaliteit invoermedium bestanden beveiligen met sterke codering in rust op schijf.

    Media Services biedt codering van opslag op schijf voor de activa, niet over-the-wire zoals Digital Rights Manager (DRM).

    Als het activum opslag gecodeerd is, moet u beleid voor levering van activa. Zie [configureren activa levering beleid](media-services-dotnet-configure-asset-delivery-policy.md)voor meer informatie.

Als u voor uw activa moeten worden gecodeerd met een **CommonEncrypted** , een **EnvelopeEncypted** optie of opgeeft, moet u het activum koppelen aan een **ContentKey**. Zie voor meer informatie, [het maken van een ContentKey](media-services-dotnet-create-contentkey.md). 

Als u voor uw activa moeten worden gecodeerd met een **StorageEncrypted** optie opgeeft, maakt de Media Services SDK voor .NET een **StorateEncrypted** **ContentKey** voor het activum.


Dit onderwerp bevat Media Services .NET SDK als Media Services .NET SDK extensies gebruiken om bestanden te uploaden naar een actief Media-Services.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Uploaden van een bestand met Media Services .NET SDK 

De onderstaande voorbeeldcode wordt .NET SDK gebruikt de volgende taken uitvoeren: 

- Hiermee maakt u een lege actief.
- Hiermee maakt u een AssetFile-exemplaar dat u wilt koppelen aan het activum.
- Maakt een instantie van AccessPolicy waarmee de machtigingen en de duur van de toegang naar het activum.
- Maakt een instantie Locator dat toegang tot de activa biedt.
- Een bestand uploadt in Media Services. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Uploaden van meerdere bestanden met Media Services .NET SDK 

De volgende code ziet u hoe een actief maken en uploaden van meerdere bestanden.

De code doet het volgende:
    
-   Hiermee maakt u een lege actief met de methode CreateEmptyAsset is gedefinieerd in de vorige stap.
    
-   Maakt een instantie van **AccessPolicy** waarmee de machtigingen en de duur van de toegang naar het activum.
    
-   Maakt een instantie **Locator** dat toegang tot de activa biedt.
    
-   Hiermee maakt u een exemplaar van **BlobTransferClient** . Dit type geeft een client die wordt uitgevoerd op de Azure BLOB's. In dit voorbeeld gebruiken we de client voor het controleren van de voortgang van het uploaden. 
    
-   Bestanden in de opgegeven directory wordt middels en maakt een instantie van het **AssetFile** voor elk bestand.
    
-   De bestanden worden geüpload in Media-Services met behulp van de methode **UploadAsync** . 
    
>[AZURE.NOTE] Gebruik de methode UploadAsync om ervoor te zorgen dat de gesprekken niet blokkeren en de bestanden zijn geüpload in parallel.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Houd rekening met het volgende tijdens het uploaden van een groot aantal activa.

- Maak een nieuw **CloudMediaContext** -object per thread. De klasse **CloudMediaContext** is niet thread-safe.
 
- NumberOfConcurrentTransfers vergroten van de standaardwaarde van 2 op een hogere waarde zoals 5. Deze instelling is van invloed op alle exemplaren van de **CloudMediaContext**. 
 
- Houdt de ParallelTransferThreadCount op de standaardwaarde is 10.
 
##<a id="ingest_in_bulk"></a>Activa die in Bulk, met behulp van Media Services .NET SDK ingesting 

Activa met grote bestanden uploaden, kan een knelpunt tijdens het maken van de activa zijn. Activa die in Bulk of "Bulk Ingesting" ingesting, heeft betrekking op actief maken van het uploadproces ontkoppeling. Maak een manifest (IngestManifest) beschrijving van de activa en de bijbehorende bestanden voor het gebruik van een ingesting benadering bulk. Gebruik vervolgens de uploadmethode van uw keuze de bijbehorende bestanden te uploaden naar een blob-container van het manifest. Microsoft Azure Media Services horloges de blob-container die is gekoppeld aan het manifest. Zodra een bestand wordt geüpload naar de container blob, heeft Microsoft Azure Media Services is voltooid het activum maken op basis van de configuratie van het activum in het manifest (IngestManifestAsset).


Om een nieuwe oproep voor IngestManifest de maken door op de CloudMediaContext de IngestManifests-collectie weergegeven. Deze methode maakt een nieuwe IngestManifest met de kennelijke die u opgeeft.

    IIngestManifest manifest = context.IngestManifests.Create(name);

De activa die gekoppeld aan de bulk IngestManifest zijn maken. Configureer de gewenste coderingsopties voor het activum voor bulk ingesting.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Een IngestManifestAsset wordt een actief met een massa van IngestManifest voor bulk ingesting. Ook de AssetFiles die elk activum wordt gekoppeld. Gebruik wilt maken van een IngestManifestAsset, de methode Create van de servercontext.

Het volgende voorbeeld wordt toegevoegd twee nieuwe IngestManifestAssets die de twee activa hebt gemaakt voor de massa koppelen nemen manifest. Elke IngestManifestAsset koppelt een set van bestanden die worden geüpload voor elk activum ook tijdens de bulk ingesting.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
U kunt een clienttoepassing hoge snelheid kan de activa-bestanden uploaden naar de blob storage container URI opgegeven door de eigenschap **IIngestManifest.BlobStorageUriForUpload** van de IngestManifest. Een opvallend hoge snelheid upload service is [Aspera op verzoek voor de toepassing van Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). U kunt ook code schrijven om te uploaden bestanden activa zoals in het volgende voorbeeld.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

De code voor het uploaden van bestanden van de activa voor de steekproef gebruikt in dit onderwerp wordt weergegeven in het volgende voorbeeld.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

U kunt de voortgang van de bulk ingesting voor alle activa die zijn gekoppeld aan een **IngestManifest** door de polling-eigenschap van de statistieken van de **IngestManifest**bepalen. Om voortgangsgegevens bijwerken, moet u een nieuwe **CloudMediaContext** elke keer dat u de eigenschap statistieken opvragen.

Het volgende voorbeeld wordt een IngestManifest met een **Id**polling.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Uploaden van bestanden met behulp van uitbreidingen voor .NET SDK 

In het volgende voorbeeld ziet u hoe met behulp van uitbreidingen voor .NET SDK één bestand uploaden. In dit geval de methode **CreateFromFile** wordt gebruikt, maar de asynchrone versie is ook beschikbaar (**CreateFromFileAsync**). De methode **CreateFromFile** kunt u de naam van de optie codering en een retouraanroep rapporteren van de voortgang van het uploaden van het bestand opgeven.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

In het volgende voorbeeld roept de functie UploadFile en opslag codering bevat als de optie voor het activum maken.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Volgende stap

Nu dat u een actief naar Media Services hebt geüpload, Ga naar het onderwerp van [het verkrijgen van een Media-Processor][] .

[Het verkrijgen van een Mediaprocessor]: media-services-get-media-processor.md
 
