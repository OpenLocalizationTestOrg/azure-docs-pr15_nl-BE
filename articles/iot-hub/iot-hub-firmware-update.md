<properties
 pageTitle="Hoe u een firmware-update | Microsoft Azure"
 description="In deze zelfstudie wordt aangegeven hoe u een firmware-update"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Zelfstudie: Hoe een firmware update (voorbeeld)

## <a name="introduction"></a>Inleiding
In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u met de [apparaat-twin] [ lnk-devtwin] en [methoden van wolk naar apparaat (C2D)] [ lnk-c2dmethod] primitieven op afstand een apparaat opnieuw wordt opgestart. Deze zelfstudie maakt gebruik van de dezelfde IoT Hub primitieven en biedt hulp en ziet u hoe u een end-to-end gesimuleerde firmware-update.  Dit patroon wordt gebruikt bij de uitvoering van firmware-update voor het apparaat Intel Edison monster.

Deze zelfstudie toont u hoe te:

- Maak een consoletoepassing die de directe methode firmwareUpdate op het gesimuleerde apparaat via de hub IoT.
- Een gesimuleerde apparaat dat een firmwareUpdate directe methode die een meerfasen-proces dat wacht op de firmware-image downloaden, downloadt de firmware-image doorloopt implementeert en ten slotte past th firmware-image maken.  Voortgang in elke fase het apparaat gebruikt de twin gerapporteerd eigenschappen bijwerken uitvoeren.

Aan het einde van deze zelfstudie hebt u twee Node.js consoletoepassingen:

**dmpatterns_fwupdate_service.js**, die een directe methode voor het gesimuleerde apparaat aanroept, wordt het antwoord weergegeven en regelmatig (elke 500ms) geeft de bijgewerkte apparaatstuurprogramma twin gerapporteerd eigenschappen.

**dmpatterns_fwupdate_device.js**, die verbinding maakt met de IoT-hub met de identiteit van het apparaat eerder hebt gemaakt, ontvangt u een firmwareUpdate directe methode, wordt uitgevoerd door een proces van meerdere simuleren een firmware-update inclusief: wachten op de afbeelding downloaden, downloaden van de nieuwe afbeelding en ten slotte de afbeelding toepassen.


Deze zelfstudie hebt u het volgende nodig:

Node.js versie 0.12.x of hoger <br/>  [Bereid uw ontwikkelomgeving] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie te installeren op Windows of Linux.

Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

Volg het artikel [aan de slag met Apparaatbeheer](iot-hub-device-management-get-started.md) de IoT hub maken en de verbindingsreeks ophalen.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaat app maken

In dit gedeelte maakt u een Node.js console app die reageert op een directe methode aangeroepen door de wolk die een firmware-update van het gesimuleerde apparaat activeert en gebruikt het apparaat twin gerapporteerd eigenschappen naar het apparaat twee query's voor apparaten en wanneer ze laatst opnieuw is opgestart.

1. Maak een nieuwe lege map met de naam **manageddevice**.  Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **manageddevice** .  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Voer de volgende opdracht om te installeren op de opdrachtprompt in de map **manageddevice** , de **azure-iot-device@dtpreview** apparaat SDK-pakket en **azure-iot-device-mqtt@dtpreview** pakket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **dmpatterns_fwupdate_device.js** -bestand in de map **manageddevice** .

4. Voeg de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_fwupdate_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Een variabele **connectionString** toevoegen en deze gebruiken voor het maken van een device-client.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Voeg de volgende functie die wordt gebruikt voor het bijwerken van dubbele eigenschappen gerapporteerd

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. De volgende functies waarmee u simuleren het downloaden en toepassen van de firmware afbeelding toevoegen.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Voeg de volgende functie update van de status van de firmware-update via de twin eigenschappen gemeld aan het wachten op downloaden.  Meestal apparaten in kennis worden gesteld van een update beschikbaar en een beheerder gedefinieerd beleid wordt het downloaden en toepassen van de update van het apparaat.  Dit is waar de logica voor het inschakelen van dat beleid zou uitvoeren.  Voor eenvoud, we 4 seconden vertraagd en procedure voor het downloaden van de firmware-image. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Voeg de volgende functie update van de status van de firmware-update via de twin eigenschappen gemeld bij het downloaden van de firmware-image.  Het volgt omhoog door een firmware download simuleren en ten slotte werkt de status van de firmware-update in kennis te stellen van een download geslaagd of mislukt.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Voeg de volgende functie update van de status van de firmware-update via de twin eigenschappen gemeld bij het toepassen van de firmware-image.  Het volgt omhoog door het simuleren van een toepassing van de firmware-image en ten slotte werkt de status van de firmware-update in kennis te stellen van een succes toepassen of de fout.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Voeg de volgende functoin die de methode firmwareUpdate en tijdens het bijwerken van de firmware van meerdere fasen tot stand brengen.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Ten slotte, voeg de volgende code waarmee verbinding met IoT hub als een apparaat, 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Deze zelfstudie implementeert om dingen eenvoudig te houden, niet elk beleid opnieuw. In de productiecode opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]te nemen[lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Een externe firmware-update op het apparaat via een directe methode activeren 

In dit gedeelte maakt u een Node.js console app die een externe firmware-update op een apparaat via een directe methode Start en het apparaat twee query's gebruikt om regelmatig de status van de actieve firmware-update op dat apparaat.


1. Maak een nieuwe lege map met de naam **triggerfwupdateondevice**.  Maak een package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **triggerfwupdateondevice** .  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **triggerfwupdateondevice** en voer de volgende opdracht installeert de **azure iothub** het apparaat SDK pakket en **azure iot-apparaat mqtt** :

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Met behulp van een teksteditor, maak een nieuwe **dmpatterns_getstarted_service.js** -bestand in de map **triggerfwupdateondevice** .

4. Voeg de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Voeg de volgende variabele declaraties en de waarden van de tijdelijke aanduiding vervangen door:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Toevoegen met de volgende functie te vinden en weergeven van de waarde van de firmwareUpdate eigenschap gerapporteerd.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. De volgende functie aanroepen van de methode firmwareUpdate start het doelapparaat opnieuw op als u wilt toevoegen:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Ten slotte is een toevoegen met de volgende functie code start de firmware bijwerken reeks en start regelmatig met de twin eigenschappen:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Opslaan en sluiten van het bestand **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij de opdrachtprompt in de map **manageddevice** , de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Bij de opdrachtprompt in de map **triggerfwupdateondevice** , moet u de volgende opdracht uitvoeren aan het extern opnieuw opstarten en de query voor het apparaat te vinden van de laatste tijd opnieuw trigger.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Ziet u het reageren op de directe methode door het afdrukken van het bericht

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, gebruikt u een directe methode voor het starten van een externe firmware-update op een apparaat en de twin gerapporteerd regelmatig gebruikt eigenschappen te begrijpen van de voortgang van de firmware bijwerken proces.  

Zie informatie over het uitbreiden van uw oplossing en planning methode op meerdere apparaten roept IoT, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
