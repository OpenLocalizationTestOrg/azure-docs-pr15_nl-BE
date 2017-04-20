<properties
    pageTitle="502 Ongeldige gateway oplossen, 503 service niet beschikbaar-fouten | Microsoft Azure"
    description="Problemen met ongeldige gateway 502 en 503 service niet beschikbaar fouten in uw web app in Azure App Service gehost."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 Ongeldige gateway 503 service niet beschikbaar is 503-fout, fout 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>HTTP-fouten '502 Ongeldige gateway' en '503 service niet beschikbaar' in uw Azure web apps

'502 Ongeldige gateway' en '503 service niet beschikbaar' zijn veelvoorkomende fouten in uw web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)gehost. Dit artikel helpt u bij het oplossen van deze fouten.

Als u meer Help-informatie op elk punt in dit artikel, kunt u contact opnemen met de Azure experts op [de MSDN Azure en de forums stackoverloop](https://azure.microsoft.com/support/forums/). Ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [website voor ondersteuning van Azure](https://azure.microsoft.com/support/options/) en klik op **Ondersteuning krijgen**.

## <a name="symptom"></a>Symptoom

Wanneer u naar de web app bladert, wordt een HTTP fout "502 Ongeldige Gateway" of een HTTP-fout '503 Service niet beschikbaar'.

## <a name="cause"></a>Oorzaak

Dit probleem wordt vaak veroorzaakt door problemen met niveau van toepassingen, zoals:

-   aanvragen duurt erg lang
-   toepassing met behulp van hoge geheugen/CPU
-   toepassing vastlopen vanwege een uitzondering.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Stappen voor probleemoplossing '502 Ongeldige gateway' en '503 service niet beschikbaar'-fouten oplossen

Het oplossen van problemen kan worden onderverdeeld in drie afzonderlijke taken in de juiste volgorde:

1.  [Observeren, en controleert de werking van de toepassing](#observe)
2.  [Gegevens verzamelen](#collect)
3.  [Het probleem oplossen](#mitigate)

[App Service Web Apps](/services/app-service/web/) hebt u verschillende opties bij elke stap.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. Houd en gedrag van de toepassing controleren

####    <a name="track-service-health"></a>Bijhouden van gezondheid Service

Microsoft Azure publicizes telkens wanneer er een service wordt onderbroken of de prestaties afbraak is. U kunt de gezondheid van de service op de [Azure Portal](https://portal.azure.com/)bijhouden. Zie voor meer informatie, [Track service gezondheid](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Controleren van uw web app

Met deze optie kunt u meer als uw toepassing problemen ondervindt. Klik op de tegel **aanvragen en fouten** in uw webtoepassing-blade. De **Metric** blade ziet u de parameters die u kunt toevoegen.

Sommige van de criteria die u wilt controleren voor uw web app

-   Gemiddelde geheugen werkset
-   Gemiddelde reactietijd
-   CPU-tijd
-   Geheugen-werkset
-   Aanvragen

![monitor web app oplossen HTTP-fouten met ongeldige gateway 502 en 503 service niet beschikbaar](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Zie voor meer informatie:

-   [Monitor Web Apps in Azure App-Service](web-sites-monitor.md)
-   [Meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. gegevens verzamelen

####    <a name="use-the-azure-app-service-support-portal"></a>De ondersteuning van Azure App Service Portal gebruiken

Web Apps biedt u de mogelijkheid om het oplossen van problemen met betrekking tot uw web app door te kijken naar de HTTP-Logboeken, Logboeken, proces dumpen en meer. U hebt toegang tot alle informatie op onze Support portal op **http://&lt;de naam van uw app >.scm.azurewebsites.net/Support**

De Service ondersteuning voor Azure App portal biedt drie afzonderlijke tabbladen voor de ondersteuning van de drie stappen van een gebruikelijk scenario voor het oplossen van problemen:

1.  Huidige gedrag observeren
2.  Door de ingebouwde analyzers diagnostische informatie verzamelen en analyseren
3.  Beperken

Als het probleem nu voordoet zich, klikt u op **analyseren** > **Diagnostische gegevens** > **Diagnose nu** een diagnostische sessie voor u te maken, waarbij HTTP wordt verzameld zich aanmeldt, Logboeken, geheugen dumpen, PHP foutenlogboeken en PHP rapport verwerkt.

Zodra de gegevens worden verzameld, zal ook een analyse uitvoeren op gegevens en u voorzien van een HTML-rapport.

In het geval u downloaden van de gegevens, standaard wilt, zou dit worden opgeslagen in de map D:\home\data\DaaS.

Zie voor meer informatie over de ondersteuning van Azure App Service portal, [Nieuwe Updates van de Site uitgebreide ondersteuning voor Azure Websites](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Gebruik de Foutopsporingsconsole Kudu

Web Apps wordt geleverd met een debug console die u gebruiken kunt voor het opsporen van fouten, verkennen, het uploaden van bestanden als JSON eindpunten voor het verkrijgen van informatie over de omgeving. Dit heet de _Kudu Console_ of het _Dashboard SCM_ voor uw web app.

U kunt toegang krijgen tot dit dashboard door op de koppeling **https://&lt;de naam van uw app >.scm.azurewebsites.net/**.

Enkele van de dingen die Kudu biedt, zijn:

-   de instellingen voor uw toepassing
-   logboek-stream
-   diagnostische dump
-   Debug console waarin u Powershell-cmdlets en elementaire DOS-opdrachten kunt uitvoeren.


Een andere nuttige functie van Kudu is dat, in het geval uw toepassing first-chance uitzonderingen genereren is, kunt u Kudu en de SysInternals-hulpprogramma Procdump voor het maken van geheugen dumpen. De geheugendumps zijn momentopnamen van het proces en kunnen vaak helpen bij het oplossen van complexere problemen met uw web app.

Zie voor meer informatie over de functies die beschikbaar zijn in de Kudu, [Azure Websites on line tools die u moet weten over](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

 ![Start de app om op te lossen, HTTP-fouten met ongeldige gateway 502 en 503 service niet beschikbaar](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

U kunt ook uw web app met Azure Powershell beheren. Zie [Azure PowerShell Azure Resource Manager gebruiken](../powershell-azure-resource-manager.md)voor meer informatie.
