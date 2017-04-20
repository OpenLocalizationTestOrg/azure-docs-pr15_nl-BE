<properties
    pageTitle="Azure IoT apparaat SDK voor C - IoTHubClient | Microsoft Azure"
    description="Meer informatie over het gebruik van de IoTHubClient-bibliotheek in het apparaat Azure IoT SDK voor C"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT apparaat SDK voor C – meer informatie over IoTHubClient

Het [eerste artikel](iot-hub-device-sdk-c-intro.md) in deze serie het **Microsoft Azure IoT apparaat SDK voor C**geïntroduceerd. Dat artikel wordt uitgelegd dat er twee architecturale lagen in de SDK zijn. Aan de basis is de **IoTHubClient** -bibliotheek die communicatie met IoT Hub rechtstreeks beheert. Er is ook de **serializer** -bibliotheek die is gebaseerd op deze voor serialisatie services. In dit artikel vindt u meer details op de **IoTHubClient** -bibliotheek.

Het vorige artikel beschreven hoe u met de bibliotheek **IoTHubClient** IoT Hub gebeurtenissen verzenden en ontvangen van berichten. In dit artikel wordt uitgebreid die discussie door waarin wordt uitgelegd hoe nauwkeuriger beheren *wanneer* u gegevens verzenden en ontvangen, aan de **API's van lager niveau**. We zullen ook wordt uitgelegd hoe eigenschappen koppelen aan gebeurtenissen (en berichten ophalen) met behulp van de eigenschap functies in de bibliotheek **IoTHubClient** te verwerken. Wij bieden bovendien extra uitleg van de verschillende manieren om berichten ontvangen van IoT Hub te handelen.

Het artikel eindigt met betrekking tot een aantal diverse onderwerpen, waaronder meer over inloggegevens en het gedrag van de **IoTHubClient** door de configuratieopties wijzigen.

Om uit te leggen deze onderwerpen gebruiken we de **IoTHubClient** SDK-voorbeelden. Als u volgen wilt, Zie het **iothub\_client\_monster\_http** en **iothub\_client\_monster\_amqp** toepassingen die zijn opgenomen in de Azure IoT apparaat SDK voor C. alles in de volgende secties wordt beschreven in de volgende voorbeelden wordt aangetoond.

