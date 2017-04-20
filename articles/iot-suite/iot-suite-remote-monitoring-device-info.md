<properties
 pageTitle="Metagegevens voor apparaten informatie in de externe monitoring oplossing | Microsoft Azure"
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
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Metagegevens voor apparaten informatie in de externe controle vooraf geconfigureerde oplossing

De externe controle vooraf geconfigureerde oplossing Azure IoT Suite wordt een benadering voor het beheer van de Apparaatmetagegevens van het. Dit artikel bevat een overzicht van de aanpak van die deze oplossing nodig is om te begrijpen:

- De Apparaatmetagegevens van welk de oplossing worden opgeslagen.
- De oplossing beheert hoe de Apparaatmetagegevens.

## <a name="context"></a>Context

De externe controle vooraf geconfigureerde oplossing maakt gebruik van [Azure IoT Hub] [ lnk-iot-hub] waarmee uw apparaten om gegevens te verzenden naar de cloud. IoT Hub bevat een [apparaat identiteit register] [ lnk-identity-registry] toegang regelen tot IoT Hub. Het register IoT Hub apparaat identiteit staat los van de externe monitoring oplossing specifieke *apparaat register* dat apparaat informatie metagegevens worden opgeslagen. De remote monitoring oplossing maakt gebruik van een [DocumentDB] [ lnk-docdb] database voor de uitvoering van het register apparaat voor het opslaan van metagegevens voor apparaten informatie. De [Architectuur die Microsoft Azure IoT verwijzing] [ lnk-ref-arch] beschrijving van de functie van het register van het apparaat in een typische IoT-oplossing.

> [AZURE.NOTE] De externe controle vooraf geconfigureerde oplossing, blijft het register van de identiteit apparaat synchroon met het register van het apparaat. Beide dezelfde apparaat-id gebruikt als unieke identificatie voor elk apparaat dat is aangesloten op de hub IoT.

De [IoT Hub device management voorvertoning] [ lnk-dm-preview] functies toevoegt aan IoT-Hub die vergelijkbaar zijn met het apparaat information management-functies die in dit artikel wordt beschreven. Op dit moment gebruikt de remote monitoring oplossing alleen algemeen beschikbaar (GA) functies in IoT Hub.

## <a name="device-information-metadata"></a>Metagegevens voor apparaten informatie

Een apparaat informatie metagegevens JSON-document opgeslagen in de database apparaat register DocumentDB heeft de volgende structuur:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: het apparaat zelf schrijft deze eigenschappen en het apparaat is de autoriteit voor deze gegevens. Voorbeeld van de andere eigenschappen bevatten fabrikant, modelnummer en serienummer. 
- **DeviceID**: de unieke apparaat-id. Deze waarde is hetzelfde in het register van de IoT Hub apparaat identiteit.
- **HubEnabledState**: de status van het apparaat in de IoT Hub. Deze waarde is ingesteld op **null** totdat het apparaat voor het eerst verbinding maakt. In het portaal oplossing een **null** -waarde weergegeven als het apparaat wordt "geregistreerd maar niet aanwezig."
- **CreatedTime**: het tijdstip waarop het apparaat is gemaakt.
- **DeviceState**: de status van het apparaat gemeld.
- **UpdatedTime**: de tijd die het apparaat via de portal van de oplossing voor het laatst werd bijgewerkt.
- **SystemProperties**: de oplossing portal schrijft de Systeemeigenschappen en het apparaat heeft geen informatie over deze eigenschappen. Een eigenschap van een voorbeeld systeem is de **ICCID** als de oplossing wordt geautoriseerd met en verbonden met een SIM-kaart compatibele apparaten beheren-service.
- **Opdrachten**: een lijst van de opdrachten voor het apparaat wordt ondersteund. Het apparaat geeft deze informatie aan de oplossing.
- **CommandHistory**: een lijst van de opdrachten die zijn verzonden door de externe monitoring oplossing naar het apparaat en de status van deze opdrachten.
- **IsSimulatedDevice**: een markering waarmee een apparaat als een gesimuleerde apparaat.
- **id**: de unieke id van de DocumentDB voor dit apparaat document.

