<properties
 pageTitle="De voorbeeldtoepassing wolk naar apparaat berichten ontvangen uitvoeren | Microsoft Azure"
 description="De voorbeeldtoepassing in les 4 wordt uitgevoerd op de Pi en inkomende berichten van uw hub IoT. Een nieuwe taak voor de gulp verzendt berichten naar uw Pi van uw hub IoT LED knipperen."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 uitvoert in de voorbeeldtoepassing cloud-to-device-berichten ontvangen

In dit gedeelte vindt implementeren u een voorbeeldtoepassing op uw Pi frambozen 3. De voorbeeldtoepassing controleert inkomende berichten van uw hub-IoT. U kunt ook een gulp taak uitvoeren op uw computer om berichten te verzenden naar uw Pi van uw hub IoT. De voorbeeldtoepassing knippert bij ontvangst van de berichten, de LED. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="411-what-you-will-do"></a>4.1.1 wat u doet

- Verbinding maken met de voorbeeldtoepassing op uw hub IoT.
- Implementeren en uitvoeren van de voorbeeldtoepassing.
- Berichten verzenden vanaf uw hub-IoT naar uw Pi de LED knipperen.

## <a name="412-what-you-will-learn"></a>4.1.2 Wat leert u

- Het controleren van inkomende berichten van uw hub-IoT.
- Het wolk naar apparaat om berichten te verzenden vanaf uw hub-IoT aan uw Pi. 

## <a name="413-what-do-you-need"></a>4.1.3 wat hebt u nodig

- Een frambozen Pi-3 die is ingesteld voor gebruik. Zie voor informatie over het instellen van de Pi, [Les 1: aan de slag met uw apparaat frambozen Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
- Een hub die is gemaakt in uw abonnement Azure IoT. Zie voor informatie over het maken van uw Hub Azure IoT, [Les 2: uw Azure IoT Hub maken](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 de voorbeeldtoepassing op uw hub IoT verbinding

1. Zorg ervoor dat u in de map repo `iot-hub-node-raspberrypi-getting-started`. De voorbeeldtoepassing openen in Visual Studio-Code door de volgende opdrachten:

    ```bash
    cd Lesson4
    code .
    ```

    Aankondiging van de `app.js` -bestand in de `app` submap. De `app.js` bestand is de belangrijkste bron met de code voor het controleren van inkomende berichten van IoT Hub. De `blinkLED` functie de LED knippert.

    ![Repo-structuur](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Initialiseren van het configuratiebestand met de volgende opdrachten:

    ```bash
    npm install
    gulp init
    ```

    Als u les 3 op deze computer hebt voltooid, zijn alle configuraties zodat kunt u verdergaan met stap 4.1.5 overgenomen. Als u op een andere computer les 3 hebt uitgevoerd, moet u vervangen de tijdelijke aanduidingen in de `config-raspberrypi.json` bestand. De `config-raspberrypi.json` bestand bevindt zich in de submap van de basismap.

    ![Config](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- **[Apparaat hostnaam of het IP-adres]** vervangen door het IP-adres of hostnaam die u door de opdracht te krijgen van uw Pi`devdisco list --eth`
- **[IoT apparaat verbindingsreeks]** vervangen door de verbindingsreeks apparaat die u met de opdracht `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- **[IoT hub verbindingsreeks]** vervangen door de IoT hub verbindingstekenreeks die u met de opdracht `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 implementeren en uitvoeren van de voorbeeldtoepassing

Implementeren en uitvoeren van de voorbeeldtoepassing op uw Pi door de volgende opdrachten:
  
```
gulp
```

De gulp-opdracht wordt eerst de installatie van extra taak uitgevoerd. Vervolgens wordt de voorbeeldtoepassing op uw Pi gedistribueerd. Ten slotte wordt de toepassing uitgevoerd op uw Pi en een afzonderlijke taak op de hostcomputer 20 knipperen om berichten te verzenden naar uw Pi van uw hub IoT.

Zodra de voorbeeldtoepassing wordt uitgevoerd, wordt het luisteren van berichten van uw hub-IoT. Ondertussen stuurt de gulp taak meerdere berichten 'blink' van uw IoT Hub naar uw Pi. Voor elk van de blink-bericht ontvangen, roept de voorbeeldtoepassing de functie blinkLED om de LED laten knipperen.

Hier ziet u de LED knippert elke twee seconden als de taak gulp 20 berichten worden verzonden vanaf uw hub-IoT naar uw Pi. Het laatste is een 'stop' weergegeven waarin wordt gemeld de toepassing dat afgesloten.

![Gulp](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 samenvatting

U hebt verzonden berichten vanaf uw hub-IoT aan uw Pi de LED knipperen. Volgende sectie is een optionele sectie waarin u het wijzigen van de in- en uit het gedrag van de LED.

## <a name="next-steps"></a>Volgende stappen

[Optionele sectie: de aan-en uit het gedrag van de LED](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)