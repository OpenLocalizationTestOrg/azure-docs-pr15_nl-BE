<properties
 pageTitle="Handleiding voor ontwikkelaars - SDK's IoT-Hub | Microsoft Azure"
 description="Azure IoT Hub developer guide - informatie over en koppelingen naar de verschillende Azure IoT Hub apparaten en Services SDK's."
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>SDK's IoT Hub

## <a name="iot-hub-device-sdks"></a>IoT apparaat SDK 's

Het Microsoft Azure IoT apparaat SDK's bevatten code die gebouw apparaten en toepassingen die verbinding maken met en worden beheerd door IoT-Hub Azure services vergemakkelijkt.

De volgende IoT apparaat SDK's beschikbaar zijn voor downloaden van GitHub:

- [Azure IoT apparaat SDK voor C] [ lnk-c-device-sdk] geschreven in ANSI C (C99) voor draagbaarheid en brede platformcompatibiliteit.
- [Azure IoT apparaat SDK voor .NET][lnk-dotnet-device-sdk]
- [Azure IoT apparaat SDK voor Java][lnk-java-device-sdk]
- [Azure IoT apparaat SDK voor Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT apparaat SDK voor Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Raadpleeg de Leesmij-bestanden in de GitHub opslagplaatsen voor informatie over het installeren van binaire bestanden en afhankelijkheden op de ontwikkelcomputer met taal en platform-specifieke pakket managers.

## <a name="os-platforms-and-hardware-compatibility"></a>OS Platforms en hardware compatibiliteit

Zie voor meer informatie over compatibiliteit met bepaalde hardware-apparaten SDK de [Azure gecertificeerd voor IoT apparaat catalogus][lnk-certified].

## <a name="iot-hub-service-sdks"></a>IoT Hub service SDK 's

De service Microsoft Azure IoT-SDK's bevatten code waarmee toepassingen maken die in combinatie met IoT Hub rechtstreeks voor het beheren van apparaten en beveiliging vereenvoudigt.

De volgende IoT service SDK's beschikbaar zijn voor downloaden van GitHub:

- [Azure IoT service SDK voor .NET][lnk-dotnet-service-sdk]
- [Azure IoT service SDK voor Node.js][lnk-node-service-sdk]
- [Azure IoT service SDK voor Java][lnk-java-service-sdk]

> [AZURE.NOTE] Raadpleeg de Leesmij-bestanden in de GitHub opslagplaatsen voor informatie over het installeren van binaire bestanden en afhankelijkheden op de ontwikkelcomputer met taal en platform-specifieke pakket managers.

## <a name="azure-iot-gateway-sdk"></a>Azure IoT Gateway SDK

Deze SDK Azure IoT Gateway bevat de infrastructuur en modules om IoT gateway-oplossingen te maken. U kunt de SDK als u wilt maken die zijn toegesneden op de end-to-end scenario gateways uitbreiden.

U kunt de [Azure IoT Gateway SDK] downloaden[ lnk-gateway-sdk] van GitHub.

## <a name="online-api-reference-documentation"></a>On line documentatie voor API reference

Hieronder volgt een lijst met koppelingen naar de on line documentatie van API reference voor Azure IoT apparaat, service en gateway-bibliotheken:

- [Internet van dingen (IoT) .NET][lnk-dotnet-ref]
- [IoT Hub REST][lnk-rest-ref]
- [Azure IoT apparaat SDK voor C][lnk-c-ref]
- [Azure IoT apparaat SDK voor Java][lnk-java-ref]
- [Azure IoT service SDK voor Java][lnk-java-service-ref]
- [Azure IoT apparaat SDK voor Node.js][lnk-node-ref]
- [Azure IoT service SDK voor Node.js][lnk-node-service-ref]
- [Azure IoT gateway SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Volgende stappen

Er zijn andere zoeken naar onderwerpen in deze handleiding voor ontwikkelaars van IoT Hub:

- [IoT Hub eindpunten][lnk-devguide-endpoints]
- [Querytaal voor twins, methoden en taken][lnk-devguide-query]
- [Quota's en bandbreedteregeling][lnk-devguide-quotas]
- [IoT Hub MQTT ondersteuning][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md