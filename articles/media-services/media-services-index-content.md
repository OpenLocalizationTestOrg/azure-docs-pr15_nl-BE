<properties
    pageTitle="Mediabestanden met Azure Media indexeerfunctie indexeren"
    description="Azure Media indexeerfunctie kunt u de inhoud van uw mediabestanden doorzoekbare en voor het genereren van een transcript van de volledige tekst voor closed captioning en trefwoorden. In dit onderwerp wordt beschreven hoe Media indexering gebruiken."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Mediabestanden met Azure Media indexeerfunctie indexeren


Azure Media indexeerfunctie kunt u de inhoud van uw mediabestanden doorzoekbare en voor het genereren van een transcript van de volledige tekst voor closed captioning en trefwoorden. U kunt één bestand of meerdere mediabestanden in batch verwerken.  

>[AZURE.IMPORTANT] Wanneer u inhoud, zorg ervoor dat mediabestanden die zeer duidelijke speech (zonder achtergrondmuziek, geluid, effecten of hiss microfoon zijn) gebruiken. Voorbeelden van geschikte inhoud zijn: vergaderingen, lezingen en presentaties opgenomen. De volgende inhoud mogelijk niet geschikt voor indexering: films, tv-programma's, alles met gemengde audio- en geluidseffecten, slecht opgenomen inhoud met achtergrondruis (hiss).


Een taak indexeren kan de volgende uitgangen genereren:

- Gesloten bijschrift bestanden in de volgende indelingen: **SAMI**, **TTML**en **WebVTT**.

    Closed caption-ondertiteling bestanden bevatten een label genaamd Recognizability, welke een taak indexering op basis van het hoe herkenbaar de spraak in de bronvideo scores.  U kunt de waarde van Recognizability uitvoerbestanden scherm voor gebruiksgemak. Een lage score betekent slechte indexing resultaten als gevolg van de geluidskwaliteit.
- Trefwoordenbestand (XML).
- Audio indexing blob-bestand (AIB) voor gebruik met SQL server.

    Zie [Bestanden met behulp van AIB met Azure Media voor indexering en SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)voor meer informatie.


In dit onderwerp wordt beschreven hoe indexeertaken **Index een actief** en **meerdere bestanden te indexeren**.

