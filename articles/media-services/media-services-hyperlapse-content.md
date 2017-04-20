<properties
    pageTitle="Mediabestanden met Azure Media Hyperlapse Hyperlapse | Microsoft Azure"
    description="Azure Media Hyperlapse maakt goede verstreken tijd video's uit de eerste persoon of actie-camera-inhoud. In dit onderwerp wordt beschreven hoe Media indexering gebruiken."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Mediabestanden met Azure Media Hyperlapse Hyperlapse

Azure Media Hyperlapse is een Media Processor (MP) die vloeiend verstreken tijd video's uit de eerste persoon of actie-camera-inhoud worden gemaakt.  Broer wolk is gebaseerd op [Microsoft-Research desktop Hyperlapse Pro en Hyperlapse mobiele telefoon](http://aka.ms/hyperlapse), Microsoft Hyperlapse voor Azure Media Services maakt gebruik van de enorme omvang van het verwerken van Azure Media Services Media platform horizontaal schalen en parallelize bulksgewijs Hyperlapse verwerken.

>[AZURE.IMPORTANT]Microsoft Hyperlapse is ontworpen om te werken het beste op de inhoud van de eerste persoon met een bewegende camera.  Hoewel stilstaande camera beelden nog wel werken kan, kunnen niet de prestaties en de kwaliteit van de Processor Azure Media Hyperlapse Media worden gegarandeerd voor andere soorten inhoud.  Voor meer informatie over Microsoft Hyperlapse Azure Media Services en bekijk een voorbeeld video's, bekijk de [inleidende blogberichten](http://aka.ms/azurehyperlapseblog) van de public preview.

Een Azure Media Hyperlapse taak als krijgt het invoerbestand een MP4 of MOV WMV activa en een configuratiebestand dat bepaalt u welke frames van de video verstreken tijd moeten worden en met welke snelheid (bv. de eerste 10.000 frames 2 x).  De uitvoer is een gestabiliseerde en verstreken tijd vertoning van de video input.

Zie voor de meest recente updates voor Azure Media Hyperlapse, [blogs Media Services](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse activa

Eerst moet u uw gewenste invoerbestand uploaden naar Azure Media Services.  Lees voor meer informatie over de concepten die betrokken zijn bij het uploaden en beheren van inhoud, het [artikel inhoud beheren](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Definitie van de configuratie voor Hyperlapse

Als uw inhoud uw Media Services-account is, moet u uw configuratie voorinstelling maken.  De volgende tabel beschrijft de door de gebruiker opgegeven velden:

 Veld | Beschrijving
-------|-------------
StartFrame|Het frame waarop de Microsoft Hyperlapse verwerking moet beginnen.
NumFrames|Het aantal frames worden verwerkt
Snelheid|De factor waarmee de video input versnellen.

Hier volgt een voorbeeld van een conform de configuratiebestand in XML en JSON:

**XML-definitie:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON voorinstelling:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse met de AMS .NET SDK

De volgende methode uploadt een media-bestand als een actief en een taak maakt met de Processor Azure Media Hyperlapse Media.

> [AZURE.NOTE] In het bereik met de naam 'context' voor deze code werkt, moet u al een CloudMediaContext hebben.  Voor meer informatie hierover, lees het [artikel inhoud beheren](media-services-dotnet-get-started.md).

> [AZURE.NOTE] Het argument string 'hyperConfig' is waarschijnlijk een conform de configuratie vooraf in JSON of XML, zoals hierboven beschreven.

statische bool RunHyperlapseJob (string input tekenreeksuitvoer, string hyperConfig) {/ / Maak activa met invoerbestand IAsset activa = context. Activa. CreateAssetAndUploadSingleFile (invoer, 'Mijn Hyperlapse Input', AssetCreationOptions.None);

exemplaren van Azure Media Hyperlapse MP IMediaProcessor mp pak = context. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Taak maken met de taak IJob Hyperlapse = context. Taken. Maken (String.Format (input "Hyperlapse {0}"));

Als (String.IsNullOrEmpty(hyperConfig)) {/ / config kan niet leeg return false;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = project. Tasks.AddNew ('Hyperlapse task', mp, hyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse uitvoer", AssetCreationOptions.None);


taak. Submit();

Voortgang afdrukken en bij het controleren van taken taak progressPrintTask = nieuwe Task(() = > {

IJob jobQuery = null; Voer {var progressContext = context; jobQuery = progressContext.Jobs. Waar (j = > j.Id == taak. ID). First(); Console.WriteLine (string. Format ("\t {1} \t {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Voortgang)); Thread.Sleep(10000); } terwijl (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Ondersteunde bestandstypen

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
