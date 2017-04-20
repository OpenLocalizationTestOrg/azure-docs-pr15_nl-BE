<properties
    pageTitle=" Aan de slag met het leveren van inhoud op aanvraag via de portal Azure | Microsoft Azure"
    description="Deze zelfstudie doorloopt u de stappen voor het implementeren van een basisdienst Video-on-Demand (VoD) leveren van inhoud met Azure Media Services (AMS)-toepassing met behulp van de portal Azure."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Aan de slag met het leveren van inhoud op aanvraag via de portal Azure

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Deze zelfstudie doorloopt u de stappen voor het implementeren van een basisdienst Video-on-Demand (VoD) leveren van inhoud met Azure Media Services (AMS)-toepassing met behulp van de portal Azure.

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 

Deze zelfstudie bevat de volgende taken:

1.  Maak een account Azure Media Services.
2.  Eindpunt voor mediagegevensstromen configureren.
1.  Een FLV-bestand uploaden.
1.  Het bronbestand te coderen in een verzameling adaptieve bitsnelheid MP4-bestanden.
1.  Publiceren van de activa en get streaming en progressief downloaden van URL's.  
1.  De inhoud af te spelen.


## <a name="create-an-azure-media-services-account"></a>Maak een account Azure Media Services

De stappen in deze sectie wordt aangegeven hoe een AMS-account te maken.

