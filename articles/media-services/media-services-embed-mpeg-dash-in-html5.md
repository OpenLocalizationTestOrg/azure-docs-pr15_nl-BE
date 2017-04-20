<properties 
    pageTitle="Een MPEG-streepje adaptieve Streaming Video insluiten in een toepassing HTML5 met DASH.js | Microsoft Azure" 
    description="Dit onderwerp wordt beschreven hoe u een MPEG-streepje adaptieve Streaming Video insluiten in een toepassing HTML5 met DASH.js." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Een MPEG-streepje adaptieve Streaming Video insluiten in een toepassing HTML5 met DASH.js

##<a name="overview"></a>Overzicht

MPEG-streepje is een ISO-norm voor adaptieve streaming van video-inhoud, die aanzienlijke voordelen voor degenen die wensen biedt te leveren van hoge kwaliteit en geavanceerde video streaming output. Met MPEG-streepje, wordt de video-stream automatisch neerzetten in de definitie van een lager als het netwerk overbelast raakt. Hierdoor verkleint u de kans van de viewer een 'onderbroken' video bekijken terwijl de speler de volgende paar seconden om af te spelen (aka bufferen) wordt gedownload. Bij opstoppingen in het netwerk wordt beperkt, wordt op zijn beurt de videospeler terug naar een hogere kwaliteit stream. Deze mogelijkheid om aan te passen aan de vereiste bandbreedte leidt ook tot een snellere start video. Dat betekent dat de eerste paar seconden kunnen worden afgespeeld in een lagere kwaliteit segment voor snel te downloaden en vervolgens stap tot een hogere kwaliteit eenmaal voldoende inhoud is in buffer geplaatst.

Dash.js is een open-source MPEG-streepje videospeler geschreven in JavaScript. Doelstelling is om een robuuste, cross-platform-speler die vrij kan worden hergebruikt in toepassingen waarvoor het afspelen van video. Het biedt afspelen van MPEG-streepje in elke browser die vandaag W3C Media Source Extensions (MSE) ondersteunt chroom, Microsoft Edge en IE11 (andere browsers hebben aangegeven dat hun bedoelingen ter ondersteuning van MSE). Voor meer informatie over DASH.js Zie js de opslagplaats van GitHub dash.js.


##<a name="creating-a-browser-based-streaming-video-player"></a>Maken van een browser gebaseerde streaming video-speler

U maakt een eenvoudige pagina met een video-speler met de verwachte besturingselementen die een afspelen, onderbreken, terugspoelen, enzovoort, moet u:

1. Een HTML-pagina maken
1. De video-tag toevoegen
1. De speler dash.js toevoegen
1. Windows media player initialiseren
1. Bepaalde CSS-stijl toevoegen
1. De resultaten bekijken in een browser die MSE implementeert

Het initialiseren van de speler worden voltooid in enkele van de regels van de JavaScript-code. Met dash.js, echt is eenvoudig om MPEG-streepje video insluiten in uw browser gebaseerde toepassingen.

##<a name="creating-the-html-page"></a>De HTML-pagina maken

De eerste stap is het maken van een standaard HTML-pagina met de **video** -element, dit bestand opslaan als basicPlayer.html, zoals in het volgende voorbeeld wordt geïllustreerd:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>DASH.js Windows Media Player toe te voegen

Om de referentie-implementatie van dash.js aan de toepassing toevoegt, moet u Pak het bestand dash.all.js van de 1.0 release van project dash.js. Dit moet worden opgeslagen in de map JavaScript van uw toepassing. Dit bestand is een bestand met gemak dat alle benodigde dash.js code in één bestand verzamelt. Hebt u een uiterlijk rond de dash.js-bibliotheek, wordt u de afzonderlijke bestanden te zoeken, test code en nog veel meer, maar als u wilt doen gebruik dash.js, dan het bestand dash.all.js is wat u nodig hebt.

Toevoegen om de speler dash.js aan uw toepassingen toevoegen, een scriptcode aan de head-sectie van basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Vervolgens maakt u een functie in om de speler te initialiseren wanneer de pagina wordt geladen. Het volgende script toevoegen na de regel waarin u dash.all.js wilt laden:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Deze functie maakt eerst een DashContext. Dit wordt gebruikt voor het configureren van de toepassing voor een specifieke runtimeomgeving. Uit technisch oogpunt worden de klassen die de afhankelijkheid injectie kader moet worden gebruikt bij het construeren van de toepassing gedefinieerd. In de meeste gevallen gebruikt u Dash.di.DashContext.

Vervolgens de primaire klasse van het dash.js kader, MediaPlayer wordt gestart. Deze klasse bevat de belangrijkste methoden die nodig zijn, zoals afspelen en onderbreken, beheert de relatie met de video-element en beheert ook de interpretatie van de Media presentatie beschrijving (MPD)-bestand waarin de video wordt afgespeeld.

De functie startup() van de klasse MediaPlayer wordt aangeroepen om ervoor te zorgen dat de speler kan de video afspelen. Onder andere deze functie zorgt ervoor dat alle noodzakelijke klassen (zoals gedefinieerd door de context) zijn geladen. Zodra de speler gereed is, kunt u het video element koppelen aan met behulp van de functie attachView(). Hierdoor kunnen de MediaPlayer aan het element van de video-stream injecteren en ook het afspelen indien nodig.

De URL van de MPD-bestand aan de MediaPlayer doorgeven zodat deze op de hoogte van de video dat naar verwachting af te spelen. De setupVideo() functie hebt gemaakt, moet worden uitgevoerd nadat de pagina volledig is geladen. Dit doet u met behulp van de gebeurtenis onload van het body-element. Wijzigen uw <body> element:

    <body onload="setupVideo()">

Ten slotte stelt u de grootte van de video-element met CSS. In een adaptieve streaming-omgeving is dit vooral belangrijk omdat de grootte van de video wordt afgespeeld tijdens het afspelen wordt aangepast aan wisselende netwerkomstandigheden kan veranderen. Deze eenvoudige demonstratie te dwingen het video element aan 80% van de beschikbare browservenster worden door de volgende CSS-regels toe te voegen aan de sectie head van de pagina:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Afspelen van een Video

Als u een video afspeelt, de browser het bestand basicPlayback.html en klik op afspelen op de videospeler weergegeven.


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook

[Video player-toepassingen ontwikkelen](media-services-develop-video-players.md)

[Opslagplaats van GitHub dash.js](https://github.com/Dash-Industry-Forum/dash.js) 
