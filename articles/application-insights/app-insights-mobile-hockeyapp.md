<properties
    pageTitle="Prestaties controleren voor het mobiele web apps met ontwikkelaars Analytics | Microsoft Azure"
    description="Prestaties en het toezicht op gebruik voor mobiele app-ontwikkelaars. , desktop, web-service en back-end-toepassingen met HockeyApp en inzichten van toepassing."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobiele Analytics met HockeyApp en inzichten van toepassing

Toezicht op de prestaties en het gebruik van de bèta-test en geïmplementeerde mobiele en desktop apps met [HockeyApp](https://hockeyapp.net/). Toezicht op de ondersteunende web service en back-end-toepassingen met [Visual Studio-toepassing inzichten](app-insights-overview.md). Analyseren van gegevens op de client en de server apps in Analytics en de grafieken naast elkaar weergeven in een dashboard Azure.

Microsoft Developer Analytics biedt twee oplossingen voor prestatiecontrole van toepassing:

* **HockeyApp voor mobiel** en desktop apps.
 * Testversies aan geselecteerde gebruikers distribueren.
 * Crash analysis.
 * Aangepaste telemetrie voor gebruiksanalyse.
* **Toepassing inzichten voor websites** en -services en back-end-toepassingen.
 * Prestatie- en gebruiksrapporten maatstaven en waarschuwingen.
 * Uitzondering rapportering en tracering van diagnostische.
 * Diagnostische zoeken met filteren en verwante koppelingen voor telemetrie.

Beide oplossingen bieden:

 * Krachtige **[analytische querytaal](app-insights-analytics.md)** voor diagnose en analyse.
 * **[Gegevens exporteren](app-insights-export-telemetry.md)** naar uw eigen opslag.
 * **Geïntegreerde** dashboardweergave van analytische grafieken en tabellen.

## <a name="monitor-your-app-components"></a>De onderdelen van uw app controleren

Volg de instructies op deze pagina's de SDK installeren in uw code en bijhouden van uw app.

### <a name="web-apps---application-insights"></a>Web apps - toepassing inzichten

* [ASP.NET web app.](app-insights-asp-net.md) 
* [Java web app.](app-insights-java-get-started.md)
* [Node.js web app.](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure Cloud Services](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobiele apps - HockeyApp

* [iOS-app](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X app](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android app](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Universele Windows app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [App voor Windows Phone 8 en 8.1](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Presentatie Windows Foundation-app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Voor Windows desktop apps, raden wij aan HockeyApp. Maar u kunt ook de [telemetrie van een Windows-toepassing inzichten toepassingen verzenden](app-insights-windows-desktop.md). U kunt dit doen als u wilt experimenteren met inzichten van toepassing.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analytics en Export voor telemetrie van HockeyApp

U kunt aangepaste HockeyApp onderzoeken en meld u met de functies Analytics en continue exporteren van toepassing inzichten door [het instellen van een brug](app-insights-hockeyapp-bridge-app.md)telemetrie.




