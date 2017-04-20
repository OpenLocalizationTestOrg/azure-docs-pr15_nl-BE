<properties
    pageTitle="Wolk naar apparaat verzenden met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie voor meer informatie over het cloud-apparaat-berichten verzenden met Azure IoT Hub met Java."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Zelfstudie: Het verzenden van berichten met IoT Hub en Node.js wolk naar apparaat

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige bi-directionele communicatie tussen miljoenen IoT-apparaten inschakelen en weer een toepassing te beëindigen. De zelfstudie [aan de slag met IoT Hub] laat zien hoe een hub IoT maken, de identiteit van een apparaat in het inrichten en code een gesimuleerde apparaat dat apparaat-wolk-berichten verzonden.

Deze zelfstudie is gebaseerd op het [aan de slag met IoT Hub]. U ziet hoe aan:

- Vanuit uw toepassing cloud back-end wolk naar apparaat berichten naar een enkel apparaat via de IoT Hub te verzenden.
- Cloud-to-device-berichten op een apparaat ontvangen.
- Back-end uit de cloud toepassing, bevestiging van levering (*feedback*) aanvragen voor berichten die worden verzonden naar een apparaat van IoT Hub.

U vindt meer informatie over berichten wolk naar apparaat in de [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

Aan het einde van deze zelfstudie kunt u twee Node.js consoletoepassingen uitvoeren:

* **SimulatedDevice**, een gewijzigde versie van de app in [aan de slag met IoT Hub], die verbinding maakt met uw hub IoT en ontvangen van berichten van de wolk naar apparaat gemaakt.
* **SendCloudToDeviceMessage**, die een wolk naar apparaat-bericht verzendt naar het gesimuleerde apparaat via IoT Hub en vervolgens de bevestiging van de levering ontvangt.

> [AZURE.NOTE] IoT Hub heeft ondersteuning voor veel apparaat platforms en talen (zoals C, Java en Javascript) via Azure IoT apparaat SDK's SDK. Zie voor stapsgewijze instructies voor het aansluiten van uw apparaat code van deze zelfstudie, en in het algemeen Azure IoT Hub, [Azure IoT Developer Center].

Deze zelfstudie hebt u het volgende nodig:

+ Node.js versie 0.10.x of hoger.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Ontvangen van berichten op het gesimuleerde apparaat.

In deze sectie kunt u de toepassing van het gesimuleerde apparaat wijzigt u in [aan de slag met de IoT Hub] cloud naar apparaat om berichten te ontvangen van de hub IoT gemaakt.

1. Open het bestand SimulatedDevice.js in een teksteditor.

2. De functie **connectCallback** voor het afhandelen van berichten die worden verzonden vanaf de IoT Hub wijzigen. In dit voorbeeld wordt het apparaat altijd de **volledige** functie IoT Hub melden dat het bericht is worden verwerkt. De nieuwe versie van de **connectCallback** functie ziet er zo uit:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

    > [AZURE.NOTE] Als u HTTP in plaats van MQTT of AMQP als het transport gebruikt, de instantie **DeviceClient** berichten van IoT Hub zelden (minder dan 25 minuten elke) gecontroleerd. Zie voor meer informatie over de verschillen tussen MQTT, AMQP en HTTP-ondersteuning en het beperken van IoT Hub [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Een wolk naar apparaat verzenden.

In dit gedeelte maakt u een Node.js console app die cloud-to-device-berichten worden verzonden naar de toepassing van het gesimuleerde apparaat. U moet het apparaat-Id van het apparaat dat u hebt toegevoegd in de zelfstudie [aan de slag met IoT Hub] . U moet ook de verbindingsreeks voor de IoT-hub die u in de [portal Azure vinden kunt].

1. Maak een lege map met de naam **sendcloudtodevicemessage**. Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **sendcloudtodevicemessage** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **sendcloudtodevicemessage** en voer de volgende opdracht om de **azure-iothub** -pakket te installeren:

    ```
    npm install azure-iothub --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **SendCloudToDeviceMessage.js** -bestand in de map **sendcloudtodevicemessage** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **SendCloudToDeviceMessage.js** :

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Voeg de volgende code aan het bestand **SendCloudToDeviceMessage.js** . De connection string tijdelijke waarde vervangen door de verbindingsreeks voor de IoT-hub die u hebt gemaakt in de zelfstudie [aan de slag met IoT Hub] . Tijdelijke aanduiding voor de doel-apparaat vervangen door de apparaat-id van het apparaat dat u hebt toegevoegd in de zelfstudie [aan de slag met IoT Hub] :

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. De volgende functie als u wilt afdrukken, het resultaat van de bewerking aan de console toevoegen:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. De volgende functie om af te drukken Feedbackberichten levering aan de console toevoegen:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. De volgende code toevoegen om te verzenden een bericht naar uw apparaat en het Feedbackbericht verwerken wanneer het apparaat de cloud-to-device-bericht erkent:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Opslaan en sluiten **SendCloudToDeviceMessage.js** bestand.

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij de opdrachtprompt in de map **simulateddevice** , kunt u de volgende opdracht verzenden telemetrie op IoT Hub en luistert naar berichten die wolk naar apparaat uitvoeren:

    ```
    node SimulatedDevice.js 
    ```

    ![De toepassing van het gesimuleerde apparaat uitvoeren][img-simulated-device]

2. Uitvoeren bij een opdrachtprompt in de map **sendcloudtodevicemessage** , de volgende opdracht om een wolk naar apparaat verzenden en wachten op de bevestiging feedback:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Uitvoeren van de app voor het verzenden van de opdracht c2d][img-send-command]

    > [AZURE.NOTE] Deze zelfstudie implementeert voor eenvoud van mogelijk te houden, niet elk beleid opnieuw. In de productiecode te opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]nemen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe cloud-to-device-berichten verzenden en ontvangen. 

Voorbeelden van volledige end-to-end oplossingen met IoT Hub, Zie [Azure IoT Suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, [IoT Hub Developer Guide].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT Hub Developer Guide]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Fout met betrekking tot tijdelijke behandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/