<properties 
    pageTitle="Beveiligen van uw inhoud met Apple FairPlay en/of Microsoft PlayReady HLS | Microsoft Azure" 
    description="Dit onderwerp bevat een overzicht en Azure Media Services gebruikt voor het coderen van de inhoud van uw HTTP Live Streaming (HLS) past met Apple FairPlay dynamisch. Ook ziet u hoe met de bezorgservice van Media Services licentie leveren FairPlay licenties aan clients." 
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
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Beveiligen van uw inhoud met Apple FairPlay en/of Microsoft PlayReady HLS

Azure Media Services kunt u dynamisch codeert de inhoud van uw HTTP Live Streaming (HLS) past met de volgende indelingen:  

- **AES-128 envelop wissen toets** 

    Het hele segment wordt gecodeerd met de **AES-128 CBC** modus. Het decoderen van de gegevensstroom wordt door iOS en OSX player ondersteund. Zie [dit artikel](media-services-protect-with-aes128.md)voor meer informatie.

- **Apple FairPlay** 

    De afzonderlijke monsters van video en audio zijn gecodeerd met de **AES-128 CBC** modus. **FairPlay-Streaming** (FPS) is geïntegreerd in het besturingssysteem van het apparaat met native ondersteuning voor Apple TV en iOS. Safari in OS X kan met behulp van gecodeerde Media extensies (EME) interfaceondersteuning FPS.
- **Microsoft PlayReady**

De volgende afbeelding ziet u de werkstroom **HLS + FairPlay en/of PlayReady dynamische codering** .

