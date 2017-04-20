<properties
    pageTitle=" Schaal streaming eindpunten met Azure portal | Microsoft Azure"
    description="Deze zelfstudie doorloopt u de stappen voor het schalen van streaming eindpunten met Azure portal."
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


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Schaal streaming eindpunten met Azure portal

##<a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 

Als u werkt met Azure een van de meest voorkomende scenario's voor de clients levert video via bitsnelheid van adaptieve streaming Media-Services. Media-Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

Media Services biedt dynamische verpakking kunt u ervoor zorgen dat uw adaptieve bitsnelheid inhoud MP4 gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) just-in-time, zonder dat u hoeft op te slaan van voorverpakte versies van elk van deze indelingen streamen streaming.

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Het bestand met mezzanine (bron) in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie aangetoond) te coderen.  
- Maak ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u leveren uw inhoud wilt. De volgende stappen uit hoe het aantal streaming eenheden wijzigen.

Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in één opslagmedium en Media Services zal maken en het juiste antwoord op basis van aanvragen van een client fungeren.

U kunt bovendien bepalen dat de capaciteit van de service Endpoint Streaming voor het verwerken van de groeiende vraag naar bandbreedte door streaming eenheden aan te passen. Het verdient aanbeveling een of meer schaaleenheden toewijzen voor toepassingen in de productie-omgeving. Streaming eenheden bieden zowel egress toegewezen capaciteit die functies waaronder die in stappen van 200 Mbps en extra functionaliteit kan worden aangeschaft: [dynamische verpakking](media-services-dynamic-packaging-overview.md), CDN-integratie en geavanceerde configuratie. Zie [streaming eindpunten met Azure portal beheren](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

## <a name="scale-streaming-endpoints"></a>Schaal streaming eindpunten

Maken en wijzigen van het aantal gereserveerde eenheden streaming, doet u het volgende:

1. Selecteer in de [Azure portal](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer in het venster **Instellingen voor** **Streaming-eindpunten**.
3. Klik op het eindpunt voor streaming die u wilt schalen. 
4. Verplaats de schuifregelaar om op te geven van het aantal eenheden voor streaming
 
![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

De volgende overwegingen zijn van toepassing:

- De toewijzing van alle eenheden worden nieuwe streaming kan ongeveer 20 minuten duren om te voltooien. 
- Op dit moment kunt gaat uit van een positieve waarde van de eenheden op geen streaming, uitschakelen op verzoek streaming voor maximaal een uur.
- Het grootste aantal eenheden dat is opgegeven voor de periode van 24 uur wordt gebruikt bij het berekenen van de kosten. Zie voor meer informatie over de prijsdetails [Media Services prijzen Details](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


