<properties
   pageTitle="Verbinding maken met een apparaat met behulp van C op mbed | Microsoft Azure"
   description="Wordt beschreven hoe u een apparaat aansluit op de Azure IoT Suite vooraf geconfigureerde remote monitoring oplossing met behulp van een toepassing die is geschreven in C uitgevoerd op een apparaat mbed."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Verbind het apparaat met de externe controle vooraf geconfigureerde oplossing (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Bouwen en uitvoeren van de monsteroplossing C

De volgende instructies worden de stappen beschreven voor het aansluiten van een [Geschikte mbed Freescale FRDM-K64F] [ lnk-mbed-home] apparaat naar de externe monitoring oplossing.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>De mbed-apparaat aansluit op uw computer netwerk- en

1. De mbed-apparaat aansluit op uw netwerk via een Ethernet-kabel. Deze stap is noodzakelijk omdat de voorbeeldtoepassing is internettoegang vereist.

2. Zie [Aan de slag met mbed] [ lnk-mbed-getstarted] aan de mbed-apparaat aansluit op uw desktop-PC.

3. Als uw desktop-PC is waarop Windows wordt uitgevoerd, de [PC-configuratie] Zie[ lnk-mbed-pcconnect] voor het configureren van seriële poort toegang tot het apparaat mbed.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Mbed-projecten maken en importeren van de voorbeeldcode

1. Ga naar de mbed.org- [ontwikkelaar site](https://developer.mbed.org/)in de webbrowser. Als u nog niet hebt aangemeld, ziet u een optie voor het maken van een account (het is gratis). Anders kunt u inloggen met uw referenties. Klik vervolgens op **Compiler** in de rechterbovenhoek van de pagina. Deze actie wordt de interface van de *werkruimte* .

2. Controleer of de hardwareplatform dat u gebruikt, wordt weergegeven in de rechterbovenhoek van het venster of klik op het pictogram in de rechter bovenhoek te selecteren van het hardwareplatform.

3. Klik op **importeren** in het hoofdmenu. Klik vervolgens op importeren vanuit een URL-koppeling naast het logo van de hele wereld mbed **Klik hier** .

    ![][6]

4. Geef de koppeling voor de steekproef code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ in het pop-upvenster vervolgens op **importeren**.

    ![][7]

5. U ziet in het venster mbed compiler dat verschillende bibliotheken ook dit project importeren importeert. Sommige zijn geleverd en beheerd door het team Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), terwijl andere bibliotheken van derden beschikbaar zijn in de catalogus van de bibliotheken mbed.

    ![][8]

6. Open het bestand remote_monitoring\remote_monitoring.c en zoek de volgende code in het bestand:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Vervangen [apparaat-Id] en [apparaat Key], met uw apparaatgegevens de steekproef programma verbinding maken met uw hub IoT inschakelen. De Hub IoT hostnaam gebruiken ter vervanging van de [naam IoTHub] en [IoTHub achtervoegsel, dat wil zeggen azure devices.net] tijdelijke aanduidingen. Als de Hub IoT Hostname **contoso.azure devices.net**, is **contoso** bijvoorbeeld de **hubName** en alles na de **hubSuffix**is:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Doorloop de code

Als u geïnteresseerd bent in hoe het programma werkt, wordt in deze sectie wordt beschreven van bepaalde belangrijke onderdelen van de voorbeeldcode. Als u wilt dat alleen de code uit te voeren, gaat u verder met het [bouwen en uitvoeren van het programma](#buildandrun).

#### <a name="defining-the-model"></a>Het model definiëren

In dit voorbeeld gebruikt de [serializer] [ lnk-serializer] bibliotheek voor het definiëren van een model dat de berichten in het apparaat kunt IoT Hub verzenden en ontvangen van IoT Hub bevat. In dit voorbeeld wordt de **Contoso** -naamruimte gedefinieerd model **thermostaat** die de **temperatuur**, **ExternalTemperature**en **vochtigheid** telemetrie gegevens en metagegevens, zoals de apparaat-id, apparaateigenschappen en de opdrachten die het apparaat reageert op:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Met betrekking tot de modeldefinitie, zijn de definities voor de opdrachten voor **SetTemperature** en **SetHumidity** die het apparaat reageert op:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Verbinding maken met het model in de bibliotheek

De functies **sendMessage** - en **IoTHubMessage** zijn generieke code voor telemetrie van het apparaat te verzenden en berichten van IoT Hub verbinding te maken met de opdracht-handlers.

#### <a name="the-remotemonitoringrun-function"></a>De functie remote_monitoring_run

De **belangrijkste** functie van het programma roept de functie **remote_monitoring_run** wanneer de toepassing wordt gestart voor het gedrag van het apparaat als een IoT Hub device-client uitvoeren. Deze functie **remote_monitoring_run** bestaat voornamelijk uit geneste combinaties van functies:

- **Platform\_init** en **platform\_deinit** platformspecifieke initialisatie en shutdown bewerkingen uitvoeren.
- **serializer\_init** en **serialisatiefunctie\_deinit** initialiseren en los de serializer-bibliotheek te initialiseren.
- **IoTHubClient\_maken** en **IoTHubClient\_vernietigen** ingang van een client, **iotHubClientHandle**, met behulp van de inloggegevens voor het aansluiten op uw hub-IoT maken.

In het hoofdgedeelte van de functie **remote_monitoring_run** wordt de volgende bewerkingen met de greep van **iotHubClientHandle** :

- Maakt een exemplaar van het model van de thermostaat Contoso en stelt de retouraanroepen bericht voor de twee opdrachten.
- Informatie over het apparaat zelf, inclusief de opdrachten ondersteunt, op je IoT-hub met behulp van de bibliotheek serializer verzendt. Als de hub dit bericht ontvangt, verandert de status van het apparaat in het dashboard van **in behandeling** in **actief**.
- Start een lus **terwijl** die temperatuur, externe temperatuur en vochtigheid waarden naar IoT Hub elke seconde verzendt.

Ter referentie is hier een voorbeeld **DeviceInfo** bericht aan IoT Hub bij het opstarten:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Dit is een monster **telemetrie** bericht aan IoT Hub voor een verwijzing naar:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Hier volgt een voorbeeld van een **die opdracht** van IoT Hub ontvangen voor een verwijzing naar:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Bouwen en uitvoeren van het programma

1. Klik op **compileren** voor het opbouwen van het programma. U kunt veilig negeren van waarschuwingen, maar als de build fouten genereert oplossen voordat u verdergaat.

2. Als het opbouwen voltooid is, wordt de website mbed compiler genereert een BIN-bestand met de naam van het project en ze gedownload naar uw lokale computer. BIN-bestand naar het apparaat kopiëren. Opslaan van het BIN-bestand naar het apparaat wordt het apparaat opnieuw starten en uitvoeren van het programma het .bin-bestand bevat. U kunt het programma handmatig op elk gewenst moment opnieuw door te drukken op de reset-knop op het apparaat mbed.

3. Verbinding maken met het apparaat met behulp van een SSH-clienttoepassing, zoals stopverf. Hier ziet u de seriële poort die het apparaat wordt gebruikt door Windows Apparaatbeheer controleren.

    ![][11]

4. In stopverf, klikt u op het type van de **seriële** verbinding. Normaal gesproken het apparaat verbinding maakt met 9600 baud, 9600 dus voer in het vak **snelheid** . Klik vervolgens op **openen**.

5. Het programma wordt uitgevoerd. Mogelijk moet u de kaart opnieuw instellen (druk op CTRL + Break of druk op de knop Beginwaarden van de Raad) als het programma niet automatisch wordt gestart wanneer u verbinding maakt.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
