<properties
    pageTitle="Aan de slag met het leveren van inhoud op aanvraag met .NET | Azure"
    description="Deze zelfstudie doorloopt u de stappen voor het implementeren van een op vraag content delivery toepassing met Azure Media Services met behulp van .NET."
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
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Aan de slag met het leveren van inhoud op aanvraag met .NET SDK

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Overzicht 

Deze zelfstudie doorloopt u de stappen voor het implementeren van een Video-on-Demand (VoD) content delivery-toepassing met behulp van Azure Media Services (AMS) SDK voor .NET.


De zelfstudie introduceert de basiswerkstroom Media Services en de meest voorkomende programming objecten en taken die nodig zijn voor de ontwikkeling van Media Services. Aan het einde van de zelfstudie, u kunt streamen of progressief downloaden van een voorbeeld van een mediabestand dat geüpload, gecodeerd en gedownload worden.

## <a name="what-youll-learn"></a>U leert

De zelfstudie wordt beschreven hoe de volgende taken uitvoeren:

1.  Maak een Media-Services-account (via de portal Azure).
2.  Streaming eindpunt (met behulp van de portal Azure) configureren.
3.  Maken en configureren van een Visual Studio-project.
5.  Verbinding maken met de Media-Services-account.
6.  Nieuwe activa maken en uploaden van een videobestand.
7.  Het bronbestand te coderen in een verzameling adaptieve bitsnelheid MP4-bestanden.
8.  Publiceren van de activa en het ophalen van URL's voor streaming en progressief downloaden.
9.  Test uw inhoud afspelen.

## <a name="prerequisites"></a>Vereisten

De volgende zijn vereist voor het voltooien van de zelfstudie.

- Als u deze zelfstudie hebt voltooid, moet u een account Azure. 
    
    Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](/pricing/free-trial/?WT.mc_id=A261C142F). U krijgt credits die kunnen worden gebruikt om te proberen betaald Azure Services. Zelfs nadat de kredieten zijn gebruikt, kunt u de rekening houden en gratis Azure services en voorzieningen, zoals de functie Web Apps in Azure App-Service gebruikt.
- Besturingssystemen: Windows 8 of hoger, Windows 2008 R2, Windows 7.
- .NET framework 4.0 of hoger
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express) of een latere versie.


##<a name="download-sample"></a>Voorbeeld downloaden

