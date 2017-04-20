<properties 
    pageTitle="Het oplossen van problemen en vragen over de inzichten van toepassing" 
    description="Iets in de Visual Studio-toepassing inzichten onduidelijk of niet werkt? Probeer het hier." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Vragen - toepassing inzichten voor ASP.NET

## <a name="configuration-problems"></a>Configuratieproblemen

*Ik heb problemen met het instellen van mijn:*

* [.NET-app](app-insights-asp-net-troubleshoot-no-data.md)
* [Een toepassing al actief toezicht](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure diagnostische gegevens](app-insights-azure-diagnostics.md)
* [Java web app.](app-insights-java-troubleshoot.md)
* [Andere platforms](app-insights-platforms.md)

*Ik krijg geen gegevens van mijn server*

* [Firewall-uitzonderingen instellen](app-insights-ip-addresses.md)
* [Een ASP.NET-server instellen](app-insights-monitor-performance-live-website-now.md)
* [Een Java-server instellen](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Kan ik... inzicht met toepassing gebruiken?

[Zie Platforms][platforms]


## <a name="is-it-free"></a>Is dat gratis?

* Ja, als u ervoor de vrije [prijzen laag kiest](app-insights-pricing.md). Krijgt u de meeste functies en een royale contingent van gegevens. 
* U moet uw creditcardgegevens te registreren bij Microsoft Azure, maar geen kosten komen tenzij u een andere betaald voor Azure service gebruiken, of u expliciet een naar een betalende laag upgrade.
* Als uw app meer gegevens dan de maandelijkse quota voor de vrij laag verzendt, niet meer worden geregistreerd. Als dat gebeurt, kunt u ervoor kiezen om te gaan betalen of wachten totdat de quota aan het einde van de maand wordt ingesteld.
* Gebruiks- en basisgegevens is niet onderworpen aan een contingent.
* Er is ook een gratis proefperiode van 30 dagen, waarin u de betaalde functies gratis ophalen.
* De bron van elke toepassing heeft een aparte quota, en stelt u de prijzen laag onafhankelijk van alle andere.

#### <a name="what-do-i-get-if-i-pay"></a>Wat krijg ik als ik betalen?

* Een grotere [maandelijkse quota van gegevens](https://azure.microsoft.com/pricing/details/application-insights/).
* Optie betalen overschot om door te gaan met het verzamelen van gegevens over de maandelijkse quota. Als uw gegevens over quota gaat, bent u in rekening gebracht per Mb.
* [Doorlopend exporteren](app-insights-export-telemetry.md).


## <a name="q14"></a>Wat inzichten toepassing wijzigen in mijn project?

De details zijn afhankelijk van het type project. Voor een webtoepassing:


+ Deze bestanden worden toegevoegd aan het project:

 + ApplicationInsights.config. 
 + AI.js


+ Deze pakketten NuGet installeert:

 -  *Toepassing inzichten API* - de kern-API

 -  *Toepassing inzichten API voor webtoepassingen* - telemetrie verzenden vanaf de server gebruikt

 -  *Toepassing inzichten API voor JavaScript-toepassingen* - telemetrie van de client verzonden

    De pakketten bevatten deze samenstellen:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Hiermee voegt u items in:

 - Web.config

 - Packages.config

+ (Nieuwe projecten alleen - als u [toepassing inzichten aan een bestaand project toevoegen][start], moet u dit handmatig doen.) Codefragmenten invoegen in de code voor client en server geïnitialiseerd de toepassing inzichten resource-ID. Bijvoorbeeld in een toepassing MVC code ingevoegd in de basispagina Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Hoe voer ik een upgrade van oudere versies van de SDK?

Zie de [release-opmerkingen](app-insights-release-notes.md) voor de SDK nodig voor uw toepassing. 



## <a name="update"></a>Hoe kan ik wijzigen welke Azure resource verzendt gegevens naar van mijn project?

In de Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kies **Update toepassing inzichten**. U kunt de gegevens aan een bestaande of nieuwe resource in Azure verzenden. De wizard update wijzigt de sleutel instrumentation in ApplicationInsights.config, waarmee u bepaalt waar de SDK-server verzendt uw gegevens. Tenzij u 'Alle Update' uitschakelt, wordt de sleutel waar deze wordt weergegeven in uw webpagina's ook gewijzigd.


#### <a name="data"></a>Hoe lang gegevens behouden in de portal? Is het veilig?

Bekijk de [gegevens en Privacy][data].

## <a name="logging"></a>Logboekregistratie

#### <a name="post"></a>Hoe kan ik POST-gegevens in diagnostische zoekopdracht bekijken?

We geen POST-gegevens automatisch aanmelden, maar u kunt een oproep TrackTrace: de gegevens in de parameter bericht plaatsen. Dit heeft een limiet langer dan de grenzen van string-eigenschappen, terwijl u niet kunt filteren. 

## <a name="security"></a>Beveiliging

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Zijn mijn gegevens veilig in de portal? Hoe lang blijft deze behouden?

Zie [gegevens bewaren en Privacy][data].


## <a name="q17"></a>Ik ingeschakeld alles in inzichten van toepassing?

<table border="1">
<tr><th>U ziet</th><th>Hoe</th><th>Waarom u wilt</th></tr>
<tr><td>Beschikbaarheid van grafieken</td><td><a href="../app-insights-monitor-web-app-availability/">Web-tests</a></td><td>Afkomstig is van uw web app</td></tr>
<tr><td>Server app perf: responstijd,...
</td><td><a href="../app-insights-asp-net/">Inzichten van de toepassing aan uw project toevoegen</a><br/>of <br/><a href="../app-insights-monitor-performance-live-website-now/">AI Status Monitor niet installeren op de server</a> (of uw eigen code schrijven voor het <a href="../app-insights-api-custom-events-metrics/#track-dependency">bijhouden van afhankelijkheden</a>)</td><td>Perf problemen detecteren</td></tr>
<tr><td>Telemetrie afhankelijkheid</td><td><a href="../app-insights-monitor-performance-live-website-now/">AI Status Monitor op de server installeren</a></td><td>Vaststellen van problemen met databases of andere externe onderdelen</td></tr>
<tr><td>Stacktraces ophalen van uitzonderingen</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">TrackException aanroepen invoegen in uw code</a> (maar sommige worden automatisch gemeld)</td><td>Opsporen en onderzoeken van uitzonderingen</td></tr>
<tr><td>Logboek-sporen zoeken</td><td><a href="../app-insights-search-diagnostic-logs/">Een registratie-adapter toevoegen</a></td><td>Uitzonderingen, perf problemen vaststellen</td></tr>
<tr><td>Basisinformatie over het gebruik van client: paginaweergaven, sessies,...</td><td><a href="../app-insights-javascript/">Initialiseren van JavaScript in webpagina 's</a></td><td>Gebruiksanalyse</td></tr>
<tr><td>Aangepaste metrics client</td><td><a href="../app-insights-api-custom-events-metrics/">Tracking roept in webpagina 's</a></td><td>Verbeteren van de gebruikerservaring</td></tr>
<tr><td>Aangepaste serverstatistieken</td><td><a href="../app-insights-api-custom-events-metrics/">Gesprekken in de servercode bijhouden</a></td><td>Business intelligence</td></tr>
</table>


## <a name="automation"></a>Automatisering

U kunt in [PowerShell scripts schrijven](app-insights-powershell.md) om te maken en inzichten van toepassing voor resources bijwerken.

## <a name="more-answers"></a>Meer antwoorden

* [Toepassing inzichten forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 