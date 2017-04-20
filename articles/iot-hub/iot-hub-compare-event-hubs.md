<properties
 pageTitle="Azure IoT Hub met Azure gebeurtenis Hubs vergelijken | Microsoft Azure"
 description="Een vergelijking van de functionele verschillen en use-cases markeren Azure IoT Hub en Azure gebeurtenis Hubs."
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
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergelijking van Azure IoT Hub en Hubs Azure, gebeurtenis

Een van de belangrijkste use-cases voor IoT Hub is het verzamelen van telemetrie van apparaten. IoT Hub wordt daarom vaak vergeleken met [Azure gebeurtenis Hubs][]. Net als IoT-Hub is Hubs gebeurtenis gebeurtenis verwerkt waarmee de gebeurtenis en telemetrie ingress naar de cloud op grote schaal, met lage latentie en hoge betrouwbaarheid.

De services zijn echter veel verschillen, die worden beschreven in de volgende tabel.

| Gebied | IoT Hub | Gebeurtenis Hubs |
| ---- | ------- | ---------- |
| Communicatiepatronen | Kan apparaat-cloud- en cloud-to-device messaging. | U kunt alleen gebeurtenis ingress (meestal beschouwd als apparaat-wolk-scenario's). |
| Apparaat protocolondersteuning | MQTT, AMQP, AMQP ondersteunt via WebSockets en HTTP. Bovendien IoT Hub werkt met de [IoT Azure protocol gateway][lnk-azure-protocol-gateway], een aanpasbare protocol gateway-implementatie voor de ondersteuning van aangepaste protocollen. | AMQP, AMQP via WebSockets en HTTP ondersteunt. |
| Beveiliging | Per apparaat identiteits- en toegangsbeheer herroepbare biedt. Zie de [sectie beveiliging van de IoT Hub developer guide]. | Gebeurtenis Hubs gehele [gedeelde-beleid]biedt[Event Hubs - security], met beperkte intrekking ondersteuning via het [beleid van de uitgever van][Event Hubs publisher policies]. IoT oplossingen zijn vaak vereist voor het implementeren van een aangepaste oplossing voor de ondersteuning van referenties per apparaat en anti-spoofing maatregelen. |
| Activiteiten controleren | IoT oplossingen om u te abonneren op een groot aantal apparaat identity management en connectiviteit gebeurtenissen als afzonderlijk apparaat verificatiefouten, bandbreedtebeperking en onjuiste indeling uitzonderingen kunnen. Deze gebeurtenissen kunnen u snel herkennen van verbindingsproblemen op het apparaatniveau van individuele. | Alleen de statistische parameters beschrijft. |
| Schaal | Ondersteuning miljoenen van gelijktijdig aangesloten apparaten wordt geoptimaliseerd. | Een meer beperkt aantal gelijktijdige verbindingen--maximaal 5.000 AMQP verbindingen, aan de hand van [quota Azure Service Bus][]ondersteunt. Aan de andere kant kunt Hubs gebeurtenis u de partitie voor elk verzonden bericht. |
| SDK's apparaat | [SDK's apparaat] biedt[ Azure IoT Hub SDKs] voor een grote verscheidenheid aan platforms en talen. | Wordt ondersteund in .NET en C. Ook biedt AMQP en HTTP-interfaces verzenden. |
| Bestand uploaden | IoT oplossingen voor het uploaden van bestanden van apparaten naar de cloud kan. Bevat een bestand melding eindpunt voor de Werkstroomintegratie en een operationele monitoring categorie voor ondersteuning voor foutopsporing. | Patroon voor een claim wordt handmatig bestanden aanvragen van apparaten en apparaten voorzien van een sleutel opslag voor de transactie. |

Kortom, als de enige use-case ingress apparaat naar cloud telemetrie, biedt IoT Hub een service die speciaal is ontworpen voor IoT apparaat verbinding. Blijft de toegevoegde waarden voor deze scenario's met IoT-specifieke functies uitbreiden. Gebeurtenis Hubs is ontworpen voor de gebeurtenis ingress op grote schaal, zowel in de context van inter datacenter en intra-datacenter-scenario's.

Het is niet ongebruikelijk zowel IoT Hub als gebeurtenis Hubs in dezelfde oplossing--IoT Hub het apparaat naar cloud communicatie verwerkt waarbij gebeurtenis Hubs ingress later stadium gebeurtenis afhandelt in real-time verwerking motoren te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het plannen van uw implementatie IoT Hub, [schaal, HA en DR][lnk-scaling].

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

[Azure gebeurtenis Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[De Beveiligingssectie van de IoT Hub-handleiding voor ontwikkelaars]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure Service Bus quota]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
