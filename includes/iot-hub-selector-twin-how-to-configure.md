> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Inleiding

In [aan de slag met IoT Hub twins][lnk-twin-tutorial], u hebt geleerd hoe apparaat meta-gegevens van uw oplossing voor back-end met *tags*, rapport apparaat voorwaarden uit een apparaat app met *gerapporteerde eigenschappen*, instellen en opvragen van deze gegevens met behulp van een SQL-achtige taal.

In deze zelfstudie leert u hoe u met de van de twin *gewenste eigenschappen* in combinatie met de *eigenschappen vermeld*, apps apparaat op afstand te configureren. Meer in het bijzonder in deze zelfstudie wordt weergegeven hoe de twin gemeld en gewenste eigenschappen een configuratie met meerdere stappen van een toepassingsinstelling apparaat inschakelen en de zichtbaarheid van de back-end oplossing van de status van deze bewerking bieden op alle apparaten.

Deze zelfstudie volgt op een hoog niveau van het *patroon van de gewenste status* voor Apparaatbeheer. De essentie van dit patroon is dat de oplossing voor back-end geeft u de gewenste status voor de beheerde apparaten, in plaats van specifieke opdrachten verzenden. Hiermee wordt het apparaat voor het maken van de beste manier om het bereiken van de gewenste status (zeer belangrijk in scenario's waarin specifieke voorwaarden invloed hebben op het onmiddellijk uit te voeren specifieke opdrachten voor IoT), geplaatst tijdens voortdurend melden aan de back-end van de huidige toestand en mogelijke fouten. Het patroon van de gewenste status is noodzakelijk hulpmiddel voor het beheer van grote verzamelingen van apparaten, zoals zorgt ervoor dat de back-end volledig inzicht in de status van het configuratieproces hebt op alle apparaten.
U vindt meer informatie over de rol van het patroon van de gewenste status in Apparaatbeheer in [Apparaatbeheer overzicht Azure IoT Hub][lnk-dm-overview].

> [AZURE.NOTE] In scenario's waarin apparaten worden beheerd op een meer interactieve manier (op een fan van een gebruiker beheerde toepassing inschakelen), kunt u overwegen [methoden wolk naar apparaat][lnk-methods].

In deze zelfstudie, de wijzigingen in de toepassing back-end de telemetrie-configuratie van een doelapparaat en, als gevolg van die het apparaat app volgt een proces met meerdere stappen om een configuratie-update (bijvoorbeeld vereisen een herstart van de module software), die in deze zelfstudie wordt gesimuleerd met een eenvoudige vertraging).

De back-end slaat de configuratie in de gewenste eigenschappen van het apparaat-twin op de volgende manier:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Aangezien configuraties complexe objecten zijn kunnen, worden ze meestal unieke id's toegewezen (hashes of [GUID's][lnk-guid]) voor het vereenvoudigen van hun vergelijkingen.

De app van het apparaat rapporten de huidige configuratie de gewenste eigenschap **telemetryConfig** in de eigenschappen van de gerapporteerde mirroring:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

U ziet hoe de gerapporteerde **telemetryConfig** een extra eigenschap **status**, gebruikt voor het rapporteren van de status van het configuratieproces van de update.

Wanneer een nieuwe gewenste configuratie is ontvangen, meldt de app van het apparaat een configuratie in behandeling door de gegevens te wijzigen:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Klik vervolgens op een later tijdstip verslag het apparaat app het succes van het mislukken van deze bewerking door de bovenstaande eigenschap bij te werken.
U ziet hoe de back-end kunnen op elk moment de status van het configuratieproces opvragen op alle apparaten.

Deze zelfstudie toont u hoe te:

- Maak een gesimuleerde apparaat dat ontvangt updates van de configuratie van de back-end en meerdere updates als *gerapporteerde eigenschappen* rapporten over het configuratieproces van de update.
- Maak een back-end-app die de gewenste configuratie van een apparaat wordt bijgewerkt en vervolgens een query het configuratieproces van de update.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier