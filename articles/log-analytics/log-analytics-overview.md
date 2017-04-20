<properties
   pageTitle="Wat is Log Analytics? | Microsoft Azure"
   description="Logboek Analytics is een service in Operations Management Suite (OMS) die helpt u bij het verzamelen en analyseren van operationele gegevens die zijn gegenereerd door de resources in de cloud en omgeving gebouwen.  Dit artikel bevat een kort overzicht van de verschillende onderdelen van het logboek Analytics en koppelingen naar gedetailleerde inhoud."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Wat is Log Analytics?
Logboek Analytics is een service in [Operations Management Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) die helpt u bij het verzamelen en analyseren van gegevens die zijn gegenereerd door de resources in de cloud en omgevingen ruimten. Het biedt u real-time inzicht met geïntegreerde search en aangepaste dashboards miljoenen records gemakkelijk te analyseren op al uw werkbelasting en -servers, onafhankelijk van hun fysieke locatie.


## <a name="log-analytics-components"></a>Analytics-onderdelen registreren
In het midden van logboek Analytics is de OMS opslagplaats dat wordt gehost in de wolk Azure.  Gegevens worden verzameld in de opslagplaats van gekoppelde bronnen door gegevensbronnen configureren en oplossingen toe te voegen aan uw abonnement.  Gegevensbronnen en oplossingen voor maakt elk verschillende recordtypen die hun eigen set eigenschappen, maar nog steeds worden geanalyseerd samen in query's naar de opslagplaats.  Hiermee kunt u de hulpprogramma's en de methoden gebruiken om te werken met verschillende soorten gegevens die worden verzameld door verschillende bronnen.


![OMS-opslagplaats](media/log-analytics-overview/overview.png)


Verbonden bronnen zijn de computers en andere bronnen die het genereren van gegevens die zijn verzameld door logboek Analytics.  Agenten die zijn geïnstalleerd op [Windows](log-analytics-windows-agents.md) en [Linux](log-analytics-linux-agents.md) -computers die rechtstreeks met elkaar te verbinden of agenten kunnen dit zijn in een [groep van System Center Operations Manager management verbonden](log-analytics-om-agents.md).  Logboek Analytics kan ook gegevens verzamelen uit [de opslag voor Azure](log-analytics-azure-storage.md).

Er zijn verschillende soorten verzamelde gegevens uit elke bron verbonden [gegevensbronnen](log-analytics-data-sources.md) .  Deze bevat gebeurtenissen en [prestatiegegevens](log-analytics-data-sources-performance-counters.md) van [Windows](log-analytics-data-sources-windows-events.md) en Linux agenten naast bronnen, zoals [IIS-logboeken](log-analytics-data-sources-iis-logs.md)en [Logboeken voor aangepaste tekst](log-analytics-data-sources-custom-logs.md).  U configureren elke gegevensbron die u wilt verzamelen, en de configuratie automatisch wordt geleverd aan elke gekoppelde bron.


## <a name="analyzing-log-analytics-data"></a>Logboek voor Analytics-gegevens analyseren
De meeste van uw interactie met Analytics logboek worden via de portal OMS dat wordt uitgevoerd in een browser en toegang tot de configuratie-instellingen en meerdere hulpprogramma's te analyseren en te reageren op verzamelde gegevens.  Vanaf de portal kunt u gebruikmaken van [logboek zoeken](log-analytics-log-searches.md) waarin het maken van query's voor het analyseren van de verzamelde gegevens, [dashboards](log-analytics-dashboards.md) die u kunt aanpassen met grafische weergaven van uw meest waardevolle zoekopdrachten en [oplossingen](log-analytics-add-solutions.md) bieden extra functies en hulpmiddelen voor analyse.

![OMS-portal](media/log-analytics-overview/portal.png)


Logboek Analytics biedt een querysyntaxis om snel te halen en samenvoegen van gegevens in de bibliotheek.  U kunt maken en opslaan van [Logboek zoekopdrachten](log-analytics-log-searches.md) direct om gegevens te analyseren in de portal OMS of logboek zoekopdrachten uitgevoerd automatisch waarschuwingen maken als de resultaten van de query een belangrijke voorwaarde blijkt.

![Logboek zoeken](media/log-analytics-overview/log-search.png)

Om een snelle grafische weergave van de gezondheid van uw gehele omgeving, kunt u de visualisaties voor zoekopdrachten opgeslagen logboek toevoegen aan uw [dashboard](log-analytics-dashboards.md).   

![Dashboard](media/log-analytics-overview/dashboard.png)

Te analyseren gegevens buiten logboek Analytics, kunt u de gegevens uit de opslagplaats OMS in hulpmiddelen voor [Power BI](log-analytics-powerbi.md) of Excel exporteren.  U kunt ook gebruikmaken van de [Zoek-API logboek](log-analytics-log-search-api.md) aangepaste oplossingen bouwen die gebruikmaken van logboek Analytics-gegevens te integreren met andere systemen.

## <a name="solutions"></a>Oplossingen
Oplossingen toevoegen functionaliteit aan logboek Analytics.  Ze voornamelijk uitgevoerd in de cloud en analyse van gegevens die zijn verzameld in de opslagplaats OMS. Ze bepalen ook nieuwe recordtypen te verzamelen die kunnen worden geanalyseerd met logboek zoeken of door extra gebruikersinterface van de oplossing in het dashboard OMS.  

![Oplossing voor bijhouden wijzigen](media/log-analytics-overview/change-tracking.png)


Oplossingen voor een groot aantal verschillende functies beschikbaar zijn en kunt u gemakkelijk bladeren beschikbare oplossingen en [Voeg deze toe aan uw werkruimte OMS](log-analytics-add-solutions.md) uit de galerie met oplossingen.  Veel worden automatisch geïmplementeerd en start direct terwijl anderen kunnen sommige configuratie vereist.

![Galerie van oplossingen](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Logboek Analytics architectuur
De eisen van het logboek Analytics zijn minimaal, omdat de centrale onderdelen worden gehost in de wolk Azure.  Dit omvat de opslagplaats naast de services die u voor het analyseren van de verzamelde gegevens.  De portal toegankelijk vanuit elke browser dus er geen vereiste voor clientsoftware is.

U moet agents installeren op computers met [Windows](log-analytics-windows-agents.md) en [Linux](log-analytics-linux-agents.md) , maar er is geen extra agent is vereist voor computers die al lid van een [SCOM management groep verbonden zijn](log-analytics-om-agents.md).  Agenten SCOM blijven communiceren met servers voor het beheer die hun gegevens naar het logboek voor Analytics doorstuurt.  Sommige oplossingen maar vereist agents communiceren rechtstreeks met logboek Analytics.  De documentatie voor elke oplossing geeft de communicatie-eisen.

Wanneer u [zich aanmelden voor logboek Analytics](log-analytics-get-started.md), maakt u een werkruimte OMS.  U kunt de werkruimte beschouwen als een unieke OMS-omgeving met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen. U kunt meerdere werkruimten maken in uw abonnement op de ondersteuning voor meerdere omgevingen zoals productie en testen.

![Logboek Analytics architectuur](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Volgende stappen

- [Aanmelden voor een gratis logboek Analytics-account](log-analytics-get-started.md) in uw eigen omgeving testen.
- Bekijk de verschillende [Gegevensbronnen](log-analytics-data-sources.md) om gegevens te verzamelen in de opslagplaats OMS beschikbaar.
- De [Bladeren van de oplossingen die beschikbaar zijn in de galerie met oplossingen](log-analytics-add-solutions.md) functionaliteit toevoegen aan een logboek Analytics.
