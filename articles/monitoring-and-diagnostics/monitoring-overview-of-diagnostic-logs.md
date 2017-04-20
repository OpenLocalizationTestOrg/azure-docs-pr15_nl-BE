<properties
    pageTitle="Overzicht van de diagnostische logboeken Azure | Microsoft Azure"
    description="Informatie over diagnostische logboeken Azure en hoe u ze kunt gebruiken om te begrijpen van gebeurtenissen die plaatsvinden binnen een Azure-bron."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Overzicht van Azure diagnoselogboeken
**Azure diagnostische logboeken** worden logboeken uitgestoten door een resource die rijke, vaak gegevens over de werking van die bron. De inhoud van deze logboeken is afhankelijk van het resourcetype (bijvoorbeeld Windows-gebeurtenislogboeken systeem zijn één categorie van diagnostische logboeken voor VMs en blob, tabel en wachtrij logs zijn categorieën van diagnostische logboeken voor opslag accounts) en de verschillen van het [logboek voor faxactiviteit (voorheen bekend als controlelogboek of operationele logboek)](monitoring-overview-activity-logs.md), die inzicht geven in de bewerkingen die worden uitgevoerd op de resources in uw abonnement. Niet alle bronnen ondersteuning voor het nieuwe type van diagnostische logboeken die hier worden beschreven. De lijst met ondersteunde Services hieronder ziet u welke resourcetypen ondersteunen de nieuwe diagnostische logboeken.

