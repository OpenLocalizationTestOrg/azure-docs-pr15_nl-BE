<properties
    pageTitle="Detecteren gezicht en emoties met Azure Media-Analytics | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe gezichten en emoties met Azure Media Analytics detecteren."
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
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Gezicht en emoties met Analytics Azure Media detecteren

##<a name="overview"></a>Overzicht

De processor in **Azure Media gezicht Detector** media (MP) kunt u tellen, verplaatsingen bijhouden en zelfs meter publiek deelname en reactie via gezichtsuitdrukkingen. Deze service bevat twee functies: 

- **Face detection**

    Face detection zoekt en menselijke gezichten in een video wordt bijgehouden. Meerdere vlakken kunnen worden gedetecteerd en vervolgens worden bijgehouden als ze onderweg, met de tijd en locatie metagegevens die worden geretourneerd in een JSON-bestand. Tijdens het bijhouden, wordt geprobeerd om een consistente ID geven op het vlak van dezelfde, terwijl de persoon is verplaatsen op het scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het frame laat.

    >[AZURE.NOTE]Deze services geen gelaatsherkenning uitgevoerd. Een persoon die het frame blijft of wordt ondervindt hinder van obstakels voor te lang krijgt een nieuwe ID als resultaat.

- **Detectie van emoties**
    
    Detectie van emoties is een optioneel onderdeel van de Face Detection Media Processor die analyse op meerdere emotionele kenmerken van de vlakken gedetecteerd wordt, met inbegrip van vriendschap, sadness, angst, boosheid en meer. 

**Azure Media gezicht Detector** MP is momenteel in de voorvertoning.

Dit onderwerp bevat details over **Azure Media gezicht Detector** en ziet u hoe u met Media Services SDK voor .NET gebruiken.

##<a name="face-detector-input-files"></a>Detector invoerbestanden wordt geconfronteerd.

FLV-bestanden. Op dit moment de volgende indelingen worden ondersteund: MP4, MOV en WMV.

##<a name="face-detector-output-files"></a>De uitvoerbestanden Detector wordt geconfronteerd.

Het gezicht en het bijhouden API biedt hoge precisie gezicht locatie detectie en bijhouden die maximaal 64 menselijke gezichten in een video kan detecteren. Frontale vlakken bieden de beste resultaten, terwijl de zijkanten en kleine vlakken (kleiner dan of gelijk aan 24 x 24 pixels) mogelijk niet zo nauwkeurig.

De vlakken gevonden en bijgehouden met coördinaten (left, top, width en height) geretourneerd die de locatie van de vlakken in de afbeelding in pixels, als een face id-nummer waarmee wordt aangegeven dat afzonderlijke volgen. Face id-nummers zijn gevoelig voor opnieuw instellen onder omstandigheden wanneer het frontale vlak verloren is gegaan of overlapt in het frame, waardoor sommige mensen meerdere id's krijgen toegewezen.

###<a id="output_elements"></a>Elementen van het uitvoerbestand JSON

Het resultaat van de uitvoer bevat voor de opsporing van het gezicht en het bijhouden van de bewerking, de metagegevens van de vlakken in het gegeven bestand in JSON-indeling.

De opsporing van het gezicht en het bijhouden van JSON bevat de volgende kenmerken:

Element|Beschrijving
---|---
Versie|Dit is de versie van de Video-API.
Tijdschaal|"Tikken" per seconde van de video.
Offset|Dit is het tijdverschil voor tijdstempels. Versie 1.0 van de Video-API's, zal dit altijd 0 zijn. In toekomstige scenario's die we ondersteunen deze waarde wijzigen.
Framesnelheid|Frames per seconde van de video.
Fragmenten|De metagegevens is in verschillende segmenten genoemd fragmenten van gesegmenteerde. Elk fragment bevat een begindatum, duur, Intervalnummer en gebeurtenis(sen).
Start|De begintijd van de eerste gebeurtenis in 'tikken'.
Duur|De lengte van het fragment, in "tikken".
Interval|Het interval van elke vermelding in het fragment, in "tikken".
Gebeurtenissen|Elke gebeurtenis bevat de vlakken gedetecteerd en wordt bijgehouden in die tijdsduur aangeeft. Het is een matrix van een matrix van gebeurtenissen. De buitenste matrix vertegenwoordigt een tijdsinterval. De inner matrix bestaat uit 0 of meer gebeurtenissen die op dat moment gebeurd. Een lege haak [] betekent dat geen vlakken zijn gedetecteerd.
ID| De ID van het oppervlak dat wordt bijgehouden. Dit aantal kan per ongeluk wijzigen als een gezicht niet gevonden wordt. Een bepaalde persoon dezelfde ID gedurende de gehele video moet hebben, maar dit kan niet worden gegarandeerd vanwege beperkingen in de detectie-algoritme (Occlusie, enz.)
X, Y|De linkerbovenhoek X en Y-coördinaten van het gezicht omsluitend kader in een gestandaardiseerde schaal van 0,0 en 1,0. <br/>-X en Y coördinaten zijn ten opzichte van altijd, liggend hebt u een staand video (of ondersteboven, in het geval van iOS), hebt u de coördinaten dienovereenkomstig transponeren.
Breedte, hoogte|De breedte en hoogte van het omsluitende kader in een gestandaardiseerde schaal van 0,0 en 1,0 vlak.
facesDetected|Dit wordt aan het einde van de JSON-resultaten gevonden en geeft een overzicht van het aantal vlakken die het algoritme tijdens de video ontdekt. Omdat de id's per ongeluk kunnen worden hersteld als een gezicht niet gevonden wordt (bijvoorbeeld vlak scherm ziet er weg), gaat dit nummer mogelijk niet altijd gelijk is aan het eigenlijke aantal vlakken in de video.

