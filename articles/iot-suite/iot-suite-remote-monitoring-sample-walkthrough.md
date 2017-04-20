<properties
 pageTitle="Externe controle vooraf geconfigureerde oplossing scenario | Microsoft Azure"
 description="Een beschrijving van de IoT Azure vooraf geconfigureerde oplossing voor externe controle en de architectuur."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Externe controle vooraf geconfigureerde oplossing scenario

## <a name="introduction"></a>Inleiding

De IoT Suite externe controle [vooraf geconfigureerde oplossing] [ lnk-preconfigured-solutions] is een implementatie van een end-to-end monitoring oplossing voor meerdere machines die worden uitgevoerd op externe locaties. De oplossing combineert belangrijke Azure services een algemene implementatie van het scenario en kunt u als uitgangspunt voor uw eigen toepassing. U kunt [aanpassen] [ lnk-customize] de oplossing voor uw eigen specifieke eisen voldoen.

Dit artikel begeleidt u bij enkele van de belangrijkste elementen van de externe monitoring oplossing om te begrijpen hoe het werkt. Deze kennis kunt u:

- Problemen in de oplossing.
- Plan het aanpassen van de oplossing voor uw eigen specifieke behoeften. 
- Ontwerp uw eigen oplossing IoT die Azure services gebruikt.

## <a name="logical-architecture"></a>Logische architectuur

