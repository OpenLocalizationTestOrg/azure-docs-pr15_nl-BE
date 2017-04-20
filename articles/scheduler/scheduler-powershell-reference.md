<properties
 pageTitle="Scheduler PowerShell Cmdlets Reference"
 description="Scheduler PowerShell Cmdlets Reference"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Scheduler PowerShell Cmdlets Reference

In de volgende tabel wordt beschreven en koppelingen naar de pagina met naslaginformatie voor elk van de belangrijkste cmdlets Azure planner.

Azure PowerShell te installeren en te koppelen aan uw abonnement Azure, Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md). 

Zie voor meer informatie over [Azure Resource Manager-cmdlets](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx) [Gebruiken Azure PowerShell Azure Resource Manager](../powershell-azure-resource-manager.md).

|Cmdlet|Beschrijving van de cmdlet|
|---|---|
[Uitschakelen AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Een collectie van de taak, wordt uitgeschakeld. 
[Inschakelen AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Hiermee bepaalt u een taak.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Met deze eigenschap wordt een geplande taken.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Collecties-taak als resultaat gegeven.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Taakgeschiedenis haalt.
[Nieuwe AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Hiermee maakt u een HTTP-taak.
[Nieuwe AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Hiermee maakt u een collectie taak.
[Nieuwe AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Hiermee maakt u een taak van de wachtrij bus.
[Nieuwe AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Hiermee maakt u een taak van de bus-onderwerp.
[Nieuwe AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Hiermee maakt u een wachtrijtaak opslag. 
[Verwijderen AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Hiermee verwijdert u een geplande taak.  
[Verwijderen AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Hiermee verwijdert u een collectie van de taak. 
[Set AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Hiermee wijzigt u een job Scheduler HTTP.
[Set AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |De collectie van een taak wijzigt. 
[Set AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Hiermee wijzigt u de wachtrijtaak voor een service bus.  
[Set AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Hiermee wijzigt u een taak van de bus-onderwerp. 
[Set AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Hiermee wijzigt u een wachtrijtaak opslag.   

Voor meer gedetailleerde informatie kunt u de volgende cmdlets uitvoeren: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Zie ook


 [Wat is Taakplanner?](scheduler-intro.md)

 [Azure Scheduler concepten en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met de planner in Azure portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure planner](scheduler-plans-billing.md)

 [Azure Scheduler REST API: naslag](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Uitgaande verificatie van Azure Scheduler](scheduler-outbound-authentication.md)
