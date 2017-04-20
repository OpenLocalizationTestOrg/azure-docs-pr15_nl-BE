<properties 
    pageTitle="Configureer het coderingsprogramma NewTek TriCaster voor het verzenden van een enkele live bitsnelheid | Microsoft Azure" 
    description="In dit onderwerp ziet u hoe de Tricaster live encoder voor het verzenden van een enkele bitsnelheid op AMS kanalen die zijn ingeschakeld voor live-codering configureren." 
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
    ms.author="juliako;cenkd;anilmur"/>

#<a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Het coderingsprogramma NewTek TriCaster gebruiken voor het verzenden van een enkele live bitsnelheid

> [AZURE.SELECTOR]
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Vorm van het actieve element](media-services-configure-elemental-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

In dit onderwerp wordt beschreven hoe de [TriCaster van NewTek](http://newtek.com/products/tricaster-40.html) live encoder voor het verzenden van een enkele bitsnelheid op AMS kanalen die zijn ingeschakeld voor live-codering configureren. Zie [werken met kanalen, die voor het uitvoeren van Live-codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.

Deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met het hulpprogramma Azure Media Services Explorer (AMSE). Dit hulpprogramma kan alleen worden uitgevoerd op Windows-PC. Als u op Mac of Linux, gebruikt u de Azure portal [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)te maken.

>[AZURE.NOTE]Wanneer u Tricaster gebruikt voor het verzenden van een bijdrage aan AMS kanalen die zijn ingeschakeld voor het coderen van live-feed, kunnen er problemen met video en audio in live-gebeurtenis als u bepaalde functies van Tricaster zoals snel tussen feeds knippen of overschakelen van leien. Het team AMS werkt aan het oplossen van deze kwesties, tot die tijd, is het niet aanbevolen om deze functies te gebruiken.


##<a name="prerequisites"></a>Vereisten

- [Maak een account Azure Media Services](media-services-portal-create-account.md)
- Zorg er is een eindpunt Streaming met ten minste één streaming eenheid worden toegewezen. Zie [Streaming eindpunten op een rekening van Media Services](media-services-portal-manage-streaming-endpoints.md) voor meer informatie.
- Installeer de nieuwste versie van het hulpprogramma [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Het hulpprogramma starten en verbinding maken met uw account AMS.

##<a name="tips"></a>Tips

- Gebruik waar mogelijk een ' hardwired ' internet-verbinding.
- Een goede vuistregel bij het bepalen van de bandbreedtevereisten voor is het dubbele van de streaming bitsnelheid. Dit is niet een dwingende eisen gelden, het helpen bij het verminderen van de impact van opstoppingen in het netwerk.
- Bij het gebruik van software op basis van encoders, sluit u overbodige programma's.

## <a name="create-a-channel"></a>Een kanaal maken

1.  Ga naar het tabblad **Live** in het hulpprogramma AMSE en klik met de rechtermuisknop binnen het gebied van het kanaal. Selecteer **maken kanaal...** in het menu.

![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. De naam van een kanaal, geeft het beschrijvingsveld is optioneel. Kanaal Selecteer onder instellingen voor **standaard** voor de optie Live-codering met het Protocol invoer is ingesteld op **RTMP**. Kunt u andere instellingen is.


Zorg ervoor dat de **Start nu het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.
![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

>[AZURE.NOTE] Het kanaal duurt zo lang als 20 minuten te starten.


U kunt [het coderingsprogramma configureren](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp)terwijl het kanaal wordt gestart.

>[AZURE.IMPORTANT] Houd er rekening mee dat verrekening als kanaal in gereedheid gaat begint. Zie voor meer informatie, [de lidstaten van het kanaal](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_tricaster_rtmp></a>Configureer het coderingsprogramma NewTek TriCaster

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


###<a name="configuration-steps"></a>Configuratiestappen

1. Maak een nieuwe **TriCaster van NewTek** project afhankelijk van welke video-invoerbron wordt gebruikt. 
2. Eenmaal in dat project, vinden de **Stream** -knop en klik op het pictogram van het vistuig ernaast voor toegang tot het menu stream configuratie.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Als het menu is geopend, klikt u op **Nieuw** onder de kop van de verbinding. Als u wordt gevraagd het verbindingstype, selecteert u **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)

4. Klik op **OK**.

5. Een FMLE-profiel kan nu worden geïmporteerd door te klikken op de vervolgkeuzepijl onder **Profiel Streaming** en navigeren **door bladeren**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)

6. Navigeer naar waar het geconfigureerde FMLE profiel is opgeslagen.
7. Selecteren en druk op **OK**.

    Nadat het profiel is geüpload, gaat u verder met de volgende stap.

6. Invoer van het kanaal-URL te toe te wijzen aan het Tricaster **RTMP eindpunt**ophalen.
    
    Ga terug naar het hulpprogramma AMSE en controleren van de voltooiingsstatus van het kanaal. Zodra de status is gewijzigd van **eerste** **uitgevoerd**, kunt u de ingevoerde URL.
      
    Als het kanaal wordt uitgevoerd, rechts Klik op de naam van het kanaal Ga naar zweven over **Invoer-URL kopiëren naar Klembord** en selecteer vervolgens **URL voor primaire invoer**.  
    
    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)

7. Deze gegevens in het veld **locatie** onder **Flash Server** binnen het project Tricaster plakken. Een stream-naam in het veld **ID Stream** ook toewijzen. 

    Als stroom informatie is toegevoegd aan het profiel van de FMLE, kan deze ook worden geïmporteerd naar deze sectie door te klikken op **Instellingen importeren**, navigeren naar het opgeslagen profiel met FMLE en op **OK**te klikken. De relevante velden in de Flash-Server moeten gevuld met de gegevens van FMLE.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)

9. Wanneer u klaar bent, klikt u op **OK** onder aan het scherm. Wanneer u video en audio-ingangen in de Tricaster gereed zijn, beginnen met streaming naar AMS door te klikken op de knop **gegevensstroom** .

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

>[AZURE.IMPORTANT]Voordat u de **stroom**op, u **moet** ervoor zorgen dat het kanaal gereed is. 
>Zorg ervoor dat u niet het kanaal gereed verlaten zonder een invoer bijdrage langer dan 15 minuten > feed. 

##<a name="test-playback"></a>Test afspelen
  
1. Ga naar het hulpprogramma AMSE en klik met de rechtermuisknop het kanaal te worden getest. Plaats de muisaanwijzer op het **afspelen van de voorvertoning** en selecteer **met Azure Media Player**in het menu.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Als de stroom wordt weergegeven in Windows media player, is vervolgens het coderingsprogramma correct is geconfigureerd voor verbinding met AMS. 

Als een fout wordt ontvangen, wordt het kanaal moet worden ingesteld en encoder instellingen aangepast. Zie het onderwerp [problemen oplossen](media-services-troubleshooting-live-streaming.md) voor hulp.  

##<a name="create-a-program"></a>Een programma maken

1. Nadat het kanaal afspelen is bevestigd, een programma maken. Klik op het tabblad **Live** in het hulpprogramma AMSE Klik met de rechtermuisknop in het gebied programma en selecteer **Nieuw programma maken**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)

2. Naam van het programma en wijzig indien nodig de **Archief venster lengte** (die standaard ingesteld op 4 uur). U kunt ook een opslaglocatie opgeven of laten als standaard.  
3. Schakel het selectievakje voor **het programma nu starten** .
4. Klik op **programma**.  
  
    Opmerking: Het programma maken kost minder tijd dan kanaal maken.    
 
5. Zodra het programma wordt uitgevoerd, bevestigt dat door rechts te klikken op het programma en om het **afspelen van de programma's** te navigeren en vervolgens te selecteren **met Azure Media Player**afspelen.  
6. Zodra bevestigd, klik met de rechtermuisknop het programma opnieuw en selecteert u **de URL van de uitvoer naar Klembord kopiëren** (of deze gegevens ophalen uit de optie **programma-informatie en instellingen** in het menu). 

De stroom is nu klaar om te worden ingesloten in een speler of verspreid op een doelgroep voor live weergave.  


## <a name="troubleshooting"></a>Het oplossen van problemen

Zie het onderwerp [problemen oplossen](media-services-troubleshooting-live-streaming.md) voor hulp. 


##<a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
