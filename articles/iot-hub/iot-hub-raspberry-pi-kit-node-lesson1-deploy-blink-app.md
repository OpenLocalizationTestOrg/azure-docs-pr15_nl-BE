<properties
 pageTitle="Maken en implementeren van de toepassing blink | Microsoft Azure"
 description="De voorbeeldtoepassing Node.js van Github klonen en gulp voor de implementatie van deze toepassing op uw bord frambozen Pi 3. Deze voorbeeldtoepassing knippert de LED elke twee seconden op het moederbord aangesloten."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 maken en implementeren van de toepassing laten knipperen

## <a name="131-what-you-will-do"></a>1.3.1 wat u doet

De voorbeeldtoepassing Node.js van Github te klonen en de gulp gebruiken voor de implementatie van de voorbeeldtoepassing op uw Pi frambozen 3. De voorbeeldtoepassing knippert de LED elke twee seconden op het moederbord aangesloten. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="132-what-you-will-learn"></a>1.3.2 Wat leert u

- Het gebruik van de `device-discover-cli` gereedschap voor het ophalen van netwerkgegevens over uw Pi.
- Het implementeren en uitvoeren van de voorbeeldtoepassing op uw Pi.
- Het implementeren en foutopsporing op afstand worden uitgevoerd op de Pi-toepassingen.

## <a name="133-what-you-need"></a>1.3.3 wat u nodig hebt

U moet hebben voltooid in de volgende secties in les 1:

- [Uw apparaat configureren](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [De hulpprogramma's ophalen](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 het IP-adres en hostnaam de naam van uw Pi verkrijgen

Open een opdrachtprompt in Windows of macOS of Ubuntu een terminal-venster en voer de volgende opdracht:

```bash
devdisco list --eth
```

Hier ziet u uitvoer die vergelijkbaar is met de volgende:

![apparaatdetectie](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Let op de `IP address` en `hostname` van de Pi. U moet deze informatie verderop in deze sectie.

> [AZURE.NOTE] Zorg ervoor dat uw Pi is aangesloten op hetzelfde netwerk bevinden als uw computer. Bijvoorbeeld, als uw computer is verbonden met een draadloos netwerk terwijl de Pi op een bekabeld netwerk is aangesloten, kunnen er geen het IP-adres in de uitvoer van de devdisco.

## <a name="135-clone-the-sample-application"></a>1.3.5 de voorbeeldtoepassing klonen

U opent de voorbeeldcode door als volgt te werk:

1. De bibliotheek monster van Github klonen met de volgende opdracht:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. De voorbeeldtoepassing openen in Visual Studio-Code door de volgende opdrachten:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Repo-structuur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

De `app.js` -bestand in de `app` submap is het belangrijkste bronbestand met de code bepalen de LED.

### <a name="136-install-application-dependencies"></a>1.3.6 afhankelijkheden voor toepassingen installeren

De bibliotheken en andere modules die u nodig hebt voor de voorbeeldtoepassing met de volgende opdracht installeren:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 aansluiting van het apparaat configureren

Configureren van de apparaatverbinding, als volgt te werk:

1. Het configuratiebestand apparaat genereren door de volgende opdracht uit te voeren:

    ```bash
    gulp init
    ```

    Het configuratiebestand `config-raspberrypi.json` bevat de gebruikersreferenties die u gebruikt voor aanmelding in uw Pi. Als u wilt voorkomen dat de lekkage van de referenties van de gebruiker, het configuratiebestand wordt gegenereerd in de submap `.iot-hub-getting-started` van de basismap op uw computer.

2. Het configuratiebestand apparaat openen in Visual Studio-Code door de volgende opdracht uit te voeren:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Vervang de tijdelijke aanduiding `[device hostname or IP address]` met het IP-adres of de hostnaam die u in de sectie 1.3.4 krijgen.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Gefeliciteerd! De eerste voorbeeldtoepassing voor uw Pi hebt gemaakt.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 implementeren en uitvoeren van de voorbeeldtoepassing

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 Node.js en NPM installeren op uw Pi

Node.js en NPM op uw Pi installeren door de volgende opdracht uit te voeren:

```bash
gulp install-tools
```

Het duurt tien minuten duren de eerste keer dat u deze taak uitvoert.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 implementeren en uitvoeren van het monster app

Implementeren en uitvoeren van de voorbeeldtoepassing op door de volgende opdracht uit te voeren:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 controleren of de app werkt

U ziet nu de LED op de Pi elke twee seconden knipperen.  Als u niet dat de LED knippert ziet, Zie de [handleiding voor het oplossen van problemen](iot-hub-raspberry-pi-kit-node-troubleshooting.md) voor oplossingen voor veelvoorkomende problemen.
![Knipperende LED](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Gebruik `Ctrl + C` om de toepassing te beëindigen.

## <a name="139-summary"></a>1.3.9 overzicht

U hebt de vereiste hulpmiddelen voor het werken met uw Pi geïnstalleerd en geïmplementeerd, een voorbeeldtoepassing om uw Pi de LED knipperen. U kunt nu op verplaatsen naar de volgende les wilt maken, implementeren en uitvoeren van een andere voorbeeldtoepassing waarmee uw Pi Azure IoT Hub voor het verzenden en ontvangen van berichten verbinding maakt.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om les 2 die begint met het [ophalen van de hulpprogramma's voor Azure](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
