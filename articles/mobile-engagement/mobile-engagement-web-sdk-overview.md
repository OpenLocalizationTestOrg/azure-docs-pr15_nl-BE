<properties
    pageTitle="Azure betrokkenheid bij de mobiele Web SDK-overzicht | Microsoft Azure"
    description="De meest recente updates en procedures voor de Web SDK voor Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure betrokkenheid bij de mobiele Web SDK

Start hier voor alle details over het integreren van Azure Mobile Engagement in een web app. Als u eens proberen wilt voordat u aan de slag met uw eigen web app, Zie onze [zelfstudie 15 minuten](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Integratie-procedures
1. Informatie over [het integreren van Mobile Engagement in uw web app](mobile-engagement-web-integrate-engagement.md).

2. Informatie over [het gebruik van geavanceerde mobiele aanstelling API in uw web app tags](mobile-engagement-web-use-engagement-api.md)voor code-implementatie plan.

## <a name="release-notes"></a>Release-opmerkingen

### <a name="202-10182016"></a>2.0.2 (18-10/2016)

-   Vaste crash op InPrivate navigatie (Safari).
-   Vaste crash op browsers die met cookies uitgeschakeld.

Zie de [volledige release-opmerkingen](mobile-engagement-web-release-notes.md)voor alle versies.

## <a name="upgrade-procedures"></a>Upgradeprocedures

### <a name="upgrade-from-121-to-200"></a>1.2.1 upgraden naar 2.0.0

In de volgende secties wordt beschreven hoe u een Mobile Engagement Web SDK-integratie van de Capptain-service aangeboden door Capptain SAS, een app Azure Mobile Engagement migreren. Als u van een versie ouder dan 1.2.1 migreert, neem contact op met de website Capptain om te migreren naar 1.2.1 eerst en vervolgens de volgende procedures toe te passen.

Deze versie van de SDK van Mobile Engagement Web ondersteunt geen Samsung Smart TV, tv-Opera, webOS of de functie bereiken.

>[AZURE.IMPORTANT] Capptain en Azure Mobile Engagement zijn niet dezelfde service en selecteert u de volgende procedures alleen het migreren van de clienttoepassing. Migratie van de Mobile Engagement Web SDK in de app wordt niet uw gegevens migreren van een Capptain-server naar een server voor Mobile Engagement.

#### <a name="javascript-files"></a>JavaScript-bestanden

Het bestand capptain-sdk.js vervangen door het bestand azure-engagement.js en vervolgens het script invoer dienovereenkomstig bijwerken.

#### <a name="remove-capptain-reach"></a>Capptain bereik verwijderen

Deze versie van de SDK van Mobile Engagement Web ondersteunt de Reach-functie niet. Capptain bereik hebt geïntegreerd in uw toepassing, moet u deze verwijderen.

Het importeren van CSS bereiken verwijderen vanaf de pagina en het bijbehorende CSS-bestand (capptain-reach.css, standaard) verwijderen.

De volgende Reach-bronnen verwijderen: de sluiten (capptain-close.png, standaard) en het pictogram merk (standaard capptain-melding-pictogram).

De gebruikersinterface voor het bereiken van-app-meldingen te verwijderen. De standaard lay-out ziet er zo uit:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Verwijder de UI bereiken voor tekst en web aankondigingen en stemmingen. De standaard lay-out ziet er zo uit:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Verwijder de `reach` object uit uw configuratie, als deze bestaat. Als volgt uitziet:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Andere aanpassingen met Reach, zoals categorieën verwijderen.

#### <a name="remove-deprecated-apis"></a>Afgekeurde API's verwijderen

Sommige API's Capptain zijn in de SDK van Mobile Engagement Web afgeschaft.

Verwijder alle oproepen naar de volgende API's: `agent.connect`, `agent.disconnect`, `agent.pause`, en `agent.sendMessageToDevice`.

Een van de volgende callbacks verwijderen uit de configuratie van de Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, en `onPushMessageReceived`.

#### <a name="configuration"></a>Configuratie

Betrokkenheid bij de mobiele wordt een verbindingsreeks SDK-id's, bijvoorbeeld de toepassings-id configureren.

De toepassings-ID vervangen door de verbindingsreeks. Merk op dat het globale object voor de SDK-configuratie wordt gewijzigd van `capptain` op `azureEngagement`.

Voor de migratie:

    window.capptain = {
      appId: ...,
      [...]
    };

Na de migratie:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

De verbindingsreeks voor de toepassing wordt weergegeven in de portal Azure.

#### <a name="javascript-apis"></a>JavaScript-API 's

De globale JavaScript-object `window.capptain` heeft gekregen `window.azureEngagement`, maar u kunt de `window.engagement` alias voor API-aanroepen. U kunt deze alias niet gebruiken voor het definiëren van de SDK-configuratie.

Bijvoorbeeld, `capptain.deviceId` wordt `engagement.deviceId`, `capptain.agent.startActivity` wordt `engagement.agent.startActivity`, enzovoort.

U hebt al een eerdere versie van de SDK Azure Mobile Engagement Web geïntegreerd in uw toepassing, lees dan over het [bijwerken van de procedures](mobile-engagement-web-upgrade-procedure.md).
