<properties
    pageTitle="Overzicht van controleprogramma in Microsoft Azure | Microsoft Azure"
    description="Bovenste niveau overzicht van bewaking en diagnostiek in Microsoft Azure, met inbegrip van de waarschuwingen, webhooks, automatisch schalen en meer."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Overzicht van controleprogramma in Microsoft Azure

Dit artikel bevat een overzicht van het toezicht op Azure bronnen. Verwijzingen naar informatie biedt over specifieke soorten bronnen.  Zie voor algemene informatie over het controleren van uw toepassing vanuit oogpunt niet Azure [richtsnoeren voor bewaking en diagnose](../best-practices-monitoring.md).

Cloud-toepassingen zijn complex met veel bewegende onderdelen. Controle levert gegevens om ervoor te zorgen dat uw toepassing van blijft en uitgevoerd in orde zijn. Ook kunt u mogelijke problemen voorkomen of oplossen van problemen met het verleden zijn. Bovendien kunt u controlegegevens diep inzicht krijgen over de toepassing. Die kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of automatiseren van acties die zouden anders handmatige tussenkomst vereist is.

Het volgende diagram ziet een overzicht van Azure monitoring, met inbegrip van het type van de logboeken die kunt u verzamelen en wat u kunt doen met deze gegevens.   

