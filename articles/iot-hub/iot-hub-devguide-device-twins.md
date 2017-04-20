<properties
 pageTitle="Developer guide - apparaat twins begrijpen | Microsoft Azure"
 description="Azure IoT Hub developer guide - gebruik apparaat twins status en configuratie van gegevens tussen IoT Hub en de apparaten synchroniseren"
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

# <a name="understand-device-twins---preview"></a>Twins apparaat begrijpen - voorbeeld

## <a name="overview"></a>Overzicht

*Twins apparaat* zijn JSON-documenten waarin apparaat staat informatie (metagegevens, configuraties en omstandigheden). IoT Hub aanhoudt een twin apparaat voor elk apparaat dat u op de IoT aansluit Hub. In dit artikel wordt beschreven:

* de structuur van het apparaat twin: *labels*, *gewenste* en *eigenschappen vermeld*, en
* de bewerkingen die apparaat apps en het vorige eindigt op apparaat twins kunnen uitvoeren.

> [AZURE.NOTE] Op dit moment zijn apparaat twins alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

### <a name="when-to-use"></a>Wanneer gebruikt u

Twins apparaat te gebruiken:

* Apparaatspecifieke meta-gegevens opslaan in de cloud, zoals implementatie-locatie van een Verkoopautomaat.
* Verslag informatie over de huidige status zoals beschikbare mogelijkheden en voorwaarden van uw app apparaat, bijvoorbeeld een verbinding maken via een mobiele telefoon of wifi apparaat.
* De status van werkstromen langdurige tussen apparaat app en de back end, bijvoorbeeld back-end geven de nieuwe firmware-versie te installeren en de rapportage van de verschillende fasen van het updateproces apparaat-app synchroniseren.
* Query uitvoeren op uw apparaat meta-gegevens, de configuratie of de staat.

[Apparaat-wolk - berichten] gebruikt[ lnk-d2c] voor een reeks gebeurtenissen zoals de tijdreeks van sensorgegevens of alarmen voorzien van een tijdstempel. [Cloud-apparaat - methoden] gebruiken[ lnk-methods] voor interactieve besturing van apparaten, zoals het inschakelen van een ventilator.

## <a name="device-twins"></a>Twins apparaat

Twins apparaat opgeslagen gegevens over het apparaat dat:

- Uiteinden apparaat en weer kunnen gebruiken om te synchroniseren apparaat voorwaarden en configuratie.
- De toepassing back-end query en kunt doel langdurige bewerkingen.

De levenscyclus van een twin apparaat is gekoppeld aan het bijbehorende [apparaat-id][lnk-identity]. Twins worden impliciet gemaakt en verwijderd wanneer u een nieuw apparaat-id wordt gemaakt of verwijderd in IoT Hub.

Een apparaat twin is een JSON-document bevat:

* **Tags**. Een JSON-document gelezen en geschreven door de back-end. Tags zijn niet zichtbaar voor apparaat apps.
* **Gewenste eigenschappen**. Gebruikt in combinatie met de eigenschappen van gerapporteerde te synchroniseren met de apparaatconfiguratie of voorwaarde. Gewenste eigenschappen kunnen alleen worden ingesteld door de toepassing back end en kunnen worden gelezen door de app van het apparaat. De app van het apparaat is ook gesteld in realtime wijzigingen op de gewenste eigenschappen.
* **Eigenschappen gemeld**. Gebruikt in combinatie met de gewenste eigenschappen voor het synchroniseren van de configuratie of voorwaarde. Gerapporteerde eigenschappen kunnen kan alleen worden ingesteld door de toepassing van het apparaat en worden gelezen en ondervraagd door de toepassing back-end.

De hoofdmap van het apparaat twin bevat tevens de alleen-lezen eigenschappen van de bijbehorende identiteit, als die in het [register van apparaat-id][lnk-identity].

![][img-twin]

Hier volgt een voorbeeld van een apparaat twin JSON-document:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