> [AZURE.NOTE] Gegevens van een apparaat kunt ook metagegevens beschrijven de telemetrie die het apparaat met IoT Hub verzendt. De remote monitoring oplossing gebruikt deze metagegevens telemetrie aanpassen hoe [dynamische telemetrie]wordt weergegeven in het dashboard[lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Levenscyclus

Wanneer u een apparaat in de portal solution maakt, de oplossing een post gemaakt in het register van het apparaat zoals eerder weergegeven. Veel van de informatie is in eerste instantie uit stubbed en de **HubEnabledState** is ingesteld op **null**. De oplossing maakt op dit moment ook een vermelding voor het apparaat in het register apparaat identiteit, waardoor de toetsen die het apparaat gebruikt om te verifiëren met IoT Hub worden gegenereerd.

Wanneer een apparaat eerst verbinding maakt met de oplossing, stuurt het een apparaat weergegeven. Dit bericht apparaat informatie omvat eigenschappen zoals de fabrikant van het apparaat, het modelnummer en het serienummer van een apparaat. Een informatiebericht apparaat bevat ook een lijst met opdrachten die worden ondersteund door het apparaat met inbegrip van informatie over de opdrachtparameters. Wanneer de oplossing in dit bericht ontvangt, worden de metagegevens apparaat informatie in het register apparaat bijgewerkt.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Weergeven en bewerken van gegevens van een apparaat in de portal oplossing

De lijst met apparaten in de portal oplossing de volgende eigenschappen van een apparaat weergeven als kolommen: **Status**, **DeviceId**, **fabrikant**, **Modelnummer**, **serienummer**, **Firmware**, **Platform**, **Processor**en **RAM -geheugen geïnstalleerd**. De locatie in de kaart Bing station de apparaateigenschappen **breedtegraad** en **lengtegraad** op het dashboard. 

![Lijst met apparaten][img-device-list]

Als u op **bewerken** in het deelvenster **Details van apparaat** in de portal oplossing, kunt u alle eigenschappen te bewerken. Deze eigenschappen bewerken van de record voor het apparaat in de DocumentDB database bijgewerkt. Als een apparaat een bericht van bijgewerkte apparaatstuurprogramma info verzendt, overschrijft deze de wijzigingen in het portaal oplossing. U kunt de **DeviceId**, **hostnaam**, **HubEnabledState**, **CreatedTime**, **DeviceState**en **UpdatedTime** -eigenschappen in het portaal oplossing niet bewerken omdat alleen het apparaat bevoegd is over deze eigenschappen.

![Apparaat bewerken][img-device-edit]

De oplossing-portal kunt u een apparaat verwijderen uit uw oplossing. Wanneer u een apparaat verwijdert, verwijdert u de metagegevens voor apparaten informatie uit het register oplossing apparaat de oplossing, en verwijdert u de apparaatvermelding in het register van de IoT Hub apparaat identiteit. Voordat u een apparaat te verwijderen, moet u deze uitschakelen.

![Apparaat verwijderen][img-device-remove]

## <a name="device-information-message-processing"></a>Apparaat informatieverwerking

Informatie die door een apparaat verzonden berichten zijn berichten telemetrie. Apparaat informatieberichten bevatten de eigenschappen van een apparaat, een apparaat kan reageren op opdrachten en de opdrachtgeschiedenis van elke. IoT Hub zelf heeft geen kennis van de metagegevens in een informatiebericht apparaat en het bericht op dezelfde manier alle apparaat-wolk-berichten verwerkt verwerkt. In de externe monitoring oplossing een [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) taak leest de berichten van IoT Hub. De stream **DeviceInfo** analytics taak filters voor berichten met **"ObjectType": "DeviceInfo"** en stuurt deze door naar de **EventProcessorHost** host-exemplaar dat wordt uitgevoerd in een web-project. Logica in het **EventProcessorHost** -exemplaar gebruikt de apparaat-id te zoeken van de record DocumentDB voor het specifieke apparaat en de record bij te werken. De record apparaat register bevat nu de apparaateigenschappen, opdrachten en opdrachten.

> [AZURE.NOTE] Een informatiebericht apparaat is een standaard apparaat voor cloud-bericht. De oplossing wordt onderscheid gemaakt tussen apparaat informatieberichten en telemetrie berichten met behulp van ASA's.

## <a name="example-device-information-records"></a>Voorbeeld apparaat gegevensrecords

De externe controle vooraf geconfigureerde oplossing gebruikt twee typen apparaat gegevensrecords: records voor de gesimuleerde apparaten met de oplossing en de records voor de aangepaste apparaten als u verbinding met de oplossing maakt wordt geïmplementeerd.

### <a name="simulated-device"></a>Gesimuleerde apparaat

In het volgende voorbeeld ziet u de gegevensrecord JSON-apparaat voor een gesimuleerde apparaat. Deze record heeft een waarde ingesteld voor **UpdatedTime**, geeft dat het apparaat een **DeviceInfo** bericht is verzonden met IoT Hub. De record bevat enkele algemene eigenschappen van apparaten, definieert de zes opdrachten de gesimuleerde apparaten ondersteunen en de vlag **IsSimulatedDevice** is ingesteld op **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Aangepast apparaat

In het volgende voorbeeld toont de JSON-apparaat informatie record voor een aangepast apparaat en is de vlag **IsSimulatedDevice** is ingesteld op **0**. U ziet dat deze aangepaste twee opdrachten ondersteunt en de portal oplossing heeft een **SetTemperature** opdracht naar het apparaat verzonden.

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Hier volgt het bericht JSON **DeviceInfo** het apparaat voor het bijwerken van de metagegevens voor apparaten informatie verzonden:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Nu u klaar bent met het leren hoe u de vooraf geconfigureerde oplossingen kunt aanpassen, vindt u enkele van de andere functies en mogelijkheden van de IoT Suite vooraf geconfigureerde oplossingen:

- [Preventief onderhoud vooraf geconfigureerde oplossing, overzicht][lnk-predictive-overview]
- [Veelgestelde vragen voor IoT Suite][lnk-faq]
- [Beveiliging van de grond omhoog IoT][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
