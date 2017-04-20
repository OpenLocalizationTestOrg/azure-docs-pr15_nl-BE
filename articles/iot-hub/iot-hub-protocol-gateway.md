<properties
   pageTitle="Azure IoT-protocol gateway | Microsoft Azure"
   description="Wordt beschreven hoe u een gateway voor Azure IoT-protocol uit te breiden de mogelijkheden en ondersteuning van Azure IoT Hub-protocol gebruiken."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>Ondersteuning van aanvullende protocollen voor IoT Hub

Azure IoT Hub ondersteunt communicatie via de MQTT, AMQP en HTTP-protocollen. In sommige gevallen, apparaten of gateways veld mogelijk niet een van deze standaard protocollen te gebruiken en zal moeten worden aangepast protocol. In dergelijke gevallen kunt u een aangepaste gateway. Een aangepaste gateway kunt protocol aanpassing voor IoT Hub eindpunten inschakelen door het overbruggen van het verkeer van en naar de IoT Hub. U kunt de [Azure IoT-protocol gateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) als aangepaste gateway aanpassing voor IoT Hub protocol inschakelen.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT-protocol gateway

De gateway Azure IoT-protocol is een raamwerk voor de aanpassing van protocol dat is ontworpen voor hoge schaalbaarheid, bidirectionele communicatie tussen met IoT Hub. De gateway protocol is een Pass Through-onderdeel dat apparaatverbindingen op een bepaald protocol accepteert. Het verkeer op de IoT Hub UC overbrugt via AMQP 1.0. De gateway Azure IoT-protocol is beschikbaar als een project van open source software om flexibiliteit te bieden voor het toevoegen van ondersteuning voor een groot aantal protocollen en protocol versies.

U kunt de gateway protocol in Azure in een zeer schaalbare manier implementeren met behulp van Azure Cloud Services werknemer rollen. Bovendien kan de gateway protocol worden ingezet in voor bedrijfsruimten omgevingen, zoals een veld gateways.

De gateway Azure IoT-protocol bevat een MQTT protocol adapter waarmee u het MQTT protocol gedrag aanpassen indien nodig. Omdat IoT Hub ingebouwde ondersteuning voor het protocol MQTT v3.1.1 biedt, moet u alleen overwegen het protocol MQTT adapter hebt u behoefte aan een protocol aanpassingen of specifieke eisen voor extra functionaliteit.

Het programmeermodel voor het bouwen van adapters voor andere protocollen protocol wordt ook getoond in de MQTT-adapter. Bovendien kunt het programmeermodel van Azure IoT-protocol gateway u aangepaste onderdelen voor speciale verwerking zoals aangepaste verificatie, bericht transformaties, compressie/decompressie of ontsleutelen van verkeer tussen de apparaten en IoT Hub aansluit.

Flexibiliteit, de gateway protocol en de uitvoering van de MQTT vindt u in een project van open source software. Hiermee kunt u de toepassing aanpassen indien nodig.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de gateway Azure IoT-protocol en het te gebruiken en te implementeren als onderdeel van uw oplossing IoT, Zie:

* [Azure IoT-protocol gateway opslagplaats op GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT gateway protocol ontwikkelaarshandleiding](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Voor meer informatie over het plannen van uw implementatie IoT Hub, Zie:

- [Vergelijken met de gebeurtenis Hubs][lnk-compare]
- [Schalen, HA en DR][lnk-scaling]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
