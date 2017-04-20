<properties
    pageTitle="Een echt apparaat gebruiken met de SDK IoT Gateway | Microsoft Azure"
    description="Azure IoT Gateway SDK scenario een Texas Instruments SensorTag-apparaat gebruikt om gegevens te verzenden naar IoT Hub via een gateway die wordt uitgevoerd op een Intel Edison Compute Module"
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT Gateway SDK (beta) – verzenden apparaat-wolk-berichten met een echt apparaat met Linux

In dit scenario van het [Bluetooth-lage energie monster] [ lnk-ble-samplecode] ziet u hoe u de [SDK van Azure IoT Gateway] [ lnk-sdk] voor forward apparaat naar cloud telemetrie op IoT-Hub van een fysiek apparaat en hoe u opdrachten van IoT Hub met een fysiek apparaat.

Dit scenario omvat:

* **Architectuur**: belangrijke architectonische informatie over het monster Bluetooth-lage energie.

* **Bouwen en uitvoeren**: de vereiste stappen voor het maken en uitvoeren van het monster.

## <a name="architecture"></a>Architectuur

De procedure ziet u hoe te bouwen en uitvoeren van een IoT-Gateway op een Intel Edison Compute Module waarop Linux wordt uitgevoerd. De gateway is gebouwd met behulp van de Gateway IoT SDK. Het monster een Texas Instruments SensorTag Bluetooth Low Energy (bel)-apparaat gebruikt om temperatuur gegevens te verzamelen.

Wanneer u de gateway uitvoert het:

- Maakt verbinding met een SensorTag-apparaat met behulp van het protocol Bluetooth Low Energy (bel).
- Maakt verbinding met IoT Hub met behulp van het HTTP-protocol.
- Telemetrie doorstuurt van het SensorTag apparaat naar IoT Hub.
- Routes opdrachten van IoT Hub het apparaat SensorTag.

De gateway bevat de volgende modules:

- Een *Bel-module* die in combinatie met een apparaat Bel temperatuur om gegevens te ontvangen uit het apparaat en verzenden naar het apparaat.
- Een *Wolk Bel naar apparaat module* die de JSON-berichten die afkomstig zijn van de wolk in Bel-instructies voor de *module Bel*vertaalt.
- Een *logboek module* die alle berichten van de gateway registreert.
- Een *module voor de toewijzing van beveiligingsidentiteit* tussen Azure IoT Hub apparaat identiteiten en bel apparaat MAC-adressen omgezet.
- Een *IoT Hub module* telemetriegegevens uploaden naar een IoT hub en apparaatopdrachten ontvangt van een hub IoT.
- *Bel printer module* dat telemetrie van het apparaat Bel interpreteert en afgedrukt opgemaakte gegevens naar de console inschakelen voor probleemoplossing en foutopsporing.

### <a name="how-data-flows-through-the-gateway"></a>Hoe gegevens worden uitgewisseld via de Gateway

Het volgende blokdiagram illustreert de telemetrie uploaden data stroom pijpleiding:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

De stappen waarmee een item van telemetrie onderweg vanaf een apparaat Bel met IoT Hub zijn:

1. Het apparaat Bel genereert een monster van de temperatuur en stuurt dit via Bluetooth naar de module Bel in de gateway.
2. De module Bel ontvangt het monster en publiceert deze op de makelaar samen met het MAC-adres van het apparaat.
3. De module identity toewijzing van dit bericht wordt opgehaald en een interne tabel gebruikt voor het vertalen van het MAC-adres van het apparaat in een IoT Hub apparaat identiteit (apparaat-ID en apparaatsleutel). Vervolgens een nieuw bericht met de voorbeeldgegevens temperatuur, het MAC-adres van het apparaat, de apparaat-ID en de sleutel van het apparaat wordt gepubliceerd.
4. De module IoT Hub ontvangt deze nieuw bericht (gegenereerd door de toewijzing identity module) en publiceert deze op IoT Hub.
5. De module logger registreert alle berichten uit de broker naar een diskette.

