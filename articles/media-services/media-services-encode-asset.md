<properties 
    pageTitle="Overzicht en vergelijking van Azure op vraag media encoders | Microsoft Azure" 
    description="Dit onderwerp geeft een overzicht en vergelijking van Azure op vraag media encoders." 
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
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Overzicht en vergelijking van Azure op vraag media encoders

##<a name="encoding-overview"></a>Codering, overzicht

Azure Media Services biedt meerdere opties voor de codering van de media in de cloud.

Bij het starten van Media Services, is het belangrijk dat u begrijpt het verschil tussen de codecs en bestandsindelingen.
Codecs zijn de software waarmee de compressie/decompressie algoritmen worden geïmplementeerd dat bestandsindelingen zijn containers waarin de gecomprimeerde video.

Dynamische verpakking kunt u voor het leveren van de inhoud van uw adaptieve bitsnelheid MP4 of goede Streaming gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) streaming biedt Media Services zonder dat u hoeft pakket opnieuw opslaan in deze indelingen streamen.

Als u wilt profiteren van [dynamische verpakking](media-services-dynamic-packaging-overview.md), moet u het volgende doen:

- Het bestand met mezzanine (bron) in een reeks geavanceerde bitsnelheid MP4-bestanden of adaptieve bitsnelheid goede Streaming-bestanden (de codering stappen worden verderop in deze zelfstudie aangetoond) te coderen.
- Ten minste één On-Demand streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen. Voor meer informatie Zie [On-Demand Streaming gereserveerde schaaleenheden](media-services-portal-manage-streaming-endpoints.md).

De volgende ondersteunt Media Services op vraag encoders die in dit artikel worden beschreven:

