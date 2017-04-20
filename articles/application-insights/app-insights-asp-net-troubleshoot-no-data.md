<properties 
    pageTitle="Er zijn geen gegevens - toepassing inzichten voor .NET oplossen" 
    description="Gegevens in een Visual Studio-toepassing inzichten niet zien? Probeer het hier." 
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
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Er zijn geen gegevens - toepassing inzichten voor .NET oplossen

## <a name="some-of-my-telemetry-is-missing"></a>Sommige van mijn telemetrie ontbreekt

*In de toepassing van inzichten Zie ik slechts een fractie van de gebeurtenissen die worden gegenereerd door mijn app.*

* Als u consequent de dezelfde fractie ziet, is het waarschijnlijk vanwege de adaptieve [bemonstering](app-insights-sampling.md). Om dit te controleren, zoeken openen (van het overzicht blade) en zoek naar een exemplaar van een aanvraag of een andere gebeurtenis. Klik op "..." Als u volledige informatie onder aan de sectie-eigenschappen. Als Count > 1 aanvragen en vervolgens de bemonstering wordt uitgevoerd. 
* Anders is het mogelijk dat u bent kunt u door een [gegevenssnelheid beperken](app-insights-pricing.md#limits-summary) voor de planning van de prijzen. Deze beperkingen worden toegepast per minuut.

## <a name="no-data-from-my-server"></a>Er zijn geen gegevens van mijn server

*Ik mijn app op mijn webserver geïnstalleerd en nu zie ik niet alle telemetrie uit. Het heeft gewerkt OK op mijn dev machine.*

* Waarschijnlijk een probleem met firewall. [Firewalluitzonderingen voor de inzichten van toepassing voor het verzenden van gegevens instellen](app-insights-ip-addresses.md).

*Ik [Statuscontrole geïnstalleerd](app-insights-monitor-performance-live-website-now.md) op de webserver om te controleren van bestaande apps. Ik zie geen resultaten opgeleverd.*

* Zie [Statuscontrole voor het oplossen van problemen](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Geen optie 'Toepassing inzichten toevoegen' in Visual Studio

*Wanneer maak ik een nieuw project in Visual Studio, of als ik een bestaand project in de Solution Explorer met de rechtermuisknop, zie ik niet alle opties van de inzichten van toepassing.*

+ Niet alle soorten .NET project worden ondersteund door de hulpprogramma's. Web- en WCF projecten worden ondersteund. Voor andere projecttypen zoals desktop- of toepassingen kunt u nog steeds [handmatig een inzichten Application SDK aan uw project toevoegen](app-insights-windows-desktop.md).
+ Zorg ervoor dat u beschikt over [Visual Studio 2013 Update 3 of hoger](http://go.microsoft.com/fwlink/?LinkId=397827). Wordt geleverd met hulpmiddelen voor inzicht van toepassingen voorgeïnstalleerd.
+ Selecteer **hulpprogramma's**, **uitbreidingen en Updates** en controleer dat **Inzichten hulpprogramma's** is geïnstalleerd en ingeschakeld. Als dit het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
+ Open het dialoogvenster Nieuw Project en kiest u ASP.NET-webtoepassing. Als u de optie toepassing inzichten er ziet, worden de hulpprogramma's geïnstalleerd. Als dat niet het geval is, probeert te verwijderen en vervolgens opnieuw installeren van de hulpprogramma's inzichten.


## <a name="q02"></a>Toepassing inzichten toevoegen is mislukt

*Ik zie een foutbericht wanneer ik een nieuwe webproject maakt, of wanneer ik probeer inzichten toepassing toevoegen aan een bestaand project.*

Mogelijke oorzaken:

* Communicatie met de portal Application inzichten is mislukt; of
* Er is een probleem met uw account Azure;
* U alleen hebben [leestoegang](app-insights-resources-roles-access-control.md)tot het abonnement of de groep waar u probeerde te maken van de nieuwe resource.

FIX:

+ Controleer of u referenties hebt opgegeven voor de account rechts Azure. 
+ Controleer dat u toegang tot de [Azure portal hebt](https://portal.azure.com)in uw browser. Open instellingen en of er een beperking is.
+ [Inzichten van toepassing toevoegen aan uw bestaande project](app-insights-asp-net.md): Solution Explorer uw project met de rechtermuisknop op en kies "Add Application inzichten."
+ Als het nog steeds niet werkt, voert u de [Handmatige procedure](app-insights-windows-services.md) voor een resource in de portal toevoegen en vervolgens de SDK aan uw project toevoegen. 

## <a name="emptykey"></a>Ik krijg een foutmelding ' Instrumentation sleutel mag niet leeg zijn'

Lijkt erop dat er iets misgegaan is tijdens installatie toepassing inzichten of misschien een adapter voor logboekregistratie.

In de Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kies de **Inzichten van toepassing configureren**. Krijgt u een dialoogvenster met een uitnodiging aan te melden Azure en maak een bron inzichten van toepassing, of een bestaande opnieuw gebruiken.


##<a name="NuGetBuild"></a>"NuGet pakket(ten) ontbreken" op mijn server bouwen

*Alles OK is gebaseerd wanneer ik ben foutopsporing op mijn machine ontwikkeling, maar ik krijg een fout NuGet op de server maken.*

Zie [NuGet pakket herstellen](http://docs.nuget.org/Consume/Package-Restore) en [Automatische pakket herstellen](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Ontbrekende opdracht toepassing inzichten vanuit Visual Studio openen

*Ik zie geen opdrachten inzichten van toepassing wanneer ik mijn project Solution Explorer met de rechtermuisknop, of een opdracht openen inzichten van toepassing wordt niet weergegeven.*

Mogelijke oorzaken:

* Als u de resource van toepassing inzichten die handmatig zijn gemaakt, of als het project van een type dat niet wordt ondersteund door de tools inzichten van toepassing is.
* De tools inzichten van toepassing zijn uitgeschakeld in de Visual Studio.
* De Visual Studio is ouder dan 2013 Update 3.

FIX:

* Zorg ervoor dat uw versie van Visual Studio 2013 update 3 of hoger.
* Selecteer **hulpprogramma's**, **uitbreidingen en Updates** en controleer dat **Inzichten hulpprogramma's** is geïnstalleerd en ingeschakeld. Als dit het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
* Met de rechtermuisknop op het project in de Solution Explorer. Als u de opdracht **Toepassing inzichten configureren**wordt weergegeven, gebruikt u uw project verbinding te maken met de bron in de service Application inzichten.


Anders wordt is niet uw project rechtstreeks ondersteund door de tools inzichten van toepassing. Voor een overzicht van de telemetrie, aanmelden bij de [Azure portal](https://portal.azure.com), kiest u inzichten van toepassing op de linkernavigatiebalk en selecteer uw toepassing.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>'Toegang geweigerd' op de inzichten van de toepassing openen vanuit Visual Studio

*De opdracht 'Open toepassing inzichten' Ga ik naar de Azure portal, maar ik krijg een foutbericht 'toegang geweigerd'.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

De Microsoft aanmelden die u het laatst is gebruikt in de browser geen toegang tot [de bron die is gemaakt tijdens de inzichten van toepassing is toegevoegd aan deze toepassing](app-insights-asp-net.md)zijn. Er zijn twee mogelijke redenen: 

* U hebt meer dan één Microsoft-account - misschien een werk en een persoonlijke Microsoft-account? Het aanmelden die u het laatst is gebruikt in uw standaardbrowser was voor een andere account dan de toegang tot de [toepassing inzichten aan het project toevoegen](app-insights-asp-net.md). 

 * FIX: Klik op de naam op van uw top-rechts van het browservenster en afmelden. Meld u aan met de account die toegang heeft. Klik op de linkernavigatiebalk op van toepassing inzichten en selecteer uw app.

* Iemand anders inzichten toepassing toegevoegd aan het project en ze vergeten te geven u [toegang tot de resourcegroep](app-insights-resources-roles-access-control.md) waarmee deze is gemaakt. 

 * FIX: Als ze een organisatie-account gebruikt, ze kunnen u toevoegen aan het team; of ze kunnen u afzonderlijke toegang verlenen aan de resourcegroep.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Activa niet gevonden' op de inzichten van de toepassing openen vanuit Visual Studio

*De opdracht 'Open toepassing inzichten' Ga ik naar de Azure portal, maar ik krijg een foutmelding 'activa niet gevonden'.*

Mogelijke oorzaken:

* De bron van de inzichten van toepassing voor uw toepassing is verwijderd; of
* De sleutel instrumentation is ingesteld of gewijzigd in ApplicationInsights.config door het direct bewerken zonder het project-bestand bijwerken. 

De sleutel instrumentation in ApplicationInsights.config besturingselementen waarbij de telemetrie wordt verzonden. Een regel in het project-bestand bepaalt welke resource wordt geopend wanneer u de opdracht in Visual Studio gebruiken. 

FIX:

* In Solution Explorer met de rechtermuisknop op het project en kies toepassing inzichten, ideeën voor toepassingen configureren. U kunt in het dialoogvenster, hetzij telemetrie verzenden naar een bestaand middel of een nieuwe maken. Of:
* De resource rechtstreeks openen. Aanmelden bij [de Azure portal](https://portal.azure.com), klikt u op toepassing inzichten in de linkernavigatiebalk en selecteer vervolgens uw app.



## <a name="where-do-i-find-my-telemetry"></a>Waar vind ik mijn telemetrie

*Ik heb ondertekend in de [portal van Microsoft Azure](https://portal.azure.com)en ik zoek op het dashboard voor Azure thuis. Dus waar vind ik mijn gegevens inzichten van toepassing?*

* Klik op de linkernavigatiebalk inzichten van toepassing, wordt de naam van uw app. Als er geen projecten bevat, moet u [toevoegen](app-insights-asp-net.md)of configureren van inzicht in uw webproject Application.

    U ziet er sommige grafieken overzicht. Kunt u ze allemaal meer detail te zien.

* Klik op de knop toepassing inzichten in Visual Studio, terwijl u uw app in Foutopsporing.


## <a name="q03"></a>Er zijn geen servergegevens (geen gegevens of helemaal)

*Ik heb mijn app uitgevoerd en vervolgens de service Application inzichten opent in Microsoft Azure, maar alle grafieken weergeven 'Informatie over het verzamelen van...' of 'Niet geconfigureerd'.* Of, *alleen gegevens in de weergave pagina en de gebruiker, maar geen servergegevens.*

+ Uw toepassing uitvoeren in de foutopsporingsmodus in Visual Studio (F5). Gebruik de toepassing dat sommige telemetrie genereren. Controleer of u gebeurtenissen die zijn vastgelegd in het venster Visual Studio output kunt zien. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Open [Diagnostische zoeken](app-insights-diagnostic-search.md)in de portal Application inzichten. Gegevens verschijnt meestal hier eerst.
+ Klik op de knop vernieuwen. Het blad wordt vernieuwd regelmatig, maar u kunt ook dit handmatig doen. Het vernieuwingsinterval is langer voor grotere tijd bereiken.
+ Controleer dat de sleutels instrumentation overeenkomen. Kijk op de belangrijkste blade voor uw app in de portal Application inzichten in de vervolgkeuzelijst **Essentials** **Instrumentation sleutel**. In het project in Visual Studio, open vervolgens ApplicationInsights.config en zoek de `<instrumentationkey>`. Controleer of de twee sleutels gelijk zijn. Als dit niet het geval:
 + In de portal Application inzichten op en zoek de bron app met de juiste sleutel; of
 + In Visual Studio Solution Explorer met de rechtermuisknop op het project en kies toepassing inzichten, configureren. Opnieuw de app telemetrie verzenden naar de juiste bron.
 + Als u de bijbehorende sleutels niet kunt vinden, moet u controleren dat u dezelfde referenties-in in Visual Studio, zoals in de portal gebruikt.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ In het [Microsoft Azure thuis dashboard](https://portal.azure.com)de gezondheid Service kaart bekijken. Als er enige waarschuwing aanwijzingen, wacht u totdat ze hebt teruggezonden naar OK sluit en opnieuw uw toepassing inzichten toepassing blade opent.
+ Controleer tevens [de status blog](http://blogs.msdn.com/b/applicationinsights-status/).
+ Heeft u een code te schrijven voor de [server-side SDK](app-insights-api-custom-events-metrics.md) die kunnen veranderen van de sleutel instrumentation in `TelemetryClient` exemplaren of in `TelemetryContext`? Of hebt u een [filter of bemonstering configuratie](app-insights-api-filtering-sampling.md) die door het filter worden mogelijk te veel?
+ Als u ApplicationInsights.config hebt bewerkt, moet u zorgvuldig de configuratie van [TelemetryInitializers en TelemetryProcessors](app-insights-api-filtering-sampling.md)controleren. Een verkeerde naam type of parameter kan leiden tot de SDK om geen gegevens te verzenden.

## <a name="q04"></a>Er zijn geen gegevens over het gebruik van paginaweergaven, Browsers,

*Gegevens in grafieken reactietijd van de Server en de Server aanvraagt, maar geen gegevens in de laadtijd van pagina weergeven of weergegeven in de Browser of gebruik de blades.*

De gegevens afkomstig van scripts op de webpagina's. 

+ Als u een toepassing inzichten toegevoegd aan een bestaand webproject, [hebt u de scripts toevoegen met de hand](app-insights-javascript.md).
+ Zorg ervoor dat Internet Explorer uw site niet wordt weergegeven in de compatibiliteitsmodus.
+ Functie voor foutopsporing van de browser (F12 op sommige browsers, kies netwerk) gebruiken om te controleren of de gegevens wordt verzonden naar `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Geen afhankelijkheid of uitzondering gegevens

Zie [afhankelijkheid telemetrie](app-insights-asp-net-dependencies.md) en [telemetrie uitzondering](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Geen prestatiegegevens

Prestatiegegevens (CPU, I/O snelheid, enzovoort) voor [Java webservices](app-insights-java-collectd.md), [Windows desktop apps](app-insights-windows-desktop.md) [IIS web-apps en services als u de statuscontrole installeert](app-insights-monitor-performance-live-website-now.md)en [Azure Cloud Services](app-insights-azure.md)beschikbaar is. u vindt deze onder instellingen voor Servers.

Het is niet beschikbaar voor Azure websites.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Geen gegevens (server) sinds ik de app op mijn server is gepubliceerd

+ Controleer of u alle Microsoft daadwerkelijk gekopieerd. ApplicationInsights dll-bestanden naar de server, samen met Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ Mogelijk moet u [enkele TCP-poorten](app-insights-ip-addresses.md#data-access-api)in uw firewall.
+ Als er een proxyserver gebruikt voor het verzenden van uw bedrijfsnetwerk, [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) in Web.config instellen
+ Windows Server 2008: Zorg ervoor dat u hebt de volgende updates geïnstalleerd: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt.

* Controleer de [status blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u uw maandelijkse quota van gegevenspunten raakt? Open de instellingen/Quota en prijzen om erachter te komen. In dat geval kunt u uw planning bijwerken of betalen voor extra capaciteit. Zie het [tarievenstelsel](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik heb verwacht

Als uw toepassing veel gegevens worden verzonden en u de toepassing inzichten SDK voor ASP.NET versie 2.0.0-beta3 of hoger gebruikt, kan de functie [apparaatafhankelijke bemonstering](app-insights-sampling.md) werken en slechts een percentage van de telemetrie verzenden. 

U kunt deze optie uitschakelen, maar dit wordt niet aanbevolen. Bemonstering is zodanig ontworpen dat verwante telemetrie correct wordt verzonden, voor diagnostische doeleinden. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Verkeerde geografische gegevens in gebruiker telemetrie

De plaats, regio en land dimensies zijn afgeleid van de IP-adressen en niet altijd juist.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Uitzondering 'methode niet gevonden' op in Azure Cloud Services wordt uitgevoerd

U voor .NET 4.6 opbouwen? 4.6 wordt niet automatisch ondersteund in Azure Cloud Services-rollen. [4.6 installeren op elke rol](../cloud-services/cloud-services-dotnet-install-dotnet.md) voordat de toepassing wordt uitgevoerd.

## <a name="still-not-working"></a>Nog steeds werkt niet...

* [Toepassing inzichten forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

