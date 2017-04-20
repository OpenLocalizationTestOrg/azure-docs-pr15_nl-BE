<properties
 pageTitle="Ondersteuning voor IoT Hub MQTT | Microsoft Azure"
 description="Beschrijving van MQTT in de hub IoT-niveau ondersteunen"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT Hub MQTT ondersteuning

IoT Hub kunnen apparaten om te communiceren met de IoT Hub apparaat eindpunten met behulp van de [MQTT v3.1.1] [ lnk-mqtt-org] protocol op poort 8883 of MQTT v3.1.1 via WebSocket-protocol op poort 443. IoT Hub is vereist voor alle apparaat-communicatie te beveiligen met TLS/SSL (dus IoT Hub ondersteunt geen niet-beveiligde verbindingen via poort. 1883).

## <a name="connecting-to-iot-hub"></a>Verbinding maken met IoT Hub

Een apparaat kan het MQTT-protocol gebruiken verbinding maken met een IoT hub via de bibliotheken in de [Microsoft Azure IoT SDK's] [ lnk-device-sdks] of protocol direct met behulp van de MQTT.

## <a name="using-the-device-client-sdks"></a>Met behulp van de apparaat-client SDK 's

[Device-client SDK's] [ lnk-device-sdks] die ondersteuning voor het protocol van MQTT zijn beschikbaar voor Java, Node.js, C, C# en Python. De tekenreeks standaard IoT Hub het apparaat client SDK's gebruiken een verbinding maken met een hub IoT. Het MQTT-protocol gebruikt, moet de parameter client protocol worden ingesteld op **MQTT**. Standaard de device-client SDK's verbinding maken met een IoT Hub met de **CleanSession** vlag ingesteld op **0** en **1 QoS** voor uitwisseling van berichten met de IoT-hub.

Wanneer een apparaat is aangesloten op een hub IoT, bieden de device-client SDK's methoden waarmee het apparaat om berichten te verzenden en ontvangen van een hub IoT.

De volgende tabel bevat koppelingen naar voorbeelden van code voor elke ondersteunde taal en geeft u de parameter waarvoor u een verbinding maken met IoT Hub met het MQTT-protocol.

| Taal                   | De parameter protocol        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure iot-apparaat mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Een apparaat app migratie van AMQP naar MQTT
Als u de [device-client SDK's][lnk-device-sdks], overschakelen van AMQP om MQTT te gebruiken moet de parameter protocol in de initialisatie van de client wijzigen zoals hierboven aangegeven.

Wanneer u dit doet, Controleer de volgende items:

* AMQP retourneert fouten voor veel voorwaarden, terwijl MQTT de verbinding verbreekt. Als gevolg daarvan de logica voor de verwerking van uitzonderingen mogelijk enkele wijzigingen.
* MQTT biedt geen ondersteuning voor de bewerkingen *negeren* bij het ontvangen van [berichten C2D][lnk-messaging]. Als uw back-end een antwoord ontvangen van de app van het apparaat moet, kunt u overwegen [directe methoden][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Het directe gebruik van het protocol van MQTT

Als een apparaat niet kan het apparaat client SDK's, kan deze nog steeds verbinding maken met de eindpunten van de openbare apparaten met het MQTT-protocol. In het pakket **VERBINDEN** moet het apparaat de volgende waarden gebruiken:

- Gebruik de **deviceId**voor het veld **ClientId** . 
- Gebruik voor het veld **Username** , `{iothubhostname}/{device_id}`, waarbij {iothubhostname} is de volledige CName van de hub IoT.

    Bijvoorbeeld als de naam van uw hub-IoT **contoso.azure devices.net** en de naam van het apparaat **MyDevice01 is**, het volledige veld **Username** moet bevatten `contoso.azure-devices.net/MyDevice01`.

- Gebruik een SAS-token voor het veld **wachtwoord** . De indeling van het SAS-token is hetzelfde als voor de HTTP- en AMQP protocollen:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Voor meer informatie over het genereren van SAS tokens, raadpleegt u de sectie apparaat van [beveiligingstokens IoT Hub met behulp van][lnk-sas-tokens].
    
    Bij het testen, ook kunt u het [Apparaat Explorer] [ lnk-device-explorer] snel een SAS-token die u kunt kopiëren en plakken in uw eigen code genereren:
    
    1. Ga naar het tabblad **beheer** in Explorer apparaat.
    2. Klik op een **SAS-Token** (rechtsboven).
    3. Op **SASTokenForm**, selecteer het apparaat in de **DeviceID** vervolgkeuzelijst. De **TTL-waarde**instellen.
    4. Klik op **genereren** als u wilt maken van uw token.
    
    De SAS-token dat wordt gegenereerd, is deze structuur:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Het deel van deze token als het veld **wachtwoord** verbinding te maken met behulp van MQTT gebruikt:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

MQTT verbinding maken en verbreken van pakketten, IoT Hub problemen een gebeurtenis op het kanaal voor het **Controleren van bewerkingen** met gegevens die u helpen kan bij het oplossen van verbindingsproblemen.

### <a name="sending-messages-to-iot-hub"></a>Berichten verzenden naar IoT Hub

Nadat u een verbinding, een apparaat kunt berichten verzenden naar IoT Hub met `devices/{device_id}/messages/events/` of `devices/{device_id}/messages/events/{property_bag}` als **De naam van het onderwerp**. De `{property_bag}` element kunt u het apparaat voor het verzenden van berichten met extra eigenschappen in een url-gecodeerde indeling. Bijvoorbeeld:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] Dit `{property_bag}` element gebruikt dezelfde codering als voor de query-tekenreeks in het HTTP-protocol.

Device-clienttoepassing kun `devices/{device_id}/messages/events/{property_bag}` als de **onderwerpnaam zal** *worden berichten* worden doorgestuurd als bericht telemetrie definiëren.

IoT Hub biedt geen ondersteuning voor QoS-2 berichten. Als een client apparaat een bericht met **QoS-2 publiceert**, gesloten IoT Hub de netwerkverbinding.
IoT Hub blijven behouden berichten niet worden bewaard. IoT Hub wordt toegevoegd als een apparaat een bericht met de vlag **bewaren** is ingesteld op 1 verzendt, de **x-opt-behoudt** de eigenschap application bij het bericht. In dit geval in plaats van de persistent maken het bericht behouden, IoT Hub wordt doorgegeven aan de toepassing van de back-end.

### <a name="receiving-messages"></a>Ontvangen berichten

Om berichten te ontvangen van IoT Hub, een apparaat moet aanmelden met behulp van `devices/{device_id}/messages/devicebound/#` als **Onderwerp Filter**. Het jokerteken voor meerdere niveaus **#** in het onderwerp Filter wordt alleen gebruikt om het apparaat voor het ontvangen van aanvullende eigenschappen in de naam van het onderwerp. IoT Hub is niet toegestaan bij het gebruik van de **#** of **?** jokertekens voor het filteren van de onderliggende onderwerpen. Aangezien IoT Hub niet een algemene pub sub messaging broker, alleen ondersteund de onderwerpnamen van gedocumenteerde en onderwerp filters.

Neem de opmerking, dat het apparaat geen berichten van IoT Hub, ontvangt voordat deze zich heeft geabonneerd op het eindpunt van de apparaat-specifieke vertegenwoordigd door de `devices/{device_id}/messages/devicebound/#` onderwerp filter. Nadat het abonnement succesvol is gebracht, start het apparaat ontvangen van wolk naar apparaat alleen berichten die zijn verzonden naar het na de tijd van het abonnement. Als het apparaat verbinding met de vlag **CleanSession** is ingesteld op **0 maakt**, wordt het abonnement in verschillende sessies worden vastgelegd. In dit geval ontvangt de volgende keer dat verbinding wordt gemaakt met **CleanSession 0** het apparaat openstaande berichten die zijn verzonden naar het terwijl deze verbinding is verbroken. Als het apparaat maakt gebruik van de vlag **CleanSession** is ingesteld op **1** , hoewel het ontvangt geen berichten van IoT Hub totdat het voldoet aan het apparaat-eindpunt.

IoT-Hub levert berichten met de **Naam van het onderwerp** `devices/{device_id}/messages/devicebound/`, of `devices/{device_id}/messages/devicebound/{property_bag}` als u de eigenschappen van elk bericht. `{property_bag}`bevat de url-gecodeerde sleutel/waarde-paren van berichteigenschappen. Alleen eigenschappen van toepassingen en de gebruiker instelbare Systeemeigenschappen (zoals **messageId** of **correlationId**) worden opgenomen in de eigenschappenverzameling. Eigenschapnamen hebben het voorvoegsel **$**, eigenschappen van de oorspronkelijke eigenschapnaam gebruikt met geen voorvoegsel.

Als een device-client zich op een onderwerp met **QoS-2 abonneert**, geeft IoT Hub het maximale QoS-niveau 1 in het pakket **SUBACK** . Daarna wordt IoT Hub berichten versturen naar het apparaat met behulp van QoS-1.

### <a name="additional-considerations"></a>Aanvullende overwegingen

Als een definitieve vergoeding wanneer moet u het gedrag van het protocol MQTT aan de kant van cloud aanpassen raadpleegt u de [Azure IoT-protocol gateway] [ lnk-azure-protocol-gateway] waarmee u een krachtige, aangepaste protocol gateway rechtstreeks aan de IoT Hub implementeren. De gateway Azure IoT-protocol kunt u de apparaat-protocol voor brownfield MQTT implementaties of andere aangepaste protocollen wilt aanpassen. Deze aanpak is echter vereist u uitvoert en een gateway aangepast protocol worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie, Zie [toelichting MQTT ondersteuning voor] [ lnk-mqtt-devguide] in de Azure IoT Hub developer guide.

Zie de [documentatie van de MQTT]voor meer informatie over het protocol MQTT,[lnk-mqtt-docs].

Voor meer informatie over het plannen van uw implementatie IoT Hub, Zie:

- [Azure gecertificeerd voor IoT apparaat catalogus][lnk-devices]
- [Aanvullende protocollen ondersteunen][lnk-protocols]
- [Vergelijken met de gebeurtenis Hubs][lnk-compare]
- [Schalen, HA en DR][lnk-scaling]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
