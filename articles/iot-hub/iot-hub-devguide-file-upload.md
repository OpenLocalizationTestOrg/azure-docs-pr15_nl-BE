<properties
 pageTitle="Handleiding voor ontwikkelaars - bestand uploaden | Microsoft Azure"
 description="Azure IoT Hub developer guide - uploaden van bestanden vanaf een apparaat dat op de IoT Hub"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>Uploaden van bestanden vanaf een apparaat

## <a name="overview"></a>Overzicht

Zoals vastgelegd in de [eindpunten IoT Hub] [ lnk-endpoints] artikel apparaten tot stand kunnen brengen geüploade bestanden door het verzenden van een melding via een gerichte apparaat eindpunt (**/devices/ {deviceId} / bestanden**).  Wanneer een apparaat IoT Hub van een voltooide uploaden meldt, genereert de IoT Hub bestand uploaden meldingen die u via een service gerichte eindpunt (**/messages/servicebound/filenotifications**) als berichten kunt ontvangen.

In plaats van berichten via IoT Hub zelf bemiddelen fungeert IoT Hub in plaats daarvan als een coördinator aan een gekoppeld account voor de opslag van Azure. Een apparaat vraagt een token voor de opslag van IoT-Hub die specifiek is voor het bestand dat het apparaat wil uploaden. Het apparaat de SAS URI gebruikt het bestand te uploaden naar de opslag en wanneer de upload voltooid is het apparaat stuurt een melding van voltooiing op IoT-Hub. IoT Hub wordt gecontroleerd of het bestand is geüpload en vervolgens een bestand uploaden melding toegevoegd aan het nieuwe service gerichte bestand melding messaging-eindpunt.

Voordat u een bestand naar IoT Hub van een apparaat uploadt, moet u de hub door het [koppelen van een opslag Azure] [ lnk-associate-storage] naar deze rekening.

Uw apparaat kan vervolgens [uploaden initialiseren] [ lnk-initialize] en een [hub IoT kennis] [ lnk-notify] wanneer het uploaden is voltooid. Optioneel, wanneer een apparaat IoT Hub de upload is voltooid meldt, de service kan genereren een [melding][lnk-service-notification].

### <a name="when-to-use"></a>Wanneer gebruikt u

Deze functie IoT Hub gebruiken wanneer u een bestand van een apparaat met uw back-end-service in plaats van het verzenden van een bericht via de IoT Hub te uploaden.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Een opslag Azure-account koppelen aan IoT Hub