Zie voor de nieuwste updates van de indexeerfunctie van Azure Media [Media Services blogs](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Met behulp van configuratie- en manifest-bestanden voor het indexeren van taken

Kunt u meer details voor uw taken indexeren met behulp van de configuratie van een taak. U kunt bijvoorbeeld opgeven welke metagegevens voor uw media-bestand. Deze metagegevens wordt gebruikt door de engine taal uitbreiden van de woordenschat en sterk verbetert de nauwkeurigheid van de spraakherkenning.  Bent u ook kunnen de gewenste output-bestanden opgeven.

U kunt ook meerdere mediabestanden tegelijk verwerken met behulp van een manifestbestand.

Zie voor meer informatie [Taak vooraf voor Azure Media indexeerfunctie](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Een activum indexeren

De volgende methode uploadt een media-bestand als een actief en maakt u een opdracht voor het indexeren van de activa.

Houd er rekening mee dat als u geen bestand opgeeft, het bestand wordt geïndexeerd met alle standaardinstellingen.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a id="output_files"></a>Output-bestanden

Een indexing taak genereert standaard de volgende output-bestanden. De bestanden worden opgeslagen in de eerste uitvoer actief.

Als er meer dan één invoer mediabestand, indexeerfunctie genereert een manifestbestand voor de job-uitgangen, met de naam 'JobResult.txt'. Voor elke invoer van het mediabestand, worden de resulterende AIB, SAMI TTML, WebVTT en trefwoord-bestanden opeenvolgend genummerd en met de naam met behulp van de 'Alias'.

Bestandsnaam | Beschrijving
----------|------------
__InputFileName.aib__ | Audio indexing blob-bestand. <br /><br /> Audiobestand Indexing Blob (AIB) is een binair bestand dat in Microsoft SQL server met behulp van zoeken in volledige tekst kan worden gezocht.  Het bestand AIB is krachtiger dan de eenvoudige bijschrift bestanden, omdat deze alternatieven voor elk woord, waardoor een veel rijkere zoekfunctie bevat. <br/> <br/>Dit is de installatie van de invoegtoepassing voor indexering SQL op een computer waarop Microsoft SQL server 2008 of hoger vereist. Zoeken in de AIB met behulp van Microsoft SQL biedt zoekactie in volledige tekst nauwkeuriger resultaten dan de ondertiteling bestanden gegenereerd door WAMI zoeken. Dit is omdat de AIB woord-alternatieven die klinken bevat dat de ondertiteling-bestanden de hoogste betrouwbaarheid word voor elk segment van de audio bevatten. Als gesproken woorden zoekt van upmost belang is, wordt aanbevolen gebruik van het AIB In combinatie met Microsoft SQL Server.<br/><br/> Downloaden van de invoegtoepassing, klikt u op <a href="http://aka.ms/indexersql">Azure Media indexeerfunctie SQL-invoegtoepassing</a>. <br/><br/>Het is ook mogelijk gebruikmaken van de andere zoekmachines zoals Apache Lucene/Solr gewoon de video op basis van de ondertiteling en trefwoord XML-bestanden indexeren, maar dit leidt tot minder nauwkeurige resultaten.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Gesloten bijschrift (CC)-bestanden in indelingen SAMI, TTML en WebVTT.<br/><br/>Ze kunnen worden gebruikt om audio en video bestanden toegankelijk maken voor mensen met auditieve handicap.<br/><br/>Closed Caption-bestanden een label genaamd <b>Recognizability</b> , welke scores een taak indexering op basis van hoe herkenbaar spraak in de bronvideo wordt opgenomen.  U kunt de waarde van <b>Recognizability</b> uitvoerbestanden scherm voor gebruiksgemak. Een lage score betekent slechte indexing resultaten als gevolg van de geluidskwaliteit.
__InputFileName.kw.xml<br />InputFileName.info__ |Trefwoord en info-bestanden. <br/><br/>Trefwoordenbestand is een XML-bestand met de trefwoorden die worden opgehaald uit de inhoud van de spraak, met de frequentie en de marge-informatie. <br/><br/>Infobestand is een tekstbestand met gedetailleerde informatie over elke term herkend. De eerste regel is speciaal en bevat de score van Recognizability. Elke volgende regel is een door tabs gescheiden lijst met de volgende gegevens: start time, eindtijd, woord of woordgroep, vertrouwen. De tijden in seconden worden gegeven en het vertrouwen wordt weergegeven als een getal tussen 0-1. <br/><br/>Voorbeeld van de regel: "word 1,20 1,45 0.67" <br/><br/>Deze bestanden kunnen worden gebruikt voor een aantal doeleinden, zoals spraak analytics, uitvoeren of zoekmachines zoals Bing, Google of Microsoft SharePoint om de mediabestanden op te sporen, of zelfs die worden gebruikt voor het leveren van meer relevante advertenties worden blootgesteld.
__JobResult.txt__ |Uitvoer manifest, alleen aanwezig als er meerdere bestanden met de volgende gegevens te indexeren:<br/><br/><table border="1"><tr><th>Invoerbestand</th><th>Alias</th><th>MediaLength</th><th>Fout</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Als niet alle invoer mediabestanden worden geïndexeerd, de indexering taak mislukt met foutcode 4000. Zie voor meer informatie de [foutcodes](#error_codes).

## <a name="index-multiple-files"></a>Meerdere bestanden indexeren

De volgende methode meerdere mediabestanden geüpload als een actief en maakt u een opdracht voor het indexeren van deze bestanden in een batch.

Een manifest-bestand met de extensie .lst wordt gemaakt en uploaden naar het activum. Het manifestbestand bevat de lijst van alle bestanden van de activa. Zie voor meer informatie [Taak vooraf voor Azure Media indexeerfunctie](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Taak gedeeltelijk voltooid

Als niet alle invoer mediabestanden worden geïndexeerd, de indexering taak mislukt met foutcode 4000. Zie voor meer informatie de [foutcodes](#error_codes).


De uitgangen dezelfde (zoals taken voltooid) worden gegenereerd. U kunt verwijzen naar het uitvoerbestand manifest om erachter te komen welke invoerbestanden, volgens de waarden van de kolom fout mislukt. Voor invoerbestanden mislukte, de resulterende AIB, SAMI, TTML, WebVTT en trefwoord worden bestanden niet gegenereerd.

### <a id="preset"></a>Definitie van de taak voor Azure Media Indexer

De verwerking van Azure Media indexeerfunctie kan worden aangepast door middel van een optionele taak vooraf naast de taak.  De volgende tabel ziet de indeling van deze configuratie xml.

Naam | Vereist | Beschrijving
----|----|---
__invoer__ | False | Activa-bestanden die u wilt indexeren.</p><p>Indexering van Azure Media ondersteunt de volgende media-indelingen: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>(Zoals hieronder wordt weergegeven), kunt u de naam van het bestand (s) in het kenmerk **of de **lijst** ** van het element **input** . Als u niet welk bestand actief index opgeeft, wordt het primaire bestand opgenomen. Als er geen primaire elementbestand is ingesteld, wordt het eerste bestand in de invoer activa geïndexeerd.</p><p>Als u expliciet de naam van het activum, doen:<br />`<input name="TestFile.wmv">`<br /><br />U kunt ook meerdere activa bestanden tegelijk (maximaal 10) index. Om dit te doen:<br /><br /><ol class="ordered"><li><p>Een tekstbestand (manifest-bestand) maken en hieraan de extensie .lst. </p></li><li><p>Een lijst met alle namen van de activa in uw invoer actief aan dit manifest bestand toevoegen. </p></li><li><p>Thanifest-bestand (uploaden) toevoegen aan het activum.  </p></li><li><p>Geef de naam van het manifestbestand in het kenmerk van de invoer.<br />`<input list="input.lst">`</li></ol><br /><br />Opmerking: Als u meer dan 10 bestanden aan het manifestbestand toevoegt, de indexering taak mislukt met de foutcode van 2006.
__metagegevens__ | False | Metagegevens voor het opgegeven activa-bestanden gebruikt voor de aanpassing van de woordenlijst.  Handig om voor te bereiden voor indexering herkennen niet-standaard woordenschat woorden zoals eigennamen.<br />`<metadata key="..." value="..."/>` <br /><br />U kunt opgeven dat de __waarden__ voor de vooraf gedefinieerde __sleutels__. De volgende sleutels worden momenteel ondersteund:<br /><br />"titel" en "description" - om de taal voor woordenlijst aanpassing gebruikt model voor uw project en de nauwkeurigheid van de spraakherkenning verbeteren.  De waarden zaad zoekacties op Internet om te zoeken contextueel relevante tekstdocumenten, met behulp van de inhoud te verbeteren de interne woordenlijst voor de duur van de taak van de indexering.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__functies__ <br /><br /> In versie 1.2 wordt toegevoegd. Momenteel is de enige ondersteunde functie Spraakherkenning ('automatisch Systeemherstel').| False | De functie voor spraakherkenning heeft de sleutels van de volgende instellingen:<table><tr><th><p>Sleutel</p></th>     <th><p>Beschrijving</p></th><th><p>Van de Voorbeeldwaarde</p></th></tr><tr><td><p>Taal</p></td><td><p>De natuurlijke taal wordt herkend in het multimediabestand.</p></td><td><p>Engels, Spaans</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>een door puntkomma's gescheiden lijst met de gewenste bijschrift uitvoerindelingen (indien aanwezig)</p></td><td><p>ttml, sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Een Boole-vlag die aangeeft of een bestand AIB vereist (voor gebruik met SQL Server en een IFilter voor indexering van de klant is).  Zie <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Bestanden met behulp van AIB met Azure Media voor indexering en SQL Server</a>voor meer informatie.</p></td><td><p>True; False</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Een Boole-vlag die aangeeft of een trefwoord XML-bestand vereist is.</p></td><td><p>True; De waarde False. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Een Boole-vlag geven al dan niet gedwongen volledige bijschriften (ongeacht het niveau van betrouwbaarheid).  </p><p>Standaard is ingesteld op false, in welk geval woorden en zinnen die een minder dan 50%-betrouwbaarheidsniveau zijn weggelaten uit de uiteindelijke bijschrift uitgangen en vervangen door puntjes ("...").  De puntjes zijn handig voor het bijschrift kwaliteitsbeheersing en controle.</p></td><td><p>True; De waarde False. </p></td></tr></table>

### <a id="error_codes"></a>Foutcodes

In het geval van een fout, Azure Media indexeerfunctie verslag moet één van de volgende foutcodes weergegeven:

Code | Naam | Mogelijke oorzaken
-----|------|------------------
2000 | Ongeldige configuratie | Ongeldige configuratie
2001 | Ongeldige invoer activa | Invoer van activa of lege activa ontbreekt.
2002 | Ongeldig manifest | Het manifest is leeg of het manifest bevat ongeldige items.
2003 | Kan niet downloaden media-bestand | Ongeldige URL in het manifestbestand.
2004 | Niet-ondersteund protocol | Het protocol van media-URL wordt niet ondersteund.
2005 | Niet-ondersteund bestandstype | Invoermedium bestandstype wordt niet ondersteund.
2006 | Te veel invoerbestanden | Er zijn meer dan 10 bestanden in het manifest van de invoer.
3000 | Media-bestand is mislukt. | Niet-ondersteunde mediacodec <br/>of<br/> Beschadigde media-bestand <br/>of<br/> Geen audio-stream in invoermedium.
4000 | Gedeeltelijk batch indexeren is voltooid | Sommige van de invoer mediabestanden zijn kan niet worden geïndexeerd. Zie voor meer informatie de <a href="#output_files">uitvoerbestanden</a>.
andere | Interne fouten | Neem contact op met support team. indexer@microsoft.com


## <a id="supported_languages"></a>Ondersteunde talen

De talen Engels en Spaans worden momenteel ondersteund. Zie voor meer informatie [het weblogbericht v1.2 release](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics overzicht](media-services-analytics-overview.md)

[AIB-bestanden gebruiken in Azure Media voor indexering en SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indexering van mediabestanden met Azure Media indexeerfunctie 2 Preview](media-services-process-content-with-indexer2.md)
