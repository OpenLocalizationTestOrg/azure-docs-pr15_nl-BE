<properties 
    pageTitle="Activa levering beleid configureren met de SDK voor .NET | Microsoft Azure" 
    description="Dit onderwerp bevat een ander activum levering beleid configureren met Azure Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>Levering van activa met .NET SDK configureren
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Overzicht

Als u van plan activa levering gecodeerd bent, wordt een van de stappen in de workflow van Media Services leveren van inhoud levering beleid voor activa configureren. De levering actief beleid Media Services vertelt hoe u het activum wilt worden geleverd: in welke streaming protocol moet uw activa worden dynamisch verpakt (voor bijvoorbeeld MPEG-streepje, HLS, soepele Streaming, of alle), al dan niet u wilt coderen dynamisch uw activa en hoe (envelop of gemeenschappelijke codering).

Dit onderwerp wordt uitgelegd waarom en hoe u kunt maken en configureren van de levering van activa.

>[AZURE.NOTE]Als u dynamische verpakking en dynamische codering gebruikt, moet u ervoor zorgen dat ten minste één schaaleenheid (ook bekend als streaming eenheid). Zie [voor het schalen van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
>
>Het activum moet ook een verzameling adaptieve bitsnelheid MP4s of adaptieve bitsnelheid goede Streaming-bestanden bevatten.

U kunt verschillende soorten beleid toepassen aan dezelfde activa. U kan bijvoorbeeld PlayReady codering toepassen op goede Streaming en envelop AES-codering MPEG-streepje en HLS. Alle protocollen die niet zijn gedefinieerd in het beleid voor een levering (bijvoorbeeld u één beleid toevoegen die alleen HLS als het protocol) van streaming worden geblokkeerd. De uitzondering hierop is als u geen activa levering beleid helemaal gedefinieerd. Vervolgens worden alle protocollen kunnen in de open.

Als u bieden een versleutelde activabeheer voor opslag wilt, moet u beleid voor levering van het actief. Voordat uw activa kan worden gestreamd, de server voor mediagegevensstromen verwijdert u de codering voor de opslag en het stroomsgewijs inhoud met het beleid voor levering opgegeven. Bijvoorbeeld, als u wilt ervoor zorgen dat uw activa die zijn gecodeerd met een coderingssleutel voor envelop Advanced Encryption Standard (AES), het beleidstype ingesteld op **DynamicEnvelopeEncryption**. Opslag-codering verwijderen en het actief in de stream, stelt u het type op **NoDynamicEncryption**. Voorbeelden van het configureren van deze beleidstypen volgen.

Afhankelijk van hoe u de levering actief beleid configureert u zou kunnen dynamisch inpakken, dynamisch coderen en stroomsgewijs verzenden van de volgende streaming protocollen: streams goede Streaming, HLS MPEG-streepje en harde schijven.

De volgende lijst bevat de indelingen gebruiken om te streamen vloeiend, HLS, STREEPJES en harde schijven.

Goede Streaming:

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-STREEPJE

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HARDE SCHIJVEN

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Zie [een streaming URL maken](media-services-deliver-streaming-content.md)voor instructies over het publiceren van een actief en een streaming URL maken.

##<a name="considerations"></a>Overwegingen met betrekking tot

- U kunt een AssetDeliveryPolicy die is gekoppeld aan een actief, terwijl een locator OnDemand (streaming) voor de activa bestaat niet verwijderen. De aanbeveling is het beleid van het activum te verwijderen voordat u het beleid verwijdert.
- Een streaming locator kan niet worden gemaakt op een opslag gecodeerde actief wanneer er geen levering actief beleid is ingesteld.  Als het activum niet gecodeerd opslag, het systeem, kunt u een locator maken en de activa in de wissen zonder een beleid actief levering streamen.
- Hebt u meerdere actief beleid voor de levering die is gekoppeld aan een enkel actief, maar u kunt slechts één manier voor het verwerken van een bepaalde AssetDeliveryProtocol opgeven.  Wat betekent dat als u probeert te koppelen van twee beleidsregels voor levering die het AssetDeliveryProtocol.SmoothStreaming-protocol die in een fout resulteren omdat het systeem niet zeker dat u wilt toepassen weet wanneer een client een aanvraag voor een soepele Streaming opgeven.
- Als u een actief met een bestaande streaming locator, kunt u een nieuw beleid niet koppelen aan het activum (u kunt ontkoppelen van een bestaand beleid uit het actief, of bijwerken van een beleid voor levering die aan het activum is gekoppeld).  U moet eerst de streaming-locator verwijderen, aanpassen van het beleid en de streaming-locator opnieuw maken.  U kunt de dezelfde locatorId wanneer u opnieuw de streaming-locator, maar u ervoor dat niet toe leiden dat problemen voor clients zorgen moet aangezien inhoud kan worden opgeslagen in de oorsprong of een stroomafwaartse CDN.


##<a name="clear-asset-delivery-policy"></a>Beleid voor levering activa wissen

De volgende **ConfigureClearAssetDeliveryPolicy** methode bevat dynamische codering niet van toepassing en ervoor zorgen dat de stroom in een van de volgende protocollen: MPEG-streepje HLS en goede Streaming-protocollen. U kunt dit beleid van toepassing op de activa van de opslag versleuteld.

Voor meer informatie over welke waarden u kunt opgeven bij het maken van een AssetDeliveryPolicy, Zie de sectie [bij het definiëren van AssetDeliveryPolicy gebruikt](#types) .

statische public void ConfigureClearAssetDeliveryPolicy (asset IAsset) {IAssetDeliveryPolicy beleid = _context. AssetDeliveryPolicies.Create ("Duidelijk beleid", AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

activa. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption activa levering beleid


De volgende **CreateAssetDeliveryPolicy** -methode maakt **AssetDeliveryPolicy** die is geconfigureerd voor toepassen dynamische gemeenschappelijke codering (**DynamicCommonEncryption**) naar een goede streaming protocol (andere protocollen worden geblokkeerd van streaming). De methode heeft twee parameters: **actief** (de activa die u wilt toepassen, het beleid voor levering) en **IContentKey** (de sleutel van het type **CommonEncryption** voor meer informatie Zie: [maken van een sleutel](media-services-dotnet-create-contentkey.md#common_contentkey)).

Voor meer informatie over welke waarden u kunt opgeven bij het maken van een AssetDeliveryPolicy, Zie de sectie [bij het definiëren van AssetDeliveryPolicy gebruikt](#types) .


statische public void CreateAssetDeliveryPolicy (IAsset activa, de sleutel IContentKey) {Uri acquisitionUrl = sleutel. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

AssetDeliveryPolicyConfiguration woordenlijst < AssetDeliveryPolicyConfigurationKey, string > nieuwe woordenlijst < AssetDeliveryPolicyConfigurationKey string > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services kunt u codering Widevine toevoegen. In het volgende voorbeeld ziet u zowel PlayReady als Widevine wordt toegevoegd aan het beleid van de levering activa.


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
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Bij het coderen met Widevine, zou het alleen mogelijk te leveren met streepje. Zorg ervoor dat streepje in het protocol van de levering activum opgeven.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption activa levering beleid 

De volgende **CreateAssetDeliveryPolicy** -methode maakt de **AssetDeliveryPolicy** die is geconfigureerd voor het dynamisch voertuigprofiel codering (**DynamicEnvelopeEncryption**) van toepassing op goede Streaming HLS en -streepje protocollen (als u niet geven sommige protocollen, ze worden geblokkeerd van streaming). De methode heeft twee parameters: **actief** (de activa die u wilt toepassen, het beleid voor levering) en **IContentKey** (de sleutel van het type **EnvelopeEncryption** voor meer informatie Zie: [maken van een sleutel](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Voor meer informatie over welke waarden u kunt opgeven bij het maken van een AssetDeliveryPolicy, Zie de sectie [bij het definiëren van AssetDeliveryPolicy gebruikt](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
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
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Die worden gebruikt bij het definiëren van AssetDeliveryPolicy

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


