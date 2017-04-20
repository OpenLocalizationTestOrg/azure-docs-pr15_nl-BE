<properties 
    pageTitle="Goede Streaming Plugin voor Open Source Media Framework" 
    description="Informatie over het gebruik van de invoegtoepassing Azure Services vloeiend mediastreaming voor Adobe Open Source Media kader." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Het gebruik van de Microsoft-goede Plugin voor het Framework van Adobe Open Source Media Streaming

##<a name="overview"></a>Overzicht


De invoegtoepassing Microsoft goede Streaming voor Open Source Media Framework 2.0 (SS voor OSMF) breidt de mogelijkheden van de standaard van OSMF en vloeiend Microsoft Streaming inhoud af te spelen toegevoegd aan nieuwe en bestaande OSMF spelers. Goede Streaming afspeelmogelijkheden de invoegtoepassing ook toegevoegd aan stroboscoop Media afspelen (SMP).

SS voor OSMF bevat twee versies van de invoegtoepassing:

- Statische goede Streaming-plugin voor OSMF (SWC)

- Dynamische Streaming van goede plugin voor OSMF (.swf)

Dit document wordt ervan uitgegaan dat de lezer een algemene werkervaring met OSMF en OSMF-plug-ins. Raadpleeg de documentatie van de [officiële OSMF site](http://osmf.org/)voor meer informatie over OSMF.

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Goede Streaming-plugin voor OSMF 2.0

De invoegtoepassing ondersteunt het laden en afspelen van goede Streaming inhoud op aanvraag met de volgende functies:

- Op verzoek goede Streaming afspelen (Play, Pause, Seek, Stop)
- Live Streaming van vloeiend afspelen (Play)
- DVR-functies (onderbreken, zoeken, afspelen van DVR Go-to-Live) Live
- Ondersteuning voor video-codecs - H.264
- Ondersteuning voor Audio-codecs - AAC
- Meerdere taal switchoplossing met ingebouwde OSMF-API 's
- Selectie van max afspelen kwaliteit met ingebouwde OSMF-API 's
- Zijspan closed captioning met de invoegtoepassing voor bijschriften van OSMF
- Adobe&reg; Flash&reg; Player 11,4 of hoger.
- Deze versie ondersteunt alleen OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Ondersteunde functies en bekende problemen

Zie [Dit document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf)voor een volledige lijst van ondersteunde functies, niet-ondersteunde functies en bekende problemen.


## <a name="loading-the-plugin"></a>De invoegtoepassing laden
OSMF Plug-ins kunnen worden geladen (bij compilatie) statisch of dynamisch (tijdens de uitvoering). De invoegtoepassing OSMF downloaden voor soepele Streaming bevat dynamische en statische versies.

- Statische lading: statisch geladen een statische (SWC)-bestand is vereist. Statische Plug-ins worden toegevoegd als een verwijzing naar de projecten en samenvoegen in de definitieve uitvoerbestand bij de compilatie.

- Dynamisch laden: een vooraf gecompileerde bestand (SWF) voor het dynamisch laden is vereist. Dynamische Plug-ins worden in runtime geladen en niet opgenomen in de projectuitvoer. (Gecompileerde uitvoer) Dynamische Plug-ins kunnen worden geladen met behulp van de protocollen HTTP en het bestand.

Zie voor meer informatie over statische en dynamische laden, de officiële [OSMF plugin pagina](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS voor statische lading OSMF
Het onderstaande stukje code laat zien hoe de SS-plugin voor OSMF statisch geladen en spelen een elementaire video met behulp van de klasse OSMF MediaFactory. Zorgen dat de projectverwijzing de statische invoegtoepassing "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc bevat" voor, met inbegrip van de SS OSMF code.

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS voor dynamisch laden van OSMF

Het onderstaande stukje code laat zien hoe de SS-plugin voor OSMF dynamisch laden en afspelen van een basic video met behulp van de klasse OSMF MediaFactory. De dynamische invoegtoepassing 'MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf' kopiëren naar de projectmap als u wilt laden met FILE protocol voordat, met inbegrip van de SS OSMF code of kopiëren onder een webserver voor HTTP-belasting. Er is niet nodig om "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" in de projectverwijzingen.

 
pakket {
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Stroboscoop Media afspelen met de invoegtoepassing voor de dynamische ODMF SS

Goede Streaming voor dynamische OSMF-plugin is compatibel met [Stroboscoop Media afspelen (SMP)](http://osmf.org/strobe_mediaplayback.html). Goede Streaming inhoud af te spelen op SMP toevoegen kunt u de SS voor de invoegtoepassing OSMF. Hiertoe kopieert u "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" onder een webserver voor HTTP laden met behulp van de volgende stappen uit:

1.  Blader op de [installatiepagina stroboscoop Media afspelen](http://osmf.org/dev/2.0gm/setup.html). 
2.  De src ingesteld op een soepele Streaming bron, (bv. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Breng de gewenste configuratiewijzigingen en klik op Voorvertoning bijwerken.
 
    **Opmerking** De inhoud van de webserver moet een geldig bestand crossdomain.xml. 
4.  Kopieer en plak de code in een eenvoudige HTML-pagina met behulp van uw favoriete teksteditor, zoals in het volgende voorbeeld:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Goede Streaming OSMF-invoegtoepassing toevoegen aan de embed code en sla.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  De HTML-pagina opslaan en publiceren naar een webserver. Ga naar de gepubliceerde pagina's met uw favoriete Flash&reg; Player Internet-browser (Internet Explorer, Chrome, Firefox, enzovoort) ingeschakeld.
7.  Geniet van goede Streaming inhoud in Adobe&reg; Flash&reg; Player.

Zie voor meer informatie over de algemene ontwikkeling van de OSMF, de officiële [OSMF ontwikkeling pagina](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Zie ook

[Microsoft Adaptive Streaming Plugin voor OSMF Update](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
