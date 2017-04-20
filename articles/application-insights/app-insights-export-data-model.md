<properties 
    pageTitle="Toepassing inzichten gegevensmodel" 
    description="Beschrijving van de eigenschappen van continue export in JSON geëxporteerd en als filter." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Toepassing inzichten Export Data Model

Deze tabel bevat de eigenschappen van de telemetrie van de [Toepassing inzichten](app-insights-overview.md) SDK's verzonden naar de portal. Hier ziet u deze eigenschappen in de uitvoer van [Continue exporteren](app-insights-export-telemetry.md).
Ze worden ook weergegeven in de eigenschappenfilters in [Explorer Metric](app-insights-metrics-explorer.md) en [Diagnostische zoeken](app-insights-diagnostic-search.md).

Punten om te onthouden:

* `[0]`in deze tabellen aangegeven een punt in het pad waar u hebt een index; invoegen maar het is niet altijd 0.
* Tijdsduur in tienden van een microseconde, dus 10000000 zijn == 1 seconde.
* Datums en tijden zijn UTC en zijn vermeld in de ISO-notatie`yyyy-MM-DDThh:mm:ss.sssZ`

Er zijn verschillende [voorbeelden](app-insights-export-telemetry.md#code-samples) die illustreren hoe u ze gebruikt.



## <a name="example"></a>Voorbeeld

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Context

Alle soorten telemetrie vergezeld gaan van een gedeelte van de. Niet alle van deze velden worden verzonden met elk gegevenspunt.



|Pad|Type|Notities|
|---|---|---|
| context.Custom.Dimensions [0]  | object]  | Sleutel / waarde-reeksparen instellen door de parameter voor aangepaste eigenschappen. Max lengte van 100, maximale lengte van waarden 1024. Meer dan 100 unieke waarden, de eigenschap kan worden gezocht, maar kan niet worden gebruikt voor segmentatie. Max 200 sleutels per ikey.  |
| context.Custom.metrics [0]  | object]  | Sleutel / waarde-paren instellen door de parameter aangepaste afmetingen en TrackMetrics. Max lengte 100, waarden mag alleen cijfers bevatten. |
| context.data.eventTime | tekenreeks | UTC |
| context.data.isSynthetic | Boole-waarde | Aanvraag lijkt te komen van een bot of web test. |
| context.data.samplingRate | nummer | Percentage van de telemetrie gegenereerd door de SDK die wordt verzonden naar de portal. Bereik 0,0 100,0.|
| context.Device | object | Clientapparaat |
| context.Device.browser | tekenreeks | IE, Chrome... |
| context.device.browserVersion | tekenreeks | Chroom 48,0... |
| context.device.deviceModel | tekenreeks | |
| context.device.deviceName | tekenreeks | |
| context.Device.id | tekenreeks | |
| context.Device.locale | tekenreeks | en-GB, nl-nl... |
| context.Device.Network | tekenreeks | |
| context.device.oemName | tekenreeks | |
| context.device.osVersion | tekenreeks | Host OS |
| context.device.roleInstance | tekenreeks | ID van serverhost |
| context.device.roleName | tekenreeks | |
| context.Device.type | tekenreeks | PC, Browser... |
| context.Location | object | Afgeleid van client-IP. |
| context.Location.City | tekenreeks | Afgeleid van client-IP, indien bekend  |
| context.Location.clientip | tekenreeks | Laatste achthoek is anoniem op 0. |
| context.Location.continent | tekenreeks | |
| context.Location.Country | tekenreeks | |
| context.Location.Province | tekenreeks | Staat of provincie |
| context.Operation.id | tekenreeks | Artikelen met dezelfde bewerking-id worden weergegeven als verwante Items in de portal. Meestal de aanvraag-id. |
| context.Operation.name | tekenreeks | URL of aanvraag |
| context.operation.parentId | tekenreeks | Hiermee kunt geneste gerelateerde artikelen. |
| context.Session.id | tekenreeks | De id van een groep bewerkingen uit dezelfde bron. Een periode van 30 minuten, zonder een bewerking geeft het einde van een sessie. |
| context.session.isFirst | Boole-waarde | |
| context.user.accountAcquisitionDate | tekenreeks | |
| context.user.anonAcquisitionDate | tekenreeks | |
| context.user.anonId | tekenreeks | |
| context.user.authAcquisitionDate | tekenreeks | [Geverifieerde gebruiker](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | Boole-waarde | |
| internal.data.documentVersion | tekenreeks | |
| Internal.Data.id | tekenreeks | |



## <a name="events"></a>Gebeurtenissen

Aangepaste gebeurtenissen die worden gegenereerd door [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event). 


|Pad|Type|Notities|
|---|---|---|
| het aantal gebeurtenissen [0] | geheel getal | 100 / ([sampling](app-insights-sampling.md) -frequentie). Voorbeeld 4 =&gt; 25%. |
| de naam van de gebeurtenis [0] | tekenreeks | De naam van de gebeurtenis.  Maximumlengte 250. |
| url van de gebeurtenis [0] | tekenreeks | |
| gebeurtenis [0] urlData.base | tekenreeks | |
| gebeurtenis [0] urlData.host | tekenreeks | |

## <a name="exceptions"></a>Uitzonderingen

[Uitzonderingen](app-insights-asp-net-exceptions.md) in de server en de browser meldt. 


|Pad|Type|Notities|
|---|---|---|
| basicException [0] assembly | tekenreeks | |
| aantal basicException [0] | geheel getal | 100 / ([sampling](app-insights-sampling.md) -frequentie). Voorbeeld 4 =&gt; 25%. |
| basicException [0] exceptionGroup | tekenreeks | |
| basicException [0] exceptionType | tekenreeks | |tekenreeks | |
| basicException [0] failedUserCodeMethod | tekenreeks | |
| basicException [0] failedUserCodeAssembly | tekenreeks | |
| basicException [0] handledAt | tekenreeks | |
| basicException [0] hasFullStack | Boole-waarde | |
| [0] basicException-id | tekenreeks | |
| de methode basicException [0] | tekenreeks | |
| message [0] basicException | tekenreeks | Uitzonderingsbericht. Maximumlengte 10k.|
| basicException [0] outerExceptionMessage | tekenreeks | |
| basicException [0] outerExceptionThrownAtAssembly | tekenreeks | |
| basicException [0] outerExceptionThrownAtMethod | tekenreeks | |
| basicException [0] outerExceptionType | tekenreeks | |
| basicException [0] outerId | tekenreeks | |
| basicException [0] parsedStack [0] assembly | tekenreeks | |
| basicException [0] parsedStack [0] bestandsnaam | tekenreeks | |
| basicException [0] [0] parsedStack niveau | geheel getal | |
| basicException [0] [0] parsedStack lijn | geheel getal | |
| basicException [0] [0] parsedStack-methode | tekenreeks | |
| stapel basicException [0] | tekenreeks | Maximumlengte van 10k|
| typeName [0] basicException | tekenreeks | |



## <a name="trace-messages"></a>Berichten traceren

Verzonden door [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)en de [registratie van adapters](app-insights-asp-net-trace-logs.md).


|Pad|Type|Notities|
|---|---|---|
| Message [0] Loggernaam | tekenreeks ||
| parameters Message [0] | tekenreeks ||
| raw Message [0] | tekenreeks | Het logboekbericht maximale lengte 10k. |
| Foutcode Message [0] | tekenreeks | |



## <a name="remote-dependency"></a>Externe afhankelijkheid

Verzonden door TrackDependency. Gebruikt voor het rapportprestaties en het gebruik van [afhankelijkheden aanroepen](app-insights-asp-net-dependencies.md) in de server en AJAX roept in de browser.

|Pad|Type|Notities|
|---|---|---|
| asynchrone remoteDependency [0] | Boole-waarde | |
| baseName [0] remoteDependency | tekenreeks |  |
| de opdrachtnaam remoteDependency [0] | tekenreeks | Bijvoorbeeld 'home/index" |
| aantal remoteDependency [0] | geheel getal | 100 / ([sampling](app-insights-sampling.md) -frequentie). Voorbeeld 4 =&gt; 25%. |
| remoteDependency [0] dependencyTypeName | tekenreeks | HTTP, SQL... |
| remoteDependency [0] durationMetric.value | nummer | Tijd voor aanroep van antwoord van afhankelijkheid is voltooid |
| [0] remoteDependency-id | tekenreeks | |
| de naam remoteDependency [0] | tekenreeks | URL. Maximumlengte 250.|
| remoteDependency [0] resultCode | tekenreeks | van de afhankelijkheid van HTTP |
| succes remoteDependency [0] | Boole-waarde | |
| type remoteDependency [0] | tekenreeks | HTTP, Sql... |
| de url remoteDependency [0] | tekenreeks |  Maximumlengte 2000 |
| remoteDependency [0] urlData.base | tekenreeks | Maximumlengte 2000 |
| remoteDependency [0] urlData.hashTag | tekenreeks | |
| remoteDependency [0] urlData.host | tekenreeks | Maximumlengte 200 |


## <a name="requests"></a>Aanvragen

Verzonden door [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). De standaardmodules Gebruik deze rapporten server responstijd, gemeten op de server. 


|Pad|Type|Notities|
|---|---|---|
| het aantal aanvragen [0] | geheel getal | 100 / ([sampling](app-insights-sampling.md) -frequentie). Bijvoorbeeld: 4 =&gt; 25%. |
| aanvraag [0] durationMetric.value | nummer | Tijdstip van verzoek binnenkomen op antwoord. 1e7 == 1s |
| [0] aanvraag-id | tekenreeks | Bewerking-id |
| naam van de aanvraag [0] | tekenreeks | GET/POST + basis-url.  Maximale lengte 250 |
| aanvraag [0] responseCode | geheel getal | HTTP-antwoord naar de client verzonden |
| succes aanvraag [0] | Boole-waarde | Standaard == (responseCode &lt; 400) |
| url van de aanvraag [0] | tekenreeks | Met uitzondering van host |
| aanvraag [0] urlData.base | tekenreeks | |
| aanvraag [0] urlData.hashTag | tekenreeks |  |
| aanvraag [0] urlData.host | tekenreeks | |


## <a name="page-view-performance"></a>In de weergave pagina-prestaties

Door de browser wordt verzonden. Meet de tijd om een pagina, gebruiker starten de aanvraag weer te geven (met uitzondering van asynchrone AJAX calls) voltooid.

Context-waarden geven de client-OS en browser-versie. 


|Pad|Type|Notities|
|---|---|---|
| clientPerformance [0] clientProcess.value | geheel getal | De tijd vanaf het einde van het ontvangen van de HTML-code op de pagina weer te geven. |
| de naam clientPerformance [0] | tekenreeks | |
| clientPerformance [0] networkConnection.value | geheel getal | De tijd die nodig is om een netwerkverbinding tot stand brengen. |
| clientPerformance [0] receiveRequest.value | geheel getal | De tijd van het einde van de aanvraag wordt verzonden naar de HTML-code in antwoord ontvangen. |
| clientPerformance [0] sendRequest.value | geheel getal | Van tijd genomen om de HTTP-aanvraag te verzenden. |
| clientPerformance [0] total.value | geheel getal | Tijd voor het verzenden van de aanvraag tot de weergave van de pagina wordt gestart. |
| de url clientPerformance [0] | tekenreeks | De URL van deze aanvraag |
| clientPerformance [0] urlData.base | tekenreeks | |
| clientPerformance [0] urlData.hashTag | tekenreeks | |
| clientPerformance [0] urlData.host | tekenreeks | |
| clientPerformance [0] urlData.protocol | tekenreeks | |

## <a name="page-views"></a>Paginaweergaven

Verzonden door trackPageView() of [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Pad|Type|Notities|
|---|---|---|
| aantal weergaven [0] | geheel getal | 100 / ([sampling](app-insights-sampling.md) -frequentie). Voorbeeld 4 =&gt; 25%. |
| weergave [0] durationMetric.value | geheel getal | Waarde desgewenst instellen in trackPageView() of startTrackPage() - stopTrackPage(). Niet hetzelfde als clientPerformance waarden. |
| de naam van de weergave [0] | tekenreeks | Titel van de pagina.  Maximale lengte 250 |
| url weergeven [0] | tekenreeks | |
| weergave [0] urlData.base | tekenreeks | |
| weergave [0] urlData.hashTag | tekenreeks | |
| weergave [0] urlData.host | tekenreeks | |



## <a name="availability"></a>Beschikbaarheid

[Beschikbaarheid van web tests](app-insights-monitor-web-app-availability.md)rapporten.

|Pad|Type|Notities|
|---|---|---|
| beschikbaarheid van [0] availabilityMetric.name | tekenreeks | beschikbaarheid |
| beschikbaarheid van [0] availabilityMetric.value | nummer |1.0 of 0,0 |
| aantal beschikbaarheid [0] | geheel getal | 100 / ([sampling](app-insights-sampling.md) -frequentie). Voorbeeld 4 =&gt; 25%. |
| beschikbaarheid van [0] dataSizeMetric.name | tekenreeks | |
| beschikbaarheid van [0] dataSizeMetric.value | geheel getal | |
| beschikbaarheid van [0] durationMetric.name | tekenreeks | |
| beschikbaarheid van [0] durationMetric.value | nummer | De duur van de proef. 1e7 == 1s |
| beschikbaarheidsbericht [0] | tekenreeks | Fout diagnose |
| resultaat van de beschikbaarheid van [0] | tekenreeks | Doorslaggevend |
| beschikbaarheid van [0] runLocation | tekenreeks | Geo-bron van HTTP-req |
| beschikbaarheid van [0] testName | tekenreeks | |
| beschikbaarheid van [0] testRunId | tekenreeks | |
| beschikbaarheid van [0] testTimestamp | tekenreeks | |




## <a name="metrics"></a>Statistieken

Gegenereerd door TrackMetric().

De metrische waarde wordt gevonden in context.custom.metrics[0]

Bijvoorbeeld:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Over metrische waarden

Metrische waarden, zowel in metrische en elders, worden met de structuur van een standaard-object gemeld. Bijvoorbeeld:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Momenteel - maar dit kan veranderen in de toekomst - in alle waarden gemeld in de standaardmodules SDK, `count==1` en alleen de `name` en `value` zijn nuttig. Het enige geval waar ze verschillend zouden zijn zou zijn als u uw eigen gesprekken TrackMetric schrijven in waarop u de overige parameters instellen. 

Het doel van de andere velden is dat de statistieken kunnen worden samengevoegd in de SDK, verkeer verkleinen tot de portal. U kan bijvoorbeeld verschillende opeenvolgende metingen gemiddelde voor elke metrische rapport verzenden. Vervolgens zou u de min, max, standaarddeviatie en statistische waarde (som of gemiddelde) berekenen en aantal ingesteld op het aantal metingen wordt vertegenwoordigd door het rapport. 

In de tabellen, hebben we het aantal velden zelden gebruikt, min, max, STDEV en sampledValue weggelaten.

In plaats van vooraf verzamelen metrics, kunt u [de bemonstering](app-insights-sampling.md) moet u het volume van de telemetrie verminderen.


### <a name="durations"></a>Duur

Tenzij anders vermeld, worden de duur in tienden van een microseconde, aangegeven zodat 10000000.0 1 seconde betekent.



## <a name="see-also"></a>Zie ook

* [Inzichten van toepassing](app-insights-overview.md) 
* [Doorlopende exporteren](app-insights-export-telemetry.md)
* [Codevoorbeelden](app-insights-export-telemetry.md#code-samples)