- [Media Encoder-standaard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Dit artikel bevat een kort overzicht van de op afroep media encoders en koppelingen naar artikelen met meer gedetailleerde informatie geven. Het onderwerp bevat ook een vergelijking van de encoders.

Houd er rekening mee dat elke Media Services rekening standaard een actieve codering taak tegelijk hebben kunt. U kunt codering eenheden waarmee u meerdere codering taken tegelijkertijd worden uitgevoerd, één voor elke codering gereserveerde eenheid aankoop reserveren. Voor meer informatie, Zie [schaal codering eenheden](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media Encoder-standaard

###<a name="how-to-use"></a>Het gebruik van

[Hoe te coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Indelingen

[Indelingen en -codecs](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Voorinstellingen

Media Encoder standaard is geconfigureerd met een van de encoder voorinstellingen beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Invoer en uitvoer, metagegevens

De invoer metagegevens encoders wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

De uitvoer encoders metagegevens wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Genereren van miniaturen

Zie voor meer informatie, [het genereren van miniaturen met Media Encoder standaard](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Trim video's (knippen)

Zie voor informatie [hoe u voor het bijsnijden van video's met Media Encoder standaard](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Bedekkingen maken

Voor meer informatie, Zie [Media Encoder standaard met bedekkingen maken](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Zie ook

[De blog Media Services](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media Encoder Premium Workflow

###<a name="overview"></a>Overzicht

[Introductie van Premium Services in Azure Media-codering](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Het gebruik van

Media Encoder Premium werkstroom is geconfigureerd met behulp van werkstromen. Workflow-bestanden kunnen worden gemaakt en bijgewerkt met behulp van het hulpprogramma [Workflow Designer](media-services-workflow-designer.md) .

[Het gebruik van Premium Services in Azure Media-codering](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Bekende problemen

Als uw video input geen bevat nog ondertiteling, de uitvoer activa bevatten steeds een leeg TTML bestand. 


##<a id="compare_encoders"></a>Encoders vergelijken

###<a id="billing"></a>Facturering meter gebruikt door elke encoder

Mediumnaam Processor|Geldende prijzen|Notities
---|---|---
**Media Encoder-standaard** |ENCODER|Codering taken aan de grootte van de uitvoer wordt afgeschreven activum in GBytes opgegeven snelheid [hier][1], onder de kolom ENCODER.
**Media Encoder Premium Workflow** |PREMIUM-ENCODER|Codering van taken worden berekend volgens de grootte van de uitvoer activa in GBytes opgegeven snelheid [hier][1], onder de kolom PREMIUM ENCODER.


In deze sectie wordt de codering mogelijkheden van **Media Encoder Standard** en **Premium Workflow van Media Encoder**vergeleken.


###<a name="input-containerfile-formats"></a>Container/bestandsindelingen invoer

Container/bestandsindelingen invoer|Media Encoder-standaard|Media Encoder Premium Workflow
---|---|---
Adobe® Flash® F4V           |Ja|Ja
MXF/SMPTE 377M              |Ja|Ja
GXF                         |Ja|Ja
MPEG-2-transportstromen    |Ja|Ja
MPEG-2-programma-Streams      |Ja|Ja
MPEG-4/MP4                  |Ja|Ja
Windows Media/ASF           |Ja|Ja
AVI (niet-gecomprimeerde 8-bits/10-bits)|Ja|Ja
3GPP/3GPP2                  |Ja|Nee
Goede Streaming-bestandsindeling (PIFF 1.3)|Ja|Nee
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Ja|Nee
Matroska/WebM               |Ja|Nee
QuickTime (MOV) |Ja|Nee

###<a name="input-video-codecs"></a>Invoer van Video-Codecs

Invoer van Video-Codecs|Media Encoder-standaard|Media Encoder Premium Workflow
---|---|---
AVC 8-bits/10-bits, tot 4:2:2, met inbegrip van AVCIntra   |8 bit 4:2:0 en 4:2:2|Ja
Fervente DNxHD (in MXF)                                 |Ja|Ja
DVCPro/DVCProHD (in MXF)                            |Ja|Ja
JPEG2000                                            |Ja|Ja
MPEG-2 (422 profiel en een hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)|Maximaal 422 profiel|Ja
MPEG-1                                              |Ja|Ja
Windows Media Video/VC-1                            |Ja|Ja
Canopus HQ/HQX                                      |Nee|Nee
MPEG-4 Part 2                                       |Ja|Nee
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja|Nee
Apple ProRes 422    |Ja|Nee
Apple ProRes 422 LT |Ja|Nee
Apple ProRes 422 HQ |Ja|Nee
Apple ProRes Proxy|Ja|Nee
Apple ProRes 4444 |Ja|Nee
Apple ProRes 4444 XQ |Ja|Nee

###<a name="input-audio-codecs"></a>Invoer van Audio-Codecs

Invoer van Audio-Codecs|Media Encoder-standaard|Media Encoder Premium Workflow
---|---|---
AES (SMPTE 331 M en M 302, AES3-2003)        |Nee|Ja
E Dolby®                                    |Nee|Ja
Dolby® Digital (AC3)                        |Nee|Ja
Dolby® Digital Plus (E-AC3)                 |Nee|Ja
AAC (AAC-LC en AAC HE AAC-HEv2; maximaal 5.1)|Ja|Ja
MPEG Layer 2|Ja|Ja
MP3 (MPEG-1 Audio Layer 3)|Ja|Ja
Windows Media Audio|Ja|Ja
WAV/PCM|Ja|Ja
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja|Nee
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Ja|Nee
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja|Nee


###<a name="output-containerfile-formats"></a>Container of het bestand uitvoerindelingen

Container of het bestand uitvoerindelingen|Media Encoder-standaard|Media Encoder Premium Workflow
---|---|---
Adobe® Flash® F4V|Nee|Ja
MXF (OP1a, XDCAM en AS02)|Nee|Ja
DPP (inclusief AS11)|Nee|Ja
GXF|Nee|Ja
MPEG-4/MP4|Ja|Ja
MPEG-TS|Ja|Ja
Windows Media/ASF|Nee|Ja
AVI (niet-gecomprimeerde 8-bits/10-bits)|Nee|Ja
Goede Streaming-bestandsindeling (PIFF 1.3)|Nee|Ja

###<a name="output-video-codecs"></a>Output Video-Codecs

Output Video-Codecs|Media Encoder-standaard|Media Encoder Premium Workflow
---|---|---
AVC (H.264; 8-bits; niveau tot hoge profiel, 5.2; 4 K Ultra HD; AVC-Intra)|Alleen 8-bits 4:2:0|Ja
Fervente DNxHD (in MXF)|Nee|Ja
MPEG-2 (422 profiel en een hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)|Nee|Ja
MPEG-1|Nee|Ja
Windows Media Video/VC-1|Nee|Ja
Maken van JPEG-miniaturen|Nee|Ja

###<a name="output-audio-codecs"></a>Uitvoer van Audio-Codecs

Uitvoer van Audio-Codecs|Media Encoder-standaard|Media Encoder Premium Workflow
---|---|---
AES (SMPTE 331 M en M 302, AES3-2003)|Nee|Ja
Dolby® Digital (AC3)|Nee|Ja
Dolby® Digital Plus (E-AC3) tot 7.1|Nee|Ja
AAC (AAC-LC en AAC HE AAC-HEv2; maximaal 5.1)|Ja|Ja
MPEG Layer 2|Nee|Ja
MP3 (MPEG-1 Audio Layer 3)|Nee|Ja
Windows Media Audio|Nee|Ja


##<a name="error-codes"></a>Foutcodes  

De volgende tabel worden de foutcodes die in het geval er een fout tijdens de uitvoering van de codering taken opgetreden is kunnen worden geretourneerd.  Als u details van fouten in .NET-code, gebruikt u de klasse [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) . Als u details van fouten in de code van de REST, de REST [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) API te gebruiken.

ErrorDetail.Code|Mogelijke oorzaken van fout
-----|-----------------------
Onbekend| Onbekende fout bij het uitvoeren van de taak
ErrorDownloadingInputAssetMalformedContent|Categorie van fouten die betrekking heeft op fouten bij het downloaden van invoer actief zoals ongeldige bestandsnamen, nul lengte bestanden niet juist opgemaakt enzovoort.
ErrorDownloadingInputAssetServiceFailure|De categorie van fouten die betrekking heeft op problemen aan de kant van de service - voorbeeld van de netwerk- of fouten bij het downloaden.
ErrorParsingConfiguration|Categorie van fouten wanneer taak <see cref="MediaTask.PrivateData"/> (configuratie) is niet geldig, bijvoorbeeld de configuratie is niet vooraf een geldig systeem of ongeldige XML bevat.
ErrorExecutingTaskMalformedContent|De categorie van fouten tijdens de uitvoering van de taak waar problemen binnen de invoer mediabestanden storing veroorzaken.
ErrorExecutingTaskUnsupportedFormat|Categorie van fouten waar de opgegeven bestanden kan niet worden verwerkt door de Mediaprocessor - media-indeling niet wordt ondersteund of komt niet overeen met de configuratie. Bijvoorbeeld probeert voor de productie van een alleen audio-uitvoer van een actief dat is de enige video
ErrorProcessingTask|Categorie van andere fouten die de Mediaprocessor ontdekt tijdens het verwerken van de taak die niet gerelateerd aan de inhoud zijn.
ErrorUploadingOutputAsset|Categorie van fouten tijdens het uploaden van de activa van de uitvoer
ErrorCancelingTask|Categorie van fouten die betrekking hebben op fouten wanneer u probeert de taak annuleren
TransientError|Categorie van fouten ter dekking van voorbijgaande problemen (bv.) tijdelijke netwerkproblemen met Azure opslag)


Als u de help van het team van **Media Services** , open een [ticket te ondersteunen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Verwante artikelen

- [Geavanceerde codering taken uitvoeren met de standaard van Media Encoder-voorinstellingen aanpassen](media-services-custom-mes-presets-with-dotnet.md)
- [Quota's en beperkingen](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
