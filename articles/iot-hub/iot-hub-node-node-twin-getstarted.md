<properties
    pageTitle="Aan de slag met twins | Microsoft Azure"
    description="In deze zelfstudie wordt u met twins"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Zelfstudie: Aan de slag met apparaat twins (voorbeeld)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelfstudie hebt u twee Node.js consoletoepassingen:

* **AddTagsAndQuery.js**, een app Node.js is bedoeld om te worden uitgevoerd vanuit de back-end die codes worden toegevoegd en twins apparaat-query's.
* **TwinSimulatedDevice.js**, een Node.js app die overeenkomt met een apparaat dat op uw hub IoT met de identiteit van het apparaat eerder hebt gemaakt verbindt en rapporten de toestand van de verbinding.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de diverse SDK's waarmee u kunt apparaat en de back-end-toepassingen te bouwen.

Deze zelfstudie hebt u het volgende nodig:

+ Node.js versie 0.10.x of hoger.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>De service app maken

In dit gedeelte maakt u een Node.js console app die locatie metagegevens worden toegevoegd aan de twin **myDeviceId**gekoppeld. Vervolgens de twins opgeslagen in de hub te selecteren van de apparaten zich in de Verenigde Staten en die die het melden van een mobiele verbinding query's.

1. Maak een nieuwe lege map met de naam **addtagsandqueryapp**. Maak een nieuw package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **addtagsandqueryapp** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **addtagsandqueryapp** en voer de volgende opdracht om de **azure-iothub** -pakket te installeren:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **AddTagsAndQuery.js** -bestand in de map **addtagsandqueryapp** .

4. Voeg de volgende code in het bestand **AddTagsAndQuery.js** en vervangen door een tijdelijke aanduiding voor de **{verbindingsreeks service}** met de verbindingstekenreeks die u hebt gekopieerd bij het maken van uw hub:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    Het **register** -object bevat alle methoden die nodig zijn om te communiceren met twins apparaat van de service. De vorige code initialiseert eerst het object **register** en vervolgens haalt de twin voor **myDeviceId**en ten slotte de tags wordt bijgewerkt met informatie over de gewenste locatie.

    Na het bijwerken van de tags roept de functie **queryTwins** .

7. De volgende code toevoegen aan het einde van de **AddTagsAndQuery.js** voor de uitvoering van de functie **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    De vorige code twee query's wordt uitgevoerd: alleen de twins van apparaten die zich in de plant **Redmond43** apparaat selecteert u de eerste en de tweede verfijning in de query, schakelt u alleen de apparaten die ook zijn verbonden via een netwerk voor mobiele telefoons.

    Houd er rekening mee dat de vorige code bij het maken van de **query** -object een maximum aantal geretourneerde documenten geeft. Het **query** -object bevat een Booleaanse eigenschap van **hasMoreResults** die u gebruiken kunt om op te roepen de methoden **nextAsTwin** meerdere malen om alle resultaten te halen. Een methode met de **volgende** naam is beschikbaar voor resultaten die niet twins apparaat bijvoorbeeld de resultaten van query's samenvoegen.

8. Start de toepassing met:

        node AddTagsAndQuery.js

    Hier ziet u een apparaat in de resultaten van de query gevraagd voor alle apparaten die zich in **Redmond43** en geen voor de query die de resultaten beperkt tot apparaten die gebruikmaken van een netwerk voor mobiele telefoons.

    ![][1]

In het volgende gedeelte maakt u een apparaat app die rapporteert de verbindingsgegevens en wordt het resultaat van de query in de vorige sectie.

## <a name="create-the-device-app"></a>Het apparaat app maken

In dit gedeelte maakt u een console app die is verbonden met de hub als **myDeviceId**en vervolgens bijgewerkt van de twin eigenschappen bevatten de informatie die is verbonden met een netwerk voor mobiele telefoons gerapporteerd Node.js.

> [AZURE.NOTE] Op dit moment zijn apparaat twins alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

1. Maak een nieuwe lege map met de naam **reportconnectivity**. Maak een nieuw package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **reportconnectivity** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **reportconnectivity** en voer de volgende opdracht om de **azure iot apparaten**en **azure iot-apparaat mqtt** pakket te installeren:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **ReportConnectivity.js** -bestand in de map **reportconnectivity** .

4. Voeg de volgende code in het bestand **ReportConnectivity.js** en vervangen door een tijdelijke aanduiding voor de **{verbindingsreeks apparaat}** met de verbindingstekenreeks die u bij het maken van de identiteit **myDeviceId** apparaat gekopieerd:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Het **Client** -object bevat alle methoden die u nodig hebt om te communiceren met twins apparaat van het apparaat. De vorige code na het initialiseren van het object **Client** haalt de twin voor de **myDeviceId** en de gerapporteerde eigenschap bijgewerkt met de gegevens over de connectiviteit.

5. De toepassing van het apparaat uitvoeren

        node ReportConnectivity.js

    Ziet u het bericht `twin state reported`.

6. Nu dat de gegevens over de connectiviteit gerapporteerd, moet deze worden weergegeven in beide query's. Ga terug in de map **addtagsandqueryapp** en voer de query opnieuw uit:

        node AddTagsAndQuery.js

    Deze tijd **myDeviceId** moet worden weergegeven in de resultaten van beide query.

    ![][3]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie, een nieuwe IoT hub in de Azure portal worden geconfigureerd en wordt gemaakt van de identiteit van een apparaat in het register van de hub van de identiteit. U apparaat metagegevens toegevoegd als labels vanuit een back-endtoepassing en een app gesimuleerde apparaat naar apparaat verbinding rapportgegevens in de twin apparaat geschreven. Ook hebt u geleerd hoe u deze gegevens met behulp van de IoT Hub querytaal zoals SQL query.

Gebruik de volgende bronnen voor meer informatie over hoe u kunt:

- Telemetrie van apparaten met de [aan de slag met IoT Hub] verzenden[ lnk-iothub-getstarted] zelfstudie,
- apparaten configureren met de gewenste eigenschappen van twin met de [gewenste eigenschappen voor het configureren van apparaten gebruiken] [ lnk-twin-how-to-configure] zelfstudie,
- besturen van apparaten interactief (bijvoorbeeld uitschakelt op een fan van een door de gebruiker gecontroleerde app), met de [rechtstreekse methoden gebruik] [ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md