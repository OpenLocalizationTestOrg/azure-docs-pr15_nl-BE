<properties
    pageTitle="Gebruik dynamische telemetrie | Microsoft Azure"
    description="Volg deze zelfstudie voor meer informatie over het gebruik van dynamische telemetrie met de externe controle vooraf geconfigureerde oplossing."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Gebruik dynamische telemetrie van de vooraf geconfigureerde oplossing voor externe controle

## <a name="introduction"></a>Inleiding

Dynamische telemetrie kunt u eventuele telemetrie verzonden naar de externe controle vooraf geconfigureerde oplossing te visualiseren. De gesimuleerde apparaten die u met de vooraf geconfigureerde oplossing implementeren verzenden telemetrie van temperatuur en vochtigheid, die u op het dashboard kunt visualiseren. Als u de bestaande gesimuleerde apparaten aanpassen, nieuwe gesimuleerde apparaten maken of fysieke apparaten op de vooraf geconfigureerde oplossing aansluiten kunt u andere telemetrie waarden zoals de externe temperatuur, windsnelheid of RPM verzenden. U kunt vervolgens visualiseren dit extra telemetrie op het dashboard.

In deze zelfstudie wordt een eenvoudige Node.js gesimuleerde apparaat u eenvoudig aanpassen kunt om te experimenteren met dynamische telemetrie.

Deze zelfstudie hebt u nodig:

- Een abonnement op Azure active. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure][lnk_free_trial].
- [Node.js] [ lnk-node] versie 0.12.x of hoger.

U kunt deze zelfstudie op elk besturingssysteem, zoals Windows of Linux, waar u Node.js kunt installeren.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Configureer het gesimuleerde apparaat Node.js

1. Op de externe controle dashboard op **+ een apparaat toevoegen** en vervolgens een aangepast apparaat toevoegen. Maak een notitie van de IoT Hub hostname, apparaat-id en apparaatsleutel. U moet deze verderop in deze handleiding bij het voorbereiden van de remote_monitoring.js device-clienttoepassing.

2. Ervoor te zorgen dat Node.js versie 0.12.x of hoger is geïnstalleerd op de ontwikkelcomputer. Voer `node --version` achter de opdrachtprompt of in een shell de versie controleren. Zie voor meer informatie over het gebruik van een package manager Node.js installeren onder Linux [Node.js installeren via package manager][node-linux].

