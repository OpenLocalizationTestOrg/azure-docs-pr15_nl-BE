<properties
    pageTitle="Azure IoT apparaat SDK voor C - Serializer | Microsoft Azure"
    description="Meer informatie over het gebruik van de Serializer library in de Azure IoT apparaat SDK voor C"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT apparaat SDK voor C – meer informatie over serializer

Het [eerste artikel](iot-hub-device-sdk-c-intro.md) in deze serie de **Azure IoT apparaat SDK voor C**geïntroduceerd. Het volgende artikel verstrekt een meer gedetailleerde beschrijving van de [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). In dit artikel is de dekking van de SDK door middel van een meer gedetailleerde omschrijving van het overige onderdeel voltooid: de **serializer** library.

De inleidende artikel beschreven hoe de **serializer** library gebruiken om gebeurtenissen te verzenden en ontvangen van IoT Hub. In dit artikel verlengen we die discussie door middel van een meer volledige uitleg over hoe u uw gegevens met de taal van de macro **serializer** model. Het artikel ook bevat meer details over hoe de bibliotheek berichten serialiseert (en in sommige gevallen hoe u kunt bepalen het gedrag serialisatie). Sommige parameters die kunt u bepalen de grootte van de modellen die u maakt ook beschreven.

Ten slotte, het artikel revisits sommige onderwerpen worden besproken in de vorige artikelen zoals bericht en verwerking van de eigenschap. Als we leert, werken deze functies op dezelfde manier met de **serializer** library als met de **IoTHubClient** -bibliotheek.

Alles dat wordt beschreven in dit artikel is gebaseerd op de **serializer** SDK-voorbeelden. Als u volgen wilt, Zie het **simplesample\_amqp** en **simplesample\_http** -toepassingen van het apparaat Azure IoT SDK voor C.