![Bescherm met FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

In dit onderwerp wordt beschreven hoe Azure Media Services gebruiken voor het coderen van de inhoud van de HLS met Apple FairPlay dynamisch. Ook ziet u hoe met de bezorgservice van Media Services licentie leveren FairPlay licenties aan clients.

>[AZURE.NOTE] Als u ook coderen van de inhoud van de HLS met PlayReady wilt, moet u een gemeenschappelijke sleutel inhoud maken en koppelen aan uw activa. Moet u ook de inhoud van de sleutel vergunning om beleid te configureren, zoals wordt beschreven in het onderwerp [PlayReady met behulp van dynamische gemeenschappelijke codering](media-services-protect-with-drm.md) .

    
## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

- Het volgende is vereist bij het gebruik van AMS leveren HLS gecodeerd met FairPlay en FairPlay licenties te leveren.

    - Een Azure-account. Zie voor meer informatie, [Gratis proefperiode van Azure](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Een Media-Services-account. Zie maken van een Media-Services-account, [Account maken](media-services-portal-create-account.md).
    - Aanmelden bij [Apple Development programma](https://developer.apple.com/).
    - Apple moet de eigenaar van de inhoud het [implementatiepakket](https://developer.apple.com/contact/fps/)downloaden. Staat de aanvraag die u al geïmplementeerd KSM (Key Security Module) met Azure Media Services en dat u het uiteindelijke FPS pakket aanvraagt. Er zijn aanwijzingen in het uiteindelijke pakket FPS genereren certificering en het verkrijgen van vragen die u gebruiken wilt voor het configureren van FairPlay. 

    - Azure .NET SDK van Media Services versie **3.6.0** of hoger.

- De volgende zaken moeten worden ingesteld op AMS key levering aan de zijkant:
    - **App-Cert (AC)** - .pfx-bestand met de persoonlijke sleutel. Dit bestand is gemaakt door de klant en met een wachtwoord is gecodeerd met dezelfde klant. 
        
        Wanneer de klant belangrijke levering beleid configureert, moeten zij dit wachtwoord en het .pfx in base64-indeling opgeven.

        De volgende stappen wordt beschreven hoe u een pfx-certificaat te genereren voor FairPlay.
        
        1. OpenSSL installeren vanaf https://slproweb.com/products/Win32OpenSSL.html
        
            Ga naar de map waarin het certificaat FairPlay en andere bestanden die door Apple worden geleverd.
        
        2. Het cer converteren naar pem opdrachtregel:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-der kennis-in fairplay.cer-out fairplay-out.pem
        
        3. Opdrachtregel omzetten in pem pfx met de persoonlijke sleutel (het wachtwoord voor het pfx-bestand wordt vervolgens gevraagd door OpenSSL).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-Exporteer - fairplay-out.pfx-inkey privatekey.pem-in fairplay out.pem passin - file:privatekey-pem-pass.txt 
        
    - **App Cert-wachtwoord** - wachtwoord van de klant voor het maken van het pfx-bestand.
    - **Wachtwoord-ID app Cert** - de klant moet het wachtwoord dat vergelijkbaar is met hoe ze andere sleutels AMS en enum-waarde **ContentKeyType.FairPlayPfxPassword** uploaden uploaden. Hierdoor krijgt ze AMS id die dit is wat ze moeten binnen de optie belangrijke levering beleid gebruiken.
    - **iv** - willekeurige waarde van 16 bytes, moet overeenkomen met de iv in de levering actief beleid. Klant genereert de IV en plaatst deze in beide plaatsen: actief en de sleutel aflevering beleid optie voor levering. 
    - **ASK** - ASK (toepassing geheime sleutel) wordt ontvangen bij het genereren van de certificering met behulp van de Apple Developer-portal. Elk ontwikkelingsteam ontvangt een unieke vragen. Sla een kopie van de vraag en slaat u deze op een veilige plaats. U moet vragen als FairPlayAsk later Azure Media Services configureren. 
    -  **Vraag-ID** - wordt verkregen wanneer de klant vragen in AMS uploadt. De klant moet uploaden ASk **ContentKeyType.FairPlayASk** enum-waarde gebruiken. De AMS-id wordt geretourneerd als resultaat en dit is wat moet worden gebruikt bij het instellen van de optie belangrijke levering beleid.

- De volgende zaken moeten worden ingesteld door de client FPS:
    - **App-Cert (AC)** -.cer/.der-bestand met openbare sleutel die OS wordt gebruikt voor het coderen van een nettolading. AMS moet weten omdat deze door Windows media player is vereist. De bezorgservice sleutel wordt gedecodeerd met behulp van de bijbehorende persoonlijke sleutel.

- Een versleutelde gegevensstroom FairPlay afspelen moet u echte ASK eerste en vervolgens een echte certificaat te genereren. Dit proces maakt alle 3 delen:

    -  .der, 
    -  .pfx en 
    -  het wachtwoord voor de .pfx.
 
- Clients die ondersteuning bieden voor HLS met **AES-128 CBC** codering: Safari op OS X, Apple TV, iOS.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Stappen voor het configureren van FairPlay dynamische codering en licentie leveringsservices

Hieronder volgen de algemene stappen die u uitvoeren moet bij het beschermen van uw activa met FairPlay, de bezorgservice van Media Services-licentie gebruikt en ook dynamische codering gebruikt.

1. Een actief maken en uploaden van bestanden naar het activum. 
1. De activa die het bestand naar de bitsnelheid van het adaptieve die MP4 ingesteld te coderen.
1. Een sleutel maken en koppelen aan het gecodeerde actief.  
1. De inhoud van de sleutel autorisatiebeleid configureren. Bij het maken van de inhoud belangrijk beleid, moet u het volgende opgeven: 
    
    - leveringsmethode (in dit geval FairPlay) 
    - Opties voor configuratie van FairPlay. Zie voor meer informatie over het configureren van FairPlay methode ConfigureFairPlayPolicyOptions() in het onderstaande voorbeeld.
    
        >[AZURE.NOTE] Normaal gesproken wilt FairPlay beleid om opties te configureren slechts eenmaal, aangezien u alleen één set van certificering en vragen hebt.
-beperkingen (open of token)- en specifieke informatie voor het type sleutel levering waarmee wordt gedefinieerd hoe de sleutel naar de client wordt geleverd. 
    
2. Activa levering beleid configureren. Configuratie van beleid voor de levering omvat: 

    - levering-protocol (HLS), 
    - het type dynamische codering (algemene CBC codering) 
    - de URL van licentie. 
    
    >[AZURE.NOTE]Als u leveren een stroom die zijn gecodeerd door FairPlay + een ander DRM wilt, moet u afzonderlijke levering beleid configureren 
    >
    >- IAssetDeliveryPolicy één streepje configureren met CENC (PlayReady + WideVine) en vloeiend met PlayReady. 
    >- Een andere IAssetDeliveryPolicy FairPlay configureren voor HLS

1. Een OnDemand locator om een streaming URL maken.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>FairPlay key levering door apps player-client gebruikt

Klanten kunnen ontwikkelen player apps iOS SDK gebruiken. Klanten moeten licenties exchange protocol implementeren te kunnen FairPlay inhoud af te spelen. De licentie voor exchange-protocol is niet opgegeven door Apple. Het is de taak elke app het verzenden van belangrijke levering aanvragen. De belangrijkste leveringsservices AMS FairPlay verwacht dat de SPC komen als www-form-url post gecodeerd bericht in de volgende vorm: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player ondersteunt geen FairPlay afspelen uit de doos. Klanten moeten de monster-speler van Apple developer account voor FairPlay afspelen op MAC OSX te verkrijgen. 
 
##<a name="streaming-urls"></a>Streaming URL 's

Als het activum is gecodeerd met meer dan één DRM, moet u een tag codering in de streaming URL: (format = 'm3u8-aapl' codering = 'xxx').

De volgende overwegingen zijn van toepassing:

- Alleen nul of een coderingstype dat kan worden opgegeven.
- Coderingstype hoeft niet te worden opgegeven in de url als er slechts één codering is toegepast op het activum.
- Coderingstype is niet hoofdlettergevoelig.
- De volgende coderingstypen u kunt opgeven:  
    - **cenc**: gemeenschappelijke codering (Playready of Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: envelop AES-codering.


##<a name="net-example"></a>.NET-voorbeeld


In het volgende voorbeeld ziet u de functionaliteit die is geïntroduceerd in Azure Media Services SDK voor .net-versie 3.6.0 (de mogelijkheid om Azure Media Services gebruiken om uw inhoud is gecodeerd met de FairPlay). De volgende Nuget package opdracht is gebruikt om het pakket te installeren:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Maak een Console-project.
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
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Mediaservices leren werken met paden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