![Logische plaatsing van de diagnostische logboeken](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Wat u kunt doen met diagnostische logboeken
Hier zijn enkele dingen die u met diagnostische logboeken doen kunt:

- Opslaan naar een **Opslag Account** voor controle- of handmatige controle. U kunt de retentietijd (in dagen) met de **Diagnostische instellingen**opgeven.
- [Deze **Gebeurtenis Hubs** streamen](monitoring-stream-diagnostic-logs-to-event-hubs.md) voor opname door een service van derden of aangepaste analytics oplossing zoals PowerBI.
- Ze analyseren met [OMS logboek Analytics](../log-analytics/log-analytics-azure-storage-json.md)

## <a name="diagnostic-settings"></a>Diagnostische instellingen
Diagnostische logboeken voor resources niet berekenen worden geconfigureerd met behulp van diagnostische instellingen. **Diagnostische instellingen** voor het besturingselement van een resource:

- Waarnaar diagnostische logboeken (Account opslag, gebeurtenis Hubs en/of OMS logboek Analytics) worden verzonden.
- Welke categorieën logboek worden verzonden.
- Hoe lang elke categorie logboek moet worden gehandhaafd op een rekening van de opslag: een inhouding van nul dagen betekent dat de logboeken altijd worden gehouden. Anders is tussen deze waarde 1 en 2147483647. Als bewaarbeleid zijn ingesteld maar de logboeken op te slaan in een opslag-Account is uitgeschakeld (bijvoorbeeld als alleen de gebeurtenis Hubs of OMS opties zijn geselecteerd), het bewaarbeleid hebben geen effect.

Deze instellingen zijn eenvoudig geconfigureerd via de blade diagnostische gegevens voor een resource in de portal Azure, via opdrachten Azure PowerShell en CLI of via de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [AZURE.WARNING] Gebruik [een afzonderlijk mechanisme voor de configuratie en de selectie van outputs](../azure-diagnostics.md)diagnostische logboeken en metrics voor Compute bronnen (bijvoorbeeld VMs of Fabric-Service).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Het verzamelen van diagnostische logboeken inschakelen
Collectie van diagnostische logboeken kan worden ingeschakeld als onderdeel van het maken van een resource of een resource wordt gemaakt via blade van de resource in de Portal. U kunt ook diagnostische logboeken inschakelen op elk gewenst moment met Azure PowerShell of CLI-opdrachten, of de Azure Monitor REST API.

> [AZURE.TIP] Deze instructies kunnen niet rechtstreeks toepassen op elke bron. Zie de koppelingen schema onderaan deze pagina te begrijpen speciale stappen die u op bepaalde brontypen toepassen kunnen.

[Dit artikel wordt beschreven hoe u kunt een sjabloon resource diagnostische instellingen inschakelen bij het maken van een resource](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Diagnostische logboeken inschakelen in de portal
U kunt diagnostische logboeken in Azure portal inschakelen wanneer u compute brontypen maken door de extensie van Windows of Linux Azure Diagnostics inschakelen:

1.  Ga naar **Nieuw** en kies de bron die u geïnteresseerd bent.
2.  Na het configureren van de basisinstellingen en het selecteren van een grootte, in de blade **Instellingen** onder **toezicht**, selecteer **ingeschakeld** en een opslag kiezen waar u wilt de diagnostische logboeken opslaan. Er zijn normale gegevenssnelheden voor opslag en transacties in rekening gebracht wanneer u diagnostische gegevens naar een opslag verzendt.

    ![Diagnostische logboeken inschakelen tijdens het maken van een resource](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Klik op **OK** en de bron maakt.

Voor bronnen niet berekenen, kunt u diagnostische logboeken in Azure portal inschakelen nadat u een resource hebt gemaakt als volgt:

1.  Ga naar het blad voor de resource en open de blade **Diagnostische gegevens** .
2.  Klik **op** en kies een Account voor opslag en/of gebeurtenis Hub.

    ![Diagnostische logboeken inschakelen na het maken van een resource](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  Selecteer onder **Logboeken**, welke **Categorieën logboek** wilt verzamelen of te streamen.
4.  Klik op **Opslaan**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Diagnoselogboeken via PowerShell inschakelen
Als u wilt dat diagnostische logboeken via de Azure PowerShell-Cmdlets, gebruik de volgende opdrachten.

Gebruik deze opdracht zodat de opslag van diagnostische logboeken in een opslag-Account:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

De opslag-ID is de resource-id voor de opslag die u wilt verzenden de logboeken.

Gebruik deze opdracht zodat stroomsgewijze verzending van diagnostische logboeken op een Hub gebeurtenis:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

De Bus regel-ID is een tekenreeks met de volgende notatie: `{service bus resource ID}/authorizationrules/{key name}`.

Gebruik deze opdracht zodat verzenden van diagnostische logboeken naar een werkruimte Analytics logboek:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] De parameter WorkspaceId is niet beschikbaar in de versie van oktober. Het komen in de November release beschikbaar.

U kunt uw logboek Analytics werkruimte-ID in de portal voor Azure.

U kunt deze parameters meerdere Uitvoeropties voor inschakelen als u wilt combineren.

### <a name="enable-diagnostic-logs-via-cli"></a>Diagnoselogboeken via CLI inschakelen
Als u wilt dat diagnostische logboeken via de CLI Azure, moet u de volgende opdrachten gebruiken:

Gebruik deze opdracht zodat de opslag van diagnostische logboeken in een opslag-Account:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

De opslag-ID is de resource-id voor de opslag die u wilt verzenden de logboeken.

Gebruik deze opdracht zodat stroomsgewijze verzending van diagnostische logboeken op een Hub gebeurtenis:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

De Bus regel-ID is een tekenreeks met de volgende notatie: `{service bus resource ID}/authorizationrules/{key name}`.

Gebruik deze opdracht zodat verzenden van diagnostische logboeken naar een werkruimte Analytics logboek:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] De parameter workspaceId is niet beschikbaar in de versie van oktober. Het komen in de November release beschikbaar.

U kunt uw logboek Analytics werkruimte-ID in de portal voor Azure.

U kunt deze parameters meerdere Uitvoeropties voor inschakelen als u wilt combineren.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Diagnoselogboeken via REST API inschakelen
Diagnostische instellingen Azure Monitor REST API gebruiken, Zie [Dit document](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Diagnostische instellingen in de portal beheren

U kunt om ervoor te zorgen dat alle resources zich correct zijn ingesteld met diagnostische instellingen, Ga naar de bladeserver **controleren** in de portal en de blade **Diagnostische logboeken** te openen.

![Diagnostische logboeken blade in de portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

U moet klikt u op 'Meer services' te vinden van de bladeserver controleren.

U kunt in deze blade, weergeven en filteren van alle bronnen die ondersteuning bieden voor diagnostische logboeken om te zien als ze beschikken over diagnostische gegevens ingeschakeld en welke opslag account, hub gebeurtenis en/of logboek Analytics werkruimte die naar deze logboeken worden doorloopt.

![Diagnostische logboeken blade resultaten in de portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Te klikken op een resource, worden alle logboeken die zijn opgeslagen in de opslag en krijgt u de mogelijkheid uitschakelen of wijzigen van de diagnostische instellingen weergegeven. Klik op het downloadpictogram om te downloaden van Logboeken voor een bepaalde periode.

![Diagnostische logboeken blade één resource](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Diagnostische logboeken wordt alleen in deze weergave worden weergegeven en worden gedownload als u diagnostische instellingen deze op te slaan naar een opslag hebt geconfigureerd.

Te klikken op de koppeling voor **Diagnostische instellingen** weergeven het blade diagnostische instellingen, waar u kunt inschakelen, uitschakelen of wijzigen van de diagnostische instellingen voor de geselecteerde resource.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Ondersteunde services en het schema voor diagnostische logboeken
Het schema voor diagnostische logboeken varieert afhankelijk van de categorie resource- en logboekbestanden. Hieronder vindt u de ondersteunde diensten en hun schema.

| Service                       | Schema & Docs                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Software-taakverdeling     |    [Logboek analytics voor Azure-taakverdeling (voorbeeld)](../load-balancer/load-balancer-monitor-log.md)             |
|    Netwerk-beveiligingsgroepen    |    [Logboek analytics voor beveiligingsgroepen netwerk (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Toepassingsgateways       |    [Registratie van diagnostische gegevens voor Application Gateway](../application-gateway/application-gateway-diagnostics.md)     |
|    Sleutel kluis                  |    [Logboekregistratie van Azure sleutel kluis](../key-vault/key-vault-logging.md)                                                 |
|    Azure zoeken               |    [Inschakelen en gebruiken van Search verkeer Analytics](../search/search-traffic-analytics.md)                         |
|    Lake gegevensarchief            |    [Toegang tot diagnostische logboeken voor Azure Lake gegevensarchief](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Gegevens Lake Analytics        |    [Diagnostische logboeken openen voor Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Logica Apps                 |    Geen schema beschikbaar.                                                                                         |
|    Azure Batch                |    [Azure Batch vastleggen van diagnostische gegevens](../batch/batch-diagnostics.md)                                              |
|    Azure automatisering           |    [Logboek analytics voor Azure automatisering](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Gebeurtenis Hub                  |    Geen schema beschikbaar.                                                                                         |
|    Bus service                |    Geen schema beschikbaar.                                                                                         |
|    Stream Analytics           |    Geen schema beschikbaar.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Logboek categorieën per resourcetype wordt ondersteund

|Resourcetype|Categorie|Categorienaam|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Logboeken van de taak|
|Microsoft.Automation/automationAccounts|JobStreams|Taak Streams|
|Microsoft.Batch/batchAccounts|ServiceLog|Service, Logboeken|
|Microsoft.DataLakeAnalytics/accounts|Audit|Controlelogboeken|
|Microsoft.DataLakeAnalytics/accounts|Aanvragen|Aanvragen van logboek|
|Microsoft.DataLakeStore/accounts|Audit|Controlelogboeken|
|Microsoft.DataLakeStore/accounts|Aanvragen|Aanvragen van logboek|
|Microsoft.EventHub/namespaces|ArchiveLogs|Logboeken archiveren|
|Microsoft.EventHub/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.KeyVault/vaults|AuditEvent|Controlelogboeken|
|Microsoft.Logic/workflows|WorkflowRuntime|Diagnostische gebeurtenissen voor workflow-runtime|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Network Security Group, gebeurtenis|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Network Security Group regel teller|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Network Security Group regel stroom, gebeurtenis|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Waarschuwing gebeurtenissen Balancer laadt|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Load Balancer sonde gezondheidsstatus|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Toepassingslogboek van het Gateway-toegang|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Toepassingslogboek van het Gateway-prestaties|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Toepassingslogboek Gateway Firewall|
|Microsoft.Search/searchServices|OperationLogs|Logboeken|
|Microsoft.ServerManagement/nodes|RequestLogs|Aanvragen van logboek|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.StreamAnalytics/streamingjobs|Uitvoering|Uitvoering|
|Microsoft.StreamAnalytics/streamingjobs|Ontwerpen|Ontwerpen|

## <a name="next-steps"></a>Volgende stappen
- [Diagnostische logboeken naar **gebeurtenis Hubs** Stream](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Diagnostische instellingen wijzigen met de Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Analyseren van de logbestanden met OMS logboek Analytics](../log-analytics/log-analytics-azure-storage-json.md)
