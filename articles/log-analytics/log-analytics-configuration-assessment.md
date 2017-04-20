<properties
    pageTitle="Configuratie beoordeling oplossing in logboek Analytics | Microsoft Azure"
    description="De beoordeling van de configuratie-oplossing in logboek Analytics biedt gedetailleerde informatie over de huidige status van uw serverinfrastructuur van System Center Operations Manager bij het gebruik van agents Operations Manager of een groep Operations Manager management."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="configuration-assessment-solution-in-log-analytics"></a>Configuratie beoordeling oplossing in logboek Analytics

De beoordeling van de configuratie-oplossing in logboek Analytics helpt u potentiële problemen met de server configuratie via waarschuwingen en aanbevelingen van kennis vinden.

Deze oplossing moet System Center Operations Manager. Beoordeling van de configuratie is niet beschikbaar als u alleen direct verbonden agenten.

Informatie weergeven in configuratie bepalen oplossing is de Silverlight-invoegtoepassing voor uw browser vereist.

>[AZURE.NOTE] Vanaf 5 juli 2016, de oplossing voor de beoordeling van de configuratie kan niet meer worden toegevoegd aan logboek Analytics werkruimten en deze oplossing niet langer beschikbaar voor bestaande gebruikers na 1 augustus 2016. Voor klanten met deze oplossing voor SQL Server- of Active Directory, is het raadzaam dat u in plaats daarvan gebruikt de [SQL Server-beoordeling](log-analytics-sql-assessment.md), [Beoordeling van Active Directory](log-analytics-ad-assessment.md) en [Active Directory Replication Status](log-analytics-ad-replication-status.md) oplossingen. Voor klanten met de beoordeling van de configuratie voor Windows, Hyper-V en System Center Virtual Machine Manager wordt aangeraden u gebruikt de gebeurtenis collectie en mogelijkheden voor het krijgen van een holistische visie van problemen binnen uw omgeving voor bijhouden van wijzigingen.

![Configuratie beoordeling naast elkaar](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Configuratiegegevens verzameld van gecontroleerde servers en vervolgens verzonden naar de OMS-service in de cloud voor verwerking. Logica wordt toegepast op de gegevens ontvangen en de service cloud gegevens. Verwerkte gegevens voor de servers weergegeven voor de volgende gebieden:

- **Waarschuwingen:** De proactieve, waarschuwingen die zijn gegenereerd voor uw gecontroleerde servers bevat. Deze worden geproduceerd door de regels die zijn gemaakt door Microsoft Customer Support organisatie (CSS) met beste praktijken uit het veld.
- **Aanbevelingen knowledge:** Ziet u de Microsoft Knowledge Base-artikelen die worden aanbevolen voor workloads die zijn gevonden in uw infrastructuur; Deze worden automatisch voorgesteld op basis van uw configuratie via het gebruik van machine learning.
- **-Servers en werkbelasting geanalyseerd:** Ziet u de servers en de werklast die door OMS zijn worden gecontroleerd.

![Configuratie beoordeling dashboard](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Technologieën die kunt u met configuratie beoordeling analyseren

OMS configuratie beoordeling analyseert de volgende belasting:

- Windows Server 2012 en Microsoft Hyper-V Server 2012
- Windows Server 2008 en Windows Server 2008 R2, met inbegrip van:
    - Active Directory
    - Hyper-V host
    - Algemene besturingssysteem
- SQL Server 2008 en hoger
    - SQL Server-Database-Engine
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 en Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 en Lync Server 2010
- System Center 2012 SP1 – Virtual Machine Manager

Voor SQL Server, worden de volgende 32-bits en 64-bits edities ondersteund voor analyse:

- SQL Server 2016 - alle edities
- SQL Server 2014 - alle edities
- SQL Server 2008 en 2008 R2 - alle edities

De SQL Server-Database-Engine wordt geanalyseerd op alle ondersteunde versies. Ook wordt de 32-bits editie van SQL Server ondersteund in de uitvoering WOW64.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie te installeren en configureren van de oplossing.

- Operations Manager is vereist voor de beoordeling van de configuratie-oplossing.
- U hebt een Operations Manager agent op elke computer waar u de configuratie ervan te beoordelen.
- De beoordeling van de configuratie-oplossing toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.

## <a name="configuration-assessment-data-collection-details"></a>Beoordeling van gegevens verzameling configuratiegegevens

Configuratie bepalen verzamelt gegevens en metagegevens staat gegevens met behulp van de agenten die u hebt ingeschakeld.

De volgende tabel bevat methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor de beoordeling van de configuratie.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows|![Nee](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Nee](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| twee keer per dag|

De volgende tabel ziet u voorbeelden van gegevenstypen die worden verzameld door de beoordeling van de configuratie:

|**Gegevenstype**|**Velden**|
|---|---|
|Configuratie|Klant-id, AgentID, id van de entiteit, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate|
|Metagegevens|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, netwerknaam, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, naam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Staat|StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Configuratie van beoordeling van meldingen
U kunt bekijken en waarschuwingen van de beoordeling van de configuratie met de pagina Waarschuwingen beheren. Meldingen geven aan het probleem dat is gevonden, de oorzaak en hoe het probleem op te lossen. Ze bevatten ook informatie over configuratie-instellingen in uw omgeving die problemen kunnen veroorzaken.

![waarschuwingen weergeven](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] De beoordeling van de configuratie van waarschuwingen zijn anders dan andere waarschuwingen in logboek Analytics. Waarschuwingen weergeven is een Silverlight Plug-in voor uw browser vereist.

Wanneer u een item of items op de pagina meldingen categorie selecteert, ziet u een lijst met servers of werkbelasting met waarschuwingen die voor elk item gelden.

![signaal](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Elke melding bevat een koppeling naar een artikel in de Microsoft Knowledge Base. Deze artikelen bevatten meer informatie over de waarschuwing.

>[AZURE.TIP] Het maximale aantal waarschuwingen worden weergegeven is 2.000. Klik op de melding balk boven de lijst met waarschuwingen waarschuwingen meer.

U kunt klikken op een willekeurig item in de lijst aan het KB-artikel dat mogelijk op te lossen de oorzaak van het probleem dat het signaal wordt gegenereerd.

![KB-artikel](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

U kunt waarschuwingsregels te negeren, specifieke regels of een regels-klassen beheren.

![Waarschuwingsregels beheren](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Knowledge-aanbevelingen
Als u knowledge aanbevelingen bekijken, bent u logboek zoekresultaten aanbiedingen aanbevolen voor de werkbelasting en de computers waarmee u meer informatie over de waarschuwing Microsoft KB-artikelen gepresenteerd.

![zoekresultaten voor knowledge aanbevelingen](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Servers en werkbelasting geanalyseerd
Als u knowledge aanbevelingen bekijken, bent u zoekresultaten voor logboek met alle servers en de werklast die bekend is dat het Kantoorbeheersysteem van Operations Manager gepresenteerd.

![Servers en werkbelasting](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Volgende stappen

- [Zoekopdrachten in Analytics logboek logboek](log-analytics-log-searches.md) gebruiken om gedetailleerde beoordeling-configuratiegegevens weer te geven.
