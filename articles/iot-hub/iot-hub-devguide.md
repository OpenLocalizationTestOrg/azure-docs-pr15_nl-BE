<properties
 pageTitle="Developer guide onderwerpen voor IoT Hub | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars met IoT Hub eindpunten, beveiliging, apparaat-id-register, Apparaatbeheer en messaging"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT Hub-handleiding voor ontwikkelaars

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige bi-directionele communicatie tussen miljoenen IoT-apparaten inschakelen en weer een toepassing te beëindigen.

Azure IoT Hub beschikt u over:

* Beveiligde communicatie met behulp van de beveiligingsreferenties per apparaat en toegangsbeheer.
* Betrouwbare apparaat-cloud- en cloud-to-device hyperscale '-berichten.
* Eenvoudig apparaat verbinding met apparaatbibliotheken voor de meest populaire talen en platformen.

Deze IoT Hub-handleiding voor ontwikkelaars bevat de volgende artikelen:

- [Berichten verzenden en ontvangen met IoT Hub] [ devguide-messaging] beschrijving van de messaging functies (apparaat-cloud- en cloud-to-device) IoT Hub beschrijft. Het artikel bevat tevens informatie over onderwerpen zoals indelingen, en de ondersteunde communicatieprotocollen en de poortnummers die ze gebruiken.
- [Uploaden van bestanden vanaf een apparaat] [ devguide-upload] wordt beschreven hoe u bestanden van een apparaat kunt uploaden. Het artikel bevat tevens informatie over onderwerpen als de meldingen die het proces uplaod kunt verzenden.
- [Apparaat identiteiten beheren in IoT Hub] [ devguide-identities] wordt beschreven wat informatie van elke hub IoT apparaat identiteit register worden opgeslagen en hoe u openen en wijzigen.
- [Toegang tot de IoT Hub] [ devguide-security] beschrijft het beveiligingsmodel dat wordt gebruikt voor toegang tot de IoT Hub functionaliteit voor beide apparaten en onderdelen van de wolk. Dit artikel bevat informatie over het gebruik van tokens en x.509-certificaten en de details van de machtigingen die u kunt verlenen.
- [Twins apparaat gebruiken voor het synchroniseren van staat en configuraties] [ devguide-device-twins] beschrijving van het *apparaat twin* concept en de functionaliteit zoals het synchroniseren van een apparaat met de twin worden getoond. Dit artikel bevat informatie over de gegevens die zijn opgeslagen in een twin apparaat.
- [Een directe methode aanroept op een apparaat] [ devguide-directmethods] beschrijving van de levenscyclus van een directe methode informatie over hoe u methoden op een apparaat in uw back-end-app en de directe methode op uw apparaat.
- [Plannen van taken op verschillende apparaten] [ devguide-jobs] wordt beschreven hoe u taken op verschillende apparaten kunt plannen. Het artikel wordt beschreven hoe de taken die taken uitvoeren als een directe methode, een devcie met een devcie twin bijwerken uitvoeren. Ook wordt beschreven hoe de status van een taak opvragen.
- [Referentie - eindpunten IoT Hub] [ devguide-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen. Het artikel wordt tevens beschreven hoe u kunt een veld gateway waarmee sommige apparaten verbinding maken met uw IoT Hub eindpunten.
- [Referentie - querytaal voor twins, methoden en taken] [ devguide-query] die querytaal waarmee gegevens worden opgehaald uit de hub over uw apparaat twins en taken worden beschreven.
- [Referentie - quota en de beperking van] [ devguide-quotas] geeft een overzicht van de quota die zijn ingesteld in de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u een quotum overschrijdt.
- [Referentie - apparaten en Services SDK's] [ devguide-sdks] de SDK's kunt u lijsten ontwikkelen zowel apparaten en services die met de hub IoT samenwerken. Het artikel bevat koppelingen naar de on line documentatie van de API.
- [Referentie - ondersteuning voor IoT Hub MQTT] [ devguide-mqtt] bevat gedetailleerde informatie over hoe IoT Hub het protocol MQTT ondersteunt. Het artikel wordt de ondersteuning voor het protocol MQTT ingebouwd in de SDK's beschreven en vindt u informatie over het gebruik van het protocol MQTT rechtstreeks.
- [Verklarende woordenlijst] [ devguide-glossary] een lijst met algemene IoT Hub-termen.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