In het hoofdobject zijn de Systeemeigenschappen en de container-objecten voor `tags` en beide `reported` en `desired properties`. De `properties` container bevat sommige elementen met het kenmerk alleen-lezen (`$metadata`, `$etag`, en `$version`) die respectievelijk worden beschreven in de [metagegevens Twin] [ lnk-twin-metadata] en [optimistische gelijktijdigheid] [ lnk-concurrency] secties.

### <a name="reported-property-example"></a>Voorbeeld van de eigenschap gemeld

In het bovenstaande voorbeeld de twin apparaat bevat een `batteryLevel` eigenschap die wordt gerapporteerd via de app van het apparaat. Deze eigenschap kunt u opvragen en op apparaten op basis van de laatste gerapporteerde batterij niveau opereren. Een ander voorbeeld zou de mogelijkheden van apparaat app rapport apparaat of hebben connectiviteitsopties.

Let op hoe de gerapporteerde eigenschappen vereenvoudigen scenario's waar de back-end is geïnteresseerd in het laatste bekende waarde van een eigenschap. [Apparaat-wolk - berichten] gebruikt[ lnk-d2c] als de back-end moet de telemetrie apparaat in de vorm van reeksen voorzien van een tijdstempel gebeurtenissen, zoals de tijdreeksen te verwerken.

### <a name="desired-configuration-example"></a>Voorbeeld van de gewenste configuratie

In het bovenstaande voorbeeld, de `telemetryConfig` eigenschappen gewenste en gemeld door de back end en apparaat-app worden gebruikt voor het synchroniseren van de configuratie van de telemetrie voor dit apparaat. Bijvoorbeeld:

