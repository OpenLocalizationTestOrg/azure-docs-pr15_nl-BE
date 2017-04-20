<properties
    pageTitle="Azure Media Analytics gebruiken om de tekstinhoud in de FLV-bestanden worden geconverteerd naar digitale tekst | Microsoft Azure"
    description="Azure Media Analytics OCR (optische tekenherkenning) kunt u de tekstinhoud in de FLV-bestanden converteren naar bewerkbare, doorzoekbare digitale tekst.  Hiermee kunt u voor het automatiseren van de winning van zinvolle metagegevens van het videosignaal van uw media."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Azure Media Analytics gebruiken om de tekstinhoud in de FLV-bestanden worden geconverteerd naar digitale tekst 

##<a name="overview"></a>Overzicht

Als u tekst van uw FLV-bestanden uitpakken en het genereren van een bewerkbare, doorzoekbare digitale tekst, moet u Azure Media Analytics OCR (optische tekenherkenning). Deze Processor Azure Media detecteert tekstinhoud in u FLV-bestanden en tekstbestanden voor gebruik genereert. OCR, kunt u automatiseren de winning van zinvolle metagegevens van het videosignaal van uw media.

Wanneer gebruikt in combinatie met een zoekmachine, kunt u gemakkelijk uw media indexering van tekst en de detectie van uw inhoud te verbeteren. Dit is bijzonder nuttig in zeer tekstuele video, net als een video-opname of een schermopname van een diavoorstelling. De Azure OCR Media-Processor is geoptimaliseerd voor digitale tekst.

De processor **Azure Media OCR** media is momenteel in de voorvertoning.

Dit onderwerp bevat details over **Azure Media OCR** en ziet u hoe u met Media Services SDK voor .NET gebruiken. Zie [deze blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/)voor meer informatie en voorbeelden.

##<a name="ocr-input-files"></a>OCR-invoerbestanden

FLV-bestanden. Op dit moment de volgende indelingen worden ondersteund: MP4, MOV en WMV.

##<a name="task-configuration"></a>Taken configureren 

Configuratie van de taak (de standaardoptie). Wanneer u een taak maakt met **Azure Media OCR**, moet u een voorinstelling met JSON- of XML-configuratie. 

###<a name="attribute-descriptions"></a>Beschrijvingen van kenmerken

Naam van kenmerk|Beschrijving
---|---
Taal|(optioneel) beschrijving van de taal van de tekst waarnaar moet worden gezocht. Een van de volgende opties: automatisch detecteren (standaard), Arabisch, ChineseSimplified, ChineseTraditional, Tsjechisch Deens, Nederlands, Engels, Fins, Frans, Duits, Grieks, Hongaars, Italiaans, Japans, Koreaans, Noors, Pools, Portugees, Roemeens, Russisch, SerbianCyrillic, SerbianLatin, Slowaaks, Spaans, Zweeds, Turks.
TextOrientation|(optioneel) beschrijving van de stand van de tekst waarnaar moet worden gezocht.  "Left" betekent dat de bovenkant van hoofdletters worden verwezen naar links.  De standaardtekst (zoals die welke kunnen worden gevonden in een boek) kan worden aangeroepen 'Up' geplaatst.  Een van de volgende opties: automatisch detecteren (standaard), boven, rechts, omlaag, naar links.
TimeInterval|(optioneel) beschrijving van de sampling-frequentie.  De standaardwaarde is 1/2 seconde.<br/>JSON-indeling –: mm: ss. SSS (standaard 00:00:00.500)<br/>XML-indeling W3C XSD-duur primitief (standaard PT0.5)
DetectRegions|(optioneel) Een matrix van DetectRegion objecten opgeven van regio's in het videoframe waar tekst vinden.<br/>Een DetectRegion-object wordt gemaakt van de volgende vier waarden:<br/>Links: pixels vanaf de linkermarge<br/>Boven: pixels vanaf de bovenmarge<br/>Breedte: de breedte van het gebied in pixels<br/>Hoogte: hoogte van het gebied in pixels

####<a name="json-preset-example"></a>Vooraf ingestelde JSON-voorbeeld
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>Vooraf ingestelde XML-voorbeeld

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>OCR-uitvoerbestanden

De uitvoer van de OCR-media-processor is een JSON-bestand.

###<a name="elements-of-the-output-json-file"></a>Elementen van het uitvoerbestand JSON

De OCR voor Video-uitvoer levert gegevens gesegmenteerd van tijd op de tekens in uw video vinden.  U kunt kenmerken zoals of de afdrukstand in woorden dat u geïnteresseerd in het analyseren van bent rovider. 

De uitvoer bevat de volgende kenmerken:

Element|Beschrijving
---|---
Tijdschaal|"tikken" per seconde van de video
Offset|tijd voor tijdstempels worden gecompenseerd. Versie 1.0 van de Video-API's, zal dit altijd 0 zijn.
Framesnelheid|Frames per seconde van de video
Breedte|breedte van de video in pixels
hoogte|hoogte van de video in pixels
Fragmenten|matrix van video waarin de metagegevens wordt gesegmenteerde stukken op basis van tijd
Start|de begintijd van een fragment in "tikken"
duur|de lengte van een fragment in "tikken"
interval|interval van elke gebeurtenis in het opgegeven fragment
gebeurtenissen|matrix met regio 's
regio|woorden of woordgroepen die object gevonden
taal|taal van de tekst in een gebied aangetroffen
afdrukstand|richting van de tekst in een gebied aangetroffen
regels|matrix van regels met tekst in een gebied aangetroffen
tekst|de feitelijke tekst

###<a name="json-output-example"></a>Voorbeeld van de uitvoer JSON

In het volgende voorbeeld van uitvoer bevat de algemene informatie voor video en verschillende video fragmenten. In elke video fragment bevat elk gebied dat met de taal die wordt gedetecteerd door OCR MP en de richting van de tekst. Het gebied bevat ook elke regel word in dit gebied met de tekst van de lijn, de positie van de regel en elke informatie word (word-inhoud, positie en vertrouwen) op deze regel. Hier volgt een voorbeeld en ik enkele opmerkingen in de tekst plaatsen.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Voorbeeld van code

Het volgende programma ziet u hoe:

1. Maak een actief en een media-bestand uploaden naar het activum.
1. Een taak maakt met een bestand met OCR configuratie/voorinstelling.
1. Uitvoer JSON bestanden worden gedownload. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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
