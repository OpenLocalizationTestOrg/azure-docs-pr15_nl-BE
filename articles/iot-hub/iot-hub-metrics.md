<properties
 pageTitle="Diagnostische metrics IoT Hub"
 description="Een overzicht van Azure IoT Hub metrics, waarin de gebruiker voor de beoordeling van de algehele gezondheid van de bron"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Inleiding tot diagnostische metrics

Diagnostische gegevens krijgt u betere gegevens over de toestand van de Azure resources van uw abonnement op Azure. Metrische gegevens kunt u voor de beoordeling van de algehele gezondheid van de service en de erop aangesloten apparaten. Gebruikerspagina statistieken zijn belangrijk omdat ze zien wat er gebeurt met uw problemen IoT hub en help de onderliggende oorzaak te zonder contact op met ondersteuning van Azure.

U kunt diagnostische maatstaven van het portal voor Azure inschakelen.

## <a name="how-to-enable-diagnostic-metrics"></a>Het inschakelen van diagnostische gegevens

1. Maak een hub IoT. U vindt instructies voor het maken van een hub IoT in de [Slag] [ lnk-get-started] gids.

2. Open het blad van de hub IoT. Klik op **Diagnostische gegevens**vanuit deze map.

    ![][1]

3. De diagnostische gegevens van de status is ingesteld op **aan** en selecteer een account Azure opslagruimte voor het opslaan van de diagnostische gegevens configureren. Controleer **Metrics**en drukt u vervolgens op **Opslaan**. Houd er rekening mee dat de account Azure opslag tijdig moet worden gemaakt en dat u afzonderlijk in rekening voor opslag gebracht worden. U kunt ook diagnostische gegevens verzenden naar een eindpunt gebeurtenis Hubs.

    ![][2]

4. Nadat u de diagnostische gegevens hebt ingesteld, terug naar het **overzicht van** IoT hub blade. Metrische gegevens is in de sectie **bewaking** van het blad gevuld. Op de grafiek te klikken, wordt het deelvenster parameters kunt u een overzicht weergeven van de metrische gegevens voor uw IoT hub en de keuze van de parameters die worden weergegeven in het diagram bewerken geopend. U kunt ook waarschuwingen op basis van metrische waarden.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Maatstaven en het gebruik ervan

IoT Hub biedt diverse maatstelsels om u een overzicht geven van de gezondheid van de hub en het totale aantal apparaten zijn aangesloten. U kunt gegevens uit meerdere cijfers tekenen van een grotere afbeelding van de status van de hub IoT combineren. De volgende tabel beschrijft de maatstaven die elke IoT hub wordt bijgehouden en hoe elke maateenheid betrekking heeft op de algehele status van de hub IoT.

| Metric | Metrische beschrijving | Wat de metric wordt gebruikt |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | Het aantal berichten op alle apparaten | Overzicht gegevens op het bericht wordt verzonden |
| d2c.telemetry.ingress.Success | De telling van alle geslaagde berichten op de hub | Overzicht van ingress bericht is voltooid op de hub |
| c2d.Commands.egress.complete.Success | De telling van alle berichten bij de opdracht uitgevoerd door het ontvangende apparaat op alle apparaten | Met de parameters van de methode abandon en afkeuren geeft een overzicht van de algehele slagingspercentage van C2D-opdracht |
| c2d.Commands.egress.Abandon.Success | De telling van alle berichten die zijn achtergelaten door het ontvangende apparaat op alle apparaten | Mogelijke problemen worden gemarkeerd als berichten vaker achtergelaten ophalen dan verwacht |
| c2d.Commands.egress.Reject.Success | De telling van alle berichten met succes zijn geweigerd door het ontvangende apparaat op alle apparaten | Mogelijke problemen worden gemarkeerd als berichten worden vaker dan verwacht afgewezen |
| devices.totalDevices | De gemiddelde, min en max van het aantal apparaten op de hub IoT geregistreerd | Het aantal apparaten op de hub geregistreerd |
| devices.connectedDevices.allProtocol | De gemiddelde, min en max van het aantal gelijktijdig aangesloten apparaten | Overzicht van het aantal apparaten aangesloten op de hub |

## <a name="next-steps"></a>Volgende stappen

U kunt een overzicht van de diagnostische gegevens hebt gezien, volg deze koppeling voor meer informatie over het beheren van Azure IoT Hub:

- [Activiteiten controleren][lnk-monitor]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
