<properties
    pageTitle="Inhoud leveren aan klanten | Microsoft Azure"
    description="Dit onderwerp geeft een overzicht van wat is betrokken bij het leveren van uw inhoud met Azure Media Services."
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="deliver-content-to-customers"></a>Inhoud leveren aan klanten
U bent uw streaming of video-on-demand inhoud leveren aan klanten, is het doel video van hoge kwaliteit leveren aan verschillende apparaten onder verschillende omstandigheden.

Om dit doel te bereiken, kunt u het volgende doen:

- Codeer uw stream naar een videostream multi-bitrate (adaptieve bitsnelheid). Dit zal zorgen voor kwaliteit en de netwerkomstandigheden.
- Gebruik Microsoft Azure Media Services [dynamische verpakking](media-services-dynamic-packaging-overview.md) om dynamisch opnieuw uw stream naar verschillende protocollen. Dit zal zorgen voor streaming op verschillende apparaten. Media Services biedt ondersteuning voor de levering van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

Dit artikel geeft een overzicht van belangrijke inhoud levering concepten.

Bekende problemen Zie [problemen](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamische verpakking

Met de dynamische Media Services biedt verpakking kunt u de inhoud van uw adaptieve bitsnelheid MP4 of goede Streaming gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) zonder het pakket opnieuw opslaan in deze indelingen streamen streaming leveren. Wij raden u aan uw inhoud met dynamische verpakking levert.

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Het bestand met mezzanine (bron) in een reeks geavanceerde bitsnelheid MP4-bestanden of adaptieve bitsnelheid goede Streaming-bestanden coderen.
- Streaming eenheid van ten minste één-op-verzoek voor het streaming eindpunt waaruit u wilt ervoor zorgen dat uw inhoud ophalen. Zie [schalen streaming eenheden gereserveerd op verzoek](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Met dynamische verpakking, opslaan en betalen voor de bestanden in één opslagmedium. Media Services zal bouwen en dienen de juiste reactie op basis van de aanvragen.

Naast toegang tot dynamische verpakking mogelijkheden biedt voorzien streaming eenheden gereserveerd op verzoek u van speciale egress-capaciteit die kan worden gekocht in stappen van 200 Mbps. Standaard streaming op verzoek geconfigureerd in een gedeeld exemplaar model voor welke server resources (bijvoorbeeld compute of egress capaciteit) met alle andere gebruikers worden gedeeld. Door de aanschaf van streaming gereserveerde eenheden op verzoek kunt u een-op-verzoek streaming doorvoer verbeteren.

Zie [dynamische verpakking](media-services-dynamic-packaging-overview.md)voor meer informatie.

## <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten

Voor de activa met Media-Services kunt u filters definiëren. Deze filters worden serverregels waarmee uw klanten acties als het afspelen van een bepaald gedeelte van een video of een subset van vertoningen van audio en video-apparaat van uw klant kan verwerken (in plaats van alle vertoningen die gekoppeld aan het activum zijn) opgeven. Dit filter wordt bereikt door middel van *dynamische manifesten* die worden gemaakt wanneer uw klant wil een videogegevensstroom op basis van een of meer opgegeven filters.

Zie voor meer informatie, [Filters en dynamische manifesten](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locators

Om de gebruiker met een URL die kan worden gebruikt om te streamen of te downloaden van uw inhoud, moet u eerst uw activa publiceren door het maken van een locator. Een locator biedt een ingangspunt voor toegang tot de bestanden in een actief. Media Services ondersteunt twee typen locators:

- OnDemandOrigin-locators. Deze worden gebruikt om media te streamen (bijvoorbeeld MPEG-streepje, HLS of goede Streaming) of progressief downloaden van bestanden.
-  Gedeelde toegang handtekening (SAS) URL-locators. Deze worden gebruikt voor het downloaden van media-bestanden op uw lokale computer.

Een *-beleid* wordt gebruikt voor het definiëren van de machtigingen (zoals lezen, schrijven en lijst) en duur waartoe een client toegang voor een bepaald activum heeft. Opmerking de lijst met machtigingen (AccessPermissions.List) mag niet worden gebruikt bij het maken van een OrDemandOrigin-locator.

Locators hebben verloopdatum. De Azure portal stelt een verloopdatum in de toekomst 100 jaar voor locators.

>[AZURE.NOTE] Als u de portal Azure locators vóór maart 2015 gemaakt, zijn deze locators verloopt na twee jaar ingesteld.

Als u wilt bijwerken met een vervaldatum op een locator [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API's te gebruiken. Houd er rekening mee dat als u de vervaldatum van een SAS-locator, de URL is gewijzigd.

Locators zijn niet ontworpen voor het beheren van toegangsbeheer op gebruikersniveau. U kunt verschillende toegangsrechten voor afzonderlijke gebruikers met behulp van Digital Rights Management (DRM) oplossingen geven. Zie [Media beveiligen](http://msdn.microsoft.com/library/azure/dn282272.aspx)voor meer informatie.

Wanneer u een locator maakt, kan er een vertraging van 30 seconden vereist, opslag en het doorgeven van de processen in Azure opslag.


## <a name="adaptive-streaming"></a>Adaptive streaming

Bitsnelheid adaptieve technologieën kunnen videospeler toepassingen netwerkomstandigheden bepalen en selecteer uit diverse bitsnelheid. Wanneer netwerkcommunicatie vermindert, selecteren de client een lagere bitsnelheid zodat afspelen met een lagere beeldkwaliteit kan worden voortgezet. Als het netwerk overbelast, moet de client kunt overschakelen naar een hogere bitsnelheid met verbeterde videokwaliteit. Azure Media Services ondersteunt de volgende geavanceerde bitsnelheid technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven.

Om gebruikers met streaming URL's, moet u eerst een locator OnDemandOrigin maken. Maken van de locator kunt u het basispad naar het activum met de inhoud die u wilt streamen. Als u voor het stroomsgewijs verzenden van deze inhoud, moet u echter verder dit pad te wijzigen. Als u wilt een volledige URL aan het manifestbestand streaming samenstellen, samenvoegen padwaarde van de locator en het manifest (filename.ism) de naam van bestand. Vervolgens **toevoegen/Manifest** en de juiste indeling (indien nodig) het pad locator.

>[AZURE.NOTE]U kunt ook uw inhoud stroomsgewijs via een SSL-verbinding. Hiervoor moet dat uw streaming URL's beginnen met HTTPS.

U kunt alleen streamen via SSL als het streaming eindpunt van waaruit u de inhoud bezorgen na 10 September 2014 is gemaakt. Als uw streaming URL's zijn gebaseerd op de streaming eindpunten gemaakt na 10 September 2014, bevat de URL 'streaming.mediaservices.windows.net'. Streaming URL's met 'origin.mediaservices.windows.net' (oude indeling) bieden geen ondersteuning voor SSL. Als uw URL in de oude indeling is en u wilt streamen via SSL, maakt u een nieuwe streaming eindpunt. Op basis van het nieuwe endpoint streaming URL's gebruiken voor het stroomsgewijs verzenden van inhoud via SSL.


## <a name="streaming-url-formats"></a>Streaming-indelingen, URL

### <a name="mpeg-dash-format"></a>De indeling MPEG-streep

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-time-csf)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) past V4-indeling

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3-indeling

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS) past opmaak met alleen audio filter

Standaard alleen audio nummers zijn opgenomen in de HLS manifest. Dit is vereist voor de certificering Apple Store voor netwerken voor mobiele telefonie. In dit geval als een client niet over voldoende bandbreedte beschikt of is verbonden via een 2G verbinding, afspelen schakelt over naar alleen audio. Hiermee kunt u inhoud stroomsgewijs zonder bufferen behouden, maar er is geen video. In sommige gevallen buffering player mogelijk voorkeur boven alleen audio. Als u verwijderen van het spoor alleen audio wilt, toevoegen **alleen audio = false** naar de URL.

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3,audio-Only=False)

