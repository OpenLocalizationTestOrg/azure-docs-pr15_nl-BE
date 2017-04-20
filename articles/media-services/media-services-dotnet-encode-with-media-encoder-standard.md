<properties 
    pageTitle="Een actief coderen met Media Encoder Standard met .NET | Microsoft Azure" 
    description="In dit onderwerp ziet u hoe u .NET gebruiken om Media Encoder Strandard wordt gebruikt." 
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
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Een actief met Media Encoder Standard met .NET coderen

Codering taken zijn een van de meest voorkomende bewerkingen in Media Services. U maakt codering taken om mediabestanden te converteren van één codering naar de andere. Wanneer u codeert, kunt u de ingebouwde Media Encoder van Media Services. U kunt ook een coderingsprogramma geleverd door een partner van Media Services; derden encoders zijn beschikbaar via de markt Azure. 

In dit onderwerp ziet u hoe u .NET gebruiken om uw activa met Media Encoder Standard (MES). Media Encoder standaard is geconfigureerd met een van de encoder voorinstellingen beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Het is raadzaam altijd de mezzanine-bestanden te coderen in een adaptieve bitsnelheid MP4 instellen en vervolgens de set converteren naar de gewenste indeling met behulp van de [Dynamische verpakking](media-services-dynamic-packaging-overview.md). Als u wilt profiteren van dynamische verpakking, moet eerst krijgt u ten minste één On-demand streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt. Zie [schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Als uw uitvoer activum opslag gecodeerd, moet u beleid voor levering van activa. Zie [configureren activa levering beleid](media-services-dotnet-configure-asset-delivery-policy.md)voor meer informatie.

>[AZURE.NOTE]MES produceert een uitvoerbestand met een naam die de eerste 32 tekens van de naam van het invoerbestand bevat. De naam is gebaseerd op wat is opgegeven in het bestand. Bijvoorbeeld: "bestandsnaam": "_ {Basename} {Index} {extensie}". {Basename} wordt vervangen door de eerste 32 tekens van de naam van het invoerbestand.

###<a name="mes-formats"></a>MES-indelingen

[Indelingen en -codecs](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>MES voorinstellingen

Media Encoder standaard is geconfigureerd met een van de encoder voorinstellingen beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Invoer en uitvoer, metagegevens

Wanneer u een input-element (of activa) MES codeert, krijgt u een uitvoer actief op de succesvolle afronding van die taak coderen. De activa van de uitvoer bevat video, audio, miniaturen, manifest, enz., op basis van de codering voorinstelling die u wilt gebruiken.

De activa van de uitvoer bevat ook een bestand met metagegevens over het activum invoer. De naam van het XML-bestand met metagegevens heeft de volgende indeling: < asset_id > _metadata.xml (bijvoorbeeld 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), waarbij < asset_id > de item-id hebt waarde van de invoer actief is. Het schema van deze invoer XML-metagegevens wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

De activa van de uitvoer bevat ook een bestand met metagegevens over het activum uitvoer. De naam van het XML-bestand met metagegevens heeft de volgende indeling: < source_file_name > _manifest.xml (bijvoorbeeld BigBuckBunny_manifest.xml). Het schema van deze uitvoer metadata XML wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Als u controleren op een van de twee bestanden met metagegevens wilt, kunt u een SAS-locator maken en download het bestand naar uw lokale computer. U vindt een voorbeeld voor het maken van een SAS-locator en downloaden van een bestand met de serverextensies Media Services .NET SDK.

##<a name="download-sample"></a>Voorbeeld downloaden

Ophalen en uitvoeren van een steekproef van [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt .NET SDK van Media Services gebruikt de volgende taken uitvoeren:

- Maak een taak codering.
- Een verwijzing naar het coderingsprogramma Media Encoder standaard ophalen.
- Geef voor het gebruik van de "H264 meerdere bitsnelheid 720p" vooraf ingestelde. U kunt zien dat alle voorinstellingen [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Bekijk het schema waarnaar deze voorinstellingen moeten voldoen [hier](https://msdn.microsoft.com/library/mt269962.aspx) onderwerp.
- Codering één taak aan de taak toevoegen. 
- Geef het invoer actief moeten worden gecodeerd.
- Een uitvoer actief met het gecodeerde actief maken.
- Als u wilt controleren van de voortgang van het project een gebeurtenis-handler toevoegen.
- Indienen van de taak.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
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


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook 

[Het genereren van miniaturen met Media Encoder standaard met .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services codering, overzicht](media-services-encode-asset.md)
