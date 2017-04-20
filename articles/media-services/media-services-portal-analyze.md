<properties
    pageTitle="Analyseren van uw media met de Azure portal | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe u uw media met Analytics Media media processors (MPs) met behulp van de portal Azure verwerken."
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


# <a name="analyze-your-media-using-the-azure-portal"></a>Analyseren van uw media met de Azure portal

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Overzicht

Azure Media Services Analytics is een verzameling van spraak en visie onderdelen (op onderneming schaal, compliance, beveiliging en mondiaal bereik) die het eenvoudiger voor bedrijven en ondernemingen voor het afleiden van inzichten die hun video's sneller. Voor een gedetailleerd overzicht van Azure Media Services Analytics raadpleegt u [Dit](media-services-analytics-overview.md) onderwerp. 

In dit onderwerp wordt beschreven hoe u uw media met Analytics Media media processors (MPs) met behulp van de portal Azure verwerken. Media-Analytics MPs produceren MP4 of JSON bestanden. Als een Mediaprocessor geproduceerd een MP4-bestand, kunt u het bestand progressief downloaden. Als een Mediaprocessor geproduceerd een JSON-bestand, kunt u het bestand kunt downloaden van de Azure blobopslag. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Kies een activum dat u wilt analyseren 
 
1. Selecteer in de [Azure portal](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer in het venster **Instellingen** **activa**.  
.
    ![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Selecteer het element dat u wilt analyseren en druk op de knop **analyseren** .
        
    ![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. Selecteer in het venster **proces media activa met Media-Analytics** de processor. 

    De rest van het artikel wordt uitgelegd waarom en hoe u elke processor wordt gebruikt. 
   
4. Drukt u op **maken** naar het begin een taak.

## <a name="azure-media-indexer"></a>De indexeerfunctie Azure Media

De **Indexeerfunctie van Azure Media** Mediaprocessor kunt u mediabestanden en inhoud doorzoekbaar maken als gesloten closed captioning nummers te genereren. In deze sectie kunt enkele details over de opties die u voor deze MP opgeven kunt.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Taal

De natuurlijke taal wordt herkend in het multimediabestand. Engels of Spaans. 

### <a name="captions"></a>Bijschriften

U kunt een bijschrift-indeling die worden gegenereerd uit uw inhoud. Een taak indexing kunt ondertiteling bestanden genereren in de volgende indelingen:  

- **Sami**
- **TTML**
- **WebVTT**

Gesloten bijschrift (CC) bestanden in deze indeling kunnen worden gebruikt om audio en video bestanden toegankelijk maken voor mensen met auditieve handicap.

### <a name="aib-file"></a>AIB-bestand

Selecteer deze optie als u de Audio-Index Blob-bestand voor gebruik met de aangepaste IFilter voor SQL Server genereren. Zie [deze](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog voor meer informatie.

### <a name="keywords"></a>Trefwoorden

Selecteer deze optie als u dat wilt voor het genereren van een XML-bestand van trefwoorden. Dit bestand bevat trefwoorden opgehaald uit de inhoud van de spraak, met de frequentie en de marge-informatie.

### <a name="job-name"></a>Taaknaam

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt controleren de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de uitvoer inhoud te identificeren. 

## <a name="azure-media-hyperlapse"></a>Hyperlapse Azure Media

Azure Media Hyperlapse is een MP die vloeiend verstreken tijd video's uit de eerste persoon of actie-camera-inhoud worden gemaakt.  Raadpleeg [Dit](media-services-hyperlapse-content.md) onderwerp voor meer informatie. In deze sectie kunt enkele details over de opties die u voor deze MP opgeven kunt.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Snelheid 

Geef de snelheid waarmee de video input versnellen. De uitvoer is een gestabiliseerde en verstreken tijd vertoning van de video input.

### <a name="job-name"></a>Taaknaam

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt controleren de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de uitvoer inhoud te identificeren. 

## <a name="azure-media-face-detector"></a>Azure Media gezicht Detector

De processor in **Azure Media gezicht Detector** media (MP) kunt u tellen, verplaatsingen bijhouden en zelfs meter publiek deelname en reactie via gezichtsuitdrukkingen. Deze service bevat twee functies: 

- **Face detection**

    Face detection zoekt en menselijke gezichten in een video wordt bijgehouden. Meerdere vlakken kunnen worden gedetecteerd en vervolgens worden bijgehouden als ze onderweg, met de tijd en locatie metagegevens die worden geretourneerd in een JSON-bestand. Tijdens het bijhouden, wordt geprobeerd om een consistente ID geven op het vlak van dezelfde, terwijl de persoon is verplaatsen op het scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het frame laat.

    >[AZURE.NOTE]Deze services geen gelaatsherkenning uitgevoerd. Een persoon die het frame blijft of wordt ondervindt hinder van obstakels voor te lang krijgt een nieuwe ID als resultaat.

- **Detectie van emoties**
    
    Detectie van emoties is een optioneel onderdeel van de Face Detection Media Processor die analyse op meerdere emotionele kenmerken van de vlakken gedetecteerd wordt, met inbegrip van vriendschap, sadness, angst, boosheid en meer. 

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Detectie-modus

Een van de volgende modi kan worden gebruikt door de processor:

- Face detection
- per gezicht emoties detectie
- statistische emoties detectie

### <a name="job-name"></a>Taaknaam

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt controleren de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de uitvoer inhoud te identificeren. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector

De **Media-Bewegingsdetector Azure** Mediaprocessor (MP) kunt u gedeelten van een belang in een video anders lange en probleemloze efficiënt te identificeren. Beweging detectie kan worden gebruikt op statische camera beelden voor gedeelten van de video waar beweging plaatsvindt. Genereert een JSON-bestand met een metagegevens met tijdstempels en de omringende regio waar de gebeurtenis plaatsvond.

Deze technologie is gericht op beveiliging video-feeds, kunnen categoriseren beweging in relevante gebeurtenissen en vals-positief zoals belichting en schaduwen. Hiermee kunt u waarschuwingen genereren uit camera feeds zonder spam met oneindig veel niet-relevante gebeurtenissen, terwijl momenten van belang van toezicht op zeer lange video's uitpakken.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Video Azure mediaminiaturen

Deze processor kunt u samenvattingen van lange video's maken door het automatisch selecteren van interessante fragmenten uit de bronvideo. Dit is handig wanneer u een snel overzicht van wat u wilt kunt verwachten in een lange video. Zie voor uitgebreide informatie en voorbeelden [Azure Media Video miniaturen gebruiken voor het maken van een Video-samenvatting](media-services-video-summarization.md)

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Taaknaam

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt controleren de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de uitvoer inhoud te identificeren. 


##<a name="next-steps"></a>Volgende stappen

Weergave Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


