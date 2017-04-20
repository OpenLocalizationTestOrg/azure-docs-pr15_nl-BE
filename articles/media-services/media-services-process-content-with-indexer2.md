<properties
    pageTitle="Indexering van mediabestanden met Azure Media indexeerfunctie 2 Preview | Microsoft Azure"
    description="Azure Media indexeerfunctie kunt u de inhoud van uw mediabestanden doorzoekbare en voor het genereren van een transcript van de volledige tekst voor closed captioning en trefwoorden. Dit onderwerp wordt beschreven hoe Media indexeerfunctie 2 Preview gebruiken."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indexering van mediabestanden met Azure Media indexeerfunctie 2 Preview

##<a name="overview"></a>Overzicht

De processor in **Azure Media indexeerfunctie 2 Preview** media (MP) kunt u mediabestanden en inhoud doorzoekbaar maken als gesloten closed captioning nummers te genereren. Vergeleken met de vorige versie van [Media voor indexering Azure](media-services-index-content.md), **Azure Media indexeerfunctie 2 Preview** sneller indexeren uitvoert en bredere ondersteuning biedt. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees, Portugees en Arabisch.

**Azure Media indexeerfunctie 2 Preview** MP is momenteel in de voorvertoning.

In dit onderwerp wordt beschreven hoe indexeertaken met **Azure Media indexeerfunctie 2 Preview**.

>[AZURE.NOTE]De volgende overwegingen zijn van toepassing:
>
>2 indexeerfunctie wordt niet ondersteund in Azure China en de regering van Azure.
>
>Het voorbeeld is beperkt tot ~ 10 minuten van de verwerking, maar is gratis voor alle klanten.
>
>Wanneer u inhoud, zorg ervoor dat mediabestanden die zeer duidelijke speech (zonder achtergrondmuziek, geluid, effecten of hiss microfoon zijn) gebruiken. Voorbeelden van geschikte inhoud zijn: vergaderingen, lezingen en presentaties opgenomen. De volgende inhoud mogelijk niet geschikt voor indexering: films, tv-programma's, alles met gemengde audio- en geluidseffecten, slecht opgenomen inhoud met achtergrondruis (hiss).


Dit onderwerp bevat details over **Azure Media indexeerfunctie 2 Preview** en ziet u hoe u met Media Services SDK voor .NET gebruiken

##<a name="input-and-output-files"></a>Invoer en uitvoer van bestanden

###<a name="input-files"></a>Invoerbestanden

Audio-of videobestanden

###<a name="output-files"></a>Output-bestanden

Een taak indexing kunt ondertiteling bestanden genereren in de volgende indelingen:  

- **Sami**
- **TTML**
- **WebVTT**

Gesloten bijschrift (CC) bestanden in deze indeling kunnen worden gebruikt om audio en video bestanden toegankelijk maken voor mensen met auditieve handicap.

##<a name="task-configuration-preset"></a>Taken configureren (definitie)

Als u een taak indexing met **Azure Media indexeerfunctie 2 Preview**, moet u een voorinstelling voor configuratie.

De volgende JSON stelt parameters beschikbaar.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Ondersteunde talen  

Azure Media indexeerfunctie 2 Preview ondersteunt spraak-naar-tekst voor de volgende talen (bij het opgeven van de taalnaam van de in de configuratie van de taak, code gebruik 4 tekens tussen haakjes als volgt):

- Engels [EnUs]
- Spaans [EsEs]
- Chinees [ZhCn]
- Frans [FrFr]
- Duits [DeDe]
- Italiaans [ItIt]
- Portugees [PtBr]
- Arabisch (Egyptisch) [ArEg]


## <a name="sample-code"></a>Voorbeeld van code

Het volgende programma ziet u hoe:

1. Maak een actief en een media-bestand uploaden naar het activum.
1. Een taak maakt met een indexering op basis van een configuratiebestand met de volgende definitie van json-taak.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. De uitvoerbestanden downloads. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                static void Main(string[] args)
                {
        
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        
                    progressJobTask.Wait();
        
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
        
                    return job.OutputMediaAssets[0];
                }
        
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
        
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
        
                    return asset;
                }
        
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
        
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
        
                    return processor;
                }
        
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
            }
        }


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)