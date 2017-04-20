<properties
    pageTitle="Azure Monitor CLI slag monsters. | Microsoft Azure"
    description="CLI Voorbeeldopdrachten voor Azure Monitor functies. Azure Monitor is een Microsoft Azure service kunt u waarschuwingsberichten verzenden, web-URL's op basis van de waarden van de geconfigureerde telemetrie data, en automatisch schalen Cloud-Services, virtuele Machines en Web Apps bellen."
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure Monitor platforms CLI quick start-voorbeelden

In dit artikel ziet u voorbeelden opdrachtregelinterface (CLI) opdrachten waarmee u toegang tot de functies van Azure Monitor. Azure Monitor kunt u automatisch schalen Cloud-Services, virtuele Machines en Web Apps en meldingen verzenden of bel web-URL's op basis van de waarden van de geconfigureerde telemetriegegevens.

>[AZURE.NOTE] Azure Monitor is de nieuwe naam voor de zogenoemde "Azure inzichten" tot 25 september 2016. Echter bevatten de naamruimten en dus ook de onderstaande opdrachten de 'inzichten'.


## <a name="prerequisites"></a>Vereisten

Als u de CLI Azure al hebt geïnstalleerd, Zie [de Azure CLI installeren](../xplat-cli-install.md). Als u niet bekend met Azure CLI bent, kunt u meer lezen over het op [de CLI Azure voor Mac, Linux en Windows Azure Resource Manager gebruiken](../xplat-cli-azure-resource-manager.md).


In Windows, npm te installeren vanaf de [website van Node.js](https://nodejs.org/). Nadat u de installatie hebt voltooid, CMD.exe met met uitvoeren als Administrator-bevoegdheden uitvoeren het volgende uit de map waarin de npm is geïnstalleerd:

```console
npm install azure-cli --global
```

Ga vervolgens naar een willekeurige map/locatie u wilt gebruiken en typ op de opdrachtregel:

```console
azure help
```

## <a name="log-in-to-azure"></a>Log in op Azure

De eerste stap is het aanmelden bij uw account Azure.

```console
azure login
```

Nadat deze opdracht is uitgevoerd, moet u aanmelden via de instructies op het scherm. Hierna ziet u uw Account, TenantId en standaard-abonnement-id. Alle opdrachten werken in de context van uw standaardabonnement.

Gebruik de volgende opdracht als u de details van uw huidige abonnement.

```console
azure account show
```

Als u een ander abonnement context werken, moet u de volgende opdracht gebruiken.

```console
azure account set "subscription ID or subscription name"
```

Azure Resource Manager en Azure Monitor als opdrachten wilt gebruiken, moet u in de modus Azure Resource Manager.

```console
azure config mode arm
```

Een overzicht van alle ondersteunde Azure Monitor opdrachten als volgt te werk.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Controlelogboeken weergeven voor een abonnement

Ga als volgt te werk om een lijst weergeven van controlelogboeken.

```console
azure insights logs list [options]
```

Probeer het volgende om alle beschikbare opties weer te geven.

```console
azure insights logs list -help
```

Hier volgt een voorbeeld aan logboeken door een resourceGroup lijst

```console
azure insights logs list --resourceGroup "myrg1"
```

Voorbeeld van de lijst met Logboeken door beller

```console
azure insights logs list --caller "myname@company.com"
```

Voorbeeld van de lijst met Logboeken door beller op een brontype binnen een begin- en datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Werken met waarschuwingen
U kunt de informatie in de sectie werken met waarschuwingen.

### <a name="get-alert-rules-in-a-resource-group"></a>Waarschuwingsregels ophalen in een resourcegroep

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Een metrische waarschuwingsregel maken

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>De waarschuwingsregel voor een logboek maken

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Webtest waarschuwingsregel maken

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Een waarschuwingsregel verwijderen

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Aanmelden, profielen
Gebruik de informatie in deze sectie voor het werken met profielen in logboek.

### <a name="get-a-log-profile"></a>Een logboek profiel ophalen

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Een logboek profiel zonder inhouding toevoegen

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Een logboek profiel verwijderen

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Een logboek profiel zonder inhouding toevoegen

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Een logboek profiel met behoud en EventHub toevoegen

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostische gegevens
Gebruik de informatie in deze sectie om te werken met diagnostische instellingen.

### <a name="get-a-diagnostic-setting"></a>Een diagnostische instelling ophalen

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Een diagnostische instelling uitschakelen

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Een diagnose is ingeschakeld zonder inhouding

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatisch schalen
Gebruik de informatie in deze sectie om te werken met instellingen voor automatisch schalen. U moet deze voorbeelden aanpassen.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Instellingen voor een resourcegroep automatisch schalen

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Automatisch schalen instellingen opvragen met de naam in een resourcegroep

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Auotoscale instellen

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
