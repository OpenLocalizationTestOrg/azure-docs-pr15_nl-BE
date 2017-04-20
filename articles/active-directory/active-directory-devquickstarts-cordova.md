<properties
    pageTitle="Azure AD Cordova aan de slag | Microsoft Azure"
    description="Het bouwen van een Cordova-toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Azure AD integreren met een Apache Cordova app

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova kunt u HTML5/JavaScript-toepassingen die kunnen worden uitgevoerd op mobiele apparaten als volwaardige toepassingen ontwikkelen.
Met Azure Active Directory, kunt u de enterprise grade verificatiefuncties voor uw toepassingen Cordova toevoegen. Dankzij een native SDK's Azure AD tekstterugloop op iOS, Android, Windows Store en Windows Phone-Cordova-invoegtoepassing kunt u de toepassing ondersteuning voor aanmelden op met uw gebruikersaccount AD, toegang krijgen tot Office 365 en Azure API en zelfs uw eigen aangepaste Web API-aanroepen te beschermen kunt verbeteren.

In deze zelfstudie gebruiken we de Apache Cordova plugin voor Active Directory verificatie bibliotheek (ADAL) voor het verbeteren van een eenvoudige app met de volgende functies:

-   AD gebruikers verifiëren en een token voor het aanroepen van de API Azure AD grafiek ophalen met een paar regels code.
-   Gebruik deze token aan te roepen van de Graph API voor het opvragen van die directory en de resultaten weergeven  
-   Maak gebruik van de ADAL tokencache voor verificatie wordt gevraagd die de gebruiker tot een minimum beperken.

Hiervoor moet u naar:

2. Een toepassing met Azure Active Directory registreren
2. Voeg code toe aan uw app op verzoek tokens
3. Code voor het gebruik van het token voor de Graph API query's toevoegen en weergeven van resultaten.
4. Het Cordova implementatie-project maken met alle platforms die u wilt richten en de invoegtoepassing Cordova ADAL en test u de oplossing in emulators.

## <a name="0--prerequisites"></a>*0. voorwaarden voor*

Deze zelfstudie hebt voltooid:

- Een Azure AD huurder waar u een account met rechten voor app-ontwikkeling hebt
- Een ontwikkelomgeving die is geconfigureerd voor het gebruik van Apache Cordova  

Als u beide al hebt ingesteld, gaat u rechtstreeks naar stap 1.

Als u een huurder Azure AD niet hebt, kunt u vinden [instructies voor het verkrijgen van een hier](active-directory-howto-tenant.md).

Als u geen Apache Cordova instellen op uw computer hebt, installeert u het volgende:

