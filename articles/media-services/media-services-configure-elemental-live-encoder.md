<properties 
    pageTitle="Configureer het coderingsprogramma elementair Live voor het verzenden van een enkele live bitsnelheid | Microsoft Azure" 
    description="In dit onderwerp wordt beschreven hoe de elementaire Live encoder voor het verzenden van een enkele bitsnelheid op AMS kanalen die zijn ingeschakeld voor live-codering configureren." 
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
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>De elementaire Live encoder gebruiken voor het verzenden van een enkele live bitsnelheid

> [AZURE.SELECTOR]
- [Vorm van het actieve element](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

In dit onderwerp wordt beschreven hoe de [Elementaire Live](http://www.elementaltechnologies.com/products/elemental-live) encoder voor het verzenden van een enkele bitsnelheid op AMS kanalen die zijn ingeschakeld voor live-codering configureren.  Zie [werken met kanalen, die voor het uitvoeren van Live-codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.

Deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met het hulpprogramma Azure Media Services Explorer (AMSE). Dit hulpprogramma kan alleen worden uitgevoerd op Windows-PC. Als u op Mac of Linux, gebruikt u de Azure portal [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)te maken.

##<a name="prerequisites"></a>Vereisten

- Moet een praktische kennis van het gebruik van elementaire Live web-interface voor het maken van live gebeurtenissen.
- [Maak een account Azure Media Services](media-services-portal-create-account.md)
- Zorg er is een eindpunt Streaming met ten minste één streaming eenheid worden toegewezen. Zie [Streaming eindpunten op een rekening van Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
- Installeer de nieuwste versie van het hulpprogramma [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Het hulpprogramma starten en verbinding maken met uw account AMS.

##<a name="tips"></a>Tips

- Gebruik waar mogelijk een ' hardwired ' internet-verbinding.
- Een goede vuistregel bij het bepalen van de bandbreedtevereisten voor is het dubbele van de streaming bitsnelheid. Dit is niet een dwingende eisen gelden, het helpen bij het verminderen van de impact van opstoppingen in het netwerk.
- Bij het gebruik van software op basis van encoders, sluit u overbodige programma's.

## <a name="elemental-live-with-rtp-ingest"></a>Consumptie van elementaire Live met RTP

In dit gedeelte ziet u hoe de elementaire Live-encoder, die een enkele live bitsnelheid worden verzonden via het RTP configureren.  Zie voor meer informatie, [MPEG TS stream via RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Een kanaal maken

1.  Ga naar het tabblad **Live** in het hulpprogramma AMSE en klik met de rechtermuisknop binnen het gebied van het kanaal. Selecteer **maken kanaal...** in het menu.

![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. De naam van een kanaal, geeft het beschrijvingsveld is optioneel. Kanaal Selecteer onder instellingen voor **standaard** voor de optie Live-codering met het Protocol invoer is ingesteld op **RTP (MPEG-TS)**. Kunt u andere instellingen is.


Zorg ervoor dat de **Start nu het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.
![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Het kanaal duurt zo lang als 20 minuten te starten.

U kunt [het coderingsprogramma configureren](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp)terwijl het kanaal wordt gestart.

>[AZURE.IMPORTANT] Houd er rekening mee dat verrekening als kanaal in gereedheid gaat begint. Zie voor meer informatie, [de lidstaten van het kanaal](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>Configureer het coderingsprogramma elementair Live 

In deze zelfstudie worden de volgende output instellingen gebruikt. De rest van deze sectie beschreven configuratiestappen uitvoeriger. 

**Video**:
 
- Codec: H.264 
- Profiel: Hoog (niveau 4.0) 
- Bitsnelheid: 5000 k 
- Hoofdframe: 2 seconden (60 seconden) 
- Frame Rate: 30
 
**Audio**:

- Codec: AAC (LC) 
- Bitsnelheid: 192 kbps 
- Bemonsteringsfrequentie: 44,1 kHz


####<a name="configuration-steps"></a>Configuratiestappen

1. Ga naar de **Elementaire Live** web-interface en de encoder voor **UDP/TS** streaming instellen. 

2. Zodra een nieuwe gebeurtenis is gemaakt, Ga naar de uitvoer-groepen en voeg de groep van de output **UDP/TS** . 

3. Maak een nieuwe uitvoer door **nieuwe Stream** te selecteren en vervolgens op **Uitvoer toevoegen**.  
    
    ![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Het wordt aanbevolen dat de elementaire gebeurtenis de tijdcode die is ingesteld op 'Klok heeft' waarmee de encoder opnieuw in het geval van een storing van de stream.

4. Nu de Output is gemaakt, klikt u op **toevoegen van stroom**. De uitvoerinstellingen kunnen nu worden geconfigureerd. 
5. Ga naar de "Stream 1" die zojuist is gemaakt, klikt u op het tabblad **Video** aan de linkerkant en vouw de sectie **Advanced** settings. 

    ![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Als elementair Live een breed scala heeft van beschikbare aanpassen, worden de volgende instellingen worden aanbevolen voor aan de slag met streaming naar AMS. 
    
    - Resolutie: 1280 x 720 
    - Framesnelheid: 30 
    - GOP grootte: 60 frames 
    - Geïnterlinieerd modus: progressieve 
    - Bitsnelheid: 5000000 bits/s (dit kan worden aangepast op basis van beperkingen op het netwerk) 
    

    ![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Invoer van het kanaal-URL ophalen.
    
    Ga terug naar het hulpprogramma AMSE en controleren van de voltooiingsstatus van het kanaal. Zodra de status is gewijzigd van **eerste** **uitgevoerd**, kunt u de ingevoerde URL.
      
    Als het kanaal wordt uitgevoerd, rechts Klik op de naam van het kanaal Ga naar zweven over **Invoer-URL kopiëren naar Klembord** en selecteer vervolgens **URL voor primaire invoer**.  
    
    ![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Deze informatie in het veld **Primaire doel** van de vorm van het element te plakken. Alle andere instellingen kunnen blijven de standaardwaarde.
    
    ![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Herhaal deze stappen met de URL van de secundaire invoer door het maken van een afzonderlijk tabblad van 'Uitvoer' voor het streamen van UDP/TS extra redundantie in te bouwen.
    
7. Klik op **maken** (als u een nieuwe gebeurtenis is gemaakt) of **bijwerken** (als u een bestaande gebeurtenis bewerken) en gaat u verder met het coderingsprogramma start. 

>[AZURE.IMPORTANT]Voordat u op **Start** op de elementaire Live web-interface, u **moet** ervoor zorgen dat het kanaal gereed is. 
>Zorg ervoor dat u niet in een staat klaar zonder een gebeurtenis langer dan 15 minuten > laat u het kanaal.

Nadat de stroom gedurende 30 seconden wordt uitgevoerd, Ga terug naar het afspelen van de AMSE gereedschap en test.  

###<a name="test-playback"></a>Test afspelen
  
1. Ga naar het hulpprogramma AMSE en klik met de rechtermuisknop het kanaal te worden getest. Plaats de muisaanwijzer op het **afspelen van de voorvertoning** en selecteer **met Azure Media Player**in het menu.  

    ![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Als de stroom wordt weergegeven in Windows media player, is vervolgens het coderingsprogramma correct is geconfigureerd voor verbinding met AMS. 

Als een fout wordt ontvangen, wordt het kanaal moet worden ingesteld en encoder instellingen aangepast. Zie het onderwerp [problemen oplossen](media-services-troubleshooting-live-streaming.md) voor hulp.   

###<a name="create-a-program"></a>Een programma maken

1. Nadat het kanaal afspelen is bevestigd, een programma maken. Klik op het tabblad **Live** in het hulpprogramma AMSE Klik met de rechtermuisknop in het gebied programma en selecteer **Nieuw programma maken**.  

    ![Elementair](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Naam van het programma en wijzig indien nodig de **Archief venster lengte** (die standaard ingesteld op 4 uur). U kunt ook een opslaglocatie opgeven of laten als standaard.  
3. Schakel het selectievakje voor **het programma nu starten** .
4. Klik op **programma**.  
  
    Opmerking: Het programma maken kost minder tijd dan kanaal maken.    
 
5. Zodra het programma wordt uitgevoerd, bevestigt dat door rechts te klikken op het programma en om het **afspelen van de programma's** te navigeren en vervolgens te selecteren **met Azure Media Player**afspelen.  
6. Zodra bevestigd, klik met de rechtermuisknop het programma opnieuw en selecteert u **de URL van de uitvoer naar Klembord kopiëren** (of deze gegevens ophalen uit de optie **programma-informatie en instellingen** in het menu). 

De stroom is nu klaar om te worden ingesloten in een speler of verspreid op een doelgroep voor live weergave.  

## <a name="troubleshooting"></a>Het oplossen van problemen

Zie het onderwerp [problemen oplossen](media-services-troubleshooting-live-streaming.md) voor hulp. 


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
