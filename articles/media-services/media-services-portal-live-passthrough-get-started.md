<properties 
    pageTitle="Het uitvoeren van live streaming met lokale encoders met de Azure portal | Microsoft Azure" 
    description="Deze zelfstudie doorloopt u de stappen voor het maken van een kanaal dat is geconfigureerd voor een Pass Through-levering." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Het uitvoeren van live streaming met lokale encoders met de Azure portal

> [AZURE.SELECTOR]
- [Portal]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [REST]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Deze zelfstudie doorloopt u de stappen van het gebruik van de portal Azure een **kanaal** dat is geconfigureerd voor een Pass Through-levering te maken. 

##<a name="prerequisites"></a>Vereisten

De volgende zijn vereist voor het voltooien van de zelfstudie:

- Een Azure-account. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Een Media-Services-account. Zie [het maken van een Media-Services-Account](media-services-portal-create-account.md)maken van een Media-Services-account.
- Een webcam. Bijvoorbeeld: [Telestream Wirecast-encoder](http://www.telestream.net/wirecast/overview.htm).

Het wordt nadrukkelijk aanbevolen om te controleren van de volgende artikelen:

- [Azure mediaservices RTMP ondersteunen en Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Overzicht van Live indien gestoomd met Azure Media Services](media-services-manage-channels-overview.md)
- [Live streaming met lokale encoders die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Gangbare scenario voor live streaming

De volgende stappen beschrijven de taken die betrokken zijn bij het maken van gemeenschappelijke live streaming toepassingen die gebruikmaken van kanalen die zijn geconfigureerd voor Pass Through-levering. Deze zelfstudie laat zien hoe een Pass Through-kanaal en live gebeurtenissen maken en beheren.

1. Een video-camera aansluit op een computer. Starten en configureren van een in de lokalen live encoder dat een multi-RTMP of MP4 gefragmenteerd bitsnelheid oplevert. Zie voor meer informatie [Azure Media Services RTMP-ondersteuning en Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.

1. Maak en start een Pass Through-kanaal.
1. Ophalen van het kanaal URL nemen. 

    De URL ingest wordt gebruikt door het coderingsprogramma live stream verzenden naar het kanaal.
1. De voorvertoning kanaal URL ophalen. 

    Deze URL gebruiken om te controleren of uw kanaal juist de live gegevensstroom ontvangt.

3. Een live evenementenprogramma maken. 

    Wanneer u de portal Azure, maken van een live gebeurtenis wordt ook een actief gemaakt. 
      
    >[AZURE.NOTE]Zorg ervoor dat ten minste één gereserveerde eenheid op de streaming eindpunt waarvoor u inhoud wilt streamen.
1. Het evenementenprogramma start wanneer u klaar bent om te beginnen met streaming en archivering.
2. De live-encoder kan eventueel starten een advertentie worden gesignaleerd. De advertentie wordt in de uitvoerstroom ingevoegd.
1. Het evenementenprogramma stoppen wanneer u wilt stoppen streaming en archivering van de gebeurtenis.
1. Het evenementenprogramma verwijderen (en eventueel verwijderen van de activa).     

>[AZURE.IMPORTANT] Bekijk [Live streaming met lokale encoders die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md) voor meer informatie over concepten en overwegingen ten aanzien van live streaming met lokale encoders en Pass Through-kanalen.

##<a name="to-view-notifications-and-errors"></a>Meldingen en fouten bekijken

Als u weergeven wilt, meldingen en fouten die worden geproduceerd door de portal Azure, klik op het pictogram in het systeemvak.

![Meldingen](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Streaming eindpunten configureren 

Biedt dynamische verpakking, zodat u uw multi-bitrate MP4s leveren in de volgende indelingen voor streaming Media Services: MPEG-streepje, HLS, soepele Streaming of harde schijven, zonder dat u hoeft om te verpakken in deze indelingen streamen. Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in de opslagindeling in één en Media Services maakt en het juiste antwoord op basis van aanvragen van een client fungeert.

Als u wilt profiteren van dynamische verpakking, moet u ten minste één streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen.  

Maken en wijzigen van het aantal gereserveerde eenheden streaming, doet u het volgende:

1. Aanmelden op de [portal Azure](https://portal.azure.com/).
1. Klik in het venster **Instellingen voor** **Streaming eindpunten**. 

2. Klik op het eindpunt streaming standaard. 

    Het venster **Std. STREAMING EINDPUNTDETAILS** weergegeven.

3. Als u het aantal eenheden streaming, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming-eenheden](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van alle nieuwe eenheden kan maximaal 20 minuten in beslag nemen.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Maak en start Pass Through-kanalen en gebeurtenissen

Een kanaal is gekoppeld aan gebeurtenissen/programma's waarmee u de publicatie en de opslag van segmenten in een live gegevensstroom. Kanalen beheren voor gebeurtenissen. 
    
U kunt het aantal uren dat u bewaren van de opgenomen inhoud van het programma wilt door het instellen van de lengte van het **Archief venster** opgeven. Deze waarde kan worden ingesteld van een minimum van 5 minuten tot een maximum van 25 uur. Archief venster lengte bepaalt ook dat de maximale hoeveelheid tijd clients kunt zoeken in de tijd vanaf de huidige positie van de levende. Gebeurtenissen kunnen uitvoeren via de opgegeven hoeveelheid tijd, maar de inhoud achter de lengte van het venster wordt voortdurend genegeerd. Deze waarde van deze eigenschap bepaalt ook hoe lang de manifesten client te vergroten.

Elke gebeurtenis is gekoppeld aan een actief. U moet een OnDemand locator voor de gekoppelde activa maken voor het publiceren van de gebeurtenis. Deze locator kunt u een streaming URL die u aan uw klanten bieden kunt maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve gebeurtenissen zodat u meerdere archieven van dezelfde binnenkomende gegevensstroom kunt maken. Hiermee kunt u publiceren en archiveren van verschillende delen van een gebeurtenis als nodig is. Bijvoorbeeld, is uw behoeften voor het archiveren van 6 uur van een programma, maar alleen de laatste 10 minuten wordt uitgezonden. Om dit te bereiken, moet u twee gelijktijdig actieve programma's te maken. Een programma voor het archiveren van 6 uur van de gebeurtenis is ingesteld, maar wordt niet gepubliceerd. Het andere programma is ingesteld voor het archiveren van 10 minuten en dit programma wordt gepubliceerd.

U moet niet opnieuw gebruiken bestaande live gebeurtenissen. In plaats daarvan maakt en start een nieuwe gebeurtenis voor elke gebeurtenis.

Start de gebeurtenis wanneer u klaar bent om te beginnen met streaming en archivering. Het programma stopt wanneer u wilt stoppen streaming en archivering van de gebeurtenis. 

Gearchiveerde inhoud verwijderen, als u wilt stoppen en de gebeurtenis verwijderen en verwijder vervolgens de gekoppelde activa. Een activum kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis; de gebeurtenis moet eerst worden verwijderd. 

Nadat u stoppen en de gebeurtenis verwijderen, zijn de gebruikers zou kunnen uw gearchiveerde inhoud streamen als video op aanvraag, voor, als u het activum niet verwijderen.

Als u wilt de gearchiveerde inhoud behouden, maar niet beschikbaar voor streaming, verwijdert u de streaming-locator.

###<a name="to-use-the-portal-to-create-a-channel"></a>De portal gebruiken om een kanaal te maken 

In dit gedeelte ziet u hoe u de optie **Snelle invoer** gebruiken om een Pass Through-kanaal te maken.

Zie voor meer informatie over Pass Through-kanalen, [Live streaming met lokale encoders die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md).

1. Selecteer in de [Azure portal](https://portal.azure.com/)uw Azure Media Services-account.
2. Klik in het venster **Instellingen** op **Live streaming**. 

    ![Aan de slag](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Het venster **Live streaming** verschijnt.

3. Klik op de **Snelle invoer** wilt maken van een Pass Through-kanaal met de RTMP protocol nemen.

    Het **Nieuwe kanaal maken** verschijnt.
4. Het nieuwe kanaal een naam en klik op **maken**. 

    Hiermee maakt u een Pass Through-kanaal met de RTMP protocol nemen.

##<a name="create-events"></a>Gebeurtenissen maken

1. Selecteer een kanaal dat u wilt toevoegen van een gebeurtenis.
2. Druk op de knop **Live gebeurtenis** .

![Gebeurtenis](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Get consumptie van URL 's

Wanneer het kanaal is gemaakt, kunt u een consumptie van URL's die u naar de live encoder biedt. Het coderingsprogramma gebruikt deze URL's invoeren van een live gegevensstroom.

![Gemaakt](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>De gebeurtenis te bekijken

De gebeurtenis bekijken, klikt u op **controle** in Azure portal of de streaming URL kopiëren en gebruiken van een speler van uw keuze. 
 
![Gemaakt](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Live-gebeurtenis worden automatisch geconverteerd naar inhoud op aanvraag wanneer gestopt.

##<a name="clean-up"></a>Opschonen

Zie voor meer informatie over Pass Through-kanalen, [Live streaming met lokale encoders die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md).

- Een kanaal kan worden gestopt wanneer alle gebeurtenissen/programma's op het kanaal zijn gestopt.  Nadat het kanaal is gestopt, wordt deze niet eventuele kosten oplopen. Wanneer u opnieuw start, heeft dit dezelfde URL te nemen zodat u niet hoeft te configureren van de encoder.
- Een kanaal kan alleen worden verwijderd als alle live gebeurtenissen op het kanaal is verwijderd.

##<a name="view-archived-content"></a>Gearchiveerde inhoud weergeven

Nadat u stoppen en de gebeurtenis verwijderen, zijn de gebruikers zou kunnen uw gearchiveerde inhoud streamen als video op aanvraag, voor, als u het activum niet verwijderen. Een activum kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis; de gebeurtenis moet eerst worden verwijderd. 

Uw activa beheren, selecteer **instelling** en klik op **activa**.

![Activa](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