- [GIT](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova-CLI](https://cordova.apache.org/) (kan eenvoudig worden geïnstalleerd via de NPM Pakketbeheer: `npm install -g cordova`)

Overigens die zowel op de PC als op de Mac. werkt

Elk doelplatform heeft verschillende vereisten.

- Bouwen en uitvoeren van Windows Tablet PC of telefoon app versie
    - [Visual Studio 2013 voor Windows Update 2 of hoger](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express of een andere versie).
- Bouwen en uitvoeren voor iOS
    -   Xcode 5.x of hoger. Download het http://developer.apple.com/downloads of de [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios-sim](https://www.npmjs.org/package/ios-sim) – kun je iOS-apps in de iOS Simulator vanaf de opdrachtregel starten (kan eenvoudig worden geïnstalleerd via de terminal: `npm install -g ios-sim`)

- Bouwen en uitvoeren van de toepassing voor Android
    - Installeer [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) of hoger. Zorg ervoor dat `JAVA_HOME` (omgevingsvariabele) correct ingesteld op basis van JDK-installatiepad (bijvoorbeeld C:\Program Files\Java\jdk1.7.0_75).
    - [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) installeren en toevoegen `<android-sdk-location>\tools` locatie (bijvoorbeeld C:\tools\Android\android-sdk\tools) op uw `PATH` omgevingsvariabele.
    - Open beheer van Android SDK (bijvoorbeeld via een terminal: `android`) en installeren
    - *Android 5.0.1 (21 API)* platform SDK
    - *Build android SDK-hulpprogramma's* van versie 19.1.0 of hoger
    - *Ondersteuning voor android-opslagplaats* (Extra)

  Android sdk biedt een standaardexemplaar emulator niet. Maak een nieuwe door `android avd` van de terminal en *maken...* te selecteren als u wilt uitvoeren van de Android app op emulator. Aanbevolen *Niveau Api* is 19 of hoger, Zie AVD Manager (http://developer.android.com/tools/help/avd-manager.html) voor meer informatie over opties voor Android-emulator en maken.


## <a name="1--register-an-application-with-azure-ad"></a>*1. een toepassing met Azure Active Directory registreren*

Opmerking: deze __stap is optioneel__. De zelfstudie verstrekt vooraf ingerichte waarden waarmee u het monster in actie zien zonder een inrichting in uw eigen huurder. Het wordt echter aanbevolen dat u deze stap uitvoeren en vertrouwd zijn met het proces, zoals dit vereist is als u uw eigen toepassingen maken.

Azure AD verleent slechts tokens voor bekende toepassingen. Voordat u Azure AD van uw app kunt, moet u een vermelding maken voor het in de huurder.  Een nieuwe toepassing in uw huurder, registreren

-   Aanmelden bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de nav links op in **Active Directory**
-   Selecteer de huurder waar u wilt dat om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klik op **toevoegen** in de onderste lade.
-   Volg de aanwijzingen en maak een nieuwe **Toepassing Native Client** (ondanks het feit dat Cordova apps HTML gebaseerd zijn, zijn we dus hier native clienttoepassing maken `Native Client Application` optie moet zijn ingeschakeld, anders wordt de toepassing werkt niet).
    -   De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    -   De **URI voor omleiden** is de URI tokens terug naar uw app gebruikt. Voer `http://MyDirectorySearcherApp`.

Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke client-id.  Moet u deze waarde in de volgende secties: deze kunt u vinden in het tabblad **configureren** van de zojuist gemaakte app.

Om te kunnen starten `DirSearchClient Sample`, nieuwe app toestemming te vragen de _Azure AD Graph API_:
-   Ga naar de sectie 'Machtigingen voor andere toepassingen' in het tabblad **configureren** .  Voeg de machtiging voor **toegang tot de map als de gebruiker ingelogd** onder **Overgedragen machtigingen**voor de toepassing 'Azure Active Directory'.  Hierdoor kan uw toepassing kan een query uitvoeren op de Graph API voor gebruikers.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. de monster app bibliotheek vereist voor de zelfstudie klonen*

Vanuit de shell of vanaf de opdrachtregel, typt u de volgende opdracht:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Maak de app Cordova*

Er zijn verschillende manieren voor het maken van Cordova toepassingen. In deze zelfstudie gebruiken we de Cordova command line interface (CLI).
Vanuit de shell of vanaf de opdrachtregel, typt u de volgende opdracht:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Dat maakt de mapstructuur en steiger voor het project van Cordova, de inhoud van het project starter in de www submap te kopiëren.
Verplaatsen naar de nieuwe map in de DirSearchClient.

    cd .\DirSearchClient

Voeg de "witte" lijst plugin nodig om de Graph API aan te roepen.

     cordova plugin add cordova-plugin-whitelist

Voeg vervolgens alle platforms die u wilt ondersteunen. Om een voorbeeld van een werkmap, moet u ten minste één van de onderstaande opdrachten uitvoeren. Houd er rekening mee dat u niet zal kunnen emuleren iOS op Windows of Windows/Windows Phone op een Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Ten slotte kunt u het ADAL voor Cordova-invoegtoepassing toevoegen aan uw project.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. code toevoegen om te verifiëren van gebruikers en het verkrijgen van tokens van AAD*

De toepassing die u in deze zelfstudie ontwikkelt krijgt u een zoekfunctie bZijn bone directory waar de eindgebruiker kan Typ de alias van de gebruiker in Active directory en visualiseren van bepaalde fundamentele kenmerken.  De starter project bevat de definitie van de basic-gebruikersinterface van een toepassing (in www/index.html) en de steiger die kabels van fundamentele app gebeurtenis cycli, gebruikersinterface bindingen en resultaten weergeven logica (in www/js/index.js). Het enige wat u bent vergeten is de uitvoering van taken identiteit logica toevoegen.

Het eerste wat dat u moet doen is in te voeren in de code de protocol-waarden die door AAD worden gebruikt voor het identificeren van uw app en de bronnen die u als doel. Deze waarden worden gebruikt om later op de token aanvragen samen te stellen. Voeg het fragment hieronder helemaal boven aan het bestand index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

De `redirectUri` en `clientId` waarden moeten overeenkomen met de waarden die met een beschrijving van uw app in AAD. Vindt u die in het tabblad configureren in Azure portal, zoals beschreven in stap 1 eerder in deze handleiding.
Opmerking: als u ervoor hebt gekozen voor het registreren van een nieuwe toepassing niet in uw eigen huurder, kunt u gewoon plakken de vooraf geconfigureerde waarden hierboven is - waarmee u voor een overzicht van het voorbeeld uitvoeren, maar moet u altijd uw eigen vermelding maken voor uw apps bedoeld voor de productie.

Vervolgens moet de werkelijke tokenaanvraag code toevoegen. Voeg het volgende fragment tussen de `search `en `renderdata `definities.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
We bekijken die functie door deze te splitsen in de twee belangrijkste onderdelen.
In dit voorbeeld is ontworpen voor gebruik met de huurder, in om te worden gekoppeld aan een bepaalde plaats. Het eindpunt '/ common', kan de gebruiker een account invoeren tijdens verificatie en stuurt de aanvraag wordt de huurder behoort.
Dit eerste deel van de methode inspecteert de ADAL cache om te zien of er al een token opgeslagen is - en als er, de huurder die afkomstig is van de voor het opnieuw initialiseren van ADAL wordt gebruikt. Dit is nodig om te voorkomen dat extra aanwijzingen op het scherm, zoals het gebruik van "/ gemeenschappelijk" altijd resulteert in het vraagt de gebruiker een nieuwe account invoeren.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Het tweede deel van de methode voert de juiste tokewn-aanvraag.
De `acquireTokenSilentAsync` methode vraagt naar ADAL aan een token voor de opgegeven resource retourneren zonder dat eventuele UX Dat kan gebeuren als de cache al een geschikte toegangstoken is opgeslagen, of als er een token vernieuwen die kan worden gebruikt om een nieuwe toegangstoken zonder shwoing prompt.
Als die poging mislukt, we terugvallen op `acquireTokenAsync` -die zichtbaar wordt gevraagd of de gebruiker te verifiëren.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Nu dat we het token hebben, kunnen we ten slotte de Graph API aanroepen en de zoekopdracht die we willen uitvoeren. Voeg het volgende fragment rechts onder de `authenticate` definitie.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Het eerste punt-bestanden geleverd een barebone UX voor de alias van de gebruiker invoert in een textbox. Deze methode wordt die waarde aan een query maken, combineren met het toegangstoken verzenden naar de grafiek en de resultaten worden geparseerd. De methode renderData, al in het eerste punt-bestand zorgt voor het visualiseren van de resultaten.

## <a name="4-run"></a>*4. run*
Uw app is ten slotte kan worden uitgevoerd. Het besturingssysteem is zeer eenvoudig: zodra de toepassing wordt gestart, voert u in het vak de alias van de gebruiker die u wilt opzoeken - Klik op de knop. U wordt om verificatiegegevens worden gevraagd. Bij een geslaagde verificatie en zoeken, worden de kenmerken van de gebruiker gezocht weergegeven. Latere uitvoeringen wordt de zoekactie uitvoeren zonder dat u elke prompt, dankzij de aanwezigheid in de cache van de eerder verkregen token weergegeven.
De concrete stappen voor het uitvoeren van de app verschillen per platform.

####<a name="windows-10"></a>Windows 10:

   Tablet PC:`cordova run windows --archs=x64 -- --appx=uap`

   Mobiel (Windows10 Mobile-apparaat aangesloten op PC vereist):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Opmerking__: tijdens de eerste uitvoering wordt u mogelijk gevraagd aan te melden voor een ontwikkelaarslicentie. Zie [Developer-licentie](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) voor meer informatie.

####<a name="windows-81-tabletpc"></a>Windows 8.1 Tablet PC:

   `cordova run windows`

   __Opmerking__: tijdens de eerste uitvoering wordt u mogelijk gevraagd aan te melden voor een ontwikkelaarslicentie. Zie [Developer-licentie](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) voor meer informatie.

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Uit te voeren op het aangesloten apparaat:`cordova run windows --device -- --phone`

   Uit te voeren op standaard-emulator:`cordova emulate windows -- --phone`

   Gebruik `cordova run windows --list -- --phone` voor een overzicht van alle beschikbare doelen en `cordova run windows --target=<target_name> -- --phone` toepassing uit te voeren op een specifiek apparaat of een emulator (bijvoorbeeld: `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Uit te voeren op het aangesloten apparaat:`cordova run android --device`

   Uit te voeren op standaard-emulator:`cordova emulate android`

   __Opmerking__: Zorg ervoor dat u gemaakte emulator exemplaar *AVD Manager* gebruiken zoals deze is weergegeven in de sectie *vereisten* .

   Gebruik `cordova run android --list` voor een overzicht van alle beschikbare doelen en `cordova run android --target=<target_name>` toepassing uit te voeren op een specifiek apparaat of een emulator (bijvoorbeeld: `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Uit te voeren op het aangesloten apparaat:`cordova run ios --device`

   Uit te voeren op standaard-emulator:`cordova emulate ios`

   __Opmerking__: Zorg ervoor dat u hebt `ios-sim` pakket geïnstalleerd op de emulator wordt uitgevoerd. Zie de sectie *vereisten* voor meer informatie.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Gebruik `cordova run --help` Zie extra samenstellen en uitvoeren van opties.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  U kunt nu verplaatsen in geavanceerde scenario's (ok en interessanter).  U kunt proberen:

[Een Node.js Web API met Azure Active Directory beveiligen >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
