<properties
 pageTitle="Handleiding voor ontwikkelaars - quota's en bandbreedteregeling | Microsoft Azure"
 description="Azure IoT Hub developer guide - beschrijving van de quota voor IoT Hub en bandbreedteregeling normaal"
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

# <a name="reference---quotas-and-throttling"></a>Referentie - quota's en bandbreedteregeling

## <a name="quotas-and-throttling"></a>Quota's en bandbreedteregeling

Elk abonnement Azure kan maximaal 10 IoT hubs hebben.

Elke hub IoT is ingericht met een bepaald aantal eenheden in een specifieke SKU (Zie voor meer informatie, [Prijzen van Azure IoT Hub][lnk-pricing]). De SKU's en het aantal eenheden bepalen het maximale dagelijkse quotum van berichten die u kunt verzenden.

De SKU bepaalt ook de bandbreedteregeling limieten IoT Hub wordt afgedwongen voor alle bewerkingen.

## <a name="operation-throttles"></a>Bewerking throttles

Throttles bewerking zijn snelheid beperkingen die worden toegepast in de minuut bereiken en zijn bedoeld om misbruik te voorkomen. IoT-Hub probeert te voorkomen dat er fouten zo veel mogelijk geretourneerd, maar het begint uitzonderingen retourneren als de gasklep al lang wordt geschonden.

Het volgende is de lijst van throttles afgedwongen. Waarden verwijzen naar een afzonderlijke hub.

| Gashendel | Waarde per hub |
| -------- | ------------- |
| Identiteit registerbewerkingen (maken, ophalen, lijst, bijwerken en verwijderen) | 5000/min/eenheid (S3) <br/> 100/min/eenheid (S1 en S2). |
| Apparaatverbindingen | sec-6000/eenheid (voor S3), 120/seconde/eenheid (S2), 12/seconde/eenheid (S1). <br/>Minimaal 100 per seconde. <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24/sec, maar u hebt ten minste 100 per seconde voor uw units. Met negen eenheden S1, hebt u 108 per seconde (9\*12) voor de units. |
| Hiermee verzendt u apparaat naar cloud | sec-6000/eenheid (voor S3), 120/seconde/eenheid (S2), 12/seconde/eenheid (S1). <br/>Minimaal 100 per seconde. <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24/sec, maar u hebt ten minste 100 per seconde voor uw units. Met negen eenheden S1, hebt u 108 per seconde (9\*12) voor de units. |
| Wolk naar apparaat verzenden | min-5000/eenheid (voor S3), 100/min/eenheid (S1 en S2). |
| Wolk naar apparaat ontvangt | min-50000/eenheid (voor S3), 1000/min/eenheid (S1 en S2). |
| Uploaden van bestanden | 5000 bestand uploaden min-meldingen/eenheid (voor S3), 100 bestand uploaden meldingen/min/eenheid (voor S1 en S2). <br/> 10000 SAS-URI's kunnen zijn van een account Azure opslag in één keer.<br/> 10 SAS URI's / apparaat kan worden uitgevoerd in één keer. | 

Het is belangrijk om aan te geven dat de *apparaatverbindingen* gashendel regelt de snelheid waarmee nieuwe verbindingen voor apparaten met een hub IoT en niet het maximum aantal gelijktijdig aangesloten apparaten kunnen worden vastgesteld. De vertraging is afhankelijk van het aantal eenheden die zijn ingericht voor de hub.

Als u een eenheid van S1 koopt, krijgt u bijvoorbeeld een vertraging van 100 verbindingen per seconde. Dit betekent dat 100.000 apparaten de verbinding wordt het duurt ten minste 1000 seconden (ongeveer 16 minuten). U kunt echter zoveel gelijktijdig aangesloten apparaten, zoals u geregistreerd in het register van de identiteit apparaat apparaten hebt hebben.

Voor een diepgaande bespreking van IoT Hub de blog boeken [IoT Hub beperken en u]procesbeperking gedrag, Zie[lnk-throttle-blog].

>[AZURE.NOTE] Op een bepaald moment kan quota of gashendel limieten verhogen door het aantal ingerichte eenheden in een hub IoT te verhogen.

>[AZURE.IMPORTANT] Identiteit registerbewerkingen zijn bedoeld voor gebruik tijdens runtime in Apparaatbeheer en toewijzing van scenario's. Lezen of bijwerken van een groot aantal identiteiten apparaat wordt ondersteund door middel van [importeren en exporteren van projecten][lnk-importexport].

## <a name="next-steps"></a>Volgende stappen

Er zijn andere zoeken naar onderwerpen in deze handleiding voor ontwikkelaars van IoT Hub:

- [IoT Hub eindpunten][lnk-devguide-endpoints]
- [Querytaal voor twins, methoden en taken][lnk-devguide-query]
- [IoT Hub MQTT ondersteuning][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md