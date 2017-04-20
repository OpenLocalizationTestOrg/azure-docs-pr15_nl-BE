<properties
    pageTitle="Detecteren van bewegingen met Azure Media-Analytics | Microsoft Azure"
    description="De Media-Bewegingsdetector Azure Mediaprocessor (MP) kunt u gedeelten van een belang in een video anders lange en probleemloze efficiënt te identificeren."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Bewegingen met Analytics Azure Media detecteren

##<a name="overview"></a>Overzicht

De **Media-Bewegingsdetector Azure** Mediaprocessor (MP) kunt u gedeelten van een belang in een video anders lange en probleemloze efficiënt te identificeren. Beweging detectie kan worden gebruikt op statische camera beelden voor gedeelten van de video waar beweging plaatsvindt. Genereert een JSON-bestand met een metagegevens met tijdstempels en de omringende regio waar de gebeurtenis plaatsvond.

Deze technologie is gericht op beveiliging video-feeds, kunnen categoriseren beweging in relevante gebeurtenissen en vals-positief zoals belichting en schaduwen. Hiermee kunt u waarschuwingen genereren uit camera feeds zonder spam met oneindig veel niet-relevante gebeurtenissen, terwijl momenten van belang van toezicht op zeer lange video's uitpakken.

**Azure Media Bewegingsdetector** MP is momenteel in de voorvertoning.

Dit onderwerp bevat details over **Azure Media Bewegingsdetector** en ziet u hoe u met Media Services SDK voor .NET gebruiken


##<a name="motion-detector-input-files"></a>Motion Detector invoerbestanden

FLV-bestanden. Op dit moment de volgende indelingen worden ondersteund: MP4, MOV en WMV.

##<a name="task-configuration-preset"></a>Taken configureren (definitie)

Wanneer u een taak maakt met **Azure Media Motion Detector**, moet u een voorinstelling voor configuratie. 

###<a name="parameters"></a>Parameters

U kunt de volgende parameters gebruiken:

Naam|Opties|Beschrijving|Standaard
---|---|---|---
sensitivityLevel|Tekenreeks: 'laag', 'Gemiddeld', 'hoog'|Hiermee stelt de gevoeligheid niveau op welke bewegingen wordt gerapporteerd. Dit om aan te passen bedrag valse meldingen aanpassen.|'normaal'
frameSamplingValue|Positief geheel getal|Stelt de frequentie aan welk algoritme wordt uitgevoerd. 1 gelijk is aan elk frame, 2 betekent dat elke 2e frame, enzovoort.|1
detectLightChange|Boolean: 'true', 'false'|Hiermee wordt ingesteld of lichte wijzigingen worden gemeld in de resultaten|'False'
mergeTimeThreshold|Xs-tijd:: Mm: ss<br/>Bijvoorbeeld: 00:00:03|Hiermee geeft u het tijdvenster tussen beweging gebeurtenissen waarbij 2 gebeurtenissen worden gecombineerd en gerapporteerd als 1.|00:00:00
detectionZones|Een matrix van detectie van zones:<br/>-Zone detectie is een matrix van 3 of meer punten<br/>-Punt is een x en een y coördinaat van 0 naar 1.|Beschrijving van de lijst met zones veelhoekige detectie moet worden gebruikt.<br/>Resultaten met de zones worden vermeld als een ID, met de eerste afbeelding wordt 'id': 0|Één zone die het gehele frame dekt.

###<a name="json-example"></a>JSON-voorbeeld

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Motion Detector uitvoerbestanden

Een beweging detectie taak retourneren een JSON-bestand in de beschrijving van de beweging waarschuwingen en hun categorieën binnen de video output activa. Het bestand bevat informatie over het tijdstip en de duur van de beweging in de video wordt gedetecteerd.

De Motion Detector-API biedt indicatoren zodra er objecten in beweging in een vaste achtergrond video (bv. een surveillance video zijn). De Motion Detector is opgeleid false alarmsignalen, zoals verlichting en schaduw verminderen. Huidige beperkingen van de algoritmen zijn 's nachts vision video's, semi-transparante objecten en kleine objecten.

