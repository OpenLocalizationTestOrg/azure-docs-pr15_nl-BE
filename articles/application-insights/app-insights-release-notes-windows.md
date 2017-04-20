<properties 
    pageTitle="Release-opmerkingen voor toepassing inzichten voor Windows" 
    description="De nieuwste updates voor Windows Store SDK." 
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
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Release-opmerkingen voor toepassing inzichten SDK voor Windows Phone en op te slaan

De toepassing inzichten SDK stuurt verzamelen over uw live app naar [Inzichten van toepassing](https://azure.microsoft.com/services/application-insights/), waar u, gebruik en de prestaties analyseren kunt.


## <a name="version-111"></a>Versie 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Vast een loopt vast tijdens vastlopen bij het gebruik van de Windows Phone-Silverlight SDK. Elke crash die later dan ~ 2 seconden na het aanroepen van WindowsAppInitialier.InitializeAsync(...) gebeurt na deze wijziging wordt gehandhaafd voor schijf en wordt verzonden de volgende keer dat de toepassing wordt gestart. Als een crash vóór ~ 2 seconden na het gesprek gebeurt, wordt dit genegeerd.  
- De NuGet van de afhankelijkheden instellen op een specifieke versie van de kern- en Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Core SDK

- Core wordt beheerd in de github. Toekomstige release-informatie van de Core-SDK kunnen u vinden [in de github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Versie 1.1

### <a name="core-sdk"></a>Core SDK

- Nu introduceert nieuwe telemetrie type SDK ```DependencyTelemetry``` die informatie bevat over de oproep afhankelijkheid van toepassing
- Nieuwe methode ```TelemetryClient.TrackDependency``` kunt u voor het verzenden van informatie over het aanroepen van de afhankelijkheid van toepassing

## <a name="version-100"></a>Versie 1.0.0

### <a name="windows-app-sdk"></a>SDK voor Windows App

- Nieuwe initialisatie van de Windows-Apps. Nieuwe `WindowsAppInitializer` klasse met `InitializeAsync()` methode kan voor het bootstrappen van de initialisatie van de SDK-collectie. Deze wijziging kunt meer controle en belangrijke app initialisatie prestaties over vorige ApplicationInsights.config techniek.
- DeveloperMode wordt niet meer automatisch ingesteld. DeveloperMode om gedrag te wijzigen die moet u in de code.
- NuGet package injects niet meer ApplicationInsights.config. Aanbevolen gebruik van de nieuwe WindowsAppInitializer tijdens het handmatig toevoegen van NuGet package.
- ApplicationInsights.config leest alleen `<InstrumentationKey>`, alle andere instellingen in de voorkeuren voor WindowsAppInitializer-instellingen worden genegeerd.
- Winkel markt worden automatisch verzameld door de SDK.
- Een heleboel bugfixes en stabiliteitsverbeteringen in en prestatieverbeteringen.

### <a name="core-sdk"></a>Core SDK

- ApplicationInsights.config bestand is niet langer requiered. En worden niet toegevoegd door het pakket NuGet. Configuratie kan volledig worden opgegeven in de code.
- NuGet package wordt niet langer een doelbestand toevoegen aan uw oplossing. Hiermee verwijdert u de automatische instelling van DeveloperMode tijdens een foutopsporingsversie. DeveloperMode moet handmatig worden ingesteld in de code.

## <a name="version-017"></a>Versie 0.17

### <a name="windows-app-sdk"></a>SDK voor Windows App

- SDK voor Windows-App ondersteunt nu Universal Windows Apps gemaakt tegen technische preview van Windows 10 en met de VS 2015 RC.

### <a name="core-sdk"></a>Core SDK

- De standaardinstelling is TelemetryClient om te initialiseren met de InMemoryChannel.
- Nieuwe API toegevoegd, `TelemetryClient.Flush()`. Deze methode Flush blokkerende uploaden direct alle telemetrie vastgelegd dat de client moet worden geactiveerd. Hierdoor kunnen handmatig activeren van uploaden voordat de procedure wordt afgesloten.
- NuGet pakket toegevoegd een doel .net 4.5. Dit doel heeft geen externe afhankelijkheden (verwijderde BCL gebeurtenisbron afhankelijkheden en).

## <a name="version-016"></a>Versie 0.16 

2015-04-28-voorbeeld

- SDK ondersteunt nu het doelplatform DNX zodat de controle van de [kern van de .NET framework](http://www.dotnetfoundation.org/NETCore5) -toepassingen.
- Instanties van ```TelemetryClient``` Instrumentation sleutel niet meer in cache. Nu als instrumentation sleutel niet is ingesteld in ```TelemetryClient``` expliciet ```InstrumentationKey``` wordt null geretourneerd. Er wordt een probleem verholpen wanneer u ```TelemetryConfiguration.Active.InstrumentationKey``` na sommige telemetrie al is verzameld, verzamelaar afhankelijkheid, zoals telemetrie modules, web aanvragen-collectie en de prestaties tellers gegevensverzamelaarset nieuwe instrumentation-sleutel wordt gebruikt.

## <a name="version-015"></a>Versie 0,15

- Nieuwe eigenschap ```Operation.SyntheticSource``` nu beschikbaar op ```TelemetryContext```. U kunt nu uw telemetrie objecten markeren als "niet een echte gebruikersverkeer" en opgeven hoe dit verkeer is gegenereerd. Als een voorbeeld van het instellen van deze eigenschap kunt u verkeer van uw test automatisering van load test verkeer te onderscheiden.
- Kanaal logica is verplaatst naar de afzonderlijke NuGet Microsoft.ApplicationInsights.PersistenceChannel genoemd. Standaardkanaal heet nu InMemoryChannel
- Nieuwe methode ```TelemetryClient.Flush``` kunnen items telemetrie synchroon uit de buffer leegmaken

## <a name="version-013"></a>Versie 0,13

Geen release-opmerkingen voor oudere versies beschikbaar. 