1. Aanmelden op de [portal Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Web + Mobile** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer in het vak **Accountnaam**de naam van de nieuwe AMS-account. De naam van een Media Services alle kleine letters, cijfers of letters zonder spaties en 3 tot en met 24 tekens lang is.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling bronnen die delen van de levenscyclus, machtigingen en beleid. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Selecteer de regio wordt op **locatie**, voor het opslaan van de records media en metagegevens voor uw Media Services-account gebruikt. Deze regio wordt gebruikt voor het verwerken en media streamen. De beschikbare Media Services gebieden worden weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer een opslag voor blobopslag van de media-inhoud van uw Media Services-account in **Rekening voor opslag**. Kunt u een bestaande account voor opslag in hetzelfde geografische gebied, als uw Media Services-account, of kunt u een account voor opslag. Een nieuwe account voor de opslag wordt gemaakt in dezelfde regio. De regels voor namen van opslag zijn hetzelfde als voor de accounts van Media Services.

        Meer informatie over opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan het dashboard** voor een overzicht van de voortgang van de implementatie rekening.
    
7. Klik op **maken** onder aan het formulier.

    Zodra de account is gemaakt, verandert de status **actief**. 

    ![Instellingen voor Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld video's te uploaden, activa te coderen, bewaken van de voortgang van het project) het venster **Instellingen** .

## <a name="manage-keys"></a>Sleutels beheren

U moet de naam van de account en de primaire sleutel gegevens via programmering toegang tot de Media Services-account.

1. Selecteer uw account in het portal voor Azure. 

    Het venster **Instellingen** verschijnt aan de rechterkant. 

2. Selecteer in het venster **Instellingen** **toetsen**. 

    De **sleutels beheren** van windows bevat de naam en de primaire en secundaire sleutels worden weergegeven. 
3. Druk op de knop kopiëren om de waarden te kopiëren.
    
    ![Media Services-toetsen](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Streaming eindpunten configureren

Als u werkt met Azure een van de meest voorkomende scenario's voor de clients levert video via bitsnelheid van adaptieve streaming Media-Services. Media-Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

Media Services biedt dynamische verpakking, kunt u ervoor zorgen dat uw adaptieve bitsnelheid inhoud MP4 gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) just-in-time, zonder dat u hoeft op te slaan van voorverpakte versies van elk van deze indelingen streamen streaming.

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Het bestand met mezzanine (bron) in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie aangetoond) te coderen.  
- Maak ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u leveren uw inhoud wilt. De volgende stappen uit hoe het aantal streaming eenheden wijzigen.

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in de opslagindeling in één en Media Services maakt en het juiste antwoord op basis van aanvragen van een client fungeert.

Maken en wijzigen van het aantal gereserveerde eenheden streaming, doet u het volgende:


1. Klik in het venster **Instellingen voor** **Streaming eindpunten**. 

2. Klik op het eindpunt streaming standaard. 

    Het venster **Std. STREAMING EINDPUNTDETAILS** weergegeven.

3. Als u het aantal eenheden streaming, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming-eenheden](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van alle nieuwe eenheden kan maximaal 20 minuten in beslag nemen.

## <a name="upload-files"></a>Bestanden uploaden

Om te streamen video's met Azure Media Services, moet u de bron video's uploaden, coderen in meerdere bitsnelheid en het resultaat publiceren. De eerste stap wordt in deze sectie gedekt. 

1. Klik in het venster **instelling van** **activa**.

    ![Bestanden uploaden](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Klik op de knop **uploaden** .

    Het **uploaden van een video actief** venster verschijnt.

    >[AZURE.NOTE] Er is geen bestandsgrootte.
    
4. Blader naar de gewenste video op uw computer, selecteert u het en klik op OK.  

    De upload is gestart en ziet u de voortgang van de naam van het bestand.  

Nadat het uploaden is voltooid, ziet u het nieuwe activum weergegeven in het venster van de **activa** . 

## <a name="encode-assets"></a>Coderen van activa

Als u werkt met Azure een van de meest voorkomende scenario's voor de clients levert bitsnelheid van adaptieve streaming Media-Services. Media-Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders). Uw video's om voor te bereiden bitsnelheid adaptieve streaming, moet u uw bronvideo in multi-bitrate bestanden coderen. Gebruik het coderingsprogramma **Media Encoder-standaard** voor het coderen van uw video's.  

Ook biedt dynamische verpakking, zodat u uw multi-bitrate MP4s leveren in de volgende indelingen voor streaming Media Services: MPEG-streepje, HLS, soepele Streaming of harde schijven, zonder dat u hoeft om te verpakken in deze indelingen streamen. Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in de opslagindeling in één en Media Services maakt en het juiste antwoord op basis van aanvragen van een client fungeert.

Als u wilt profiteren van dynamische verpakking, moet u het volgende doen:

- Het bronbestand te coderen in een aantal multi-bitrate MP4-bestanden (de codering stappen worden aangetoond in deze sectie).
- Ten minste één streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen. Zie de [streaming eindpunten configureren](media-services-portal-vod-get-started.md#configure-streaming-endpoints)voor meer informatie. 

### <a name="to-use-the-portal-to-encode"></a>Met behulp van de portal coderen

Deze sectie beschrijft de stappen die u nemen kunt om uw inhoud met Media Encoder standaard coderen.

1.  Selecteer in het venster **Instellingen** **activa**.  
2.  Selecteer in het venster **activa** het activum dat u wilt coderen.
3.  Druk op de knop **coderen** .
4.  Selecteer de 'Media Encoder Standard' processor en een voorinstelling in het venster **een actief coderen** . Bijvoorbeeld, als u uw video input heeft een resolutie van 1920 x 1080 pixels, vervolgens kunt u de "H264 meerdere Bitrate 1080p ' vooraf ingesteld. Voor meer informatie over voorinstellingen, Zie [Dit](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikel: het is belangrijk de definitie die het meest geschikt is voor uw video invoer selecteren. Als u beschikt over een lage resolutie (640 x 360)-video, wordt u mag worden gemaakt van de standaard "H264 meerdere Bitrate 1080p ' vooraf ingesteld.
    
    U hebt een optie om de naam van de activa van de uitvoer en de naam van de taak te bewerken voor eenvoudiger beheer.
        
    ![Coderen van activa](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Druk op **maken**.

### <a name="monitor-encoding-job-progress"></a>Monitor codering van de voortgang van het project

Om de voortgang van het project coderen, klikt u op **Instellingen** (bovenaan de pagina) en selecteer vervolgens de **taken**.

![Taken](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Inhoud publiceren

Om de gebruiker met een URL die kan worden gebruikt om te streamen of te downloaden van uw inhoud, moet u eerst 'publiceren' uw activa door het maken van een locator. Locators bieden toegang tot bestanden in de activa. Media Services ondersteunt twee typen locators: 

- Streaming (OnDemandOrigin)-locators, gebruikt voor adaptieve streaming (bijvoorbeeld naar streepje MPEG stream, HLS of goede Streaming). Als u wilt een streaming locator maken uw bezit, moeten een bestand .ism bevatten. 
- Progressief (SAS)-Locator, die wordt gebruikt voor de levering van video via progressief downloaden.


Een streaming URL heeft de volgende indeling en kunt u deze activa goede Streaming afgespeeld.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Voor het opbouwen van een HLS streaming URL toevoegen (format = m3u8 aapl) naar de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Voor het opbouwen van een MPEG-streepje streaming URL toevoegen (format = mpd-tijd-csf) naar de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Een SAS-URL heeft de volgende indeling.

    {blob container name}/{asset name}/{file name}/{SAS signature}

>[AZURE.NOTE] Als u de portal locators gemaakt vóór maart 2015, zijn locators met een vervaldatum van twee jaar gemaakt.  

Als u wilt bijwerken met een vervaldatum op een locator [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API's te gebruiken. Als u de vervaldatum van een SAS-locator, verandert de URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>De portal te gebruiken voor het publiceren van een actief

Voor het gebruik van de portal voor het publiceren van een actief, het volgende doen:

1. Selecteer **Instellingen** > **activa**.
1. Selecteer het element dat u wilt publiceren.
1. Klik op de knop **publiceren** .
1. Selecteer het type locator.
2. Druk op **toevoegen**.

    ![Publiceren](./media/media-services-portal-vod-get-started/media-services-publish1.png)

De URL wordt toegevoegd aan de lijst met **URL's gepubliceerd**.

## <a name="play-content-from-the-portal"></a>Afspelen vanaf de portal-inhoud

De Azure portal biedt een content-speler die u gebruiken kunt voor het testen van uw video.

Klik op de gewenste video en klik op de knop **afspelen** .

![Publiceren](./media/media-services-portal-vod-get-started/media-services-play.png)

Sommige overwegingen zijn van toepassing:

- Controleer of dat de video is gepubliceerd.
- Deze **Mediaspeler** speelt uit het standaardbeleid voor streaming eindpunt. Als u afspelen vanaf een niet-standaard streaming eindpunt wilt, klikt u op de URL kopiëren en het gebruik van een andere speler. Bijvoorbeeld: [Azure Services mediaspeler](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


