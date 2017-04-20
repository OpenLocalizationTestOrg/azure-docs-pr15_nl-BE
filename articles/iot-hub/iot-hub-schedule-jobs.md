<properties
 pageTitle="Hoe u taken wilt plannen | Microsoft Azure"
 description="In deze zelfstudie wordt aangegeven hoe u taken wilt plannen"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Zelfstudie: Plannen en broadcast-taken (voorbeeld)

## <a name="introduction"></a>Inleiding
Azure IoT Hub is een volledig beheerde service waarmee een back-end toepassing maken en bijhouden van taken die plannen en miljoenen apparaten bij te werken.  Taken kunnen worden gebruikt voor de volgende acties:

- Apparaateigenschappen twin gewenst bijwerken
- Bijwerken van apparaat twin tags
- Cloud-to-device-methoden

In dat geval een taak loopt een van deze acties en de voortgang van de uitvoering voor een aantal apparaten, die wordt gedefinieerd door een query twin.  Bijvoorbeeld een taak met een back-end van toepassing kan een reboot methode aanroept op 10.000 apparaten, die door een query twin en op een later tijdstip gepland.  Die toepassing kan vervolgens de voortgang bijhouden als elk van die apparaten ontvangen en de methode voor opnieuw uitvoeren.

Meer informatie over elk van deze mogelijkheden in deze artikelen:

- Apparaat twin en eigenschappen: [aan de slag met twin] [ lnk-get-started-twin] en [Zelfstudie: het gebruik van dubbele eigenschappen][lnk-twin-props]
- Cloud-to-device-methoden: [Developer guide - methoden direct] [ lnk-dev-methods] en [Zelfstudie: C2D methoden][lnk-c2d-methods]

Deze zelfstudie toont u hoe te:

- Een gesimuleerde apparaat met een directe methode waarmee lockDoor die kan worden aangeroepen door de toepassing back end kunt maken.
- Maak een consoletoepassing die de directe methode lockDoor aangeroepen voor het gesimuleerde apparaat met behulp van een project en de gewenste twin eigenschappen met behulp van een apparaat bijwerkt.

Aan het einde van deze zelfstudie hebt u twee Node.js consoletoepassingen:

**simDevice.js**, die verbinding maakt met de IoT hub met de identiteit van het apparaat en een lockDoor directe methode ontvangt.

**scheduleJobService.js**, die een directe methode wordt aangeroepen op het gesimuleerde apparaat en van de twin disired eigenschappen met behulp van een taak bijwerken.

Deze zelfstudie hebt u het volgende nodig:

Node.js versie 0.12.x of hoger <br/>  [Bereid uw ontwikkelomgeving] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie te installeren op Windows of Linux.

Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaat app maken

In dit gedeelte maakt u een Node.js console app die reageert op een directe methode aangeroepen door de wolk die gesimuleerde apparaat opnieuw activeert en gebruikt het apparaat twin gerapporteerd eigenschappen naar het apparaat twee query's voor apparaten en wanneer ze laatst opnieuw is opgestart.

1. Maak een nieuwe lege map met de naam **simDevice**.  Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **simDevice** .  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **simDevice** en voer de volgende opdracht installeert de **azure iot apparaat** het apparaat SDK pakket en **azure iot-apparaat mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **simDevice.js** -bestand in de map **simDevice** .

4. Voeg de volgende 'vereist' instructies aan het begin van het bestand **simDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Een variabele **connectionString** toevoegen en deze gebruiken voor het maken van een device-client.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Voeg de volgende functie voor het afhandelen van de methode lockDoor.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Voeg de volgende code voor het registreren van de handler voor de lockDoor-methode.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Opslaan en sluiten van het bestand **simDevice.js** .

> [AZURE.NOTE] Deze zelfstudie implementeert om dingen eenvoudig te houden, niet elk beleid opnieuw. In de productiecode opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]te nemen[lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Taken plannen voor een directe methode wordt aangeroepen en het bijwerken van de eigenschappen van een twin

In dit gedeelte vindt u een Node.js console app waarmee een externe lockDoor op een apparaat via een directe methode eigenschappen maken en bijwerken van de twin.

1. Maak een nieuwe lege map met de naam **scheduleJobService**.  Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **scheduleJobService** .  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **scheduleJobService** en voer de volgende opdracht installeert de **azure iothub** het apparaat SDK pakket en **azure iot-apparaat mqtt** :

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Met behulp van een teksteditor, maak een nieuwe **scheduleJobService.js** -bestand in de map **scheduleJobService** .

4. Voeg de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_gscheduleJobServiceetstarted_service.js** :

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Voeg de volgende variabele declaraties en de waarden van de tijdelijke aanduiding vervangen door:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Voeg de volgende functie die wordt gebruikt voor het controleren van de uitvoering van de taak:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Voeg de volgende code om het plannen van de taak die de methode van het apparaat:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Voeg de volgende code als u de taak voor het bijwerken van de twin plannen:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Opslaan en sluiten van het bestand **scheduleJobService.js** .

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij de opdrachtprompt in de map **simDevice** , de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.

    ```
    node simDevice.js
    ```

2. Bij de opdrachtprompt in de map **scheduleJobService** , moet u de volgende opdracht uitvoeren aan het extern opnieuw opstarten en de query voor het apparaat te vinden van de laatste tijd opnieuw trigger.

    ```
    node scheduleJobService.js
    ```

3. Ziet u de uitvoer van het apparaat en de back-end-toepassingen.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een taak voor het plannen van een directe methode op een apparaat en het bijwerken van de eigenschappen van het apparaat-twin.

Zie het volgende om door te gaan aan de slag met IoT Hub en device management patronen zoals extern via de lucht firmware-update:

[Zelfstudie: Hoe u een firmware-update][lnk-fwupdate]

Zie [aan de slag met de SDK IoT Gateway]om door te gaan aan de slag met IoT Hub[lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx