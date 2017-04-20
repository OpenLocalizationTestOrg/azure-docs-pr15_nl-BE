<properties
    pageTitle="Geconfronteerd redactie met Azure media-analytics | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe vlakken met Azure media analytics redigeren."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Redactie met Azure media analytics gezicht

##<a name="overview"></a>Overzicht

**Azure Media Redactor** is een [Media-Analytics Azure](media-services-analytics-overview.md) media-processor (MP) biedt schaalbare gezicht redactie in de cloud. Redactie gezicht kunt u uw video te vervagen vlakken van geselecteerde gebruikers wijzigen. U kunt de redactie gezicht service gebruiken in scenario's voor openbare veiligheid en de nieuws media. Een paar minuten beeldmateriaal met meerdere vlakken uren Redigeren handmatig kunnen nemen, maar met deze service vereist de redactie gezicht proces een paar eenvoudige stappen. Zie [deze](https://azure.microsoft.com/blog/azure-media-redactor/) blog voor meer informatie.

Dit onderwerp bevat details over **Azure Media Redactor** en ziet u hoe u met Media Services SDK voor .NET gebruiken.

**Azure Media Redactor** MP is momenteel in de voorvertoning.

## <a name="face-redaction-modes"></a>Nominale redactie modi

Vlekken redactie werkt door het detecteren van vlakken in elk frame van de video en het gezicht-object beide voorwaarts en achterwaarts in de tijd bijhouden zodat dezelfde persoon kan worden vaag uit ook andere hoeken. De redactie van geautomatiseerde proces is zeer complex en heeft niet altijd 100% produceren van de gewenste uitvoer, om deze reden Media Analytics beschikt u over een aantal manieren voor het wijzigen van de uiteindelijke uitvoer.

Naast een volledig automatische modus is er een twee keer workflow waarmee de selectie/de-selection gevonden vlakken via een lijst met id's. Wordt ook een bestand met metagegevens in JSON-indeling om willekeurige per frame aanpassingen van het Beheerderspaneel. Deze werkstroom is opgedeeld in de modi **analyseren** en **Redact** . U kunt combineren de twee modi in één stap met beide taken in een project; in deze modus wordt de **gecombineerde**genoemd.

###<a name="combined-mode"></a>Gecombineerde modus

Dit wordt automatisch een geredigeerde mp4 produceren zonder eventuele handmatige invoer.

Werkgebied|Bestandsnaam|Notities
---|---|---
Invoer van activa|foo.bar|Video in WMV, MOV- en MP4-indeling
Invoer-config|De configuratie van vooraf ingestelde|{'version':'1.0 ', 'opties': {'mode': de combinatie'}}
Uitvoer activa|foo_redacted.mp4|Video met vervaging toegepast

####<a name="input-example"></a>Voorbeeld van invoer:

[Deze video bekijken](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Voorbeeld van de uitvoer:

[Deze video bekijken](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Modus analyseren

Het **analyseren** van de werkstroom twee keer neemt een video-invoer en produceert een JSON-bestand van gezicht locaties en jpg-afbeeldingen van elk gevonden vlak.

Werkgebied|Bestandsnaam|Notities
---|---|----
Invoer van activa|foo.bar|Video in WMV, MPV of MP4-indeling
Invoer-config|De configuratie van vooraf ingestelde|{'version':'1.0 ', 'opties': {'mode': 'analyseren'}}
Uitvoer activa|foo_annotations.JSON|Gegevens aantekening van gezicht locaties in JSON-indeling. Dit kan worden bewerkt door de gebruiker voor het wijzigen van de omsluitende kaders vervagen. Zie onderstaand voorbeeld.
Uitvoer activa|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Van elke bijgesneden jpg gezicht, waar het nummer de labelId van het gezicht geeft ontdekt

####<a name="output-example"></a>Voorbeeld van de uitvoer:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… afgekapt


###<a name="redact-mode"></a>Modus Redigeren

De tweede stap van de werkstroom wordt een groter aantal ingangen dat moet worden gecombineerd tot één enkel actief.

Dit omvat een lijst van id's om te vervagen, de oorspronkelijke video en de aantekeningen JSON. Deze modus wordt de aantekeningen toe te passen op de video input vervagen.

De uitvoer van de pass analyseren omvat niet de oorspronkelijke video. De video moet worden geüpload naar het activum input voor de taak Redact modus en als het primaire bestand geselecteerd.

Werkgebied|Bestandsnaam|Notities
---|---|---
Invoer van activa|foo.bar|Video in WMV, MPV of MP4-indeling. Hetzelfde als in stap 1 video.
Invoer van activa|foo_annotations.JSON|het metagegevensbestand aantekeningen van fase één, met optionele wijzigingen.
Invoer van activa|foo_IDList.txt (optioneel)|Optionele nieuwe regel gescheiden lijst met face id's te redigeren. Als u geen vervaging dit alle vlakken.
Invoer-config|De configuratie van vooraf ingestelde|{'version':'1.0 ', 'opties': {'mode': 'Redigeren'}}
Uitvoer activa|foo_redacted.mp4|Video met vervaging toegepast op basis van aantekeningen

####<a name="example-output"></a>Voorbeeld van de uitvoer

Dit is de uitvoer van een IDList met een ID die geselecteerd is.

[Deze video bekijken](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Beschrijvingen van kenmerken

De redactie MP biedt hoge precisie gezicht locatie detectie en bijhouden die maximaal 64 menselijke gezichten in een video frame kan detecteren. Frontale vlakken geven de beste resultaten, terwijl de zijkanten en kleine vlakken (kleiner dan of gelijk aan 24 x 24 pixels) uitdaging.

De vlakken gevonden en bijgehouden worden geretourneerd met coördinaten die de locatie van de vlakken, alsmede een gezicht id-nummer waarmee wordt aangegeven dat afzonderlijke volgen. Face id-nummers zijn gevoelig voor opnieuw instellen onder omstandigheden wanneer het frontale vlak verloren is gegaan of overlapt in het frame, waardoor sommige mensen meerdere id's krijgen toegewezen.

Zie voor een uitgebreide beschrijving van de kenmerken [Face detecteren en emoties met Azure Media Analytics](media-services-face-and-emotion-detection.md) onderwerp.

## <a name="sample-code"></a>Voorbeeld van code

Het volgende programma ziet u hoe:

1. Maak een actief en een media-bestand uploaden naar het activum.
1. Een taak maken met een nominale redactie taak op basis van een configuratiebestand met de volgende json-voorinstelling. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Download de JSON-uitvoerbestanden. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