U kunt de **Azure IoT apparaat SDK voor C** vinden in de bibliotheek van [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub en details van de API in de [C-API reference](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)bekijken.

## <a name="the-lower-level-apis"></a>De API's van lager niveau

Het vorige artikel beschreven de werking van de **IotHubClient** in het kader van de **iothub\_client\_monster\_amqp** toepassing. Bijvoorbeeld, het bij het initialiseren van de bibliotheek met behulp van deze code toegelicht.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Het beschreven verzenden van gebeurtenissen met behulp van deze functie-aanroep ook.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Het artikel wordt ook beschreven hoe u berichten ontvangen door een callback-functie registreren.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Het artikel ook bleek het vrijmaken van bronnen met behulp van de volgende code.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Er zijn echter aanvullende functies voor elk van deze API's:

-   IoTHubClient\_ES\_CreateFromConnectionString

-   IoTHubClient\_ES\_SendEventAsync

-   IoTHubClient\_ES\_SetMessageCallback

-   IoTHubClient\_ES\_vernietigen


"Alles" opnemen deze alle functies in de naam van de API. Dat, zijn de parameters van elk van deze functies zijn identiek aan hun tegenhangers niet alles. Het gedrag van deze functies verschilt echter op één belangrijk.

Wanneer u belt **IoTHubClient\_CreateFromConnectionString**, de onderliggende bibliotheken maken een nieuwe thread die op de achtergrond wordt uitgevoerd. Deze thread gebeurtenissen, berichten verzonden en ontvangen van IoT Hub. Geen van deze thread wordt gemaakt wanneer u werkt met de API's van "Alles". Het maken van de thread op de achtergrond is ten behoeve van de ontwikkelaar. U hebt geen zorgen te maken over het expliciet gebeurtenissen verzenden en ontvangen van berichten van IoT-Hub gebeurt automatisch op de achtergrond. Daarentegen geven "LL" API's u expliciete controle over de communicatie met IoT Hub, als u deze nodig hebt.

Om dit beter te begrijpen, bekijk een voorbeeld:

Wanneer u belt **IoTHubClient\_SendEventAsync**, wat u daadwerkelijk doet is de gebeurtenis plaatsen in een buffer. De thread op de achtergrond gemaakt bij het aanroepen van **IoTHubClient\_CreateFromConnectionString** voortdurend bewaakt deze buffer en verzendt gegevens die deze bevat naar IoT Hub. Dit gebeurt op de achtergrond op het moment dat de hoofdthread andere werkzaamheden uitvoert.

Op dezelfde manier als het registreren van een callback-functie voor berichten met **IoTHubClient\_SetMessageCallback**, bent u de SDK om de thread op de achtergrond de callback-functie aanroepen wanneer een bericht ontvangen, onafhankelijk van de hoofdthread is instructie.

De API's van "Alles" maken niet een achtergrondthread. In plaats daarvan moet een nieuwe API worden aangeroepen om expliciet gegevens verzenden en ontvangen van IoT Hub. Dit wordt geïllustreerd in het volgende voorbeeld.

De **iothub\_client\_monster\_http** toepassing die opgenomen in de SDK van de API's van lager niveau wordt getoond. In de steekproef verzenden we gebeurtenissen naar IoT Hub met bijvoorbeeld de volgende code:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

De eerste drie regels maakt u het bericht en de laatste regel wordt de gebeurtenis verzonden. Echter, zoals gezegd, 'verzenden' de gebeurtenis wordt de gegevens eenvoudigweg in een buffer geplaatst. Niets in het netwerk wordt verzonden wanneer we noemen **IoTHubClient\_ES\_SendEventAsync**. Om te ingress daadwerkelijk de gegevens op de IoT Hub, belt u **IoTHubClient\_ES\_DoWork**, zoals in het volgende voorbeeld:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Deze code (uit de **iothub\_client\_monster\_http** toepassing) herhaaldelijk opgeroepen **IoTHubClient\_ES\_DoWork**. Elke keer dat **IoTHubClient\_ES\_DoWork** is genoemd, stuurt het sommige gebeurtenissen uit de buffer naar IoT Hub en een bericht in de wachtrij worden verzonden naar het apparaat worden opgehaald. Het laatste geval betekent dat als we een callback-functie voor berichten hebt geregistreerd, vervolgens de retouraanroep is aangeroepen (ervan uitgaande dat er berichten worden in de wachtrij geplaatst). We zou hebben een retouraanroepfunctie geregistreerd met bijvoorbeeld de volgende code:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

De reden dat **IoTHubClient\_ES\_DoWork** wordt vaak in een lus genoemd is dat elke keer dat deze wordt aangeroepen, *Sommige* gebufferde gebeurtenissen stuurt naar IoT Hub en haalt *het volgende* bericht dat in de wachtrij geplaatst voor het apparaat. Elk gesprek is niet gegarandeerd voor het verzenden van alle gebufferde gebeurtenissen of voor het ophalen van alle berichten in de wachtrij. Als u wilt dat alle gebeurtenissen in de buffer verzenden en klik vervolgens op met andere verwerkingsprocessen dat kunt u deze lus vervangen door de volgende code:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Deze code roept **IoTHubClient\_ES\_DoWork** totdat alle gebeurtenissen in de buffer zijn verzonden naar de IoT Hub. Opmerking die dit ook niet impliceert dat alle berichten in de wachtrij zijn ontvangen. Deel van de reden hiervoor is dat een actie als deterministisch controleren op berichten "Alles" niet. Wat gebeurt er als u 'alle' berichten ophalen, maar vervolgens een andere wordt verzonden naar het apparaat meteen na? Een betere manier omgaan met een geprogrammeerde time-out. Het bericht callback-functie kan bijvoorbeeld een timer instellen telkens wanneer deze wordt aangeroepen. Vervolgens kunt u de logica om te gaan met de verwerking als bijvoorbeeld geen berichten zijn ontvangen in de afgelopen *X* seconden.

Wanneer u bent klaar ingressing gebeurtenissen en ontvangen van berichten, moet u de bijbehorende functie voor het opschonen van bronnen.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

In principe is er slechts één set API's te verzenden en ontvangen van gegevens met een thread op de achtergrond en een andere set API's die hetzelfde zonder de thread op de achtergrond doet. Veel ontwikkelaars mogelijk de voorkeur aan de niet - LL API's, maar de API's van lager niveau zijn handig als de ontwikkelaar wil expliciete controle over het netwerk. Bijvoorbeeld, verzamelen sommige apparaten gegevens gedurende een bepaalde tijd alleen ingress-gebeurtenissen met opgegeven intervallen (bijvoorbeeld één keer een uur of één keer per dag). De API's van lager niveau kunnen u expliciet beheren als u gegevens verzenden en van IoT Hub ontvangen. Anderen zullen gewoon de voorkeur aan die de API's van lager niveau bieden. Alles wat er gebeurt op de hoofdthread in plaats van sommige werk gebeurt op de achtergrond.

Het model dat u kiest, moet u consistente welke API's die u gebruikt. Als u met het aanroepen van Start **IoTHubClient\_ES\_CreateFromConnectionString**, zorg ervoor dat u alleen de bijbehorende lager niveau API's gebruiken voor alle werkzaamheden:

-   IoTHubClient\_ES\_SendEventAsync

-   IoTHubClient\_ES\_SetMessageCallback

-   IoTHubClient\_ES\_vernietigen

-   IoTHubClient\_ES\_DoWork

Het omgekeerde is ook waar. Als u met begint **IoTHubClient\_CreateFromConnectionString**, vervolgens de niet - LL-API's te gebruiken voor elke verdere verwerking.

Zie de Azure IoT apparaat SDK voor C, de **iothub\_client\_monster\_http** toepassing voor een volledig voorbeeld van de API's van lager niveau. De **iothub\_client\_monster\_amqp** toepassing voor een volledig voorbeeld van de niet - LL API's kan worden verwezen.

## <a name="property-handling"></a>Eigenschap verwerking

Tot nu toe wanneer we verzenden gegevens hebt beschreven, we hebt zijn die verwijzen naar de hoofdtekst van het bericht. Neem bijvoorbeeld deze code:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

In het volgende voorbeeld stuurt een bericht naar IoT Hub met de tekst 'Hello World'. IoT Hub kunnen echter ook de eigenschappen worden gekoppeld aan elk bericht. Eigenschappen zijn naam/waarde-paren die kunnen worden gekoppeld aan het bericht. Zo kunnen we de voorgaande code als u een eigenschap toevoegen aan het bericht wilt wijzigen:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Begin met het aanroepen van **IoTHubMessage\_eigenschappen** en doorgeven van de ingang van ons bericht. Wat krijgen we terug is een **kaart\_verwerken** verwijzing waarmee we beginnen met het toevoegen van eigenschappen. Deze laatste wordt gerealiseerd door het aanroepen van **kaart\_AddOrUpdate**, die krijgt een verwijzing naar een MAP\_greep, naam van de eigenschap en waarde van de eigenschap. Met deze API kunnen we zoveel eigenschappen als we willen toevoegen.

Wanneer de gebeurtenis wordt gelezen uit de **Gebeurtenis Hubs**, kan de ontvanger de eigenschappen opsommen en de bijbehorende waarden worden opgehaald. Bijvoorbeeld in .NET zou dit worden bereikt door toegang tot de [collectie Properties van het object EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

In het vorige voorbeeld koppelt wij eigenschappen aan een gebeurtenis die wij met IoT Hub verzendt. Eigenschappen kunnen ook worden gekoppeld aan berichten ontvangen van IoT Hub. Als we ophalen van de eigenschappen van een bericht willen, kunnen we de volgende code gebruiken in onze bericht callback-functie:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

De aanroep van **IoTHubMessage\_eigenschappen** retourneert de **kaart\_verwerken** verwijzing. Vervolgens geven we die verwijzing naar **overzicht\_GetInternals** een verwijzing naar een matrix van het naam/waarde-paren (evenals een aantal van de eigenschappen). Op dat moment is het een eenvoudige kwestie zijn van het inventariseren van de eigenschappen voor de waarden die we willen.

U hebt geen eigenschappen te gebruiken in uw toepassing. Echter, als u deze instelt op gebeurtenissen of berichten ophalen, de bibliotheek **IoTHubClient** eenvoudig.

## <a name="message-handling"></a>Afhandeling van berichten

Zoals eerder vermeld wanneer er berichten binnenkomen van IoT Hub reageert door het aanroepen van een geregistreerde callback-functie in de bibliotheek **IoTHubClient** . Er is een return-parameter van deze functie, een extra uitleg verdient. Hier is een uittreksel van de callback-functie in de **iothub\_client\_monster\_http** -voorbeeldtoepassing:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Opmerking: het retourtype is **IOTHUBMESSAGE\_DISPOSITION\_resultaat** en in dit geval keren we terug **IOTHUBMESSAGE\_goedgekeurd**. Er zijn andere keren we terug van deze functie waarden die wijzigen hoe de bibliotheek **IoTHubClient** aan de retouraanroep bericht reageert. Hier vindt u de opties.

-   **IOTHUBMESSAGE\_GEACCEPTEERDE** – het bericht is verwerkt. De **IoTHubClient** -bibliotheek wordt met hetzelfde bericht opnieuw de callback-functie niet aanroepen.

-   **IOTHUBMESSAGE\_afgewezen** : het bericht niet is verwerkt en er is geen behoefte om dit te doen in de toekomst. De bibliotheek **IoTHubClient** moet met hetzelfde bericht opnieuw de callback-functie niet aanroepen.

-   **IOTHUBMESSAGE\_ABANDONED** – het bericht niet kan worden verwerkt, maar de **IoTHubClient** -bibliotheek moet de callback-functie met hetzelfde bericht opnieuw aanroepen.

Voor de eerste twee retourcodes stuurt de bibliotheek **IoTHubClient** een bericht naar IoT Hub waarmee wordt aangegeven dat het bericht moet worden verwijderd uit de apparaatwachtrij en niet bezorgd opnieuw. Het netto-effect is hetzelfde (het bericht is verwijderd uit de apparaatwachtrij), maar nog steeds of het bericht is geaccepteerd of afgewezen is vastgelegd.  Opnemen van dit onderscheid is handig om de afzenders van het bericht kunnen luisteren naar feedback te vinden en als een apparaat heeft geaccepteerd of geweigerd van een bepaald bericht.

In het laatste geval ook een bericht wordt verzonden met IoT Hub, maar geeft u aan dat het bericht moet worden geleverd. Meestal zult u een bericht annuleren als u er een fout optreden maar probeer het bericht opnieuw te verwerken. Daarentegen is afwijzen van een bericht geschikt wanneer er een onherstelbare fout optreedt (of als u gewoon dat u niet wilt dat het bericht verwerkt).

In ieder geval worden op de hoogte van de verschillende retourcodes zodat kunt u het uit de bibliotheek **IoTHubClient** het gewenste gedrag veroorzaken.

## <a name="alternate-device-credentials"></a>Alternatieve inloggegevens

Zoals eerder vermeld, het eerste wat te doen bij het werken met de **IoTHubClient** -bibliotheek te verkrijgen is een **IOTHUB\_CLIENT\_verwerken** met de volgende oproep:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

De argumenten voor **IoTHubClient\_CreateFromConnectionString** zijn de verbindingsreeks van onze apparaat en een parameter die aangeeft van het protocol dat wordt gebruikt om te communiceren met IoT Hub. De verbindingsreeks heeft een indeling die er als volgt uitziet:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Er zijn vier soorten informatie in deze reeks: naam IoT Hub, IoT Hub achtervoegsel, apparaat-ID en gedeelde toegangssleutel. Verkrijgen van de volledig gekwalificeerde domeinnaam (FQDN) van een hub IoT wanneer u uw exemplaar van de hub IoT in de portal Azure maken — Hiermee geeft u de naam van de IoT hub (het eerste deel van de FQDN-naam) en achtervoegsel (de rest van de FQDN-naam) van de hub IoT. Krijgt u de apparaat-ID en de toegang tot gedeelde sleutel tijdens de registratie van uw apparaat met IoT Hub (zoals beschreven in het [vorige artikel](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** beschikt u over een manier om de bibliotheek te initialiseren. Als u wilt, kunt u een nieuwe **IOTHUB\_CLIENT\_verwerken** met behulp van deze afzonderlijke parameters in plaats van de verbindingsreeks. Dit wordt bereikt met de volgende code:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Dit gebeurt hetzelfde als **IoTHubClient\_CreateFromConnectionString**.

Lijkt voor de hand liggende zou willen gebruiken **IoTHubClient\_CreateFromConnectionString** in plaats van deze uitgebreidere methode van het initialisatieproces. Houd er echter rekening mee, dat wanneer u een apparaat in de IoT Hub registreren wat u krijgt is een apparaat-ID en de apparaatsleutel (niet een verbindingsreeks). Het **Device Manager** SDK-hulpprogramma opgenomen in het [vorige artikel](iot-hub-device-sdk-c-intro.md) wordt de verbindingsreeks van de apparaat-ID, apparaatsleutel en IoT Hub hostnaam bibliotheken in de **IoT Azure service SDK** . Dus het aanroepen van **IoTHubClient\_ES\_maken** wellicht beter omdat de stap slaat van het genereren van een verbindingsreeks. Gebruik de methode die geschikt is.

## <a name="configuration-options"></a>Configuratie-opties

Tot nu toe weerspiegelt alles beschreven over de werking van de bibliotheek **IoTHubClient** het standaardgedrag. Er zijn echter een aantal opties die u instellen kunt om te wijzigen hoe de bibliotheek werkt. Dit wordt gedaan door gebruik te maken van de **IoTHubClient\_ES\_SetOption** API. Bekijk het volgende voorbeeld:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Er zijn een aantal opties die vaak worden gebruikt:

-   **SetBatching** (bool) – als **true**, en vervolgens gegevens verzonden naar IoT Hub in batches wordt verzonden. Als **de waarde false**en vervolgens berichten afzonderlijk worden verzonden. De standaardwaarde is **false**. Houd er rekening mee dat de optie **SetBatching** is alleen van toepassing op het HTTP-protocol en niet op de MQTT of AMQP protocollen.

-   **Time-out** (niet-ondertekende int): deze waarde wordt weergegeven in milliseconden. Als een HTTP-verzoek of een antwoord ontvangen langer duurt dan deze tijd, wordt de time-out voor de verbinding wilt verzenden.

De optie batchen is belangrijk. Standaard wordt de bibliotheek ingresses gebeurtenissen afzonderlijk (één gebeurtenis is alles wat u doorgeeft aan **IoTHubClient\_LL\_SendEventAsync**). Als de optie batchen **ingesteld op true is**, verzamelt de bibliotheek zo veel gebeurtenissen zoals kan het uit de buffer (tot de maximale berichtgrootte die IoT Hub accepteert).  De gebeurtenis batch wordt verzonden naar de IoT Hub in één HTTP-aanroep (afzonderlijke gebeurtenissen zijn gebundeld in een matrix JSON). Inschakelen batchen doorgaans resulteert in grote prestatieverbeteringen omdat u bent omweg via netwerk verminderen. Bovendien vermindert het bandbreedte aangezien u één set HTTP-headers met een gebeurtenis batch in plaats van een set headers voor elke afzonderlijke gebeurtenis verzendt. Tenzij u een specifieke reden hebt, zult u doorgaans batchen inschakelen.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven in detail de werking van de bibliotheek **IoTHubClient** is gevonden in de **Azure IoT apparaat SDK voor C**. Met deze informatie hebt u een goed inzicht in de mogelijkheden van de **IoTHubClient** -bibliotheek. Het [volgende artikel](iot-hub-device-sdk-c-serializer.md) biedt vergelijkbare gegevens over de **serializer** library.

Zie voor meer informatie over het ontwikkelen voor IoT Hub, de [IoT Hub SDK's][lnk-sdks].

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
