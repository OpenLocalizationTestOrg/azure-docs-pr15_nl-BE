<properties
    pageTitle="Het Azure activiteitenlogboek op gebeurtenis Hubs streamen | Microsoft Azure"
    description="Informatie over het streamen van het activiteitenlogboek Azure op gebeurtenis Hubs."
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
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream de Azure activiteitenlogboek op gebeurtenis Hubs
Het [**Logboek voor faxactiviteit Azure**](./monitoring-overview-activity-logs.md) kan worden gestreamd in bijna real-time voor alle toepassingen met behulp van de ingebouwde optie 'Exporteren' in de portal of doordat de Bus regel-Id in een logboek profiel via de Azure PowerShell-Cmdlets of Azure CLI.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Wat u kunt doen met het logboek met faxactiviteit en gebeurtenis-Hubs
Hier zijn een paar manieren kunt u de streaming mogelijkheden voor het logboek:

- **Stroom aan derden logboekregistratie en telemetrie systemen** – na verloop van tijd gebeurtenis Hubs streaming worden het mechanisme voor het logboek met faxactiviteit in derde SIEMs pipe en meld u analytics oplossingen.
- **Bouwen van een aangepaste Telemetrie en logboekregistratie platform** – als u al een op maat gemaakte telemetrie platform of terugblikt een zijn, zeer schaalbare publicatie abonnementen aard van de gebeurtenis Hubs kunt u flexibel consumptie van het logboek voor faxactiviteit. [Zie Dan Rosanova de handleiding voor het gebruik van Hubs gebeurtenis op een wereldwijde schaal telemetrie platform hier.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Streaming van het activiteitenlogboek inschakelen
U kunt het inschakelen van het logboek voor faxactiviteit streaming via programmacode of via de portal. In beide gevallen kiest u een Service Bus Namespace en een gedeelde access-beleid voor die naamruimte en een Hub gebeurtenis in die naamruimte wordt gemaakt als de eerste nieuwe gebeurtenis voor logboek met faxactiviteit. Als u een Service Bus Namespace niet hebt, moet u eerst een maken. Als u hebt eerder gestreamd activiteit gebeurtenissen naar deze Service Bus Namespace, wordt de gebeurtenis Hub die eerder is gemaakt opnieuw worden gebruikt. Het beleid voor gedeelde toegang definieert de machtigingen die de streaming mechanisme heeft. Vandaag, vereist machtigingen **beheren**, **lezen**en **verzenden** naar een gebeurtenis Hubs streaming. U kunt maken of wijzigen van Service Bus Namespace gedeeld-beleid in de klassieke portal op het tabblad 'Configureren' voor uw Service Bus Namespace. Om te werken in het logboek voor faxactiviteit logboek profiel opnemen streaming, moet de gebruiker die de wijziging gemachtigd zijn de ListKey op die verificatieregel Service Bus.

### <a name="via-azure-portal"></a>Via Azure-portal 
1. Ga naar het **Logboek voor faxactiviteit** blade via het menu aan de linkerkant van de portal.

    ![Ga naar het logboek voor faxactiviteit in portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klik op de knop **exporteren** aan de bovenkant van het blad.

    ![De knop exporteren in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. In het blad dat wordt weergegeven, kunt u de regio's die u wilt streamen gebeurtenissen en de Service Bus Namespace waarin u een gebeurtenis Hub dat wilt moet worden gemaakt voor het streamen van deze gebeurtenissen.

    ![Logboek voor faxactiviteit blade exporteren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klik op **Opslaan** om deze instellingen opslaan. De instellingen worden direct worden toegepast op uw abonnement.


### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Als een logboek profiel al bestaat, moet u eerst dat profiel verwijderen.

1. Gebruik `Get-AzureRmLogProfile` om aan te duiden als een logboek profiel bestaat
2. In dat geval gebruikt u `Remove-AzureRmLogProfile` om deze te verwijderen.
3. Gebruik `Set-AzureRmLogProfile` om een profiel te maken:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

De Bus regel-ID is een tekenreeks met de volgende notatie: {service bus resource-ID} /authorizationrules/ {naam}, bijvoorbeeld 

### <a name="via-azure-cli"></a>Via Azure CLI
Als een logboek profiel al bestaat, moet u eerst dat profiel verwijderen.

1. Gebruik `azure insights logprofile list` om aan te duiden als een logboek profiel bestaat
2. In dat geval gebruikt u `azure insights logprofile delete` om deze te verwijderen.
3. Gebruik `azure insights logprofile add` om een profiel te maken:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

De Bus regel-ID is een tekenreeks met de volgende notatie: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hoe ik de logboekgegevens van de gebeurtenis Hubs verbruiken?
[Het schema voor het logboek voor faxactiviteit is hier beschikbaar](./monitoring-overview-activity-logs.md). Elke gebeurtenis wordt in een array van JSON BLOB's genaamd "administratie".

## <a name="next-steps"></a>Volgende stappen
- [Het logboek voor faxactiviteit opslag naar een archief](./monitoring-archive-activity-log.md)
- [Lees het overzicht van het activiteitenlogboek Azure](./monitoring-overview-activity-logs.md)
- [Een waarschuwing op basis van een gebeurtenis van het logboek voor faxactiviteit instellen](./insights-auditlog-to-webhook-email.md)
