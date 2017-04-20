<properties
 pageTitle="Handleiding voor ontwikkelaars - eindpunten IoT Hub | Microsoft Azure"
 description="Azure IoT Hub developer guide - naslaginformatie over IoT Hub eindpunten"
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

# <a name="reference---iot-hub-endpoints"></a>Referentie - IoT Hub eindpunten

## <a name="list-of-iot-hub-endpoints"></a>Lijst van eindpunten IoT Hub

Azure IoT Hub is een service met meerdere huurder de functionaliteit voor de verschillende actoren beschrijft. Het volgende diagram toont de verschillende eindpunten dat IoT Hub beschrijft.

![IoT Hub eindpunten][img-endpoints]

Hieronder volgt een beschrijving van de eindpunten:

* **Resource provider**. De provider IoT Hub bron beschrijft een [Azure Resource Manager] [ lnk-arm] interface waarmee eigenaren van Azure abonnement maken en verwijderen van IoT hubs en IoT hub eigenschappen bijwerken. IoT Hub eigenschappen [beveiliging op gebruikersniveau hub beleid]te sturen[lnk-accesscontrol], in plaats van toegangsbeheer op gebruikersniveau apparaat en functionele opties voor wolk naar apparaat en apparaat voor cloud messaging. De IoT Hub resource provider kunt u ook [identiteiten apparaat]exporteren[lnk-importexport].
* **Identiteitsbeheer apparaat**. Elke hub IoT geeft een set HTTP-REST eindpunten apparaat identiteiten beheren (maken, ophalen, bijwerken en verwijderen). [Apparaat-id's] [ lnk-device-identities] worden gebruikt voor verificatie en toegangsbeheer van apparaat.
* **Twin Apparaatbeheer**. Elke hub IoT beschrijft een aantal gerichte service HTTP REST eindpunt kunnen opvragen en bijwerken van [apparaat twins] [ lnk-twins] (update voor labels en eigenschappen).
* **Beheer taken**. Elke hub IoT geeft een set HTTP-REST-eindpunt gerichte service opvragen en beheren van [taken][lnk-jobs].
* **Apparaat eindpunten**. Voor elk apparaat in het register van de identiteit apparaat ingericht, beschikt IoT Hub over een set van eindpunten die een apparaat gebruiken kan voor het verzenden en ontvangen van berichten:
    - *Apparaat-wolk - berichten verzenden*. Gebruik dit eindpunt apparaat-wolk - berichten te [verzenden][lnk-d2c].
    - *Berichten van de wolk naar apparaat ontvangen*. Een apparaat gebruikt dit eindpunt gerichte [berichten wolk naar apparaat]ontvangen[lnk-c2d].
    - *Starten van de geüploade bestanden*. Een apparaat dit eindpunt gebruikt voor het ontvangen van een URI Azure opslag SAS van IoT Hub voor het [uploaden van een bestand][lnk-upload].
    - *Ophalen en bijwerken twin van eigenschappen*. Een apparaat gebruikt deze eindpunten voor toegang tot het [apparaat twin][lnk-twins]van eigenschappen.
    - *Ontvangen rechtstreekse methoden aanvragen*. Een apparaat deze eindpunten gebruikt om te luisteren naar de [rechtstreekse methoden][lnk-methods]van aanvragen.

    Deze eindpunten worden blootgesteld met behulp van [v3.1.1 MQTT][lnk-mqtt], HTTP 1.1 en [AMQP 1.0] [ lnk-amqp] protocollen. AMQP is ook beschikbaar via [WebSockets] [ lnk-websockets] op poort 443.
    
    Endpoins de twins en methoden zijn beschikbaar voor alleen [v3.1.1 MQTT][lnk-mqtt].

* **Service-endpoints**. Elke hub IoT beschrijft een aantal eindpunten die back-end van uw toepassing gebruiken kunt om te communiceren met uw apparaten. Deze eindpunten zijn momenteel alleen belicht met behulp van de [AMQP] [ lnk-amqp] , met uitzondering van het eindpunt van methode aanroepen die beschikbaar zijn via de HTTP 1.1-protocol.
    - *Apparaat naar cloud-berichten ontvangen*. Dit eindpunt is compatibel met [Azure gebeurtenis Hubs][lnk-event-hubs]. Een back-end-service kunt alle [apparaat-wolk - berichten] [ lnk-d2c] die door de apparaten worden verzonden.
    - *Cloud-apparaat - berichten verzenden en ontvangen van levering bevestigingen*. Deze eindpunten inschakelen in uw toepassing back-end voor het verzenden van betrouwbare [berichten wolk naar apparaat][lnk-c2d], en de bijbehorende levering of vervaldatum bevestigingen ontvangen.
    - *Bestand-meldingen ontvangen*. Dit eindpunt messaging kunt u meldingen ontvangen wanneer uw apparaten met succes een bestand uploaden. 
    - *Directe methode aanroepen*. Dit eindpunt kan een back-end-service een [directe methode] aan te roepen[ lnk-methods] op een apparaat.

De [IoT Hub-API's en SDK's] [ lnk-sdks] artikel beschrijft de verschillende manieren toegang tot deze eindpunten.

Ten slotte is het belangrijk te weten dat alle IoT Hub eindpunten gebruikt de [TLS] -[ lnk-tls] -protocol en geen eindpunt ooit ongecodeerd verzonden/niet-beveiligde kanalen wordt weergegeven.

## <a name="field-gateways"></a>Veld gateways

In een oplossing IoT is een *veld gateway* bevindt zich tussen uw apparaten en de eindpunten IoT Hub. Deze bevindt zich doorgaans dicht bij uw apparaten. De apparaten communiceren rechtstreeks met het veld gateway met behulp van een protocol dat wordt ondersteund door de apparaten. Het veld gateway verbinding met een IoT Hub eindpunt met behulp van een protocol dat wordt ondersteund door de IoT Hub. Een veld gateway zijn uiterst gespecialiseerde hardware of laag vermogen computers software die doet het end-to-end scenario waarvoor de gateway is bestemd.

U kunt de [SDK van Azure IoT Gateway] [ lnk-gateway-sdk] voor het implementeren van een veld gateway. Deze SDK biedt specifieke functionaliteit zoals de mogelijkheid om de communicatie van meerdere apparaten op dezelfde verbinding met IoT Hub multiplexen.

## <a name="next-steps"></a>Volgende stappen

Er zijn andere zoeken naar onderwerpen in deze handleiding voor ontwikkelaars van IoT Hub:

- [Querytaal voor twins, methoden en taken][lnk-devguide-query]
- [Quota's en bandbreedteregeling][lnk-devguide-quotas]
- [IoT Hub MQTT ondersteuning][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md