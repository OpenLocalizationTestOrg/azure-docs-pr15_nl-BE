<properties
    pageTitle="Azure IoT Hub voor Node.js aan de slag | Microsoft Azure"
    description="Azure IoT Hub met Node.js aan zelfstudie is gestart. Azure IoT Hub en Node.js met Microsoft Azure IoT SDK's gebruiken voor het implementeren van een oplossing voor Internet van dingen."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Aan de slag met Azure IoT Hub voor Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het einde van deze zelfstudie hebt u drie Node.js consoletoepassingen:

* **CreateDeviceIdentity.js**, die een apparaat identiteit en de bijbehorende beveiligingssleutel het gesimuleerde apparaat verbinding maakt.
* **ReadDeviceToCloudMessages.js**, waarin de telemetrie verzonden door het gesimuleerde apparaat.
* **SimulatedDevice.js**, die zijn verbonden met de hub IoT met de identiteit van het apparaat eerder hebt gemaakt en een bericht telemetrie elke tweede met behulp van het protocol van AMQP.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de diverse SDK's waarmee u kunt beide toepassingen uit te voeren op apparaten en back-end van uw oplossing te bouwen.

Deze zelfstudie hebt u het volgende nodig:

+ Node.js versie 0.10.x of hoger.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw hub IoT gemaakt. U hebt de hostnaam IoT Hub en de IoT Hub verbindingstekenreeks die u nodig hebt voor de rest van deze zelfstudie.

## <a name="create-a-device-identity"></a>Een apparaat-id maken

In dit gedeelte maakt u een Node.js console app waarmee de identiteit van een apparaat in het register van de identiteit in de hub IoT. Een apparaat kan geen verbinding maken met IoT hub tenzij er een vermelding in het register van de identiteit apparaat. Raadpleeg de sectie **Apparaat identiteit register** van [IoT Hub Developer Guide] [ lnk-devguide-identity] voor meer informatie. Wanneer u deze consoletoepassing uitvoert, wordt een uniek apparaat-ID en de sleutel die het apparaat gebruiken kunt om zichzelf te identificeren bij het apparaat naar cloud-berichten met IoT Hub verzenden gegenereerd.

1. Maak een nieuwe lege map met de naam **createdeviceidentity**. Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **createdeviceidentity** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **createdeviceidentity** , voer de volgende opdracht om de **azure iothub** Service SDK-pakket installeren:

    ```
    npm install azure-iothub --save
    ```

3. Maak met een teksteditor een bestand **CreateDeviceIdentity.js** in de map **createdeviceidentity** .

4. Voeg de volgende `require` instructie aan het begin van het bestand **CreateDeviceIdentity.js** :

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Voeg de volgende code in het bestand **CreateDeviceIdentity.js** en vervang de aanduidingswaarde van de tijdelijke door de verbindingsreeks voor de IoT-hub die u in de vorige sectie hebt gemaakt: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Voeg de volgende code om een definitie van het apparaat in het register van de identiteit apparaat in uw hub IoT maken. Deze code maakt u een apparaat als de apparaat-id bestaat niet in het register hebben, anders wordt de sleutel van het apparaat:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Opslaan en sluiten **CreateDeviceIdentity.js** bestand.

8. Voor het uitvoeren van de toepassing van de **createdeviceidentity** , de volgende opdracht bij de opdrachtprompt worden uitgevoerd in de map createdeviceidentity:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Maak een notitie van de **apparaat-id** en **sleutel apparaat**. U moet deze waarden later bij het maken van een toepassing die is verbonden met de IoT Hub als een apparaat.

> [AZURE.NOTE] Het register van de identiteit IoT Hub slaat alleen apparaat-id's voor beveiligde toegang tot de hub. Toetsen wilt gebruiken als gebruikersnaam en een vlag ingeschakeld/uitgeschakeld, kunt u de toegang voor een bepaald apparaat uitschakelen en apparaat-id's worden opgeslagen. Als uw toepassing nodig heeft voor het andere apparaat-specifieke metagegevens opslaan, moet er een specifieke winkel gebruiken. Zie handleiding voor [ontwikkelaars] van IoT Hub[ lnk-devguide-identity] voor meer informatie.

## <a name="receive-device-to-cloud-messages"></a>Apparaat-wolk-berichten ontvangen

In dit gedeelte maakt u een Node.js console app die van IoT Hub apparaat-wolk-berichten. Een hub IoT beschrijft een [Gebeurtenis Hubs][lnk-event-hubs-overview]-compatibel eindpunt zodat u kunt apparaat-wolk-berichten lezen. Om dingen eenvoudig te houden, maakt deze zelfstudie basic lezer is niet geschikt voor de implementatie van een hoge gegevensdoorvoer. Het [apparaat naar cloud-berichten verwerken] [ lnk-process-d2c-tutorial] handleiding hoe u apparaat-wolk-berichten op een schaal te verwerken. Het is [Aan de slag met Hubs gebeurtenis] [ lnk-eventhubs-tutorial] zelfstudie biedt meer informatie over het op berichten van gebeurtenis Hubs en is van toepassing op de eindpunten IoT Hub gebeurtenis Hub-compatibel.

