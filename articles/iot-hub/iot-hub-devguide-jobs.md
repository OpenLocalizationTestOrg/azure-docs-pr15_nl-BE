<properties
 pageTitle="Handleiding voor ontwikkelaars - projecten | Microsoft Azure"
 description="Azure IoT Hub developer guide - plannen van taken uit te voeren op meerdere apparaten aangesloten op de hub"
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

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Taken plannen op meerdere apparaten (voorbeeld)

## <a name="overview"></a>Overzicht

Zoals beschreven door de voorgaande artikelen, Azure IoT Hub kunt een aantal bouwstenen ([twin apparaateigenschappen en -codes] [ lnk-twin-devguide] en [methoden van de wolk naar apparaat][lnk-dev-methods]).  Meestal inschakelen back-end-toepassingen IoT apparaat beheerders en operators te werken en werken met IoT-apparaten in bulk en op een gepland tijdstip.  Taken inkapselen van de uitvoering van updates voor apparaatstuurprogramma twin en C2D methoden voor een aantal apparaten op een tijdstip plannen.  Een exploitant zou bijvoorbeeld een back-end-toepassing wilt starten en bijhouden van een project om een set van hulpmiddelen bij het bouwen van 43 en floor 3 op een tijdstip dat niet storend voor de bewerkingen van het gebouw opnieuw te gebruiken.

### <a name="when-to-use"></a>Wanneer gebruikt u

Houd rekening met behulp van taken wanneer: een oplossing voor back beëindigen moet plannen en volgen van de voortgang van een van de volgende activiteiten op een reeks apparaat:

- Apparaateigenschappen twin gewenst bijwerken
- Bijwerken van apparaat twin tags
- C2D methoden

## <a name="job-lifecycle"></a>De levenscyclus van het project

Taken worden gestart door de oplossing voor back-end en onderhouden door IoT Hub.  U kunt een taak door middel van een service gerichte URI starten (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) en de voortgang van een taak uitvoeren via een gerichte service-URI-query (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Als een taak wordt gestart, worden opvragen van taken ingeschakeld in de toepassing back-end voor het vernieuwen van de status van de uitvoering van taken.

> [AZURE.NOTE] Wanneer u een taak start, namen en waarden mogen alleen US-ASCII-afdrukbare alfanumerieke, met uitzondering van een in de volgende set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor verwijzing bieden meer informatie over het gebruik van taken.

## <a name="jobs-to-execute-direct-methods"></a>Taken direct methoden uit te voeren

Hieronder ziet u de details van de HTTP 1.1-aanvraag voor het uitvoeren van een [directe methode] [ lnk-dev-methods] van een aantal apparaten met behulp van een taak:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Taken bijwerken apparaateigenschappen twin

Dit is de HTTP 1.1-aanvraaggegevens twin-eigenschappen van een apparaat met behulp van een taak bijwerken:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Opvragen van de voortgang van taken

Het volgende is de HTTP 1.1 request details voor [query's voor taken][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
De continuationToken wordt geleverd door het antwoord.  

## <a name="jobs-properties"></a>Taken eigenschappen

Hier volgt een lijst met eigenschappen en de bijbehorende beschrijvingen, die kunnen worden gebruikt voor het uitvoeren van query's voor taken of de resultaten van de taak.

| Eigenschap | Beschrijving |
| -------------- | -----------------|
| **taak-id** | Toepassing opgegeven ID voor de taak. |
| **Starttijd** | Toepassing opgegeven starttijd (ISO 8601) voor de taak. |
| **Eindtijd** | IoT Hub vastgestelde datum (ISO 8601) voor wanneer de taak is voltooid. Geldig nadat de taak de status 'voltooid' wordt bereikt. | 
| **type** | Soorten taken: |
| | **scheduledUpdateTwin**: een taak die wordt gebruikt voor het bijwerken van een set eigenschappen de gewenste twin of tags. |
| | **scheduledDeviceMethod**: een taak die wordt gebruikt voor het aanroepen van een methode van het apparaat op een reeks twin. |
| **status** | Huidige status van de taak. Mogelijke waarden voor de status: |
| | **in behandeling** : gepland en die nog moet worden opgenomen door de taak service. |
| | **geplande** : gepland voor een tijd in de toekomst. |
| | **actief** : actieve taak. |
| | **geannuleerd** : taak is geannuleerd. |
| | **mislukt** : functie is mislukt. |
| | **voltooid** : taak is voltooid. |
| **deviceJobStatistics** | Statistische gegevens over de uitvoering van het project. |

Tijdens het voorbeeld van is het object deviceJobStatistics alleen beschikbaar nadat de taak is voltooid.

| Eigenschap | Beschrijving |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Het aantal apparaten in de taak. |
| **deviceJobStatistics.failedCount** | Aantal apparaten waarin de taak is mislukt. |
| **deviceJobStatistics.succeededCount** | Aantal apparaten waarin de taak is voltooid. |
| **deviceJobStatistics.runningCount** | Het aantal apparaten dat de taak worden uitgevoerd. |
| **deviceJobStatistics.pendingCount** | Aantal apparaten die in behandeling zijn de taak uit te voeren. |


### <a name="additional-reference-material"></a>Aanvullend referentiemateriaal

Er zijn andere zoeken naar onderwerpen in de handleiding voor ontwikkelaars:

- [Eindpunten IoT Hub] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen.
- [Procesbeperking en quota's] [ lnk-quotas] de quota die van toepassing zijn op de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u de service wordt beschreven.
- [SDK's IoT Hub apparaat- en] [ lnk-sdks] geeft een overzicht van de verschillende taal SDK's u een gebruiken, wanneer u zowel apparaat als service toepassingen ontwikkelt die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] de querytaal kunt u gegevens ophalen uit de IoT Hub over uw apparaat twins, methoden en taken beschreven.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over de IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Als u proberen sommige van de concepten die in dit artikel worden beschreven wilt, kunt u mogelijk geïnteresseerd in de volgende zelfstudie voor IoT Hub:

- [Taken plannen en broadcast][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
