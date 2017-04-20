<properties
    pageTitle="Push-meldingen aan Apache Cordova App met Azure mobiele Apps toevoegen | Azure App-Service"
    description="Leren werken met Azure Mobile Apps push-meldingen verzenden naar uw app Apache Cordova."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Push-meldingen toevoegen aan uw app Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie voegt u push-meldingen aan het project [start Apache Cordova snel] zodat een push-bericht wordt verzonden naar het apparaat elke keer dat een record wordt ingevoegd.

Als u niet het project van de server gedownload snel starten, moet u het pakket push notification-extensie. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="prerequisites"></a>Vereisten

Deze zelfstudie heeft betrekking op een Apache Cordova-toepassing is ontwikkeld met Visual Studio 2015 die wordt uitgevoerd op de Google Android-Emulator, een Android apparaat, een Windows-apparaat en een iOS-apparaat.

Deze zelfstudie hebt u het volgende nodig:

* Een PC met [Visual Studio Community 2015] of latere versies.
* [Visual Studio Tools for Apache Cordova].
* Een [actieve Azure account](https://azure.microsoft.com/pricing/free-trial/).
* Een voltooid project van [Apache Cordova snel starten] .
* (Android) Een [Google-account] met een geverifieerd e-mailadres.
* (iOS) Het lidmaatschap van een Apple-ontwikkelaar programma en een iOS-apparaat (iOS Simulator biedt geen ondersteuning voor push).
* (Windows) Een Windows Store Developer-Account en een apparaat met Windows 10.

##<a name="configure-hub"></a>Een hub meldingen configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Bekijk een video met de stappen in deze sectie](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Update van het serverproject om push-meldingen te verzenden

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Wijzigen van uw app Cordova push-meldingen ontvangen

U moet ervoor zorgen dat uw project Apache Cordova app klaar is voor push-meldingen afgehandeld door de invoegtoepassing Cordova push plus een platform-specifieke push-services installeren.

#### <a name="update-the-cordova-version-in-your-project"></a>De versie van Cordova in uw project werken.

Het is raadzaam het client-project te werken naar Cordova 6.1.1 als uw project is geconfigureerd met een oudere versie. Als u wilt bijwerken in het project, met de rechtermuisknop op config.xml om de configuratie van designer te openen. Selecteer het tabblad Platforms en kiest u in het tekstvak **Cordova CLI** 6.1.1.

Kies **Opbouwen**en vervolgens **Oplossing bouwen** voor het bijwerken van het project.

#### <a name="install-the-push-plugin"></a>De push-invoegtoepassing installeren

Apache Cordova toepassingen verwerken niet standaard apparaat of netwerk mogelijkheden.  Deze mogelijkheden worden geboden door de Plug-ins die zijn gepubliceerd op de [npm](https://www.npmjs.com/) of op GitHub.  De `phonegap-plugin-push` plugin wordt gebruikt voor het verwerken van netwerk push-meldingen.

U kunt de push-invoegtoepassing in een van de volgende manieren installeren:

**Vanaf de opdrachtprompt:**

Voer de volgende opdracht uit:

    cordova plugin add phonegap-plugin-push

**Vanuit Visual Studio:**

1.  Open in de Solution Explorer, de `config.xml` bestand Klik op **Plug-ins** > **aangepast**, **Git** als de installatiebron selecteren, voert u `https://github.com/phonegap/phonegap-plugin-push` als de bron.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Klik op de pijl naast de installatiebron.

3. In **SENDER_ID**, als er al een numerieke project-ID voor het project Google Developer-Console kunt u toevoegen dit hier. Voer anders de aanduidingswaarde van een tijdelijke, zoals 777777, en voor Android kunt u deze waarde in config.xml later bijwerken.

4. Klik op **toevoegen**.

De push-invoegtoepassing is nu geïnstalleerd.

####<a name="install-the-device-plugin"></a>De invoegtoepassing voor apparaat installeren

Volg de procedure waarmee u de push-plugin installeren, maar vindt u de invoegtoepassing voor het apparaat in de lijst met Core plugins (Klik op **Plug-ins** > **Core** te vinden). U moet deze invoegtoepassing om de naam van het platform te verkrijgen (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Registreer uw apparaat voor push op opstarten

In eerste instantie nemen we enkele minimale code voor Android. Later, maken we enkele kleine wijzigingen uit te voeren op iOS of Windows 10.

1. Een aanroep naar **registerForPushNotifications** toevoegen tijdens de retouraanroep voor de aanmelding of onder aan de methode **onDeviceReady** :

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    In dit voorbeeld ziet u **registerForPushNotifications** aanroepen nadat de verificatie is uitgevoerd, hetgeen wordt aanbevolen bij gebruik van zowel verificatie als push-meldingen in de app.

2. De nieuwe methode van **registerForPushNotifications** als volgt toevoegen:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) Vervang in de bovenstaande code `Your_Project_ID` project met de numerieke ID voor uw app uit de [Google Developer-Console].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Optioneel) Configureren en uitvoeren van de app op Android

Voltooi deze sectie als u de push-meldingen voor Android.

####<a name="enable-gcm"></a>Firebase inschakelen Cloud Messaging

Aangezien we zijn gericht op het Google Android platform in eerste instantie, moet u de Firebase Cloud Messaging inschakelen. Als u Microsoft Windows-apparaten zijn gericht, zou u ook WNS ondersteuning inschakelen.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>De backend mobiele App voor het verzenden van FCM met push-aanvragen configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Configureer uw Cordova-app voor Android

In uw app Cordova config.xml open en vervang `Your_Project_ID` project met de numerieke ID voor uw app uit de [Google Developer-Console].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Index.js openen en bijwerken van de code voor het gebruik van uw numerieke project-ID.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Het Android apparaat voor USB-foutopsporing configureren

Voordat u uw toepassing met het Android apparaat implementeren kunt, moet u de USB-foutopsporing inschakelen.  Voer de volgende stappen uit op uw Android telefoon:

1. Ga naar **Instellingen** > **over de telefoon**en vervolgens tikken **Build-nummer** totdat de modus voor ontwikkelaars (ongeveer 7 maal) is ingeschakeld.

2. In de **Instellingen** > **Opties voor ontwikkelaars** **USB-foutopsporing**inschakelen en vervolgens uw Android-telefoon verbindt met de ontwikkeling van PC met een USB-kabel.

We getest dit met behulp van een Google Nexus 5 X-apparaat met Android 6.0 (Boterbloem).  De technieken worden echter gebruikt in een moderne versie van de Android.

#### <a name="install-google-play-services"></a>Google Play Services installeren

De push-invoegtoepassing maakt gebruik van Android Google Play Services voor push-meldingen.  

1.  Klik op **hulpprogramma's**in **Visual Studio** > **Android** > **Android SDK Manager**, vouw de map **Extra's** en schakel het selectievakje in om ervoor te zorgen dat elk van de volgende SDK's is geïnstalleerd.
    * Android 2.3 of hoger
    * Google bibliotheek herziening 27 of hoger
    * Google Play Services 9.0.2 of hoger

2.  Klik op **Pakketten installeren** en wacht totdat de installatie is voltooid.

De huidige vereiste bibliotheken worden vermeld in de [documentatie voor de phonegap-plugin-push installatie].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Push-meldingen in de app op Android test

U kunt nu testen push-meldingen waarop de toepassing wordt uitgevoerd en het invoegen van items in de tabel TodoItem. U kunt dit doen vanuit hetzelfde apparaat of vanuit een tweede apparaat als u van de dezelfde backend gebruikmaakt. Test uw app Cordova op het Android-platform in een van de volgende manieren:

- **Op een fysiek apparaat:**  
Uw Android apparaat aangesloten op de computer met een USB-kabel.  In plaats van **Google Android Emulator**, selecteer **apparaat**. Visual Studio wordt de toepassing op het apparaat implementeren en uit te voeren.  Vervolgens kunt u werken met de toepassing op het apparaat.  
De ontwikkeling te verbeteren.  Scherm delen van toepassingen, zoals [Mobizen] kunnen u helpen bij het ontwikkelen van een Android toepassing door het Android-scherm bij een webbrowser op uw PC projecteren.

- **Op een Android-emulator:**  
Er zijn extra configuratiestappen vereist wanneer een emulator wordt uitgevoerd.

    Zorg ervoor dat u wilt implementeren of foutopsporing op een virtueel apparaat met Google APIs ingesteld als het doel, zoals hieronder wordt weergegeven in beheer van Android virtueel apparaat (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Als u met een snellere x86 emulator, [het HAXM-stuurprogramma installeren](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) en configureren van de emulator gebruikt.

    Een Google-account toevoegen aan het Android apparaat door te klikken op **Apps** > **Instellingen** > **account toevoegen**en volg de aanwijzingen voor het toevoegen van een bestaande Google-account aan het apparaat (aangeraden met behulp van een bestaande account in plaats van een nieuwe te maken).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    De takenlijst van toepassing als voor uitvoeren en een nieuwe todo item invoegen. Deze periode wordt wordt een pictogram in het systeemvak weergegeven in het systeemvak. U kunt de melding lade om de volledige tekst van het bericht weer te openen.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Optioneel) Configureren en uitvoeren op iOS

Deze sectie is voor het uitvoeren van het project Cordova op iOS-apparaten. Als u niet met iOS apparaten werkt, kunt u deze sectie overslaan.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Installeren en uitvoeren van de iOS remotebuild agent op een Mac of cloud-service

Voordat u een Cordova-app voor iOS met behulp van Visual Studio uitvoeren kunt, gaat u door de stappen in de [iOS-installatiehandleiding](http://taco.visualstudio.com/en-us/docs/ios-guide/) wilt installeren en uitvoeren van de agent remotebuild.

Controleer of dat u kunt de app voor iOS maken. De stappen in de installatiehandleiding zijn voor iOS maken vanuit Visual Studio vereist. Als u niet een Mac hebt, kunt u bouwen voor iOS met behulp van de remotebuild-agent op een service, zoals MacInCloud. Zie voor meer info, [uitvoeren van uw iOS-app in de cloud](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] XCode 7 of hoger is vereist voor het gebruik van de invoegtoepassing push op iOS.

####<a name="find-the-id-to-use-as-your-app-id"></a>De ID moet worden gebruikt als uw App-ID zoeken

Voordat u uw app voor push-meldingen open config.xml in uw app Cordova registreren vindt de `id` de waarde in het element widget en kopiëren voor later gebruik. In het volgende XML-, de ID is `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Deze id later gebruiken bij het maken van een App-ID op de Apple developer-portal. (Als u een andere App ID maken in de portal voor ontwikkelaars wilt gebruiken die u moet een paar extra stappen worden verderop in deze handleiding kunt u deze ID in config.xml wijzigen. De ID van het object element moet overeenkomen met de App-ID op de portal developer.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>De app voor push-meldingen op de Apple developer-portal registreren

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Bekijk een video met vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Azure voor het verzenden van push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Controleer of uw App-ID komt overeen met uw app Cordova

Als de App-ID die u hebt gemaakt in uw Apple Developer Account al overeenkomt met de ID van het object element in config.xml, kunt u deze stap overslaan. Als de id's niet overeenkomen, echter de volgende stappen uitvoeren:

1. Verwijder de map platforms van uw project.

2. Verwijder de map Plug-ins van uw project.

3. Verwijder de map node_modules van uw project.

4. Het id-kenmerk van het element widget in config.xml gebruiken de App-ID die u hebt gemaakt in uw Apple Developer Account bijwerken.

5. Het project opnieuw.

#####<a name="test-push-notifications-in-your-ios-app"></a>Test push-meldingen in de iOS-app

1. Zorg ervoor dat **iOS** is geselecteerd als implementatiedoel voor de in Visual Studio en kies vervolgens een **apparaat** uit te voeren op uw verbonden iOS-apparaat.

    U kunt uitvoeren op een iOS-apparaat aangesloten op uw PC met iTunes. De iOS Simulator biedt geen ondersteuning voor push-meldingen.

2. Druk op de knop **uitvoeren** of **F5** in Visual Studio voor het bouwen van het project en de app starten in een iOS-apparaat en klik vervolgens op **OK** om te accepteren van push-meldingen.

    >[AZURE.NOTE] U moet expliciet push-meldingen accepteren van uw app. Deze aanvraag komt alleen voor de eerste keer dat de toepassing wordt uitgevoerd.

3. Typ een taak in de app, en klik vervolgens op het plusteken (+) pictogram.

4. Controleer of dat een melding wordt ontvangen en klik vervolgens op OK om het bericht te sluiten.

##<a name="optional-configure-and-run-on-windows"></a>(Optioneel) Configureren en uitvoeren in Windows

Deze sectie is voor het uitvoeren van het project Apache Cordova app op Windows 10-apparaten (de PhoneGap push-invoegtoepassing wordt ondersteund op Windows 10). Als u niet met Windows-apparaten werkt kunt u deze sectie overslaan.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Uw Windows-app voor push-meldingen registreren bij WNS

Selecteer als u de opties in Visual Studio, Windows doel uit de lijst oplossing Platforms, zoals **Windows-x64** of **Windows-x86** ( **/ Windows-platform** voorkomen voor push-meldingen).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Bekijk een video met vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>De hub meldingen configureren voor WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Uw app Cordova ter ondersteuning van push-meldingen van Windows configureren

De ontwerper van de configuratie (Klik met de rechtermuisknop op config.xml en selecteer **View Designer**) te openen, selecteer het tabblad **Windows** en kies **Windows 10** onder **Windows-versie van doel**.

>[AZURE.NOTE] Als u een versie Cordova voor Cordova 5.1.1 (6.1.1 aanbevolen) gebruikt, moet u ook de Toast staat vlag instellen op true in config.xml.

Ter ondersteuning van push meldingen in de standaard (debug) wordt gemaakt, openen build.json bestand. Kopieer de configuratie 'release' naar de debug-configuratie.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Na de update, de bovenstaande code ziet er zo uit.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

De app bouwen en controleer of er geen fouten. U client app dient nu te registreren voor de kennisgevingen van de backend Mobile App. Herhaal deze sectie voor elk Windows-project in de oplossing.

####<a name="test-push-notifications-in-your-windows-app"></a>Test push-meldingen in uw Windows-app

Zorg ervoor dat Windows-platform als implementatiedoel voor de, zoals **Windows-x64** of **Windows-x86**is geselecteerd in Visual Studio. Kies de app uitvoeren op een Windows 10 PC die als host fungeert voor Visual Studio, **Lokale Machine**.

Druk op de knop uitvoeren voor het bouwen van het project en de app start.

Typ een naam voor een nieuwe todoitem in de app, en klik op het plusteken (+) pictogram toe te voegen.

Controleren of een kennisgeving is ontvangen wanneer het item wordt toegevoegd.

##<a name="next-steps"></a>Volgende stappen

* Meer informatie over [Hubs voor meldingen] over push-meldingen.
* Als u dit nog niet hebt gedaan, blijven de zelfstudie door [Verificatie toe te voegen] aan uw app Apache Cordova.

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs -->
[Verificatie toe te voegen]: app-service-mobile-cordova-get-started-users.md
[Snelle start Apache Cordova]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google-account]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google Developer-Console]: https://console.developers.google.com/home/dashboard
[phonegap-plugin-push installatie-documentatie]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Melding Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
