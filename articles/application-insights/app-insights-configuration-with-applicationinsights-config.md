<properties 
    pageTitle="Toepassing inzichten SDK configureren met ApplicationInsights.config of XML" 
    description="In- of uitschakelen data collectie modules en prestatiemeteritems en andere parameters toevoegen" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>De toepassing inzichten SDK configureren met ApplicationInsights.config of XML

De toepassing inzichten .NET SDK bestaat uit een aantal NuGet pakketten. De [core-pakket](http://www.nuget.org/packages/Microsoft.ApplicationInsights) biedt de API voor telemetrie verzenden naar de inzichten van toepassing. [Extra pakketten](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) voorzien telemetrie _modules_ en _begin_ telemetrie automatisch bijhouden van uw toepassing en de context. Door het configuratiebestand aan te passen, kunt u inschakelen of uitschakelen van telemetrie modules en begin en parameters instellen voor een aantal van hen.

Het configuratiebestand met de naam `ApplicationInsights.config` of `ApplicationInsights.xml`, afhankelijk van het type van uw toepassing. Wordt deze automatisch toegevoegd aan uw project als u [in de meeste versies van de SDK installeren][start]. Ook wordt toegevoegd aan een web app door [Status Monitor op een IIS-server][redfield], of wanneer u de Appplication inzichten [te verlengen voor een Azure website of VM](app-insights-azure-web-apps.md)selecteert.

Er is niet een equivalente bestand om te bepalen van de [SDK in een webpagina][client].

Dit document beschrijft de secties u in de configuratie ziet bestand, hoe zij bepalen dat de onderdelen van de SDK, en welke pakketten NuGet die onderdelen worden geladen.

## <a name="telemetry-modules-aspnet"></a>Telemetrie Modules (ASP.NET)

Elke module telemetrie een bepaald type gegevens verzamelt en de kern-API gebruikt om de gegevens te verzenden. De modules worden geïnstalleerd door verschillende NuGet-pakketten, die ook de vereiste regels aan het .config-bestand toevoegen.

Er is een knooppunt in het configuratiebestand voor elke module. Het knooppunt verwijderen of opmerking het uitschakelen van een module.



### <a name="dependency-tracking"></a>Bijhouden van afhankelijkheid

[Bijhouden van afhankelijkheid](app-insights-dependencies.md) verzamelt telemetrie over aanroepen van die uw app voor databases en externe diensten en databases maakt. Als u deze module om te werken in een IIS-server, moet u [installeren statuscontrole][redfield]. Te gebruiken in Azure web apps of VMs, [selecteert u de extensie inzichten van toepassing](app-insights-azure-web-apps.md).

U kunt ook uw eigen afhankelijkheid bijhouden met behulp van de [API TrackDependency](app-insights-api-custom-events-metrics.md#track-dependency)code schrijven.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet package.

### <a name="performance-collector"></a>Prestaties, gegevensverzamelaarsets

[Systeemprestatiemeteritems verzamelt](app-insights-performance-counters.md) zoals CPU, geheugen en netwerk laden vanuit IIS-installaties. U kunt aangeven welke items u wilt verzamelen, met inbegrip van de prestatiemeteritems die u zelf hebt gedefinieerd.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet package.


### <a name="application-insights-diagnostics-telemetry"></a>Toepassing inzichten Diagnostics telemetrie

De `DiagnosticsTelemetryModule` rapporteert fouten in de toepassing inzichten instrumentation code zelf. Bijvoorbeeld, als de code geen toegang de prestatiemeters tot of als een `ITelemetryInitializer` een uitzondering. Trace telemetrie bijgehouden door deze module wordt weergegeven in de [Diagnostische zoeken][diagnostic]. Diagnostische gegevens verzendt naar dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package. Als u dit pakket alleen installeren, wordt het bestand ApplicationInsights.config niet automatisch gemaakt. 

### <a name="developer-mode"></a>Modus voor ontwikkelaars

`DeveloperModeWithDebuggerAttachedTelemetryModule`zorgt ervoor dat de toepassing inzichten `TelemetryChannel` voor het verzenden van gegevens onmiddellijk telemetrie één item tegelijk als een debugger wordt gekoppeld aan het toepassingsproces. Dit vermindert de hoeveelheid tijd tussen het moment dat wanneer uw toepassing wordt bijgehouden Telemetrie en wanneer deze wordt weergegeven op de portal Application inzichten. Dit zorgt ervoor dat aanzienlijke overhead in CPU- en bandbreedte.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Toepassing inzichten Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package

### <a name="web-request-tracking"></a>Webaanvraag bijhouden

Rapporten van de [antwoordcode en het resultaat](app-insights-asp-net.md) van een HTTP-aanvragen. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package

### <a name="exception-tracking"></a>Uitzondering bijhouden

`ExceptionTrackingTelemetryModule`sporen van niet-verwerkte uitzonderingen in uw web app. [Fouten en uitzonderingen]Zie[exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`- [uitzonderingen niet in acht genomen taak](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx)wordt bijgehouden.
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-niet-verwerkte uitzonderingen voor werknemer rollen, windows services en consoletoepassingen worden bijgehouden.
* [Toepassing inzichten Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Het Microsoft.ApplicationInsights-pakket bevat de [kern-API](https://msdn.microsoft.com/library/mt420197.aspx) van de SDK. De andere telemetrie modules gebruikt en kunt u ook [gebruiken om uw eigen telemetrie definiëren](app-insights-api-custom-events-metrics.md).

* Geen vermelding in de ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package. Als u alleen deze NuGet installeert, wordt er geen .config-bestand gegenereerd.

## <a name="telemetry-channel"></a>Telemetrie-kanaal

Het kanaal telemetrie beheert bufferen en verzending van de telemetrie voor de service Application inzichten. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`is de standaardkanaal voor services. Deze buffers gegevens in het geheugen.
* `Microsoft.ApplicationInsights.PersistenceChannel`is een alternatief voor consoletoepassingen. Bespaart unflushed gegevens permanent opslaan wanneer uw app wordt afgesloten en het wordt verzonden wanneer de toepassing opnieuw wordt gestart.


## <a name="telemetry-initializers-aspnet"></a>Telemetrie-initalisatiefuncties (ASP.NET)

Telemetrie begin contexteigenschappen instellen die worden verzonden met elk item van telemetrie. 

U kunt het [schrijven van uw eigen begin](app-insights-api-filtering-sampling.md#add-properties) contexteigenschappen wilt instellen.

De standaard begin is ingesteld door de Web- of WindowsServer NuGet pakketten:


* `AccountIdTelemetryInitializer`Hiermee stelt u de eigenschap AccountId.
* `AuthenticatedUserIdTelemetryInitializer`de eigenschap AuthenticatedUserId wordt ingesteld door de SDK voor JavaScript.
* `AzureRoleEnvironmentTelemetryInitializer`updates de `RoleName` en `RoleInstance` eigenschappen van de `Device` context voor alle telemetrie artikelen met informatie uit de Azure runtime-omgeving.
* `BuildInfoConfigComponentVersionTelemetryInitializer`updates de `Version` eigenschap van de `Component` context voor alle telemetrie artikelen met de waarde die is opgehaald uit de `BuildInfo.config` dat is gemaakt door MS Build.
* `ClientIpHeaderTelemetryInitializer`updates `Ip` eigenschap van de `Location` context van alle telemetrie items op basis van de `X-Forwarded-For` HTTP-header van de aanvraag.
* `DeviceTelemetryInitializer`updates van de volgende eigenschappen van de `Device` context voor alle artikelen met telemetrie.
 - `Type`is ingesteld op 'PC'
 - `Id`is ingesteld op de naam van het domein van de computer waarop de toepassing wordt uitgevoerd.
 - `OemName`is ingesteld op de waarde die is opgehaald uit de `Win32_ComputerSystem.Manufacturer` veld met WMI.
 - `Model`is ingesteld op de waarde die is opgehaald uit de `Win32_ComputerSystem.Model` veld met WMI.
 - `NetworkType`is ingesteld op de waarde die is opgehaald uit de `NetworkInterface`.
 - `Language`is ingesteld op de naam van de `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`updates de `RoleInstance` eigenschap van de `Device` context voor alle telemetrie artikelen met de domeinnaam van de computer waarop de toepassing wordt uitgevoerd.
* `OperationNameTelemetryInitializer`updates de `Name` eigenschap van de `RequestTelemetry` en de `Name` eigenschap van de `Operation` context van alle telemetrie items op basis van de HTTP-methode, evenals de namen van ASP.NET MVC-controller en de actie die wordt gestart om de aanvraag te verwerken.
* `OperationIdTelemetryInitializer`of `OperationCorrelationTelemetryInitializer` updates de `Operation.Id` de contexteigenschap van alle telemetrie items bijgehouden tijdens het verwerken van een aanvraag met automatisch gegenereerde `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`updates de `Id` eigenschap van de `Session` context alle telemetrie items waarbij de waarde opgehaald uit de `ai_session` cookie gegenereerd door de instrumentatie ApplicationInsights JavaScript code die wordt uitgevoerd in de browser van de gebruiker. 
* `SyntheticTelemetryInitializer`of `SyntheticUserAgentTelemetryInitializer` updates de `User`, `Session` en `Operation` contexten eigenschappen van alle telemetrie items bijgehouden bij het verwerken van een aanvraag van een kunstmatige bron, zoals een beschikbaarheid testen of de engine bot zoeken. Standaard weergeeft [Metrics Explorer](app-insights-metrics-explorer.md) synthetische telemetrie niet. 

    De `<Filters>` set waarmee eigenschappen van de aanvragen.
* `UserAgentTelemetryInitializer`updates de `UserAgent` eigenschap van de `User` context van alle telemetrie items op basis van de `User-Agent` HTTP-header van de aanvraag.
* `UserTelemetryInitializer`updates de `Id` en `AcquisitionDate` eigenschappen van `User` context voor alle telemetrie artikelen met waarden die zijn opgehaald uit de `ai_user` cookie gegenereerd door de toepassing inzichten JavaScript instrumentation code uitgevoerd in de browser van de gebruiker.
* `WebTestTelemetryInitializer`Hiermee stelt u de gebruikers-id, de sessie-id en de broneigenschappen van synthetische voor HTTP-aanvragen die afkomstig van de [beschikbaarheid van tests zijn](app-insights-monitor-web-app-availability.md).
De `<Filters>` set waarmee eigenschappen van de aanvragen.

## <a name="telemetry-processors-aspnet"></a>Telemetrie-Processors (ASP.NET)

Telemetrie-processors kunnen filteren en elk item telemetrie wijzigen net voordat het wordt verzonden door de SDK op de portal.

U kunt [uw eigen processors telemetrie schrijven](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptieve bemonstering telemetrie processor (2.0.0-beta3)

Dit is standaard ingeschakeld. Als uw app een heleboel telemetrie verzendt, verwijdert deze processor delen van de afbeelding.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

De parameter geeft het doel dat het algoritme probeert te bereiken. Elk exemplaar van de SDK werkt onafhankelijk van elkaar, zodat als uw server een cluster van meerdere machines, de werkelijke omvang van de telemetrie dienovereenkomstig moet worden vermenigvuldigd.

[Meer informatie over de bemonstering](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Vaste rente bemonstering telemetrie processor (2.0.0-beta1)

Er is een standaard [bemonstering telemetrie processor](app-insights-api-filtering-sampling.md#sampling) (2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanaalparameters (Java)

Deze parameters zijn van invloed op hoe de SDK voor Java moet opslaan en leegmaken van de telemetriegegevens die worden verzameld.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Het aantal telemetrie items die kunnen worden opgeslagen in de SDK van geheugenopslag. Als dit aantal is bereikt, de telemetrie-buffer wordt geleegd - dat wil zeggen de telemetrie artikelen worden verzonden naar de server inzichten van toepassing.

-   Min: 1
-   Max: 1000
-   Standaard: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Hiermee bepaalt u hoe vaak de gegevens die zijn opgeslagen in de opslag in het geheugen moet worden leeggemaakt (verzonden naar de inzichten van toepassing).

-   Min: 1
-   Max: 300
-   Standaard: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Hiermee bepaalt u de maximumgrootte in MB die is toegewezen aan de permanente opslag op de lokale schijf. Deze opslag wordt gebruikt voor persistente telemetrie artikelen die niet worden verzonden naar het eindpunt van de inzichten van toepassing. Wanneer de maximale grootte is voldaan, moeten nieuwe telemetrie items worden verwijderd.

-   Min: 1
-   Max: 100
-   Standaard: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Hiermee bepaalt u de toepassing inzichten bron waarin uw gegevens worden weergegeven. Meestal maakt u een afzonderlijke resource met een aparte sleutel voor elke toepassing.

Als u instellen van de sleutel dynamisch - bijvoorbeeld wilt als u resultaten wilt verzenden vanuit de toepassing naar verschillende bronnen - kunt u de sleutel uit het configuratiebestand weglaten en in plaats daarvan in de code instellen.

Als de sleutel is ingesteld voor alle exemplaren van de TelemetryClient, stel inclusief standaard telemetrie modules, de sleutel in TelemetryConfiguration.Active. Dit doet u in een initialisatiemethode, zoals global.aspx.cs in een ASP.NET-service:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Als u alleen verzenden van een specifieke reeks gebeurtenissen aan een andere resource wilt, kunt u de sleutel instellen voor een specifieke TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Meer informatie over de API][api].

Om een nieuwe sleutel, [maakt u een nieuwe bron in de portal Application inzichten][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

