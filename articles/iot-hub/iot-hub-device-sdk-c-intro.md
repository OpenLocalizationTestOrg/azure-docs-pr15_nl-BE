<properties
    pageTitle="Met behulp van het apparaat Azure IoT SDK voor C | Microsoft Azure"
    description="Meer informatie over en werken met de voorbeeldcode in het apparaat Azure IoT SDK voor C."
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

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Introductie van het apparaat Azure IoT SDK voor C

De **Azure IoT apparaat SDK** is een verzameling van bibliotheken die zijn ontworpen om het proces van gebeurtenissen verzenden en ontvangen berichten van de **Azure IoT Hub** service. Dit artikel beschrijft de **Azure IoT apparaat SDK voor C**, maar er zijn verschillende varianten van de SDK, elk gericht op een specifiek platform.

Het apparaat Azure IoT SDK voor C is geschreven in ANSI C (C99) overdraagbaarheid maximaliseren. Dit maakt het zeer geschikt om te rijden op een aantal platforms en apparaten, vooral wanneer slechts weinig schijfruimte en geheugen footprint is een prioriteit.  

Er zijn tal van platforms waarop de SDK is getest (Zie de [Azure gecertificeerd voor IoT apparaat-catalogus](https://catalog.azureiotsuite.com/) voor details). Hoewel dit artikel scenario's van de voorbeeldcode is uitgevoerd op het Windows-platform bevat, houd er rekening mee dat de code in dit artikel beschreven precies hetzelfde over het aantal ondersteunde platformen is.

In dit artikel zult u worden ingevoerd om de architectuur van het apparaat Azure IoT SDK voor C. We zullen laten zien hoe het DLL-bestand te initialiseren, gebeurtenissen verzenden met IoT Hub, evenals berichten ontvangen. De informatie in dit artikel moet voldoende zijn om aan de slag met de SDK, maar ook vindt u hier verwijzingen naar aanvullende informatie over de bibliotheken.

## <a name="sdk-architecture"></a>SDK-architectuur

U kunt de **Azure IoT apparaat SDK voor C** vinden in de bibliotheek van [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub en details van de API in de [C-API reference](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)bekijken.

De meest recente versie van de bibliotheken vindt u in de **master** tak van deze opslagplaats:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Deze bibliotheek bevat de hele familie van Azure IoT apparaat SDK's. Dit artikel is echter over het apparaat Azure IoT SDK *voor C* die u in de map **c vinden kunt** .

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* De uitvoering van de kern van de SDK vindt u in de **iothub\_client** map die de uitvoering van de onderste laag van de API in de SDK bevat: de **IoTHubClient** -bibliotheek. De bibliotheek **IoTHubClient** bevat uitvoering van ruwe berichten voor het verzenden van berichten naar IoT Hub als ontvangen van deze API's. Wanneer u deze bibliotheek gebruikt, bent u verantwoordelijk voor de uitvoering van de serialisatie van bericht (uiteindelijk met behulp van de serializer voorbeeld hieronder beschreven), maar de overige bijzonderheden van de communicatie met IoT Hub voor u worden afgehandeld.
* De map **serializer** bevat hulpfuncties en voorbeelden waarin wordt getoond hoe serialiseren gegevens vóór verzending naar Azure IoT Hub met de clientbibliotheek. Houd er rekening mee dat het gebruik van de serializer niet verplicht en uitsluitend is als service aangeboden. Als u de bibliotheek **serializer** , begint u met het definiëren van een model dat de gebeurtenissen die u verzenden naar IoT Hub zoals u verwacht wilt te ontvangen van deze berichten bevat. Nadat het model is gedefinieerd, biedt de SDK een API oppervlak dat u gemakkelijk werken met gebeurtenissen en -berichten kunt zonder te hoeven maken over serialisatie details.
De bibliotheek is afhankelijk van andere open-source-bibliotheken die worden geïmplementeerd met behulp van verschillende protocollen (MQTT, AMQP) transport.
* De bibliotheek **IoTHubClient** is afhankelijk van andere open-source-bibliotheken:
   * De [Azure C hulpprogramma gedeelde](https://github.com/Azure/azure-c-shared-utility) bibliotheek met algemene functionaliteit voor basistaken (als tekenreeks, lijst manipulatie, i/o, enz....) die nodig zijn in verschillende Azure gerelateerde C SDK's
   * De bibliotheek [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) client side implementatie van AMQP is geoptimaliseerd voor resource beperking apparaten.
   * De [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) bibliotheek is een algemene bibliotheek ter uitvoering van het protocol van MQTT en geoptimaliseerd voor resource beperking apparaten.

Dat is gemakkelijker te begrijpen is door te kijken naar de voorbeeldcode. In de volgende secties leest u enkele van de voorbeeldtoepassingen die zijn opgenomen in de SDK. Hiermee geeft u een goed idee voor de verschillende mogelijkheden van de architectuur van de SDK-lagen als een inleiding tot de werking van de API's.

## <a name="before-running-the-samples"></a>Voordat de monsters worden uitgevoerd

Voordat de monsters u in het apparaat Azure IoT SDK voor C uitvoeren kunt moet u een exemplaar van de service op uw Azure abonnement als u niet al een hebt en 2 taken uitvoeren:
* Bereid uw ontwikkelomgeving
* inloggegevens aanvragen.

Als u een exemplaar maken van Azure IoT Hub op uw abonnement Azure, volgt u de instructies [hier](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

Het [Leesmij-bestand](https://github.com/Azure/azure-iot-sdks/tree/master/c) van de SDK bevat instructies voor het voorbereiden van uw ontwikkelomgeving en inloggegevens te verkrijgen.
In de volgende secties bevatten enkele extra commentaar bij deze instructies.

### <a name="preparing-your-development-environment"></a>Voorbereiden van uw ontwikkelomgeving

Terwijl de pakketten zijn beschikbaar voor sommige platforms (zoals NuGet voor Windows of apt_get voor Debian en Ubuntu) en de voorbeelden gebruiken deze pakketten, indien beschikbaar, de onderstaande instructies in detail te beschrijven hoe u de bibliotheek en de monsters formulier rechtstreeks de code.

Eerst moet u een kopie van de SDK downloaden van GitHub en vervolgens de bron te maken. U moet een kopie van de bron van de **master** branch van de [opslagplaats van GitHub](https://github.com/Azure/azure-iot-sdks).

Wanneer u een kopie van de bron hebt gedownload, moet u de stappen die worden beschreven in de SDK-artikel ['Voorbereiden uw ontwikkelomgeving'](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)uitvoeren.


Hier volgen enkele tips voor het uitvoeren van de procedure die wordt beschreven in de handleiding van het preparaat:

-   Wanneer u het hulpprogramma **CMake** installeert, kiest u de optie **CMake** toevoegen aan het systeem pad voor **alle gebruikers** (toe te voegen aan **de huidige gebruiker** werkt ook):

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Voordat u de **opdrachtprompt Developer voor VS2015**opent, installeert de Git opdrachtregelprogramma's. Als u deze hulpprogramma's installeert, moet u de volgende stappen uitvoeren:

    1. Start het setup-programma (of **Microsoft Visual Studio 2015** gekozen uit de **programma's en onderdelen** van het Configuratiescherm en selecteer **wijzigen**) **Visual Studio 2015** .
    
    2. Zorg ervoor dat de **Git voor Windows** -functie is ingeschakeld in het installatieprogramma, maar u kunt ook de optie **GitHub extensie voor Visual Studio** IDE integratie om te controleren:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. Voltooi de installatiewizard installeert de hulpprogramma's.

    4. De Git tools **bin** -map toevoegen aan de omgevingsvariabele **PATH** van het systeem. Op Windows, dit ziet er als volgt uit:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Als u alle stappen die worden beschreven in de pagina ['Uw ontwikkelomgeving voorbereiden'](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) hebt voltooid, kunt u bent de voorbeeldtoepassingen compileren.

### <a name="obtaining-device-credentials"></a>Inloggegevens aanvragen

Nu uw ontwikkelomgeving is ingesteld, het volgende dat te doen is om een set inloggegevens te krijgen.  Een apparaat kan toegang tot een IoT hub, moet u eerst het apparaat toevoegen aan het register IoT Hub apparaat. Wanneer u het apparaat toevoegt krijgt u een set met de inloggegevens die u nodig hebt om het apparaat aansluiten op een hub IoT. De voorbeeldtoepassingen die zullen we in de volgende sectie verwacht dat deze referenties in de vorm van een **verbindingsreeks apparaat**.

Er zijn een aantal hulpprogramma's in de opslagplaats van de open-source SDK om te helpen beheren van de IoT Hub. Een is Windows-toepassing met de naam van de Explorer-apparaat, in het tweede voorbeeld is dat een node.js cross-platform CLI hulpprogramma met de naam iothub explorer gebaseerd. Voor meer informatie over deze hulpprogramma's [hier](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Als we gaan door middel van de voorbeelden in dit artikel waarop Windows, gebruiken we het programma Explorer apparaat. Maar u kunt ook iothub explorer gebruiken als u liever CLI hulpmiddelen.

Het hulpprogramma [Apparaat Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) gebruikt de IoT Azure service bibliotheken op verschillende functies uitvoeren op de IoT Hub, waaronder het toevoegen van apparaten. Als u de Explorer-apparaat een apparaat toevoegen, krijgt u een bijbehorende verbindingsreeks. U moet deze verbindingsreeks maken het monster toepassingen worden uitgevoerd.

In het geval u nog niet vertrouwd bent met het proces, is de volgende procedure wordt beschreven hoe apparaat Explorer gebruiken op een apparaat toevoegen en u een verbindingsreeks apparaat.

Voor het apparaat Explorer tool op de [SDK release pagina](https://github.com/Azure/azure-iot-sdks/releases)vindt u een Windows installer. Maar u kunt ook het hulpprogramma uitvoeren rechtstreeks vanuit de code **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** openen in **Visual Studio 2015** en de oplossing bouwen.

Tijdens het uitvoeren van het programma ziet u deze interface:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

De **Verbindingsreeks IoT-Hub** in het eerste veld en klik op **bijwerken**. Hiermee wordt het hulpprogramma zodat het met IoT Hub communiceren kan.

Klik op het tabblad **beheer** zodra de verbindingsreeks IoT Hub is geconfigureerd:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Dit is waar u de apparaten die zijn geregistreerd in de hub IoT gaat beheren.

U kunt een apparaat maken door te klikken op de knop **maken** . Een dialoogvenster wordt weergegeven met een set vooraf ingestelde sleutels (primaire en secundaire). Hoeft u is een **Apparaat-ID** invoeren en klik vervolgens op **maken**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Zodra het apparaat is gemaakt, wordt de lijst met apparaten wordt bijgewerkt met alle geregistreerde apparaten, met inbegrip van een die zojuist gemaakt. Als u met de rechtermuisknop op het nieuwe apparaat, ziet u dit menu:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Als u de optie **kopiëren verbindingsreeks voor het geselecteerde apparaat** , is de verbindingsreeks voor het apparaat naar het Klembord gekopieerd. Bewaar een kopie van de verbindingsreeks. U moet deze tijdens het uitvoeren van de voorbeeldtoepassingen in de volgende secties wordt beschreven.

Als u de bovenstaande stappen hebt uitgevoerd, bent u klaar om bepaalde code wordt uitgevoerd. Beide voorbeelden hebben een constante aan de bovenkant van het belangrijkste bron-bestand waarmee u een verbindingsreeks opgeven. De bijbehorende regel uit de **iothub\_client\_monster\_amqp** toepassing als volgt wordt weergegeven.

```
static const char* connectionString = "[device connection string]";
```

Als u volgen wilt, geef hier uw apparaat verbindingsreeks opnieuw compileren van de oplossing en u zult kunnen uitvoeren van de steekproef.

## <a name="iothubclient"></a>IoTHubClient

In de **iothub\_client** map in de bibliotheek azure-iot-SDK's is een map met **voorbeelden** waarin een toepassing genoemd **iothub\_client\_monster\_amqp**.

De Windows-versie van de **iothub\_client\_monster\_ampq** toepassing bevat de volgende Visual Studio-oplossing:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

Deze oplossing bevat een enkel project. Het is vermeldenswaardig dat er vier NuGet pakketten geïnstalleerd in deze oplossing zijn:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Het pakket **Microsoft.Azure.C.SharedUtility** moet u altijd wanneer u met de SDK werkt. Omdat in dit voorbeeld is gebaseerd op AMQP, moet u ook de **Microsoft.Azure.uamqp** en **Microsoft.Azure.IoTHub.AmqpTransport** -pakketten (Er zijn vergelijkbare pakketten voor MQTT en HTTP). Omdat het voorbeeld wordt de **IoTHubClient** -bibliotheek gebruikt, moet u ook het **Microsoft.Azure.IoTHub.IoTHubClient** -pakket in de oplossing.

U vindt de uitvoering voor de voorbeeldtoepassing in de **iothub\_client\_monster\_amqp.c** bronbestand.

We gebruiken deze voorbeeldtoepassing u stapsgewijs door wat nodig is om de **IoTHubClient** -bibliotheek gebruiken.

### <a name="initializing-the-library"></a>Het initialiseren van de bibliotheek

> [AZURE.NOTE] Voordat u begint te werken met de bibliotheken, moet u mogelijk sommige specifieke initialisatie platform. Als u van plan bent te gebruiken van AMQP op Linux moet u bijvoorbeeld de bibliotheek OpenSSL initialiseren. De monsters in de [opslagplaats van GitHub](https://github.com/Azure/azure-iot-sdks) roept het hulpprogramma functie **platform_init** wanneer de client wordt gestart en de functie **platform_deinit** aanroepen voordat u afsluit. Deze functies zijn gedefinieerd in het bestand 'platform.h'-header. U moet de definities van deze functies voor uw doelplatform in de [bibliotheek](https://github.com/Azure/azure-iot-sdks) om te bepalen of u voor elk platform initialisatiecode opnemen in uw client moet onderzoeken.

Als u wilt werken met de bibliotheken die u moet eerst een ingang IoT Hub client toewijzen:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Houd er rekening mee dat geven we een kopie van onze verbindingsreeks apparaat aan deze functie (die we van het apparaat Explorer verkregen). We wijzen ook het protocol dat u wilt gebruiken. In dit voorbeeld wordt de AMQP maar MQTT en HTTP zijn ook een optie.

Als u hebt een geldige **IOTHUB\_CLIENT\_verwerken**, kunt u beginnen de gebeurtenissen verzenden en ontvangen van berichten van IoT Hub-API's aanroepen. Zullen we hierna.

### <a name="sending-events"></a>Gebeurtenissen verzenden

Gebeurtenissen verzenden naar IoT Hub is vereist dat u de volgende stappen:

Maak eerst een bericht:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Vervolgens wordt het bericht verzenden:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Telkens wanneer u een bericht verzendt, kunt u een verwijzing naar een callback-functie die wordt aangeroepen wanneer de gegevens worden verzonden opgeven:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Let op de aanroep van de **IoTHubMessage\_vernietigen** wanneer u klaar bent met het bericht. U moet bellen om vrij de resources verdeeld wanneer u het bericht hebt gemaakt.

### <a name="receiving-messages"></a>Ontvangen berichten

Het bericht is een asynchrone bewerking. Eerst registreert u een retouraanroep moet worden aangeroepen als het apparaat een bericht ontvangt:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

De laatste parameter is een void verwijzing naar wat u wilt. Een pointer naar een geheel getal is in het monster, maar is het mogelijk een pointer naar een meer complexe gegevensstructuur. Hierdoor wordt de callback-functie om te rijden op de gedeelde status met de aanroeper van deze functie.

Als het apparaat een bericht ontvangt, worden de geregistreerde callback-functie wordt aangeroepen:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

U gebruikt de **IoTHubMessage\_GetByteArray** de functie voor het ophalen van het bericht in het volgende voorbeeld een tekenreeks wordt.

### <a name="uninitializing-the-library"></a>De bibliotheek uninitializing

Als u klaar bent met het verzenden van gebeurtenissen en -berichten ontvangt, kunt u de bibliotheek IoT initialisatie. Hiertoe verlenen de volgende functie aanroepen:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Hierdoor komt de middelen die eerder door de **IoTHubClient\_CreateFromConnectionString** functie.

Zoals u ziet, is het gemakkelijk om gebeurtenissen te verzenden en ontvangen van berichten met de **IoTHubClient** -bibliotheek. De communicatie met IoT Hub, met inbegrip van de te gebruiken protocol gegevens verwerkt voor de bibliotheek (vanuit het perspectief van de ontwikkelaar, is dit een optie voor eenvoudige configuratie).

De **IoTHubClient** bibliotheek biedt ook een nauwkeurige controle over het serialiseren van de gebeurtenissen die uw apparaat met IoT Hub verzendt. In sommige gevallen is dit een voordeel, maar in andere gevallen is dit een implementatie details die u niet wilt worden betrokken. Als dit het geval is, kunt u overwegen met behulp van de **serializer** library, die in de volgende sectie beschreven.

## <a name="serializer"></a>Serializer

In dat geval de **serializer** library bevindt zich bovenaan in de bibliotheek **IoTHubClient** in de SDK. De bibliotheek **IoTHubClient** wordt gebruikt voor de onderliggende communicatie met IoT Hub, maar modellering mogelijkheden die de last van het omgaan met serialisatie bericht uit de ontwikkelaar verwijderen worden toegevoegd. Hoe deze bibliotheek werkt best wordt geïllustreerd door een voorbeeld.

Binnen de **serializer** map in de opslagplaats azure-iot-SDK's is een map **monsters** met een toepassing met de naam **simplesample\_amqp**. De Windows-versie van dit voorbeeld bevat de volgende Visual Studio-oplossing:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Net als bij het vorige voorbeeld bevat deze een verschillende NuGet pakketten:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

We de meeste in het vorige voorbeeld hebt gezien, maar **Microsoft.Azure.IoTHub.Serializer** is nieuw. Dit is nodig wanneer we de **serializer** library gebruikt.

U vindt de uitvoering van de voorbeeldtoepassing in de **simplesample\_amqp.c** bestand.

In de volgende secties leest u de belangrijkste onderdelen van dit voorbeeld.

### <a name="initializing-the-library"></a>Het initialiseren van de bibliotheek

Als u wilt werken met de **serializer** -bibliotheek, moet u de initialisatie van de API's aanroepen:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Het aanroepen van de **serializer\_init** functie is een eenmalige oproep en wordt gebruikt om de onderliggende bibliotheek te initialiseren. Vervolgens roept u de **IoTHubClient\_CreateFromConnectionString** -functie, die de dezelfde als in het monster **IoTHubClient** -API. Dit gesprek wordt de verbindingsreeks apparaat ingesteld (dit is ook waar u het protocol kiezen u wilt gebruiken). Opmerking in dit voorbeeld wordt AMQP gebruikt als het transport, maar dat kan zijn HTTP.

Tenslotte roept de **CREATE\_MODEL\_instantie** functie. Let erop dat **WeatherStation** de naamruimte van het model en de **ContosoAnemometer is** is de naam van het model. Als het exemplaar van het model is gemaakt, kunt u deze gebeurtenissen verzenden en ontvangen van berichten. Het is echter belangrijk te begrijpen wat een model is.

### <a name="defining-the-model"></a>Het model definiëren

Een model in de bibliotheek **serializer** definieert de gebeurtenis die het apparaat met IoT Hub en de *Acties* genoemd in de taal van modellering, die het kan ontvangen berichten kunt verzenden. U definieert een model met een verzameling C macro's als in de **simplesample\_amqp** -voorbeeldtoepassing:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

De **beginnen met\_NAAMRUIMTE** en **END\_NAAMRUIMTE** zowel macro's uitvoeren in de naamruimte van het model als een argument. De verwachting dat alles wat tussen deze macro's is de definitie van de modellen en gegevensstructuren die gebruikmaken van de modellen.

In dit voorbeeld is er één model **ContosoAnemometer**genoemd. Dit model definieert twee gebeurtenissen die uw apparaat naar IoT Hub verzenden kunt: **DeviceId** en **windsnelheid**. Het definieert eveneens drie acties (berichten) die uw apparaat kan ontvangen: **TurnFanOn**, **TurnFanOff**en **SetAirResistance**. Elke gebeurtenis heeft een type en elke actie heeft een naam (en eventueel een set parameters).

Gebeurtenissen en acties die zijn gedefinieerd in het model definiëren een oppervlak API kunt u gebeurtenissen verzenden met IoT Hub, alsmede reageren op berichten die worden verzonden naar het apparaat. Dit is het best begrijpen door middel van een voorbeeld.

### <a name="sending-events"></a>Gebeurtenissen verzenden

Het model definieert de gebeurtenis die u naar IoT Hub verzenden kunt. In dit voorbeeld betekent dat een van de twee gebeurtenissen die zijn gedefinieerd met behulp van de macro **WITH_DATA** . Bijvoorbeeld als u wilt dat een gebeurtenis **windsnelheid** verzenden naar een hub IoT, zijn er een paar stappen betrokken bij het maken van dit gebeuren. De eerste is het instellen van de gegevens die we willen verzenden:

```
myWeather->WindSpeed = 15;
```

Het model dat we eerder hebt gedefinieerd, kunnen wij om dit te doen door een lid van een **struct**. We serialiseren vervolgens de gebeurtenis die we wilt verzenden:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Deze code serialiseert de gebeurtenis aan een buffer (waarnaar wordt verwezen door de **bestemming**). We sturen de gebeurtenis ten slotte naar IoT hub met deze code:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Dit is een Help-functie in de voorbeeldtoepassing die onze geserialiseerde gebeurtenis naar IoT Hub verzendt:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Deze code is vergelijkbaar met wat we zagen de **iothub\_client\_monster\_amqp** toepassing, waarin we gemaakt een bericht uit een matrix van bytes en vervolgens **IoTHubClient\_SendEventAsync** naar IoT Hub te verzenden. Na dat we zojuist hebben om de greep bericht vrij en geserialiseerd gegevensbuffer die we eerder toegewezen.

De tweede naar de laatste parameter van **IoTHubClient\_SendEventAsync** is een verwijzing naar een callback-functie die wordt aangeroepen wanneer de gegevens verzonden. Hier volgt een voorbeeld van een callback-functie:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

De tweede parameter is een aanwijzer naar de gebruikerscontext; de dubbele pijl we doorgegeven aan **IoTHubClient\_SendEventAsync**. In dit geval de context van een simpele teller is kan, maar alles wat die u wilt.

Dat is alles is met het verzenden van gebeurtenissen. Het enige wat om te dekken is het ontvangen van berichten.

### <a name="receiving-messages"></a>Ontvangen berichten

Het bericht werkt op dezelfde manier op de manier waarop berichten in de bibliotheek **IoTHubClient werken** . Ten eerste kunt u een bericht callback-functie registreren:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Schrijft u vervolgens de callback-functie die wordt aangeroepen wanneer een bericht wordt ontvangen:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Deze code wordt de standaardtekst, dat is hetzelfde voor elke oplossing. Deze functie ontvangt het bericht en zorgt voor het bewerkingsplan aan de juiste functie via het aanroepen van **uitvoeren\_opdracht**. De aangeroepen functie is op dit moment is afhankelijk van de definitie van de acties in ons model.

Bij het definiëren van een actie in het model wordt vereist voor het implementeren van een functie die wordt aangeroepen wanneer het apparaat het bijbehorende bericht ontvangt. Bijvoorbeeld, als uw model definieert deze actie:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Een functie met deze handtekening, moet u definiëren:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Noteer de naam van de actie in het model komt overeen met de naam van de functie en de parameters voor de actie komen overeen met de parameters van de functie. De eerste parameter is altijd verplicht en bevat een verwijzing naar het exemplaar van het model.

Als het apparaat een bericht dat overeenkomt met deze handtekening ontvangt, wordt de bijbehorende functie aangeroepen. In combinatie met op te nemen van de generieke code uit **IoTHubMessage**, is ontvangen van berichten daarom slechts een kwestie van een eenvoudige functie voor elke actie die is gedefinieerd in het model te definiëren.

### <a name="uninitializing-the-library"></a>De bibliotheek uninitializing

Als u klaar bent met het verzenden van gegevens en berichten ontvangt, kunt u de bibliotheek IoT initialisatie:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Elk van deze drie functies worden uitgelijnd met de drie initialisatie functies die eerder zijn beschreven. Deze API's aanroepen zorgt ervoor dat u eerder toegewezen middelen vrijmaken.

## <a name="next-steps"></a>Volgende stappen

In dit artikel betrekking op de basisbeginselen van het gebruik van de bibliotheken in de **Azure IoT apparaat SDK voor C**. Zij voorzien u voldoende informatie om te begrijpen wat wordt opgenomen in de SDK, de architectuur en het werken met de Windows-monsters aan de slag. De beschrijving van de SDK blijft volgend artikel duidelijk te maken [over de IoTHubClient-bibliotheek](iot-hub-device-sdk-c-iothubclient.md).

Zie voor meer informatie over het ontwikkelen voor IoT Hub, de [IoT Hub SDK's][lnk-sdks].

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
