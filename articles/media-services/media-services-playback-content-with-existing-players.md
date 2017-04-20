<properties 
    pageTitle="Afspelen van de inhoud van uw | Microsoft Azure" 
    description="Dit onderwerp bevat een overzicht van de bestaande spelers die u voor het afspelen van uw inhoud gebruiken kunt." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Afspelen van uw inhoud met bestaande spelers

Azure Media Services biedt ondersteuning voor veel populaire streaming-indelingen, zoals goede Streaming HTTP Live Streaming en MPEG-streepje. Dit onderwerp verwijst naar bestaande spelers die u gebruiken kunt voor het testen van uw stromen.

>[AZURE.NOTE]Play dynamisch verpakt of dynamisch gecodeerde inhoud, zorg ervoor dat u ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt uw inhoud te leveren. Zie voor meer informatie over het schalen van streaming-eenheden: [eenheden streaming schaalaanpassing](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure portal Media Services inhoud Windows media player

De portal **Azure** biedt een content-speler die u gebruiken kunt voor het testen van uw video.

Klik op de gewenste video (Zorg ervoor dat deze is [gepubliceerd](media-services-portal-publish.md)) en klik op de knop **afspelen** onderaan in de portal.

Sommige overwegingen zijn van toepassing:

- De **Diensten inhoud MEDIASPELER** speelt uit het standaardbeleid voor streaming eindpunt. Als u afspelen vanaf een niet-standaard streaming eindpunt wilt, gebruikt u een andere speler. Bijvoorbeeld: [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure MediaPlayer

[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) gebruiken voor het afspelen van inhoud (wissen of beveiligde) in een van de volgende indelingen:

- Goede Streaming
- MPEG-STREEPJE
- HLS
- Progressieve MP4


###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>AES gecodeerd met Token

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight-spelers

####<a name="monitoring"></a>Monitoring

[http://SMF.cloudapp.NET/HealthMonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady met Token

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>STREEPJE-spelers

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Andere

HLS-URL's die u kunt testen:

- **Safari** op een iOS-apparaat of
- **3ivx HLS Player** op Windows.

##<a name="developing-video-players"></a>Videospelers ontwikkelen

Zie voor informatie over het ontwikkelen van uw eigen spelers [ontwikkelingslanden videospelers](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