Ophalen en uitvoeren van een steekproef van [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Een Azure Media Services account maken met de Azure portal

De stappen in deze sectie wordt aangegeven hoe een AMS-account te maken.

1. Aanmelden op de [portal Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Media + CDN** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer in het vak **Accountnaam**de naam van de nieuwe AMS-account. De naam van een Media Services alle kleine letters, cijfers of letters zonder spaties en 3 tot en met 24 tekens lang is.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling bronnen die delen van de levenscyclus, machtigingen en beleid. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Selecteer de regio wordt op **locatie**, voor het opslaan van de records media en metagegevens voor uw Media Services-account gebruikt. Deze regio wordt gebruikt voor het verwerken en media streamen. De beschikbare Media Services gebieden worden weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer een opslag voor blobopslag van de media-inhoud van uw Media Services-account in **Rekening voor opslag**. Kunt u een bestaande account voor opslag in hetzelfde geografische gebied, als uw Media Services-account, of kunt u een account voor opslag. Een nieuwe account voor de opslag wordt gemaakt in dezelfde regio. De regels voor namen van opslag zijn hetzelfde als voor de accounts van Media Services.

        Meer informatie over opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan het dashboard** voor een overzicht van de voortgang van de implementatie rekening.
    
7. Klik op **maken** onder aan het formulier.

    Zodra de account is gemaakt, verandert de status **actief**. 

    ![Instellingen voor Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld video's te uploaden, activa te coderen, bewaken van de voortgang van het project) het venster **Instellingen** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Streaming eindpunten met de Azure portal configureren

Als u werkt met Azure een van de meest voorkomende scenario's voor de clients levert video via bitsnelheid van adaptieve streaming Media-Services. Media-Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

Media Services biedt dynamische verpakking, kunt u ervoor zorgen dat uw adaptieve bitsnelheid inhoud MP4 gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) just-in-time, zonder dat u hoeft op te slaan van voorverpakte versies van elk van deze indelingen streamen streaming.

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Het bestand met mezzanine (bron) in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie aangetoond) te coderen.  
- Maak ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u leveren uw inhoud wilt. De volgende stappen uit hoe het aantal streaming eenheden wijzigen.

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in de opslagindeling in één en Media Services maakt en het juiste antwoord op basis van aanvragen van een client fungeert.

Maken en wijzigen van het aantal gereserveerde eenheden streaming, doet u het volgende:


1. Klik in het venster **Instellingen voor** **Streaming eindpunten**. 

2. Klik op het eindpunt streaming standaard. 

    Het venster **Std. STREAMING EINDPUNTDETAILS** weergegeven.

3. Als u het aantal eenheden streaming, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming-eenheden](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van alle nieuwe eenheden kan maximaal 20 minuten in beslag nemen.

##<a name="create-and-configure-a-visual-studio-project"></a>Maken en configureren van een Visual Studio-project

1. Maakt een nieuwe C#-consoletoepassing in Visual Studio 2013, Visual Studio 2012 of Visual Studio 2010 SP1. Voer de **naam**, de **locatie**en de **naam van de oplossing**en klik vervolgens op **OK**.

2. Gebruik het pakket [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) NuGet **Azure Media Services .NET SDK-extensies**installeren.  De Media Services .NET SDK-extensies is een set van uitbreidingsmethoden en Help-functies die uw code eenvoudiger en gemakkelijker te ontwikkelen met Media-Services. Installatie van dit pakket ook **.NET SDK van Media Services** wordt geïnstalleerd en alle vereiste afhankelijkheden toegevoegd.

3. Voeg een verwijzing naar de assembly System.Configuration. Deze assembly bevat de klasse **System.Configuration.ConfigurationManager** die wordt gebruikt voor toegang tot de configuratiebestanden, bijvoorbeeld App.config.

4. Open het bestand App.config (het bestand toevoegen aan uw project als het is niet standaard toegevoegd) en een sectie *appSettings* toevoegen aan het bestand. Stel de waarden voor uw Azure Media Services-account de naam en account sleutel, zoals in het volgende voorbeeld wordt getoond. Als u de accountnaam en belangrijke informatie, gaat u naar de [portal Azure](https://portal.azure.com/) en selecteer de account van de AMS. Selecteer **Instellingen** > **sleutels**. De sleutels beheren van windows bevat de naam en de primaire en secundaire sleutels worden weergegeven.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. De bestaande **met behulp van** instructies aan het begin van het Program.cs-bestand overschrijven met de volgende code.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Maak een nieuwe map onder de map projecten en kopieer een .mp4 of .wmv-bestand dat u wilt coderen en streamen of progressief te downloaden. In dit voorbeeld wordt het pad "C:\VideoFiles" gebruikt.

##<a name="connect-to-the-media-services-account"></a>Verbinding maken met de Media-Services-account

Als u Media Services met .NET, moet u de klasse **CloudMediaContext** voor de meeste Media Services programmeertaken: verbinding maken met Media-Services-account; maken, bijwerken, openen en verwijderen van de volgende objecten: activa, activa-bestanden, taken, -beleid, Locator, enz.

De standaard programma klasse overschrijven met de volgende code. De code ziet u hoe de waarden van de verbinding van het bestand App.config lezen en het maken van het object **CloudMediaContext** de verbinding met Media Services. Zie voor meer informatie over verbinding maken met Media Services [verbinding maken met Media-Services met de Media Services SDK voor .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

De **belangrijkste** functie roept methoden die worden gedefinieerd in deze sectie verder.

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
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Nieuwe activa maken en uploaden van een videobestand

In Media-Services u uploaden of consumptie van uw digitale bestanden in een actief. De entiteit **activa** kan bevatten, video, audio, afbeeldingen, miniaturen, verzamelingen, tekst tracks, en ondertiteling bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden zijn geüpload, wordt de inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. De bestanden in de activa zijn **Activa bestanden**genoemd.

De **UploadFile** methode aanroepen van **CreateFromFile** (zoals gedefinieerd in de .NET SDK extensies) hieronder. **CreateFromFile** maakt een nieuwe activa waarop het opgegeven bestand is geüpload.

De methode **CreateFromFile** wordt **AssetCreationOptions** , waarin u kunt opgeven dat een van de volgende opties voor het activum maken:

- **Geen** - geen codering wordt gebruikt. Dit is de standaardwaarde. Er rekening mee dat wanneer u deze optie gebruikt, de inhoud niet in doorvoer of opslag.
Als u van plan bent om een MP4 met progressieve download deze optie gebruiken.
- **StorageEncrypted** - Gebruik deze optie voor het coderen van duidelijke inhoud lokaal met Advanced Encryption Standard (AES)-256-bits codering, die vervolgens geüpload naar Azure opslag waar het is opgeslagen in rust gecodeerd. Activa met opslag codering beveiligd worden ongecodeerd automatisch geplaatst in een gecodeerde-bestandssysteem voor de codering en eventueel opnieuw gecodeerd voordat u het uploadt terug als een nieuwe uitvoer actief. Het primaire gebruik geval voor opslag codering is wanneer u de hoge kwaliteit invoermedium bestanden beveiligen met sterke codering in rust op schijf.
- **CommonEncryptionProtected** - Gebruik deze optie als u inhoud die al zijn gecodeerd en beveiligd met een gemeenschappelijke codering of PlayReady DRM (bijvoorbeeld goede Streaming beveiligd met DRM PlayReady) wilt uploaden.
- **EnvelopeEncryptionProtected** : Gebruik deze optie als u HLS versleuteld met AES uploaden wilt. Houd er rekening mee dat de bestanden moeten zijn gecodeerd en gecodeerd door Manager transformeren.

De methode **CreateFromFile** kunt u ook een retouraanroep voor het rapporteren van de voortgang van het uploaden van het bestand opgeven.

In het volgende voorbeeld geven we **geen** voor de opties van het activum.

De volgende methode toevoegen aan de klasse van het programma.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Het bronbestand in een verzameling adaptieve bitsnelheid MP4-bestanden coderen

Na ingesting activa in Media Services, media kunnen worden gecodeerd, transmuxed, een watermerk, enzovoort, voordat deze wordt afgeleverd aan clients. Deze activiteiten worden gepland en uitgevoerd voor meerdere exemplaren van achtergrond rol te garanderen optimale prestaties en beschikbaarheid. Deze activiteiten worden taken genoemd, en elke taak bestaat uit atomaire taken waarmee de werkelijke hoeveelheid werk voor het bestand actief.

Zoals is gezegd, bij het werken met Azure Media Services, levert een van de meest voorkomende scenario's adaptieve bitsnelheid die stroomsgewijs naar uw klanten. Media-Services dynamisch adaptieve bitsnelheid MP4-bestanden kunt inpakken in een van de volgende indelingen: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Coderen of transcoderen de mezzanine (bron) bestanden in een verzameling adaptieve bitsnelheid MP4-bestanden of adaptieve bitsnelheid goede Streaming-bestanden.  
- Ten minste één streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen.

De volgende code laat zien hoe een taak codering. De taak bevat een taak die te transcoderen het mezzanine-bestand in een set met **Media Encoder standaard**adaptief bitsnelheid MP4s. De code dient de taak en wacht totdat deze is voltooid.

Zodra de taak is voltooid, kunt u zou kunnen streamen van uw activa of MP4-bestanden die zijn gemaakt door transcodering progressief te downloaden.
Overigens hoeft u niet meer dan 0 streaming eenheden hebben progressief MP4-bestanden te downloaden.

De volgende methode toevoegen aan de klasse van het programma.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Publiceren van de activa en ophalen van URL's voor streaming en progressief downloaden

Als u wilt streamen of downloaden van een actief, moet u eerst 'publiceren' door het maken van een locator. Locators bieden toegang tot bestanden in de activa. Media Services biedt ondersteuning voor twee soorten locators: OnDemandOrigin locators, stream media (bijvoorbeeld MPEG-streepje, HLS of goede Streaming) en Access-handtekening (SAS)-locators gebruikt om mediabestanden te downloaden.

Nadat u de locators hebt gemaakt, kunt u de URL's die worden gebruikt om te streamen of te downloaden van uw bestanden maken.


Voor soepele Streaming streaming URL heeft de volgende indeling:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Een streaming URL voor HLS heeft de volgende indeling:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Voor MPEG-streepje een streaming-URL heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Een SAS-URL die wordt gebruikt om bestanden te downloaden, heeft de volgende indeling:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Uitbreidingen voor .NET SDK van Media Services bieden handige methoden die resulteren in opgemaakte URL's voor de gepubliceerde activa.

De volgende code gebruikt .NET SDK extensies locators maken en streaming ophalen en progressieve download-URL's. De code toont ook het downloaden van bestanden naar een lokale map.

De volgende methode toevoegen aan de klasse van het programma.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Test uw inhoud afspelen  

Als u het programma dat is gedefinieerd in de vorige sectie hebt uitgevoerd, wordt de URL van de volgende strekking wordt weergegeven in het consolevenster.

Adaptieve streaming URL's:

Goede Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG-STREEPJE

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Progressieve download-URL's (audio en video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Als u video streamen, [Azure Services mediaspeler](http://amsplayer.azurewebsites.net/azuremediaplayer.html)te gebruiken.

Progressieve download testen, plak een URL in een browser (bijvoorbeeld Internet Explorer, Chrome of Safari).


##<a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Mediaservices leren werken met paden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Op zoek naar iets anders?

Als dit onderwerp niet wat u verwachtte, is er iets ontbreekt, of in een andere wijze niet aan uw behoeften voldoet, geef ons uw feedback met behulp van de onderstaande Disqus-thread.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
