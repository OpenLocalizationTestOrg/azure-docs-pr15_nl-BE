<properties
 pageTitle="Azure IoT Hub schalen | Microsoft Azure"
 description="Beschrijving van het schalen van Azure IoT Hub."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>IoT Hub schalen

Azure IoT Hub kan maximaal een miljoen gelijktijdig aangesloten apparaten ondersteunen. Zie voor meer informatie, [prijzen IoT Hub][lnk-pricing]. Elke eenheid IoT Hub kunt een bepaald aantal berichten per dag.

Om uw oplossing correct schalen, rekening houden met uw gebruik van IoT Hub. In het bijzonder rekening houden met de vereiste piek doorvoer voor de volgende categorieën van activiteiten:

* Apparaat-wolk-berichten
* Cloud-to-device-berichten
* Identiteit registerbewerkingen

Naast deze informatie doorvoer Zie [IoT Hub quota's en throttles][] en ontwerp uw oplossing dienovereenkomstig.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Doorvoer van apparaat-cloud- en cloud-to-device-bericht

De beste manier om het formaat van een IoT Hub oplossing is voor de evaluatie van het verkeer op basis van per eenheid.

Apparaat-wolk-berichten als volgt aanhoudende doorvoer.

| Laag | Aanhoudende doorvoer | Aanhoudende verzendsnelheid |
| ---- | -------------------- | ------------------- |
| S1 | Tot en met 1111 KB per minuut per eenheid<br/>(1,5 GB/dag/eenheid) | Gemiddelde van 278 berichten per minuut per eenheid<br/>(400.000 berichten per dag per eenheid) |
| S2 | Tot en met 16 MB per minuut per eenheid<br/>(22.8 GB/dag/eenheid) | Gemiddelde van 4167 berichten per minuut per eenheid<br/>(6 miljoen berichten per dag per eenheid) |
| S3 | Tot 814 MB per minuut per eenheid<br/>1144.4 GB/dag/unit) | Gemiddelde van 208,333 berichten per minuut per eenheid<br/>(300 miljoen berichten per dag per eenheid) |

## <a name="identity-registry-operation-throughput"></a>Identiteit register bewerking doorvoer

IoT Hub identiteit registerbewerkingen zijn niet moet runtime-bewerkingen, zoals ze voornamelijk betrekking op het apparaat wordt ingericht hebben.

Zie voor specifieke burst prestaties cijfers, [IoT Hub quota's en throttles][].

## <a name="sharding"></a>Sharding

Terwijl een hub met één IoT miljoenen apparaten schalen kunt, vereist soms uw oplossing specifieke prestatie-eigenschappen die een enkele IoT hub kan niet garanderen. In dat geval is het raadzaam dat u uw apparaten in meerdere IoT hubs verdelen. Meerdere IoT hubs verkeer bursts vloeiend en de vereiste doorvoer of bewerking tarieven die nodig zijn.

## <a name="next-steps"></a>Volgende stappen

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quota's en throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
