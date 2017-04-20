<properties 
    pageTitle="Filters en dynamische manifesten | Microsoft Azure" 
    description="In dit onderwerp wordt beschreven hoe om filters te maken zodat de client om bepaalde delen van de stream van een stream gebruiken kan. Dynamische manifesten te archiveren deze selectieve streaming maken Media Services" 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten

Media Services kunt vanaf versie 2.11, u filters voor de activa definiëren. Deze filters worden serverregels waardoor uw klanten kiezen voor dingen zoals: afspelen slechts een gedeelte van een video (in plaats van de hele video afspelen), of alleen een subset van vertoningen van audio en video-apparaat van uw klant kan verwerken (in plaats van alle vertoningen die gekoppeld aan het activum zijn) opgeven. Met deze filterfunctie van uw activa wordt via **Dynamische Manifest**s die zijn gemaakt op verzoek van de klant voor het stroomsgewijs verzenden van een video op basis van een opgegeven filter gearchiveerd.

Deze onderwerpen worden besproken veelvoorkomende scenario's beschreven waarin zeer voordelig voor uw klanten en koppelingen naar onderwerpen waarin wordt getoond hoe u via programmacode filters maken met behulp van filters zou zijn (op dit moment kunt u filters maken met REST API's alleen).

##<a name="overview"></a>Overzicht

Uw inhoud leveren aan klanten (live gebeurtenissen of video-on-demand streaming) is uw doel een video van hoge kwaliteit leveren aan verschillende apparaten onder verschillende omstandigheden. Deze doelstelling als volgt bereiken:

- coderen van uw stream op multi-([adaptieve bitsnelheid](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) video bitsnelheid (dit wordt gedaan van de kwaliteit en het netwerk) en 
- Gebruik Media Services [Dynamische verpakking](media-services-dynamic-packaging-overview.md) om dynamisch opnieuw uw stream naar verschillende protocollen (dit wordt gedaan op verschillende apparaten streaming). Media Services biedt ondersteuning voor de levering van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders). 

###<a name="manifest-files"></a>Manifest-bestanden 

Wanneer u een activum voor het streamen van adaptieve bitsnelheid codeert, wordt een **manifest** (afspeellijst)-bestand gemaakt (het bestand is op basis van tekst of XML). **Het manifestbestand** bevat metagegevens, zoals streaming: type (audio, video of tekst) bijhouden, bijhouden van naam, begin-en eindtijd, bitsnelheid (kwaliteiten), spoor-talen presentatievenster (schuifvenster bij met vaste duur), video-codec (FourCC). Ook worden via Windows media player het volgende fragment ophalen door het verstrekken van informatie over de volgende afspeelbare video fragmenten beschikbaar en hun locatie. Fragmenten (of segmenten) zijn de werkelijke 'chunks' van een video-inhoud.


Hier volgt een voorbeeld van een manifest bestand: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dynamische manifesten