In het volgende blokdiagram ziet u de apparaat opdracht-pipeline voor stroom:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. De module IoT Hub controleert regelmatig de IoT hub voor nieuwe berichten.
2. Wanneer de module IoT Hub een nieuw bericht ontvangt, worden deze gepubliceerd met de makelaar.
3. De module identity toewijzing bericht opvangt en een interne tabel gebruikt voor het vertalen van de IoT Hub apparaat-ID naar MAC-adres van een apparaat. Vervolgens een nieuw bericht met het MAC-adres van het doelapparaat in het overzicht van de eigenschappen van het bericht wordt gepubliceerd.
4. De Cloud Bel naar apparaat module dit bericht wordt opgehaald en omgezet in de juiste tabel-instructie voor de module Bel. Vervolgens wordt een nieuw bericht gepubliceerd.
5. De module Bel dit bericht wordt opgehaald en de i/o-instructies worden uitgevoerd door te communiceren met het apparaat Bel.
6. De module logger registreert alle berichten uit de broker naar een diskette.

## <a name="prepare-your-hardware"></a>Uw hardware voorbereiden

In deze zelfstudie wordt ervan uitgegaan dat u een [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) -apparaat dat is verbonden met een Intel Edison-kaart.

### <a name="set-up-the-edison-board"></a>Instellen van de Raad van Edison

Voordat u begint, moet u ervoor zorgen dat kunt u uw apparaat Edison op uw draadloze netwerk. Als u uw apparaat Edison instelt, moet u deze verbinding maken met een host-computer. Intel biedt aan de slag handleidingen voor de volgende besturingssystemen:

- [Aan de slag met de Intel Edison Development Board op 64-bits Windows][lnk-setup-win64].
- [Aan de slag met de Intel Edison Development Board op Windows 32-bits][lnk-setup-win32].
- [Aan de slag met de Intel Edison Development Board op Mac OS X][lnk-setup-osx].
- [Aan de slag met de Intel® Edison college op Linux][lnk-setup-linux].

Stel het apparaat Edison en vertrouwd raken met deze, overigens alle artikelen de stappen in deze 'aan de slag", met uitzondering van de laatste stap"Kies IDE", die voor de huidige zelfstudie is niet nodig. U hebt aan het einde van de installatieprocedure Edison:

- De Edison met de nieuwste firmware geknipperd.
- Een seriële verbinding van de host naar de Edison vastgesteld.
- Voer het script **configure_edison** WiFi op uw Edison en een wachtwoord instellen.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Connectiviteit met het SensorTag-apparaat van uw bord Edison inschakelen

Voordat u het voorbeeld uitvoert, moet u controleren of uw kamer Edison verbinding met het apparaat SensorTag maken kunt.

Moet u eerst controleren of uw Edison verbinding met het apparaat SensorTag maken kunt.

1. Blokkering van bluetooth op de Edison en controleer of het versienummer **5.37 is**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. De opdracht **bluetoothctl** uitvoeren. U bent nu een bluetooth interactieve shell. 

3. Voer de opdracht **aan** tot de macht van de Bluetooth-controller. Hier ziet u uitvoer die vergelijkbaar is met:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Voer de opdracht **scannen op** om te zoeken naar Bluetooth-apparaten in de shell interactieve bluetooth. Hier ziet u uitvoer die vergelijkbaar is met:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Het SensorTag-apparaat detecteerbaar maken door op de kleine knop (de groene LED knippert). De Edison moet het apparaat SensorTag ontdekken:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    In dit voorbeeld ziet u dat het MAC-adres van het apparaat SensorTag **A0:E6:F8:B5:F6:00 is**.

