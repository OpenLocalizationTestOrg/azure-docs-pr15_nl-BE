<properties
    pageTitle="Het gebruik van Apache Cordova Plugin voor Azure mobiele Apps"
    description="Het gebruik van Apache Cordova Plugin voor Azure mobiele Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Het gebruik van Apache Cordova Client Library voor Azure mobiele Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u voor het uitvoeren van veelvoorkomende scenario's met behulp van de meest recente [Apache Cordova Plugin voor Azure Mobile Apps]. Bent u nieuw in Azure Mobile Apps, eerste volledige [Azure Mobile Apps Quick Start] voor het maken van een back-end een tabel maken en downloaden van een vooraf gebouwde Apache Cordova-project. In deze handleiding richten we ons op de client-side Apache Cordova-invoegtoepassing.

## <a name="supported-platforms"></a>Ondersteunde Platforms

Deze SDK ondersteunt Apache Cordova v6.0.0 en later op iOS, Android en Windows apparaten.  De platformondersteuning is als volgt:

* Android (KitKat via noga) 19-24-API
* iOS 8.0 en hoger.
* Windows Phone 8.0
* Windows Phone 8.1
* Universal Windows Platform

##<a name="Setup"></a>Installatie en vereisten

Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel hetzelfde schema als de tabellen in deze zelfstudie. Deze handleiding wordt ervan uitgegaan dat u de invoegtoepassing Apache Cordova hebt toegevoegd aan uw code.  Als u nog niet gedaan, kunt u de invoegtoepassing Apache Cordova kunt toevoegen aan uw project op de opdrachtregel:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Zie de documentatie voor meer informatie over het maken van [uw eerste app in Apache Cordova].

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Procedure: gebruikers verifiëren

Azure App-Service biedt ondersteuning voor verificatie en autorisatie van app-gebruikers met verschillende externe id-providers: Facebook, Google, Microsoft-Account en Twitter. U kunt machtigingen instellen voor tabellen voor het beperken van toegang voor specifieke bewerkingen tot alleen geverifieerde gebruikers. U kunt ook de identiteit van geverifieerde gebruikers implementeren verificatieregels in serverscripts. Zie voor meer informatie de handleiding [aan de slag met verificatie] .

Wanneer u gebruikmaakt van verificatie in een app Apache Cordova, moeten de volgende Cordova plugins beschikbaar zijn:

* [cordova-plugin-apparaat]
* [cordova-plugin-inappbrowser]

Twee stromen voor verificatie worden ondersteund: een stroom van de server en een client-stroom.  De stroom van de server biedt de meest eenvoudige verificatie ervaring, dit is afhankelijk van de webinterface voor verificatie van de provider. De stroom van de client kunt u betere integratie met apparaatspecifieke mogelijkheden, zoals single-sign-on zoals deze is afhankelijk van de provider-specifieke apparaat-specifieke SDK's.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Procedure: uw mobiele-App voor omleiding van externe URL's configureren.

Verschillende soorten Apache Cordova toepassingen gebruiken een loopback-mogelijkheid OAuth UI stromen te verwerken.  Stromen op localhost OAuth UI tot problemen leiden omdat de verificatieservice weet alleen hoe benut u uw service standaard.  Voorbeelden van problematische OAuth UI stromen zijn:

- De rimpel-emulator.
- Opnieuw laden met ionische Live.
- De mobiele backend lokaal uitgevoerd
- De mobiele back-end in een andere Azure App Service dan de ene die verificatie uitgevoerd.

Volg de instructies om uw lokale instellingen toevoegen aan de configuratie:

1. Log in op de [Azure portal]
2. Selecteer **alle resources** of **App Services** Klik op de naam van uw mobiele App.
3. Klik op **Extra**
4. **Resource explorer** in het menu OBSERVE Klik op **Go**.  Een nieuw venster of tabblad wordt geopend.
5. Vouw de **config**, **authsettings** knooppunten voor uw site in de linkernavigatiebalk.
6. Klik op **bewerken**
7. Zoek naar het element 'allowedExternalRedirectUrls'.  Deze kan worden ingesteld op null of een matrix met waarden.  Wijzig de waarde in de volgende waarde:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    De URL's vervangen door de URL's van uw service.  Voorbeelden zijn 'http://localhost:3000' (voor de Node.js monster service) of "http://localhost:4400" (voor de rimpel-service).  Deze URL's zijn echter voorbeelden - uw situatie, met inbegrip van de diensten vermeld in de voorbeelden kunnen afwijken.
8. Klik op de knop **Lezen/schrijven** in de rechterbovenhoek van het scherm.
9. Klik op de groene knop **plaatsen** .

Op dit moment worden de instellingen opgeslagen.  Sluit de browservenster niet totdat u klaar bent met de instellingen opslaan.
Ook deze loopback-URL's toevoegen aan de CORS-instellingen voor uw App Service:

1. Log in op de [Azure portal]
2. Selecteer **alle resources** of **App Services** Klik op de naam van uw mobiele App.
3. De instellingen voor blade wordt automatisch geopend.  Als dat niet het geval is, klikt u op **Alle instellingen**.
4. Klik op **CORS** onder het menu API.
5. ENTER de URL die u wilt toevoegen in het vak voorwaarde en druk op Enter.
6. Indien nodig extra URL's invoeren.
7. Klik op **Opslaan** om de instellingen te slaan.

Het duurt ongeveer 10-15 seconden om de nieuwe instellingen van kracht te laten worden.

##<a name="register-for-push"></a>Procedure: registreren voor Push-meldingen

Installeer de [phonegap-plugin-push] push-meldingen te verwerken.  Deze invoegtoepassing gemakkelijk kan worden toegevoegd met behulp van de `cordova plugin add` opdracht op de opdrachtregel of via de Git plugin installer binnen Visual Studio.  De volgende code in uw Apache Cordova app registreert het apparaat voor push-meldingen:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Gebruik de SDK melding Hubs push om meldingen te verzenden vanaf de server.  Nooit push-meldingen verzenden rechtstreeks van clients. Hierdoor kan worden gebruikt voor het starten van een denial of service-aanval tegen melding Hubs of de PNS.  De PNS kan het verkeer als gevolg van dergelijke aanvallen been.

<!-- URLs. -->
[Azure portal]: https://portal.azure.com
[Azure mobiele Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Aan de slag met verificatie]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova Plugin voor Azure mobiele Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[uw eerste app in Apache Cordova]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-plugin-apparaat]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
