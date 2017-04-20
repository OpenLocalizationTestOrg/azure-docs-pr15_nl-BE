<properties
    pageTitle="Push-meldingen verzenden naar apps met Azure melding Hubs chroom | Microsoft Azure"
    description="Leren werken met Azure melding Hubs push om meldingen te verzenden naar een toepassing van chroom."
    services="notification-hubs"
    keywords="mobiele push-meldingen, push-meldingen push notification, chroom push-meldingen"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Push-meldingen verzenden naar apps met Azure melding Hubs chroom

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In dit onderwerp wordt beschreven hoe u Azure melding Hubs push om meldingen te verzenden naar een toepassing chroom, die wordt weergegeven in het kader van de Google Chrome browser gebruiken. In deze zelfstudie maken we een chroom-app die push-meldingen ontvangen via [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/). 

>[AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

De zelfstudie doorloopt u deze basisstappen push-meldingen inschakelen:

* [Google Cloud Messaging inschakelen](#register)
* [Uw melding hub configureren](#configure-hub)
* [Uw App Chrome verbinding met de melding hub](#connect-app)
* [Een push-bericht verzenden naar uw App chroom](#send)
* [Extra functionaliteit en mogelijkheden](#next-steps)

>[AZURE.NOTE] Chrome app push-meldingen zijn niet algemeen browser-meldingen - zij specifiek zijn voor de browser uitbreidbaarheid model (Zie [Chroom Apps-overzicht] voor meer informatie). Naast de desktop browser uitvoeren Chrome apps via Apache Cordova in mobile (Android en iOS). Zie [Chroom Apps op Mobile] voor meer informatie.

GCM en Azure melding Hubs configureren is gelijk aan het configureren voor Android, aangezien [Google Cloud Messaging voor chroom] is afgeschaft en de dezelfde GCM biedt nu ondersteuning voor zowel Android apparaten en chroom exemplaren.

##<a id="register"></a>Google Cloud Messaging inschakelen

1. Ga naar de website van [Google Cloud Console] aanmelden met uw Google-account-referenties en klik vervolgens op de knop **Project maken** . Een geschikte **Naam**opgeven en klik vervolgens op de knop **maken** .

    ![Google Cloud Console - Project maken][1]

2. Noteer het **Nummer van het Project** op de pagina **projecten** voor het project dat u zojuist hebt gemaakt. U kunt dit als in de App Chrome **GCM Sender ID** registreren bij GCM.

    ![Google Cloud Console - projectnummer][2]

3. **API's & auth**, klik in het linkerdeelvenster en schuif naar beneden en klik op de in-/ uitschakelen als u **Google Cloud Messaging voor Android**. U hebt geen **Google Cloud Messaging voor chroom**inschakelen.

    ![Google Cloud - serversleutel-Console][3]

4. Klik in het linkerdeelvenster op **referenties** > **Nieuwe sleutel maken** > **Sleutel Server** > **maken**.

    ![Google Cloud Console - referenties][4]

5. Maak een notitie van de **API-sleutel**van de server. Configureert u dit in uw melding hub vervolgens om push-meldingen verzenden naar GCM.

    ![Google Cloud Console - API-sleutel][5]

##<a id="configure-hub"></a>Uw melding hub configureren

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. in de **Instellingen** -blade Selecteer **Notification Services** en **Google (GCM)**. De API-sleutel invoeren en opslaan.

&emsp;&emsp;![Azure melding Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Uw App Chrome verbinding met de melding hub

De hub van uw melding is nu geconfigureerd voor het werken met GCM, en u hebt de verbindingsreeksen voor het registreren van uw app als u wilt ontvangen en verzenden van push-meldingen. LK

###<a name="create-a-new-chrome-app"></a>Maak een nieuwe App voor chroom

Het onderstaande voorbeeld is gebaseerd op het [Chrome App GCM monster] en de aanbevolen manier voor het maken van een App Chrome gebruikt. We wordt de stappen die specifiek betrekking heeft op Azure melding Hubs gemarkeerd. 

>[AZURE.NOTE] U wordt aangeraden de bron voor dit App chroom uit [Chroom App melding Hub monster]te downloaden.

De App Chrome via JavaScript wordt gemaakt en kunt u een van uw favoriete word editors voor het maken van het. Hieronder vindt u deze App Chrome zoeken.

![Google Chrome App][15]

1. Maak een map en geef deze de naam `ChromePushApp`. De naam is natuurlijk willekeurige - als noem maar iets anders, moet dat u het pad in de segmenten van de vereiste code vervangen.

2. Download de [crypto-js bibliotheek] in de map die u hebt gemaakt in de tweede stap. Deze map bevat twee submappen: `components` en `rollups`.

3. Maak een `manifest.json` bestand. Alle chroom Apps worden ondersteund door een manifestbestand met de metagegevens van de app en de meeste is dat alle machtigingen die zijn toegekend aan de app wanneer de gebruiker is geïnstalleerd.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Aankondiging van de `permissions` -element geeft aan dat dit App chroom push-meldingen ontvangen van GCM. De URI Azure melding Hubs waar de App Chrome wordt gebeld REST om te registreren moet ook opgeven.
    Onze monster app gebruikt ook een pictogrambestand `gcm_128.png`, die vindt u bij de bron die u opnieuw vanaf de oorspronkelijke GCM monster wordt gebruikt. U kunt vervangen door deze een installatiekopie die voldoet aan de [criteria van het pictogram](https://developer.chrome.com/apps/manifest/icons).

4. Maak een bestand met de naam `background.js` met de volgende code:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Dit is het bestand definieert ook de handler **messageReceived** voor het verwerken van de inkomende push-bericht dat verschijnt het venster chroom App HTML (**register.html**).

5. Maak een bestand met de naam `register.html` -Hiermee definieert u de gebruikersinterface van de toepassing van chroom. 

   >[AZURE.NOTE] In dit voorbeeld gebruikt **CryptoJS v3.1.2**. Als u een andere versie van de bibliotheek hebt gedownload, controleert u of u goed vervangen door de versie in de `src` pad.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Maak een bestand met de naam `register.js` met de volgende code. Dit bestand bevat het script achter `register.html`. Chroom Apps staan geen in line worden uitgevoerd, hebt u een aparte back-script maken voor de gebruikersinterface.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Het bovenstaande script heeft de volgende belangrijke parameters:
    - **Window.OnLoad** definieert de gebeurtenis click van de knop van de twee knoppen in de gebruikersinterface. Een registreert bij GCM en andere registratie-ID die wordt geretourneerd na inschrijving bij GCM registreren met Azure melding Hubs worden gebruikt.
    - **updateLog** is de functie waarmee wij eenvoudige logboekfuncties verwerken.
    - **registerWithGCM** is de eerste knop Klik handler, waardoor de `chrome.gcm.register` tot GCM chroom App-exemplaar registreren.
    - **registerCallback** is de callback-functie die wordt aangeroepen wanneer de registratie GCM aanroep retourneert.
    - **registerWithNH** is de tweede knop Klik handler, bij melding Hubs registreert. Deze `hubName` en `connectionString` (die de gebruiker heeft opgegeven) en de melding Hubs registratie REST API-aanroep crafts.
    - **splitConnectionString** en **generateSaSToken** zijn helpers met daarin de JavaScript-uitvoering van een proces voor het maken van token van SaS, die moet worden gebruikt in alle overige API-aanroepen. Zie [Algemene begrippen](http://msdn.microsoft.com/library/dn495627.aspx)voor meer informatie.
    - **sendNHRegistrationRequest** is de functie die een HTTP-overige bellen naar Azure melding Hubs.
    - **registrationPayload** definieert de XML-registratie nettolading. Zie [Registratie NH REST API maken]voor meer informatie. De registratie-ID in het bijgewerkt met wat we van GCM ontvangen.
    - **client** is een exemplaar van het **XMLHttpRequest** gebruiken we de HTTP POST-aanvraag. Opmerking: we werken de `Authorization` kop met `sasToken`. Deze oproep is voltooid wordt dit exemplaar chroom App met Azure melding Hubs registreren.


De algehele mapstructuur voor dit project moet er als volgt:     ![Google Chrome App - mapstructuur][21]

###<a name="set-up-and-test-your-chrome-app"></a>Instellen en testen van uw App chroom

1. Open de browser Chrome. Open **Chrome extensies** en inschakelen van de **modus voor ontwikkelaars**.

    ![Google Chrome - de modus voor ontwikkelaars inschakelen][16]

2. Klik op **laden extensie uitgepakt** en navigeer naar de map waar u de bestanden hebt gemaakt. U kunt desgewenst ook de **chroom Apps & extensies ontwikkelprogramma**. Dit hulpprogramma is een chroom-App op zichzelf (geïnstalleerd vanuit de Chrome Web Store) en biedt geavanceerde mogelijkheden voor foutopsporing voor de ontwikkeling van uw App chroom.

    ![Google Chrome - extensie uitgepakt laden][17]

3. Als de App Chrome zonder fouten wordt gemaakt, ziet u uw chroom App weergegeven.

    ![Google Chrome - Chrome App weergeven][18]

4. Het **Projectnummer** dat u eerder van de **Google wolk Console** als de afzender-ID hebt ontvangen, en klik op **registreren bij GCM**. Ziet u het bericht **registratie met geslaagd GCM.**

    ![Google Chrome - Chrome App aanpassen][19]

5. Uw **Melding hubnaam** en **DefaultListenSharedAccessSignature** die u eerder via de portal verkregen, en klik op **registreren met Azure melding Hub**. Ziet u het bericht **melding Hub is geregistreerd!** en de details van het registratie-antwoord, waarin de Azure melding Hubs registratie-id.

    ![Google Chrome - melding Hub Details opgeven][20]  

##<a name="send"></a>Een bericht verzenden naar uw App chroom

Voor testdoeleinden, sturen we chroom push-meldingen met behulp van een .NET console application. 

>[AZURE.NOTE] Push-meldingen met Hubs melding kunt u verzenden vanaf een back-end via onze openbare <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">interface van de REST</a>. Bekijk onze [documentatie portal](https://azure.microsoft.com/documentation/services/notification-hubs/) voor meer voorbeelden van cross-platform.

1. Selecteer **Nieuw** en klik vervolgens op **Project**in Visual Studio in het menu **bestand** . Klik op **Windows** en **Consoletoepassing**in **Visual C#**, en klik op **OK**.  Hiermee maakt u een nieuwe console application-project.

2. Klik in het menu **Extra** op **Library Package Manager** en klik vervolgens op **Package Manager-Console**. De Package Manager-Console worden weergegeven.

3. In het consolevenster kunt u de volgende opdracht uitgevoerd:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee voegt u een verwijzing naar de SDK Azure Service Bus met de <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>toe.

4. Open `Program.cs` en voeg de volgende `using` instructie:

        using Microsoft.Azure.NotificationHubs;

5. In de `Program` klasse, voegt u de volgende methode:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Vervang de `<hub name>` de tijdelijke aanduiding voor de naam van de hub melding die wordt weergegeven in de [portal](https://portal.azure.com) in uw melding Hub blade. Ook de connection string tijdelijke aanduiding vervangen door de verbindingsreeks genoemd `DefaultFullSharedAccessSignature` die u in de configuratiesectie van kennisgeving hub hebt gekregen.

    >[AZURE.NOTE] Zorg ervoor dat u de verbindingsreeks met **volledige** toegang, niet **luisteren** . De verbindingsreeks **luisteren** toegang verleent geen machtigingen om push-meldingen te verzenden.

5. Toevoegen van de volgende aanroepen in de `Main` methode:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Zorg ervoor dat Chrome wordt uitgevoerd en de consoletoepassing worden uitgevoerd.

7. U moet de volgende melding pop up zien op uw bureaublad.

    ![Google Chrome - melding][13]

8. U kunt ook alle uw meldingen zien met behulp van het venster chroom meldingen op de taakbalk (in Windows) als chroom wordt uitgevoerd.

    ![Google Chrome - lijst met meldingen][14]

>[AZURE.NOTE] U hoeft niet de App Chrome wordt uitgevoerd of openen in de browser (Hoewel de Chrome browser zelf moet worden uitgevoerd). Ook krijgt u een totaaloverzicht van alle meldingen die je in het venster berichten voor chroom.

## <a name="next-steps"> </a>Volgende stappen

Meer informatie over Hubs melding in het [Systeemvak Hubs overzicht].

Verwijzen naar de zelfstudie [Azure melding Hubs gebruikers waarschuwen] om specifieke doelpubliek. 

Als u wilt dat uw gebruikers door belangengroepen in segmenten, kunt u de zelfstudie [Azure melding Hubs belangrijk nieuws] te volgen.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App melding Hub monster]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud-Console]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Melding Hubs-overzicht]: notification-hubs-push-notification-overview.md
[Overzicht Apps chroom]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM monster]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Apps op Mobile chroom]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Registratie NH REST API maken]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[Crypto-js library]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging voor chroom]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure melding Hubs waarschuwen gebruikers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Hubs melding laatste nieuws]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
