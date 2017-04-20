Er zijn enkele beperkingen op het aantal parameters en gebeurtenissen per toepassing (dat wil zeggen per instrumentation sleutel). 

Grenzen zijn afhankelijk van de [prijzen laag](https://azure.microsoft.com/pricing/details/application-insights/) die u kiest.

**Resource** | **Standaardlimiet** | **Maximum aantal**
-------- | ------------- | -------------
Sessiegegevens, punten<sup>1, 2</sup> per maand | onbeperkt | 
Totale gegevenspunten per maand voor de aanvraag, gebeurtenis, afhankelijkheid, trace, uitzondering en in de paginaweergave | 5 miljoen | 50 miljoen<sup>3</sup>
Gegevenssnelheid [traceren en logboek](../articles/application-insights/app-insights-search-diagnostic-logs.md) | 200 dp/s | 500 dp/s
[Uitzondering](../articles/application-insights/app-insights-asp-net-exceptions.md) gegevenssnelheid | 50 dp/s | 50 dp/s
Totale gegevenssnelheid voor aanvraag, gebeurtenis, afhankelijkheid en telemetrie van pagina weergeven | 200 dp/s | 500 dp/s
Behoud van onbewerkte gegevens [Zoeken](../articles/application-insights/app-insights-diagnostic-search.md) en [Analytics](../articles/application-insights/app-insights-analytics.md) | 7 dagen
Geaggregeerde gegevens bewaren voor [metrische explorer](../articles/application-insights/app-insights-metrics-explorer.md) | 90 dagen
[De eigenschap](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) naam aantal | 100 |
Lengte van de naam eigenschap | 150 | 
Lengte van de eigenschap waarde | 8192 | 
Trace en uitzondering lengte van bericht | 10000 |
[Metric](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) naam aantal | 100 |
Lengte van metrische |  150 | 
[Beschikbaarheid van tests](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> een gegevenspunt is een individuele metrische waarde of een gebeurtenis met gekoppelde eigenschappen en metingen.

<sup>2</sup> een sessie gegevens registreert het begin of einde van een sessie en gebruikersidentiteit zich aanmeldt.

<sup>3</sup> extra capaciteit meer dan 50 miljoen kunt kopen.
 
[Over de prijs en quota's in de toepassing inzichten](../articles/application-insights/app-insights-pricing.md)