6. Schakel scannen door de opdracht **scan uit** te voeren.
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Verbinding maken met het SensorTag-apparaat met behulp van het MAC-adres door te geven **verbinding \<MAC-adres >**. Houd er rekening mee dat de volgende voorbeelduitvoer wordt afgekort:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Opmerking: De GATT-kenmerken van het apparaat opnieuw met de opdracht **lijst kenmerken** kunt aanbieden.

8. U kunt nu loskoppelen van het apparaat met de opdracht **verbreken** en vervolgens af te sluiten van de Bluetooth-shell met de opdracht **Afsluiten** :
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

U kunt nu het monster Bel Gateway uitvoeren op uw apparaat Edison.

## <a name="run-the-ble-gateway-sample"></a>Het monster Bel Gateway uitvoeren

Het monster van de bel op de Edison uitgevoerd, moet u drie taken uitvoeren:

- Twee monster apparaten in uw IoT Hub configureren.
- De SDK IoT-Gateway maken op uw apparaat Edison.
- Configureren en uitvoeren van het monster van de bel op het Edison apparaat.

Op het moment van schrijven ondersteunt de Gateway IoT SDK alleen gateways die via Bel modules onder Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Twee monster apparaten configureren in de IoT Hub

- [Maak een hub IoT] [ lnk-create-hub] in uw abonnement Azure, moet u de naam van de hub en deze procedure te voltooien. Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.
- Een zogeheten **SensorTag_01** op uw hub IoT toevoegen en maak een notitie van de sleutel-id en het apparaat. U kunt het [apparaat Explorer of iothub explorer] [ lnk-explorer-tools] hulpmiddelen voor het toevoegen van dit apparaat aan de IoT-hub die u in de vorige stap hebt gemaakt en voor het ophalen van de sleutel. U koppelt dit apparaat naar het apparaat SensorTag bij het configureren van de gateway.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>De SDK IoT-Gateway maken op uw apparaat Edison

De versie van **git** op de Edsion ondersteunt geen submodules. Downloaden van de volledige bron voor IoT Gateway SDK op de Edison hebt u twee mogelijkheden:

- Optie #1: De [Azure IoT Gateway SDK] klonen[ lnk-sdk] bibliotheek op uw Edison en de opslagplaats voor elke submodule vervolgens handmatig te klonen.
- Optie #2: De [Azure IoT Gateway SDK] klonen[ lnk-sdk] bibliotheek op een desktop-apparaat waar **git** submodules ondersteunt en kopieer de volledige bibliotheek met submodules naar de Edison.

Als u de optie #2, gebruik de volgende opdrachten in de **git** klonen van de Gateway IoT SDK en alle bijbehorende submodules:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

U moet vervolgens de hele lokale opslagplaats in een enkel archiefbestand zip voordat u deze naar de Edison kopiëren. U kunt een hulpprogramma zoals **pscp** die deel uitmaakt van de **stopverf** het bestand kopiëren naar de Edison. Bijvoorbeeld:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Wanneer u een volledige kopie van de opslagplaats IoT Gateway SDK op de Edison hebt, kunt u met de volgende opdracht uit de map met de SDK maken:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Configureren en uitvoeren van het monster Bel op uw apparaat Edison

Bootstrap en het monster wordt uitgevoerd, moet u elke module die deelneemt in de gateway configureren. Deze configuratie wordt geleverd in een JSON-bestand en moet u alle vijf deelnemende modules configureren. Er is een voorbeeld JSON-bestand in de opslagplaats genoemd **gateway_sample.json** die u als uitgangspunt gebruiken kunt voor het bouwen van uw eigen configuratiebestand wordt verstrekt. Dit bestand is in de map **samples/ble_gateway_hl/src** in de lokale kopie van de opslagplaats IoT Gateway SDK.

