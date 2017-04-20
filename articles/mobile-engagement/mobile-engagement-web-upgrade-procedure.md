<properties
    pageTitle="Azure Mobile Engagement Web SDK upgradeprocedures | Microsoft Azure"
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
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure upgradeprocedures van Mobile Engagement Web SDK

Als u hebt al een eerdere versie van de SDK Azure Mobile Engagement Web geïntegreerd in uw webtoepassing, moet u rekening houden met de volgende punten wanneer u een upgrade uitvoert van de SDK.

Als u meerdere versies van de SDK van Mobile Engagement Web overgeslagen, moet u mogelijk een aantal procedures uitvoeren tijdens de upgrade. Bijvoorbeeld als u van 1.4.0 naar 1.6.0 migreert, eerst als volgt de procedures voor het bijwerken van 1.4.0 naar 1.5.0. Volg de procedures voor het bijwerken van 1.5.0 naar 1.6.0.

Welke versie u een upgrade van uitvoert een eerdere versie van het bestand azure-engagement.js vervangen door de meest recente versie van het bestand.

## <a name="upgrade-from-121-to-200"></a>1.2.1 upgraden naar 2.0.0

In deze sectie wordt beschreven hoe een Mobile Engagement Web SDK-integratie van de Capptain-service aangeboden door Capptain SAS, een app Azure Mobile Engagement migreren. Als u een migratie vanuit een eerdere versie, raadpleegt u de website Capptain u eerst naar 1.2.1 migreren en vervolgens de volgende procedures toe te passen.

Deze versie van de SDK van Mobile Engagement Web ondersteunt geen Samsung Smart TV, tv-Opera, webOS of de functie bereiken.

>[AZURE.IMPORTANT] Capptain en Azure Mobile Engagement zijn niet dezelfde service. De volgende procedure wordt alleen het migreren van de clienttoepassing gemarkeerd. Migratie van de Mobile Engagement Web SDK in de app wordt niet uw gegevens migreren van een Capptain-server naar een server voor Mobile Engagement.

### <a name="javascript-files"></a>JavaScript-bestanden

Het bestand capptain-sdk.js vervangen door het bestand azure-engagement.js en vervolgens het script invoer dienovereenkomstig bijwerken.

### <a name="remove-capptain-reach"></a>Capptain bereik verwijderen

Deze versie van de SDK van Mobile Engagement Web ondersteunt de Reach-functie niet. Capptain bereiken is geïntegreerd in uw toepassing, moet u deze verwijderen.

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

### <a name="remove-deprecated-apis"></a>Afgekeurde API's verwijderen

Sommige API's Capptain zijn in de SDK van Mobile Engagement Web afgeschaft.

Verwijder alle oproepen naar de volgende API's: `agent.connect`, `agent.disconnect`, `agent.pause`, en `agent.sendMessageToDevice`.

Alle exemplaren van de volgende callbacks verwijderen uit de configuratie van de Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, en `onPushMessageReceived`.

### <a name="configuration"></a>Configuratie

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

De verbindingsreeks voor de toepassing wordt weergegeven in de Portal Azure.

### <a name="javascript-apis"></a>JavaScript-API 's

De globale JavaScript-object `window.capptain` heeft gekregen `window.azureEngagement` , maar u kunt de `window.engagement` alias voor API-aanroepen. U kunt de alias niet gebruiken voor de SDK-configuratie definiëren.

Bijvoorbeeld, `capptain.deviceId` wordt `engagement.deviceId`, `capptain.agent.startActivity` wordt `engagement.agent.startActivity`, enzovoort.