1. De app back-end Hiermee stelt u de gewenste eigenschap met de waarde van de gewenste configuratie. Dit is het gedeelte van het document met de gewenste eigenschap:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. De app van het apparaat wordt in kennis gesteld van de wijziging onmiddellijk als verbonden of bij de eerste opnieuw verbinden. De app van het apparaat en worden vervolgens de bijgewerkte configuratie (of een fout voorwaarde met behulp van de `status` eigenschap). Dit is het gedeelte van de gerapporteerde eigenschappen:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. De app back-end kunt u de resultaten van de configuratie in veel apparaten, te traceren via [query's] [ lnk-query] twins.

> [AZURE.NOTE] De bovenstaande fragmenten zijn voorbeelden, geoptimaliseerd voor leesbaarheid van een mogelijke manier voor het coderen van een configuratie en de status ervan. IoT Hub heeft een specifiek schema voor de eigenschappen van het gewenste en gerapporteerd in de twins apparaat niet opleggen.

In veel gevallen worden twins gebruikt voor het synchroniseren van langdurige bewerkingen, zoals het firmware-updates. Verwijzen naar de [gewenste eigenschappen voor het configureren van apparaten gebruiken] [ lnk-twin-properties] voor meer informatie over het gebruik van eigenschappen te synchroniseren en bijhouden van lange bewerkingen tussen apparaten wordt uitgevoerd.

## <a name="back-end-operations"></a>Back-end-activiteiten

De back-end werkt op de twin met behulp van de volgende atomaire bewerkingen via HTTP:

1. **Twin door-id ophalen**. Deze bewerking wordt de inhoud van de twin van document, inclusief codes en gewenst, gerapporteerd en Systeemeigenschappen.
2. **Twin gedeeltelijk bijwerken**. Deze bewerking kunt de back-end gedeeltelijk bijwerken van de twee labels of gewenste eigenschappen. Gedeeltelijk bijwerken wordt uitgedrukt als een JSON-document dat een van de genoemde eigenschappen bijgewerkt of toegevoegd. Eigenschappen instellen op `null` worden verwijderd. De volgende maakt u bijvoorbeeld een gewenste nieuwe eigenschap met de waarde `{"newProperty": "newValue"}`, overschrijft de bestaande waarde van `existingProperty` met `"otherNewValue"`, en wordt volledig verwijderd `otherOldProperty`. Er zijn geen wijzigingen gebeuren aan andere bestaande gewenste eigenschappen of tags:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Vervangen door de gewenste eigenschappen**. Deze bewerking kan volledig overschrijft alle bestaande gewenste eigenschappen en vervangen door een nieuw JSON-document voor de back-end `properties/desired`.
4. **Tags te vervangen**. Om de gewenste eigenschappen vervangen, analogously kunt deze bewerkingen volledig overschrijft alle bestaande codes en vervangen door een nieuw JSON-document voor de back-end `tags`.

De bovenstaande bewerkingen ondersteunen [optimistische gelijktijdigheid] [ lnk-concurrency] en de machtiging **ServiceConnect** vereisen, zoals gedefinieerd in de [beveiliging] [ lnk-security] artikel.

Naast deze transacties de back-end de twins met behulp van een SQL-achtige [query taal]kan een query[lnk-query], en bewerkingen uitvoeren in grote verzamelingen van twins met behulp van [taken][lnk-jobs].

## <a name="device-operations"></a>Apparaatbewerkingen

De app van het apparaat werkt op het gebruik van de volgende bewerkingen atomaire twin:

1. **Twin ophalen**. Deze bewerking wordt de inhoud van het document van de twin (met inbegrip van tags en gewenst, gerapporteerd en Systeemeigenschappen) voor het verbonden apparaat.
2. **Gedeeltelijk update eigenschappen vermeld**. Deze bewerking kan de gedeeltelijke update van de gerapporteerde eigenschappen van het verbonden apparaat. Deze maakt gebruik van dezelfde JSON update indeling als de back-end naast elkaar gedeeltelijk bijwerken van de gewenste eigenschappen.
3. **Observe de gewenste eigenschappen**. De momenteel aangesloten apparaat kunt worden gesteld van updates op de gewenste eigenschappen als ze gebeuren. Het apparaat ontvangt dezelfde vorm van update (volledige of gedeeltelijke vervanging) uitgevoerd door de back-end.

De bovenstaande bewerkingen toestemming **DeviceConnect** , zoals gedefinieerd in de [beveiliging] [ lnk-security] artikel.

De [Azure IoT apparaat SDK's] [ lnk-sdks] kunt u eenvoudig de bovenstaande bewerkingen uit veel talen en platforms gebruiken. Meer informatie over de details van de primitieven IoT-Hub voor de synchronisatie van de gewenste eigenschappen vindt u in het [apparaat opnieuw verbinden stroom][lnk-reconnection].

> [AZURE.NOTE] Op dit moment zijn apparaat twins alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor verwijzing bieden meer informatie over het beheren van toegang tot uw hub IoT.

## <a name="tags-and-properties-format"></a>Indeling voor labels en eigenschappen

Tags, gewenste en gerapporteerde eigenschappen zijn JSON-objecten met de volgende beperkingen:

* Alle sleutels in JSON-objecten zijn hoofdlettergevoelig 128 tekens Unicode-tekenreeksen. Toegestane tekens Unicode-besturingstekens (segmenten C0 en C1) uitsluiten en `'.'`, `' '`, en `'$'`.
* Alle waarden in JSON-object zijn de volgende soorten van JSON: boolean, getal, tekenreeks en object. Matrices zijn niet toegestaan.

## <a name="twin-size"></a>Dubbele grootte

IoT Hub wordt afgedwongen dat een beperking van 8KB grootte van de waarden van `tags`, `properties/desired`, en `properties/reported`, met uitzondering van de elementen in alleen-lezen.
De grootte wordt berekend door het tellen van alle tekens, met uitzondering van UNICODE tekens (segmenten C0 en C1) en ruimte bepalen `' '` wanneer deze wordt weergegeven buiten een tekenreeksconstante.
IoT Hub afwijzen met fout alle bewerkingen die de omvang van deze documenten boven de limiet zou vergroten.

## <a name="twin-metadata"></a>Twin-metagegevens

IoT Hub onderhoudt de tijdstempel van de laatste update voor elk JSON-object in het gewenste en gerapporteerd. De tijdstempels in UTC en gecodeerd in de indeling [ISO8601] `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Bijvoorbeeld:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Deze informatie wordt bewaard op elk niveau (niet alleen de bladeren van de JSON-structuur) te behouden die objectsleutels verwijderen.

## <a name="optimistic-concurrency"></a>Optimistische gelijktijdigheid

Tags, ondersteuning voor gewenste gerapporteerde eigenschappen en alle optimistische gelijktijdigheid.
Labels hebben een etag, aan de hand van [RFC7232], die van de tag JSON representatie vertegenwoordigt. U kunt dit gebruiken in voorwaardelijke bewerkingen uit de back-end samenhang te waarborgen.

Gewenste en gerapporteerde eigenschappen niet etags, maar hebben een `$version` is gegarandeerd incrementele waarde. Analogously met een etag, kan de versie worden gebruikt door de partij bijwerken zoals (een apparaat app voor een eigenschap gemeld) of de back-end voor een gewenste eigenschap gebruikt om de consistentie van de updates.

Versies zijn ook handig als een agent observing (zoals het apparaat app inachtneming van de gewenste eigenschappen) afstemmen races tussen het resultaat van een bewerking ophalen en een wijzigingswaarschuwing heeft. De sectie [apparaat opnieuw verbinden stroom] [ lnk-reconnection] vindt u meer informatie.

## <a name="device-reconnection-flow"></a>Apparaat opnieuw verbinden stroom

IoT Hub behoudt niet de gewenste eigenschappen updatemeldingen voor niet-verbonden apparaten. Hieruit volgt dat een apparaat dat verbinding maakt de gewenste eigenschappen volledig document abonneren op updatemeldingen moet ophalen. Gelet op de mogelijkheid van races tussen updatemeldingen en volledige ophalen, moet de volgende stroom worden gezorgd voor:

1. App apparaat verbindt met een hub IoT.
2. Apparaat app geabonneerd voor de gewenste eigenschappen updatemeldingen.
3. Apparaat app haalt het volledige document voor de gewenste eigenschappen.

De app van het apparaat kunt negeren alle meldingen met `$version` kleiner dan of gelijk is dan de versie van het volledige document opgehaald. Dit is mogelijk omdat de IoT Hub zorgt ervoor dat versies altijd verhoogd.

> [AZURE.NOTE] Deze logica wordt al geïmplementeerd in de [Azure IoT apparaat SDK's][lnk-sdks]. Deze beschrijving is handig als het apparaat app van Azure IoT apparaat SDK's niet gebruiken en de MQTT-interface rechtstreeks moet programmeren.

## <a name="additional-reference-material"></a>Aanvullend referentiemateriaal

Er zijn andere zoeken naar onderwerpen in de handleiding voor ontwikkelaars:

- [Eindpunten IoT Hub] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT beschikbaar worden gesteld voor de uitvoering en het beheer van bewerkingen.
- [Procesbeperking en quota's] [ lnk-quotas] de quota die van toepassing zijn op de IoT Hub-service en de bandbreedteregeling gedrag u kunt verwachten als u de service wordt beschreven.
- [SDK's IoT Hub apparaat- en] [ lnk-sdks] geeft een overzicht van de verschillende taal SDK's u een gebruiken, wanneer u zowel apparaat als service toepassingen ontwikkelt die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] de querytaal kunt u gegevens ophalen uit de IoT Hub over uw apparaat twins, methoden en taken beschreven.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over de IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd over apparaat-twins je wellicht geïnteresseerd in de volgende onderwerpen in de handleiding voor ontwikkelaars:

- [Een directe methode aanroept op een apparaat][lnk-methods]
- [Taken plannen op meerdere apparaten][lnk-jobs]

Als u proberen sommige van de concepten die in dit artikel worden beschreven wilt, kunt u mogelijk geïnteresseerd in de volgende zelfstudies voor IoT Hub:

- [Het gebruik van het apparaat twin][lnk-twin-tutorial]
- [Het gebruik van dubbele eigenschappen][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png