In de volgende secties wordt beschreven hoe u dit configuratiebestand voor het monster Bel bewerken en wordt ervan uitgegaan dat de opslagplaats IoT Gateway SDK in de **/home/root/azure-iot-gateway-sdk /** map op het apparaat Edison. Als de bibliotheek elders is, moet u de paden overeenkomstig aanpassen:

#### <a name="logger-configuration"></a>Configuratie van de Logger

Ervan uitgaande dat de gateway opslagplaats bevindt zich in de map **/home/root/azure-iot-gateway-sdk /**, de module van het logboek als volgt configureren:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Bel module configuratie

De voorbeeld-configuratie voor het apparaat van de bel wordt ervan uitgegaan dat een Texas Instruments SensorTag apparaat. Een standaard Bel-apparaat werken kunt, zoals een randapparaat GATT moet werken, maar u moet de karakteristieke GATT-id's en gegevens (voor instructies schrijven) bijwerken. Het MAC-adres van het SensorTag-apparaat toevoegen: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>Module IoT Hub

De naam van uw Hub-IoT toevoegen. De Achtervoegselwaarde is gewoonlijk **azure-devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Identity module configuratie

Het MAC-adres van het apparaat SensorTag en de Id van het apparaat en de sleutel van het **SensorTag_01** -apparaat dat u hebt toegevoegd aan uw IoT Hub toevoegen:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Bel printerconfiguratie module

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Configuratie van de routering

De volgende configuratie zorgt voor het volgende:
- De module **Logger** ontvangt en alle berichten.
- De module **SensorTag** verzendt berichten naar de **toewijzing** en de **Printer Bel** modules.
- De module **toewijzing** verzendt berichten naar de module **IoTHub** toegezonden op uw IoT Hub.
- De module **IoTHub** verzendt berichten naar de module van de **toewijzing** .
- De module **toewijzing** verzendt berichten naar de module **SensorTag** .

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Het voorbeeld alleen uitvoeren kunt u de binaire doorgeven van het pad naar het configuratiebestand JSON **ble_gateway_hl** uitvoeren. Als u het bestand **gateway_sample.json** hebt gebruikt, wordt de opdracht uit te voeren als volgt weergegeven:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Wellicht moet u op de kleine knop drukken op het SensorTag-apparaat detecteerbaar voordat u het voorbeeld uitvoert.

Als u het voorbeeld uitvoert, kunt u het [apparaat Explorer of iothub explorer] [ lnk-explorer-tools] hulpprogramma voor het controleren van de berichten die de gateway van het SensorTag apparaat doorstuurt.

## <a name="send-cloud-to-device-messages"></a>Wolk naar apparaat verzenden

De bel-module ondersteunt ook de instructies van de Hub Azure IoT verzenden naar het apparaat. U kunt de [Explorer Azure IoT Hub-apparaat](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) of de [IoT Hub Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) JSON berichten verzenden die de bel gateway-module wordt doorgegeven op het apparaat Bel. Bijvoorbeeld als u het apparaat Texas Instruments SensorTag vervolgens u kunt de volgende JSON berichten verzenden naar het apparaat van IoT Hub.

- Alle LED's en het alarm opnieuw instellen (uitschakelen)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- I/O configureren als 'extern'

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- De rode LED inschakelen

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- De groene LED inschakelen

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Inschakelen van het alarm

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Het standaardgedrag voor een apparaat met behulp van het HTTP-protocol verbinding maken met IoT Hub is te controleren voor een nieuwe opdracht om de 25 minuten. Als u meerdere afzonderlijke opdrachten verzendt moet u wachten 25 minuten voor het apparaat voor het ontvangen van elke opdracht.

> [AZURE.NOTE] De gateway wordt ook gecontroleerd voor nieuwe opdrachten als het wordt gestart, zodat u kunt zorgen dat een opdracht door te stoppen en starten van de gateway worden verwerkt.

## <a name="next-steps"></a>Volgende stappen

Als u wilt experimenteren met enkele codevoorbeelden en krijgen een uitgebreidere kennis van de SDK IoT-Gateway, gaat u naar de volgende zelfstudies voor ontwikkelaars en bronnen:

- [Azure IoT Gateway SDK][lnk-sdk]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
