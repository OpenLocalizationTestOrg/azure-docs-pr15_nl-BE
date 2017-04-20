<properties
    pageTitle="Azure diagnostische logboeken naar gebeurtenis Hubs streamen | Microsoft Azure"
    description="Informatie over het streamen van Azure diagnostische logboeken naar gebeurtenis Hubs."
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
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Stream-Azure diagnostische logboeken naar gebeurtenis Hubs

**[Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)** kunnen worden gestreamd in bijna real-time voor alle toepassingen met behulp van de ingebouwde 'Exporteren naar gebeurtenis Hubs' optie in de Portal of doordat de Bus regel-Id in een diagnostische instelling via de Azure PowerShell-Cmdlets of Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Wat u kunt doen met diagnostische logboeken en gebeurtenis-Hubs
Hier zijn een paar manieren kunt u de streaming mogelijkheden voor diagnostische logboeken:

- **Stream logboeken 3de partij logging en telemetrie systemen** – na verloop van tijd gebeurtenis Hubs streaming worden het mechanisme voor de diagnostische logboeken naar derden SIEMs pipe en meld u analytics oplossingen.

- **View service gezondheid door "hot pad" gegevensstromen naar PowerBI** – gebeurtenis Hubs met behulp van Analytics stroom en PowerBI, u de diagnostische gegevens in real-time inzicht in de buurt van uw Azure services eenvoudig kunt veranderen. In [dit artikel documentatie geeft een overzicht van de manier waarop een gebeurtenis Hubs instellen, gegevens met stroom Analytics, verwerken en gebruiken van PowerBI als een uitvoer](../stream-analytics/stream-analytics-power-bi-dashboard.md). Hieronder vindt u enkele tips voor het getting ingesteld met diagnostische logboeken:
    - De gebeurtenis Hubs voor een categorie van diagnostische logboeken wordt automatisch gemaakt wanneer u de optie in de portal te controleren of inschakelen via PowerShell, dus u moet de gebeurtenis Hubs selecteren in de naamruimte Bus Service met de naam die begint met "inzichten-"
    - Dit is een Stream Analytics voorbeeldquery die kunt u alle gegevens in een tabel PowerBI te parseren:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Bouwen van een aangepaste Telemetrie en logboekregistratie platform** – als u al een op maat gemaakte telemetrie platform of terugblikt een zijn, zeer schaalbare publicatie abonnementen aard van de gebeurtenis Hubs kunt u flexibel consumptie van diagnostische logboeken. [Van Zie Dan Rosanova handleiding voor het gebruik van Hubs gebeurtenis op een wereldwijde schaal telemetrie platform hier](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Streaming van diagnostische logboeken inschakelen
U kunt via programmacode, via de portal van diagnostische logboeken streaming of [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx)gebruiken inschakelen. Hoe dan ook, kiest u een Service Bus Namespace en wordt een gebeurtenis Hubs gemaakt in de naamruimte voor elke categorie logboek is ingeschakeld. Een diagnostische **Logboek categorie** is een type logboek dat een resource kan verzamelen. U kunt selecteren welke logboek categorieën die u wilt verzamelen voor een bepaalde bron in de Portal Azure onder de blade diagnostische gegevens.

![Logboek categorieën in de Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Inschakelen en streaming diagnoselogboeken van Compute bronnen (bijvoorbeeld VMs of Fabric-Service) [een andere reeks stappen vereist](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Om in te schakelen via de [Azure PowerShell Cmdlets](insights-powershell-samples.md)streaming, kunt u de `Set-AzureRmDiagnosticSetting` cmdlet met de volgende parameters:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

De Bus regel-ID is een tekenreeks met de volgende notatie: `{service bus resource ID}/authorizationrules/{key name}`, bijvoorbeeld, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Via Azure CLI
Om in te schakelen via de [Azure CLI](insights-cli-samples.md)streaming, kunt u de `insights diagnostic set` opdracht als volgt:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Gebruik voor Service Bus regel-ID dezelfde notatie zoals uiteengezet voor de PowerShell-Cmdlet.

###<a name="via-azure-portal"></a>Via Azure-Portal
Als streaming via de Portal Azure, Ga naar de instellingen van de diagnostische gegevens van een resource en selecteer 'Exporteren naar de gebeurtenis Hub.'

![Exporteren naar een gebeurtenis Hubs in de Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Om deze te configureren, selecteert u een bestaande Service Bus Namespace. Wanneer de gebeurtenis Hubs is gemaakt (als dit de eerste keer streaming diagnoselogboeken is) of stroomsgewijs verzonden naar de geselecteerde naamruimte worden (als er nog bronnen die in die categorie logboek naar deze naamruimte streaming), en het beleid bepaalt de machtigingen die de streaming mechanisme heeft. Vandaag, vereist machtigingen beheren, lezen en verzenden naar een gebeurtenis Hubs streaming. U kunt maken of wijzigen van Service Bus Namespace gedeeld-beleid in de klassieke portal op het tabblad 'Configureren' voor uw Service Bus Namespace. Om een van deze diagnostische instellingen, de client gemachtigd de ListKey op de verificatieregel Service Bus.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hoe ik de logboekgegevens van de gebeurtenis Hubs verbruiken?
Hier ziet u voorbeeldgegevens voor uitvoer van de gebeurtenis Hubs:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Elementnaam | Beschrijving                                            |
|--------------|--------------------------------------------------------|
|records       | Een matrix van alle gebeurtenissen in deze nettolading.            |
|tijd          | De tijd waarop de gebeurtenis heeft plaatsgevonden.                      |
|categorie      | Logboek categorie voor deze gebeurtenis.                           |
|resourceId    | Bron-ID van de resource die deze gebeurtenis heeft gegenereerd. |
|operationName | De naam van de bewerking.                                 |
|niveau         | Dit is optioneel. Geeft aan dat het niveau van de logboekbestanden.               |
|Eigenschappen    | Eigenschappen van de gebeurtenis.                               |


U kunt een overzicht van alle resource-providers die ondersteuning voor streaming op gebeurtenis Hub [hier](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Volgende stappen
- [Lees meer over Azure diagnoselogboeken](monitoring-overview-of-diagnostic-logs.md)
- [Aan de slag met gebeurtenis-Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
