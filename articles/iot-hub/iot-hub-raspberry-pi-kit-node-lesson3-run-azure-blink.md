<properties
 pageTitle="Uitvoeren van de voorbeeldtoepassing apparaat-wolk-berichten verzenden | Microsoft Azure"
 description="Implementeren en uitvoeren van een voorbeeldtoepassing op uw frambozen Pi 3 die berichten verzendt naar IoT hub en de LED knippert."
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

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3.2 uitvoeren voorbeeldtoepassing apparaat-wolk-berichten verzenden

## <a name="321-what-you-will-do"></a>3.2.1 wat u doet

Implementeren en een voorbeeldtoepassing op uw berichten naar uw hub IoT verzendt frambozen Pi 3 uitgevoerd. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="322-what-you-will-learn"></a>3.2.2 Wat leert u

- Het gebruik van het hulpprogramma gulp te implementeren en uitvoeren van de voorbeeldtoepassing Node.js op de Pi.

## <a name="323-what-you-need"></a>3.2.3 wat u nodig hebt

- U moet hebben voltooid de vorige sectie in deze les: [een app Azure functie en een account Azure opslag te verwerken en opslaan van IoT hub berichten maken](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 krijg je IoT hub- en -verbindingsreeksen

De verbindingsreeks voor het apparaat wordt gebruikt voor het aansluiten van de Pi op uw hub IoT. De hub IoT verbindingsreeks wordt gebruikt om de IoT hub aan de apparaat-id die uw Pi in de hub IoT vertegenwoordigt.

- De IoT hub-verbindingsreeks ophalen door de volgende Azure CLI-opdracht uit te voeren:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`is de naam die u in les 2 hebt opgegeven. Gebruik `iot-sample` als de waarde van `{resource group name}` als u niet de waarde die u in les 2.

- De verbindingsreeks apparaat ophalen door de volgende opdracht uit te voeren:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`heeft dezelfde waarde als met de vorige opdracht. Gebruik `iot-sample` als de waarde van `{resource group name}` en `myraspberrypi` als de waarde van `{device id}` als u niet de waarde die u in les 2.

## <a name="325-configure-the-device-connection"></a>3.2.5 de apparaatverbinding configureren

1. Het configuratiebestand worden geïnitialiseerd door de volgende opdrachten:

    ```bash
    npm install
    gulp init
    ```

2. Open het configuratiebestand apparaat `config-raspberrypi.json` in Visual Studio-Code door de volgende opdracht uit te voeren:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Controleer de volgende vervangingen in het `config-raspberrypi.json` bestand:

  - **[Apparaat hostnaam of het IP-adres]** vervangen door het IP-adres of hostnaam die u hebt gekregen van `device-discovery-cli` of met de waarde overgenomen van wat u in les 1 hebt geconfigureerd.
  - Vervang **[IoT apparaat verbindingsreeks]** met de `device connection string` u verkregen.
  - Vervang **[IoT hub verbindingsreeks]** met de `iot hub connection string` u verkregen.

U het `config-raspberrypi.json` -bestand zodat u de voorbeeldtoepassing van uw computer kunt implementeren.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 implementeren en uitvoeren van de voorbeeldtoepassing

Implementeren en uitvoeren van de voorbeeldtoepassing op uw Pi door de volgende opdracht uit te voeren:

```bash
gulp
```

> [AZURE.NOTE] De standaard gulp-taak wordt uitgevoerd `install-tools`, `deploy`, en `run` taken opeenvolgend. In [Les 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)hebt u deze taken een voor een afzonderlijk.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 controleren of de steekproef toepassing werkt

Hier ziet u de LED die is verbonden met uw Pi elke twee seconden knipperen. Telkens wanneer de LED knippert, de stuurt een bericht naar uw hub IoT en geverifieerd dat het bericht zijn verzonden op de hub IoT. Bovendien voor elk van het bericht is ontvangen door de hub IoT, het bericht wordt weergegeven in het consolevenster. De voorbeeldtoepassing wordt automatisch beëindigd na 20 berichten te verzenden.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 samenvatting

U hebt geïmplementeerd en de nieuwe blink-voorbeeldtoepassing op uw Pi apparaat-wolk-berichten verzenden naar uw hub IoT uitgevoerd. U kunt verplaatsen naar de volgende sectie voor het controleren van uw berichten als ze zijn geschreven naar de rekening voor de opslag van Azure.

## <a name="next-steps"></a>Volgende stappen

[3.3 lezen berichten bewaard in Azure opslag](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)