> [AZURE.NOTE] De gebeurtenis Hub-compatibele eindpunt voor het lezen van apparaat-wolk-berichten altijd het AMQP-protocol wordt gebruikt.

1. Maak een nieuwe lege map met de naam **readdevicetocloudmessages**. Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **readdevicetocloudmessages** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **readdevicetocloudmessages** , de volgende opdracht om de **azure-gebeurtenis-hubs** -pakket te installeren:

    ```
    npm install azure-event-hubs --save
    ```

3. Maak met een teksteditor een bestand **ReadDeviceToCloudMessages.js** in de map **readdevicetocloudmessages** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **ReadDeviceToCloudMessages.js** :

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Voeg de volgende declaratie van de variabele en vervang de aanduidingswaarde van de tijdelijke door de verbindingsreeks voor de hub IoT:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Voeg de volgende twee functies die in de console afdrukken:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Voeg de volgende code maakt de **EventHubClient**, opent de verbinding met de IoT-Hub en een ontvanger voor elke partitie maken. Deze toepassing gebruikt een filter wanneer een ontvanger wordt gemaakt zodat de ontvanger wordt alleen berichten worden verzonden naar de IoT gelezen Hub nadat de ontvanger is gestart. Dit filter is handig in een testomgeving, zodat u alleen de huidige set berichten zien. In een productieomgeving, moet uw code ervoor zorgen dat berichten verwerkt - Zie de [IoT Hub apparaat naar cloud berichten verwerken] [ lnk-process-d2c-tutorial] zelfstudie voor meer informatie:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Opslaan en sluiten van het bestand **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaat app maken

In dit gedeelte maakt u een Node.js console app die overeenkomt met een apparaat dat een hub IoT apparaat-wolk-berichten verstuurt.

1. Maak een nieuwe lege map met de naam **simulateddevice**. Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **simulateddevice** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **simulateddevice** en voer de volgende opdracht installeert de **azure iot apparaat** het apparaat SDK pakket en **azure iot-apparaat amqp** :

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **SimulatedDevice.js** -bestand in de map **simulateddevice** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **SimulatedDevice.js** :

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Een variabele **connectionString** toevoegen en deze gebruiken voor het maken van een device-client. **{Youriothostname}** vervangen door de naam van de hub IoT u de sectie *maken een IoT Hub* gemaakt. **{Yourdevicekey}** vervangen door de apparaat-sleutelwaarde die u in de sectie *maken een apparaat-id* gegenereerd:

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Voeg de volgende functie om uitvoer van de toepassing weer te geven:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Een retouraanroep maken en de functie **setInterval** verzenden elke seconde een nieuw bericht op uw hub IoT:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

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

8. De verbinding met de IoT Hub openen en beginnen met het verzenden van berichten:

    ```
    client.open(connectCallback);
    ```

9. Opslaan en sluiten van het bestand **SimulatedDevice.js** .

> [AZURE.NOTE] Deze zelfstudie implementeert om dingen eenvoudig te houden, niet elk beleid opnieuw. In de productiecode opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]te nemen[lnk-transient-faults].


## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij een opdrachtprompt in de map **readdevicetocloudmessages** , de volgende opdracht om te beginnen met de IoT hub controleren:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js IoT Hub service de clienttoepassing apparaat naar cloud berichten controleren][7]

2. Bij een opdrachtprompt in de map **simulateddevice** , kunt u de volgende opdracht om te beginnen met telemetriegegevens worden verzonden naar uw hub IoT uitvoeren:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js IoT Hub device-clienttoepassing apparaat-wolk-berichten verzenden][8]

3. De tegel voor **Gebruik** in de [Azure portal] [ lnk-portal] geeft het aantal berichten die naar de hub:

    ![Azure portal gebruik tegel met aantal berichten voor IoT Hub][43]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, een nieuwe IoT hub in de Azure portal worden geconfigureerd en wordt gemaakt van de identiteit van een apparaat in het register van de hub van de identiteit. U gebruikt deze apparaat-id voor het gesimuleerde apparaat app apparaat-wolk-berichten verzenden naar de hub. U ook een app die de berichten die zijn ontvangen door de hub wordt gemaakt. 

Zie om door te gaan aan de slag met IoT-Hub en andere IoT scenario's verkennen:

- [Aansluiten van een apparaat][lnk-connect-device]
- [Aan de slag met Apparaatbeheer][lnk-device-management]
- [Aan de slag met de SDK IoT Gateway][lnk-gateway-SDK]

Zie informatie over het uitbreiden van uw oplossing IoT en apparaat naar cloud-berichten op een schaal, de [berichten verwerken-apparaat naar cloud] [ lnk-process-d2c-tutorial] zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
