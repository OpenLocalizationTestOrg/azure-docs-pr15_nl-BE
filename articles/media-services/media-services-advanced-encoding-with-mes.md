<properties 
    pageTitle="Geavanceerde codering met Media Encoder Standard" 
    description="In dit onderwerp ziet u hoe u geavanceerde codering door Media Encoder standaard taak voorinstellingen aanpassen. Het onderwerp ziet u hoe u .NET SDK van Media Services gebruiken voor het maken van een taak en taak een codering. Ook ziet u hoe u aangepaste voorinstellingen voor de codering taak opgeven." 
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
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Geavanceerde codering met Media Encoder Standard

##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u geavanceerde codering uitvoert met Media Encoder Standard. Het onderwerp ziet u [het gebruik van .NET om te maken en een codering taak een taak die deze taak wordt uitgevoerd](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Ook ziet u hoe u aangepaste voorinstellingen voor de codering taak opgeven. Beschrijving van de elementen die worden gebruikt door de voorinstellingen, Zie [Dit document](https://msdn.microsoft.com/library/mt269962.aspx). 

De aangepaste voorinstellingen die de volgende codering uitvoeren worden getoond:

- [Genereren van miniaturen](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Trim een video (knippen)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Een overlay maken](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Een stille audio track invoegen als input geen audio heeft](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Automatische de-interlacing uitschakelen](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Voorinstellingen voor alleen audio](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [Twee of meer videobestanden samenvoegen](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Video's bijsnijden met Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Plaats geen video input heeft een videotrack](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Een video draaien](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Codering met mediaservices SDK voor .NET

In het volgende voorbeeld wordt .NET SDK van Media Services gebruikt de volgende taken uitvoeren:

- Maak een taak codering.
- Een verwijzing naar het coderingsprogramma Media Encoder standaard ophalen.
- De aangepaste XML of JSON voorinstelling laden. U kunt de XML-gegevens opslaan of JSON (bijvoorbeeld [XML](media-services-custom-mes-presets-with-dotnet.md#xml) of [JSON](media-services-custom-mes-presets-with-dotnet.md#json) in een bestand en gebruik de volgende code om het bestand te laden.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Een codering taak aan de taak toevoegen. 
- Geef het invoer actief moeten worden gecodeerd.
- Een uitvoer actief met het gecodeerde actief maken.
- Als u wilt controleren van de voortgang van het project een gebeurtenis-handler toevoegen.
- Indienen van de taak.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Ondersteuning voor de relatieve grootte

Bij het genereren van miniatuurweergaven van deze hoeft u niet altijd uitvoer breedte en hoogte in pixels opgeven. U kunt opgeven in percentages in het bereik [% 1,..., 100%].

###<a name="json-preset"></a>JSON-voorinstelling 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>XML-definitie
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Genereren van miniaturen

In dit gedeelte ziet u hoe een voorinstelling die miniaturen genereert aanpassen. De hieronder omschreven definitie bevat informatie over hoe u coderen van uw bestand, alsmede de informatie die nodig is wilt voor het genereren van miniaturen. U kunt een van de MES voorinstellingen gedocumenteerde [hier](https://msdn.microsoft.com/library/mt269960.aspx) en code toevoegen die miniaturen genereert.  

>[AZURE.NOTE]De instelling **SceneChangeDetection** in de volgende vooraf ingestelde kan uitsluitend worden ingesteld op true als u naar een enkele video bitsnelheid wilt coderen. Als u naar een multi-bitrate video-codering en **SceneChangeDetection** ingesteld op true, wordt een fout geretourneerd door het coderingsprogramma.  


Raadpleeg [Dit](https://msdn.microsoft.com/library/mt269962.aspx) onderwerp voor meer informatie over het schema.

Zorg ervoor dat de sectie [overwegingen](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>JSON-voorinstelling


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>XML-definitie


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Overwegingen met betrekking tot

De volgende overwegingen zijn van toepassing:

- Het gebruik van expliciete tijdstempels voor stap/begin van het bereik wordt ervan uitgegaan dat de invoerbron ten minste 1 minuut.
- Png-jpg/BmpImage elementen moet worden gestart, stap, en kenmerken voor verbindingsreeksen bereik – deze kunnen worden geïnterpreteerd als:

    - Frame nummer als ze niet-negatieve gehele getallen zijn, zoals 'Start': "120"
    - In verhouding tot de duur voor de gegevensbron als uitgedrukt als % volgen, zoals 'Start': "15%", of
    - Timestamp, uitgedrukt in: mm: ss... indeling, zoals 'Start': ' 00: 01:00 "

    U kunt mengen en opmerkingen als u moet overeenkomen.
    
    Start ondersteunt daarnaast ook een speciale Macro: {beste}, die probeert vast te stellen van de eerste "interessant" frame van de NOTITIE-inhoud: (stap en bereik worden genegeerd wanneer Start {beste})
    
    - Standaardinstellingen: Start: {beste}
- Uitvoerindeling moet expliciet worden opgegeven voor elke indeling voor afbeeldingen: Jpg/Png/BmpFormat. Indien aanwezig, overeenkomt MES JpgVideo tot JpgFormat enzovoort. OutputFormat introduceert een nieuwe image-codec-specifieke Macro: {Index}, welke moet worden aanwezig (eenmalig en eenmaal) voor de uitvoerindelingen afbeelding.

##<a id="trim_video"></a>Trim een video (knippen)

In deze sectie gesprekken voert over het wijzigen van de encoder-voorinstellingen om te knippen of snijd de input video waarin de invoer een zogenaamde mezzanine of bestand op aanvraag is. Het coderingsprogramma kan ook worden gebruikt om te knippen of snijden van een actief, dat is vastgelegd of gearchiveerd uit een live gegevensstroom – de details voor deze beschikbaar zijn in [deze blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Om uw video's bijsnijden, kunt u een van de MES voorinstellingen gedocumenteerde [hier](https://msdn.microsoft.com/library/mt269960.aspx) en het element van de **bronnen** wijzigen (zoals hieronder). De waarde van de starttijd moet overeenkomen met de absolute tijdstempels van de video input. Bijvoorbeeld, als het eerste frame van de video input een tijdstempel van de 12:00:10.000 heeft, vervolgens StartTime moet ten minste 12:00:10.000 en hoger. In het volgende voorbeeld nemen we aan dat de invoer video een tijdstempel begin van nul heeft. **Bronnen** worden aan het begin van de voorinstelling geplaatst. 
 
###<a id="json"></a>JSON-voorinstelling
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>XML-definitie
    
Om uw video's bijsnijden, kunt u een van de MES voorinstellingen gedocumenteerde [hier](https://msdn.microsoft.com/library/mt269960.aspx) en het element van de **bronnen** wijzigen (zoals hieronder).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Een overlay maken

De Media Encoder Standard kunt u een afbeelding op een bestaande video overlay. Op dit moment de volgende indelingen worden ondersteund: png, jpg, gif en bmp. De hieronder omschreven definitie is een eenvoudig voorbeeld van een video-overlay.

Naast een definitie van een vooraf ingestelde bestand, hebt u ook Media Services weet welk bestand het actief in de afbeelding overlay is en welk bestand is de bronvideo waarop u de afbeelding overlay wilt laten. Het videobestand is het **primaire** bestand. 

Het bovenstaande voorbeeld van .NET definieert twee functies: **UploadMediaFilesFromFolder** en **EncodeWithOverlay**. De functie UploadMediaFilesFromFolder bestanden uit een map (bijvoorbeeld, BigBuckBunny.mp4 en Image001.png) en het mp4-bestand naar het primaire bestand in het activum worden ingesteld. De **EncodeWithOverlay** wordt het aangepaste voorinstellingenbestand die werd doorgegeven (bijvoorbeeld de voorinstelling die volgt) gebruikt om de codering taak te maken. 

>[AZURE.NOTE]Huidige beperkingen:
>
>De instelling van de dekking overlay wordt niet ondersteund.
>
>Uw FLV-bronbestand en het afbeeldingsbestand overlay in hetzelfde actief en wordt het videobestand moet worden ingesteld als het primaire bestand in deze activa.

###<a name="json-preset"></a>JSON-voorinstelling
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>XML-definitie
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Een stille audio track invoegen als input geen audio heeft

Als u een met het coderingsprogramma met alleen video en geen audio-invoer verzendt vervolgens bevat de activa van de uitvoer standaard bestanden die alleen videogegevens bevatten. Sommige spelers mogelijk geen dergelijke uitvoerstromen verwerken. Deze instelling kunt u ervoor zorgen dat het coderingsprogramma een stille audio track toevoegen aan de uitvoer in dat scenario.

Als u de codering voor de productie van een actief met een stille audio track input heeft geen audio, geeft u de waarde 'InsertSilenceIfNoAudio'.

U kunt van het MES voorinstellingen gedocumenteerde [hier](https://msdn.microsoft.com/library/mt269960.aspx), en breng de volgende wijziging:

###<a name="json-preset"></a>JSON-voorinstelling

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>XML-definitie

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Automatische de-interlacing uitschakelen

Klanten hoeven niet te doen als ze worden automatisch opgeheven geïnterlinieerde inhoud interlace. Wanneer de automatische de-interlacing ingeschakeld (standaard is) wordt het MES wordt de automatische detectie van geïnterlinieerde frames en gemarkeerd als geïnterlinieerde frames alleen uit interlaces.

Kunt u de automatische de-interlacing uitschakelen. Deze optie wordt niet aanbevolen.

###<a name="json-preset"></a>JSON-voorinstelling
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>XML-definitie
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Voorinstellingen voor alleen audio

In dit gedeelte ziet u twee alleen audio MES voorinstellingen: AAC-Audio- en AAC goede kwaliteit Audio.

###<a name="aac-audio"></a>AAC-Audio 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>Goede kwaliteit AAC-Audio

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>Twee of meer videobestanden samenvoegen

In het volgende voorbeeld ziet u hoe u een voorinstelling voor het samenvoegen van twee of meer videobestanden kunt genereren. Het meest gangbare scenario is wanneer u een koptekst of een aanhangwagen aan de belangrijkste video toevoegen. Het beoogde gebruik is wanneer de videobestanden bewerkt samen delen eigenschappen (videoresolutie, framesnelheid, audiotrack tellen, enz.). U dient ervoor te zorgen niet voor het mengen van video's van verschillende framesnelheden, of met een verschillend aantal audiotracks.

###<a name="requirements-and-considerations"></a>Vereisten en overwegingen

- Video input moeten slechts één audiospoor hebben.
- Input video's moeten allemaal dezelfde beeldfrequentie hebben.
- Als u uw video's uploaden naar afzonderlijke activa en video's instellen als het primaire bestand in elk activum.
- U moet weten van de duur van uw video's.
- De vooraf ingestelde onderstaande voorbeelden wordt ervan uitgegaan dat de input video's beginnen met een tijdstempel van nul. U moet de starttijd waarden wijzigen als de video's eerste tijdstempel, zoals meestal het geval met live archieven.
- De JSON-voorinstelling kunt u expliciete verwijzingen naar de item-id hebt waarden van de activa van de invoer.
- De voorbeeldcode wordt ervan uitgegaan dat de JSON-voorinstelling is opgeslagen in een lokaal bestand, zoals "C:\supportFiles\preset.json". Ook wordt aangenomen dat er twee activa zijn gemaakt door twee video bestanden uploaden en dat de resulterende waarden van de item-id hebt.
- Het codefragment en JSON voorinstelling ziet u een voorbeeld van het samenvoegen van twee video-bestanden. U kunt deze uitbreiden naar meer dan twee video's door:

    1. Taak van de telefoonkaart. InputAssets.Add() meerdere keren toe te voegen meer video's, in volgorde.
    2. Op het element 'Bronnen' in de JSON, zodat het overeenkomt bewerkt door meer vermeldingen toe te voegen in dezelfde volgorde. 


###<a name="net-code"></a>.NET-code

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>JSON-voorinstelling

Uw aangepaste vooraf met de id's van de activa die u wilt samenvoegen, en met de juiste tijd segment voor elke video bijwerken.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Video's bijsnijden met Media Encoder Standard

Zie het onderwerp [bijsnijden video's met Media Encoder Standard](media-services-crop-video.md) .

##<a id="no_video"></a>Plaats geen video input heeft een videotrack

Als u een met het coderingsprogramma met alleen geluid en geen video-invoer verzendt vervolgens bevat de activa van de uitvoer standaard bestanden die alleen audiogegevens bevatten. Sommige spelers, inclusief Azure Media Player (Zie [deze](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) mogelijk niet voor het verwerken van deze stromen. Deze instelling kunt u ervoor zorgen dat het coderingsprogramma een monochrome videotrack toevoegen aan de uitvoer in dat scenario. 

>[AZURE.NOTE]Forceren van het coderingsprogramma invoegen een videotrack uitvoer neemt de grootte van de uitvoer actief, en daarmee de kosten voor de codering taak gemaakt. Voer tests uit om te controleren of deze resulterende toename slechts een bescheiden invloed heeft op uw maandelijkse kosten.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Video invoegen op de laagste bitsnelheid gebruikt

Stel dat u gebruikt een verschillende codering bitsnelheid, zoals vooraf ["H264 meerdere bitsnelheid 720p '](https://msdn.microsoft.com/library/mt269960.aspx) voor het coderen van uw gehele invoer catalogus voor streaming, die een combinatie van videobestanden en bestanden met alleen audio bevat. In dit scenario is wanneer de invoer geen video heeft, kunt u dwingen de encoder een monochrome video track invoegen op alleen de laagste bitsnelheid gebruikt, in plaats van video invoegen op elke uitvoer bitsnelheid gebruikt. Hiertoe moet u de vlag 'InsertBlackIfNoVideoBottomLayerOnly' opgeven.

U kunt van het MES voorinstellingen gedocumenteerde [hier](https://msdn.microsoft.com/library/mt269960.aspx), en breng de volgende wijziging:

#### <a name="json-preset"></a>JSON-voorinstelling

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-definitie

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Video invoegen in alle uitvoer bitsnelheid

Stel dat u gebruikt een verschillende codering bitsnelheid, zoals vooraf ["H264 meerdere bitsnelheid 720p](https://msdn.microsoft.com/library/mt269960.aspx) voor het coderen van uw gehele invoer catalogus voor streaming, die een combinatie van videobestanden en bestanden met alleen audio bevat. In dit scenario is wanneer de invoer geen video heeft, kunt u dwingen het coderingsprogramma invoegen een monochrome videotrack helemaal de bitsnelheid van de uitvoer. Dit zorgt ervoor dat de uitvoer activa zijn alle homogene aantal tracks voor video-en audiotracks. Hiertoe moet u de vlag 'InsertBlackIfNoVideo' opgeven.

U kunt van het MES voorinstellingen gedocumenteerde [hier](https://msdn.microsoft.com/library/mt269960.aspx), en breng de volgende wijziging:

#### <a name="json-preset"></a>JSON-voorinstelling

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-definitie
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Een video draaien

De [Media Encoder standaard](media-services-dotnet-encode-with-media-encoder-standard.md) ondersteunt roteren met hoeken van 0/90/180/270. De standaardinstelling is 'Auto', waarbij wordt geprobeerd een rotatie metagegevens in het inkomende video bestand detecteren en het compenseren. De volgende **bronnen** element op een van de voorinstellingen gedefinieerde [hier](http://msdn.microsoft.com/library/azure/mt269960.aspx)omvatten:

### <a name="json-preset"></a>JSON-voorinstelling

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>XML-definitie

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Zie ook [in dit](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) onderwerp voor meer informatie over hoe het coderingsprogramma de breedte en hoogte-instellingen in de voorinstelling, interpreteert als compensatie van de rotatie wordt geactiveerd.

U kunt de waarde '0' om aan te geven met het coderingsprogramma rotatie metagegevens, indien aanwezig, in de video input wordt genegeerd. 


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook 

[Media Services-codering, overzicht](media-services-encode-asset.md)