3. Wanneer u Node.js hebt geïnstalleerd, de nieuwste versie van de [azure-iot-SDK's] klonen[ lnk-github-repo] opslagplaats op de ontwikkelcomputer. Gebruik altijd de **master** branch voor de meest recente versie van de bibliotheken en de voorbeelden.

4. In uw lokale exemplaar van de [azure-iot-SDK's] [ lnk-github-repo] opslagplaats, de volgende twee bestanden kopiëren van de map samples/apparaat/knooppunt naar een lege map op de ontwikkelcomputer:

  - Packages.JSON
  - remote_monitoring.js

5. Open het bestand remote_monitoring.js en zoek naar de volgende definitie van de variabele:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. **[IoT Hub apparaat verbindingsreeks]** vervangen door de verbindingsreeks voor het apparaat. Gebruik de waarden voor IoT Hub hostname, apparaat-id en sleutel van het apparaat dat u een notitie van in stap 1 gemaakt. Een verbindingsreeks apparaat heeft de volgende indeling:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Als uw apparaat-id is **mydevice**uw hostnaam IoT Hub **contoso** is ziet de verbindingsreeks er het volgende:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Sla het bestand. Voer de volgende opdrachten in MS-DOS-prompt in de map waarin deze bestanden de nodige pakketten installeren en voer vervolgens de voorbeeldtoepassing of shell:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Houd rekening met dynamische telemetrie in actie

Het dashboard ziet u de temperatuur en vochtigheid telemetrie van de bestaande gesimuleerde apparaten:

![De standaarddashboard][image1]

Als u het Node.js gesimuleerde apparaat dat u in de vorige sectie hebt uitgevoerd, ziet u de temperatuur, vochtigheid en temperatuur externe telemetrie:

![Externe temperatuur toevoegen aan het dashboard][image2]

De remote monitoring oplossing automatisch detecteert het type aanvullende externe temperatuur Telemetrie en toegevoegd aan de grafiek op het dashboard.

## <a name="add-a-telemetry-type"></a>Een type telemetrie toevoegen

De volgende stap is het vervangen van de telemetrie gegenereerd door het gesimuleerde apparaat Node.js met een nieuwe set van waarden:

1. Stop het gesimuleerde apparaat Node.js door **Ctrl + C** in shell of de opdrachtprompt te typen.

2. In het bestand remote_monitoring.js ziet u de waarden van de basisgegevens voor de bestaande temperatuur, vochtigheid en temperatuur externe telemetrie. Een waarde van de basisgegevens voor **rpm** als volgt toevoegen:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Het gesimuleerde apparaat Node.js gebruikt de functie **generateRandomIncrement** in het bestand remote_monitoring.js een willekeurige reeks toevoegen aan de waarden van de basis. De **rpm** -waarde een willekeurige door het toevoegen van een regel code na de bestaande randomizations als volgt:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Het nieuwe rpm-waarde toevoegen aan de JSON-nettolading die het apparaat met IoT Hub verzendt:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. De gesimuleerde Node.js-apparaat met de volgende opdracht uitvoeren:

    ```
    node remote_monitoring.js
    ```

6. Houd rekening met het nieuwe RPM telemetrie type die worden weergegeven in de grafiek in het dashboard:

![RPM toevoegen aan het dashboard][image3]

> [AZURE.NOTE] Wellicht moet u uitschakelen en schakel het apparaat Node.js op de pagina **apparaten** in het dashboard om de wijziging direct bekijken.

## <a name="customize-the-dashboard-display"></a>De dashboardweergave aanpassen

Het **Apparaat Info** bericht kan bevatten metagegevens over de telemetrie die naar IoT Hub het apparaat kunt verzenden. Deze metagegevens kunt opgeven welke telemetrie die het apparaat verzonden. Wijzig de waarde **deviceMetaData** in het bestand remote_monitoring.js op te nemen, een **telemetrie** definitie volgens de definitie van de **opdrachten** . Het volgende codefragment toont de definitie van **opdrachten** (Zorg ervoor dat u toevoegt een `,` na de definitie van de **opdrachten** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] De remote monitoring oplossing gebruikt een niet-hoofdlettergevoelige overeenkomst te vergelijken met de gegevens in de stream telemetrie de metagegevensdefinitie van de.

Een definitie **telemetrie** toe te voegen zoals in het voorgaande codefragment wordt niet gewijzigd voor het gedrag van het dashboard. De metagegevens kan echter ook een kenmerk **DisplayName** voor het aanpassen van de weergave in het dashboard. Update voor de definitie van **telemetrie** metagegevens zoals in het volgende fragment:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

De volgende schermafbeelding ziet u hoe deze wijziging op het dashboard de legenda van de grafiek wijzigt:

![Legenda van de grafiek aanpassen][image4]

> [AZURE.NOTE] Wellicht moet u uitschakelen en schakel het apparaat Node.js op de pagina **apparaten** in het dashboard om de wijziging direct bekijken.

## <a name="filter-the-telemetry-types"></a>De typen telemetrie filteren

De grafiek op het dashboard wordt standaard elke gegevensreeks in de telemetrie-stroom. Onderdrukt de weergave van specifieke telemetrie typen in de grafiek kunt u de **Apparaat-Info** -metagegevens. 

Als u de grafiek weergeven van alleen temperatuur en vochtigheid telemetrie, **ExternalTemperature** uit weglaten de metagegevens van de **Telemetrie** **- Info-apparaat** als volgt:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

De **Outdoor temperatuur** weergegeven niet meer in de grafiek:

![De telemetrie op het dashboard filteren][image5]

Deze wijziging is alleen van invloed op de grafiekweergave. De waarden van de **ExternalTemperature** zijn nog steeds opgeslagen en beschikbaar gesteld voor de back-end verwerking.

> [AZURE.NOTE] Wellicht moet u uitschakelen en schakel het apparaat Node.js op de pagina **apparaten** in het dashboard om de wijziging direct bekijken.

## <a name="handle-errors"></a>Fouten afhandelen

Voor een gegevensstroom die wordt weergegeven in de grafiek, het **Type** in de metagegevens van het **Apparaat Info** moet overeenkomen met het gegevenstype van de telemetrie waarden. Bijvoorbeeld, als de metagegevens aangeeft dat het **Type** vochtigheid **int** en **dubbele** is gevonden in de stroom telemetrie wordt vervolgens de telemetrie vochtigheid niet weergegeven in de grafiek. De **vochtigheid** -waarden worden echter nog steeds opgeslagen en beschikbaar gesteld voor de back-end verwerking.

## <a name="next-steps"></a>Volgende stappen

U hebt gezien hoe u met dynamische telemetrie, voor meer informatie over het gebruik van gegevens van een apparaat in de vooraf geconfigureerde oplossingen: [informatie-metagegevens in het externe apparaat controle vooraf geconfigureerde oplossing][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
