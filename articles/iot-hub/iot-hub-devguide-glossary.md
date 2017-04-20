<properties
 pageTitle="Handleiding voor ontwikkelaars - verklarende woordenlijst | Microsoft Azure"
 description="Een woordenlijst met veelvoorkomende termen die betrekking hebben op de IoT Hub"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="glossary-of-iot-hub-terms"></a>Woordenlijst met termen die IoT Hub

In dit artikel worden een aantal algemene termen die zijn gekoppeld aan de IoT Hub.

## <a name="advanced-message-queueing-protocol-amqp"></a>Geavanceerde Message Queuing-Protocol (AMQP)

[AMQP](https://www.amqp.org/) is een van de messaging-protocollen IoT Hub om te communiceren met apparaten ondersteunt. Zie voor meer informatie over de messaging-protocollen IoT Hub ondersteunt, [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Wolk apparaat (C2D)

Meestal gebruikt om te verwijzen naar berichten van IoT Hub voor een aangesloten apparaat. Deze berichten zijn vaak opdrachten de opdracht geven het apparaat een actie te ondernemen. Zie voor meer informatie [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Voorwaarde

Verwijst naar apparaat staat informatie, zoals de verbindingsmethode die momenteel in gebruik, zoals gemeld door een apparaat app. Apparaten kunnen ook hun mogelijkheden melden. U kunt query toestand en het apparaat twin-mogelijkheid.

## <a name="data-point-message"></a>Gegevenspunt bericht

Een gegevenspunt bericht is een cloud-to-device-bericht met telemetriegegevens zoals windsnelheid of temperatuur.

## <a name="desired-properties"></a>Gewenste eigenschappen

Gewenste eigenschappen worden in de context van twins apparaat te synchroniseren met de apparaatconfiguratie of voorwaarde gebruikt in combinatie met de *eigenschappen vermeld* . Gewenste eigenschappen kunnen alleen worden ingesteld door de toepassing back end en worden waargenomen door de app van het apparaat. 

## <a name="device-to-cloud-d2c"></a>Apparaat cloud (D2C)

Meestal gebruikt om te verwijzen naar berichten die worden verzonden vanaf een aangesloten apparaat op IoT-Hub. Deze berichten mogelijk [gegevenspunt](#data-point-message) of [interactieve](#interactive-message) berichten. Zie voor meer informatie [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Register van apparaat-id

Het [register van apparaat-id](iot-hub-devguide-identity-registry.md) is de ingebouwde component van een IoT-hub met informatie over de afzonderlijke apparaten verbinding mogen maken met een hub.

## <a name="device"></a>Apparaat

In de context van IoT, een apparaat is meestal een kleinschalige, zelfstandige computing dat kan gegevens verzamelen of andere apparaten besturen. Bijvoorbeeld een apparaat mogelijk, een environmental monitoring-apparaat of een domeincontroller voor het drenken en ventilatie systemen in een uitstoot van broeikasgassen.

## <a name="device-twin"></a>Apparaat twin

Een [apparaat twin](iot-hub-devguide-device-twins.md) is een kopie in IoT-Hub van de voorwaarde en de configuratie-instellingen van een fysiek apparaat. U kunt een apparaat twin voor het beheren van de configuratie van het fysieke apparaat.

## <a name="direct-method"></a>Directe methode

Een [directe methode](iot-hub-devguide-direct-methods.md) is een manier voor het starten van een methode uit te voeren op een apparaat door het aanroepen van een API op uw hub-IoT.

## <a name="event-hub-compatible-endpoint"></a>Gebeurtenis Hub-compatibele eindpunt

U kunt lezen van apparaat-wolk-berichten worden verzonden naar uw IoT hub, verbinding maken met een eindpunt op de hub en elke gebeurtenis Hub-compatibele methode gebruiken om deze berichten te lezen. Gebeurtenis Hub-compatibele methoden omvatten de gebeurtenis Hubs SDK's en Azure Stream Analytics.

## <a name="field-gateway"></a>Veld gateway

Een veld gateway maakt connectiviteit mogelijk voor apparaten die geen verbinding maken met IoT Hub rechtstreeks en wordt meestal gedistribueerd lokaal met uw apparaten. Zie voor meer informatie [Wat is Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Taak

Uw oplossing voor back-end kunt taken gebruiken voor het plannen en volgen van activiteiten op een reeks apparaten die zijn geregistreerd bij de hub IoT. Activiteiten zijn bijwerken van apparaat twin de gewenste eigenschappen en methoden direct oproepen apparaat twin labels bijwerken.

## <a name="protocol-gateway"></a>Gateway protocol

Een gateway protocol is gewoonlijk geïmplementeerd in de cloud en protocol biedt vertaalservices voor apparaten die verbinding maken met IoT Hub. Zie voor meer informatie [Wat is Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interactieve bericht

Een interactieve bericht is een wolk naar apparaat bericht dat een onmiddellijke actie in de back-end van toepassing start. Een apparaat kan bijvoorbeeld een waarschuwing over een fout die automatisch moet worden vastgelegd in een CRM-systeem verzenden.

## <a name="iot-hub"></a>IoT Hub

IoT Hub is een volledig beheerde Azure service waarmee een betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en een oplossing voor back-end. Zie voor meer informatie [Wat is Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT Suite

Azure IoT Suite pakketten samen meerdere Azure services met vooraf geconfigureerde oplossingen. Deze vooraf geconfigureerde oplossingen kunnen u snel aan de slag met end-to-end-implementaties van algemene scenario's voor IoT. Zie voor meer informatie [Wat is Azure IoT Suite?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Taak

Een [taak](iot-hub-devguide-jobs.md) in IoT Hub kunt u bewerkingen uitvoeren zoals een firmware upgrade via meerdere apparaten zijn aangesloten op de hub.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) is een van de messaging-protocollen IoT Hub om te communiceren met apparaten ondersteunt. Zie voor meer informatie over de messaging-protocollen IoT Hub ondersteunt, [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Gerapporteerde eigenschappen

Gerapporteerde eigenschappen worden in de context van twins apparaat gebruikt in combinatie met de *gewenste eigenschappen* te synchroniseren met de apparaatconfiguratie of voorwaarde. Gerapporteerde eigenschappen kunnen kan alleen worden ingesteld door de toepassing van het apparaat en worden gelezen en ondervraagd door de toepassing back-end.

## <a name="tags"></a>Tags

Tags zijn in het kader van devcie twins apparaat metagegevens opgeslagen en opgehaald door de toepassing back-end in de vorm van een JSON-document. Tags zijn niet zichtbaar voor apps op een apparaat.

## <a name="system-properties"></a>Systeemeigenschappen

In het kader van het apparaat twins Systeemeigenschappen zijn alleen-lezen en bevatten informatie over het gebruik van het apparaat zoals de laatste activiteit en de verbinding staat.