In het volgende diagram bevat een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![Logische architectuur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Gesimuleerde apparaten

In de vooraf geconfigureerde oplossing geeft het gesimuleerde apparaat een koeling apparaat (bijvoorbeeld een gebouw airconditioner of faciliteit lucht verwerking unit). Als u de vooraf geconfigureerde oplossing implementeert, vier gesimuleerde apparaten die worden uitgevoerd in een [WebJob Azure]ook automatisch inrichten[lnk-webjobs]. De gesimuleerde apparaten kunnen gemakkelijk verkennen van het gedrag van de oplossing zonder te hoeven implementeren van de fysieke apparaten. Zie [verbinding maken met het apparaat naar de externe controle vooraf geconfigureerde oplossing] voor de implementatie van een reële fysieke apparaat[ lnk-connect-rm] zelfstudie.

Elke gesimuleerde apparaat kunt u de volgende berichttypen verzenden met IoT Hub:

| Bericht  | Beschrijving |
|----------|-------------|
| Opstarten  | Wanneer het apparaat wordt gestart, verzendt deze een **apparaat-info** weergegeven met informatie over zichzelf naar de back-end. Deze gegevens omvatten de apparaat-id, de Apparaatmetagegevens, een lijst van de opdrachten voor het apparaat ondersteunt en de huidige configuratie van het apparaat. |
| Aanwezigheid | Een apparaat stuurt regelmatig een bericht **aanwezigheid** te melden of het apparaat een voeler voor de aanwezigheid van een sensor. |
| Telemetrie | Een apparaat regelmatig een **telemetrie** bericht verzendt dat gesimuleerde waarden voor de temperatuur en vochtigheid verzameld van gesimuleerde sensoren van het apparaat gemeld. |


De gesimuleerde apparaten verzenden de volgende eigenschappen van een apparaat een **apparaat-info** :

| Eigenschap               |  Doel |
|------------------------|--------- |
| Apparaat-ID              | De id die wordt verstrekt of toegewezen wanneer een apparaat in de oplossing wordt gemaakt. |
| De fabrikant           | Fabrikant van het apparaat |
| Modelnummer           | Modelnummer van het apparaat |
| Serienummer          | Serienummer van het apparaat |
| Firmware               | Huidige versie van de firmware op het apparaat |
| Platform               | De platformarchitectuur van het van het apparaat |
| Processor              | -Processor met het apparaat |
| Geïnstalleerde RAM-geheugen          | Hoeveelheid RAM-geheugen op het apparaat |
| Hub staat ingeschakeld      | IoT Hub staat eigenschap van het apparaat |
| Aanmaaktijd           | Wanneer die het apparaat is gemaakt in de oplossing |
| Bijgewerkte tijd           | Laatste keer eigenschappen voor het apparaat zijn bijgewerkt |
| Latitude               | Latitude-locatie van het apparaat |
| Lengtegraad              | Locatie van de lengte van het apparaat |

De simulator van deze eigenschappen in een gesimuleerde apparaten met voorbeeldwaarden zaden.  Elke keer dat de simulator een gesimuleerde apparaat initialiseert boekt het apparaat de vooraf gedefinieerde metagegevens op IoT-Hub. Opmerking hoe dit overschrijft eventuele updates van metagegevens in de portal van het apparaat.


De gesimuleerde apparaten kunnen de volgende opdrachten uit het dashboard oplossing via de hub IoT verzonden worden verwerkt:

| Opdracht                | Beschrijving                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Verzendt een _ping_ naar het apparaat te controleren is alive   |
| StartTelemetry         | Start het apparaat verzenden telemetrie                 |
| StopTelemetry          | Hiermee stopt u het apparaat verzenden telemetrie             |
| ChangeSetPointTemp     | Hiermee wijzigt u de puntwaarde van de set waaromheen de willekeurige gegevens zijn gegenereerd |
| DiagnosticTelemetry    | Activeert de apparaatsimulator voor het verzenden van de waarde van een extra telemetrie (externalTemp) |
| ChangeDeviceState      | Hiermee wijzigt u een eigenschap van een uitgebreide status van het apparaat en het bericht apparaat info van het apparaat |

De bevestiging van de opdracht apparaat naar de oplossing voor back-end is beschikbaar via de hub IoT.

## <a name="iot-hub"></a>IoT Hub

De [IoT hub] [ lnk-iothub] ingests gegevens die worden verzonden vanaf de apparaten in de cloud en beschikbaar gemaakt voor de taken Azure Stream Analytics (ASA). IoT hub stuurt ook opdrachten naar uw apparaten voor de portal van het apparaat. Een afzonderlijke groep van IoT Hub consument elke taak stream ASA gebruikt voor het lezen van de stroom van berichten van uw apparaten.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

In de externe monitoring oplossing [Azure Stream Analytics] [ lnk-asa] (ASA) verzendt berichten ontvangen door de hub IoT en andere back-end componenten voor verwerking of opslag. Verschillende ASA taken uitvoeren bepaalde functies op basis van de inhoud van de berichten.

**Taak 1: apparaatgegevens** apparaat informatieberichten van de binnenkomende berichtenstroom filters en verzendt deze naar het eindpunt van een Hub gebeurtenis. Een apparaat stuurt informatieberichten bij het opstarten en in reactie op een **SendDeviceInfo** -opdracht. Deze taak wordt de volgende querydefinitie **apparaat info** berichten identificeren:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Deze taak stuurt de uitvoer naar een gebeurtenis Hub voor verdere verwerking.

**Taak 2: regels** binnenkomende temperatuur en vochtigheid telemetrie waarden tegen drempels per apparaat wordt geëvalueerd. Drempelwaarden worden ingesteld in de regeleditor die in het dashboard oplossing. Elk apparaat/waarde-paar wordt opgeslagen door de tijdstempel in een blob die stroom Analytics als **Verwijzingsgegevens leest**. Het project vergelijkt elke niet-lege waarde ten opzichte van de ingestelde drempelwaarde voor het apparaat. Als deze groter is dan de ' >' voorwaarde, wordt de taak de uitvoer een **alarm** -gebeurtenis die aangeeft dat de drempel wordt overschreden en het apparaat, de waarde en de waarden in timestamp biedt. Deze taak wordt de volgende querydefinitie identificeren telemetrie berichten waarmee een geluidssignaal moeten worden geactiveerd:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

De taak stuurt de uitvoer naar een gebeurtenis Hub voor verdere verwerking en details van elke waarschuwing opslaat naar een blobopslag van waar het dashboard oplossing de informatie over waarschuwingen kunt lezen.

**Taak 3: telemetrie** werkt op de binnenkomende apparaat telemetrie stream op twee manieren. De eerste stuurt alle berichten van de telemetrie van de apparaten naar permanente blob-opslag voor langere termijn opgeslagen. De tweede vochtigheid gemiddelde, minimum en maximum waarden worden berekend worden via een schuifvenster vijf minuten en stuurt deze gegevens naar een blob-opslag. Het dashboard oplossing leest de telemetriegegevens van blob-opslag voor het vullen van de grafieken. Deze taak wordt gebruikt voor de definitie van de volgende query:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Gebeurtenis Hubs

De taken van de ASA **apparaatgegevens** en **regels** hun gegevens uitvoeren naar gebeurtenis Hubs betrouwbaar doorsturen naar de **Gebeurtenis Processor** in de WebJob.

## <a name="azure-storage"></a>Azure opslag

De oplossing maakt gebruik van Azure blob-opslag om de van ruwe en samengevatte telemetriegegevens van de apparaten in de oplossing. Het dashboard leest de telemetriegegevens van blob-opslag voor het vullen van de grafieken. Om het weergeven van waarschuwingen, leest het dashboard de gegevens van blob-opslag die records wanneer telemetrie waarden de geconfigureerde drempelwaarden overschreden. Blob-opslag de oplossing ook gebruikt voor het vastleggen van de drempelwaarden die u in het dashboard instelt.

## <a name="webjobs"></a>WebJobs

Naast het apparaat simulatoren die als host fungeert, onderbrengen de WebJobs in de oplossing ook de **Gebeurtenis Processor** in een WebJob met Azure die gegevens verwerkt berichten en antwoorden van opdracht. Deze worden gebruikt:

- Apparaat het apparaat register (opgeslagen in de database DocumentDB) bijwerken met de gegevens van de huidige berichten.
- Antwoordberichten opdracht voor het bijwerken van het apparaat de opdracht Historie (opgeslagen in de database DocumentDB).

## <a name="documentdb"></a>DocumentDB

De oplossing maakt gebruik van een DocumentDB database voor het opslaan van informatie over de apparaten die zijn verbonden met de oplossing. Deze informatie omvat de Apparaatmetagegevens van het en de geschiedenis van de opdrachten van het dashboard naar apparaten verzonden.

## <a name="web-apps"></a>Web apps

### <a name="remote-monitoring-dashboard"></a>Externe bewaking dashboard
Deze pagina in de webtoepassing gebruikt PowerBI javascript-besturingselementen (Zie [PowerBI beelden repo](https://www.github.com/Microsoft/PowerBI-visuals)) de telemetriegegevens visualiseren van de apparaten. De oplossing maakt gebruik van de ASA telemetrie taak de blob-opslag telemetriegegevens schrijven.


### <a name="device-administration-portal"></a>Apparaat beheer portal

Dit web app kunt u:

- Inrichten van een nieuw apparaat. Deze actie stelt de unieke apparaat-id en de sleutel voor verificatie. Informatie over het apparaat geschreven naar het register van de identiteit IoT Hub en de DocumentDB oplossing specifieke database.
- Eigenschappen van apparaten beheren. Deze actie omvat bestaande eigenschappen weergeven en bijwerken met nieuwe eigenschappen.
- Opdrachten naar een apparaat sturen.
- De opdrachtgeschiedenis voor een apparaat weergeven.
- In- en uitschakelen van apparaten.

## <a name="next-steps"></a>Volgende stappen

De volgende TechNet-blog-updates vindt u meer informatie over het remote monitoring vooraf geconfigureerde oplossing:

- [IoT Suite - achter de schermen - bewaking op afstand](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Suite - Remote Monitoring - Live en gesimuleerde apparaten toe te voegen](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

U kunt verder aan de slag met IoT Suite door te lezen de volgende artikelen:

- [Verbind het apparaat met de externe controle vooraf geconfigureerde oplossing][lnk-connect-rm]
- [Machtigingen voor de site azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md