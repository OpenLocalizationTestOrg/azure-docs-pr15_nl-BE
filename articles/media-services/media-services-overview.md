<properties 
    pageTitle="Azure Media Services-overzicht en algemene scenario's | Microsoft Azure" 
    description="Dit onderwerp geeft een overzicht van Azure Media Services" 
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
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Azure Media Services-overzicht en algemene scenario 's

Microsoft Azure Media Services is een uitbreidbaar cloud-gebaseerde platform waarmee ontwikkelaars schaalbare media beheer- en -toepassingen kunnen maken. Media Services is gebaseerd op de REST API's waarmee u veilig uploaden, opslaan, coderen en verpakken van video- of audio-inhoud voor zowel op verzoek als live streaming levering aan verschillende clients (bijvoorbeeld TV, PC en mobiele apparaten).

End-to-end werkstromen met volledig Media Services kunt u bouwen. U kunt ook onderdelen van andere fabrikanten gebruiken voor bepaalde onderdelen van de werkstroom. Bijvoorbeeld, coderen met behulp van een derde partij-encoder. Vervolgens uploaden, beschermen, pakket, met behulp van Media Services leveren.

U kunt stroom uw inhoud live of inhoud leveren op aanvraag. Dit onderwerp bevat algemene scenario's voor het leveren van uw inhoud [live](media-services-overview.md#live_scenarios) of [op aanvraag](media-services-overview.md#vod_scenarios). Het onderwerp bevat ook koppelingen naar andere relevante onderwerpen.

## <a name="sdks-and-tools"></a>Hulpprogramma's en SDK 's

Media Services oplossingen bouwen, kunt u het volgende gebruiken:

