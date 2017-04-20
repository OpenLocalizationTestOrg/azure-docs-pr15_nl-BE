<properties
    pageTitle="Overzicht van dynamische verpakking | Microsoft Azure"
    description="Het onderwerp hebt en een overzicht van dynamische verpakking."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dynamische verpakking

##<a name="overview"></a>Overzicht

Microsoft Azure Media Services kan worden gebruikt voor het leveren van vele media bron bestandsindelingen, media streaming-indelingen en beveiligde inhoud wordt opgemaakt met tal van technologieën van de client (bijvoorbeeld iOS XBOX, Silverlight, Windows 8). Deze clients verschillende protocollen begrijpt, bijvoorbeeld iOS vereist een HTTP Live Streaming (HLS) past V4-indeling en Silverlight en Xbox vereist goede Streaming. Als er een verzameling adaptieve bitsnelheid (multi-bitrate) MP4 bestanden (ISO Base Media 14496-12) of een verzameling adaptieve bitsnelheid goede Streaming-bestanden die u wilt gebruiken voor clients die MPEG-streepje, HLS of goede Streaming begrijpen, u moet gebruikmaken van dynamische verpakking Media Services.

Met dynamische verpakking is alles wat u nodig hebt voor het maken van een actief dat bestaat uit een set van adaptieve bitsnelheid MP4 of adaptieve bitsnelheid goede Streaming-bestanden. Klik, op basis van de opgegeven indeling in het manifest of fragment verzoek, de On-Demand Streaming server zal ervoor zorgen dat de gegevensstroom te ontvangen in het protocol dat u hebt gekozen. Hierdoor hoeft u alleen te slaan en te betalen voor de bestanden in één opslagmedium en Media Services-service zal maken en het juiste antwoord op basis van aanvragen van een client fungeren.

Het volgende diagram toont de traditionele codering en statische verpakking workflow.

![Statische codering](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

In het volgende diagram ziet u de werkstroom dynamische verpakking.

![Dynamische codering](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Als u wilt profiteren van dynamische verpakking, moet eerst krijgt u ten minste één On-demand streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt. Zie [schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

##<a name="common-scenario"></a>Gangbare scenario

1. Upload een invoerbestand (ook wel mezzanine-bestand genoemd). Bijvoorbeeld, H.264, MP4 of WMV (voor de lijst van ondersteunde indelingen Zie [Indelingen worden ondersteund door de standaard Media-Encoder](media-services-media-encoder-standard-formats.md).

1. De mezzanine bestand converteren naar MP4 H.264 adaptieve bitsnelheid sets.

1. Publiceren van de activa met de bitsnelheid van het adaptieve MP4 ingesteld door het maken van de On-Demand-Locator.

1. De streaming URL's om toegang te streamen van uw inhoud maken.


##<a name="preparing-assets-for-dynamic-streaming"></a>Voorbereiden van activa voor dynamische streaming

Ter voorbereiding van de activa van het dynamische streaming, hebt u twee mogelijkheden:

1. [Een master-bestand uploaden](media-services-dotnet-upload-files.md).
2. [Gebruik het coderingsprogramma Media Encoder standaard MP4 H.264 adaptieve bitsnelheid sets produceren](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [De inhoud van uw stream](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>De opmaak die niet worden ondersteund door dynamische verpakking

De volgende gegevensbron-indelingen worden niet ondersteund door dynamische verpakking.

- Dolby digital mp4-bestanden.
- Dolby digital vloeiend-bestanden.

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
