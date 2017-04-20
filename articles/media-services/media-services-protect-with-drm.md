<properties
    pageTitle="Met PlayReady en/of Widevine dynamische gemeenschappelijke codering | Microsoft Azure"
    description="Microsoft Azure Media Services kunt u MPEG-streepje, soepele Streaming en Http-Live Streaming (HLS) past streams met Microsoft PlayReady DRM beveiligd leveren. Bovendien kunt u tot levering gecodeerd met de DRM Widevine streepje. In dit onderwerp wordt beschreven hoe dynamisch codeert met PlayReady en Widevine DRM."
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
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>PlayReady en/of Widevine dynamische gemeenschappelijke codering gebruiken

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Services kunt u MPEG-streepje, soepele Streaming en HTTP-Live Streaming (HLS) past streams die zijn beveiligd met [DRM van Microsoft PlayReady](https://www.microsoft.com/playready/overview/)leveren. Ook kunt u gecodeerde streepje stream leveren met DRM Widevine licenties. Zowel PlayReady als Widevine worden per specificatie van de gemeenschappelijke codering (ISO/IEC 23001-7 CENC) gecodeerd. Kunt u [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (te beginnen met de versie 3.5.1) of REST API voor het configureren van uw AssetDeliveryConfiguration voor het gebruik van Widevine.

Media Services biedt een service voor het leveren van PlayReady en Widevine DRM licenties. Media Services biedt ook een beveiligde inhoud van API's waarmee u bij het configureren van de rechten en beperkingen die u voor de runtime PlayReady of DRM Widevine afdwingen wilt wanneer een gebruiker wordt afgespeeld. Wanneer een gebruiker een DRM beveiligde inhoud aanvraagt, wordt de toepassing player een licentie aanvragen bij de AMS license service. De AMS license service verleent een licentie voor Windows Media Player of deze is gemachtigd. Een licentie voor PlayReady of Widevine bevat de decoderingssleutel die door de client-speler te ontsleutelen en de inhoud stroomsgewijs kan worden gebruikt.


U kunt ook de volgende AMS partners waarmee u licenties Widevine leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Zie voor meer informatie: integratie met [Axinom](media-services-axinom-integration.md) en [castLabs](media-services-castlabs-integration.md).

Media Services biedt ondersteuning voor verschillende manieren van het machtigen van gebruikers die sleutel aanvragen. De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: open of beperking token. Het token beperkte beleid moet vergezeld gaan van een token dat wordt afgegeven door een beveiligde Token Service (STS). Media Services ondersteunt tokens in de [Eenvoudige Web-Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) en [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-indeling. Zie voor meer informatie, de inhoud van de sleutel autorisatiebeleid configureren.

Als u wilt profiteren van dynamische codering, moet u een actief met een set van multi-bitrate MP4-bestanden, of bronbestanden voor soepele Streaming van multi-bitrate hebben. U moet ook de levering beleidsregels configureren voor het activum (Zie verderop in dit onderwerp). Klik, op basis van de opgegeven indeling in de streaming URL, de On-Demand Streaming server zorgt ervoor dat de stroom wordt geleverd in het protocol dat u hebt gekozen. Hierdoor hoeft u alleen te slaan en te betalen voor de bestanden in een enkele indeling en Media Services zal bouwen en de juiste HTTP-antwoord op basis van elke aanvraag van een client fungeren.

Dit onderwerp is handig voor ontwikkelaars die werken voor toepassingen die worden beveiligd met meerdere DRMs, zoals PlayReady en Widevine media leveren. Het onderwerp wordt beschreven hoe u de service PlayReady licentie levering met autorisatiebeleid zodanig configureren dat alleen clients worden PlayReady of Widevine licenties kunnen ontvangen. Ook ziet u het gebruik van dynamische codering codering met PlayReady of Widevine DRM via streepje.

>[AZURE.NOTE]Als u wilt beginnen met dynamische codering, krijgt u ten minste één schaaleenheid (ook bekend als streaming eenheid). Zie [voor het schalen van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.


##<a name="download-sample"></a>Voorbeeld downloaden

U kunt het voorbeeld dat wordt beschreven in dit artikel uit [hier](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm)downloaden.

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Dynamische gemeenschappelijke codering en DRM-licentie Delivery Services configureren

Hieronder volgen de algemene stappen die u uitvoeren moet bij het beschermen van uw activa, met PlayReady, de bezorgservice van Media Services-licentie gebruikt en ook dynamische codering gebruikt.

1. Een actief maken en uploaden van bestanden naar het activum.
1. De activa die het bestand naar de bitsnelheid van het adaptieve die MP4 ingesteld te coderen.
1. Een sleutel maken en koppelen aan het gecodeerde actief. In Media Services bevat de sleutel van het vaste activum coderingssleutel.
1. De inhoud van de sleutel autorisatiebeleid configureren. Het verificatiebeleid voor inhoud sleutel moet worden geconfigureerd door u en voldaan door de client om de sleutel aan de client worden geleverd.

Bij het maken van de inhoud sleutel verificatiebeleid moet u het volgende opgeven: levering methode (PlayReady of Widevine), beperkingen (open of token) en informatie over het type sleutel levering waarmee wordt gedefinieerd hoe de sleutel wordt geleverd aan de client ([PlayReady](media-services-playready-license-template-overview.md) of [Widevine](media-services-widevine-license-template-overview.md) licentie sjabloon).
1. Configureer het beleid voor levering voor een activum. Configuratie van beleid voor de levering omvat: aflevering protocol (bijvoorbeeld MPEG-streepje, HLS, harde schijven, soepele Streaming of alle), het type dynamische codering (bijvoorbeeld algemene codering), PlayReady of de licentie URL Widevine.

U kunt ander beleid toepassen voor elk protocol dat op dezelfde activa. U kan bijvoorbeeld PlayReady codering toepassen op vloeiend/streepje en AES HLS envelop. Alle protocollen die niet zijn gedefinieerd in het beleid voor een levering (bijvoorbeeld u één beleid toevoegen die alleen HLS als het protocol) van streaming worden geblokkeerd. De uitzondering hierop is als u geen activa levering beleid helemaal gedefinieerd. Vervolgens worden alle protocollen kunnen in de open.
1. Maak een OnDemand locator om een streaming URL.

Vindt u een volledig voorbeeld van .NET aan het einde van het onderwerp.

De volgende afbeelding ziet u de hierboven beschreven werkstroom. Hier wordt het token gebruikt voor verificatie.

![Bescherm met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

De rest van dit onderwerp vindt u gedetailleerde uitleg, voorbeelden van programmacode en koppelingen naar onderwerpen waarin wordt beschreven hoe u bereiken van de hierboven beschreven taken.

##<a name="current-limitations"></a>Huidige beperkingen

Als u toevoegen of bijwerken van een levering actief beleid, moet u de bijbehorende locator (indien aanwezig) te verwijderen en een nieuwe locator maken.

Beperking bij het coderen van Widevine met Azure Media Services: meerdere inhoud sleutels worden momenteel niet ondersteund.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Een actief maken en uploaden van bestanden naar het activum

Beheren, coderen en uw video's te streamen, moet u eerst de inhoud uploaden naar Microsoft Azure Media Services. Eenmaal geüpload, wordt de inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

Zie [Bestanden uploaden rekening Media Services](media-services-dotnet-upload-files.md)voor meer informatie.

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Coderen van de activa die het bestand naar de bitsnelheid van het adaptieve die MP4 instellen

Met dynamische codering is alles wat u nodig hebt voor het maken van een actief dat bestaat uit een set van multi-bitrate MP4-bestanden of multi-bitrate goede Streaming-bronbestanden. Klik, op basis van de opgegeven indeling in het manifest en fragment verzoek, de On-Demand Streaming server zal ervoor zorgen dat de gegevensstroom te ontvangen in het protocol dat u hebt gekozen. Hierdoor hoeft u alleen te slaan en te betalen voor de bestanden in één opslagmedium en Media Services-service zal maken en het juiste antwoord op basis van aanvragen van een client fungeren. Zie het onderwerp [Overzicht van dynamische verpakking](media-services-dynamic-packaging-overview.md) voor meer informatie.

Zie voor instructies voor het coderen van [een actief gebruik van Media Encoder standaard coderen](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Een sleutel maken en koppelen aan het gecodeerde actief

In Media Services bevat de sleutel de sleutel die u wilt coderen van een activum.

Zie voor gedetailleerde informatie de [sleutel maken](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>De inhoud van de sleutel autorisatiebeleid configureren

Media Services biedt ondersteuning voor meerdere manieren voor het verifiëren van gebruikers die sleutel aanvragen. Het verificatiebeleid voor inhoud sleutel moet worden geconfigureerd door u en voldaan door de client (player) om de sleutel aan de client worden geleverd. De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: open of beperking token.

Zie [Inhoud sleutel autorisatiebeleid configureren](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption)voor meer informatie.

##<a id="configure_asset_delivery_policy"></a>Activa-levering-beleid configureren 

Configureer het beleid voor levering voor het activum. Dingen die de beleidsconfiguratie activa levering omvat:

- De DRM-licentie de URL. 
- Het activum levering-protocol (bijvoorbeeld MPEG-streepje, HLS, harde schijven, soepele Streaming of alle). 
- Het type dynamische codering (in dit geval gemeenschappelijke codering). 

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

##<a id="example"></a>Voorbeeld


In het volgende voorbeeld ziet u de functionaliteit die is geïntroduceerd in Azure Media Services SDK voor .net-versie 3.5.2 (met name de mogelijkheid een sjabloon voor Widevine definiëren en een licentie Widevine aanvragen van Azure Media Services). De volgende Nuget package opdracht is gebruikt om het pakket te installeren:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Maak een nieuw project in de Console.
1. NuGet installeren en toevoegen van Azure Media Services .NET SDK gebruiken.
2. Aanvullende verwijzingen toevoegen: System.Configuration.
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

1. Ten minste één streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen. Zie voor meer informatie: [streaming eindpunten configureren](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. De code in uw Program.cs bestand overschrijven met de code die in deze sectie wordt weergegeven.
    
    Zorg ervoor dat variabelen om te verwijzen naar de mappen waar uw input bestanden zich bevinden.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
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
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
        
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


## <a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zie ook

[CENC met Multi-DRM- en toegangsbeheer](media-services-cenc-with-multidrm-access-control.md)

[Configureer Widevine verpakking met AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Aankondiging van Google Widevine licentie services voor bezorging in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
