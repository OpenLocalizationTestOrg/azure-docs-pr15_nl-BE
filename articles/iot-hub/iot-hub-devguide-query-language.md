<properties
 pageTitle="Handleiding voor ontwikkelaars - querytaal | Microsoft Azure"
 description="Azure IoT Hub developer guide - beschrijving van de querytaal die wordt gebruikt voor het ophalen van informatie over twins, methoden en taken van uw hub IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Referentie - querytaal voor twins en taken

## <a name="overview"></a>Overzicht

IoT Hub biedt een krachtige SQL-achtige taal voor het ophalen van informatie met betrekking tot een [apparaat twins] [ lnk-twins] en [taken][lnk-jobs]. Dit artikel bevat:

* Een inleiding tot de belangrijkste voorzieningen van de querytaal IoT-Hub, en
* De gedetailleerde beschrijving van de taal.

## <a name="getting-started-with-twin-queries"></a>Aan de slag met twin-query 's

[Apparaat twins] [ lnk-twins] willekeurige JSON-objecten kan bevatten als labels en eigenschappen. IoT Hub kan query apparaat twins als één JSON-document met alle dubbele gegevens.
Stel bijvoorbeeld dat uw hub IoT twins de volgende structuur hebben:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT Hub stelt de twins apparaat als een documentverzameling **apparaten**genoemd.
De volgende query haalt zodat de hele reeks van twins apparaat:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT Hub SDK's] [ lnk-hub-sdks] ondersteunt paginering van grote resultaten.

IoT Hub kunt filteren met willekeurige voorwaarden twins ophalen. Bijvoorbeeld:

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

haalt de twins met de tag **location.region** is ingesteld op **ons**.
Boole-operators en rekenkundige vergelijkingen worden ondersteund, bijv.

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

alle twins zich bevindt in de VS is geconfigureerd voor het verzenden van telemetrie minder vaak elke minuut opgehaald. Gemakkelijker te maken is het ook mogelijk matrixconstanten gebruiken in combinatie met de **IN** - en operatoren **NIN** (niet in). Bijvoorbeeld:

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

haalt alle twins die gerapporteerd wifi of bekabelde verbinding. Raadpleeg de [WHERE-component] [ lnk-query-where] sectie voor de volledige verwijzing van de filtermogelijkheden.

Groepering en aggregaties worden ook ondersteund. Bijvoorbeeld:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Geeft als resultaat het aantal apparaten in elke configuratie telemetrie status.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

In het bovenstaande voorbeeld ziet u een situatie waarin drie apparaten geslaagde configuratie gemeld twee nog steeds de configuratie toepast en een heeft een fout gemeld.

### <a name="c-example"></a>C#-voorbeeld

