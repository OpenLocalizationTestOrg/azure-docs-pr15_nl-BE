<properties
 pageTitle="Aan de slag met Apparaatbeheer | Microsoft Azure"
 description="In deze zelfstudie wordt beschreven hoe aan de slag met Apparaatbeheer op Azure IoT Hub u"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Zelfstudie: Aan de slag met Apparaatbeheer (voorbeeld)

## <a name="introduction"></a>Inleiding
IoT cloud-toepassingen kunnen gebruiken primitieven in Azure IoT Hub, namelijk de twin apparaat en de rechtstreekse methoden voor extern starten en device management acties op apparaten controleren.  Dit artikel bevat richtlijnen en code voor hoe een IoT cloud toepassing en een apparaat werken samen om te starten en een externe apparaat opnieuw starten met IoT Hub controleren.

Gebruiken als u wilt op afstand start device management acties op uw apparaten in een cloud-gebaseerde, back-end app controleren, Azure IoT Hub primitieven zoals [apparaat-twin] [ lnk-devtwin] en [methoden van wolk naar apparaat (C2D)][lnk-c2dmethod]. Deze zelfstudie toont u hoe een back-end-app en een apparaat samen werken kunnen zodat u tot stand brengen en het externe apparaat opnieuw starten van IoT Hub controleren.

U gebruikt een C2D te initiëren device management acties (zoals het opnieuw opstarten, factory reset en firmware-update) vanuit een back-end-app in de cloud. Het apparaat is verantwoordelijk voor:

- Verwerking van de aanvraag methode van IoT Hub.
- Het bijbehorende apparaat-specifieke actie op het apparaat wordt gestart.
- Statusupdates via het apparaat twin biedt eigenschappen IoT Hub gemeld.

U kunt een back-end-app in de cloud apparaat twin query's om te rapporteren over de voortgang van uw apparaat beheer acties uitvoeren.

Deze zelfstudie toont u hoe te:

- Gebruik de Azure portal maken een IoT Hub en de identiteit van een apparaat in uw hub IoT maken.
- Een gesimuleerde apparaat met een directe methode reboot die kan worden aangeroepen door de cloud kan maken.
- Maak een consoletoepassing die de directe methode opnieuw voor het gesimuleerde apparaat via de hub IoT aanroept.

Aan het einde van deze zelfstudie hebt u twee Node.js consoletoepassingen:

**dmpatterns_getstarted_device.js**, die verbinding met de IoT-hub met de identiteit van het apparaat eerder hebt gemaakt maakt, ontvangt een directe methode reboot simuleert een fysieke computer opnieuw wordt opgestart en rapporten van de tijd voor de laatste keer opnieuw opstarten.

**dmpatterns_getstarted_service.js**, die een directe methode wordt aangeroepen op het gesimuleerde apparaat, het antwoord weergegeven en ziet u de bijgewerkte apparaatstuurprogramma twin eigenschappen vermeld.

Deze zelfstudie hebt u het volgende nodig:

Node.js versie 0.12.x of hoger <br/>  [Bereid uw ontwikkelomgeving] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie te installeren op Windows of Linux.

Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaat app maken

In dit gedeelte maakt u een Node.js console app die reageert op een directe methode aangeroepen door de wolk die gesimuleerde apparaat opnieuw activeert en gebruikt het apparaat twin gerapporteerd eigenschappen naar het apparaat twee query's voor apparaten en wanneer ze laatst opnieuw is opgestart.

1. Maak een nieuwe lege map met de naam **manageddevice**.  Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **manageddevice** .  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Voer de volgende opdracht om te installeren op de opdrachtprompt in de map **manageddevice** , de **azure-iot-device@dtpreview** apparaat SDK-pakket en **azure-iot-device-mqtt@dtpreview** pakket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **dmpatterns_getstarted_device.js** -bestand in de map **manageddevice** .

