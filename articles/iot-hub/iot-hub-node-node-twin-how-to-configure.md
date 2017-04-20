<properties
    pageTitle="Gebruik eigenschappen twin | Microsoft Azure"
    description="In deze zelfstudie wordt u met twin-eigenschappen"
    services="iot-hub"
    documentationCenter=".net"
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

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Zelfstudie: Gewenste eigenschappen gebruiken voor het configureren van apparaten (voorbeeld)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Aan het einde van deze zelfstudie hebt u twee Node.js consoletoepassingen:

* **SimulateDeviceConfiguration.js**, een gesimuleerde apparaat app dat wacht op een update van de gewenste configuratie en de status van een gesimuleerde configuratieproces update rapporteert.
* **SetDesiredConfigurationAndQuery.js**, een app Node.js is bedoeld om te worden uitgevoerd vanuit de back-end, die de gewenste configuratie ingesteld op een apparaat en het configuratieproces van de update-query's.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de diverse SDK's waarmee u kunt apparaat en de back-end-toepassingen te bouwen.

Deze zelfstudie hebt u het volgende nodig:

+ Node.js versie 0.10.x of hoger.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

Als u het [aan de slag met apparaat twins] gevolgd[ lnk-twin-tutorial] zelfstudie, u hebt al een hub apparaat beheer is ingeschakeld en de identiteit van een apparaat genaamd **myDeviceId**; en u kunt verdergaan met het [maken van het gesimuleerde apparaat app] [ lnk-how-to-configure-createapp] sectie.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>De gesimuleerde apparaat app maken

In dit gedeelte maakt u een Node.js console app waarmee verbindt met de hub als **myDeviceId**, wacht op een update van de gewenste configuratie en updates op de gesimuleerde configuratieproces update.

1. Maak een nieuwe lege map met de naam **simulatedeviceconfiguration**. Maak een nieuw package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **simulatedeviceconfiguration** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **simulatedeviceconfiguration** en voer de volgende opdracht om de **azure iot apparaten**en **azure iot-apparaat mqtt** pakket te installeren:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **SimulateDeviceConfiguration.js** -bestand in de map **simulatedeviceconfiguration** .

4. Voeg de volgende code in het bestand **SimulateDeviceConfiguration.js** en vervangen door een tijdelijke aanduiding voor de **{verbindingsreeks apparaat}** met de verbindingstekenreeks die u bij het maken van de identiteit **myDeviceId** apparaat gekopieerd:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    Het **Client** -object bevat alle methoden die nodig zijn om te communiceren met een apparaat twins van het apparaat. De vorige code, nadat het initialiseren van het object **Client** haalt de twin voor **myDeviceId**en voegt een handler voor de update op de gewenste eigenschappen. De handler wordt gecontroleerd of dat er een werkelijke configuratieaanvraag is door vergelijking van de configIds en vervolgens roept een methode aan die de wijziging in de configuratie wordt gestart.

    Houd er rekening mee dat omwille van de eenvoud in de vorige code wordt een hardcoded standaardinstelling voor de eerste configuratie. Een reële app zou waarschijnlijk die configuratie laden vanaf een lokale opslag.
    
    > [AZURE.IMPORTANT] Gewenste eigenschap gebeurtenissen altijd één keer worden uitgestoten bij apparaatverbinding, zorg ervoor dat u controleert of er een werkelijke wijziging in de gewenste eigenschappen voor elke actie uitvoeren.

5. Toevoegen van de volgende methoden voordat u de `client.open()` aanroepen:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    De methode **initConfigChange** gemeld eigenschappen op het lokale twin-object bijgewerkt met de updateaanvraag config en wordt de status ingesteld op **in behandeling**, wordt de twin apparaat van de service bijgewerkt. Nadat de twin met succes zijn bijgewerkt, wordt een langdurige proces dat wordt beëindigd als de uitvoering van de **completeConfigChange**gesimuleerd. Deze methode werkt u de lokale twin gerapporteerde eigenschappen instellen van de status voor **succes** en het **pendingConfig** -object verwijderen. Vervolgens werkt u het dubbele van de service.

    Die opslaan bandbreedte, gemeld eigenschappen worden bijgewerkt door te geven alleen de eigenschappen te wijzigen (benoemde **patch** in de bovenstaande code), in plaats van het hele document te vervangen.

    > [AZURE.NOTE] Deze zelfstudie is niet een probleem voor de configuratie van gelijktijdige updates simuleren. Sommige configuratie update processen mogelijk aangepast aan de veranderingen in de doelconfiguratie terwijl de update wordt uitgevoerd, anderen hebben kunnen ze in de wachtrij en anderen met een fout weigeren kunnen. Zorg ervoor dat u rekening houden met het gewenste gedrag voor uw specifieke configuratie-proces en de juiste logica toevoegen voordat u de wijziging in de configuratie wordt gestart.

