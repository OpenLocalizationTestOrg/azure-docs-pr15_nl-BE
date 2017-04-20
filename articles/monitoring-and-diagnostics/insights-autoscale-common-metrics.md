<properties
    pageTitle="Azure Monitor autoscaling gemeenschappelijke maatstaven. | Microsoft Azure"
    description="Meer informatie over welke parameters worden vaak gebruikt voor autoscaling Cloud-Services, virtuele Machines en Web Apps."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor autoscaling algemene statistieken

Azure Monitor autoscaling kunt u het aantal actieve exemplaren omhoog of omlaag, op basis van telemetriegegevens (metrics). Dit document beschrijft de algemene criteria die u wilt gebruiken. De Portal Azure voor Cloud Services en Server-Farms kunt u de metric van de resource moet worden geschaald door. Echter, u kunt ook een metric van een andere resource te schalen door.

Hier vindt u de details over het zoeken en weergeven van de parameters die u schalen wilt door. Het volgende is van toepassing voor virtuele Machine schaal stelt ook schalen.

## <a name="compute-metrics"></a>Parameters berekenen
Standaard Azure VM v2 wordt geleverd met diagnostische gegevens extensie geconfigureerd en hebben de volgende metrics ingeschakeld.

- [Maatstaven voor v2 van Windows VM Gast](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Maatstaven voor v2 Linux VM Gast](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

U kunt de `Get MetricDefinitions` PoSH-API/CLI om de parameters beschikbaar voor de bron van de VMSS weer te geven. 

Als u VM schaal wordt gebruikt en u een bepaalde waarde vermeld ziet, is het waarschijnlijk *uitgeschakeld* in de uitbreiding van de diagnostische gegevens.

Als een bepaalde waarde niet wordt bemonsterd of overgebracht met de frequentie die u wilt, kunt u de configuratie van diagnostische bijwerken.

Als beide bovengenoemde gevallen ingesteld op true is, controleert u [Gebruik PowerShell om diagnostiek op een virtuele machine met Windows Azure](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) over PowerShell te configureren en uw extensie Azure VM diagnostische gegevens bijwerken zodat de metric. Dit artikel bevat tevens een voorbeeldbestand van diagnostische gegevens configureren.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Maatstaven voor Windows VM v2 als gast OS berekenen

Wanneer u een nieuwe VM (v2) in Azure maakt, is met behulp van de uitbreiding van de diagnostische gegevens van diagnostische gegevens ingeschakeld.

U kunt een lijst van de statistieken genereren met behulp van de volgende opdracht in PowerShell.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt een waarschuwing voor de volgende parameters.

|Metrische naam|   Eenheid|
|---|---|
|\Processor(_Total)\% processortijd    |Procent|
|\Processor(_Total)\% tijd in beschermde modus   |Procent|
|\Processor(_Total)\% tijd |Procent|
|\Processor (_Totaal) informatie \Processor frequentie |Aantal|
|\System\Processes| Aantal|
|\Thread het aantal \Process (_Totaal)| Aantal|
|\Handle het aantal \Process (_Totaal)  |Aantal|
|\Memory\% toegewezen Bytes In gebruik   |Procent|
|\Memory\Available bytes|   Bytes|
|\Memory\Committed bytes    |Bytes|
|\Memory\Commit beperken|  Bytes|
|\Memory\Pool wisselbaar geheugen: Bytes|  Bytes|
|\Memory\Pool-wisselbaar geheugen: Bytes|   Bytes|
|\PhysicalDisk(_Total)\% schijftijd| Procent|
|\PhysicalDisk(_Total)\% leestijd schijf|    Procent|
|\PhysicalDisk(_Total)\% schrijftijd schijf|   Procent|
|\PhysicalDisk (_Totaal) \Disk-overdrachten per seconde   |CountPerSecond|
|\PhysicalDisk (_Totaal) \Disk Paginaleesbewerkingen per seconde   |CountPerSecond|
|\PhysicalDisk (_Totaal) \Disk per seconde  |CountPerSecond|
|\PhysicalDisk (_Totaal) \Disk bytes per seconde   |BytesPerSecond|
|\PhysicalDisk (_Totaal) \Disk gelezen Bytes per seconde| BytesPerSecond|
|\PhysicalDisk (_Totaal) \Disk geschreven Bytes per seconde |BytesPerSecond|
|\Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf|  Aantal|
|\Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf lezen| Aantal|
|\Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf schrijven |Aantal|
|\LogicalDisk(_Total)\% vrije ruimte| Procent|
|\LogicalDisk (_Totaal) \Free MB|   Aantal|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Maatstaven voor Linux VM v2 als gast OS berekenen

Wanneer u een nieuwe VM (v2) in Azure maakt, is diagnostische gegevens standaard ingeschakeld met behulp van uitbreiding van de diagnostische gegevens.

U kunt een lijst van de statistieken genereren met behulp van de volgende opdracht in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 U kunt een waarschuwing voor de volgende parameters.

|Metrische naam                            |Eenheid|
|---|---|
|\Memory\AvailableMemory                |Bytes|
|\Memory\PercentAvailableMemory         |Procent|
|\Memory\UsedMemory                     |Bytes|
|\Memory\PercentUsedMemory              |Procent|
|\Memory\PercentUsedByCache             |Procent|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bytes|
|\Memory\PercentAvailableSwap           |Procent|
|\Memory\UsedSwap                       |Bytes|
|\Memory\PercentUsedSwap                |Procent|
|\Processor\PercentIdleTime             |Procent|
|\Processor\PercentUserTime             |Procent|
|\Processor\PercentNiceTime             |Procent|
|\Processor\PercentPrivilegedTime       |Procent|
|\Processor\PercentInterruptTime        |Procent|
|\Processor\PercentDPCTime              |Procent|
|\Processor\PercentProcessorTime        |Procent|
|\Processor\PercentIOWaitTime           |Procent|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Seconden|
|\PhysicalDisk\AverageWriteTime         |Seconden|
|\PhysicalDisk\AverageTransferTime      |Seconden|
|\PhysicalDisk\AverageDiskQueueLength   |Aantal|
|\NetworkInterface\BytesTransmitted     |Bytes|
|\NetworkInterface\BytesReceived        |Bytes|
|\NetworkInterface\PacketsTransmitted   |Aantal|
|\NetworkInterface\PacketsReceived      |Aantal|
|\NetworkInterface\BytesTotal           |Bytes|
|\NetworkInterface\TotalRxErrors        |Aantal|
|\NetworkInterface\TotalTxErrors        |Aantal|
|\NetworkInterface\TotalCollisions      |Aantal|




## <a name="commonly-used-web-server-farm-metrics"></a>Maatstelsel van veelgebruikte webpagina (Server-Farm)

U kunt ook automatisch schalen op basis van gemeenschappelijke web server gegevens zoals de lengte van de HTTP-uitvoeren. De metrische naam is **HttpQueueLength**.  In het volgende gedeelte geeft een overzicht van beschikbare server-farm (Web Apps) maatstaven.

### <a name="web-apps-metrics"></a>Web Apps metrics

U kunt een lijst met cijfers over de Web Apps genereren met behulp van de volgende opdracht in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt op een waarschuwing of schalen door deze gegevens.

|Metrische naam        |Eenheid|
|---                |---|
|CpuPercentage      |Procent|
|MemoryPercentage   |Procent|
|DiskQueueLength    |Aantal|
|HttpQueueLength    |Aantal|
|BytesReceived      |Bytes|
|BytesSent          |Bytes|


## <a name="commonly-used-storage-metrics"></a>Veel gebruikte maatstaven voor opslag
U kunt schalen door Storage queue lengte, het aantal berichten in de wachtrij voor de opslag is. Lengte van wachtrij voor opslag is een speciale metric en de drempel toegepast is het aantal berichten per exemplaar. Dit betekent dat als er twee exemplaren en als de drempelwaarde is ingesteld op 100, het wordt aangepast als het totale aantal berichten in de wachtrij 200 zijn. Bijvoorbeeld 100 berichten per exemplaar.

U kunt dit in de Portal Azure in de blade **Instellingen** is. Voor VM-schaal wordt ingesteld, kunt u de instelling automatisch schalen in de ARM sjabloon *metricName* als *ApproximateMessageCount* en geeft u de ID van de wachtrij voor de opslag als *metricResourceUri*bijwerken.

Met een Account met klassieke opslag kan de instelling automatisch schalen metricTrigger zou zijn:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Voor een account (klassieke) opslag, zou de metricTrigger omvatten:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Veelgebruikte Service Bus metrics

U kunt schalen door Service Bus wachtrijlengte, het aantal berichten in de wachtrij Service Bus is. Lengte van de Bus-service is een speciale metric en de drempel van het aantal berichten per exemplaar toegepast worden. Dit betekent dat als er twee exemplaren en als de drempelwaarde is ingesteld op 100, het wordt aangepast als het totale aantal berichten in de wachtrij 200 zijn. Bijvoorbeeld 100 berichten per exemplaar.

Voor VM-schaal wordt ingesteld, kunt u de instelling automatisch schalen in de ARM sjabloon *metricName* als *ApproximateMessageCount* en geeft u de ID van de wachtrij voor de opslag als *metricResourceUri*bijwerken.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Het concept van de groep resource bestaat niet voor Service Bus, maar Azure Resource Manager maakt een standaardgroep per regio. De resourcegroep is meestal in de indeling 'Default - ServiceBus-[regio]'. Bijvoorbeeld 'Standaard-ServiceBus-EastUS', 'Standaard-ServiceBus-WestUS', 'Standaard-ServiceBus-AustraliaEast' enz.
