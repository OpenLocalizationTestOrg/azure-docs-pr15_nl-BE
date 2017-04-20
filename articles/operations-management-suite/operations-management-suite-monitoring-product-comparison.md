<properties 
   pageTitle="Microsoft-vergelijking monitoring | Microsoft Azure"
   description="Microsoft Operations Management Suite (OMS) is van Microsoft cloud gebaseerde IT management-oplossing waarmee u beheren kunt en beveiligen van uw locatie op en cloud infrastructuur.  In dit artikel worden opgenomen in de OMS verschillende services en koppelingen naar gedetailleerde inhoud."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Microsoft monitoring-vergelijking

Dit artikel bevat een vergelijking tussen System Center Operations Manager (SCOM) en logboek Analytics in Operations Management Suite (OMS) in hun architectuur, de logica van hoe ze resources controleren en hun prestaties analyseren van de gegevens die zij verzamelen.  Dit is zodat u een fundamenteel inzicht in hun verschillen en de relatieve sterke.  

## <a name="basic-architecture"></a>Basisarchitectuur
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Alle SCOM-onderdelen zijn geïnstalleerd in uw datacenter.  [Agents zijn geïnstalleerd](http://technet.microsoft.com/library/hh551142.aspx) op Windows en Linux machines die worden beheerd door SCOM.  Agenten verbinden met [Management-Servers](https://technet.microsoft.com/library/hh301922.aspx) communiceren met de database- en magazijn voor SCOM.  Agenten vertrouwen op domeinverificatie verbinding maken met servers voor het beheer.  Die buiten een vertrouwde domein kunnen certificaatverificatie wordt uitgevoerd of verbinding maken met een [Gateway-Server](https://technet.microsoft.com/library/hh212823.aspx).

SCOM moet twee SQL-databases, één voor operationele gegevens en een andere datawarehouse ter ondersteuning van de rapportage en gegevensanalyse.  Een [Reporting Server](https://technet.microsoft.com/library/hh298611.aspx) SQL Reporting Services op gegevens uit het datawarehouse rapport uitgevoerd. 

SCOM kunt controleren met behulp van de beheerpakketten voor producten zoals [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708)en [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html)cloud-bronnen.  Deze management packs gebruiken een of meer lokale agenten als proxy's detecteren cloud resources en uitgevoerde werkstromen voor het meten van de prestaties en beschikbaarheid.  Proxy-agents wordt ook gebruikt om [netwerkapparaten monitor](https://technet.microsoft.com/library/hh212935.aspx) en andere externe bronnen.

De Console van de bewerkingen is een Windows-toepassing die verbinding maakt met een van de servers voor het beheer en kan de beheerder weergeven en analyseren van verzamelde gegevens en de SCOM-omgeving configureren.  Een console op het web kan worden gehost op een IIS-server en biedt een analyse van gegevens via een browser.

![Architectuur van SCOM](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Logboek Analytics

De meeste OMS-componenten zijn in de Azure cloud zodat u kunt implementeren en met minimale kosten en administratieve inspanningen beheren.  Alle gegevens die zijn verzameld door Analytics logboek wordt opgeslagen in de opslagplaats OMS.

Logboek Analytics kunt u gegevens verzamelen van een van drie bronnen:

- Fysieke en virtuele machines met Windows en de [Microsoft Monitoring Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) of Linux en het [Operations Management Suite Agent voor Linux](https://technet.microsoft.com/library/mt622052.aspx).  Deze machines kunnen worden op de bedrijfsruimten of virtuele machines in Azure of een andere wolk.
- Een account Azure opslag met [Azure diagnostische](../cloud-services/cloud-services-dotnet-diagnostics.md) gegevens verzameld door Azure werknemer rol, rol van de website of virtuele machine.
- [Verbinding met een groep SCOM management](https://technet.microsoft.com/library/mt484104.aspx).  In deze configuratie worden de agents communiceren met SCOM beheerservers die ervoor zorgen dat de gegevens in de database SCOM waar vervolgens afgeleverd aan het gegevensarchief OMS.
Beheerders verzamelde gegevens analyseren en configureren van logboek Analytics met OMS-portal die wordt gehost in Azure en vanuit elke browser kan worden geopend.  Mobiele apps voor toegang tot deze gegevens zijn beschikbaar voor de standaard-platforms.

![Logboek Analytics architectuur](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integratie van SCOM en logboek Analytics

Wanneer SCOM wordt gebruikt als gegevensbron voor Analytics logboek kunt u gebruikmaken van de functies van beide producten in een hybride omgeving controleren.  U kunt bestaande SCOM agenten via de Console bewerkingen worden beheerd met OMS, naast de verdere beheerpakketten van SCOM uitvoeren.  
Gegevens uit een gekoppelde SCOM management groep is bezorgd bij logboek Analytics met een van de vier methoden:

- Gebeurtenissen en prestatiegegevens worden verzameld door de agent en afgeleverd bij de SCOM.  Beheerservers in SCOM leveren de gegevens vervolgens naar logboek Analytics.
- Sommige gebeurtenissen zoals de IIS-logboeken en beveiliging blijven rechtstreeks afgeleverd bij Analytics logboekbestanden van de agent.
- Sommige oplossingen wordt bieden extra software aan de agent of vereist dat software worden geïnstalleerd om extra gegevens te verzamelen.  Deze gegevens worden, meestal rechtstreeks naar logboek Analytics verzonden.
- Sommige oplossingen van worden gegevens rechtstreeks vanuit SCOM beheer van servers die niet afkomstig is van de agent verzameld.  De [oplossing voor waarschuwing](https://technet.microsoft.com/library/mt484092.aspx) bijvoorbeeld verzamelt signalen van SCOM nadat ze zijn gemaakt.

## <a name="monitoring-logic"></a>Controle van de logica
SCOM en logboek Analytics met soortgelijke gegevens verzameld van agents werken maar fundamentele verschillen in hoe ze definiëren en implementeren van de logica voor het verzamelen van gegevens en hoe ze de gegevens die zij verzamelen analyseren.

### <a name="operations-manager"></a>Operations Manager
Controle-logica voor SCOM is geïmplementeerd in [beheerpakketten](https://technet.microsoft.com/library/hh457558.aspx) bevatten logica om onderdelen te bewaken, vast te stellen voor het meten van de gezondheid van deze componenten en voor het verzamelen van gegevens te analyseren.  Controle van gegevens, kan net zo eenvoudig als een gebeurtenis- of item verzamelen of complexe logica geïmplementeerd in een script gebruiken.  Beheerpakketten met volledige controle zijn beschikbaar voor allerlei [Microsoft en toepassingen van derden](http://go.microsoft.com/fwlink/?LinkId=82105) hardware en apparaten.  U kunt [uw eigen beheerpakketten ontwerpen](http://aka.ms/mpauthor) voor aangepaste toepassingen.

Beheerpakketten bevatten meerdere werkstromen dat elk sommige afzonderlijke controle werkt zoals een prestatiemeteritem bemonstering, controleren van de status van een service of een script wordt uitgevoerd.  Elke workflow wordt onafhankelijk van elkaar uitgevoerd en definieert eigen resultaten bijvoorbeeld welke database worden geschreven naar en of er een waarschuwing wordt gegenereerd. 

U kunt details van de werkstroom, zoals de frequentie van die deze worden uitgevoerd, de drempelwaarde waarbij zij rekening houden met een fout en de ernst van de genereren de waarschuwing negeren.  U kunt ook extra functionaliteit geven door uw eigen werkstromen toe te voegen.

![Overschrijvingen](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Management packs zijn geïnstalleerd in de database Operations Manager en automatisch gedistribueerd naar agenten door van beheerservers.  Elke agent automatisch gedownload van management packs en laden van werkstromen die relevant zijn voor de toepassingen die ze hebben geïnstalleerd.  Gegevens die zijn verzameld door de agent wordt teruggestuurd naar de server management voor plaatsing in het magazijn van database- en SCOM geleverd.  De Console bewerkingen kunt u bekijken en analyseren van deze gegevens door middel van aangepaste weergaven, dashboards en rapporten die zijn opgenomen in het management pack.

De verdeling van de beheerpakketten wordt geïllustreerd in het volgende diagram.

![Management pack stroom](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Logboek Analytics
#### <a name="event-and-performance-collection"></a>Gebeurtenis- en prestatie-collectie
Logboek Analytics verzamelt gebeurtenissen en prestatiemeteritems van bronnen, zoals het gebeurtenislogboek van Windows, IIS-logboeken en Syslog agent systemen.  U kunt criteria waarvoor gegevens worden opgevraagd via de portal logboek Analytics en maak logboek query's voor het analyseren van de verzamelde gegevens definiëren.  Een set criteria die standaard wordt gedefinieerd wanneer u uw werkruimte OMS maken en u aanvullende gegevens voor bepaalde toepassingen kunt. 

![Logboeken voor gebeurtenissen in logboek Analytics definiëren](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

SCOM heeft veel gedetailleerde workflows die meestal specifieke criteria definiëren voor gegevens en de actie die moet worden uitgevoerd in reactie, heeft logboek Analytics meer algemene criteria voor het verzamelen van gegevens.  Logboek voor query's en -oplossingen bieden meer gerichte criteria voor het analyseren en in specifieke gegevens in de cloud nadat deze zijn verzameld.

#### <a name="solutions"></a>Oplossingen
Oplossingen bieden extra logica voor gegevensverzameling en -analyse.  Kunt u oplossingen aan uw abonnement OMS toevoegen uit de galerie van oplossingen.

![Galerie met oplossingen](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

In de analyse van de gebeurtenissen en prestatiemeteritems die zijn verzameld in de bibliotheek OMS bieden cloud voornamelijk oplossingen worden uitgevoerd.  Ze bepalen ook aanvullende gegevens te verzamelen die kunnen worden geanalyseerd met query's logboek of door extra gebruikersinterface van de oplossing in het dashboard OMS. 

Bijvoorbeeld de [Wijzigingen bijhouden-oplossing](https://technet.microsoft.com/library/mt484099.aspx) detecteert wijzigingen in de configuratie voor agent-systemen en schrijft gebeurtenissen aan de opslagplaats voor OMS die kunnen worden geanalyseerd met verschillende grafische weergaven om samen te vatten wijzigingen gedetecteerd.  U kunt inzoomen in de weergave overzicht in logboek voor query's die de gedetailleerde gegevens die zijn verzameld door de oplossing worden weergegeven.


Terwijl u welke oplossingen u aan uw abonnement selecteren kunt toevoegen, hebt u momenteel niet de mogelijkheid om uw eigen oplossingen te maken.  U kunt de gebeurtenissen en prestatiemeteritems voor het verzamelen en het maken van aangepaste weergaven op basis van uw eigen logboek query's selecteren.

De monitoring logica voor logboek Analytics wordt samengevat in het volgende diagram.

![Logboek Analytics oplossing stroom](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Statuscontrole
### <a name="operations-manager"></a>Operations Manager
SCOM kan model van de verschillende onderdelen van een toepassing en een real-time health voor elk verstrekken.  Hierdoor kunt u niet alleen de fouten weergeven die zijn gedetecteerd en prestaties na verloop van tijd, maar ook voor het valideren van de werkelijke status van een toepassing of systeem en elk van de onderdelen op een bepaald moment.  Omdat deze de perioden die een toepassing beschikbaar is begrijpt, ondersteunt de gezondheid-engine in SCOM ook Service Level Agreements (SLA) die het analyseren en rapporteren van de beschikbaarheid van een toepassing in de tijd.

De weergave hieronder ziet u bijvoorbeeld de real-time status van SQL database-engines gecontroleerd door SCOM.  De gezondheid van de databases voor één van de database-engines weergegeven onder de helft van de weergave.

![Status weergeven](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

De Explorer gezondheid voor een van de database-engines wordt hieronder weergegeven met de monitoren die worden gebruikt voor het bepalen van de algehele gezondheid.  Deze beeldschermen worden gedefinieerd in het SQL management pack en uitgevoerd voor alle SQL-database-engines ontdekt door SCOM.

![Gezondheid explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Componenten op meerdere systemen kunnen worden gecombineerd voor het meten van de gezondheid van een gedistribueerde toepassing.  Dit is vooral handig voor zakelijke toepassingen die meerdere gedistribueerde onderdelen bevatten.  Kunt u een model dat de maatregelen die de status van elk onderdeel dat updatepakket in de beschikbaarheid van de toepassing.

Active Directory is een voorbeeld van een management pack waarmee een model voor het analyseren van de gedistribueerde onderdelen.  Hieronder het voorbeelddiagram ziet u de status van het algemene klimaat en de relatie tussen forests, domeinen en domeincontrollers.  Elk van deze onderdelen bevat subonderdelen en meerdere beeldschermen, vergelijkbaar met het bovenstaande voorbeeld van SQL.

![De weergave Netwerkdiagram SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Logboek Analytics
OMS niet opnemen van een gemeenschappelijk model toepassingen-engine of hun real-time gezondheid meten.  Individuele oplossingen kunnen de algehele gezondheid van bepaalde diensten op basis van de verzamelde gegevens beoordelen en aangepaste logica kan worden geïnstalleerd op de agent voor real-time analyse.  Omdat oplossingen worden uitgevoerd in de cloud met toegang tot de opslagplaats OMS, bieden zij vaak grondigere analyse dan worden gewoonlijk uitgevoerd door management packs. 

Bijvoorbeeld [AD beoordeling en evaluatie van SQL-oplossingen](https://technet.microsoft.com/library/mt484102.aspx) van de verzamelde gegevens te analyseren en een geven aan de verschillende aspecten van het milieu.  Het bevat aanbevelingen voor verbeteringen die kunnen worden gemaakt voor het verbeteren van de beschikbaarheid en prestaties van de omgeving.

![AD beoordeling oplossing](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Analyse van gegevens
SCOM en logboek Analytics biedt verschillende functies voor het analyseren van de verzamelde gegevens.  SCOM heeft weergaven en Dashboards in de Console bewerkingen voor het analyseren van recente gegevens in verschillende indelingen en rapporten om gegevens uit het gegevensmagazijn in tabelvorm presenteren.  Logboek Analytics biedt een volledig logboek querytaal en interface voor het analyseren van gegevens in de bibliotheek OMS.  Als SCOM voor logboek Analytics wordt gebruikt als gegevensbron, bevat de bibliotheek gegevens verzameld door SCOM, zodat het logboek analytische hulpmiddelen kunnen worden gebruikt voor het analyseren van gegevens uit beide systemen.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Weergaven
Weergaven in de Console bewerkingen kunt u verschillende gegevenstypen verzameld door SCOM bekijken in verschillende indelingen, meestal in tabelvorm voor gebeurtenissen, waarschuwingen en gegevens, en lijngrafieken voor prestatiegegevens.  Weergaven minimale analyse of consolidatie van de gegevens uitvoeren, maar kunnen u filteren op basis van bepaalde criteria. 

![Weergaven](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Beheerpakketten krijgt gewoonlijk meerdere weergaven ondersteunt de toepassing of het systeem dat wordt gecontroleerd.  Deze kan weergaven staat voor de verschillende objecten die het management pack ontdekt, waarschuwingen voor gevonden problemen, en van prestatieweergaven voor items bevatten.

Weergaven zijn vooral geschikt voor het analyseren van de huidige toestand van het milieu met inbegrip van de waarschuwingen openen en de status van bewaakte systemen en objecten.  U kunt inzoomen op gedetailleerde gebeurtenis- of prestatiegegevens van een waarschuwing voor een bepaalde ondersteuning om een diagnose van de oorzaak. U kunt ook de prestaties en de gezondheid van de verschillende onderdelen van een toepassing voor de beoordeling van de huidige status weergeven.

#### <a name="dashboards"></a>Dashboards
Dashboards in de Console bewerkingen werken voornamelijk met dezelfde gegevens als weergaven maar meer aangepast worden en rijkere visualisaties kunnen bevatten.  Een set van standaard dashboards vindt u eenvoudig kunt aanpassen voor uw eigen doeleinden.  U kunt ook een PowerShell-object waarin gegevens uit een query PowerShell geretourneerd.

![Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Ontwikkelaars hebben de mogelijkheid om aangepaste onderdelen toevoegen aan dashboards die ze in hun beheerpakketten bevatten.  Deze kunnen sterk worden gespecialiseerde voor een bepaalde toepassing zoals het dashboard in het SQL management pack hieronder weergegeven.  Dit dashboard kan ook worden gebruikt als sjabloon voor aangepaste versies.

![SQL-Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Rapporten
Rapporten in SCOM analyseren de gegevens uit het datawarehouse in tabelvorm.  Ze kunnen worden afgedrukt en gepland voor geautomatiseerde levering in verschillende bestandsindelingen, zoals PDF, CSV- en Word.  Rapporten werken met gegevens uit het datawarehouse zodat zij bijzonder geschikt voor analyse van de lange termijn trends zijn.

Beheerpakketten krijgt meestal aangepaste rapporten voor een bepaalde toepassing.  U kunt ook selecteren uit een bibliotheek van algemene rapporten die u aanpassen kunt voor uw eigen toepassingen of voor het uitvoeren van ad hoc analyse.

Hier volgt een voorbeeldrapport prestaties met gegevens die zijn verzameld door de Active Directory Management Pack.

![Rapport](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Logboek Analytics
Logboek Analytics is een [querytaal](https://technet.microsoft.com/library/mt484120.aspx) die u kunt analyses uitvoeren op gegevens uit meerdere toepassingen zonder te hoeven maken van een aangepaste weergave of het rapport.  Omdat OMS is geïmplementeerd in de cloud, prestaties van query's en analyse van gegevens niet met eventuele hardwarebeperkingen en snel query's met inbegrip van miljoenen records kunt analyseren. 

Query's in een logboek Analytics zijn ook de basis van andere functies.  U kunt een query opslaan, de resultaten exporteren naar Excel of deze automatisch met regelmatige tussenpozen uitgevoerd en genereren een waarschuwing als de resultaten voldoen aan bepaalde criteria.  

![Logboek query stroom](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Hieronder wordt een voorbeeld van een query Log Analytics.  In dit voorbeeld alle gebeurtenissen met 'gestart' in de naam zijn geretourneerd en gegroepeerd per gebeurtenis-id.  De gebruiker hoeft de query bevat en logboek Analytics genereert dynamisch de gebruikersinterface voor het uitvoeren van de analyse.  Een item selecteren in de lijst worden de met gedetailleerde gebeurtenisgegevens weer.

![Logboek-query](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Naast ad hoc analyse, kunnen query's in een logboek Analytics worden opgeslagen voor toekomstig gebruik en ook toegevoegd aan het [dashboard OMS](http://technet.microsoft.com/library/mt484090.aspx) zoals in het volgende voorbeeld.

![OMS-dashboard](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Volgende stappen

- [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx)implementeren.
- Aanmelden voor [Logboek Analytics](https://azure.microsoft.com/documentation/services/log-analytics).  
