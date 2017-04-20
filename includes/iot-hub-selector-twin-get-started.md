> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Inleiding

Twins apparaat zijn JSON-documenten waarin apparaat staat informatie (metagegevens, configuraties en omstandigheden). IoT Hub aanhoudt een twin apparaat voor elk apparaat dat u op de IoT aansluit Hub.

Twins apparaat te gebruiken:

* Apparaat meta-gegevens uit uw back-end opslag.
* Informatie over de huidige status zoals beschikbare mogelijkheden en voorwaarden (bijvoorbeeld de verbindingsmethode die wordt gebruikt) verslag van uw apparaat app.
* De status van werkstromen langdurige (zoals updates van firmware en configuratiebestanden) synchroniseren tussen het apparaat app en back-end.
* Query uitvoeren op uw apparaat meta-gegevens, de configuratie of de staat.

> [AZURE.NOTE] Twins apparaat zijn ontworpen voor synchronisatie en voor het opvragen van configuraties en voorwaarden. [Apparaat-wolk - berichten] gebruikt[ lnk-d2c] voor reeksen van gebeurtenissen (zoals telemetrie gegevensstreams sensor op basis van tijd) voorzien van een tijdstempel en [methoden van de wolk naar apparaat] [ lnk-methods] voor interactieve besturing van apparaten, zoals een ventilator van een gebruiker beheerde toepassing inschakelen.

Twins apparaat zijn opgeslagen in een hub IoT en bevatten:

* *tags*, meta-gegevens alleen toegankelijk zijn voor de back-end;
* *gewenste eigenschappen*JSON-objecten kunnen worden gewijzigd door de back-end en waarneembaar door de app van het apparaat; en
* *gerapporteerde eigenschappen*JSON-objecten kunnen worden gewijzigd door de toepassing van het apparaat en kan worden gelezen door de back end. Labels en eigenschappen geen matrices bevatten, maar de objecten kunnen worden genest.

![][img-twin]

De app back-end kan bovendien apparaat twins op basis van de bovenstaande gegevens opvragen.
Verwijzen naar [Understand apparaat twins] [ lnk-twins] voor meer informatie over het apparaat twins en de [querytaal IoT Hub] [ lnk-query] voor het uitvoeren van query's.

> [AZURE.NOTE] Op dit moment zijn apparaat twins alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

Deze zelfstudie toont u hoe te:

- Maak een back-end-app die *codes* worden toegevoegd aan een apparaat twin en een gesimuleerde apparaat dat de channel connectiviteit rapporten als een *eigenschap gerapporteerd* over de twin apparaat.
- Query apparaten uit uw back-end-app op de tags en eigenschappen die eerder zijn gemaakt met behulp van filters.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md