4. Voeg de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_getstarted_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Een variabele **connectionString** toevoegen en deze gebruiken voor het maken van een device-client.  De tekenreeks vervangen door de verbindingsreeks voor het apparaat.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. De volgende functie om de directe methode implementeert op het apparaat toevoegen

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Open de verbinding met de IoT hub en de directe methode listener starten:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Opslaan en sluiten van het bestand **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Deze zelfstudie implementeert om dingen eenvoudig te houden, niet elk beleid opnieuw. In de productiecode opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]te nemen[lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een extern opnieuw opstarten op het apparaat via een directe methode trigger 

In dit gedeelte maakt u een Node.js console app die een extern opnieuw opstarten op een apparaat via een directe methode Start en het apparaat twee query's gebruikt om te zoeken naar de laatste keer opnieuw opstarten voor het apparaat.

1. Maak een nieuwe lege map met de naam **triggerrebootondevice**.  Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **triggerrebootondevice** .  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Voer de volgende opdracht om te installeren op de opdrachtprompt in de map **triggerrebootondevice** , de **azure-iothub@dtpreview** apparaat SDK-pakket en **azure-iot-device-mqtt@dtpreview** pakket:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Met behulp van een teksteditor, maak een nieuwe **dmpatterns_getstarted_service.js** -bestand in de map **triggerrebootondevice** .

4. Voeg de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Voeg de volgende variabele declaraties en de waarden van de tijdelijke aanduiding vervangen door:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Voeg de volgende functie aanroepen van de methode apparaat om het apparaat opnieuw te starten:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Voeg de volgende functie om te zoeken op het apparaat en de laatste keer dat opnieuw opstarten:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Voeg de volgende code om de functies die de directe methode opnieuw op te starten en een query voor het laatst opnieuw opstarten:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Opslaan en sluiten van het bestand **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij de opdrachtprompt in de map **manageddevice** , de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. Bij de opdrachtprompt in de map **triggerrebootondevice** , moet u de volgende opdracht uitvoeren aan het extern opnieuw opstarten en de query voor het apparaat te vinden van de laatste tijd opnieuw trigger.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Ziet u het reageren op de directe methode door het afdrukken van het bericht

## <a name="customize-and-extend-the-device-management-actions"></a>Aanpassen en uitbreiden van de apparaat beheer acties

IoT oplossingen kunnen uitbreiden van de gedefinieerde set device management patronen of aangepaste patronen met behulp van het apparaat twin en C2D methode primitieven inschakelen. Andere voorbeelden van acties voor apparaat zijn factory reset, firmware-update, software-update, power management, beheer van netwerk en connectiviteit en gegevenscodering.

## <a name="device-maintenance-windows"></a>Apparaat onderhoud windows

Meestal configureert u apparaten bewerkingen uit te voeren op een moment dat de onderbrekingen en de uitvaltijd tot een minimum beperkt.  Apparaat onderhoud windows zijn een veel gebruikte patroon voor het definiëren van de tijd wanneer een apparaat de configuratie moet worden bijgewerkt. Uw back-end-oplossingen kunnen u de gewenste eigenschappen van het apparaat twin gebruiken definiëren en activeren van een beleid op het apparaat waarmee u een onderhoudsvenster. Wanneer een apparaat het venster onderhoudsbeleid ontvangt, kunt deze de gerapporteerde eigenschap van het apparaat twin rapport van de status van het beleid. De back-end-app kunt apparaat twin query's behaald na de conformiteit van de apparatuur en elk beleid.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, gebruikt u een directe methode voor het starten van een extern opnieuw opstarten op een apparaat, gebruikt het apparaat twin eigenschappen voor aangifte van de laatste keer opnieuw opstarten van het apparaat gemeld en gevraagd voor het apparaat om te ontdekken de laatste keer opnieuw opstarten van het apparaat uit de cloud.

Zie het volgende om door te gaan aan de slag met IoT Hub en device management patronen zoals extern via de lucht firmware-update:

[Zelfstudie: Hoe u een firmware-update][lnk-fwupdate]

Zie informatie over het uitbreiden van uw oplossing en planning methode op meerdere apparaten roept IoT, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

Zie [aan de slag met de SDK IoT Gateway]om door te gaan aan de slag met IoT Hub[lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx