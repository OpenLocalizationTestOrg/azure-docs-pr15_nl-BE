<properties 
    pageTitle="Android webweergave Bridge met native Mobile Engagement Android SDK" 
    description="Hierin wordt beschreven hoe u een brug maken tussen Javascript en de native Android SDK voor Mobile Engagement met webweergave"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Android webweergave Bridge met native Mobile Engagement Android SDK

> [AZURE.SELECTOR]
- [Android-brug](mobile-engagement-bridge-webview-native-android.md)
- [iOS-brug](mobile-engagement-bridge-webview-native-ios.md)

Sommige mobiele toepassingen zijn ontworpen als een hybride app waar de app zelf is ontwikkeld met behulp van native Android development maar enkele of zelfs alle schermen worden weergegeven in de webweergave van een Android. U kunt nog steeds Android SDK van Mobile Engagement binnen deze apps verbruiken en in deze zelfstudie wordt beschreven hoe om te gaan hierover. De volgende voorbeeldcode is gebaseerd op de Android-documentatie [hier](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Hierin wordt beschreven hoe deze gedocumenteerde benadering kan worden gebruikt voor het implementeren van hetzelfde voor Mobile Engagement Android SDK van veelgebruikte methoden, dat een webweergave van een hybride app aanvragen voor het bijhouden, gebeurtenissen, taken, fouten, app info ook starten kunt terwijl ze via onze Android SDK sluizen. 

1. Ten eerste moet u ervoor zorgen dat u onze [zelfstudie aan de slag](mobile-engagement-android-get-started.md) voor de integratie van de Android SDK van Mobile Engagement in uw hybride app hebt doorlopen. Zodra u doen door uw `OnCreate` methode het volgende zal uitzien.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Nu controleren of uw hybride app heeft een scherm met een webweergave erop. De code is vergelijkbaar met het volgende waar wij een lokaal HTML laadt-bestand **Sample.html** in de webweergave in de `onCreate` methode van het scherm. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Maak nu een bridge bestand met de naam **WebAppInterface** die Hiermee maakt u een wrapper voor enkele veelgebruikte mobiele betrokkenheid bij de Android SDK methoden met de `@JavascriptInterface` aanpak is beschreven in de [documentatie voor Android](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Nadat we het bovenstaande bridge-bestand hebt gemaakt, moeten we ervoor zorgen dat deze gekoppeld aan onze webweergave is. Om dit te gebeuren, moet u uw `SetWebview` methode zodat deze er als volgt uit:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. In het bovenstaande fragment we genoemd `addJavascriptInterface` onze klasse bridge koppelen aan onze webweergave en ook een ingang genoemd **EngagementJs** om de methoden aanroepen vanuit bridge-bestand gemaakt. 

6. Nu het volgende bestand genaamd **Sample.html** in uw project in een map met de naam van de **activa** die in de webweergave wordt geladen en wanneer noemen we de methoden van de bridge-bestand maken.

        <!doctype html>
        <html>
            <head>
                <style type='text/css'>
                    html { font-family:Helvetica; color:#222; }
                    h1 { color:steelblue; font-size:22px; margin-top:16px; }
                    h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
                </style>
        
                <script type="text/javascript">
        
                    window.onerror = function(err)
                    {
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Houd rekening met de volgende punten met betrekking tot de bovenstaande HTML-bestand:

    -   Het bevat een aantal invoervakken waar u gegevens moet worden gebruikt als namen voor de gebeurtenis, taak, fout, AppInfo kan bieden. Wanneer u op de knop ernaast klikt, een aanroep naar de Javascript die uiteindelijk de methoden vanuit het bestand bridge aanroept te geven dit gesprek naar mobiele betrokkenheid bij de Android SDK. 
    -   We zijn labels op enkele statische extra info voor gebeurtenissen, projecten en zelfs fouten om te demonstreren hoe dit kan worden gedaan. Deze extra info als het een JSON-tekenreeks die wordt verzonden als u kijkt de `WebAppInterface` -bestand, wordt geparseerd en geplaatst in een Android `Bundle` en met het verzenden van gebeurtenissen, taken, fouten wordt doorgegeven. 
    -   Een mobiele betrokkenheid bij de taak wordt gestart met de naam die u opgeeft in het invoervak uitgevoerd gedurende 10 seconden en sluit af. 
    -   Een Mobile Engagement appinfo of tag wordt met "customer_name' als de statische sleutel en de waarde die u hebt ingevoerd in de invoer als waarde voor het label doorgegeven. 
 
9. Start de app en ziet u het volgende. Nu een naam voor een test-gebeurtenis als volgt uit en klik op **verzenden** eronder. 

    ![][1]

10. Als u naar het tabblad **Monitor** van uw app en kijk onder **evenementen -> Details**gaat, ziet u nu deze gebeurtenis worden weergegeven samen met de statische app-info die we sturen. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
