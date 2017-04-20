<properties
    pageTitle="Het verzamelen van opslaggegevens in logboek Analytics overzicht Azure | Microsoft Azure"
    description="Azure resources kunnen schrijven logs en statistieken aan een account Azure opslag, vaak met Azure diagnostische gegevens. Logboek Analytics kunnen indexeren deze gegevens en doorzoekbaar maken."
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

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Het verzamelen van opslaggegevens in logboek Analytics overzicht Azure

Veel Azure bronnen kunnen logboeken en metrische gegevens schrijven naar een account Azure opslag. Logboek Analytics kan deze gegevens verwerken en gemakkelijker te controleren uw Azure bronnen.

Als u wilt schrijven naar Azure opslag kan een resource Azure diagnostische gegevens gebruiken of hebben een eigen manier om gegevens te schrijven. Deze gegevens kan worden geschreven in verschillende indelingen op een van de volgende locaties:

+ Azure tabel
+ Azure blob
+ EventHub

Logboek Analytics ondersteunt Azure services die gegevens met behulp van [Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Logboek Analytics ondersteunt daarnaast andere services die uitvoer logs en statistieken in verschillende indelingen en locaties.  

>[AZURE.NOTE] U kunt normale Azure gegevenssnelheden transacties wanneer u diagnostische gegevens naar een opslag verzendt voor opslag en logboek Analytics wanneer de gegevens uit uw account opslag leest worden aangerekend.

![Azure opslag diagram](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Ondersteunde Azure Resources

Logboek Analytics kunt verzamelen van gegevens voor de volgende Azure bronnen:

| Resourcetype | Logboeken (diagnostische categorieën) | Logboek Analytics oplossing |
| --------------------------------------- | -------------------------------- | --------------- |
| Inzichten van toepassing | Beschikbaarheid <br> Aangepaste gebeurtenissen <br> Uitzonderingen <br> Aanvragen <br> | Toepassing inzichten (voorbeeld) |
| API-beheer | | *geen* (Voorbeeld) |
| Automatisering <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (voorbeeld) |
| Sleutel kluis <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (voorbeeld) |
| Application Gateway <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (voorbeeld) |
| Network Security Group <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (voorbeeld) |
| Service-Fabric                          | ETWEvent <br> Operationele gebeurtenis <br> Betrouwbare acteur, gebeurtenis <br> Betrouwbare Service, gebeurtenis| ServiceFabric (voorbeeld) |
| Virtuele Machines | Linux Syslog <br> Windows-gebeurtenislogboek <br> IIS-logboek <br> Windows ETWEvent | *geen* |
| Web-rollen <br> Rollen van de werknemer | Linux Syslog <br> Windows-gebeurtenislogboek <br> IIS-logboek <br> Windows ETWEvent | *geen* |

>[AZURE.NOTE] Voor het controleren van Azure virtuele machines (Linux en Windows), is het raadzaam de [extensie logboek Analytics VM](log-analytics-azure-vm-extension.md)te installeren. De agent biedt u meer inzicht in uw virtuele machines dan als u de diagnostische gegevens geschreven naar de opslag.

U kunt ons extra logboeken voor OMS te analyseren door te stemmen op de [feedbackpagina](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy)van onze prioriteit.


- [Diagnoselogboeken Azure analyseren met behulp van Analytics logboek](log-analytics-azure-storage-json.md) Zie voor meer informatie over hoe Log Analytics Lees de logboeken van Azure services die ondersteuning bieden voor [Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md):
  - Azure sleutel kluis
  - Azure automatisering
  - Application Gateway
  - Netwerk-beveiligingsgroepen
- Zie [blob-opslag voor IIS gebruik en de tabelopslag voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) voor meer informatie over hoe Analytics logboek de logboeken lezen kunnen voor Azure services die schrijven diagnostische gegevens tabelopslag of IIS-logboeken geschreven naar een blob-opslag, met inbegrip van:
  - Service-Fabric
  - Web-rollen
  - Rollen van de werknemer
  - Virtuele Machines


Inzichten van toepassing is in particuliere voorbeeld en het continue exporteren naar een blob-opslag wordt gebruikt. Als u wilt deelnemen aan de particuliere voorvertoning, neem contact op met uw Microsoft-Account team of Raadpleeg de details op de [site feedback](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Volgende stappen

- [Diagnoselogboeken Azure analyseren met behulp van Analytics logboek](log-analytics-azure-storage-json.md) lezen de logboeken van Azure services dat diagnostische gegevens van het schrijven naar een blob-opslag in JSON-indeling.
- [Blob-opslag voor IIS gebruik en de tabelopslag voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) voor het lezen van de logboeken voor Azure services die schrijven diagnostische gegevens voor de tabelopslag of de IIS-logboeken geschreven naar een blob-opslag.
- [Inschakelen oplossingen](log-analytics-add-solutions.md) bieden inzicht in de gegevens.
- [Zoekopdrachten gebruiken](log-analytics-log-searches.md) om de gegevens te analyseren.