Er zijn [scenario's](media-services-dynamic-manifest-overview.md#scenarios) wanneer de client meer flexibiliteit moet dan wat wordt beschreven in het manifestbestand van het standaard actief. Bijvoorbeeld:

- Specifieke apparaat: leveren alleen de opgegeven vertoningen en/of opgegeven taal nummers die worden ondersteund door het apparaat dat is gebruikt voor het afspelen van de inhoud ("vertoning filteren"). 
- Het manifest, zodat onderliggende clips van een live gebeurtenis ('onderliggende clip filteren") verminderen.
- Snijd het begin van een video ("een video bijsnijden").
- Presentatie-venster (DVR) aanpassen om een beperkte lengte van de DVR-venster in Windows media player ("aanpassen presentatievenster").
 
U bereikt deze flexibiliteit door biedt Media Services **Dynamische manifesteert** op basis van vooraf gedefinieerde [filters](media-services-dynamic-manifest-overview.md#filters).  Zodra u de filters definiëren, kunnen uw clients gebruiken voor het stroomsgewijs verzenden van een specifieke vertoning of onderliggende clips van uw video. Ze zou in de streaming URL filter (s) opgeven. Filters kunnen worden toegepast op de bitsnelheid van adaptieve streaming protocollen die worden ondersteund door [Dynamische verpakking](media-services-dynamic-packaging-overview.md): HLS, MPEG-streepje goede Streaming en harde schijven. Bijvoorbeeld:

MPEG-streepje URL met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Goede Streaming URL met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Zie [inhoud leveren-overzicht](media-services-deliver-content-overview.md)voor meer informatie over hoe u uw inhoud leveren en bouwen van streaming URL's.


>[AZURE.NOTE]Houd er rekening mee dat dynamische manifesteert Wijzig niet de activa en de standaard-manifest voor het desbetreffende activum. De client kunt voor het aanvragen van een stream met of zonder filters kiezen. 


###<a id="filters"></a>Filters 

Er zijn twee soorten filters actief: 

- Globale filters (kan worden toegepast op alle activa in de Azure Media Services-account, een levensduur van de rekening) en 
- Lokale filters (kan alleen worden toegepast op waaraan het filter gekoppeld tijdens het maken van is activa, een levensduur van de activa). 

Globale en lokale filtertypen hebben dezelfde eigenschappen. Het belangrijkste verschil tussen de twee is voor welke scenario's voor welk type een filer meer geschikt. Globale filters zijn over het algemeen geschikt voor apparaatprofielen (vertoning filteren) waar lokale filters voor het bijsnijden van een bepaald activum kunnen worden gebruikt.


##<a id="scenarios"></a>Gebruikelijke scenario 's 

Zoals werd vermeld voordat uw inhoud leveren aan klanten (live gebeurtenissen of video-on-demand streaming) uw doel is een video van hoge kwaliteit leveren aan verschillende apparaten onder verschillende omstandigheden. Daarnaast is het mogelijk andere eisen met betrekking tot de filtering van de activa en het gebruik van **Dynamische Manifest**s hebben. In de volgende secties geven een beknopt overzicht van de verschillende scenario's voor filteren.

- Geef alleen een subset van audio en video vertoningen dat bepaalde apparaten kunnen worden verwerkt (in plaats van alle vertoningen die gekoppeld aan de activa zijn). 
- Speel alleen een gedeelte van een video (in plaats van de hele video afspelen).
- Het presentatievenster DVR aanpassen.

##<a name="rendition-filtering"></a>Weergave filteren 

U kunt kiezen voor het coderen van uw activa met meerdere Coderingsprofielen (H.264 basislijn, hoge H.264, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteit bitsnelheid. Niet alle clientapparaten ondersteunen echter de profielen en de bitsnelheid van het activum. Oudere Android apparaten ondersteunt bijvoorbeeld alleen H.264 basislijn + AACL. Leiden hogere bitsnelheden tot verzendt naar een apparaat dat de voordelen niet kan ophalen, afval van bandbreedte- en apparaat-berekening. Dit apparaat moet alle bepaalde informatie, alleen in te perken voor weergave decoderen.

U kunt apparaatprofielen maken met dynamische Manifest zoals mobile, console HD/SD, enz., en omvatten de tracks en kwaliteiten die u wilt worden een deel van elk profiel.

 
![Voorbeeld van de weergave filteren][renditions2]

In het volgende voorbeeld is een coderingsprogramma gebruikt voor het coderen van een activum mezzanine in zeven ISO MP4s video vertoningen (van 180p tot 1080p). De gecodeerde activa kan dynamisch worden verpakt in een van de volgende streaming protocollen: HLS, vloeiend, MPEG-streepje en harde schijven.  Aan de bovenkant van het diagram de HLS-manifest voor de activa zonder filters weergegeven (bevat alle zeven vertoningen).  In de onderste links, wordt de HLS manifest waarmee een filter met de naam "ott" is vereffend weergegeven. Het filter 'ott' geeft aan dat alle bitsnelheid onder 1 Mbps heeft geresulteerd in de onderste twee kwaliteitsniveaus worden verwijderd uit de reactie verwijderen.  Rechts onderaan wordt het HLS manifest waarop een filter met de naam 'mobiele' werd toegepast weergegeven. Het filter 'mobiele' geeft aan waar de resolutie groter is dan 720p, hetgeen in de twee resulteerde vertoningen verwijderen 1080p vertoningen worden verwijderd uit.

![Weergave filteren][renditions1]

##<a name="removing-language-tracks"></a>Taal verwijderen

Uw activa kunnen bevatten meerdere audio-talen zoals Engels, Spaans, Frans, enz. Normaal gesproken de managers van Windows Media Player SDK standaard audio track selectie en audiotracks per gebruikersselectie beschikbaar. Uitdaging voor de ontwikkeling van dergelijke Player SDK's, verschillende implementaties over apparaat-specifieke player-frameworks is vereist. Ook sommige platforms, Player API's zijn beperkt en bevatten geen audio selectie functie waarbij gebruikers niet selecteren of wijzigen van de standaard audio-track. Met filters voor activa, kunt u het gedrag bepalen door het maken van filters die alleen de gewenste gesproken talen bevatten.

![Taalsporen filteren][language_filter]


##<a name="trimming-start-of-an-asset"></a>Begin van een activum bijsnijden 

In de meeste live streaming evenementen uitvoeren exploitanten bepaalde tests voordat de werkelijke gebeurtenis. Ze kunnen bijvoorbeeld een slate als vóór het begin van de gebeurtenis: 'Programma begint tijdelijk'. Als het archiveren van het programma, de test en slate gegevens ook worden gearchiveerd en wordt opgenomen in de presentatie. Deze informatie moet niet worden weergegeven aan de clients. U kunt een filter start tijd maken en de ongewenste gegevens verwijderen uit het manifest met dynamische Manifest.

![Start bijsnijden][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Onderliggende clips (weergaven) maken van een live-archief

Veel live gebeurtenissen zijn langlopende en live archief meerdere gebeurtenissen kan bevatten. Na de gebeurtenis live kunt uiteinden omroeporganisaties opsplitsen in de live archive in logische programma start en stop reeksen. Vervolgens deze virtuele programma's afzonderlijk publiceren zonder post verwerking van de live archief en geen afzonderlijke activa (die krijgt geen voordeel van de bestaande fragmenten in de cache in het CDN) te maken. Voorbeelden van dergelijke virtuele programma (onderliggende clips) zijn de kwartalen van een voetbal of basketbal spel, de innings in honkbal of afzonderlijke gebeurtenissen van een middag van het programma van de Olympische spelen.

U kunt met dynamische Manifest maken van filters met de begin-/ eindtijden en virtuele weergaven maken over de bovenkant van de live archief. 

![Subclip filter][subclip_filter]

Gefilterde actief:

![Skiën][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Aanpassen van het presentatievenster (DVR)

Azure Media Services biedt momenteel ronde archief waar de duur tussen 5 minuten - 25 uur kan worden geconfigureerd. Manifest filtering kan worden gebruikt voor het maken van een rolling DVR venster over de bovenkant van het archief, zonder media verwijderen. Er zijn veel scenario's waarbij omroepen bieden een beperkte DVR venster welke verplaatst met de live rand en tegelijkertijd een groter venster archivering houden. Een omroeporganisatie kunt gebruiken de gegevens die buiten het venster DVR clips markeren of he\she wilt bieden verschillende DVR vensters voor verschillende apparaten. De meeste mobiele apparaten verwerken niet zo groot DVR windows (u kunt een venster van de DVR 2 minuut voor mobiele apparaten en 1 uur voor desktop-clients hebben).

![DVR-venster][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>LiveBackoff (live-positie) aanpassen

Manifest filtering kan worden gebruikt voor het verwijderen van enkele seconden van de levende rand van een live programma. Hiermee kunt televisie-omroeporganisaties de presentatie op het punt van de publicatie voorbeeld bekijken en reclame maken punten plaatsingskosten voordat de viewers de stroom ontvangt (meestal back-off met 30 seconden). Omroepen kunnen vervolgens duwen deze advertenties op hun client frameworks in tijd te ontvangen en verwerken van gegevens voordat de verkoopkans advertentie.

Afgezien van de advertentie-ondersteuning kan LiveBackoff worden gebruikt voor client downloaden live positie zodanig aangepast dat wanneer clients drift en klik op de rand van de levende krijgen ze toch nog fragmenten van de server in plaats van een 404- of 412 HTTP-fouten.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Combineren van meerdere regels in een enkel filter

U kunt meerdere regels voor filteren in een enkel filter combineren. Als voorbeeld kunt u een regel bereik slate verwijderen uit een levend archief en beschikbare bitsnelheid ook filteren. Het eindresultaat is voor meerdere regels voor het filteren van de samenstelling (alleen doorsnede) van deze regels.

![meerdere regels][multiple-rules]

##<a name="create-filters-programmatically"></a>Filters maken via programmacode

In dit onderwerp wordt beschreven Media Services entiteiten die zijn gerelateerd aan filters. Ook ziet het onderwerp u hoe programmacode om filters te maken.  

[Filters maken met REST API's](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Meerdere filters (filter compositie) combineren

U kunt ook meerdere filters in één URL combineren. 

In het volgende scenario laat zien waarom u kunt filters combineren:

1. U moet uw video kwaliteiten voor mobiele apparaten, zoals Android of iPAD filteren (om video kwaliteiten beperken). Als u wilt verwijderen van de ongewenste kwaliteiten, maakt u een globaal filter die geschikt voor de apparaatprofielen is. Zoals eerder vermeld, kunnen de globale filters worden gebruikt voor alle activa onder de account met dezelfde media services zonder verdere associatie. 
2. Wilt u ook trim de begin- en tijd van een activum. Hiervoor zou u een lokaal filter maakt en de begin-/ eindtijd instellen. 
3. U wilt combineren van deze filters (zonder de combinatie moet u kwaliteit filters toevoegen aan het filter bijsnijden, waardoor het gebruik van het filter lastig).

U kunt filters combineren, moet u de filternamen ingesteld op het manifest/afspeellijst URL met door puntkomma's gescheiden. Eens dat u hebt een filter met de naam *MyMobileDevice* dat filters kwaliteiten en u een andere naam *MyStartTime hebt* voor het instellen van een bepaalde begintijd. U kunt deze combineren als volgt:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

U kunt maximaal 3 filters combineren. 

Zie [deze](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog voor meer informatie.


##<a name="know-issues-and-limitations"></a>Problemen en beperkingen bekend

- Dynamische manifest in de GOP werkt grenzen (sleutel Frames) dus bijsnijden GOP nauwkeurigheid heeft. 
- U kunt dezelfde naam van het filter voor lokale en globale filters gebruiken. Houd er rekening mee dat lokale filter hebben een hogere prioriteit en overschrijft de globale filters.
- Als u een filter hebt bijgewerkt, kan het voor streaming eindpunt voor het vernieuwen van de regels tot 2 minuten duren. Als de inhoud is betekend of gebruik van bepaalde filters (en in het cachegeheugen van proxy's en CDN caches), werken deze filters kan leiden tot storingen player. Het is aanbevolen om de cache leeg na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter.


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Zie ook

[Inhoud leveren aan klanten-overzicht](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 