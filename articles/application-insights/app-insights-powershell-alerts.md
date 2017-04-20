<properties
    pageTitle="Gebruik Powershell waarschuwingen instellen in toepassing inzichten"
    description="Automatiseren van de configuratie van toepassing inzichten e-mails over metrische wijzigingen ophalen."
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
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Gebruik PowerShell waarschuwingen instellen in toepassing inzichten

U kunt de configuratie van [waarschuwingen](app-insights-alerts.md) in [Inzichten van Visual Studio-toepassing](app-insights-overview.md)kunt automatiseren.

Bovendien kunt u [webhooks voor het automatiseren van uw reactie op een signaal](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Eenmalige setup

Als u nog niet met uw abonnement Azure voordat PowerShell gebruikt:

De module Azure Powershell installeren op de computer waarop de scripts worden uitgevoerd.

 * Installeer [Microsoft Web Platform Installer (v5 of hoger)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Installeert u Microsoft Azure Powershell


## <a name="connect-to-azure"></a>Verbinding maken met Azure

Start Azure PowerShell en [verbinding maken met uw abonnement](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Ontvang berichten

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Een waarschuwing toevoegen


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Voorbeeld 1

Stuur me een e-mail als reactie op HTTP-verzoeken, gemiddeld meer dan 5 minuten van de server trager dan 1 seconde is. Mijn toepassing inzichten resource heet IceCreamWebApp en is in de resourcegroep Fabrikam. Ik ben de eigenaar van het abonnement Azure.

De GUID is de abonnement-ID (niet de sleutel instrumentatie van de toepassing).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Voorbeeld 2

Ik heb een toepassing waarin ik met [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) rapporteren de metric-waarde met de naam 'salesPerHour'. Stuur dat een e-mail naar Mijn collega's als 'salesPerHour' onder de 100, gemiddeld meer dan 24 uur.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Dezelfde regel kan worden gebruikt voor de metric die wordt gerapporteerd met behulp van de [parameter meting](app-insights-api-custom-events-metrics.md#properties) van het bijhouden van een andere aanroep zoals TrackEvent of trackPageView.

## <a name="metric-names"></a>Metrische namen

Metrische naam | Schermnaam | Beschrijving
---|---|---
`basicExceptionBrowser.count`|Browser-uitzonderingen|Het aantal niet-afgevangen uitzonderingen in de browser.
`basicExceptionServer.count`|-Uitzonderingen|Het aantal niet-verwerkte uitzonderingen die door de app.
`clientPerformance.clientProcess.value`|Verwerkingstijd van client|De tijd tussen de laatste byte van een document ontvangen totdat het DOM is geladen. Asynchrone aanvragen kunnen worden verwerkt.
`clientPerformance.networkConnection.value`|Pagina laden netwerk verbindingstijd| Duurt de browser verbinding maken met het netwerk. Is 0 als in de cache opgeslagen.
`clientPerformance.receiveRequest.value`|Responstijd ontvangen| De tijd tussen de browser aanvraag verzenden te starten om antwoord te ontvangen.
`clientPerformance.sendRequest.value`|Aanvraagtijd verzenden| Tijd die door de browser aanvraag versturen.
`clientPerformance.total.value`|Browser pagina laadtijd|Tijdstip van de aanvraag van de gebruiker totdat DOM, opmaakmodellen, scripts en afbeeldingen worden geladen.
`performanceCounter.available_bytes.value`|Beschikbaar geheugen|Fysiek geheugen onmiddellijk beschikbaar is voor een proces of systeem gebruikt.
`performanceCounter.io_data_bytes_per_sec.value`|Proces i/o-snelheid|Totaal aantal bytes per seconde gelezen van en geschreven naar bestanden, het netwerk en apparaten.
`performanceCounter.number_of_exceps_thrown_per_sec`|uitzondering tarief|Uitzonderingen per seconde.
`performanceCounter.percentage_processor_time.value`|CPU proces|Het percentage van de verstreken tijd van alle procesthreads gebruikt door de processor instructies voor het proces toepassingen.
`performanceCounter.percentage_processor_total.value`|Processortijd|Het percentage tijd dat de processor nodig in een niet-actieve threads heeft.
`performanceCounter.process_private_bytes.value`|Proces eigen bytes|Geheugen uitsluitend toegewezen aan processen van de gecontroleerde toepassingen.
`performanceCounter.request_execution_time.value`|Uitvoertijd van aanvraag voor ASP.NET|Uitvoeringstijd van de meest recente aanvraag.
`performanceCounter.requests_in_application_queue.value`|ASP.NET-aanvragen in de wachtrij kan worden uitgevoerd|De lengte van de wachtrij voor aanvragen van toepassing.
`performanceCounter.requests_per_sec`|Tarief voor ASP.NET-aanvraag|De frequentie van alle aanvragen voor de toepassing per seconde van ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Afhankelijkheid fouten|Het aantal mislukte oproepen door de servertoepassing tot externe bronnen.
`request.duration`|Reactietijd van de server|De tijd tussen de ontvangst van een HTTP-aanvraag en het antwoord wordt verzonden is voltooid.
`request.rate`|Tarief aanvragen|De frequentie van alle aanvragen voor de toepassing per seconde.
`requestFailed.count`|Mislukte aanvragen|Telling van HTTP-aanvragen dat heeft geresulteerd in een reactiecode > = 400
`view.count`|Paginaweergaven|Het aantal client gebruiker voor een webpagina. Synthetische verkeer wordt gefilterd.
{uw aangepaste metrische naam}|{Metrische naam}|De metrische waarde gemeld door [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) of in de [parameter van de metingen van een gesprek volgen](app-insights-api-custom-events-metrics.md#properties).

De parameters worden verzonden door andere telemetrie modules:

Metrische groep | Module collector
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>weergave | [Browser JavaScript](app-insights-javascript.md)
performanceCounter | [Prestaties](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Afhankelijkheid](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
verzoek,<br/>requestFailed|[Serveraanvraag](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

U kunt [uw reactie op een signaal te automatiseren](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure wordt een web-adres van uw keuze aangeroepen wanneer een waarschuwing wordt gegenereerd.

## <a name="see-also"></a>Zie ook


* [Script inzichten toepassing configureren](app-insights-powershell-script-create-resource.md)
* [Toepassing inzichten en bronnen op het web test maken van sjablonen](app-insights-powershell.md)
* [Koppeling Microsoft Azure diagnostische gegevens naar inzichten toepassing automatiseren](app-insights-powershell-azure-diagnostics.md)
* [Uw reactie op een signaal automatiseren](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
