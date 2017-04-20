<properties 
    pageTitle="De gids voor probleemoplossing voor live streaming | Microsoft Azure" 
    description="Dit onderwerp biedt suggesties voor het oplossen van problemen met live streaming." 
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
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Problemen oplossen met live streaming

Dit onderwerp biedt suggesties voor het oplossen van bepaalde problemen met live streaming.

## <a name="issues-related-to-on-premises-encoders"></a>Problemen met betrekking tot encoders voor gebouwen 

Deze sectie vindt u suggesties voor het oplossen van problemen met betrekking tot encoders voor ruimten die zijn geconfigureerd voor het verzenden van een enkele bitsnelheid op AMS kanalen die zijn ingeschakeld voor live-codering.

###<a name="problem-would-like-to-see-logs"></a>Probleem: Wil graag Zie Logboeken 

- **Probleem**: kan niet zoeken encoder die zich kan helpen bij het debuggen van problemen.
    
    - **Telestream Wirecast**: meestal vindt u logboekbestanden in C:\Users\{gebruikersnaam} \AppData\Roaming\Wirecast\ 
    - **Elementair Live**: vindt u koppelingen naar Logboeken op de portal management heeft. Klik op **Statistieken**en vervolgens op **Logboeken**. Op de pagina **Log Files** ziet u een lijst van Logboeken voor alle LiveEvent items; Selecteer de overeenkomt met uw huidige sessie. 
    - **Flash Media Live Encoder**: vindt u de **Logboekmap...** door te navigeren naar het tabblad **Logboek codering** .
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Probleem: Er is geen optie voor het uitvoeren van een progressieve stroom

- **Probleem**: het coderingsprogramma wordt gebruikt niet automatisch de interliniëring ongedaan maken. 

    **Stappen voor probleemoplossing**: zoek een optie ongedaan ineengestrengeld binnen de interface van het coderingsprogramma. Eenmaal de-interlacing is ingeschakeld, controleert u nogmaals progressieve uitvoerinstellingen. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Probleem: Verschillende encoder uitvoerinstellingen geprobeerd en nog steeds geen verbinding maken. 

- **Probleem**: Azure codering kanaal is niet correct opnieuw ingesteld. 

    **Stappen voor probleemoplossing**: Zorg ervoor dat het coderingsprogramma niet langer pusht naar AMS, stoppen en opnieuw instellen van het kanaal. Wanneer u opnieuw probeert de encoder verbinden met de nieuwe instellingen. Als dit nog niet het probleem wordt opgelost, probeert u een nieuw kanaal volledig te maken, soms kanalen beschadigd kunnen raken na verschillende mislukte pogingen.  

- **Probleem**: de GOP grootte of Sleutelframe-instellingen niet optimaal zijn. 

    **Stappen voor probleemoplossing**: GOP aanbevolen grootte of hoofdframe interval is 2 seconden. Sommige encoders berekenen van deze instelling in het aantal frames, terwijl anderen seconden. Bijvoorbeeld: bij het uitvoeren van 30fps, de grootte van de GOP is 60 frames die gelijk is aan 2 seconden.  
     
- **Probleem**: de stroom gesloten poorten blokkeren. 

    **Stappen voor probleemoplossing**: Controleer wanneer streaming via RTMP, firewall en proxy-instellingen om te bevestigen dat de uitgaande poort 1935 en 1936 geopend zijn. Wanneer u streaming RTP, bevestigt u dat uitgaande poort 2010 geopend is. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Probleem: Bij het configureren van de encoder naar stroom met het RTP-protocol, er is geen plaats voor een hostnaam opgeeft. 

- **Probleem**: veel RTP-encoders niet toestaan voor hostnamen en IP-adres moet worden verkregen.  

    **Stappen voor probleemoplossing**: zoekt het IP-adres, open een opdrachtprompt op de computer. Hiertoe in Windows opent u het venster uitvoeren starten (WIN + R) en typ 'cmd' te openen.  

    Nadat u de opdrachtprompt wordt geopend, typt u 'Ping [AMS hostnaam]'. 

    De hostnaam kan worden afgeleid door het weglaten van het poortnummer van de Azure consumptie van URL, zoals gemarkeerd in het volgende voorbeeld: 

    RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Probleem: Kan afspelen van de gepubliceerde stroom.
 
- **Probleem**: Er is geen eindpunt Streaming is uitgevoerd, of er is geen streaming eenheden schaal toegewezen. 

    **Stappen voor probleemoplossing**: Ga naar het tabblad 'Streaming eindpunt' in het hulpprogramma AMSE en er is een eindpunt Streaming met één eenheid voor streaming bevestigen. 
    


>[AZURE.NOTE] Als na het volgen van de stappen die u nog steeds niet correct streamen, dienen een support ticket via de portal Azure.

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
