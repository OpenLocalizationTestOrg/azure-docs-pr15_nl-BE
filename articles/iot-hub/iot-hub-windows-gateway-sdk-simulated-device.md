<properties
    pageTitle="Simuleren van een apparaat met de SDK IoT Gateway | Microsoft Azure"
    description="Azure IoT Gateway SDK scenario verzendende telemetrie van een gesimuleerde apparaat met behulp van de SDK Azure IoT Gateway illustreren met Windows."
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT Gateway SDK (beta) – verzenden apparaat-wolk-berichten met een gesimuleerde apparaat met behulp van Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Bouwen en uitvoeren van het monster

Voordat u begint, moet u:

- [Instellen van uw ontwikkelomgeving] [ lnk-setupdevbox] voor de samenwerking met de SDK van Windows.
- [Maak een hub IoT] [ lnk-create-hub] in uw abonnement Azure, moet u de naam van de hub en deze procedure te voltooien. Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.
- Twee apparaten toevoegen aan uw hub IoT en noteer de id's en apparaat-sleutels. U kunt het [apparaat Explorer of iothub explorer] [ lnk-explorer-tools] gereedschap om uw apparaten toevoegen aan de IoT-hub die u in de vorige stap hebt gemaakt en de sleutels op te halen.

Het monster maken:

1. Open **de opdrachtprompt Developer voor VS2015** vanaf de opdrachtprompt.
2. Ga naar de hoofdmap in het lokale exemplaar van de opslagplaats **azure iot-gateway sdk** .
3. Voer de **Extra\\build.cmd** script. Dit script maakt een bestand Visual Studio-oplossing, de oplossing is gebaseerd en de tests uitgevoerd. De Visual Studio-oplossing vindt u in de map **maken** in het lokale exemplaar van de opslagplaats **azure iot-gateway sdk** .

Het monster wordt uitgevoerd:

Open het bestand in een teksteditor, **monsters\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** in het lokale exemplaar van de opslagplaats **azure iot-gateway sdk** . Dit bestand wordt de modules in de steekproef gateway geconfigureerd:

- De module **IoTHub** verbindt met de hub IoT. U moet configureren om gegevens te verzenden naar uw hub IoT. Met name de waarde van **IoTHubName** ingesteld op de naam van uw hub-IoT en stel de waarde van de **IoTHubSuffix** op **azure-devices.net**. De waarde van het **Transport** wordt ingesteld op een van: 'HTTP', 'AMQP' of 'MQTT'. Houd er rekening mee dat op dit moment alleen 'HTTP' een TCP-verbinding voor alle berichten van het apparaat delen. Als u de waarde instellen op 'AMQP' of 'MQTT', blijft de gateway een afzonderlijke TCP-verbinding met IoT Hub voor elk apparaat.
- De module **toewijzing** wordt de MAC-adressen van de gesimuleerde apparaten aan uw IoT Hub apparaat-id's toegewezen. Zorg ervoor dat **deviceId** waarden overeenkomen met de id's van de twee apparaten die u hebt toegevoegd aan uw hub IoT en dat de waarden van de **deviceKey** bevatten de sleutels van de twee apparaten.
- De modules **BLE1** en **BLE2** zijn de gesimuleerde apparaten. U ziet hoe het MAC-adres overeenkomen met die in de module voor de **toewijzing** .
- De module **Logger** vastgelegd uw gateway-activiteit in een bestand.
- De onderstaande waarden voor het **pad van de module** wordt ervan uitgegaan dat u de bibliotheek IoT Gateway SDK naar de hoofdmap van station **C:** gekloond. Als u deze naar een andere locatie hebt gedownload, moet u de waarden voor **pad module** aanpassen.
- De matrix van de **koppelingen** onder aan het bestand JSON verbindt de modules **BLE1** en **BLE2** met de module **toewijzen** en de **toewijzing van** de module **IoTHub** module. Ook zorgt ervoor dat alle berichten die door de module van het **logboek** worden geregistreerd.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}
```

Sla eventuele wijzigingen in het configuratiebestand.

Het monster wordt uitgevoerd:

1. Bij een opdrachtprompt, Ga naar de hoofdmap van uw lokale kopie van de opslagplaats **azure iot-gateway sdk** .
2. Voer de volgende opdracht:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. U kunt het [apparaat Explorer of iothub explorer] [ lnk-explorer-tools] hulpprogramma voor het controleren van de berichten die IoT-hub van de gateway ontvangt.


## <a name="next-steps"></a>Volgende stappen

Als u wilt experimenteren met enkele codevoorbeelden en krijgen een uitgebreidere kennis van de SDK IoT-Gateway, gaat u naar de volgende zelfstudies voor ontwikkelaars en bronnen:

- [Apparaat-wolk-berichten verzenden vanuit een echt apparaat met de SDK IoT Gateway][lnk-physical-device]
- [Azure IoT Gateway SDK][lnk-gateway-sdk]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 