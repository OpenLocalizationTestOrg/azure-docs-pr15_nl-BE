<properties 
    pageTitle="Toezicht op gebruik en de prestaties van Windows desktop apps" 
    description="Gebruik en de prestaties van uw Windows desktop app met HockeyApp en inzichten die toepassingen analyseren." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Toezicht op gebruik en de prestaties in Windows Desktop apps

*Inzichten van toepassing is in het afdrukvoorbeeld.*

[Visual Studio-toepassing inzichten](app-insights-overview.md) en [HockeyApp](https://hockeyapp.net) kunt u de gebruikte toepassing voor het gebruik en de prestaties te controleren.

> [AZURE.IMPORTANT] Wij raden aan [HockeyApp](https://hockeyapp.net) voor het distribueren en apps voor desktop- en controleren. Met HockeyApp, kunt u distributie, live testen en feedback van gebruikers beheren, evenals gebruik en crash rapporten controleren. U kunt ook [exporteren en de telemetrie met Analytics opvragen](app-insights-hockeyapp-bridge-app.md).

> Telemetrie kan worden verzonden naar de inzichten van de toepassing vanuit een bureaubladtoepassing, dit is vooral handig voor het opsporen van fouten en experimentele doeleinden.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Telemetrie naar inzichten toepassing verzenden vanuit een Windows-toepassing

1. In de [portal Azure](https://portal.azure.com), [maakt u een bron inzichten van toepassing](app-insights-create-new-resource.md). Kies soort toepassing ASP.NET-toepassing.
2. Een kopie van de sleutel Instrumentation nemen. Zoek naar de sleutel in de Essentials-omlaag van de nieuwe resource die u zojuist hebt gemaakt. 
3. De NuGet pakketten van uw app project bewerken in Visual Studio en Microsoft.ApplicationInsights.WindowsServer toevoegen. (Of kies Microsoft.ApplicationInsights als u alleen de bare API, zonder de verzameling standaard telemetrie modules.)
4. Stel de sleutel instrumentation in uw code:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*uw sleutel*`";` 

    of in ApplicationInsights.config (als u een van de standaard telemetrie pakketten geïnstalleerd):
 
    `<InstrumentationKey>`*uw sleutel*`</InstrumentationKey>` 

    Als u ApplicationInsights.config gebruikt, zorg ervoor dat de eigenschappen in de Solution Explorer zijn ingesteld op **Build Action = inhoud kopiëren naar de uitvoermap kopie =**.
5. [Gebruik de API](app-insights-api-custom-events-metrics.md) voor het verzenden van telemetrie.
6. Voer uw app en Zie de telemetrie in de bron die u hebt gemaakt in de Portal Azure.

## <a name="telemetry"></a>Van voorbeeldcode

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Volgende stappen

* [Een dashboard maken](app-insights-dashboards.md)
* [Diagnostische zoeken](app-insights-diagnostic-search.md)
* [Bekijk de metrics](app-insights-metrics-explorer.md)
* [Analytics query's schrijven](app-insights-analytics.md)
 
