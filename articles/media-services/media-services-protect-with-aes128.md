<properties
    pageTitle="Met behulp van dynamische encryption AES 128 en belangrijke bezorgservice | Microsoft Azure"
    description="Microsoft Azure Media Services kunt u ervoor zorgen dat uw inhoud is gecodeerd met de AES 128-bits coderingssleutels. Media Services is ook de sleutel bezorgservice die sleutels voor gegevenscodering voor geautoriseerde gebruikers zorgt. In dit onderwerp wordt beschreven hoe dynamisch codeert met AES-128 en gebruik van de leveringsservice key."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Met behulp van dynamische encryption AES 128 en belangrijke bezorgservice

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Overzicht

>[AZURE.NOTE] Zie [deze](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video voor meer informatie over het beveiligen van uw Media-inhoud met AES-codering.

Microsoft Azure Media Services kunt u ervoor zorgen dat Http-Live-Streaming (HLS) past en goede Streams die zijn gecodeerd met Advanced Encryption Standard (AES) (met 128-bits coderingssleutels). Media Services is ook de sleutel bezorgservice die sleutels voor gegevenscodering voor geautoriseerde gebruikers zorgt. Als u Media-Services voor het coderen van een actief, moet u een coderingssleutel te koppelen aan het activum en ook autorisatiebeleid voor de sleutel te configureren. Als een stroom door een speler wordt aangevraagd, gebruikt Media Services de opgegeven sleutel voor het coderen van dynamische inhoud met AES-codering. Om te decoderen van de gegevensstroom, de speler vraagt om de sleutel van de belangrijkste leveringsservice. De service geëvalueerd om te bepalen of de gebruiker is gemachtigd om de sleutel, het autorisatiebeleid dat u hebt opgegeven voor de sleutel.

Media Services biedt ondersteuning voor meerdere manieren voor het verifiëren van gebruikers die sleutel aanvragen. De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: open of beperking token. Het token beperkte beleid moet vergezeld gaan van een token dat wordt afgegeven door een beveiligde Token Service (STS). Media Services ondersteunt tokens in de [Eenvoudige Web-Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) en [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-indeling. Zie [de inhoud van de sleutel autorisatiebeleid configureren](media-services-protect-with-aes128.md#configure_key_auth_policy)voor meer informatie.

Als u wilt profiteren van dynamische codering, moet u een actief met een set van multi-bitrate MP4-bestanden, of bronbestanden voor soepele Streaming van multi-bitrate hebben. U moet ook de levering beleid configureren voor het activum (Zie verderop in dit onderwerp). Klik, op basis van de opgegeven indeling in de streaming URL, de On-Demand Streaming server zorgt ervoor dat de stroom wordt geleverd in het protocol dat u hebt gekozen. Hierdoor hoeft u alleen te slaan en te betalen voor de bestanden in één opslagmedium en Media Services-service zal maken en het juiste antwoord op basis van aanvragen van een client fungeren.

Dit onderwerp is handig voor ontwikkelaars die werken aan toepassingen die beveiligde media leveren. Het onderwerp wordt beschreven hoe u de leveringsservice key met autorisatiebeleid zodanig configureren dat alleen gemachtigde clients de coderingssleutels kunnen ontvangen. Ook ziet u hoe u dynamische codering gebruikt.

>[AZURE.NOTE]Als u wilt beginnen met dynamische codering, krijgt u ten minste één schaaleenheid (ook bekend als streaming eenheid). Zie [voor het schalen van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128 dynamische codering en werkstroom Key levering

Hieronder volgen de algemene stappen die u uitvoeren moet bij het coderen van uw activa, met AES, de belangrijkste bezorgingsservice Media Services gebruikt en ook dynamische codering gebruikt.

1. [Een actief maken en uploaden van bestanden naar het activum](media-services-protect-with-aes128.md#create_asset).
1. [Coderen van de activa die het bestand naar de bitsnelheid van het adaptieve die MP4 instellen](media-services-protect-with-aes128.md#encode_asset).
1. [Een sleutel maken en koppelen aan de gecodeerde activa](media-services-protect-with-aes128.md#create_contentkey). In Media Services bevat de sleutel van het vaste activum coderingssleutel.
1. [De inhoud van de sleutel autorisatiebeleid configureren](media-services-protect-with-aes128.md#configure_key_auth_policy). Het verificatiebeleid voor inhoud sleutel moet worden geconfigureerd door u en voldaan door de client om de sleutel aan de client worden geleverd.
1. [Het beleid voor levering voor een activum configureren](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Configuratie van beleid voor de levering omvat: de URL en initialisatie Vector (IV) (AES 128 moet worden verstrekt bij het coderen en decoderen van de dezelfde IV), levering-protocol (bijvoorbeeld MPEG-streepje, HLS, harde schijven, soepele Streaming of alle), het type dynamische codering (bijvoorbeeld envelop of geen dynamische codering).

U kunt ander beleid toepassen voor elk protocol dat op dezelfde activa. U kan bijvoorbeeld PlayReady codering toepassen op vloeiend/streepje en AES HLS envelop. Alle protocollen die niet zijn gedefinieerd in het beleid voor een levering (bijvoorbeeld u één beleid toevoegen die alleen HLS als het protocol) van streaming worden geblokkeerd. De uitzondering hierop is als u geen activa levering beleid helemaal gedefinieerd. Vervolgens worden alle protocollen kunnen in de open.

1. [Maken een OnDemand locator](media-services-protect-with-aes128.md#create_locator) om een streaming URL.

Het onderwerp ziet u ook [hoe een clienttoepassing een sleutel van de belangrijkste bezorgservice kunt aanvragen](media-services-protect-with-aes128.md#client_request).

U vindt een volledige .NET [voorbeeld](media-services-protect-with-aes128.md#example) aan het einde van het onderwerp.

De volgende afbeelding ziet u de hierboven beschreven werkstroom. Hier wordt het token gebruikt voor verificatie.

![Beveiligen met AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

De rest van dit onderwerp vindt u gedetailleerde uitleg, voorbeelden van programmacode en koppelingen naar onderwerpen waarin wordt beschreven hoe u bereiken van de hierboven beschreven taken.

##<a name="current-limitations"></a>Huidige beperkingen

Als u toevoegen of bijwerken van beleid voor levering van het actief, moet u een bestaande locator verwijderen (indien aanwezig) en het maken van een nieuwe locator.

##<a id="create_asset"></a>Een actief maken en uploaden van bestanden naar het activum

Beheren, coderen en uw video's te streamen, moet u eerst de inhoud uploaden naar Microsoft Azure Media Services. Eenmaal geüpload, wordt de inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

Zie [Bestanden uploaden rekening Media Services](media-services-dotnet-upload-files.md)voor meer informatie.

##<a id="encode_asset"></a>Coderen van de activa die het bestand naar de bitsnelheid van het adaptieve die MP4 instellen

Met dynamische codering is alles wat u nodig hebt voor het maken van een actief dat bestaat uit een set van multi-bitrate MP4-bestanden of multi-bitrate goede Streaming-bronbestanden. Klik, op basis van de opgegeven indeling in het manifest of fragment verzoek, de On-Demand Streaming server zal ervoor zorgen dat de gegevensstroom te ontvangen in het protocol dat u hebt gekozen. Hierdoor hoeft u alleen te slaan en te betalen voor de bestanden in één opslagmedium en Media Services-service zal maken en het juiste antwoord op basis van aanvragen van een client fungeren. Zie het onderwerp [Overzicht van dynamische verpakking](media-services-dynamic-packaging-overview.md) voor meer informatie.

Zie voor instructies voor het coderen van [een actief gebruik van Media Encoder standaard coderen](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Een sleutel maken en koppelen aan het gecodeerde actief

In Media Services bevat de sleutel de sleutel die u wilt coderen van een activum.

Zie voor gedetailleerde informatie de [sleutel maken](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>De inhoud van de sleutel autorisatiebeleid configureren

Media Services biedt ondersteuning voor meerdere manieren voor het verifiëren van gebruikers die sleutel aanvragen. Het verificatiebeleid voor inhoud sleutel moet worden geconfigureerd door u en voldaan door de client (player) om de sleutel aan de client worden geleverd. De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: open, beperking of restrictie voor IP-token.

Zie [Inhoud sleutel autorisatiebeleid configureren](media-services-dotnet-configure-content-key-auth-policy.md)voor meer informatie.

##<a id="configure_asset_delivery_policy"></a>Activa-levering-beleid configureren 

Configureer het beleid voor levering voor het activum. Dingen die de beleidsconfiguratie activa levering omvat:

- De sleutel de URL. 
- De initialisatie Vector (IV) te gebruiken voor het coderen van de envelop. AES 128, moet het dezelfde IV te leveren bij het coderen en decoderen. 
- Het activum levering-protocol (bijvoorbeeld MPEG-streepje, HLS, harde schijven, soepele Streaming of alle).
- Het type dynamische codering (bijvoorbeeld AES envelop) of geen dynamische codering. 

Zie voor gedetailleerde informatie [configureren activa levering beleid ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Maak een OnDemand locator om een URL streaming streaming

U moet de gebruiker met streaming URL voor vloeiende, STREEPJES of HLS bieden.

>[AZURE.NOTE]Als u toevoegen of bijwerken van beleid voor levering van het actief, moet u een bestaande locator verwijderen (indien aanwezig) en het maken van een nieuwe locator.

Zie [een streaming URL maken](media-services-deliver-streaming-content.md)voor instructies over het publiceren van een actief en een streaming URL maken.

##<a name="get-a-test-token"></a>Een test token ophalen

Krijg een test token op basis van het token beperking die is gebruikt om het verificatiebeleid sleutel.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

De [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) kunt u de gegevensstroom testen.

##<a id="client_request"></a>Hoe kan de client een sleutel van de leveringsservice key aanvragen?

In de vorige stap moet u de URL die naar een manifestbestand verwijst samengesteld. De client moet de nodige informatie ophalen uit de streaming manifest-bestanden om een aanvraag bij de leveringsservice key.

###<a name="manifest-files"></a>Manifest-bestanden

De client moet ophalen van de URL (ook met sleutel Id (kid)) waarde van het manifestbestand. De client probeert vervolgens de sleutel ophalen van de sleutel bezorgservice. De client moet ook de waarde IV en gebruik het decoderen van de stroom te halen. Het volgende fragment wordt getoond de <Protection> -element van het manifest voor soepele Streaming.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

Het manifest van de hoofdmap is voor HLS onderverdeeld in bestanden met segment. 

Het manifest van de hoofdmap is bijvoorbeeld: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) en bevat een lijst met namen in het segment.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Als u een van de segment-bestanden in een teksteditor openen (bijvoorbeeld, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), moet deze bevatten #EXT-X-toets, waarmee wordt aangegeven dat het bestand is gecodeerd.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>De sleutel aanvragen bij de leveringsservice key

De volgende code ziet u hoe een verzoek verzonden naar de Media levering belangrijke services met behulp van een sleutel levering Uri (die is geëxtraheerd uit het manifest) en een token (dit onderwerp spreekt niet over het eenvoudig Web Tokens krijgen via een beveiligde Token Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Voorbeeld

1. Maak een nieuw project in de Console.
1. Gebruik NuGet installeren en Azure Media Services .NET SDK extensies toevoegen. Installatie van dit pakket ook .NET SDK van Media Services wordt geïnstalleerd en alle vereiste afhankelijkheden toegevoegd.
2. Config-bestand met de naam en de belangrijkste informatie toevoegen:

    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. De code in uw Program.cs bestand overschrijven met de code die in deze sectie wordt weergegeven.
    
    Zorg ervoor dat variabelen om te verwijzen naar de mappen waar uw input bestanden zich bevinden.
            
        
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
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
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
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
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
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "HLS Open Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null // no requirements needed for HLS
                                };
        
                    restrictions.Add(restriction);
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                        "policy",
                        ContentKeyDeliveryType.BaselineHttp,
                        restrictions,
                        "");
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                            new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                            new ContentKeyAuthorizationPolicyRestriction
                            {
                                Name = "Token Authorization Policy",
                                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                Requirements = tokenTemplateString
                            };
        
                    restrictions.Add(restriction);
        
                    //You could have multiple options 
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                            "Token option for HLS",
                            ContentKeyDeliveryType.BaselineHttp,
                            restrictions,
                            null  // no key delivery data is needed for HLS
                            );
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        
                    return tokenTemplateString;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
        
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
