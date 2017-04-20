<properties
 pageTitle="Beheerde apparaten achter een gateway IoT | Microsoft Azure"
 description="Richtlijnen onderwerp via een IoT Gateway gemaakt met de SDK IoT Gateway en apparaten beheerd door IoT Hub."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Beheerde apparaten achter een IoT-gateway inschakelen

## <a name="basic-device-isolation"></a>Basisstuurprogramma isolatie

Organisaties gebruiken vaak IoT gateways naar verbetert de algehele beveiliging van hun IoT oplossingen. Sommige apparaten nodig om gegevens te verzenden naar de cloud, maar zijn niet geschikt om zelf te beschermen tegen bedreigingen op het internet. U kunt deze apparaten uit externe threads beschermen door ze communiceren met de buitenwereld via een gateway.

De gateway bevindt zich aan de grens tussen een beveiligde omgeving en open internet. Apparaten communiceren met de gateway en de berichten langs de gateway doorstuurt naar de bestemming correct wolk. De gateway is harde tegen externe threads, blokkeert onbevoegde aanvragen kan geautoriseerde in verkeer en stuurt dit verkeer-gebonden met het juiste apparaat.

![][1]

U kunt ook apparaten die zichzelf achter een gateway voor een extra beveiligingslaag beschermen plaatsen. In dit scenario moet u alleen de gateway OS patches tegen de laatste beveiligingslekken, in plaats van het bijwerken van het besturingssysteem op elk apparaat behouden.

## <a name="isolation-plus-intelligence"></a>Isolatie plus intelligence

Een harde router is een voldoende gateway apparaten te isoleren. IoT oplossingen vereisen vaak echter dat een gateway meer intelligentie biedt dan gewoon isoleren van apparaten. U wilt bijvoorbeeld uw apparaten beheren vanuit de cloud. U zijn kunt gebruik [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), een standaard device management-protocol voor de cloud management deel van de oplossing. Echter verzenden de apparaten met behulp van een TCP/IP niet telemetrie ingeschakeld protocol. Voorts de apparaten produceren grote hoeveelheden gegevens en wilt u alleen een gefilterde subset van de telemetrie uploaden. U kunt een oplossing bouwen die is uitgerust met een IoT gateway kunnen omgaan met twee verschillende streams van gegevens. De gateway moet:

-   De **telemetrie**begrijpen, filteren en deze vervolgens uploaden naar de cloud via de gateway. De gateway is niet langer een eenvoudige router die gewoon gegevens door tussen het apparaat en de cloud stuurt.

-   Gewoon de **LWM2M device management gegevens** tussen de apparaten en de cloud uitwisselen. De gateway hoeft niet te begrijpen van gegevens die afkomstig zijn geladen en alleen nodig om ervoor te zorgen dat de gegevens heen en weer tussen de apparaten en de cloud wordt doorgegeven.

De volgende afbeelding ziet u in dit scenario:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>De oplossing: Azure IoT device management en de Gateway IoT SDK 

De openbare preview-release van [Azure IoT device management] [ lnk-device-management] en bèta-versie van de [SDK van Azure IoT Gateway] inschakelen in dit scenario. De gateway waarop elke gegevensstroom als volgt verwerkt:

-   **Telemetrie**: U kunt de SDK IoT Gateway bouwen van een pijpleiding die begrijpt, filters en telemetriegegevens worden verzonden naar de cloud. De Gateway IoT SDK bevat code die u implementeert de onderdelen van deze pijpleiding voor de ontwikkelaar. U vindt meer informatie over de architectuur van de SDK in de [SDK voor IoT Gateway - aan de slag] [ lnk-gateway-get-started] zelfstudie.

-   **Apparaatbeheer**: Apparaatbeheer Azure biedt een LWM2M-client die wordt uitgevoerd op het apparaat als een wolk-interface voor de afgifte van opdrachten voor het beheer aan het apparaat.
    
    U nodig niet alle speciale logica op de gateway omdat dit hoeft niet op de LWM2M gegevens uitgewisseld tussen het apparaat en de hub IoT. U kunt internet-verbinding delen, een onderdeel van veel moderne besturingssystemen op de gateway voor het uitwisselen van gegevens voor LWM2M. U kunt een geschikt besturingssysteem voor dit scenario omdat de Gateway IoT SDK een groot aantal besturingssystemen ondersteunt. Hier vindt u instructies voor het inschakelen van internet-verbinding delen in [Windows 10] en [Ubuntu], twee van de vele ondersteunde besturingssystemen.

De volgende afbeelding ziet u het hoogste niveau architectuur gebruikt voor het inschakelen van dit scenario met [Azure IoT device management] [ lnk-device-management] en de [Azure IoT Gateway SDK].

![][3]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende zelfstudies voor meer informatie over het gebruik van de Gateway IoT SDK:

- [IoT Gateway SDK - aan de slag met Linux][lnk-gateway-get-started]
- [IoT Gateway SDK-verzenden apparaat-wolk-berichten met een gesimuleerde apparaat met Linux][lnk-gateway-simulated]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure IoT Gateway SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md