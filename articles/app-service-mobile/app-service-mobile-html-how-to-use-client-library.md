<properties
    pageTitle="Het gebruik van de SDK voor JavaScript voor Azure mobiele Apps"
    description="Hoe gebruik v voor Azure mobiele Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Het gebruik van de bibliotheek van JavaScript Client voor Azure mobiele Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u voor het uitvoeren van veelvoorkomende scenario's met behulp van de nieuwste [SDK voor JavaScript voor Azure Mobile Apps]. Bent u nieuw in Azure Mobile Apps, voltooi eerst [Azure Mobile Apps Quick Start] te maken van een back-end en een tabel te maken. In deze handleiding richten we ons op het gebruik van de mobiele back-end in HTML/JavaScript-toepassingen.

## <a name="supported-platforms"></a>Ondersteunde Platforms

We beperken browserondersteuning op de huidige en de laatste versies van de belangrijkste browsers: Google Chrome, Microsoft Edge, Microsoft Internet Explorer en Mozilla Firefox.  We verwachten dat de functie met een relatief moderne browser-SDK.

Het pakket wordt gedistribueerd als een universele JavaScript-Module ondersteunt globals, AMD, en CommonJS worden opgemaakt.

##<a name="Setup"></a>Installatie en vereisten

Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel hetzelfde schema als de tabellen in deze zelfstudie.

Installatie van de SDK Azure Mobile Apps JavaScript kan worden uitgevoerd via de `npm` opdracht:

```
npm install azure-mobile-apps-client --save
```

Eenmaal geïnstalleerd, de bibliotheek bevindt zich in `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Kopieer dit bestand naar uw webpagina gebied.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

De bibliotheek kan ook worden gebruikt als een module in CommonJS omgevingen zoals Browserify en Webpack en als een bibliotheek met AMD ES2015.  Bijvoorbeeld:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Procedure: gebruikers verifiëren

Azure App-Service biedt ondersteuning voor verificatie en autorisatie van app-gebruikers met verschillende externe id-providers: Facebook, Google, Microsoft-Account en Twitter. U kunt machtigingen instellen voor tabellen voor het beperken van toegang voor specifieke bewerkingen tot alleen geverifieerde gebruikers. U kunt ook de identiteit van geverifieerde gebruikers implementeren verificatieregels in serverscripts. Zie voor meer informatie de handleiding [aan de slag met verificatie] .

Twee stromen voor verificatie worden ondersteund: een stroom van de server en een client-stroom.  De stroom van de server biedt de meest eenvoudige verificatie ervaring, dit is afhankelijk van de webinterface voor verificatie van de provider. De stroom van de client kunt u betere integratie met apparaatspecifieke mogelijkheden, zoals single-sign-on zoals deze is afhankelijk van de provider-specifieke SDK's.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Procedure: uw mobiele-App voor omleiding van externe URL's configureren.

Verschillende typen JavaScript-toepassingen gebruiken een loopback-mogelijkheid OAuth UI stromen te verwerken.  Deze mogelijkheden zijn:

* Uw service lokaal uitgevoerd
* Met behulp van Live opnieuw laden met ionische Framework
* Omleiden naar App Service voor verificatie. 

Lokaal uitgevoerd kan problemen veroorzaken omdat standaard alleen verificatie van App-Service is geconfigureerd voor toegang van uw mobiele App-end. Gebruik de volgende stappen wijzigen de instellingen App als verificatie wilt inschakelen wanneer de server lokaal wordt uitgevoerd:

1. Log in op de [Azure portal]
2. Ga naar uw mobiele App backend.
3. Selecteer de **Resource explorer** in het menu **Extra van de ontwikkeling** .
4. Klik op **Ga naar** de resource explorer voor uw backend Mobile App openen in een nieuw tabblad of venster.
5. Vouw de **config** > **authsettings** -knooppunt voor uw app.
6. Klik op de knop **bewerken** om het bewerken van de bron.
7. Zoek het element **allowedExternalRedirectUrls** , zijn null. Uw URL's in een array toevoegen:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Vervangen door de URL's van uw service, die in dit voorbeeld wordt de URL's in de matrix `http://localhost:3000` voor de lokale Node.js monster service. U kunt ook `http://localhost:4400` voor de service Rimpel of een URL, afhankelijk van de configuratie van uw app.

8. Aan de bovenkant van de pagina, **Lezen/schrijven**, klik **plaatsen** om op te slaan van uw updates.

U moet ook dezelfde loopback-URL's toevoegen aan de instellingen van de "witte" lijst CORS:

1. Ga terug naar de [portal Azure].
2. Ga naar uw mobiele App backend.
3. **CORS** Klik in het menu van de **API** .
4. Voer elke URL in het lege vak **Oorsprong toegestaan** .  Er wordt een nieuw tekstvak gemaakt.
5. Klik op **Opslaan**
    
Nadat de back-end is bijgewerkt, kunt u zijn de nieuwe loopback-URL's gebruiken in uw app.

<!-- URLs. -->
[Azure mobiele Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Aan de slag met verificatie]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portal]: https://portal.azure.com/
[JavaScript-SDK voor Azure mobiele Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

