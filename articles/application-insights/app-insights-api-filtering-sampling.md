<properties 
    pageTitle="Filteren en voorverwerking in de SDK van toepassing Insights | Microsoft Azure" 
    description="Telemetrie-Processors en telemetrie begin voor de SDK wilt filteren of eigenschappen toevoegen aan de gegevens voordat de telemetrie wordt verzonden naar de portal Application inzichten schrijven." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filteren en voorverwerking telemetrie in de inzichten Application SDK

*Inzichten van toepassing is in het afdrukvoorbeeld.*

U kunt schrijven en plug-ins voor de toepassing inzichten SDK kunt u aanpassen hoe telemetrie is vastgelegd en verwerkt voordat het wordt verzonden naar de service Application inzichten configureren. 

Deze functies zijn momenteel beschikbaar voor de ASP.NET-SDK.

* [Bemonstering](app-insights-sampling.md) vermindert u het volume van de telemetrie zonder uw statistieken. Bij elkaar blijven gerelateerde gegevenspunten zodat u tussen hen navigeren kunt bij het oplossen van een probleem. De totale aantallen worden in de portal te compenseren voor de bemonstering vermenigvuldigd.
* [Filteren met telemetrie Processors](#filtering) kunt u selecteren of wijzigen van telemetrie in de SDK voordat het wordt verzonden naar de server. U kunt bijvoorbeeld het volume van de telemetrie verminderen door met uitzondering van aanvragen van robots. Maar filteren is een eenvoudige benadering van minder verkeer dan de bemonstering. Hiermee kunt u meer controle over wat wordt verzonden, maar u te weten dat dit van invloed op de statistieken - bijvoorbeeld als u alle aanvragen filteren.
* [Telemetrie begin eigenschappen toevoegen](#add-properties) aan elk telemetrie verzonden vanaf uw app, met inbegrip van de telemetrie van de standaardmodules. Bijvoorbeeld, zou u berekende waarden; of versienummers waarmee de gegevens in de portal te filteren.
* [De SDK-API](app-insights-api-custom-events-metrics.md) wordt gebruikt voor het verzenden van aangepaste gebeurtenissen en statistieken.


Voordat u begint:

* De [toepassing inzichten SDK voor ASP.NET v2](app-insights-asp-net.md) in uw app installeren. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filtering: ITelemetryProcessor

Deze techniek biedt u meer directe controle over wat wordt opgenomen of uitgesloten van de telemetrie-stream. Kunt u deze in combinatie met de bemonstering, of afzonderlijk.

Telemetrie filter: een processor telemetrie schrijven en registreren met de SDK. Alle telemetrie doorloopt uw processor en kunt u eigenschappen toevoegen of neer te zetten van de stroom. Dit omvat de telemetrie van de standaard modules zoals de HTTP-aanvraag collector en afhankelijkheid verzamelaar en telemetrie die u zelf hebt geschreven. U kunt, bijvoorbeeld een filter telemetrie over aanvragen van robots of gesprekken succesvolle afhankelijkheid. 

> [AZURE.WARNING] Filteren van de telemetrie van de SDK worden verzonden kan met processors scheeftrekken van de statistieken die u in de portal ziet en maken het moeilijk om verwante items te volgen.
> 
> In plaats daarvan kunt u overwegen [de bemonstering](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Maak een telemetrie-processor

1. Controleer of de toepassing inzichten SDK in uw project is versie 2.0.0 of hoger. Uw project in Visual Studio Solution Explorer met de rechtermuisknop en kies NuGet pakketten beheren. Controleer de Microsoft.ApplicationInsights.Web in NuGet package manager.

1. U kunt een filter maken ITelemetryProcessor te implementeren. Dit is een ander uitbreidbaarheid punt zoals telemetrie module initialiseren Telemetrie en telemetrie-kanaal. 

    U ziet dat telemetrie-Processors een keten van verwerking maken. Wanneer u een processor telemetrie, geeft u een koppeling aan de volgende processor in de keten. Als een gegevenspunt telemetrie wordt doorgegeven aan de methode proces, zijn werk doet en roept vervolgens de volgende telemetrie Processor in de keten.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Voeg dit in ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Dit is hetzelfde gedeelte waar u een filter voor de bemonstering initialiseren).

String-waarden kunt uit het .config-bestand u doorgeven door middel van openbare benoemde eigenschappen in de klasse. 

> [AZURE.WARNING] Wees voorzichtig zodat deze overeenkomen met de naam en de namen van eigenschappen in het .config-bestand met de namen van de klasse en de eigenschap in de code. Als het .config-bestand verwijst naar een niet-bestaande of de eigenschap, kan zonder de SDK voor het verzenden van een telemetrie mislukken.

 
**U kunt ook** het filter in code kunnen worden geïnitialiseerd. In een initialisatieklasse geschikt - bijvoorbeeld AppStart in Global.asax.cs - Voeg uw processor in de keten:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients die zijn gemaakt na dit punt zal uw processors gebruiken.

### <a name="example-filters"></a>Voorbeeld van de filters

#### <a name="synthetic-requests"></a>Synthetische aanvragen

Filter proeven bots en web. Hoewel Metrics Explorer u de optie kunt voor het filteren van kunstmatige bronnen, minder verkeer door ze te filteren op de SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Verificatie mislukt

Aanvragen met een ' 401-responspagina uitfilteren. 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Snelle externe afhankelijkheid oproepen filteren

Als u alleen aanroepen die langzaam diagnose wilt, snel weg te filteren. 

> [AZURE.NOTE] Hiermee wordt een verkeerd de statistieken die u op de portal ziet. De afhankelijkheid grafiek eruitziet alsof het aanroepen van de afhankelijkheid alle fouten worden.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Vaststellen van afhankelijkheidsproblemen met

[Deze blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) wordt een project afhankelijkheidsproblemen diagnose door te sturen automatisch regelmatig pings naar afhankelijkheden beschreven.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Eigenschappen toevoegen: ITelemetryInitializer

Telemetrie begin gebruiken voor het definiëren van de globale eigenschappen die worden verzonden met alle telemetrie; en geselecteerde gedrag van de standaard telemetrie modules wijzigen. 

Bijvoorbeeld verzamelt de inzichten van toepassing voor het webpakket telemetrie over HTTP-aanvragen. Standaard worden gemarkeerd als mislukt verzoeken met reactiecode > = 400. Maar als u 400 behandelen als een groot succes wilt, kunt u een telemetrie initialiseren die de eigenschap succes bieden.

Als u een telemetrie initialiseren, wordt aangeroepen wanneer een van de methoden van de Track*() wordt genoemd. Dit omvat methoden aangeroepen door de standaard telemetrie modules. Door het Verdrag stelt deze modules niet alle eigenschappen die al is ingesteld door een initialiseren. 

**Definieer uw initialiseren**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Laadt uw initialiseren**

In ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Ook* kunt u de initialisatiefunctie in code, bijvoorbeeld in Global.aspx.cs instantiëren:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Meer zien van dit voorbeeld.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>JavaScript telemetrie begin

*JavaScript*

Voeg een telemetrie initialiseren direct na de initialisatie die u hebt gekregen van de portal: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Zie voor een overzicht van de niet-aangepaste eigenschappen beschikbaar op de telemetryItem, het [gegevensmodel](app-insights-export-data-model.md#lttelemetrytypegt).

U kunt zoveel initalisatiefuncties als u wilt toevoegen. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor en ITelemetryInitializer

Wat is het verschil tussen processors Telemetrie en telemetrie begin?

* Er zijn bepaalde overlappingen in wat u ermee kunt doen: beide eigenschappen toevoegen aan telemetrie kunnen worden gebruikt.
* TelemetryInitializers worden altijd uitgevoerd vóór de TelemetryProcessors.
* TelemetryProcessors kunt u volledig vervangen of verwijderen van een item telemetrie.
* TelemetryProcessors verwerken geen performance counter telemetrie.



## <a name="persistence-channel"></a>Persistentie kanaal 

Als uw app wordt uitgevoerd wanneer de internet-verbinding is niet altijd beschikbaar of traag, kunt u het kanaal persistentie in plaats van de standaard in het geheugen. 

Het standaardkanaal in het geheugen verliest de telemetrie die niet is verzonden op het moment dat de toepassing wordt gesloten. Hoewel u met `Flush()` wilt verzenden van alle gegevens in de buffer, het nog steeds verliest gegevens als er geen internet-verbinding, of als de toepassing wordt afgesloten voordat de overdracht voltooid is.

Het kanaal persistentie buffers daarentegen telemetrie in een bestand, voordat deze naar de portal verzonden. `Flush()`zorgt ervoor dat de gegevens in het bestand is opgeslagen. Als de gegevens niet worden verzonden op het moment dat de toepassing wordt gesloten, blijft deze in het bestand. Wanneer u de toepassing opnieuw wordt gestart, wordt de gegevens vervolgens verzonden als er een internet-verbinding. Gegevens worden bij elkaar opgeteld in het bestand zo lang als nodig is een verbinding beschikbaar is. 

### <a name="to-use-the-persistence-channel"></a>Op het kanaal persistentie

1. Het pakket NuGet [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3)importeren.
2. Deze code opnemen in uw app, op een geschikte initialisatie-locatie:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Gebruik `telemetryClient.Flush()` voordat de toepassing wordt gesloten, om te controleren of de gegevens worden verzonden naar de portal of in het bestand opgeslagen.

    Houd er rekening mee dat Flush() voor het kanaal persistentie synchroon, maar asynchroon is voor andere kanalen is.

 
Het kanaal persistentie is geoptimaliseerd voor scenario's apparaten, waarbij het aantal gebeurtenissen dat door de toepassing relatief klein is en de verbinding is vaak onbetrouwbaar. Dit kanaal worden gebeurtenissen naar de schijf schrijven aan betrouwbare opslag eerst en vervolgens probeert te verzenden. 

#### <a name="example"></a>Voorbeeld

Stel dat u wilt controleren, niet-verwerkte uitzonderingen. U abonneren op de `UnhandledException` gebeurtenis. U opnemen in de retouraanroep, een aanroep naar Flush om ervoor te zorgen dat de telemetrie blijft behouden.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Wanneer de toepassing wordt afgesloten, wordt er een bestand in `%LocalAppData%\Microsoft\ApplicationInsights\`, waarin de gecomprimeerde gebeurtenissen bevat. 
 
Volgende keer dat u deze toepassing start, wordt het kanaal dit bestand ophalen en telemetrie leveren aan de inzichten van toepassing als dat mogelijk is.

#### <a name="test-example"></a>Voorbeeld van de test

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


De code van het kanaal persistentie is op [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Referentie docs

* [API-overzicht](app-insights-api-custom-events-metrics.md)

* [ASP.NET-naslaginformatie](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK-Code

* [ASP.NET-Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript-SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Volgende stappen


* [Zoeken-gebeurtenissen en Logboeken][diagnostic]
* [Bemonstering](app-insights-sampling.md)
* [Het oplossen van problemen][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