De queryfunctionaliteit wordt weergegeven door de [C#-service SDK] [ lnk-hub-sdks] in de de klasse **RegistryManager** .
Hier volgt een voorbeeld van een eenvoudige query:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Let op hoe de **query** -object wordt gestart met een pagina (maximaal 1000) en vervolgens meerdere pagina's kunnen worden opgehaald door het aanroepen van de methoden **GetNextAsTwinAsync** meerdere keren.
Het is belangrijk te weten dat het queryobject meerdere bevat **volgende\***, afhankelijk van de optie deserialisatie is vereist door de query, dat wil zeggen twin of objecten of gewone Json moet worden gebruikt bij het gebruik van de uitstekende delen van taken.

### <a name="node-example"></a>Voorbeeld van knooppunt

De queryfunctionaliteit wordt weergegeven door het [knooppunt service SDK] [ lnk-hub-sdks] in de **Registry** -object.
Hier volgt een voorbeeld van een eenvoudige query:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Let op hoe de **query** -object wordt gestart met een pagina (maximaal 1000) en vervolgens meerdere pagina's kunnen worden opgehaald door het aanroepen van de methoden **nextAsTwin** meerdere keren.
Het is belangrijk te weten dat het queryobject meerdere bevat **volgende\***, afhankelijk van de optie deserialisatie is vereist door de query, dat wil zeggen twin of objecten of gewone Json moet worden gebruikt bij het gebruik van de uitstekende delen van taken.

### <a name="limitations"></a>Beperkingen

Op dit moment projecties worden alleen ondersteund als gebruik aggregaties, dat wil zeggen niet-samengevoegde query's kunnen alleen worden gebruikt `SELECT *`. Ook aggregatie worden alleen ondersteund in combinatie met de groepering.

## <a name="getting-started-with-jobs-queries"></a>Aan de slag met query's voor taken

[Taken] [ lnk-jobs] bieden een manier om uit te voeren bewerkingen op sets van apparaten. Elk apparaat twin bevat de gegevens van de taken die het onderdeel in een verzameling **taken**genoemd is.
Logisch,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Deze collectie is momenteel queriable als **devices.jobs** in de querytaal IoT Hub.

Bijvoorbeeld, als u alle taken (afgelopen en geplande) die van invloed zijn op een enkel apparaat, kunt u de volgende query:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Let op hoe deze query vindt u de status van apparaat-specifieke (en eventueel ook de reactie van de directe methode) van elke taak als resultaat gegeven.
Het is ook mogelijk om te filteren met willekeurige Booleaanse voorwaarden op alle eigenschappen van de objecten in de collectie **devices.jobs** .
Bijvoorbeeld de volgende query:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

alle voltooide twin update taken voor het apparaat **myDeviceId** die zijn gemaakt na September 2016 opgehaald.

Het is ook mogelijk om op te halen van de resultaten per apparaat van één taak.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Beperkingen
Op dit moment bieden query's op **devices.jobs** geen ondersteuning voor:

* Uitstekende delen, dat wil zeggen alleen `SELECT *` mogelijk is;
* Voorwaarden die betrekking op het apparaat twee eigenschappen van een taak hebben zoals hierboven;
* Peforming aggregaties, zoals count, avg, groeperen op.

## <a name="basics-of-an-iot-hub-query"></a>Basisbeginselen van een query IoT Hub

Elke query IoT Hub bestaat van een selectie van componenten en door optionele waar en GROEPEREN op componenten. Elke query wordt uitgevoerd op een collectie van JSON-documenten, zoals apparaat-twins. De FROM-component geeft de collectie van het document te worden herhaald op (**apparaten** of **devices.jobs**). Het filter in de WHERE-component wordt vervolgens toegepast. In het geval van aggregaties, de resultaten van deze stap worden gegroepeerd als opgegeven in de GROUP BY-component, en voor elke groep, een rij wordt gegenereerd zoals opgegeven in de SELECT-component.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM-component

De component **van < from_specification >** kan slechts twee waarden aannemen: **van apparaten**voor query apparaat twins of **van devices.jobs**om details van query per apparaat.

## <a name="where-clause"></a>WHERE-component

De **waarbij < filter_condition >** -component is optioneel. Deze geeft de voorwaarden die de JSON-documenten in de collectie van voldoen moeten om te worden opgenomen als onderdeel van het resultaat. Een JSON-document, moet de opgegeven voorwaarden op "true" moet worden opgenomen in het resultaat opleveren.

De toegestane voorwaarden worden beschreven in de sectie [expressies en voorwaarden][lnk-query-expressions].

## <a name="select-clause"></a>SELECT-component

De SELECT-component (**< select_list > Selecteer**) is verplicht en geeft aan welke waarden worden opgehaald uit de query. Deze geeft de JSON-waarden worden gebruikt voor het genereren van nieuwe JSON-objecten voor elk element van de subset van gefilterde (en eventueel gegroepeerd) van de collectie van de Projectiefase genereert een nieuw object van JSON, gebouwd met de waarden die zijn opgegeven in de SELECT-component.

Dit is de grammatica van de SELECT-component:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

waarbij **attribute_name** verwijst naar een eigenschap van het JSON-document in de FROM-collectie. Enkele voorbeelden van de SELECT-component kunnen u vinden in de [aan de slag met query's twin] [ lnk-query-getstarted] sectie.

Op dit moment selectie componenten dan andere **selecteren \* ** worden alleen ondersteund in de samenvoegquery op twins.

## <a name="group-by-clause"></a>GROUP BY-component

De component **GROUP BY < group_specification >** is een optionele stap die kan worden uitgevoerd nadat het filter dat is opgegeven in de WHERE-component, en vóór de opgegeven selecteert u in de projectie. Deze documenten op basis van de waarde van een kenmerk worden gegroepeerd. Deze groepen worden gebruikt voor het genereren van de geaggregeerde waarden zoals opgegeven in de SELECT-component.

Een voorbeeld van een query met GROUP BY is:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

De formele syntaxis voor GROUP BY is:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

waarbij **attribute_name** verwijst naar een eigenschap van het JSON-document in de FROM-collectie. 

GROUP BY-component wordt momenteel alleen ondersteund bij het opvragen van twins.

## <a name="expressions-and-conditions"></a>Expressies en voorwaarden

Op een hoog niveau, een *expressie*:

* Resulteert in een exemplaar van een JSON-type (d.w.z. Boolean, getal, tekenreeks, array, of object), en
* Wordt gedefinieerd door het manipuleren van gegevens die afkomstig zijn van het apparaat JSON-document en met behulp van ingebouwde operatoren en functies-constanten zijn.

*Voorwaarden* zijn expressies die resulteert in een Boolean-waarde, dat wil zeggen elke constante anders dan een Boole-waarde **true** als **de waarde false** (incl. **null**, **ongedefinieerde**, object of array-instantie, een willekeurige tekenreeks en duidelijk de Boolean **false**) wordt beschouwd.

De syntaxis voor expressies is:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

waar:

| Symbool | Definitie |
| -------------- | -----------------|
| attribute_name | Een eigenschap van de JSON-document in de collectie van. |
| unary_operator | Een unaire operator zoals bepaald in punt Operators. |
| binary_operator | Een binaire operator zoals bepaald in punt Operators. |
| decimal_literal | Een zwevend uitgedrukt in decimale notatie met punten. |
| hexadecimal_literal | Een getal dat wordt uitgedrukt door de tekenreeks '0 x' gevolgd door een reeks van hexadecimale cijfers. |
| string_literal | Letterlijke tekenreeks worden vertegenwoordigd door een reeks van nul of meer tekens in Unicode-escape-reeksen Unicode-tekenreeksen. Letterlijke tekenreeks tussen enkele aanhalingstekens (enkel aanhalingsteken: ') of dubbele aanhalingstekens (aanhalingsteken: "). Hiermee heft u toegestaan: `\'`, `\"`, `\\`, `\uXXXX` voor Unicode-tekens die zijn gedefinieerd door 4 hexadecimale cijfers. |

### <a name="operators"></a>Operators

De volgende operatoren worden ondersteund:

| Familie | Operators |
| -------------- | -----------------|
| Rekenkundige operatoren | +,-,*,/,% |
| Logische | EN, OF NIET |
| Vergelijking |  =,! =, <>,, < =, > =, <> |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het uitvoeren van query's in uw toepassingen met [IoT Hub SDK's][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md