- [Mediaservices REST API](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Een van de beschikbare client SDK's:
- [Azure mediaservices SDK voor .NET](https://github.com/Azure/azure-sdk-for-media-services)
- [Azure SDK voor Java](https://github.com/Azure/azure-sdk-for-java)
- [Azure PHP-SDK](https://github.com/Azure/azure-sdk-for-php)
- [Azure mediaservices voor Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Dit is een niet-Microsoft-versie van een Node.js SDK. Het wordt beheerd door een community en heeft nog geen een dekking van 100% van de APIs AMS).
- Bestaande hulpprogramma's:
- [Azure portal](https://portal.azure.com/)
- [Azure Media-Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is een Winforms / C#-toepassing voor Windows)

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

U kunt paden hier leren AMS weergeven:

- [AMS Live Streaming Workflow](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS op de vraag Workflow Streaming](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Vereisten

Azure Media Services starten, hebt u het volgende:

3. Een Azure-account. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com).
2. Azure Media Services-account. Gebruik de Azure portal, .NET of REST API Azure Media Services-account te maken. Zie [Account maken](media-services-portal-create-account.md)voor meer informatie.
3. (Optioneel) Ontwikkelomgeving instellen. .NET of REST API voor uw ontwikkelomgeving te kiezen. Voor meer informatie Zie [omgeving instellen](media-services-dotnet-how-to-use.md).

Ook informatie over hoe u verbinding maakt via een programma [verbinding maken](media-services-dotnet-connect-programmatically.md).
4. (Aanbevolen) Een of meer schaaleenheden toewijzen. Het verdient aanbeveling een of meer schaaleenheden toewijzen voor toepassingen in de productie-omgeving.   Voor meer informatie, Zie [beheren streaming eindpunten](media-services-portal-manage-streaming-endpoints.md).

##<a name="concepts-and-overview"></a>Overzicht van concepten en

Zie [begrippen](media-services-concepts.md)voor Azure Media Services-concepten.

Zie voor een reeks procedures die u maakt kennis met de belangrijkste onderdelen van Azure Media Services [Azure Media Services stapsgewijze zelfstudies](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Deze reeks heeft een uitgebreid overzicht van concepten en het hulpprogramma AMSE wordt gebruikt om aan te tonen van de taken van de AMS. Houd er rekening mee dat AMSE tool een Windows-hulpprogramma is. Dit hulpprogramma ondersteunt de meeste van de taken die u programmatisch met [AMS SDK voor .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK voor Java](https://github.com/Azure/azure-sdk-for-java)of [PHP-SDK Azure bereiken kunt](https://github.com/Azure/azure-sdk-for-php).

##<a id="vod_scenarios"></a>Media op verzoek met Azure Media Services leveren: algemene scenario's en taken

In deze sectie worden algemene scenario's beschreven en vindt u koppelingen naar relevante onderwerpen. Het volgende diagram toont de grote delen van het platform van Media Services die betrokken zijn bij het bezorgen van inhoud op aanvraag. 

![VoD-werkstroom](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Beveiliging van inhoud in de opslag en levering van streaming media in de (niet-gecodeerde)

1. Een hoge kwaliteit mezzanine-bestand uploaden naar een actief.
    
    Het is raadzaam opslag codering optie toepassen op uw activa ter bescherming van de inhoud tijdens het uploaden en op de rest in de opslag.
 
1. Op een set van adaptieve bitsnelheid MP4-bestanden te coderen. 

    Het is raadzaam de opslagoptie-codering toepassen op de uitvoer activa ter bescherming van uw inhoud in rust.
    
1. Activa levering beleid (gebruikt door dynamische verpakking) configureren. 
    
    Als het activum is opslag versleuteld, configureren u **moet** actief levering beleid. 

1. Het actief maken van een OnDemand locator publiceren.

    Zorg ervoor dat ten minste één gereserveerde eenheid op de streaming eindpunt waarvoor u inhoud wilt streamen.

1. Stream gepubliceerde inhoud.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Beveiliging van inhoud in de opslag, levering dynamisch gecodeerde streaming media  

U moet ten minste één eenheid van streaming gereserveerde eerst op de streaming eindpunt waaruit u gecodeerde inhoud stroomsgewijs wilt ophalen om te kunnen gebruikmaken van dynamische codering.

1. Een hoge kwaliteit mezzanine-bestand uploaden naar een actief. De optie codering opslag van toepassing op het activum.
1. Op een set van adaptieve bitsnelheid MP4-bestanden te coderen. De optie codering opslag aan de activa van de uitvoer van toepassing.
1. Inhoud coderingssleutel voor het activum dat u wilt dynamisch worden gecodeerd tijdens het afspelen van maken.
2. Inhoud sleutel autorisatiebeleid configureren.
1. Activa levering beleid (gebruikt door dynamische verpakking en dynamische codering) configureren.
1. Het actief maken van een OnDemand locator publiceren.
1. Stream gepubliceerde inhoud. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Analytics voor Media gebruiken om sneller inzicht afleiden van uw video 's 

Media-Analytics is een verzameling onderdelen voor spraakherkenning en visie die het eenvoudiger voor bedrijven en ondernemingen voor het afleiden van inzichten die hun video's sneller. Zie [Overzicht van Azure Media Services Analytics](media-services-analytics-overview.md)voor meer informatie.

1. Een hoge kwaliteit mezzanine-bestand uploaden naar een actief.
2. Een van de volgende Media-Analytics services gebruiken voor het verwerken van uw video's:
    
    - **Indexering** – [Process video's met Azure Media indexeerfunctie 2](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** – [Hyperlapse mediabestanden met Azure Media Hyperlapse](media-services-hyperlapse-content.md)
    - **Detectie van beweging** – [Beweging detectie voor Azure Media Analytics](media-services-motion-detection.md).
    - **Face detection en emoties gezicht** – [Face en detectie van Azure Media Analytics emoties](media-services-face-and-emotion-detection.md).
    - **Video samenvatting** : [Azure Media Video miniaturen gebruiken naar een Video-samenvatting maken](media-services-video-summarization.md)
3. Media-Analytics media processors produceren MP4 of JSON bestanden. Als een Mediaprocessor geproduceerd een MP4-bestand, kunt u het bestand progressief downloaden. Als een Mediaprocessor geproduceerd een JSON-bestand, kunt u het bestand kunt downloaden van de Azure blobopslag. 


###<a name="deliver-progressive-download"></a>Ervoor zorgen dat een progressief downloaden 

1. Een hoge kwaliteit mezzanine-bestand uploaden naar een actief.
1. Om een MP4 bestand te coderen.
1. Het actief maken van OnDemand- of SAS-locator publiceren.

    Als OnDemand locator, zorg ervoor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waarop u van plan bent inhoud progressief te downloaden.

    Als SAS-locator gebruikt, wordt de inhoud gedownload uit de Azure blobopslag. In dit geval nodig niet om streaming gereserveerde eenheden.
  
1. Progressief downloaden van inhoud.

##<a id="live_scenarios"></a>Live Streaming evenementen met Azure mediaservices leveren

Als u werkt met Live Streaming vaak de volgende onderdelen betrokken zijn:

- Een camera die wordt gebruikt voor het uitzenden van een gebeurtenis.
- Een live video encoder die worden omgezet in signalen van de camera streams die worden verzonden naar een live streaming service.

U kunt desgewenst meerdere levende tijd gesynchroniseerd encoders. Voor bepaalde essentiële live gebeurtenissen vraag zeer hoge beschikbaarheid en de kwaliteit van de ervaring, het verdient aanbeveling om redundante encoders actieve tijd synchronisatieAls bereiken naadloze failover zonder verlies van gegevens.
- Een live streaming-service waarmee u het volgende doen:

- consumptie van live inhoud met behulp van verschillende live streaming protocollen (bijvoorbeeld RTMP of goede Streaming)
- (optioneel) de stream te coderen in adaptieve bitsnelheid
- Voorbeeld van de live gegevensstroom
- opnemen en de aangezogen inhoud opslaan om later (Video-on-Demand) worden gestreamd
- de inhoud door middel van algemene streaming protocollen (bijvoorbeeld MPEG-streepje, vloeiend, HLS, harde schijven) leveren rechtstreeks aan uw klanten, of om een inhoud Delivery Network (CDN) voor verdere distributie.


**Azure Microsoft mediaservices** (AMS) biedt de mogelijkheid om te nemen, coderen, weergeven, opslaan en leveren uw live streaming inhoud.

Uw inhoud leveren aan klanten is uw doel een video van hoge kwaliteit leveren aan verschillende apparaten onder verschillende omstandigheden. Om te zorgen voor kwaliteit en de netwerkomstandigheden, live encoders te gebruiken om de stream op multi-bitrate (adaptief) video bitsnelheid.  Gebruiken om te zorgen voor streaming op verschillende apparaten, Media Services [dynamische verpakking](media-services-dynamic-packaging-overview.md) uw stream naar verschillende protocollen dynamisch opnieuw inpakken. Media Services biedt ondersteuning voor de levering van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

In Azure Media Services verwerken **kanalen**, **programma's**en **StreamingEndpoints** alle live streaming functies waaronder ingest, opmaak, DVR, beveiliging, schaalbaarheid en redundantie.

Een **kanaal** staat voor een pijpleiding voor het verwerken van live streaming inhoud. Een kanaal kan ontvangen een live streams op de volgende manieren invoeren:

- Een bedrijf op live encoder verzendt multi-bitrate **RTMP** of **Goede Streaming** (gefragmenteerde MP4) op het kanaal dat is geconfigureerd voor de levering van de **Pass Through-query** . De levering van de **Pass Through-query** is wanneer de aangezogen streams **kanaal**s passeren zonder verdere verwerking. U kunt de volgende live encoders die uitvoer met goede Streaming van multi-bitrate: vorm van het element, Envivio, Cisco.  De volgende live encoders uitvoer RTMP: Adobe Flash Live Telestream Wirecast en Tricaster transcoders.  Een live-encoder kan ook een enkele bitsnelheid verzenden naar een kanaal dat niet is ingeschakeld voor live-codering, maar die niet wordt aanbevolen. Bij een aanvraag voor levert Media Services de stroom aan klanten.

>[AZURE.NOTE] Een Pass Through-methode is de voordeligste manier om te live streaming wanneer u meerdere gebeurtenissen gedurende een lange periode van tijd doet, en u al hebt geïnvesteerd in encoders voor gebouwen. Zie details [prijzen](/pricing/details/media-services/) .

- Een live encoder op het bedrijf een enkele bitsnelheid verzendt naar het kanaal dat is ingeschakeld voor live-codering met Media-Services in een van de volgende indelingen: RTP (MPEG-TS), RTMP of goede Streaming (MP4 gefragmenteerd). Het kanaal voert live-codering van de binnenkomende één bitsnelheid op een multi-(adaptief) video bitsnelheid. Bij een aanvraag voor levert Media Services de stroom aan klanten.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Werken met de kanalen die multi-bitrate live gegevensstroom van encoders voor bedrijfsruimten (Pass Through-query ontvangen)

Het volgende diagram ziet de grote delen van het platform AMS die betrokken zijn in de workflow van de **Pass Through-query** .

![Live workflow][live-overview2]

Voor meer informatie, Zie [werken met kanalen die ontvangen Multi-Live bitsnelheid van On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Werken met de kanalen die zijn ingeschakeld voor live-codering met Azure Media Services

Het volgende diagram ziet de grote delen van het platform AMS die betrokken zijn in Live Streaming werkstroom waarin een kanaal voor het uitvoeren van live-codering met Media Services is ingeschakeld.

![Live workflow][live-overview1]

Zie [werken met kanalen, die voor het uitvoeren van Live-codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.


##<a name="consuming-content"></a>Die inhoudstypen

Azure Media Services biedt de hulpprogramma's die u nodig hebt voor het maken van krachtige, dynamische speler clienttoepassingen voor de meeste platforms, waaronder: iOS apparaten, Android-apparaten, Windows, Windows Phone, Xbox en Set-top-vakken. Het volgende onderwerp bevat links naar de SDK's en -Player kaders die u gebruiken kunt voor het ontwikkelen van uw eigen clienttoepassingen die streaming Media Services-media kunnen worden gebruikt.

[Video Player-toepassingen ontwikkelen](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Azure CDN inschakelen

Media Services ondersteunt integratie met Azure CDN. Zie [How to Streaming eindpunten op een rekening van Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie over het inschakelen van Azure CDN.

##<a name="scaling-a-media-services-account"></a>Schaal van een Media-Services-account

U kunt **Media Services** door te geven van het aantal **Gereserveerde eenheden Streaming** en **Codering gereserveerde eenheden** die u uw account dat wilt moet worden ingericht met schalen.

U kunt uw Media Services-account ook schalen opslag rekeningen op te tellen. Elke account opslag is beperkt tot 500 TB. Als u wilt uw opslag buiten de standaardbeperkingen uitgevouwen, kunt u meerdere opslag accounts koppelen aan één account Media Services.

[Dit](media-services-portal-scale-streaming-endpoints.md) onderwerp bevat koppelingen naar relevante onderwerpen.

##<a name="support"></a>Ondersteuning

[Azure Support](https://azure.microsoft.com/support/options/) biedt opties voor Azure, met inbegrip van de Media.

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Service Level Agreement (SLA)

- Voor het coderen van Media Services garanderen we 99,9% beschikbaarheid van transacties met REST API.
- Voor Streaming, zullen wij aanvragen met een garantie van 99,9% beschikbaarheid voor bestaande media-inhoud is service wanneer ten minste één eenheid Streaming wordt aangeschaft.
- Wij garanderen dat kanalen uitgevoerd externe connectiviteit ten minste 99,9% van de tijd hebben zal voor Live-kanalen.
- Wij garanderen dat we zullen met succes te voldoen aan de sleutel aanvragen ten minste 99,9% van de tijd voor Content Protection.
- Voor indexering, zullen we met succes indexeerfunctie verzoeken verwerkt met een codering gereserveerd service Unit 99,9% van de tijd.

Zie voor meer informatie [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
