<properties 
    pageTitle="Het uitvoeren van live streaming Azure Media Services gebruiken voor het maken van multi-bitrate streams met Azure portal | Microsoft Azure" 
    description="Deze zelfstudie doorloopt u de stappen voor het maken van een kanaal dat u ontvangt een live gegevensstroom één bitsnelheid en op de Azure portal met multi-bitsnelheid wordt gecodeerd." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
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


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Het uitvoeren van live streaming multi-bitrate streams maken met Azure portal met Azure Media Services

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Deze zelfstudie doorloopt u de stappen voor het maken van een **kanaal** dat een live gegevensstroom één bitsnelheid ontvangt en op meerdere bitsnelheid wordt gecodeerd.

>[AZURE.NOTE]Zie voor meer conceptuele informatie over kanalen die zijn ingeschakeld voor live-codering, [Live streaming met Media Services Azure multi-bitrate streams maken](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Gangbare Scenario voor Live Streaming

Hieronder volgen de algemene stappen die betrokken zijn bij het maken van gemeenschappelijke live streaming toepassingen.

>[AZURE.NOTE] Momenteel is de maximale aanbevolen duur van een live gebeurtenis 8 uur. Neem contact op met amslived op Microsoft.com als u nodig hebt voor het uitvoeren van een kanaal voor een langere periode.

1. Een video-camera aansluit op een computer. Starten en configureren van een live op gebouwen-encoder die een enkele bitsnelheid in een van de volgende protocollen: RTMP, soepele Streaming of RTP (MPEG-TS). Zie voor meer informatie [Azure Media Services RTMP-ondersteuning en Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.

1. Maak en start een kanaal. 

1. Ophalen van het kanaal URL nemen. 

    De URL ingest wordt gebruikt door het coderingsprogramma live stream verzenden naar het kanaal.
1. De voorvertoning kanaal URL ophalen. 

    Deze URL gebruiken om te controleren of uw kanaal juist de live gegevensstroom ontvangt.

3. Maak een gebeurtenis/programma (maakt ook een actief). 
1. De gebeurtenis (dat maakt een OnDemand locator voor de gekoppelde activa) publiceren.  

    Zorg ervoor dat ten minste één gereserveerde eenheid op de streaming eindpunt waarvoor u inhoud wilt streamen.
1. Start de gebeurtenis wanneer u klaar bent om te beginnen met streaming en archivering.
2. De live-encoder kan eventueel starten een advertentie worden gesignaleerd. De advertentie wordt in de uitvoerstroom ingevoegd.
1. Stop de gebeurtenis wanneer u wilt stoppen streaming en archivering van de gebeurtenis.
1. De gebeurtenis verwijderen (en eventueel verwijderen van de activa).   

##<a name="in-this-tutorial"></a>In deze zelfstudie

In deze zelfstudie wordt de Azure portal gebruikt om de volgende taken uitvoeren: 

2.  Streaming eindpunten configureren.
3.  Maak een kanaal dat is ingeschakeld voor live-codering.
1.  De consumptie van URL ophalen om live encoder verstrekken. De live encoder gebruikt deze URL om de stroom in het kanaal te nemen. .
1.  Een evenementenprogramma (en een actief) maken
1.  Publiceren van de activa en streaming URL's ophalen  
1.  Uw inhoud afspelen 
2.  Opruimen

##<a name="prerequisites"></a>Vereisten

De volgende zijn vereist voor het voltooien van de zelfstudie.

- Als u deze zelfstudie hebt voltooid, moet u een account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/).
- Een Media-Services-account. Zie maken van een Media-Services-account, [Account maken](media-services-portal-create-account.md).
- Een webcam en een coderingsprogramma dat een enkele live bitsnelheid kunt verzenden.

##<a name="configure-streaming-endpoints"></a>Streaming eindpunten configureren 

Biedt dynamische verpakking waarmee u uw multi-bitrate MP4s leveren in de volgende indelingen voor streaming Media Services: MPEG-streepje, HLS, soepele Streaming of harde schijven, zonder dat u hoeft pakket opnieuw opslaan in deze indelingen streamen. Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in één opslagmedium en Media Services zal maken en het juiste antwoord op basis van aanvragen van een client fungeren.

Als u wilt profiteren van dynamische verpakking, moet u ten minste één streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen.  

Maken en wijzigen van het aantal gereserveerde eenheden streaming, doet u het volgende:

1. Log in op de [Azure portal](https://portal.azure.com/) en selecteer uw account AMS.
1. Klik in het venster **Instellingen voor** **Streaming eindpunten**. 

2. Klik op het eindpunt streaming standaard. 

    Het venster **Std. STREAMING EINDPUNTDETAILS** weergegeven.

3. Als u het aantal eenheden streaming, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming-eenheden](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van alle nieuwe eenheden kan maximaal 20 minuten in beslag nemen.

##<a name="create-a-channel"></a>Een kanaal maken

1. In de [portal Azure](https://portal.azure.com/)Media Services selecteren en klikt u vervolgens op de naam van uw Media Services.
2. Selecteer **Live Streaming**.
3. Selecteer **aangepast**. Met deze optie kunt u een kanaal dat is ingeschakeld voor live-codering maken.

    ![Een kanaal maken](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Klik op **Instellingen**.
    
    1.  Kies het type **Codering Live** kanaal. Dit type geeft aan dat u maken van een kanaal dat is ingeschakeld wilt voor live-codering. Dit betekent dat de binnenkomende één bitsnelheid wordt verzonden naar het kanaal en gecodeerd in een multi-bitsnelheid met opgegeven live encoder instellingen. Zie [Live streaming met Media Services Azure multi-bitrate streams maken](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie. Klik op OK.
    2. Geef de naam van een kanaal.
    3. Klik op OK onder aan het scherm.
    
5. Selecteer het tabblad **Ingest** .

    1. Op deze pagina kunt u een streaming protocol. Voor het kanaaltype **Live codering** zijn geldige protocolopties:
        
        - Enkele bitsnelheid Fragmented MP4 (goede Streaming)
        - Enkele bitsnelheid RTMP
        - RTP (MPEG-TS): MPEG-2 transportstream via RTP.
        
        Zie voor gedetailleerde uitleg over elk protocol [Live streaming met Media Services Azure multi-bitrate streams maken](media-services-manage-live-encoder-enabled-channels.md).
    
        U kunt de protocoloptie bij het kanaal niet wijzigen of de bijbehorende gebeurtenissen/programma's worden uitgevoerd. Als u andere protocollen nodig hebt, moet u de afzonderlijke kanalen voor elk streaming protocol maken.  

    2. U kunt IP-beperkingen toepassen op de ingest. 
    
        Hier kunt u de IP-adressen die tot het nemen van een video op dit kanaal. Toegestane IP-adressen kunnen worden opgegeven als een enkel IP-adres (bijvoorbeeld ' 10.0.0.1"), een IP-adresbereik met een IP-adres en een subnetmasker CIDR (bijvoorbeeld ' 10.0.0.1/22') of een IP-adresbereik met een IP-adres en een subnetmasker voor het gestippelde decimale (b.v." 10.0.0.1(255.255.252.0)').

        Als er geen IP-adressen zijn opgegeven en er is geen regeldefinitie wordt geen IP-adres worden toegestaan. Als u wilt dat elk IP-adres, een regel maken en stel 0.0.0.0/0.

6. Op het tabblad **voorbeeld** IP-beperking voor de voorvertoning van toepassing.
7. Geef de voorinstelling voor codering op het tabblad **codering** . 

    Op dit moment het enige systeem voorinstelling kunt u selecteren is **standaard de 720 p**. Als u een aangepaste voorinstelling, opent u een Microsoft support ticket. Voer vervolgens de naam van de voorinstelling voor u gemaakt. 

>[AZURE.NOTE] Op dit moment tot 30 minuten kan duren voordat het begin van het kanaal. Kanaal opnieuw instellen kan 5 minuten duren.

U kunt als u het kanaal hebt gemaakt, klikt u op het kanaal en selecteer **Instellingen** , waarin u uw kanalen configuraties kunt weergeven. 

Zie [Live streaming met Media Services Azure multi-bitrate streams maken](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.


##<a name="get-ingest-urls"></a>Get consumptie van URL 's

Wanneer het kanaal is gemaakt, kunt u een consumptie van URL's die u naar de live encoder biedt. Het coderingsprogramma gebruikt deze URL's invoeren van een live gegevensstroom.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Maken en beheren van gebeurtenissen

###<a name="overview"></a>Overzicht

Een kanaal is gekoppeld aan gebeurtenissen/programma's waarmee u de publicatie en de opslag van segmenten in een live gegevensstroom. Kanalen beheren gebeurtenissen/programma's. De kanaal- en relatie is vergelijkbaar met de traditionele media, waarbij een kanaal is een constante stroom van inhoud en een programma is binnen het bereik van sommige getimede gebeurtenis op dat kanaal.

U kunt het aantal uren dat u bewaren van de opgenomen inhoud voor de gebeurtenis wilt door het instellen van de lengte van het **Archief venster** opgeven. Deze waarde kan worden ingesteld van een minimum van 5 minuten tot een maximum van 25 uur. Archief venster lengte bepaalt ook dat de maximale hoeveelheid tijd clients kunt zoeken in de tijd vanaf de huidige positie van de levende. Gebeurtenissen kunnen uitvoeren via de opgegeven hoeveelheid tijd, maar de inhoud achter de lengte van het venster wordt voortdurend genegeerd. Deze waarde van deze eigenschap bepaalt ook hoe lang de manifesten client te vergroten.

Elke gebeurtenis is gekoppeld aan een actief. U moet een OnDemand locator voor de gekoppelde activa maken voor het publiceren van de gebeurtenis. Met deze locator kunt u een streaming URL die u aan uw klanten bieden kunt maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve gebeurtenissen zodat u meerdere archieven van dezelfde binnenkomende gegevensstroom kunt maken. Hiermee kunt u publiceren en archiveren van verschillende delen van een gebeurtenis als nodig is. Bijvoorbeeld, is uw behoeften voor het archiveren van 6 uur van een gebeurtenis, maar alleen de laatste 10 minuten wordt uitgezonden. Om dit te bereiken, moet u twee gelijktijdig uitgevoerd gebeurtenis. Een gebeurtenis is ingesteld voor het archiveren van 6 uur van de gebeurtenis, maar wordt niet gepubliceerd. De andere gebeurtenis is ingesteld voor het archiveren van 10 minuten en dit programma wordt gepubliceerd.

U moet niet opnieuw gebruiken voor bestaande programma's voor nieuwe gebeurtenissen. In plaats daarvan maakt en start een nieuw programma voor elke gebeurtenis.

Een evenementenprogramma start wanneer u klaar bent om te beginnen met streaming en archivering. Stop de gebeurtenis wanneer u wilt stoppen streaming en archivering van de gebeurtenis. 

Gearchiveerde inhoud verwijderen, als u wilt stoppen en de gebeurtenis verwijderen en verwijder vervolgens de gekoppelde activa. Een activum kan niet worden verwijderd als het wordt gebruikt door de gebeurtenis; de gebeurtenis moet eerst worden verwijderd. 

Nadat u stoppen en de gebeurtenis verwijderen, zijn de gebruikers zou kunnen uw gearchiveerde inhoud streamen als video op aanvraag, voor, als u het activum niet verwijderen.

Als u wilt de gearchiveerde inhoud behouden, maar niet beschikbaar voor streaming, verwijdert u de streaming-locator.

###<a name="createstartstop-events"></a>Gebeurtenissen maken/starten/stoppen

Zodra u de stroom stroomt in het kanaal kunt u streaming kan beginnen door te maken van een actief, het programma en de Streaming-Locator. Dit zal de stroom archiveren en beschikbaar maken voor gebruikers via de Streaming-eindpunt. 

Er zijn twee manieren om gebeurtenissen: 

1. Druk op **Live-gebeurtenis** om een nieuwe gebeurtenis toevoegen vanaf de pagina **kanalen** .

    Opgeven: naam van de gebeurtenis, Activumnaam venster archief en optie codering.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Als u **deze live gebeurtenis nu publiceren** is ingeschakeld, wordt de gebeurtenis de publicatie-URL's gemaakt.
    
    Drukt u op **Start**, wanneer u klaar bent om te streamen van de gebeurtenis.

    Als u de gebeurtenis start, drukt u op **bekijken** om te starten met het afspelen van de inhoud.

2. U kunt ook een snelkoppeling en druk op **Go Live** op de pagina **kanalen** . Hiermee maakt u een standaard actief, programma- en Streaming-Locator.

    De gebeurtenis heeft **standaard** de naam en het venster archief is ingesteld op 8 uur.

U kunt de gepubliceerde gebeurtenis van het **Live-evenement** pagina bekijken. 

Als u op **Uit lucht**, stopt alle live gebeurtenissen. 


##<a name="watch-the-event"></a>De gebeurtenis te bekijken

De gebeurtenis bekijken, klikt u op **controle** in Azure portal of de streaming URL kopiëren en gebruiken van een speler van uw keuze. 
 
![Gemaakt](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Live-gebeurtenis omgezet automatisch in gebeurtenissen inhoud op aanvraag wanneer gestopt.

##<a name="clean-up"></a>Opschonen

Als u klaar bent streaming gebeurtenissen en voor het opschonen van de bronnen die eerder is ingericht, kunt u de volgende procedure volgen.

- Stop de gegevensstroom van het coderingsprogramma te duwen.
- Stop het kanaal. Nadat het kanaal is gestopt, wordt deze niet eventuele kosten oplopen. Wanneer u opnieuw start, heeft dit dezelfde URL te nemen zodat u niet hoeft te configureren van de encoder.
- U kunt uw eindpunt Streaming stoppen, tenzij u doorgaan wilt met het bieden van het archief van uw live-gebeurtenis als een gegevensstroom op aanvraag. Als het kanaal gestopt is, wordt deze niet eventuele kosten oplopen.
  
##<a name="view-archived-content"></a>Gearchiveerde inhoud weergeven

Nadat u stoppen en de gebeurtenis verwijderen, zijn de gebruikers zou kunnen uw gearchiveerde inhoud streamen als video op aanvraag, voor, als u het activum niet verwijderen. Een activum kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis; de gebeurtenis moet eerst worden verwijderd. 

Uw activa beheren, selecteer **instelling** en klik op **activa**.

![Activa](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Overwegingen met betrekking tot

- Momenteel is de maximale aanbevolen duur van een live gebeurtenis 8 uur. Neem contact op met amslived op Microsoft.com als u nodig hebt voor het uitvoeren van een kanaal voor een langere periode.
- Zorg ervoor dat ten minste één gereserveerde eenheid op de streaming eindpunt waarvoor u inhoud wilt streamen.


##<a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