![Logisch Model voor de bewaking en diagnostiek voor resources niet berekenen](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Figuur 1: Conceptueel Model voor de bewaking en diagnostiek voor resources niet berekenen

<br/>

![Logisch Model voor de bewaking en diagnostiek voor compute resources](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Afbeelding 2: Conceptueel Model voor de bewaking en diagnostiek voor compute resources


## <a name="monitoring-sources"></a>Bronnen controleren
### <a name="activity-logs"></a>Logboeken voor faxactiviteit
Voor meer informatie kunt over de resource, zoals gezien door de Azure infrastructuur u het logboek voor faxactiviteit (voorheen operationeel of controlelogboeken) zoeken. Het logboek bevat informatie zoals de tijden wanneer resources worden gemaakt of vernietigd.  

### <a name="host-vm"></a>VM-host
**Alleen berekenen**


Sommige bronnen zoals Cloud-Services, virtuele Machines, berekenen en Fabric-Service een specifieke Host VM ze ermee hebben. De Host-VM is gelijk aan het basis-VM in de Hyper-V-hypervisor model. In dit geval kunt u parameters verzamelen op alleen de Host VM naast het Gastbesturingssysteem.  

Voor andere Azure services is niet noodzakelijkerwijs een 1:1 toewijzing tussen de bron en een bepaalde Host VM dus host VM metrics niet beschikbaar zijn.


### <a name="resource---metrics-and-diagnostics-logs"></a>Resource - maatstaven en diagnostische logboeken
Collectable metrics afhankelijk van het resourcetype. Virtuele Machines bevat bijvoorbeeld statistieken over het percentage CPU en schijf-i/o. Maar deze statistieken niet aanwezig zijn voor een Service Bus wachtrij, waarmee u in plaats daarvan parameters zoals wachtrij grootte bericht en de doorvoer.

Voor compute resources kunt u parameters op de Gast OS en diagnostische modules zoals Azure diagnostische gegevens. Azure diagnostische gegevens kunt verzamelen en doorsturen verzamelen diagnostische gegevens naar andere locaties, met inbegrip van Azure opslag.

Een lijst met momenteel collectable metrics is beschikbaar op de [ondersteunde parameters](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Toepassing - diagnostische logboeken toepassingen en statistieken
**Alleen berekenen**

Toepassingen kunnen worden uitgevoerd op de Gast OS in de compute model. Het uitzenden van een eigen set van Logboeken en statistieken.

Soorten metrics

- Prestatiemeteritems
- Toepassingslogboeken
- Windows-gebeurtenislogboeken
- Gebeurtenisbron .NET
- IIS-logboeken
- Het manifest op basis van ETW
- Crashdumps
- Klant foutenlogboeken


## <a name="uses-for-monitoring-data"></a>Wordt gebruikt voor het controleren van gegevens

### <a name="visualize"></a>Visualiseren
Visualiseren van de meetgegevens in afbeeldingen en grafieken kunt u veel sneller dan de gegevens zelf kijken trends vinden.  

Een paar visualisatie methoden omvatten:

- Gebruik de Azure portal
- Gegevens routeren naar Azure toepassing inzichten
- Gegevens routeren naar Microsoft PowerBI
- De gegevens doorsturen naar een derde partij visualisatie tool met live streaming of met het hulpprogramma voor het lezen uit een archief in Azure opslag

### <a name="archive"></a>Archief
Controle van gegevens is meestal geschreven naar Azure opslag en er worden bewaard totdat u deze verwijdert.

Een paar manieren om deze gegevens te gebruiken:

- Een eenmaal geschreven, kunt u andere hulpprogramma's binnen of buiten de Azure worden gelezen en verwerkt hebben.
- U downloadt de gegevens voor een lokale archief lokaal of het bewaarbeleid in de cloud te bewaren gegevens voor langere tijd wijzigen.  
- U laat de gegevens in Azure opslag voor onbepaalde tijd, maar u moet betalen voor Azure opslag op basis van de hoeveelheid gegevens die u wilt behouden.
-

### <a name="query"></a>Query
U kunt de Azure Monitor REST-API, cross-platform opdrachtregelinterface (CLI) opdrachten, PowerShell-cmdlets of de .NET SDK toegang tot de gegevens in het systeem of Azure opslag

Voorbeelden zijn:

-  Ophalen van gegevens voor een aangepaste controle toepassing die u hebt geschreven.
-  Aangepaste query's maken en die gegevens verzenden naar een toepassing van derden.

### <a name="route"></a>Route
U kunt de controlegegevens op een andere locatie in real-time streamen.

Voorbeelden zijn:

- Verzenden naar toepassing inzichten zodat u er de visualisatiehulpmiddelen kunt gebruiken.
- Naar Hubs gebeurtenis verzenden, zodat u naar hulpprogramma's van derden doorsturen kunt voor real-time analyse.

### <a name="automate"></a>Automatiseren
Controlegegevens trigger-gebeurtenissen kunt u of zelfs hele processen voorbeelden zijn:

- Gegevens gebruiken om automatisch schalen compute exemplaren omhoog of omlaag op basis van een toepassing laden.
- E-mailberichten verzenden wanneer een metric een vooraf bepaalde drempel overschrijdt.
- Aanroepen van een web-URL (webhook) voor het uitvoeren van een actie in een systeem buiten Azure
- Een runbook in Azure automatisering voor het uitvoeren van een groot aantal taken starten

## <a name="methods-of-use"></a>Methoden voor het gebruik
In het algemeen kunt u gegevens bijhouden, routeren en ophalen met behulp van een van de volgende manieren bewerken. Niet alle methoden zijn beschikbaar voor alle acties of gegevenstypen.

- [Azure portal](https://portal.azure.com)
- [PowerShell](insights-powershell-samples.md)  
- [Cross-platform Command Line Interface (CLI)](insights-cli-samples.md)
- [REST-API](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Monitoring van Azure-aanbiedingen
Azure is beschikbaar voor het controleren van de services van kaal infrastructuur voor telemetrie van toepassing. De beste strategie controle worden gecombineerd gebruik van alle drie tot uitgebreide, gedetailleerde inzicht in de gezondheid van uw diensten.

- [Azure Monitor](http://aka.ms/azmondocs) – aanbiedingen visualisatie, query, routering, kennisgevingen, automatisch schalen en automatisering van gegevens afkomstig uit de Azure infrastructuur (activiteitenlogboek) en elke afzonderlijke Azure resource (diagnostische logboeken). Dit artikel maakt deel uit van de documentatie bij de Monitor Azure. De naam Azure Monitor is uitgebracht op 25 September op Ignite 2016.  De oude naam was "Azure inzichten."  
- [Inzichten van toepassing](https://azure.microsoft.com/documentation/services/application-insights/) – biedt uitgebreide detectie en diagnose voor problemen op de toepassingslaag van uw service, goed geïntegreerd op gegevens uit het toezicht op Azure. Het is de standaard diagnostische platform voor Web Apps van App-Service.  U kunt gegevens uit andere services te routeren.  
- [Logboek Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) bevat [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – een holistische oplossing voor IT-beheer voor zowel op locatie als derden cloud-infrastructuur (zoals AWS) naast Azure bronnen.  Gegevens van Azure Monitor kunnen worden gerouteerd rechtstreeks naar het logboek voor Analytics zodat u Logboeken en maatstaven voor uw hele omgeving op één plaats kunt zien.     


## <a name="next-steps"></a>Volgende stappen
Meer informatie over

- [Azure Monitor in een video van Ignite 2016](https://myignite.microsoft.com/videos/4977)
- [Aan de slag met Azure Monitor](monitoring-get-started.md)
- [Azure Diagnostics](../azure-diagnostics.md) als u probeert te diagnosticeren in uw toepassing Cloud Service, virtuele Machine of Fabric-Service.
- [Inzichten van toepassing](https://azure.microsoft.com/documentation/services/application-insights/) als u om diagnostische problemen in uw App Service Web app probeert.
- [Azure opslag het oplossen van problemen](../storage/storage-e2e-troubleshooting.md) bij het gebruik van opslag BLOB's, tabellen of wachtrijen
- [Logboek Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) en het [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)