Het bestand uploaden als functionaliteit wilt gebruiken, moet u eerst een opslag Azure-account koppelen aan de IoT Hub. U kunt dit doen via de [Azure portal][lnk-management-portal], of via een programma via de [IoT Hub resource provider REST API's][lnk-resource-provider-apis]. Zodra u een account Azure opslag hebt gekoppeld aan uw IoT Hub, de service geeft als resultaat een URI SAS naar een apparaat wanneer het apparaat geeft aan een bestand uploaden.

> [AZURE.NOTE] De [SDK's Azure IoT Hub] [ lnk-sdks] automatisch ophalen van de URI SAS, het bestand uploaden en IoT Hub van een voltooide uploaden melding afhandelen.

## <a name="initialize-a-file-upload"></a>Initialiseren van een bestand is geüpload

IoT Hub heeft een eindpunt voor apparaten voor het aanvragen van een URI SAS voor opslag naar een bestand te uploaden. Het apparaat start het uploadproces van het bestand door een bericht te sturen naar de hub IoT op `{iot hub}.azure-devices.net/devices/{deviceId}/files` met de volgende JSON-instantie:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub als resultaat waarmee het apparaat wordt gebruikt om het bestand te uploaden:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Vervangen: het uploaden van een bestand met een GET initialiseren

> [AZURE.NOTE] Deze sectie beschrijft de functionaliteit voor het ontvangen van een URI SAS van IoT-Hub die is vervangen. Gebruik de methode POST hierboven beschreven.

IoT Hub heeft twee REST eindpunten ter ondersteuning van uploaden van het bestand, een om de URI SAS voor opslag en de andere op de hoogte stellen van de hub IoT een voltooide geüpload. Het apparaat start het uploadproces van het bestand door een GET te sturen naar de hub IoT op `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. De hub geeft als resultaat een URI SAS specifieke naar het bestand te uploaden en een correlatie-ID moet worden gebruikt nadat het uploaden is voltooid.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Kennis van de Hub van een bestandsupload voltooide IoT

Het apparaat is verantwoordelijk voor het uploaden van het bestand naar de opslag met behulp van de SDK's Azure-opslag. Nadat het uploaden is voltooid, het apparaat een bericht verzendt naar de hub IoT op `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` met de volgende JSON-instantie:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

De waarde van `isSuccess` is een Boolean-waarde die aangeeft of het bestand is geüpload of niet. De statuscode voor `statusCode` is de status voor het uploaden van het bestand wilt opslaan, en de `statusDescription` komt overeen met de `statusCode`.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende verwijzing onderwerpen bevatten meer informatie over het uploaden van bestanden vanaf een apparaat.

## <a name="file-upload-notifications"></a>Bestand uploaden meldingen

Wanneer een apparaat een bestand ophaalt en IoT-Hub van de upload is voltooid waarschuwt, wordt de service eventueel een bericht met de locatie en de opslag van het bestand gegenereerd.

Zoals uiteengezet in de [eindpunten][lnk-endpoints], IoT Hub biedt bestand uploaden meldingen via een gerichte service-eindpunt (**/messages/servicebound/fileuploadnotifications**) als berichten. De semantiek ontvangen voor het bestand uploaden meldingen zijn hetzelfde als voor cloud-to-device-berichten en hebben de hetzelfde [bericht lifecycle][lnk-lifecycle]. Elk bericht wordt opgehaald uit het bestand uploaden melding eindpunt is een JSON-record met de volgende eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| EnqueuedTimeUtc | Tijdstempel dat aangeeft wanneer de melding is gemaakt. |
| DeviceId | **DeviceId** van het apparaat waarmee het bestand wordt geüpload. |
| BlobUri | De URI van het geüploade bestand. |
| BlobName | De naam van het geladen bestand. |
| LastUpdatedTime | Tijdstempel dat aangeeft wanneer het bestand het laatst is bijgewerkt. |
| BlobSizeInBytes | De grootte van het geüploade bestand. |

**Voorbeeld**. Dit is de hoofdtekst van een voorbeeld van een melding bestand uploaden.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Bestand uploaden Meldingsopties configureren

Elke hub IoT bevat de volgende configuratieopties voor meldingen van bestand uploaden:

| Eigenschap | Beschrijving | Bereik en standaard |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Hiermee bepaalt u of meldingen over bestand worden geschreven naar het bestand meldingen eindpunt. | BOOL. Default: True. |
| **fileNotifications.ttlAsIso8601** | Standaard-TTL voor meldingen over bestand. | ISO_8601 interval tot 48 uur (minimaal 1 minuut). Standaard: 1 uur. |
| **fileNotifications.lockDuration** | De duur van de vergrendeling voor de wachtrij bestand uploaden meldingen. | 5-300 seconden (minimaal 5 seconden). Standaard: 60 seconden. |
| **fileNotifications.maxDeliveryCount** | Levering van maximum aantal voor het bestand uploaden wachtrij. | 1 tot en met 100. Standaard: 100. |

## <a name="additional-reference-material"></a>Aanvullend referentiemateriaal

Er zijn andere zoeken naar onderwerpen in de handleiding voor ontwikkelaars:

- [Eindpunten IoT Hub] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen.
- [Procesbeperking en quota's] [ lnk-quotas] de quota die van toepassing zijn op de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u de service wordt beschreven.
- [SDK's IoT Hub apparaat- en] [ lnk-sdks] geeft een overzicht van de verschillende taal SDK's u een gebruiken, wanneer u zowel apparaat als service toepassingen ontwikkelt die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] de querytaal kunt u gegevens ophalen uit de IoT Hub over uw apparaat twins, methoden en taken beschreven.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over de IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u het uploaden van bestanden van apparaten met IoT Hub hebt geleerd, kunt u mogelijk geïnteresseerd in de volgende onderwerpen in de handleiding voor ontwikkelaars:

- [Identiteiten in IoT Hub apparaat beheren][lnk-devguide-identities]
- [Toegang tot de IoT Hub][lnk-devguide-security]
- [Twins apparaat gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Een directe methode aanroept op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u proberen sommige van de concepten die in dit artikel worden beschreven wilt, kunt u mogelijk geïnteresseerd in de volgende zelfstudie voor IoT Hub:

- [Het uploaden van bestanden van apparaten naar de cloud met IoT Hub][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