Zie [ondersteuning voor dynamische Manifest van de samenstelling en HLS extra functies uitvoeren](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)voor meer informatie.


### <a name="smooth-streaming-format"></a>Goede Streaming-indeling

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Voorbeeld:

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Goede Streaming 2.0 manifest (oude manifest)

Goede Streaming manifest format bevat de herhaalsnelheid tag (r-tag). Sommige spelers ondersteunen echter niet de r-code. Clients met deze spelers kunnen een notatie gebruikt die de r-code uitgeschakeld:

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>Harde schijven (voor Adobe WIP/Access licentiehouders)

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Progressief downloaden

Met progressieve download kunt u starten met het afspelen van media voordat het hele bestand is gedownload. U kan geleidelijk .ism * (isma ismv ismt of ismc) de bestanden downloaden.

Geleidelijk om inhoud te downloaden, gebruikt u het OnDemandOrigin type locator. In het volgende voorbeeld ziet u de URL die is gebaseerd op het type OnDemandOrigin locator:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

U moet de opslag versleuteld elementen die u wilt streamen vanaf de oorsprong van de service voor progressieve download decoderen.

## <a name="download"></a>Downloaden

Als u wilt uw inhoud downloaden naar een clientapparaat, moet u een SAS-Locator. De SAS-locator krijgt u toegang tot de Azure opslag container waar uw bestand zich bevindt. Als u de download-URL, hebt u tussen de host en SAS handtekening de naam van het bestand insluiten.

