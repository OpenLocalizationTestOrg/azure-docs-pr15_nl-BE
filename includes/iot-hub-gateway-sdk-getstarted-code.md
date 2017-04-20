## <a name="typical-output"></a>Normale uitvoer

Hieronder ziet u een voorbeeld van de uitvoer naar het logboekbestand geschreven door het voorbeeld HelloWorld. Nieuwe regel en Tab tekens zijn voor de leesbaarheid toegevoegd:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Codefragmenten

In deze sectie worden enkele belangrijke delen van de code in het voorbeeld HelloWorld besproken.

### <a name="gateway-creation"></a>Gateway maken

De ontwikkelaar moet de *gateway-proces*te schrijven. Dit programma maakt de interne infrastructuur (broker), laadt de modules en wordt alles goed te laten functioneren. De SDK bevat de functie **Gateway_Create_From_JSON** om te bootstrappen gateway uit een JSON-bestand. Voor het gebruik van de functie **Gateway_Create_From_JSON** moet u doorgeven dat het pad naar een JSON-bestand waarin de modules worden geladen. 

Vindt u de code voor het proces van de gateway in het voorbeeld HelloWorld in de [main.c] [ lnk-main-c] bestand. Voor leesbaarheid ziet het fragment hieronder u een verkorte versie van de code gateway proces. Dit programma maakt een gateway en vervolgens wacht tot de gebruiker op de **ENTER** -toets drukken voordat het breekt u de gateway. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

De JSON-instellingenbestand bevat een lijst met modules worden geladen. Elke module moet a: opgeven

- **module_name**: een unieke naam voor de module.
- **module_path**: het pad naar de bibliotheek met de module. Linux heeft de .so, op Windows is dit een DLL-bestand.
- **args**: alle configuratie-informatie de module moet.

De JSON-bestand bevat ook de koppelingen tussen de modules die worden doorgegeven aan de makelaar. Een koppeling heeft twee eigenschappen:
- **bron**: de naam van een module van de `modules` sectie, of "\*".
- **sink**: de naam van een module van de `modules` sectie

Elke koppeling definieert een bericht route en richting. Berichten van de module `source` moeten worden geleverd aan de module `sink`. De `source` kan worden ingesteld op "\*", die aangeeft dat de berichten vanuit elke willekeurige module zal worden ontvangen door `sink`.

In het volgende voorbeeld wordt het bestand JSON-instellingen configureren in het voorbeeld HelloWorld op Linux. Elk bericht dat wordt geproduceerd door de module `hello_world` zal worden gebruikt door een module `logger`. Een module of vereist dat een argument is afhankelijk van het ontwerp van de module. In dit voorbeeld wordt kent de module logger gebruikt een argument is het pad naar het uitvoerbestand en de module Hallo wereld geen argumenten:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Hello World module bericht publiceren

U vindt de code die wordt gebruikt door de module 'hello world' berichten publiceren in de ['hello_world.c'] [ lnk-helloworld-c] bestand. Het fragment hieronder ziet u een nieuwe versie met extra opmerkingen en sommige foutafhandelingscode voor leesbaarheid verwijderd:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Hello World module verwerking

De module Hallo wereld moet nooit voor het verwerken van alle berichten die in andere modules met de makelaar publiceren. Hierdoor kunt u uitvoering van de callback bericht in de module Hallo wereld geen functie.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Logger module bericht publiceren en verwerking

De Logger module vervolgens berichten ontvangt van de broker en schrijft deze naar een bestand. Alle berichten wordt nooit gepubliceerd. De code van de module logger roept daarom nooit de functie **Broker_Publish** .

De functie **Logger_Recieve** in de [logger.c] [ lnk-logger-c] -bestand is de retouraanroep de broker roept de module logboek berichten worden afgeleverd. Het fragment hieronder ziet u een nieuwe versie met extra opmerkingen en sommige foutafhandelingscode voor leesbaarheid verwijderd:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van de Gateway IoT SDK, Zie de volgende:

- [IoT Gateway SDK-apparaat-wolk-berichten verzenden met een gesimuleerde apparaat gebruik Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK] [ lnk-gateway-sdk] op GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md