U kunt de **Azure IoT apparaat SDK voor C** vinden in de bibliotheek van [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub en details van de API in de [C-API reference](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)bekijken.

## <a name="the-modeling-language"></a>De taal van de modellering

De [inleidende artikel](iot-hub-device-sdk-c-intro.md) in deze serie geïntroduceerd **Azure IoT apparaat SDK voor C** modelleertaal via het voorbeeld in de **simplesample\_amqp** toepassing:

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

Zoals u ziet, wordt de taal van het model is gebaseerd op C macro's. Beginnen altijd met de definitie van **BEGIN\_NAAMRUIMTE** en altijd eindigt met **END\_NAAMRUIMTE**. Het is gebruikelijk als naam voor de naamruimte voor uw bedrijf of, zoals in dit voorbeeld wordt het project waaraan u werkt.

Wat gaat binnen de naamruimte zijn definities. In dit geval is er één model voor een anemometer. Nogmaals, het model kan een betekenisvolle naam geven, maar meestal deze voor het apparaat of het type gegevens dat u wilt uitwisselen met IoT Hub wordt genoemd.  

Modellen bevatten een definitie van de gebeurtenissen kunt u ingress met IoT Hub ( *gegevens*) en de berichten die u van IoT Hub ( *acties ontvangen kunt*). Als u in het voorbeeld zien kunt, hebben gebeurtenissen een type en een naam; acties hebben een naam en optionele parameters (elk met een type).

Wat niet in dit voorbeeld wordt aangetoond, zijn extra gegevenstypen die worden ondersteund door de SDK. Hierna komen.

> [AZURE.NOTE] IoT Hub verwijst naar de gegevens die een apparaat als *gebeurtenissen*, stuurt terwijl de taal van het model naar deze *gegevens verwijst* (gedefinieerd met behulp van **WITH_DATA**). IoT Hub verwijst ook naar de gegevens die u naar apparaten als *berichten*, verzendt terwijl de taal van het model naar het als *Acties verwijst* (gedefinieerd met behulp van **WITH_ACTION**). Vergeet niet dat deze termen door elkaar kunnen worden gebruikt in dit artikel.

### <a name="supported-data-types"></a>Ondersteunde gegevenstypen

De volgende gegevenstypen worden ondersteund in de modellen die zijn gemaakt met de **serializer** library:

| Type                    | Beschrijving                            |
|-------------------------|----------------------------------------|
| dubbel                  | dubbele precisiegetal met drijvende komma |
| int                     | 32-bits geheel getal                         |
| Float                   | getal met drijvende komma enkele precisie |
| lange                    | lange integer                           |
| int8\_t                 | 8-bits geheel getal                          |
| Int16\_t                | 16-bits geheel getal                         |
| Int32\_t                | 32-bits geheel getal                         |
| Int64\_t                | 64-bits geheel getal                         |
| BOOL                    | Boole-waarde                                |
| ASCII\_char\_ptr        | ASCII-reeks                           |
| EDM\_datum\_keer\_OFFSET | datum tijd verschuiving                       |
| EDM\_GUID               | GUID                                   |
| EDM\_binaire             | binair                                 |
| DECLAREER\_STRUCT         | complexe gegevens                      |

Laten we beginnen met het laatste gegevenstype. De **DECLARE\_STRUCT** kunt u complexe gegevenstypen, dat wil zeggen de primitieve typen groepen definiëren. Deze groeperingen kunnen we definiëren een model dat er als volgt uit:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Ons model bevat een enkele gebeurtenis van het type **TestType**. **TestType** is een complex met verschillende leden die collectief aantonen primitieve typen worden ondersteund door de **serializer** modelleertaal.

Met een model als volgt kunnen we schrijven code om gegevens te verzenden met IoT-Hub die er als volgt uitziet:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

We zijn in principe een waarde toewijst aan elk lid van de structuur van de **Test** en vervolgens roepen **SendAsync** voor het verzenden van de gebeurtenis **toets** naar de cloud. **SendAsync** is een helperfunctie die een enkele gebeurtenis naar IoT Hub verzendt:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Deze functie de opgegeven gebeurtenis serialiseert en verzendt dit naar de IoT Hub met **IoTHubClient\_SendEventAsync**. Dit is dezelfde code als beschreven in de vorige artikelen (**SendAsync** kapselt de logica in een handige functie).

Een andere helperfunctie die wordt gebruikt in de vorige code is **GetDateTimeOffset**. Deze functie worden de gegeven tijd omgezet in een waarde van het type **EDM\_datum\_keer\_OFFSET**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Als u deze code uitvoert, wordt het volgende bericht verzonden naar de IoT Hub:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Houd er rekening mee dat de serialisatie is JSON, de indeling die wordt gegenereerd door de **serializer** library. Vergeet niet dat elk lid van de geserialiseerde JSON-object komt overeen met de leden van het **TestType** die we in onze model worden gedefinieerd. De waarden ook exact overeenkomen met die welke worden gebruikt in de code. Bedenk wel dat de binaire gegevens gecodeerd met base64 wordt: 'AQID' is de base64-codering van {0x01, 0x02, 0x03}.

In dit voorbeeld wordt het voordeel van de **serializer** library--Hierdoor kunnen wij JSON verzenden naar de cloud, zonder dat expliciet omgaan met serialisatie in onze toepassing. Alle hebben we bang is de waarden van de Gegevensgebeurtenissen instellen in ons model en vervolgens de gebeurtenissen die worden verzonden naar de cloud-eenvoudige API's roepen.

Met deze gegevens definiëren we modellen die het bereik van de ondersteunde gegevenstypen, met inbegrip van complexe typen (we kunnen zelfs complexe typen binnen andere complexe typen bevatten) bevatten. Echter, hij geserialiseerd JSON gegenereerd door het bovenstaande voorbeeld wordt een belangrijk punt. Bepaalt *hoe* we verzenden gegevens met de **serializer** library precies hoe de JSON is gevormd. Dat bepaalde punt is wat komen volgende.

## <a name="more-about-serialization"></a>Meer informatie over serialisatie

De vorige sectie markeert een voorbeeld van de uitvoer gegenereerd door de bibliotheek **serializer** . In deze sectie leggen we hoe de bibliotheek serialiseert gegevens en hoe u kunt bepalen dat gedrag met behulp van de API's van serialisatie.

Om door te gaan in de discussie over serialisatie, zullen wij samenwerken met een nieuw model op basis van een thermostaat. Ten eerste bieden we enkele achtergrond op het scenario hebben we geprobeerd om het adres.

We willen als model voor een thermostaat die meet de temperatuur en vochtigheid. Elk gedeelte van de gegevens gaat worden verzonden naar de IoT Hub anders. Standaard wordt de ingresses van de thermostaat een gebeurtenis temperatuur elke 2 minuten; een gebeurtenis vochtigheid is ingressed om de 15 minuten. Wanneer een gebeurtenis ingressed is, moet een tijdstempel dat wordt de tijd weergegeven in de bijbehorende temperatuur of vochtigheid werd gemeten bevatten.

Gezien dit scenario, gaat ingegaan op twee verschillende manieren om de gegevens te modelleren en leggen we het effect dat model op de geserialiseerde uitvoer heeft.

### <a name="model-1"></a>Model 1

Dit is de eerste versie van een model dat in het vorige scenario ondersteunt:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Opmerking dat het model twee Gegevensgebeurtenissen bevat: **temperatuur** en **vochtigheid**. In tegenstelling tot de vorige voorbeelden is het type van elke gebeurtenis een structuur die is gedefinieerd met behulp van **DECLARE\_STRUCT**. **TemperatureEvent** bevat een temperatuurmeting en een tijdstempel; **HumidityEvent** bevat een vochtigheid meting en een tijdstempel. Dit model biedt ons een natuurlijke manier als model voor de gegevens voor het bovenstaande scenario. Als we een gebeurtenis naar de cloud verzendt, sturen ofwel wij een temperatuur/tijdstempel of een combinatie van vochtigheid/tijdstempel.

We kunt een gebeurtenis temperatuur verzenden naar de cloud met bijvoorbeeld de volgende code:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

We zullen hard gecodeerde waarden gebruiken voor temperatuur en vochtigheid in de voorbeeldcode, maar Stel dat we feitelijk deze waarden ophaalt door bemonstering van de overeenkomstige sensoren op de thermostaat.

De bovenstaande code maakt gebruik van het hulpprogramma voor **GetDateTimeOffset** die eerder werd geïntroduceerd. Om redenen die duidelijk hoger, scheidt u deze code expliciet de taak van het serialiseren van en het verzenden van de gebeurtenis. De vorige code serialiseert de temperatuur gebeurtenis in een buffer. **SendMessage** is een Help-functie (opgenomen in **simplesample\_amqp**) die de gebeurtenis met IoT Hub verzendt:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Deze code is een subset van de helper **SendAsync** is beschreven in de vorige sectie, zodat we won't gaan over het weer hier.

Wanneer we de vorige code voor het verzenden van de gebeurtenis temperatuur uitvoert, wordt dit formulier serienummer van de gebeurtenis verzonden naar IoT Hub:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

We bij het verzenden van een temperatuur van het type **TemperatureEvent** en die struct bevat een lid van de **temperatuur** en **tijd** . Dit wordt direct doorgevoerd in de geserialiseerde gegevens.

Op dezelfde manier kan een gebeurtenis vochtigheid met deze code worden verzonden:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

De geserialiseerde formulier dat wordt verzonden naar de IoT Hub wordt als volgt weergegeven:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Nogmaals, dit is zoals verwacht.

Met dit model, kunt u zich voorstellen hoe meer gebeurtenissen kunnen eenvoudig worden toegevoegd. U definieert meer structuren met behulp van **DECLARE\_STRUCT**, en de bijbehorende gebeurtenis opgenomen in het model met **met\_gegevens**.

Nu gaan we het aanpassen zodat het dezelfde gegevens bevat, maar met een andere structuur.

### <a name="model-2"></a>Model 2

Bekijk dit alternatief model een:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

In dit geval we hebt geëlimineerd de **DECLARE\_STRUCT** macro's en gewoon de gegevensitems in ons scenario met behulp van eenvoudige typen van de modellering taal definieert.

Voor het moment dat we negeren **de tijdsgebeurtenis** . Met deze braakgelegde is hier de code voor ingress **temperatuur**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Deze code wordt de volgende gebeurtenis geserialiseerde verzendt naar IoT Hub:

```
{"Temperature":75}
```

En de code voor het verzenden van de gebeurtenis vochtigheid wordt als volgt weergegeven:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Deze code stuurt dit naar IoT Hub:

```
{"Humidity":45}
```

Er zijn tot nu toe nog geen verrassingen. Nu gaan we veranderen hoe we de macro GESERIALISEERD gebruiken.

De macro **GESERIALISEERD** kunt u meerdere Gegevensgebeurtenissen nemen als argumenten. Dit stelt ons in staat de **temperatuur** en **vochtigheid** gebeurtenis samen serialiseren en stuur ze naar IoT Hub in één aanroep:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

U kan raden dat het resultaat van deze code is dat twee gebeurtenissen worden verzonden met IoT Hub:

[

{'Temperature': 75},

{"Vochtigheid": 45}

]

Met andere woorden, zou u verwachten dat deze code hetzelfde is als het verzenden van **temperatuur** en **vochtigheid** afzonderlijk. Het is gewoon gemakkelijker te maken voor het doorgeven van beide gebeurtenissen aan **GESERIALISEERD** in dezelfde aanroep. Dat is echter niet het geval. De bovenstaande code wordt in plaats daarvan deze gegevensgebeurtenis enkele verzendt naar IoT Hub:

{'Temperature': 75, "vochtigheid": 45}

Dit lijkt vreemd omdat ons model voor **temperatuur** en **vochtigheid** wordt gedefinieerd als twee *afzonderlijke* gebeurtenissen:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Niet model meer naar het punt, deze gebeurtenissen waar de **temperatuur** en **vochtigheid** in dezelfde structuur zijn we:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Als wij dit model gebruikt, is het eenvoudiger te begrijpen hoe de **temperatuur** en **vochtigheid** in hetzelfde serienummer bericht zou worden verzonden. Echter deze mogelijk niet duidelijk waarom het op die manier werkt wanneer u beide Gegevensgebeurtenissen doorgeeft aan een **GESERIALISEERD** met behulp van model 2.

Dit is gemakkelijker te begrijpen als u weet dat de veronderstellingen die de **serializer** library. Om dit gevoel eens terug naar ons model:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Dit model in de object-georiënteerde context beschouwen. In dit geval we bij het modelleren van een fysiek apparaat (thermostaat) en dat apparaat bevat kenmerken zoals **temperatuur** en **vochtigheid**.

De gehele staat van ons model met bijvoorbeeld de volgende code kan worden verzonden:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ervan uitgaande dat de waarden van de temperatuur, vochtigheid en tijd zijn ingesteld, zien we een gebeurtenis als deze verzonden naar IoT Hub:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

U kunt soms slechts *enkele* eigenschappen van het model te verzenden naar de cloud (dit geldt met name als het model een groot aantal Gegevensgebeurtenissen bevat). Is het handig om alleen een subset van Gegevensgebeurtenissen, zoals in het eerdere voorbeeld verzenden:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Dit genereert precies dezelfde geserialiseerde gebeurtenis als we hadden een **TemperatureEvent** gedefinieerd met een lid van **temperatuur** en **tijd** , net zoals wij gedaan met model 1. In dit geval kon precies dezelfde geserialiseerde gebeurtenis genereren met behulp van een ander model (model 2), omdat we een **GESERIALISEERD** op een andere manier aangeroepen.

Het belangrijkste is dat als u meerdere Gegevensgebeurtenissen aan **GESERIALISEERD, doorgeeft** dan wordt aangenomen dat elke gebeurtenis is een eigenschap in een JSON-object.

De beste aanpak is afhankelijk van en hoe u denkt over uw model. Als u 'events' naar de cloud verzendt en elke gebeurtenis een gedefinieerde set eigenschappen bevat, wordt de eerste benadering zeer geschikt. In dat geval gebruikt u **DECLARE\_STRUCT** definiëren de structuur van elke gebeurtenis en plaatst deze in het model met de **met\_DATA** macro. Vervolgens sturen elke gebeurtenis zoals we in het eerste voorbeeld hierboven. In deze benadering zou u alleen een enkele gebeurtenis aan **SERIALIZER**doorgeven.

Als u over uw model op een object-georiënteerde manier denkt, vervolgens de tweede aanpak kan naar eigen inzicht. In dit geval de elementen gedefinieerd met behulp van **met\_DATA** zijn 'Eigenschappen' van je object. U geeft welke subset van gebeurtenissen aan **GESERIALISEERD** die u bevalt, afhankelijk van hoeveel van uw "object" staat die u wilt verzenden naar de cloud.

Nether benadering is goed of fout. Alleen rekening houden met de werking van de bibliotheek **serializer** en kies vervolgens de modellering benadering die het beste past bij uw behoeften.

## <a name="message-handling"></a>Afhandeling van berichten

Tot nu toe dit artikel alleen verzenden gebeurtenissen met IoT Hub heeft besproken en ontvangen van berichten niet is opgelost. De reden hiervoor is dat we moeten weten over het ontvangen van berichten grotendeels in een [eerder artikel gedekt wordt](iot-hub-device-sdk-c-intro.md). Intrekken van dat artikel dat u berichten verwerken door te registreren een callback-functie van het bericht:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

U schrijft vervolgens de callback-functie die wordt aangeroepen wanneer een bericht wordt ontvangen:

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

Deze implementatie van **IoTHubMessage** wordt de specifieke functie voor elke actie in uw model. Bijvoorbeeld, als uw model definieert deze actie:

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

**SetAirResistance** wordt vervolgens aangeroepen wanneer dat bericht wordt verzonden naar uw apparaat.

Wat we nog niet uitgelegd is wat de geserialiseerde versie van het bericht ziet er nog. Met andere woorden, als u wilt dat een **SetAirResistance** -bericht te verzenden naar uw apparaat, hoe ziet dat eruit?

Als u een bericht naar een apparaat verzendt, zou u dat doen via de IoT Azure service SDK. Moet u toch weten welke tekenreeks om te verzenden naar een bepaalde actie oproepen. De algemene notatie voor het verzenden van een bericht wordt weergegeven als volgt:

```
{"Name" : "", "Parameters" : "" }
```

U verzendt een geserialiseerd object JSON met twee eigenschappen: **Dit is de naam van de actie (bericht)** en **Parameters** bevat de parameters voor die actie.

Als u wilt aanroepen van **SetAirResistance** kunt u dit bericht kunt verzenden naar een apparaat:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

De naam van de actie moet precies overeenkomen met een actie in het model worden gedefinieerd. De parameternamen moeten ook overeenkomen. Let ook op hoofdletters en kleine letters. **Naam** en **Parameters** zijn altijd hoofdletters. Zorg ervoor dat overeenkomt met de naam van de actie en parameters in het model. In dit voorbeeld wordt de naam van de actie 'SetAirResistance' en niet 'setairresistance'.

In deze sectie wordt alles wat die u moet weten wanneer gebeurtenissen verzenden en ontvangen berichten met de **serializer** library beschreven. Voordat u doorgaat, zullen we dekken sommige kunt u parameters die bepalen hoe groot uw model is.

## <a name="macro-configuration"></a>Macro-configuratie

Als u gebruikmaakt van de bibliotheek **Serializer** is een belangrijk onderdeel van de SDK van gevonden in de bibliotheek azure-c-gedeeld-hulpprogramma.
Als u de bibliotheek Azure-iot-SDK's uit met de optie--recursieve GitHub hebt gekopieerd, wordt u dit gedeelde hulpprogrammabibliotheek hier vinden:

```
.\\c\\azure-c-shared-utility
```

Als u de bibliotheek niet hebben gekloond, vindt u deze [hier](https://github.com/Azure/azure-c-shared-utility).

In de hulpprogrammabibliotheek gedeelde vindt u de volgende map:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Deze map bevat een Visual Studio-oplossing genoemd **macro\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Gegenereerd door het programma in deze oplossing de **macro\_utils.h** bestand. Er is een standaardmacro\_utils.h bestand van de SDK. Deze oplossing kunt u sommige parameters wijzigen en vervolgens opnieuw maken op basis van deze parameters het header-bestand.

De twee belangrijkste parameters worden betrokken bij zijn **nArithmetic** en **nMacroParameters** zijn gedefinieerd in de volgende twee regels in de macro\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Deze waarden zijn de standaardparameters die bij de SDK worden geleverd. Elke parameter heeft de volgende betekenis:

-   nMacroParameters – bepaalt hoeveel parameters die u kunt hebben in een DECLARE\_MODEL macro definiëren.

-   nArithmetic – bepaalt het totaal aantal leden zijn toegestaan in een model.

De reden dat deze parameters belangrijk zijn is omdat zij bepalen hoe groot uw model. Neem bijvoorbeeld de modeldefinitie van dit:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Zoals eerder vermeld **DECLARE\_MODEL** is gewoon een macro C. De namen van het model en de **met\_DATA** -instructie (nog een andere macro) worden de parameters van **DECLARE\_MODEL**. **nMacroParameters** bepaalt hoeveel parameters kunnen worden opgenomen in **DECLARE\_MODEL**. Hiermee wordt effectief, hoeveel gegevens gebeurtenis en actie declaraties u hebt. Als zodanig met de standaardlimiet van 124 betekent dit dat u een model met een combinatie van ongeveer 60 acties en evenementen van de gegevens kunt definiëren. Als u deze limiet overschrijdt, ontvangt u compilerfouten die er ongeveer zo:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

De parameter **nArithmetic** is meer over de interne werking van de macrotaal dan uw toepassing.  Hiermee kunt u het totale aantal leden die het model, waaronder **DECLARE_STRUCT** macro's kan bevatten. Als u compilerfouten zoals dit te zien start, moet u proberen **nArithmetic**verhogen:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Als u deze parameters wijzigt, wijzigt u de waarden in de macro\_utils.tt bestand, opnieuw compileren van de macro\_utils\_h\_generator.sln-oplossing en het gecompileerde programma start. Als u dit doet, een nieuwe macro\_utils.h-bestand is gegenereerd en worden in de. \\algemene\\inc directory.

Om de nieuwe versie van de macro\_utils.h, de **serializer** NuGet package verwijderen uit uw oplossing en in plaats daarvan omvatten de **serializer** Visual Studio-project. Hierdoor wordt uw code te compileren met de broncode van de serializer library. Dit geldt ook voor de bijgewerkte macro\_utils.h. Als u dit doen wilt voor **simplesample\_amqp**, begin met het verwijderen van het pakket NuGet voor de serializer-bibliotheek van de oplossing:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Dit project vervolgens toevoegen aan de Visual Studio-oplossing:

> . \\c\\serialisatiefunctie\\bouwen\\windows\\serializer.vcxproj

Wanneer u bent klaar, uw oplossing ziet er zo uit:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Nu wanneer het compileren van uw oplossing, de bijgewerkte macro\_utils.h is opgenomen in de binaire notatie.

Houd er rekening mee dat deze waarden hoog genoeg verhogen compiler overschrijden kan. Tot op dit moment is de **nMacroParameters** de belangrijkste parameter te worden betrokken. De C99-specificatie geeft aan dat een minimum van 127 parameters zijn toegestaan in een macro definiëren. Het compileerprogramma van Microsoft de spec precies volgt (en heeft een limiet van 127), zodat u niet meer **nMacroParameters** dan de standaardwaarde te verhogen. Andere compilers kunt u mogelijk om dit te doen (bijvoorbeeld de GNU-compiler ondersteunt een hogere limiet).

Tot dusver hebben we vrijwel alles wat die u moet weten over het schrijven van code met de bibliotheek **serializer** behandeld. Vóór de sluiting van, laten we terugkeren naar bepaalde onderwerpen uit de voorgaande artikelen die u vraagt zich misschien af over.

## <a name="the-lower-level-apis"></a>De API's van lager niveau

De voorbeeldtoepassing waarop dit artikel gericht is **simplesample\_amqp**. In dit voorbeeld gebruikt het op een hoger niveau (de niet-' Alles ') API's om gebeurtenissen te verzenden en ontvangen van berichten. Als u deze API's gebruikt, wordt een thread op de achtergrond uitgevoerd die zorgt voor beide gebeurtenissen verzenden en ontvangen van berichten. Echter, kunt u de API's van lager niveau (LL) te elimineren deze thread op de achtergrond en expliciete controle over wanneer u gebeurtenissen verzenden of ontvangen van berichten vanuit de cloud.

Er is een set functies die uit de API's van een hoger niveau bestaat, zoals beschreven in het [vorige artikel](iot-hub-device-sdk-c-iothubclient.md):

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_vernietigen

Deze API's worden aangetoond **simplesample\_amqp**.

Er is ook een andere soortgelijke set API's van lager niveau.

-   IoTHubClient\_ES\_CreateFromConnectionString

-   IoTHubClient\_ES\_SendEventAsync

-   IoTHubClient\_ES\_SetMessageCallback

-   IoTHubClient\_ES\_vernietigen

Houd er rekening mee dat de API's van lager niveau werken op dezelfde manier als beschreven in de vorige artikelen. U kunt de eerste set van API's als u wilt dat een achtergrondthread gebeurtenissen verzenden en ontvangen berichten worden verwerkt. U de tweede set van API's gebruiken als u wilt dat expliciete controle over wanneer u gegevens verzenden en ontvangen van IoT Hub. Een set API's werken even goed met de **serializer** library.

Zie voor een voorbeeld van het gebruik van de API's van lager niveau met de **serializer** library, de **simplesample\_http** toepassing.

## <a name="additional-topics"></a>Extra onderwerpen

Een aantal onderwerpen opgemerkt zijn opnieuw eigendom verwerking, het gebruik van alternatieve inloggegevens en configuratieopties. Dit zijn alle onderwerpen in een [Vorig artikel](iot-hub-device-sdk-c-iothubclient.md). Het belangrijkste punt is dat al deze functies op dezelfde manier met de **serializer** library werken als met de **IoTHubClient** -bibliotheek. Bijvoorbeeld, als u eigenschappen koppelen aan een gebeurtenis van het model wilt, u **IoTHubMessage\_eigenschappen** en **kaart**\_**AddorUpdate**, dezelfde zoals eerder beschreven:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Maakt niet uit of de gebeurtenis is gegenereerd op basis van de **serializer** library of handmatig met behulp van de bibliotheek **IoTHubClient** gemaakt.

Apparaat met alternatieve referenties met **IoTHubClient\_ES\_maken** net zo goed werkt als **IoTHubClient\_CreateFromConnectionString** voor het toewijzen van een **IOTHUB\_CLIENT\_verwerken**.

Als u de **serializer** library gebruikt, kunt u ten slotte met configuratie-opties instellen **IoTHubClient\_ES\_SetOption** net als toen met de **IoTHubClient** -bibliotheek.

Een functie die uniek is voor de **serializer** library worden de initialisatie van de API's. Voordat u kunt werken met de bibliotheek, moet u bellen **serializer\_init**:

```
serializer_init(NULL);
```

Dit wordt gedaan voordat u contact opneemt met **IoTHubClient\_CreateFromConnectionString**.

Op dezelfde manier als u klaar bent met de bibliotheek werken, de laatste aanroep u maakt is **serialisatiefunctie\_deinit**:

```
serializer_deinit();
```

Anders werken alle van de bovengenoemde functies in de bibliotheek **serializer** hetzelfde als in de bibliotheek **IoTHubClient** . Zie het [vorige artikel](iot-hub-device-sdk-c-iothubclient.md) in deze serie voor meer informatie over deze onderwerpen.

## <a name="next-steps"></a>Volgende stappen

Dit artikel beschrijft in detail de unieke aspecten van de **serializer** library opgenomen in de **Azure IoT apparaat SDK voor C**. Met de informatie verstrekt moet u beschikken over een goed inzicht in het gebruik van modellen voor gebeurtenissen berichten verzenden en ontvangen van IoT Hub.

Dit wordt ook de driedelige serie over het ontwikkelen van toepassingen met de **Azure IoT apparaat SDK voor C**. Dit moet genoeg informatie om niet alleen krijgt u aan de slag, maar geven u een gedegen kennis van de werking van de API's. Voor meer informatie zijn enkele voorbeelden in de SDK die hier niet wordt behandeld. Anders wordt is de [SDK-documentatie](https://github.com/Azure/azure-iot-sdks) een goede bron voor meer informatie.


Zie voor meer informatie over het ontwikkelen voor IoT Hub, de [IoT Hub SDK's][lnk-sdks].

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