6. De app van het apparaat worden uitgevoerd:

        node SimulateDeviceConfiguration.js

    Ziet u het bericht `retrieved device twin`. Houd de app actief.

## <a name="create-the-service-app"></a>De service app maken

In dit gedeelte maakt u een Node.js console app waarmee de *gewenste eigenschappen* op de dubbele **myDeviceId** met een nieuwe telemetrie-configuratie-object gekoppeld. Vervolgens vraagt de twins van het apparaat opgeslagen in de hub en bevat het verschil tussen de gewenste en gerapporteerde configuraties van het apparaat.

1. Maak een nieuwe lege map met de naam **setdesiredandqueryapp**. Maak een nieuw package.json-bestand met de volgende opdracht bij de opdrachtprompt in de map **setdesiredandqueryapp** . Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **setdesiredandqueryapp** en voer de volgende opdracht om de **azure-iothub** -pakket te installeren:

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Met behulp van een teksteditor, maak een nieuwe **SetDesiredAndQuery.js** -bestand in de map **addtagsandqueryapp** .

4. Voeg de volgende code in het bestand **SetDesiredAndQuery.js** en vervangen door een tijdelijke aanduiding voor de **{verbindingsreeks service}** met de verbindingstekenreeks die u hebt gekopieerd bij het maken van uw hub:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    Het **register** -object bevat alle methoden die nodig zijn om te communiceren met twins apparaat van de service. De vorige code na het initialiseren van het object **register** haalt de twin voor de **myDeviceId**en de gewenste eigenschappen wordt bijgewerkt met een nieuwe telemetrie-configuratieobject. Daarna roept de functie **queryTwins** gebeurtenis 10 seconden.

    > [AZURE.IMPORTANT] Deze toepassing een query IoT Hub elke 10 seconden ter illustratie. Met query's gebruikerspagina om rapporten te genereren over veel apparaten en niet om wijzigingen te herkennen. Als uw oplossing moet real-time meldingen van apparaatgebeurtenissen met de [apparaat-wolk - berichten][lnk-d2c].

5. Toevoegen de volgende code rechts vóór het `registry.getDeviceTwin()` aanroep voor het implementeren van de functie **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    De vorige code query's de twins opgeslagen in de hub en de gewenste en gerapporteerde telemetrie-configuraties worden afgedrukt. Verwijzen naar de [querytaal IoT Hub] [ lnk-query] voor meer informatie over het uitgebreide rapporten genereren over al uw apparaten.


8. Met **SimulateDeviceConfiguration.js** uitgevoerd, start u de toepassing met:

        node SetDesiredAndQuery.js 5m

    U ziet nu de wijziging in de configuratie van **succes** op **in behandeling** voor **succes** opnieuw met de nieuwe actieve frequentie van vijf minuten in plaats van 24 uur verzonden.

    > [AZURE.IMPORTANT] Er is een vertraging van maximaal een minuut tussen de bewerking van het apparaat en het resultaat van de query. Dit is de infrastructuur van de query om te werken met zeer hoge schaal inschakelen. U haalt met consistente weergave van een enkele twin de **getDeviceTwin** -methode in de klasse van het **register** .

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, een gewenste configuratie als de *gewenste eigenschappen* instellen van een back-endtoepassing en schreef een gesimuleerde apparaat app om te detecteren die wijzigen en een rapportage zijn status als *Eigenschappen gerapporteerd* aan de twin meerledige updateproces te simuleren.

Gebruik de volgende bronnen voor meer informatie over hoe u kunt:

- Telemetrie van apparaten met de [aan de slag met IoT Hub] verzenden[ lnk-iothub-getstarted] zelfstudie,
- plannen of bewerkingen uitvoeren op grote verzamelingen van apparaten Zie [taken gebruiken voor het plannen en uitzending van apparaat] [ lnk-schedule-jobs] zelfstudie.
- besturen van apparaten interactief (bijvoorbeeld uitschakelt op een fan van een door de gebruiker gecontroleerde app), met de [rechtstreekse methoden gebruik] [ lnk-methods-tutorial] zelfstudie.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
