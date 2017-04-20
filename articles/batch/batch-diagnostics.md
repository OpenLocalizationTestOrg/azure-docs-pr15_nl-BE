<properties
   pageTitle="Azure Batch vastleggen van diagnostische gegevens | Microsoft Azure"
   description="Vastleggen en analyseren van diagnostische gebeurtenissen voor Azure Batch account bronnen zoals toepassingen en taken."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure Batch vastleggen van diagnostische gegevens

Net als bij veel Azure services, zendt de Batch-service voor bepaalde resources de gebeurtenissen tijdens de levensduur van de resource. U kunt Batch Azure diagnostische logboeken te registreren gebeurtenissen voor bronnen, zoals toepassingen en taken inschakelen en vervolgens met behulp van de logboeken voor toezicht en evaluatie van diagnostische. Gebeurtenissen zoals de groep van toepassingen maken, verwijderen van toepassingen, begin, voltooid en anderen in Batch diagnostische logboeken zijn opgenomen.

>[AZURE.NOTE] Logboekregistratie van gebeurtenissen voor Batch account resources zelf wordt beschreven, niet de taak en taak uitvoergegevens. Zie voor meer informatie over het opslaan van de gegevens over de output van uw werkzaamheden en taken [persistent Azure project en batchuitvoer](batch-task-output.md).

## <a name="prerequisites"></a>Vereisten

* [Azure Batch-account](batch-account-create-portal.md)

* [Azure opslag account](../storage/storage-create-storage-account.md#create-a-storage-account)

  Persistent maken Batch diagnostische logboeken, moet u een opslag Azure account waar Azure de logboekbestanden wilt opslaan. U deze opslag account opgeven wanneer u [Diagnostische logboekregistratie inschakelen](#enable-diagnostic-logging) voor uw Batch-account. De opslag-account die u opgeeft wanneer u de collectie logboek inschakelen is niet hetzelfde als bedoeld in de artikelen [toepassingspakketten](batch-application-packages.md) en [taak uitvoer persistentie](batch-task-output.md) opslag gekoppelde account.

  >[AZURE.WARNING] Bent u **in rekening gebracht** voor de gegevens die zijn opgeslagen in uw account Azure opslag. Dit omvat de diagnostische logboeken die in dit artikel besproken. Houd hier rekening mee bij het ontwerpen van het [bewaarbeleid voor logboeken](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Diagnostische logboekregistratie inschakelen

Diagnostische logboekregistratie is niet ingeschakeld voor uw account voor de Batch standaard. Registratie van diagnostische gegevens voor elke Batch-account die u wilt controleren, moet u expliciet inschakelen:

[Het verzamelen van diagnostische logboeken inschakelen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Wij raden aan dat u het volledige [overzicht van Azure diagnoselogboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel voor een goed begrip van niet alleen hoe het inschakelen van logboekregistratie, maar de categorieën log wordt ondersteund door de verschillende Azure services lezen. Azure Batch ondersteunt bijvoorbeeld momenteel één logboek categorie: **Service, logboeken**.

## <a name="service-logs"></a>Service, Logboeken

Azure Batch, Service, logboeken bevatten gebeurtenissen die door de service Azure Batch uitgestoten gedurende de levensduur van een Batch-bron, zoals een groep of taak. Elke gebeurtenis die wordt uitgestoten door de Batch wordt opgeslagen in de opgegeven account opslag in JSON-indeling. Dit is bijvoorbeeld de hoofdtekst van een steekproef **van toepassingen gebeurtenis maken**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Hoofdtekst van de gebeurtenis zich bevindt in een .json-bestand in de opgegeven account voor de opslag van Azure. Als u rechtstreeks toegang tot de logboeken, kunt u het [schema van de diagnostische logboeken in de opslag-account](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account)controleren.

## <a name="service-log-events"></a>De gebeurtenissen in service

De Batch-service zendt op dit moment de volgende Service-gebeurtenissen. Deze lijst mag niet uitputtend, omdat extra gebeurtenissen zijn toegevoegd sinds dit artikel voor het laatst is bijgewerkt.

| **De gebeurtenissen in service** |
| ------------------ |
| [Groep van toepassingen maken][pool_create] |
| [Begin van groep verwijderen][pool_delete_start] |
| [Toepassingen verwijderen voltooid][pool_delete_complete] |
| [Groep grootte van start][pool_resize_start] |
| [Volledige groep formaat wijzigen][pool_resize_complete] |
| [Taak starten][task_start] |
| [Taak voltooid][task_complete] |
| [Taak mislukt][task_fail] |

## <a name="next-steps"></a>Volgende stappen

Naast diagnostische gebeurtenissen op te slaan in een opslag Azure-account, kunt u ook streamen Batch Service gebeurtenissen naar een [Azure gebeurtenis Hub](../event-hubs/event-hubs-what-is-event-hubs.md)en stuur ze naar [Azure logboek Analytics](../log-analytics/log-analytics-overview.md).

* [Stream-Azure diagnostische logboeken naar gebeurtenis Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Diagnostische gebeurtenissen aan de uiterst schaalbaar gegevens ingress-service gebeurtenis Hubs Batch streamen. Gebeurtenis Hubs kunnen miljoenen gebeurtenissen per seconde, die u vervolgens kunt transformeren en opslaan met behulp van een real time analytics provider te nemen.

* [Azure diagnostische logboeken met Analytics logboek analyseren](../log-analytics/log-analytics-azure-storage-json.md)

  De diagnostische logboeken naar logboek Analytics kunt u analyseren ze in de portal Operations Management Suite (OMS), of deze exporteren voor analyse in Power BI of Excel sturen.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
