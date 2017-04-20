<properties
    pageTitle="Aan de slag met Azure mobiele Engagement voor Cordova/Phonegap"
    description="Informatie over het gebruik Azure Mobile Engagement met Analytics en Push-meldingen voor apps Cordova/Phonegap."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Aan de slag met Azure mobiele Engagement voor Cordova/Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp wordt beschreven hoe u Azure Mobile Engagement gebruiken om te begrijpen van het gebruik van app en push-meldingen verzenden naar gesegmenteerde gebruikers voor een mobiele toepassing is ontwikkeld met het Cordova.

In deze zelfstudie wordt een lege Cordova app Mac gebruik maken en integreren Mobile Engagement SDK. Het basic analytics gegevens verzamelt en ontvangt de push-meldingen met Apple Push Notification System (APNS) voor iOS en Google Cloud Messaging (GCM) voor Android. We zullen distribueren naar een iOS- of Android-apparaat om te testen. 

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Deze zelfstudie is het volgende vereist:

+ XCode, die u vanuit de Mac App Store installeren kunt (voor het implementeren van iOS)
+ [Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (voor het implementeren van Android)
+ Push notification certificaat (.p12) die u van Apple Dev Center for APNS krijgen kunt
+ GCM projectnummer die u van de Google Developer-Console voor GCM krijgen kunt
+ [Invoegtoepassing voor betrokkenheid bij de mobiele Cordova](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] U vindt de broncode en het Leesmij-bestand voor de invoegtoepassing Cordova op [Github](https://github.com/Azure/azure-mobile-engagement-cordova)

##<a id="setup-azme"></a>Mobiele betrokkenheid bij de instellingen voor uw app Cordova

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Uw app verbinding met de back-end Mobile Engagement

Deze zelfstudie bevat een "basic integration" is de minimale vereiste gegevens verzamelen en verzenden van een push-bericht instellen. 

Een eenvoudige app maken we met Cordova om aan te tonen van de integratie:

###<a name="create-a-new-cordova-project"></a>Maak een nieuw project van Cordova

1. *Terminal* -venster op uw Mac-computer start en typt u het volgende op die van de standaardsjabloon een nieuw project van Cordova maakt. Zorg ervoor dat het profiel voor publiceren u uiteindelijk implementeren met uw iOS-app 'com.mycompany.myapp' wordt gebruikt als de App ID. 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Als uw project voor **iOS** configureren en uitvoeren in de iOS Simulator uitvoeren:

        $ cordova platform add ios 
        $ cordova run ios

3. Als uw project configureren voor **Android** en voer het uit in de Android-emulator worden uitgevoerd. Zorg dat de instellingen van de Android SDK Emulator haar doelstelling als Google-APIs (Google Inc.) met de CPU / ABI als Google APIs ARM.  

        $ cordova platform add android
        $ cordova run android

4.  De invoegtoepassing Cordova Console toevoegen. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinding maken met uw app Mobile Engagement backend

1. De invoegtoepassing voor Azure Mobile Engagement Cordova terwijl de variabele waarden voor het configureren van de invoegtoepassing installeren:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Pictogram voor android bereiken* : moet de naam van de bron zonder een extensie of drawable voorvoegsel (ex: mynotificationicon), en het pictogrambestand moet worden gekopieerd naar uw android project (platforms/android/res/drawable)

*iOS pictogram bereiken* : de naam van de resource met de extensie (ex: mynotificationicon.png), en het bestand moet worden toegevoegd in uw project iOS met XCode (via het Menu bestanden toevoegen)

##<a id="monitor"></a>Realtime-controle inschakelen

1. Bewerk in het project Cordova - **www/js/index.js** om toe te voegen met de aanroep van Mobile Engagement om aan te geven een nieuwe activiteit één keer de gebeurtenis *deviceReady* wordt ontvangen.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. De toepassing wordt uitgevoerd:
        
    - **Voor iOS**
    
        In `Terminal` venster uw app in een nieuw exemplaar van de Simulator starten door het uitvoeren van de volgende:

            cordova run ios

    - **Voor Android**
        
        In `Terminal` venster uw app in een nieuw exemplaar van de emulator starten door het uitvoeren van de volgende:

            cordova run android

3. U kunt zien in de console Logboeken als volgt:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en messaging-app

Mobile Engagement kunt u communiceren met uw gebruikers met Push-meldingen en berichten in het kader van de campagnes in app. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
In de volgende secties wordt uw app voor het ontvangen van deze instellingen.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Push-referenties voor Mobile Engagement configureren

Als Mobile Engagement duwen om meldingen te verzenden namens u, moet u toegang tot uw Apple iOS certificaat of GCM Server API-sleutel. 
    
1. Ga naar uw betrokkenheid bij de mobiele portal. Of u in de app die we gebruikt voor dit project en klik op de knop **Engage** onder:
    
    ![][1]
    
2. U wordt op de pagina neerzetten in uw betrokkenheid bij de Portal. Van er Klik op de sectie **Standaard Push** :
    
    ![][2]

3. IOS certificaat/GCM Server API sleutel configureren

    **[iOS]**

    een. Selecteer uw .p12, uploaden en typ uw wachtwoord:
    
    ![][3]

    **[Android]**

    een. Klik op het bewerkingspictogram voor een **API-sleutel** in de sectie instellingen GCM, en in het pop-upmenu waaruit blijkt, de sleutel van de Server GCM plakken en klik op **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Push-meldingen in de app Cordova inschakelen

Bewerken **www/js/index.js** om toe te voegen de aanroep van Mobile Engagement aanvragen push-meldingen en een handler te declareren:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>De toepassing uitvoeren

**[iOS]**

1. We kunnen bouwen en implementeren van de app op het apparaat voor het testen van push-meldingen sinds iOS kan alleen push-meldingen met een werkelijk apparaat XCode gebruiken. Ga naar de locatie waar uw project Cordova is gemaakt en Ga naar de locatie **...\platforms\ios** . Open het bestand .xcodeproj native in XCode. 
    
2. Bouwen en implementeren van de app Cordova naar het iOS-apparaat met de account met de provisioning profiel waarin het certificaat dat u zojuist hebt geüpload naar de portal Mobile Engagement en de App-Id die overeenkomt met de opgegeven tijdens het maken van de app Cordova. U kunt controleren de *bundel-id* in uw **Resources\*-info.plist** XCode aan het bestand. 

3. Ziet u de standaard iOS pop-up op uw apparaat waarin staat dat de app vraagt toestemming om meldingen te verzenden. Verleen de machtiging. 

**[Android]**

U kunt gewoon de emulator en uitvoeren van de Android app zoals GCM meldingen worden ondersteund op de Android-emulator. 

    cordova run android

##<a id="send"></a>Een bericht verzenden naar uw app

Nu maken we een eenvoudige Push-bericht campagne die een push verzendt naar uw app op het apparaat:

1. Ga naar het tabblad **bereiken** in uw betrokkenheid bij de mobiele portal

2. Klik op **Nieuwe aankondiging** om uw push campagne te maken

    ![][6]

3. Bieden ingangen om uw campagne **[Android]** te maken
    
    - Geef een **naam** voor uw campagne. 
    - Selecteer het **Type levering** als *eenvoudige* *kennisgeving van systeem*
    - Selecteer de **levertijd** als *"enige tijd"*
    - Een **titel** opgeven voor uw melding dit de eerste regel in de push is.
    - Een **bericht** voor de kennisgeving die als de hoofdtekst van het bericht dient bieden. 

    ![][11]

4. Bieden u ingangen wilt maken van uw campagne **[iOS]**

    - Geef een **naam** voor uw campagne. 
    - Selecteer de **levertijd** als *'onvoldoende app"*
    - Een **titel** opgeven voor uw melding dit de eerste regel in de push is.
    - Een **bericht** voor de kennisgeving die als de hoofdtekst van het bericht dient bieden. 
 
    ![][12]

5. Scroll naar beneden en klik in de sectie inhoud **alleen melding** selecteren

    ![][8]

6. [Optioneel] U kunt ook een actie-URL opgeven. Zorg ervoor dat gebruikmaakt van een URL-schema dat is opgegeven tijdens het configureren van de invoegtoepassing **AZME\_REDIRECT\_URL** variabele, bijvoorbeeld *myapp://test*.  

7. U klaar bent met de meest elementaire campagne mogelijke instelling. Nu opnieuw Schuif naar beneden en klik op de knop **maken** om op te slaan uw campagne.

8. Ten slotte **activeren** uw campagne
    
    ![][10]

9. U ziet nu een push-bericht op uw apparaat of de emulator als onderdeel van de campagne. 

##<a id="next-steps"></a>Volgende stappen
[Overzicht van alle methoden die beschikbaar zijn bij Cordova Mobile Engagement SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

