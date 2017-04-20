<properties
 pageTitle="IoT Hub bewerkingen controleren"
 description="Een overzicht van bewerkingen Azure IoT Hub controleren, zodat u de status van bewerkingen op de hub IoT in real-time controleren"
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
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Inleiding tot het controleren van bewerkingen

Bewerkingen van IoT Hub controleren kunt u de status van bewerkingen op de hub IoT in real-time controleren. U kunt ervoor kiezen in het verzenden van gebeurtenissen uit een of meer categorieën naar een eindpunt van de hub IoT voor verwerking en IoT Hub bijgehouden gebeurtenissen tussen verschillende categorieën van bewerkingen. U kunt de gegevens op fouten controleren of meer complexe verwerking op basis van gegevenspatronen instellen.

IoT Hub bewaakt vijf soorten gebeurtenissen:

- Bewerkingen van apparaat-id
- Telemetrie apparaat
- Cloud-to-device-opdrachten
- Verbindingen
- Uploaden van bestanden

## <a name="how-to-enable-operations-monitoring"></a>Het inschakelen van bewerkingen controleren

1. Maak een hub IoT. U vindt instructies voor het maken van een hub IoT in de [Slag] [ lnk-get-started] gids.

2. Open het blad van de hub IoT. Daar klikt u op het **controleren van bewerkingen**.

    ![][1]

3. Selecteer de monitoring categorieën die u wilt u controleren en klik vervolgens op **Opslaan**. De gebeurtenissen zijn beschikbaar voor het lezen van de gebeurtenis Hub-compatibele eindpunt in de **controle-instellingen**weergegeven. Het eindpunt IoT Hub heet `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Gebeurteniscategorieën en hoe ze te gebruiken

Elke categorie tracks monitoring bewerkingen een ander type interactie met IoT Hub en elke controle categorie is een schema dat bepaalt hoe de gebeurtenissen in die categorie zijn gestructureerd.

### <a name="device-identity-operations"></a>Bewerkingen van apparaat-id

De apparaatcategorie identiteit-bewerkingen worden bijgehouden voor fouten die optreden wanneer u probeert te maken, bijwerken of verwijderen van een vermelding in het register van de IoT-hub identiteit. Bijhouden van deze categorie is nuttig voor het inrichten van scenario's.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Telemetrie apparaat

De apparaatcategorie telemetrie fouten die optreden bij de hub IoT en hebben betrekking op de pijpleiding telemetrie bijgehouden. Deze categorie bevat fouten die optreden bij het verzenden van telemetrie-gebeurtenissen (bijvoorbeeld een beperking) en telemetrie-gebeurtenissen (bijvoorbeeld onbevoegde lezer) ontvangen. Houd er rekening mee dat deze categorie fouten veroorzaakt door code die wordt uitgevoerd op het apparaat zelf niet kan onderscheppen.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Cloud-to-device-opdrachten

De categorie cloud-to-device opdrachten fouten die optreden bij de hub IoT en hebben betrekking op de opdracht apparaat pijpleiding wordt bijgehouden. Deze categorie bevat fouten die zich bij het verzenden van opdrachten (zoals niet-toegestane afzender), ontvangen opdrachten (zoals levering aantal overschreden) en ontvangen van feedback van de opdracht (zoals de feedback verlopen). Deze categorie onderscheppen niet fouten van een apparaat dat niet goed omgaat met een opdracht als u de opdracht met succes is afgeleverd.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Verbindingen

De categorie verbindingen houdt de fouten die optreden wanneer apparaten verbinding maken of verbreken met een hub IoT. Bijhouden van deze categorie is handig voor het identificeren van ongeoorloofde verbindingen en voor het bijhouden van wanneer een verbinding verbroken voor apparaten in de gebieden van een slechte verbinding wordt.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Uploaden van bestanden

Het bestand uploaden categorie fouten die optreden bij de IoT hub en zijn gerelateerd aan het bestand uploaden functionaliteit worden bijgehouden. Deze categorie bevat fouten die met de URI SAS optreden (zoals wanneer verloopt voordat een apparaat de hub van een voltooide uploaden meldt), mislukte uploads gerapporteerd door het apparaat, en wanneer u een bestand niet is gevonden in opslag tijdens het maken van IoT Hub melding bericht. Houd er rekening mee dat deze categorie fouten die rechtstreeks optreden terwijl het apparaat een bestand naar de opslag uploaden is kan niet werkelijk.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Volgende stappen

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md