Nominale Detector maakt gebruik van technieken van fragmentatie (waar de metagegevens kan worden opgesplitst in segmenten op basis van tijd en kunt u alleen wat u nodig hebt) en segmentatie (waar de gebeurtenissen worden opgedeeld in het geval ze te groot krijgen). Sommige eenvoudige berekeningen kunt u de gegevens transformeren. Bijvoorbeeld, als een gebeurtenis die is gestart op 6300 (maten), met een tijdschaal van 2997 (maten per seconde) en de framesnelheid van 29,97 (frames per seconde), vervolgens:

- Start/tijdschaal = 2.1 seconden
- Seconden x (framesnelheid/tijdschaal) = 63 frames

Hieronder vindt u een eenvoudig voorbeeld van het extraheren van de JSON in een per frame-indeling voor gezicht detectie en bijhouden:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Geconfronteerd detectie input en output voorbeeld

###<a name="input-video"></a>Video input

[Video Input](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Taken configureren (definitie)

Wanneer u een taak maakt met **Azure Media gezicht Detector**, moet u een voorinstelling voor configuratie. De volgende definitie van de configuratie is alleen voor detectie van het gezicht.

    {"version":"1.0"}

###<a name="json-output"></a>JSON-uitvoer

In het volgende voorbeeld van JSON uitvoer is afgekapt.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Detectie van emoties invoer en uitvoer voorbeeld


###<a name="input-video"></a>Video input

[Video Input](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Taken configureren (definitie)

Wanneer u een taak maakt met **Azure Media gezicht Detector**, moet u een voorinstelling voor configuratie. JSON is gebaseerd op de detectie van emoties maken geeft de volgende definitie van de configuratie.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Beschrijvingen van kenmerken

Naam van kenmerk|Beschrijving
---|---
Modus|Vlakken: Geconfronteerd alleen detectie <br/>AggregateEmotion: Retour emoties van gemiddelde waarden voor alle vlakken in frame.
AggregateEmotionWindowMs|Gebruik als AggregateEmotion modus is geselecteerd. Hiermee geeft u de lengte van de video gebruikt voor de productie van elk resultaat statistische in milliseconden.
AggregateEmotionIntervalMs|Gebruik als AggregateEmotion modus is geselecteerd. Geeft aan met welke frequentie aan statistische resultaten opleveren.

####<a name="aggregate-defaults"></a>Standaardinstellingen voor statistische

Hieronder worden aanbevolen waarden voor de statistische venster en interval. AggregateEmotionWindowMs mag niet langer zijn dan AggregateEmotionIntervalMs.

   |Standaard (s)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON-uitvoer

JSON uitvoer voor statistische emoties (afgekapt):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Beperkingen

- De ondersteunde input video-indelingen zijn MP4, MOV en WMV.
- Het bereik van de grootte van detecteerbare gezicht is 24 x 24 tot 2048 x 2048 pixels. De vlakken uit dit bereik worden niet gedetecteerd.
- Voor elke video is het maximum aantal geretourneerde vlakken 64.
- Sommige vlakken worden niet gedetecteerd door technische problemen; bv. grote face hoeken (hoofd-pose) en grote Occlusie. Frontale en in de buurt van frontale vlakken hebben de beste resultaten.


## <a name="sample-code"></a>Voorbeeld van code

Het volgende programma ziet u hoe:

1. Maak een actief en een media-bestand uploaden naar het activum.
1. Een taak maakt met een gezicht detectie taak op basis van een configuratiebestand met de volgende json-voorinstelling. 
                    
        {
            "version": "1.0"
        }

1. Uitvoer JSON bestanden worden gedownload. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

[Azure Media Services Analytics overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)