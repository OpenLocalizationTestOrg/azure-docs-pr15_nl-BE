<properties
 pageTitle="Optionele sectie - wijzigen van de in- en uit het gedrag van de LED | Microsoft Azure"
 description="De berichten te wijzigen van de LED's in- en uitschakelen gedrag aanpassen."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 optionele sectie: de aan-en uit het gedrag van de LED

## <a name="421-what-you-will-do"></a>4.2.1 wat u doet

De berichten te wijzigen van de LED's in- en uitschakelen gedrag aanpassen. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="422-what-you-will-learn"></a>4.2.2 Wat leert u

Extra Node.js functies gebruiken voor het wijzigen van de LED's in- en uitschakelen van gedrag.

## <a name="423-what-you-need"></a>4.2.3 wat u nodig hebt

U moet hebben voltooid [4.1 uitvoert een voorbeeldtoepassing op uw Pi frambozen cloud om berichten te ontvangen](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 Node.js functies toevoegen

1. De voorbeeldtoepassing openen in Visual Studio-code door de volgende opdrachten:

    ```bash
    cd Lesson4
    code .
    ```

2. Open de `app.js` bestand en voegt u aan het einde de volgende functies:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![app.js bijwerken](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Toevoegen van de volgende voorwaarden voordat u de standaardcode in het blok switch case van de `receiveMessageCallback` functie:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    U hebt nu de voorbeeldtoepassing om te reageren op meer instructies door middel van berichten geconfigureerd. De LED wordt de instructie 'op' en de instructie "off" schakelt de LED.

4. Open het bestand gulpfile.js en voeg vervolgens een nieuwe functie voor de functie `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![gulpfile bijwerken](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. In de `sendMessage` werken, vervangen door de regel `var message = buildMessage(sentMessageCount);` met de nieuwe regel in het volgende fragment wordt getoond:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Alle wijzigingen worden opgeslagen.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 implementeren en uitvoeren van de voorbeeldtoepassing

Implementeren en uitvoeren van de voorbeeldtoepassing op uw Pi door de volgende opdracht uit te voeren:

```bash
gulp
```

Hier ziet u de LED gedurende twee seconden ingeschakeld en vervolgens uitgeschakeld voor de andere twee seconden. Het laatste bericht van 'stop' stopt de voorbeeldtoepassing wordt uitgevoerd.

![in- en uitschakelen](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Gefeliciteerd! Je hebt de berichten die worden verzonden naar de Pi van de hub IoT is aangepast.

### <a name="427-summary"></a>4.2.7 samenvatting

Deze optionele sectie demo's van de berichten aanpassen zodat de voorbeeldtoepassing de aan- en uit het gedrag van de LED op een andere manier bepalen kunt.

