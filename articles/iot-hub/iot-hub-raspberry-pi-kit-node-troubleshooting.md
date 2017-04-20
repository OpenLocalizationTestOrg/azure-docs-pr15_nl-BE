<properties
 pageTitle="Het oplossen van problemen | Microsoft Azure"
 description="De pagina voor probleemoplossing voor frambozen Pi Node.js ervaring"
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

# <a name="troubleshooting"></a>Het oplossen van problemen

## <a name="hardware-issues"></a>Hardwareproblemen

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Ook door de toepassing wordt uitgevoerd, maar niet de LED knippert

Dit probleem is altijd gerelateerd aan de hardware circuit-connectiviteit. Gebruik de volgende stappen uit om problemen te identificeren.

1. Controleer als u de juiste **GPIO** op het bord. In deze les twee poorten moeten **GPIO GND (Pin 6)** en **GPIO 04 (Pin 7)**.
2. Controleer of de polariteit van de LED juist is. De arm langer **positief**, pin anode aangeven.
3. Het **vastmaken van 3,3 v** en **GND Pin** op de Raspberry Pi 3 gebruiken. Uw Pi behandelen als de gelijkstroom. Controleer als de LED prima werkt.

![LED-spec](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Andere hardwareproblemen

Raadpleeg voor meer informatie over het oplossen van veelvoorkomende problemen op de Pi frambozen 3 naar de [officiële pagina voor probleemoplossing](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problemen met node.js pakket

### <a name="no-response-during-gulp-tasks"></a>Geen reactie tijdens gulp taken

Als u met gulp taken problemen, kunt u toevoegen de `--verbose` optie voor foutopsporing. Probeer het beëindigen van de huidige taken van de gulp met `Ctrl + C` en voer de volgende in uw consolevenster opdracht zien foutopsporingsberichten. Hier ziet u gedetailleerde foutberichten worden weergegeven in de console-uitvoer wordt afgedrukt. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problemen met apparaat ontdekking

Voor hulp bij het oplossen van veelvoorkomende problemen met de `devdisco` opdracht, controleert u het [Leesmij-bestand](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Andere problemen NPM

Probeer het NPM pakket bijwerken met de volgende opdracht:

```bash
npm install -g npm
```

Als het probleem blijft bestaan, laat uw opmerkingen aan het einde van dit artikel of een Github-probleem in onze [Steekproef bibliotheek](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started) maken

## <a name="remote-debugging"></a>Foutopsporing op afstand

### <a name="run-the-sample-application-in-debug-mode"></a>De voorbeeldtoepassing uitvoeren in de foutopsporingsmodus

```bash
gulp run --debug
```

Wanneer de motor debug gereed is, ziet u ```Debugger listening on port 5858``` in de console-uitvoer.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Code van de VS om verbinding met het externe apparaat te configureren

Open het deelvenster **Foutopsporing** aan de linkerkant.

Klik op de groene knop voor **Foutopsporing starten** (F5). VS-Code wordt geopend een **launch.json** -bestand dat u wilt bijwerken.

Het bestand **launch.json** met de volgende inhoud bijwerken. Vervangen `[device hostname or IP address]` met de werkelijke IP-adres of hostnaam.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Configuratie van externe foutopsporing](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Aan de externe toepassing koppelen

Klik op de groene knop **Start Debugging** (F5) foutopsporing te starten. 

[JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) voor meer informatie over foutopsporing lezen.

![Externe interactieve foutopsporing](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI-problemen

Azure CLI is een build van het voorbeeld. U kunt verwijzen naar de [Preview installeren gids](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) te zoeken naar oplossingen.

Als u met het hulpprogramma fouten tegenkomt, het bestand een [probleem](https://github.com/Azure/azure-cli/issues) in de sectie **problemen** van de repo GitHub.

Raadpleeg het [Leesmij-bestand](https://github.com/Azure/azure-cli/blob/master/README.rst)voor hulp bij het oplossen van veelvoorkomende problemen.

## <a name="python-installation-issues"></a>Python installatieproblemen

### <a name="legacy-installation-issues-macos"></a>Problemen met de oude installatie (macOS)

Bij het installeren van **pip**een machtigingsfout gegenereerd wanneer er oudere pakketten die zijn geïnstalleerd met **su** -machtigingen zijn. Deze situatie doet zich voor omdat de vorige installatie van Python met brew (macOS) niet volledig is verwijderd. Sommige **pip** -pakketten van een vorige installatie zijn gemaakt door root, waardoor de fout toegang. De oplossing is voor het verwijderen van de pakketten geïnstalleerd door de root. Gebruik de volgende stappen uit om deze taak te voltooien:

1. Ga naar /usr/local/lib/python2.7/site-packages
2. Pakketten maken met de hoofdmap:`ls -l | grep root`
3. Pakketten verwijderen uit stap 2:`sudo rm -rf {package name}`
4. Installeer Python.

## <a name="azure-iot-hub-issues"></a>Problemen met Azure IoT hub

Als u met succes hebt ingericht en Azure IoT hub met `azure-cli`, en moet u een hulpprogramma om verbinding te maken met de hub IoT apparaten te beheren, probeert u de volgende hulpprogramma's:

### <a name="device-explorer"></a>Apparaat Explorer

[Apparaat Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) wordt uitgevoerd op uw lokale Windows-computer en maakt verbinding met de IoT hub in Azure. Communiceren met de volgende [eindpunten IoT Hub](iot-hub-devguide.md):

- *Identiteit van Apparaatbeheer* voor het inrichten en beheren van apparaten bij de hub IoT geregistreerd.
- *Ontvangen van apparaat naar cloud* om te controleren van berichten die worden verzonden vanaf het apparaat op uw hub IoT.
- *Wolk naar apparaat verzenden* naar kunt u berichten verzenden naar uw apparaten van uw hub IoT.

Configureer de `IoT hub connection string` binnen dit hulpprogramma te gebruiken van de mogelijkheden ervan.

### <a name="iot-hub-explorer"></a>IoT hub Explorer

[IoT hub Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) is een monster meerdere platforms CLI apparaat clients beheren. Het hulpprogramma kunt u beheren de apparaten in het register van de identiteit, apparaat-wolk-berichten en opdrachten wolk naar apparaat verzenden.

Om de meest recente versie (bètaversie) van het hulpprogramma iothub explorer installeert, moet u de volgende opdracht uitvoeren in uw omgeving vanaf de opdrachtregel:

```
npm install -g iothub-explorer@latest
```

De volgende opdracht kunt u extra Help-informatie over alle iothub explorer opdrachten en parameters:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Azure portal gebruiken om uw bronnen te beheren

In deze lessen is een volledige CLI ervaring maken en beheren van uw Azure bronnen beschikbaar. U kunt ook de [Azure portal](../azure-portal-overview.md) gebruiken om te helpen bepaling, beheren en fouten opsporen in uw Azure resources.

## <a name="azure-storage-issues"></a>Problemen met Azure opslag

[Microsoft Azure Opslagverkenner (voorbeeld)](http://storageexplorer.com) is een zelfstandige toepassing van Microsoft waarmee u kunt werken met gegevens in Azure opslag op Windows, macOS en Linux. Met dit hulpprogramma kunt u verbinding maken met de tabel en de gegevens. Met dit hulpprogramma kunt u problemen met uw opslag Azure.
