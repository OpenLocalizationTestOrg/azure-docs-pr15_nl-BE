<properties
    pageTitle="Apps in Azure App-Service controleren"
    description="Leren Apps in Azure App-Service controleren met behulp van de Portal Azure."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Procedure: Apps in Azure App-Service controleren

[App-Service](http://go.microsoft.com/fwlink/?LinkId=529714) biedt een ingebouwde controle functionaliteit in [Azure Portal](https://portal.azure.com).
Dit omvat de mogelijkheid om **quota's** en **maatstaven** voor een app zoals de App serviceplan en het instellen van **waarschuwingen** en zelfs **schalen** automatisch op basis van deze gegevens te controleren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Informatie over quota en statistieken

### <a name="quotas"></a>Quota 's

Toepassingen die worden gehost in App-Service worden bepaalde *beperkingen* op de middelen die ze kunnen gebruiken. De grenzen worden gedefinieerd door het **serviceplan App** die is gekoppeld aan de app.

Als de toepassing wordt gehost in een **vrije** of **gedeeld** -indeling, worden de grenzen van de middelen die de app kunt gebruiken gedefinieerd door **quota's**.

Als de toepassing wordt gehost in een **Basic**, **Standard** of **Premium** plan en vervolgens de limieten van worden de gebruikte bronnen kunnen ingesteld **formaat** (Small, Medium, Large) en het **exemplaar tellen** (1, 2, 3,...) van het **App-serviceplan**.

**Quota** voor **vrije** of **gedeelde** toepassingen zijn:

* **CPU(Short)**
   * Bedrag van de CPU is toegestaan voor deze toepassing in een interval van 3 minuten. Dit contingent wordt opnieuw ingesteld om de 3 minuten.
* **CPU(Day)**
   * Totale bedrag van de CPU is toegestaan voor deze toepassing in een dag. Dit contingent wordt opnieuw ingesteld voor elke 24 uur om middernacht UTC.
* **Geheugen**
   * Totale hoeveelheid geheugen die is toegestaan voor deze toepassing.
* **Bandbreedte**
   * Totale hoeveelheid bandbreedte voor uitgaande toegestaan voor deze toepassing in een dag.
   Dit contingent wordt opnieuw ingesteld voor elke 24 uur om middernacht UTC.
* **FileSystem**
   * Totale toegestane hoeveelheid opslagruimte.

Het enige quotum van apps gehost op **Basic**, **Standard** en **Premium** plannen is **bestandssysteem**.

Meer informatie over de specifieke contingenten, grenzen en functies die beschikbaar zijn voor de verschillende App-Service-SKU's vindt u hier: [Azure Service abonnementen](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Het afdwingen van quota

Als een toepassing in het gebruik ervan overschrijdt de **CPU (korte)**, worden **CPU (dag)**of **bandbreedte** contingent vervolgens de toepassing gestopt totdat het quotum wordt opnieuw ingesteld. Gedurende deze periode wordt alle inkomende aanvragen zal leiden tot een **HTTP 403**.
![][http403]

Als de toepassing **geheugen** quota wordt overschreden, wordt de toepassing opnieuw wordt gestart worden.

Als het **bestandssysteem** quotum wordt overschreden, vervolgens schrijven een mislukt, inclusief naar Logboeken geschreven.

Quota kunnen worden verhoogd of door uw serviceplan App upgraden van uw app verwijderd.

### <a name="metrics"></a>Statistieken

**Statistieken** geven informatie over de app of de App serviceplan van gedrag.

Voor een **toepassing**zijn de beschikbaar:

* **Gemiddelde reactietijd**
   * De gemiddelde tijd voor de app aanvragen kunnen in ms.
* **Gemiddelde geheugen werkset**
   * De gemiddelde hoeveelheid geheugen in MIB's die door de toepassing worden gebruikt.
* **CPU-tijd**
   * Het bedrag van de CPU in seconden worden gebruikt door de app. Voor meer informatie over deze metrische Zie: [CPU-tijd vs CPU-percentage](#cpu-time-vs-cpu-percentage)
* **Gegevens In**
   * Het bedrag van de binnenkomende bandbreedte verbruikt door de app in MIB's.
* **Gegevens uit**
   * De hoeveelheid bandbreedte voor uitgaande verbruikt door de app in MIB's.
* **HTTP-2xx**
   * Het aantal aanvragen, wat resulteert in een HTTP-statuscode > 200, maar 300 < =.
* **HTTP-3xx**
   * Het aantal aanvragen, wat resulteert in een HTTP-statuscode > 300 maar 400 < =.
* **HTTP 401**
   * Aantal waardoor de statuscode 401 HTTP-aanvragen.
* **HTTP-fout 403**
   * Het aantal waardoor de statuscode HTTP 403.
* **HTTP-fout 404**
   * Het aantal waardoor de 404 HTTP statuscode.
* **HTTP 406**
   * Het aantal waardoor de statuscode HTTP 406.
* **HTTP-4xx**
   * Het aantal aanvragen, wat resulteert in een HTTP-statuscode > = 400 maar < 500.
* **De HTTP-Server-fouten**
   * Het aantal aanvragen, wat resulteert in een HTTP-statuscode > = 500, maar 600 <.
* **Geheugen-werkset**
   * Huidige hoeveelheid geheugen die wordt gebruikt door de app in MIB's.
* **Aanvragen**
   * Totaal aantal aanvragen ongeacht hun resulterende HTTP-statuscode.

De statistieken beschikbaar zijn voor een **serviceplan App**:

>[AZURE.NOTE] App Service plan parameters zijn alleen beschikbaar voor plannen in **Basic**, **Standard** en **Premium** SKU.

* **CPU-Percentage**
   * De gemiddelde CPU-gebruik in alle exemplaren van het plan.
* **Geheugenpercentage**
   * De gemiddelde hoeveelheid geheugen die wordt gebruikt in alle exemplaren van het plan.
* **Gegevens In**
   * De gemiddelde binnenkomende bandbreedte die wordt gebruikt in alle exemplaren van het plan.
* **Gegevens uit**
   * De gemiddelde uitgaande bandbreedte die wordt gebruikt in alle exemplaren van het plan.
* **Wachtrijlengte voor schijf**
   * Het gemiddelde aantal zowel lees- en schrijfopdrachten die in de wachtrij zijn geplaatst op opslag. Een hoge wachtrijlengte is een indicatie van een toepassing die als gevolg van buitensporige schijf I/O vertragen kan.
* **Lengte van wachtrij voor http**
   * Het gemiddelde aantal HTTP-aanvragen dat zitten in de wachtrij voor zijn vervuld. Een HTTP-wachtrij hoog of toenemende lengte is een symptoom van een plan bij zware belasting.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU-tijd vs CPU-percentage
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Er zijn 2 maatstaven die overeenkomen met het CPU-gebruik. **CPU-tijd** en het **percentage van de CPU**

**CPU-tijd** is handig voor toepassingen die worden gehost in **vrije** of **gedeelde** plannen, aangezien een van hun quota in CPU minuten gebruikt door de toepassing wordt gedefinieerd.

**CPU-percentage** is aan de andere kant handig voor toepassingen die worden gehost in **basic**, **standard** en **premium** plannen omdat ze die kunnen worden geschaald en deze metriek een goede indicatie van het totale verbruik in alle exemplaren is.

##<a name="metrics-granularity-and-retention-policy"></a>Metrics granulatie en bewaarbeleid

Maatstaven voor een toepassing en serviceplan app zijn vastgelegd en samengevoegd door de service met de volgende wisselkoersmodellen en bewaarbeleid:

 * **Minuut** granulatie metrics worden bewaard gedurende **48 uur**
 * **Uur** granulatie parameters blijven behouden voor **30 dagen**
 * **Dag** granulatie metrics worden bewaard gedurende **90 dagen**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Toezicht op quota en statistieken in de Portal Azure.

U kunt de status van de verschillende **contingenten** en **maatstaven** op het gebied van een toepassing in de [Portal Azure](https://portal.azure.com)bekijken.

![][quotas]
**Quota's** kunt u vinden onder Instellingen >**quota**. De UX kunt u bekijken: (1) de naam van de quota, (2) het interval voor opnieuw instellen (3) de huidige limiet en (4) de huidige waarde.

![][metrics]
**Metrics** kan toegang rechtstreeks van de bladeserver resource zijn. Kunt u de grafiek door: (1) **Klik** op en selecteer (2) **de grafiek bewerken**.
Hier kunt u de (3) **tijdsbereik**, (4) **het grafiektype**en (5) **Statistieken** weer te geven.  

Voor meer informatie over hier statistieken: [Statistieken voor Monitor-service](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Waarschuwingen en automatisch schalen
Maatstaven waarschuwt voor een toepassing of App Service plan maximaal kan worden aangesloten, voor meer informatie hierover, Zie [je de meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

App Service apps gehost in basic, standard of premium App serviceplannen voor ondersteuning voor het **automatisch schalen**. Dit kunt u regels configureren die cijfers over de App Service plan volgen en kunnen verhogen of verlagen van het exemplaar aantal aanvullende bronnen die zo nodig, of geld opslaan wanneer de toepassing te verstrekken. Voor meer informatie over het automatisch schalen hier: [hoe schaal](../monitoring-and-diagnostics/insights-how-to-scale.md) en [Aanbevolen procedures voor Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md) hier

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