In het volgende voorbeeld ziet u de URL die is gebaseerd op de SAS-locator:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

De volgende overwegingen zijn van toepassing:

- U moet de opslag versleuteld elementen die u wilt streamen vanaf de oorsprong van de service voor progressieve download decoderen.
- Een download die niet binnen 12 uur heeft voltooid zal mislukken.

## <a name="streaming-endpoints"></a>Streaming eindpunten

Een streaming eindpunt vertegenwoordigt een streaming service die inhoud rechtstreeks naar een clienttoepassing player of naar een content delivery network (CDN) voor verdere distributie leveren. De uitgaande stream van een streaming service endpoint is een live gegevensstroom of een video-on-demand actief in uw Media Services-account. Verder kunt u de capaciteit van de streaming service endpoint voor het verwerken van de groeiende vraag naar bandbreedte door streaming gereserveerde eenheden aan te passen. U moet ten minste één gereserveerde eenheid toewijzen voor toepassingen in een productieomgeving. Zie [voor het schalen van een media-service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

## <a name="known-issues"></a>Bekende problemen

### <a name="changes-to-smooth-streaming-manifest-version"></a>Wijzigingen in goede Streaming manifest versie

Vóór de release van juli 2016 service--wanneer activa geproduceerd door Media Encoder standaard Media Encoder Premium Workflow of de eerdere Azure Media Encoder zijn gestreamd met behulp van dynamische verpakking--goede Streaming manifest geretourneerd zou voldoen aan versie 2.0. In versie 2.0, de duur van het fragment niet de zogenaamde herhalen (r)-tags gebruiken. Bijvoorbeeld:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

In de servicerelease juli 2016 voldoet het gegenereerde goede Streaming-manifest aan versie 2.2, met een duur van fragment herhalen tags gebruiken. Bijvoorbeeld:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Sommige van de oudere goede Streaming-clients ondersteunen niet de herhaalde labels en kan niet worden geladen, het manifest. Om te verhelpen dit probleem, kunt u de indelingsparameter van oudere manifest **(format = fmp4 v20)** of uw client bijwerken naar de nieuwste versie die herhaalde labels ondersteunt. Zie voor meer informatie, [goede Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen

[Locators Media Services bijwerken na het walsen opslag sleutels](media-services-roll-storage-access-keys.md)