###<a id="output_elements"></a>Elementen van het uitvoerbestand JSON

>[AZURE.NOTE]In de nieuwste release, de uitvoer JSON-indeling is gewijzigd en een recente wijziging voor sommige klanten kan voorstellen.

De volgende tabel beschrijft de elementen van het uitvoerbestand JSON.

Element|Beschrijving
---|---
Versie|Dit is de versie van de Video-API. De huidige versie is 2.
Tijdschaal|"Tikken" per seconde van de video.
Offset|De time-offset voor tijdstempels in "tikken". Versie 1.0 van de Video-API's, zal dit altijd 0 zijn. In toekomstige scenario's die we ondersteunen deze waarde wijzigen.
Framesnelheid|Frames per seconde van de video.
Breedte, hoogte|Verwijst naar de breedte en hoogte van de video in pixels.
Start|De tijdstempel van start in "tikken".
Duur|De lengte van de gebeurtenis, in "tikken".
Interval|Het interval van elke vermelding in de gebeurtenis, in "tikken".
Gebeurtenissen|Elke gebeurtenis fragment bevat de beweging gedetecteerd binnen die tijdsduur.
Type|In de huidige versie is dit altijd '2' voor algemene beweging. Dit label geeft Video API's de flexibiliteit voor het categoriseren van beweging in toekomstige versies.
RegionID|Zoals hierboven vermeld, is dit altijd 0 in deze versie. Dit label geeft Video API de flexibiliteit om te zoeken naar beweging in verschillende gebieden in toekomstige versies.
Regio 's|Verwijst naar het gebied in de video waar u beweging hechten. <br/><br/>-'id' staat voor het gebied regio – in deze versie is slechts een ID 0. <br/>-"type" staat voor de vorm van de regio die u belangrijk vindt voor beweging. Op dit moment worden "rechthoek" en "polygon" ondersteund.<br/> Als u "rechthoek" hebt opgegeven, heeft de regio afmetingen in X, Y, Width en Height. De X- en Y-coördinaten vertegenwoordigen de bovenste linkerhand XY-coördinaten van het gebied in een gestandaardiseerde schaal van 0,0 en 1,0. Geven de grootte van het gebied in een gestandaardiseerde schaal van 0,0 en 1,0 de breedte en hoogte. In de huidige versie, zijn X, Y, Width en Height altijd vast op 0, 0 en 1, 1. <br/>Als u "polygon" hebt opgegeven, heeft de regio dimensies in punten. <br/>
Fragmenten|De metagegevens is in verschillende segmenten genoemd fragmenten van gesegmenteerde. Elk fragment bevat een begindatum, duur, Intervalnummer en gebeurtenis(sen). Een fragment met geen gebeurtenissen betekent dat er geen beweging is gedetecteerd tijdens de begintijd en de duur.
Rechte haken]|De beugels vertegenwoordigt één interval in de gebeurtenis. Lege haakjes voor dat interval betekent dat er geen beweging is gedetecteerd.
locaties|Deze nieuwe vermelding onder gebeurtenissen geeft een overzicht van de locatie waar de beweging heeft plaatsgevonden. Dit is specifieker dan het detectie-zones.

Hier volgt een voorbeeld van de uitvoer JSON

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Beperkingen

- De ondersteunde input video-indelingen zijn MP4, MOV en WMV.
- Beweging detectie is geoptimaliseerd voor stationaire achtergrond video's. Het algoritme is gericht op het verminderen van false alarmsignalen, zoals wijzigingen in belichting en schaduwen.
- Sommige bewegingen niet herkend vanwege technische problemen; bijvoorbeeld 's nachts vision video's, semi-transparante objecten en kleine objecten.


## <a name="sample-code"></a>Voorbeeld van code

Het volgende programma ziet u hoe:

1. Maak een actief en een media-bestand uploaden naar het activum.
1. Een taak maakt met een video beweging detectie taak op basis van een configuratiebestand met de volgende json-voorinstelling. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Uitvoer JSON bestanden worden gedownload. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Verwante koppelingen
[Azure Media Services Bewegingsdetector blog](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
