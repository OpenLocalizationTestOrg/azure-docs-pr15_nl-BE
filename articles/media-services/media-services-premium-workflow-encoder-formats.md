<properties 
    pageTitle="Media Encoder Premium Workflow-indelingen en -codecs | Microsoft Azure" 
    description="Dit onderwerp geeft een overzicht van de indelingen voor Media Encoder Premium Workflow-indelingen en -codecs" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium Workflow-indelingen en -codecs


>[AZURE.NOTE]E-mail voor premium encoder vragen mepd op Microsoft.com.
>
>Media Encoder Premium Workflow Mediaprocessor die worden beschreven in dit onderwerp is niet beschikbaar in China. 

Dit document bevat een lijst van de input en output-indelingen en -codecs die worden ondersteund door de openbare preview-versie van het coderingsprogramma **Media Encoder Premium Workflow** .

[Media Encoder Premium Worflow Input-indelingen en -Codecs](#input_formats)

[Media Encoder Premium Worflow Output-indelingen en -Codecs](#output_formats)

**Media Encoder Premium Workflow** biedt ondersteuning voor ondertiteling in [deze](#closed_captioning) sectie beschreven. 


##<a id="input_formats"></a>Media Encoder Premium Workflow Input-indelingen en -Codecs

In de volgende sectie worden de codecs en bestandsindelingen die deze Mediaprocessor als invoer ondersteunt.

###<a name="input-containerfile-formats"></a>Container/bestandsindelingen invoer

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- MPEG-2-transportstromen
- MPEG-2-programma-Streams
- MPEG-4/MP4
- Windows Media/ASF
- AVI (niet-gecomprimeerde 8-bits/10-bits)

###<a name="input-video-codecs"></a>Invoer van Video-Codecs

- AVC 8-bits/10-bits, tot 4:2:2, met inbegrip van AVCIntra
- Fervente DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- JPEG2000
- MPEG-2 (422 profiel en een hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Invoer van Audio-Codecs

- AES (SMPTE 331 M en M 302, AES3-2003)
- E Dolby®
- Dolby® Digital (AC3)
- AAC (AAC-LC en AAC HE AAC-HEv2; maximaal 5.1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV/PCM
 
##<a id="output_format"></a>Media Encoder Premium Workflow Output-indelingen en -Codecs

De volgende sectie worden de codecs en bestandsindelingen die worden ondersteund als de uitvoer van deze Mediaprocessor.

###<a name="output-containerfile-formats"></a>Container of het bestand uitvoerindelingen

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM en AS02)
- DPP (inclusief AS11)
- GXF
- MPEG-4/MP4
- Windows Media/ASF
- AVI (niet-gecomprimeerde 8-bits/10-bits)
- Goede Streaming-bestandsindeling (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Output Video-Codecs

- AVC (H.264; 8-bits; niveau tot hoge profiel, 5.2; 4 K Ultra HD; AVC-Intra)
- Fervente DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- MPEG-2 (422 profiel en een hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
- MPEG-1
- Windows Media Video/VC-1
- Maken van JPEG-miniaturen

###<a name="output-audio-codecs"></a>Uitvoer van Audio-Codecs

- AES (SMPTE 331 M en M 302, AES3-2003)
- Dolby® Digital (AC3)
- Dolby® Digital Plus (E-AC3) tot 7.1
- AAC (AAC-LC en AAC HE AAC-HEv2; maximaal 5.1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Ondersteuning voor ondertiteling

Op te nemen, **Media Encoder Premium werkstroom** ondersteunt:

1. SCC-bestanden
1. SMPTE TT-bestanden
1. CEA-608/CEA-708 – uitgevoerd als een gebruikersgegevens (SEI berichten H.264 elementaire streams, ATSC/53, SCTE20) of als bijkomende gegevens in bestanden MXF/GXF uitgevoerd
1. STL ondertitel bestanden

Op de uitvoer van zijn de volgende opties beschikbaar:

1. CEA-608 CEA 708 vertaling
1. CEA-608/CEA-708 passeren (ingesloten in berichten van elementaire H.264-streams SEI of als bijkomende gegevens in MXF bestanden uitgevoerd)
1. SCC
1. SMPTE Timed Text (vanuit bron CEA 608 per SMPTE RP2052, inclusief het maken van het bestand DFXP)
1. Sorteren ondertitel bestand
1. DVB ondertitel stromen

Opmerking: niet alle notaties van de bovenstaande uitvoer voor levering via streaming in Azure Media Services worden ondersteund.

##<a name="known-issues"></a>Bekende problemen

Als uw video input geen bevat nog ondertiteling, de uitvoer activa bevatten steeds een leeg TTML bestand. 


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
