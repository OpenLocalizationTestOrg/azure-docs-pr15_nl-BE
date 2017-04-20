<properties 
    pageTitle="Downloaden van Media-elementen" 
    description="Informatie over activa downloaden naar uw computer. Codevoorbeelden zijn geschreven in C# en de Media Services SDK voor .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Procedure: ervoor zorgen dat een actief door downloaden

In dit onderwerp worden opties voor het afspelen van media-elementen die zijn geüpload naar Media Services beschreven. In veel scenario's van toepassing kunt u de inhoud van Media Services leveren. U kunt downloaden van media-elementen of toegang tot deze met een locator. U kunt media-inhoud verzenden naar een andere toepassing of naar een andere inhoudsprovider. Voor betere prestaties en schaalbaarheid, kunt u ook inhoud leveren met behulp van een inhoud Delivery Network (CDN).

In dit voorbeeld ziet u hoe te downloaden van media-elementen van Media Services op uw lokale computer. De code opgevraagd van de taken die aan de Media Services-account is gekoppeld door taak-ID en wordt de collectie **OutputMediaAssets** (dit is de set van een of meer output media-elementen die het resultaat is van een taak wordt uitgevoerd). In dit voorbeeld ziet het downloaden van media-elementen voor uitvoer uit een taak, maar u kunt dezelfde aanpak voor het downloaden van andere activa van toepassing.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Zie ook 

[Streaming inhoud leveren](media-services-deliver-streaming-content.md)

