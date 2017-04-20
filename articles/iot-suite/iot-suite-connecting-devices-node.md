<properties
   pageTitle="Verbinding maken met een apparaat met Node.js | Microsoft Azure"
   description="Wordt beschreven hoe u een apparaat aansluit op de Azure IoT Suite vooraf geconfigureerde remote monitoring oplossing met een toepassing die is geschreven in Node.js."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Verbind het apparaat met de externe controle vooraf geconfigureerde oplossing (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Bouwen en uitvoeren van de monsteroplossing node.js

1. Klonen van de opslagplaats van de GitHub *Microsoft Azure IoT SDK's* en installeer het *Microsoft Azure IoT apparaat SDK voor Node.js* in uw desktopomgeving, gaat u als volgt het [voorbereiden van uw ontwikkelomgeving] [ lnk-github-prepare] instructies.

2. In uw lokale exemplaar van de [azure-iot-SDK's] [ lnk-github-repo] opslagplaats, de volgende twee bestanden kopiëren van de map samples/apparaat/knooppunt naar een map op het apparaat:

  - Packages.JSON
  - remote_monitoring.js

3. Open het bestand remote_monitoring.js en zoek naar de volgende variabele:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. **[IoT Hub apparaat verbindingsreeks]** vervangen door de verbindingsreeks voor het apparaat. U vindt de waarden voor de IoT Hub hostname, apparaat-id en apparaatsleutel in het remote monitoring oplossing dashboard. Een verbindingsreeks apparaat heeft de volgende indeling:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Als uw hostnaam IoT Hub **contoso** is uw apparaat-id is **mydevice**, wordt de verbindingsreeks ziet:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Sla het bestand. Voer de volgende opdrachten bij een opdrachtprompt in de map waarin deze bestanden de nodige pakketten installeren en vervolgens de voorbeeldtoepassing uitvoeren:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md