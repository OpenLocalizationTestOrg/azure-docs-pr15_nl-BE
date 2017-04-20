<properties 
    pageTitle="Het uitvoeren van live streaming met Azure Media Services multi-bitrate streams maken met .NET | Microsoft Azure" 
    description="Deze zelfstudie doorloopt u de stappen voor het maken van een kanaal dat u ontvangt een live gegevensstroom één bitsnelheid en op meerdere bitsnelheid met .NET SDK wordt gecodeerd." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Het uitvoeren van live streaming met Azure Media Services multi-bitrate streams maken met .NET

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Overzicht

Deze zelfstudie doorloopt u de stappen voor het maken van een **kanaal** dat een live gegevensstroom één bitsnelheid ontvangt en op meerdere bitsnelheid wordt gecodeerd.

Zie voor meer conceptuele informatie over kanalen die zijn ingeschakeld voor live-codering, [Live streaming met Media Services Azure multi-bitrate streams maken](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Gangbare Scenario voor Live Streaming

De volgende stappen beschrijven de taken die betrokken zijn bij het maken van gemeenschappelijke live streaming toepassingen.

>[AZURE.NOTE] Momenteel is de maximale aanbevolen duur van een live gebeurtenis 8 uur. Neem contact op met amslived op Microsoft.com als u nodig hebt voor het uitvoeren van een kanaal voor een langere periode.

1. Een video-camera aansluit op een computer. Starten en configureren van een live op gebouwen-encoder die een enkele bitsnelheid in een van de volgende protocollen: RTMP, soepele Streaming of RTP (MPEG-TS). Zie voor meer informatie [Azure Media Services RTMP-ondersteuning en Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).

Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.

1. Maak en start een kanaal.

1. Ophalen van het kanaal URL nemen.

De URL ingest wordt gebruikt door het coderingsprogramma live stream verzenden naar het kanaal.

1. De voorvertoning kanaal URL ophalen.

Deze URL gebruiken om te controleren of uw kanaal juist de live gegevensstroom ontvangt.

2. Een actief maken.
3. Als u wilt dat het activum naar dynamisch worden gecodeerd tijdens het afspelen, het volgende doen:
1. Maak een sleutel.
1. De inhoud van de sleutel autorisatiebeleid configureren.
1. Activa levering beleid (gebruikt door dynamische verpakking en dynamische codering) configureren.
3. Maak een programma en geef voor het gebruik van het activum dat u hebt gemaakt.
1. De activa die zijn gekoppeld aan het programma door het maken van een OnDemand locator publiceren.

Zorg ervoor dat ten minste één gereserveerde eenheid op de streaming eindpunt waarvoor u inhoud wilt streamen.

1. Start het programma wanneer u klaar bent om te beginnen met streaming en archivering.
2. De live-encoder kan eventueel starten een advertentie worden gesignaleerd. De advertentie wordt in de uitvoerstroom ingevoegd.
1. Het programma stopt wanneer u wilt stoppen streaming en archivering van de gebeurtenis.
1. Het programma verwijderen (en eventueel verwijderen van de activa).

## <a name="what-youll-learn"></a>U leert

In dit onderwerp wordt beschreven hoe u verschillende bewerkingen op kanalen en programma's met .NET SDK van Media Services uitvoeren. Omdat er veel bewerkingen zijn langdurige .NET API's waarmee langdurige worden bewerkingen gebruikt.

Het onderwerp ziet u hoe u het volgende doen:

1. Maak en start een kanaal. Langdurige API's worden gebruikt.
1. Get consumptie (input) eindpunt van de kanalen. Dit eindpunt moet worden verstrekt met het coderingsprogramma dat een enkele live bitsnelheid kunt verzenden.
1. Het eindpunt voorbeeld krijgen. Dit eindpunt wordt gebruikt om de stream te bekijken.
1. Activa die worden gebruikt voor het opslaan van uw inhoud maken. De levering actief beleid moeten ook, zoals in dit voorbeeld worden geconfigureerd.
1. Maak een programma en geef voor het gebruik van de activa die eerder is gemaakt. Start het programma. Langdurige API's worden gebruikt.
1. Maak een locator voor het activum, zodat de inhoud wordt gepubliceerd en kan worden gestreamd naar uw klanten.
1. Weergeven en verbergen van leien. Starten en stoppen van advertenties. Langdurige API's worden gebruikt.
1. Schoonmaken van het kanaal en de bijbehorende bronnen.


##<a name="prerequisites"></a>Vereisten

De volgende zijn vereist voor het voltooien van de zelfstudie.

- Als u deze zelfstudie hebt voltooid, moet u een account Azure.

Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](/pricing/free-trial/?WT.mc_id=A261C142F). U krijgt credits die kunnen worden gebruikt om te proberen betaald Azure Services. Zelfs nadat de kredieten zijn gebruikt, kunt u de rekening houden en gratis Azure services en voorzieningen, zoals de functie Web Apps in Azure App-Service gebruikt.
- Een Media-Services-account. Zie maken van een Media-Services-account, [Account maken](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express) of een latere versie.
- Moet u Media Services .NET SDK versie 3.2.0.0 of hoger.
- Een webcam en een coderingsprogramma dat een enkele live bitsnelheid kunt verzenden.

##<a name="considerations"></a>Overwegingen met betrekking tot

- Momenteel is de maximale aanbevolen duur van een live gebeurtenis 8 uur. Neem contact op met amslived op Microsoft.com als u nodig hebt voor het uitvoeren van een kanaal voor een langere periode.
- Zorg ervoor dat ten minste één gereserveerde eenheid op de streaming eindpunt waarvoor u inhoud wilt streamen.

##<a name="download-sample"></a>Voorbeeld downloaden

Ophalen en uitvoeren van een steekproef van [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Voor het ontwikkelen met Media Services SDK voor .NET instellen

1. Maak een consoletoepassing met behulp van Visual Studio.
1. De Media Services SDK voor .NET toevoegen aan de consoletoepassing met behulp van Media Services NuGet package.

##<a name="connect-to-media-services"></a>Verbinding maken met mediaservices
Beste, moet u een bestand app.config gebruiken voor het opslaan van de naam en account-sleutel van Media Services.

>[AZURE.NOTE]Als de naam en sleutel waarden, gaat u naar de portal Azure en selecteer uw account. Het venster instellingen verschijnt aan de rechterkant. Selecteer in het venster Instellingen toetsen. De waarde te klikken op het pictogram naast elk tekstvak worden gekopieerd naar het systeemklembord.

De sectie appSettings toevoegen aan het bestand app.config en stel de waarden voor de sleutel Media Services-account de naam en account.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Voorbeeld van code

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Op zoek naar iets anders?

Als dit onderwerp niet wat u verwachtte, is er iets ontbreekt, of in een andere wijze niet aan uw behoeften voldoet, geef ons u feedback met behulp van de onderstaande Disqus-thread.
