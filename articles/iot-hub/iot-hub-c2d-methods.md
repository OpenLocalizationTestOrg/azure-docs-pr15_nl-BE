<properties
 pageTitle="Directe methoden | Microsoft Azure"
 description="In deze zelfstudie wordt beschreven hoe u directe methoden u"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Zelfstudie: Directe methoden gebruiken

## <a name="introduction"></a>Inleiding

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige bi-directionele communicatie tussen miljoenen IoT apparaten en een toepassing back-end. Vorige zelfstudies ([aan de slag met IoT Hub] en [wolk naar apparaat verzenden met IoT Hub]) illustreren de apparaat-cloud- en cloud-to-device messaging basisfunctionaliteit van IoT Hub. IoT Hub biedt u ook de mogelijkheid om niet-duurzame methoden op apparaten uit de cloud. Methoden geven een aanvraag-antwoord interactie met een apparaat, vergelijkbaar met een HTTP-oproep in die zin dat zij slagen of (zodra een gebruiker opgegeven time-out) mislukken aan de gebruiker duidelijk de status van de oproep. [Een directe methode aanroept op een apparaat] [ lnk-devguide-methods] methoden nader worden beschreven en biedt richtlijnen over het gebruik van methoden versus cloud-to-device-berichten.

Deze zelfstudie toont u hoe te:

- Gebruik de Azure portal maken een IoT hub en de identiteit van een apparaat in uw hub IoT maken.
- Een gesimuleerde apparaat met een directe methode die kan worden aangeroepen door de cloud maken.
- Maak een consoletoepassing die een directe methode wordt aangeroepen op het gesimuleerde apparaat via de hub IoT.

> [AZURE.NOTE] Op dit moment is zijn rechtstreekse methoden alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

Aan het einde van deze zelfstudie hebt u twee Node.js consoletoepassingen:

* **CallMethodOnDevice.js**, die een methode aanroept voor het gesimuleerde apparaat en het antwoord wordt weergegeven.
* **SimulatedDevice.js**, die zijn verbonden met de hub IoT met de identiteit van het apparaat eerder hebt gemaakt en reageert op de methode die wordt aangeroepen door de cloud.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de diverse SDK's waarmee u kunt beide toepassingen uit te voeren op apparaten en back-end van uw oplossing te bouwen.

Deze zelfstudie hebt u het volgende nodig:

+ Node.js versie 0.10.x of hoger.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaat app maken

In dit gedeelte maakt u een Node.js console app die op een methode die wordt aangeroepen door de cloud reageert.

1. Maak een nieuwe lege map met de naam **simulateddevice**. Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **simulateddevice** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **simulateddevice** en voer de volgende opdracht installeert de **azure iot apparaat** het apparaat SDK pakket en **azure iot-apparaat mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **SimulatedDevice.js** -bestand in de map **simulateddevice** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **SimulatedDevice.js** :

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Een variabele **connectionString** toevoegen en deze gebruiken voor het maken van een device-client. **{Apparaat verbindingsreeks}** vervangen door de verbindingstekenreeks die u in de sectie *maken een apparaat-id* gegenereerd:

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. De volgende functie als u de methode implementeert op het apparaat wilt toevoegen:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Open de verbinding met de IoT hub en start initialiseren de listener methode:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Opslaan en sluiten van het bestand **SimulatedDevice.js** .

> [AZURE.NOTE] Deze zelfstudie implementeert om dingen eenvoudig te houden, niet elk beleid opnieuw. In de productiecode opnieuw beleid (bijvoorbeeld verbinding opnieuw), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]te nemen[lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Het aanroepen van een methode op een apparaat

In dit gedeelte maakt u een Node.js console app die een methode aanroept voor het gesimuleerde apparaat en het antwoord wordt weergegeven.

1. Maak een nieuwe lege map met de naam **callmethodondevice**. Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **callmethodondevice** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **callmethodondevice** en voer de volgende opdracht om de **azure-iothub** -pakket te installeren:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Maak met een teksteditor een bestand **CallMethodOnDevice.js** in de map **callmethodondevice** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **CallMethodOnDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Voeg de volgende declaratie van de variabele en vervang de aanduidingswaarde van de tijdelijke door de verbindingsreeks voor de hub IoT:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. U opent de verbinding op uw hub IoT maken.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Voeg de volgende functie aanroepen van de methode van het apparaat en de reactie van het apparaat naar de console afdrukken:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Opslaan en sluiten van het bestand **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij een opdrachtprompt in de map **simulateddevice** , kunt u de volgende opdracht om te beginnen met luisteren voor methodeaanroepen van de IoT Hub uitvoeren:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. Bij een opdrachtprompt in de map **callmethodondevice** , de volgende opdracht om te beginnen met de IoT hub controleren:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Het apparaat reageert op de methode door het afdrukken van het bericht en de toepassing die de methode display het antwoord van het apparaat wordt weergegeven:

    ![][9]
    
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, een nieuwe IoT hub in de Azure portal worden geconfigureerd en wordt gemaakt van de identiteit van een apparaat in het register van de hub van de identiteit. U gebruikt deze apparaat-id voor het gesimuleerde apparaat app om te reageren op de methoden die worden aangeroepen door de cloud. U ook een app roept methoden op het apparaat en de reactie van het apparaat wordt gemaakt. 

Zie om door te gaan aan de slag met IoT-Hub en andere IoT scenario's verkennen:

- [Aan de slag met IoT Hub]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Zie informatie over het uitbreiden van uw oplossing en planning methode op meerdere apparaten roept IoT, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Berichten met IoT Hub wolk naar apparaat verzenden]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
