<properties 
    pageTitle="Media Encoder standaardindelingen en codecs" 
    description="Dit onderwerp geeft een overzicht van de Media Encoder standaardindelingen en codecs." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Standaard Media Encoder-indelingen en -Codecs


Dit document bevat een overzicht van de meest voorkomende importeren en kunt u met Media Encoder Standard exportbestandsindelingen.


##<a name="input-containerfile-formats"></a>Container/bestandsindelingen invoer

Bestandsindelingen (bestandsextensies)|Ondersteund
---|---|---|---
FLV-bestand (met H.264 en AAC codecs) (.flv)          |Ja 
MXF (.mxf)                  |Ja 
GXF (.gxf)                  |Ja 
MPEG2-PS, MPEG2-TS, 3GP (.ts, PS, .3gp, .3gpp, .mpg)   |Ja 
Windows mediavideo (WMV) / ASF (.wmv, .asf) |Ja 
AVI (niet-gecomprimeerde 8-bits/10 bit) (.avi)|Ja 
MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)|Ja 
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Ja 
Matroska/WebM (.mkv)        |Ja 
WAVE/WAV (.wav) |Ja 
QuickTime (MOV) |Ja

>[AZURE.NOTE] Hierboven is een lijst van de bestandsextensies voor veelvoorkomende meer. Media Encoder standaard ondersteunt vele andere (bijvoorbeeld: .m2ts, .mpeg2video, .qt). Als u probeert een bestand te coderen en u krijgt een foutbericht over de indeling niet wordt ondersteund, geef dan een feedback [hier](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Audio-indelingen in recipiënten van invoer 

Media Encoder-standaard ondersteunt de volgende audio-indelingen in containers ingevoerde uitvoering:

- MXF, GXF en QuickTime-bestanden hebben audiotracks met afwisselende stereo of 5.1 monsters

of

- MXF, GXF en QuickTime-bestanden waarbij de audio wordt uitgevoerd als afzonderlijke tracks van PCM, maar de kanaaltoewijzing (stereo of 5.1) kan worden afgeleid uit de metagegevens van het bestand

Houd er rekening mee dat ondersteuning voor kanaaltoewijzing expliciete/een gebruiker opgegeven in de nabije toekomst zal worden verstrekt.


##<a name="input-video-codecs"></a>Invoer van Video-Codecs

Invoer van Video-Codecs|Ondersteund
---|---|---|---
AVC 8-bits/10-bits, tot 4:2:2, met inbegrip van AVCIntra   |8 bit 4:2:0 en 4:2:2 
Fervente DNxHD (in MXF)                                 |Ja 
DVCPro/DVCProHD (in MXF)                            |Ja 
Digitale video (DV) (in AVI-bestanden)                   |Ja
JPEG 2000                                           |Ja 
MPEG-2 (422 profiel en een hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)|Maximaal 422 profiel 
MPEG-1                                              |Ja 
VC-1/WMV9                                           |Ja 
Canopus HQ/HQX                                      |Nee 
MPEG-4 Part 2                                       |Ja 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja 
Niet-gecomprimeerd YUV420 of mezzanine                   |Ja
Apple ProRes 422                                    |Ja
Apple ProRes 422 LT |Ja
Apple ProRes 422 HQ |Ja
Apple ProRes Proxy|Ja
Apple ProRes 4444 |Ja
Apple ProRes 4444 XQ |Ja



##<a name="input-audio-codecs"></a>Invoer van Audio-Codecs

Invoer van Audio-Codecs|Ondersteund
---|---|---|---
AAC (AAC-LC en AAC HE AAC-HEv2; maximaal 5.1)|Ja 
MPEG Layer 2|Ja 
MP3 (MPEG-1 Audio Layer 3)|Ja 
Windows Media Audio|Ja 
WAV/PCM|Ja 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja 
[Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |Ja 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja 
AMR (adaptive multi-rate)|Ja
AES (SMPTE 331 M en M 302, AES3-2003)        |Nee 
E Dolby®                                    |Nee 
Dolby® Digital (AC3)                        |Nee 
Dolby® Digital Plus (E-AC3)                 |Nee 


##<a name="output-formats-and-codecs"></a>Output-indelingen en -codecs

De volgende tabel worden de codecs en bestandsindelingen die worden ondersteund voor het exporteren.


Bestandsindeling|Video-Codec|Audio Codec
---|---|---
MP4 <br/><br/>(met inbegrip van multi-bitrate MP4 containers) |H.264 (hoog, Main en basislijn profielen)|AAC-LC, v1 HE-AAC, HE-AAC v2 
MPEG2-TS |H.264 (hoog, Main en basislijn profielen)|AAC-LC, v1 HE-AAC, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook

[Codering van inhoud op aanvraag met Azure mediaservices](media-services-encode-asset.md)

[Hoe te coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)
