> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

In dit scenario van het [gesimuleerde apparaat Cloud uploaden voorbeeld] laat zien hoe met de [SDK van Azure IoT Gateway] [ lnk-sdk] apparaat naar cloud telemetrie verzenden naar IoT Hub van gesimuleerde apparaten.

Dit scenario omvat:

1. **Architectuur**: belangrijke architectonische informatie over de gesimuleerde apparaat Cloud uploaden steekproef.

2. **Bouwen en uitvoeren**: de vereiste stappen voor het maken en uitvoeren van het monster.

## <a name="architecture"></a>Architectuur

Het gesimuleerde apparaat Cloud uploaden-voorbeeld ziet u hoe een gateway die telemetrie van gesimuleerde apparaten aan een hub IoT maken met de SDK. De gesimuleerde apparaten kunnen niet rechtstreeks aansluiten op IoT Hub omdat:

- De apparaten gebruik niet een communicatieprotocol IoT Hub te begrijpen.
- De apparaten zijn niet slim genoeg om de identiteit die wordt toegewezen door de IoT Hub onthouden.

De gateway lost deze problemen voor de gesimuleerde apparaten op de volgende manieren:

- De gateway begrijpt het protocol dat wordt gebruikt door de gesimuleerde apparaten apparaat naar cloud telemetrie ontvangt van de apparaten en die berichten doorstuurt naar IoT Hub met een protocol dat door de hub te begrijpen.
- De gateway IoT Hub identiteiten opgeslagen ten behoeve van de gesimuleerde apparaten en fungeert als proxy wanneer de gesimuleerde apparaten berichten naar IoT Hub verzendt.

Het volgende diagram toont de belangrijkste onderdelen van het monster, met inbegrip van de gateway-modules:

![][1]


> [AZURE.NOTE] De modules niet door berichten naar elkaar. De modules publiceren berichten naar een interne broker die zorgt voor de berichten met de andere modules met een abonnement mechanisme zoals in het onderstaande diagram. Zie voor meer informatie, [aan de slag met de SDK IoT Gateway][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Protocol ingestie module

Deze module is het beginpunt voor het ophalen van gegevens van apparaten, via de gateway en in de cloud. De module worden in de steekproef vier taken uitgevoerd:

1.  Gesimuleerde temperatuur gegevens wordt gemaakt.
    
    Opmerking: als u echte apparaten gebruikt, de module zou gegevens lezen van de fysieke apparaten.

2.  De gesimuleerde temperatuur gegevens geplaatst in de inhoud van een bericht.

3.  Een eigenschap met een valse MAC-adres wordt toegevoegd aan het bericht met de gegevens van de gesimuleerde temperatuur.

4.  Het maakt het bericht naar de volgende module beschikbaar in de keten.

> [AZURE.NOTE] De module genoemd **Protocol X opname** in het bovenstaande diagram wordt **gesimuleerd apparaat** genoemd in de broncode.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; module-ID IoT-Hub

In deze module scant op berichten met een eigenschap met het MAC-adres toegevoegd door de module protocol ingestie van het gesimuleerde apparaat. Als de module een dergelijke eigenschap vindt, wordt een andere eigenschap met een IoT Hub apparaatsleutel toegevoegd aan het bericht en wordt het bericht naar de volgende module beschikbaar in de keten. Dit is hoe het monster worden gekoppeld aan IoT Hub apparaat identiteiten gesimuleerde apparaten. De ontwikkelaar stelt u de toewijzing tussen de IoT Hub identiteiten en MAC-adressen handmatig als onderdeel van de module configuratie. 

> [AZURE.NOTE]  In dit voorbeeld wordt een MAC-adres wordt gebruikt als een uniek apparaat-id en verbindt deze met een IoT Hub apparaat-id. U kunt echter uw eigen module die gebruikmaakt van een andere unieke id schrijven. Bijvoorbeeld, wellicht apparaten met unieke volgnummers of telemetriegegevens die een unieke naam opgeslagen die u gebruiken kunt om te bepalen van de identiteit van de IoT Hub apparaat heeft.

### <a name="iot-hub-communication-module"></a>Communicatiemodule IoT Hub

Deze module duurt berichten met een IoT Hub apparaat-id die wordt toegewezen door de vorige module en de inhoud van het bericht verzendt naar IoT Hub via HTTP. HTTP is een van de drie protocollen IoT Hub te begrijpen.

In plaats van een verbinding met IoT Hub voor elke gesimuleerde apparaat openen, wordt deze module Hiermee opent u een HTTP-verbinding van de gateway op de hub IoT en multiplexes verbindingen van de gesimuleerde apparaten via deze verbinding. Hiermee kunt één gateway verbinding maken met veel meer apparaten, gesimuleerde of anderszins, dan zou het mogelijk als er een unieke verbinding voor elk apparaat geopend.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Voorbeeld van gesimuleerde apparaat Cloud uploaden]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md