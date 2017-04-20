<properties
    pageTitle="Coderen van een actief Media Encoder standaard met de Azure portal | Microsoft Azure"
    description="Deze zelfstudie doorloopt u de stappen van een actief Media Encoder standaard met de Azure portal-codering."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Een actief Media Encoder standaard met de Azure portal coderen

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 

Als u werkt met Azure een van de meest voorkomende scenario's voor de clients levert bitsnelheid van adaptieve streaming Media-Services. Media-Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders). Uw video's om voor te bereiden bitsnelheid adaptieve streaming, moet u uw bronvideo in multi-bitrate bestanden coderen. Gebruik het coderingsprogramma **Media Encoder-standaard** voor het coderen van uw video's.  

Ook biedt dynamische verpakking waarmee u uw multi-bitrate MP4s leveren in de volgende indelingen voor streaming Media Services: MPEG-streepje, HLS, soepele Streaming of harde schijven, zonder dat u hoeft pakket opnieuw opslaan in deze indelingen streamen. Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in één opslagmedium en Media Services zal maken en het juiste antwoord op basis van aanvragen van een client fungeren.

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Het bronbestand te coderen in een aantal multi-bitrate MP4-bestanden (de codering stappen worden aangetoond in deze sectie).
- Ten minste één streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen. Zie de [streaming eindpunten configureren](media-services-portal-vod-get-started.md#configure-streaming-endpoints)voor meer informatie. 

Raadpleeg [Dit](media-services-portal-scale-media-processing.md) onderwerp wilt schalen media verwerking.

## <a name="encode-with-the-azure-portal"></a>Coderen met Azure portal

Deze sectie beschrijft de stappen die u nemen kunt om uw inhoud met Media Encoder standaard coderen.

1.  Selecteer in de [Azure portal](https://portal.azure.com/)uw Azure Media Services-account.
2.  Selecteer in het venster **Instellingen** **activa**.  
2.  Selecteer in het venster **activa** het activum dat u wilt coderen.
3.  Druk op de knop **coderen** .
4.  Selecteer de 'Media Encoder Standard' processor en een voorinstelling in het venster **een actief coderen** . Bijvoorbeeld, als u uw video input heeft een resolutie van 1920 x 1080 pixels, vervolgens kunt u de "H264 meerdere Bitrate 1080p ' vooraf ingesteld. Voor meer informatie over voorinstellingen, Zie [Dit](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikel: het is belangrijk de definitie die het meest geschikt is voor uw video invoer selecteren. Als u beschikt over een lage resolutie (640 x 360)-video, wordt u mag worden gemaakt van de standaard "H264 meerdere Bitrate 1080p ' vooraf ingesteld.
    
    U hebt een optie om de naam van de activa van de uitvoer en de naam van de taak te bewerken voor eenvoudiger beheer.
        
    ![Coderen van activa](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Druk op **maken**.


##<a name="next-step"></a>Volgende stap

In [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven, kunt u codering voortgang van het project met Azure portal controleren.  

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


