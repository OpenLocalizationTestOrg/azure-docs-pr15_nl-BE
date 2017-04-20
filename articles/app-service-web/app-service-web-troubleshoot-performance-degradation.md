<properties
    pageTitle="Vertragen web app in de App-Service | Microsoft Azure"
    description="Dit artikel helpt u bij trage web app prestatieproblemen oplossen in Azure App-Service."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web app prestaties, langzaam app, langzaam app"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Langzame web app prestatieproblemen oplossen in Azure App Service

Dit artikel helpt u bij trage web app prestatieproblemen oplossen in [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Als u meer Help-informatie op elk punt in dit artikel, kunt u contact opnemen met de Azure experts op [de MSDN Azure en de forums stackoverloop](https://azure.microsoft.com/support/forums/). Ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [website voor ondersteuning van Azure](https://azure.microsoft.com/support/options/) en klik op **Ondersteuning krijgen**.

## <a name="symptom"></a>Symptoom

Wanneer u surfen op het web app, het laden van pagina's traag en soms time-out.

## <a name="cause"></a>Oorzaak

Dit probleem wordt vaak veroorzaakt door problemen met niveau van toepassingen, zoals:

-   aanvragen duurt erg lang
-   toepassing met behulp van hoge geheugen/CPU
-   toepassing vastlopen vanwege een uitzondering.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

Het oplossen van problemen kan worden onderverdeeld in drie afzonderlijke taken in de juiste volgorde:

1.  [Observeren, en controleert de werking van de toepassing](#observe)
2.  [Gegevens verzamelen](#collect)
3.  [Het probleem oplossen](#mitigate)

[App Service Web Apps](/services/app-service/web/) hebt u verschillende opties bij elke stap.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. Houd en gedrag van de toepassing controleren

#### <a name="track-service-health"></a>Bijhouden van gezondheid Service

Microsoft Azure publicizes telkens wanneer er een service wordt onderbroken of de prestaties afbraak is. U kunt de gezondheid van de service op de [Azure Portal](https://portal.azure.com/)bijhouden. Zie voor meer informatie, [Track service gezondheid](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Controleren van uw web app

Met deze optie kunt u meer als uw toepassing problemen ondervindt. Klik op de tegel **aanvragen en fouten** in uw webtoepassing-blade. De **Metric** blade ziet u de parameters die u kunt toevoegen.

Sommige van de criteria die u wilt controleren voor uw web app

-   Gemiddelde geheugen werkset
-   Gemiddelde reactietijd
-   CPU-tijd
-   Geheugen-werkset
-   Aanvragen

![web app-prestaties controleren](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Zie voor meer informatie:

-   [Monitor Web Apps in Azure App-Service](web-sites-monitor.md)
-   [Meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web Eindpuntstatus controleren

Als u uw web app in de **standaard** prijzen laag, kunt Web Apps bewaken 2 eindpunten van 3 geografische locaties.

Controle eindpunt configureert web tests van geo gedistribueerde locaties met responstijd en de uptime van web-URL's te testen. De test wordt uitgevoerd een HTTP GET-bewerking op de URL van de uptime en responstijd bepalen vanaf elke locatie. Elke geconfigureerde locatie wordt een test om de vijf minuten uitgevoerd.

Uptime wordt gecontroleerd met behulp van HTTP responscodes, en responstijd wordt gemeten in milliseconden. Een controle-test mislukt als de HTTP-reactiecode groter dan of gelijk zijn aan 400 is of als het antwoord langer dan 30 seconden duurt. Een eindpunt wordt beschouwd als beschikbaar als monitoring tests uitgevoerd vanaf de opgegeven locaties.

Als u wilt instellen, Zie [hoe: web Eindpuntstatus controleren](web-sites-monitor.md#webendpointstatus).

Zie ook [Azure-websites te houden van en toezicht op eindpunt - met Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) voor een video over het controleren van het eindpunt.

#### <a name="application-performance-monitoring-using-extensions"></a>Toepassing prestatiecontrole Extensions gebruiken

De toepassingsprestaties van uw kunt u ook controleren door gebruik te maken van de _site-extensies_.

Elke App Service web app biedt een eindpunt extensible management kunt u gebruikmaken van een set krachtige hulpmiddelen die zijn geïmplementeerd als site-extensies. Deze extra bereik van source code-editors zoals [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) management tools voor gekoppelde bronnen zoals een MySQL-database die is verbonden met een web app.

[Azure toepassing inzichten](/services/application-insights/) en [Nieuwe Relic](/marketplace/partners/newrelic/newrelic/) zijn twee van de site-uitbreidingen die beschikbaar zijn voor prestatiecontrole. Voor het gebruik van nieuwe Relic, kunt u een agent tijdens runtime installeren. U opnieuw uw code met een SDK voor het gebruik van de inzichten van Azure toepassing en kunt u een extensie die toegang tot aanvullende gegevens biedt installeren. De SDK kunt u code schrijven voor het gebruik en de prestaties van uw app in meer detail controleren.

Zie [prestaties controleren in webtoepassingen](../application-insights/app-insights-web-monitor-performance.md)voor het gebruik van de inzichten van toepassing.

Zie voor het gebruik van nieuwe Relic [Nieuwe Relic Application Performance Management op Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. gegevens verzamelen

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Diagnostische logboekregistratie inschakelen voor uw web app

Het Web Apps-omgeving biedt diagnostische functies voor informatie over de logboeken van de webserver en de webtoepassing. Deze zijn logisch onderverdeeld in web server en de diagnostische gegevens van toepassing.

##### <a name="web-server-diagnostics"></a>Diagnostische gegevens van het web server

U kunt inschakelen of uitschakelen van de volgende soorten logboeken:

-   **Gedetailleerde logboekregistratie fout** - gedetailleerde informatie voor HTTP-statuscodes die wijzen op een fout opgetreden (statuscode 400 of hoger). Dit kan informatie bevatten die kunt u bepalen waarom de server de foutcode geretourneerd.
-   **Aanvragen traceren is mislukt** - gedetailleerde informatie over de mislukte aanvragen, met inbegrip van een trace van de IIS-onderdelen gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verbeteren de prestaties van het web app of komen wat een bepaalde HTTP-fout wordt veroorzaakt.
-   **Web Server Logging** - informatie over HTTP-transacties met behulp van de W3C extended-indeling. Dit is handig bij het vaststellen van algemene web app metrics, zoals het aantal aanvragen dat is verwerkt of hoeveel aanvragen afkomstig zijn van een specifiek IP-adres.

##### <a name="application-diagnostics"></a>Diagnostische gegevens van toepassing

Diagnostische gegevens van toepassing, kunt u voor het vastleggen van informatie die door een webtoepassing. ASP.NET-toepassingen kunnen gebruiken de `System.Diagnostics.Trace` klasse informatie vastleggen in het toepassingslogboek van de diagnostische gegevens.

Zie [Diagnostische gegevens vastleggen voor web apps in Azure App-Service inschakelen](web-sites-enable-diagnostic-log.md)voor gedetailleerde instructies voor het configureren van uw toepassing voor het aanmelden.

#### <a name="use-remote-profiling"></a>Gebruik externe profilering

In Azure App-Service kunnen Web Apps, API Apps en WebJobs op afstand zijn profiel. Als het proces is trager dan verwacht, of de latentie van HTTP-aanvragen zijn hoger dan normaal en het CPU-gebruik van het proces ook hoog is, als u op afstand uw proces bekijken en de CPU bemonstering stacks voor het analyseren van de activiteit en hot codepaden ophalen.

Zie voor meer informatie over [ondersteuning voor externe profilering in Azure App-Service](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>De ondersteuning van Azure App Service Portal gebruiken

Web Apps biedt u de mogelijkheid om het oplossen van problemen met betrekking tot uw web app door te kijken naar de HTTP-Logboeken, Logboeken, proces dumpen en meer. U hebt toegang tot alle informatie op onze Support portal op **http://&lt;de naam van uw app >.scm.azurewebsites.net/Support**

De Service ondersteuning voor Azure App portal biedt drie afzonderlijke tabbladen voor de ondersteuning van de drie stappen van een gebruikelijk scenario voor het oplossen van problemen:

1.  Huidige gedrag observeren
2.  Door de ingebouwde analyzers diagnostische informatie verzamelen en analyseren
3.  Beperken

Als het probleem nu voordoet zich, klikt u op **analyseren** > **Diagnostische gegevens** > **Diagnose nu** een diagnostische sessie voor u te maken, waarbij HTTP wordt verzameld zich aanmeldt, Logboeken, geheugen dumpen, PHP foutenlogboeken en PHP rapport verwerkt.

Zodra de gegevens worden verzameld, zal ook een analyse uitvoeren op gegevens en u voorzien van een HTML-rapport.

In het geval u downloaden van de gegevens, standaard wilt, zou dit worden opgeslagen in de map D:\home\data\DaaS.

Zie voor meer informatie over de ondersteuning van Azure App Service portal, [Nieuwe Updates van de Site uitgebreide ondersteuning voor Azure Websites](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Gebruik de Foutopsporingsconsole Kudu

Web Apps wordt geleverd met een debug console die u gebruiken kunt voor het opsporen van fouten, verkennen, het uploaden van bestanden als JSON eindpunten voor het verkrijgen van informatie over de omgeving. Dit heet de _Kudu Console_ of het _Dashboard SCM_ voor uw web app.

U kunt toegang krijgen tot dit dashboard door op de koppeling **https://&lt;de naam van uw app >.scm.azurewebsites.net/**.

Enkele van de dingen die Kudu biedt, zijn:

-   de instellingen voor uw toepassing
-   logboek-stream
-   diagnostische dump
-   Debug console waarin u Powershell-cmdlets en elementaire DOS-opdrachten kunt uitvoeren.


Een andere nuttige functie van Kudu is dat, in het geval uw toepassing first-chance uitzonderingen genereren is, kunt u Kudu en de SysInternals-hulpprogramma Procdump voor het maken van geheugen dumpen. De geheugendumps zijn momentopnamen van het proces en kunnen vaak helpen bij het oplossen van complexere problemen met uw web app.

Zie voor meer informatie over de functies die beschikbaar zijn in de Kudu, [Azure Websites Team Services-hulpprogramma's die u moet weten over](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. het probleem oplossen

####    <a name="scale-the-web-app"></a>Schaal van de web app.

Voor betere prestaties en de doorvoer, in Azure App-Service, kunt u de schaal waarop u met uw toepassing aanpassen. Schalen van een web app omvat twee verwante acties: uw serviceplan App wijzigen in een hogere prijzen laag, en bepaalde instellingen te configureren nadat u hebt overgeschakeld naar de hogere prijzen laag.

Zie voor meer informatie op schaal [schaal een web app in Azure App-Service](web-sites-scale.md).

Bovendien kunt u uw toepassing uitvoert op meer dan één exemplaar. Dit niet alleen beschikt u over meer verwerkingscapaciteit, maar kunt u ook een bedrag van fouttolerantie. Als het proces één exemplaar gezet, blijft het andere exemplaar voldoen aan aanvragen.

U kunt instellen om de schaal handmatig of automatisch worden.

####    <a name="use-autoheal"></a>AutoHeal gebruiken

AutoHeal wordt het werkproces voor uw app op basis van de instellingen die u (zoals wijzigingen in de configuratie, aanvragen, beperkingen op basis van het geheugen of de tijd die nodig is kiest voor het uitvoeren van een aanvraag) herhaald. De meeste van de tijd, is Prullenbak, het proces de snelste manier om te herstellen van een probleem. Maar u kunt altijd opnieuw de web app uit rechtstreeks vanuit de Portal Azure, wordt AutoHeal deze automatisch voor u doen. U hoeft te doen is sommige triggers in web.config voor de hoofdmap voor uw webtoepassing toevoegen. Houd er rekening mee dat deze instellingen op dezelfde manier werken zou, zelfs als uw toepassing niet een .net.

Zie [Automatische retoucheren Azure-websites](/blog/auto-healing-windows-azure-web-sites/)voor meer informatie.

####    <a name="restart-the-web-app"></a>Opnieuw starten van de web app.

Dit is vaak de eenvoudigste manier om te herstellen van eenmalige problemen. Op de [Azure Portal](https://portal.azure.com/)hebt op van uw web app blade, u de opties voor het stoppen of opnieuw starten van de app.

 ![opnieuw opstarten web app prestatieproblemen oplossen](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

U kunt ook uw web app met Azure Powershell beheren. Zie [Azure PowerShell Azure Resource Manager gebruiken](../powershell-azure-resource-manager.md)voor meer informatie.
