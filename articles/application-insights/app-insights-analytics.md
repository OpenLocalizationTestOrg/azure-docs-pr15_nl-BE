<properties 
    pageTitle="Analytics - de krachtige zoekfunctie van toepassing Insights | Microsoft Azure" 
    description="Overzicht van Analytics, krachtige diagnostische zoekfunctie van inzichten van toepassing. " 
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
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Analytics in inzichten van toepassing


[Analytics](app-insights-analytics.md) is de krachtige zoekfunctie van [Inzichten van toepassing](app-insights-overview.md). Deze pagina's beschrijven de lanquage Analytics query. 

* **[Bekijk de inleidende video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Test drive Analytics op onze gesimuleerde gegevens](https://analytics.applicationinsights.io/demo)** als uw app is niet van gegevens naar inzichten toepassing nog verzenden.

## <a name="queries-in-analytics"></a>Query's in Analytics
 
Een typische query is *een brontabel gevolgd door een reeks van *operatoren* , gescheiden door* `|`. 

Bijvoorbeeld, we weten het tijdstip waarop de burgers van Hyderabad onze web app proberen. En ondanks dat wij er, laten we zien welke resultaatcodes HTTP-aanvragen worden geretourneerd. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

We tellen onderscheiden client-IP-adressen, te groeperen per uur van de dag de afgelopen 7 dagen. 

Laten we de resultaten met de presentatie te stapelen de resultaten van verschillende responscodes staafdiagram weergeven:

![Kies staafdiagram, x en y-as en segmentatie](./media/app-insights-analytics/020.png)

Lijkt op de onze app is populairste lunchpauze en bed-tijd in Hyderabad. (En gaan we deze 500 codes).


Er zijn krachtige statistische bewerkingen:

![](./media/app-insights-analytics/025.png)


De taal heeft veel voordelen bieden:

* [Filter](app-insights-analytics-reference.md#where-operator) uw onbewerkte app telemetrie op alle velden, met inbegrip van de aangepaste eigenschappen en parameters.
* [Lid worden van](app-insights-analytics-reference.md#join-operator) meerdere tabellen – correlate aanvragen met paginaweergaven, afhankelijkheid oproepen, uitzonderingen en logboek wordt getraceerd.
* Krachtige statistische [aggregaties](app-insights-analytics-reference.md#aggregations).
* Net zo krachtig zijn als SQL, maar veel gemakkelijker voor complexe query's: in plaats van instructies nesten, u de gegevens van de ene elementaire bewerking naar de volgende pipe.
* Directe en krachtige visualisaties.







## <a name="connect-to-your-application-insights-data"></a>Verbinding maken met de gegevens van uw toepassing inzichten


Open Analytics uit van uw app [Overzicht blade](app-insights-dashboards.md) in inzichten van toepassing: 

![Portal.azure.com openen, opent u de toepassing inzichten resource en klik op Analytics.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Limieten

Queryresultaten zijn momenteel beperkt tot alleen gedurende een week van oude gegevens.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Volgende stappen


* Het is raadzaam dat u starten met de [rondleiding door de taal](app-insights-analytics-tour.md).