<properties
    pageTitle="Azure PowerShell Monitor slag monsters. | Microsoft Azure"
    description="Gebruik PowerShell Azure Monitor functies zoals automatisch schalen, waarschuwingen, webhooks en activiteitenlogboeken zoeken."
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
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure PowerShell Monitor quick start-voorbeelden

In dit artikel aangegeven voorbeeld van PowerShell-opdrachten waarmee u toegang tot de functies van Azure Monitor. Azure Monitor kunt u automatisch schalen Cloud-Services, virtuele Machines en Web Apps en meldingen verzenden of bel web-URL's op basis van de waarden van de geconfigureerde telemetriegegevens.

>[AZURE.NOTE] Azure Monitor is de nieuwe naam voor de zogenoemde "Azure inzichten" tot 25 september 2016. Echter bevatten de naamruimten en dus ook de onderstaande opdrachten de 'inzichten'.

## <a name="set-up-powershell"></a>PowerShell instellen
Als u nog niet gedaan hebt, moet u instellen PowerShell uitvoeren op uw computer. Zie voor meer informatie [het installeren en configureren van PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Voorbeelden in dit artikel

De voorbeelden in het artikel ziet hoe u kunt cmdlets Azure Monitor. U kunt ook de volledige lijst van Azure Monitor PowerShell cmdlets [Cmdlets Azure Monitor (inzichten)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx)bekijken.


## <a name="sign-in-and-use-subscriptions"></a>Log in en gebruik van abonnementen

Meld u eerst in uw abonnement Azure.

```PowerShell
Login-AzureRmAccount
```

Hiervoor moet u zich aanmelden. Als u dit, uw Account doet, worden TenantId en standaard-abonnement-Id weergegeven. De Azure cmdlets werken in het kader van het standaardabonnement. De lijst met abonnementen hebt u toegang, gebruikt u de volgende opdracht.

```PowerShell
Get-AzureRmSubscription
```

Als u een ander abonnement uw context werken, moet u de volgende opdracht gebruiken.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Ophalen van controlelogboeken voor een abonnement
Gebruik de `Get-AzureRmLog` cmdlet.  Hieronder vindt u enkele algemene voorbeelden.

Logboekvermeldingen opvragen vanaf deze datum te presenteren:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Logboekvermeldingen tussen een tijd/datum bereik opvragen:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Logboekvermeldingen krijgen van een bepaalde resourcegroep:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Logboekvermeldingen opvragen bij een specifieke resource provider tussen een bereik van tijd/datum:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Alle vermeldingen in het logboek met een specifieke beller ophalen:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

De volgende opdracht haalt de laatste 1000 gebeurtenissen in het controlelogboek:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`ondersteunt veel andere parameters. Zie de `Get-AzureRmLog` voor meer informatie.

>[AZURE.NOTE] `Get-AzureRmLog`biedt slechts 15 dagen. Met de parameter **- MaxEvents** kunt u de laatste N gebeurtenissen na 15 dagen. Gebeurtenissen die ouder zijn dan 15 dagen, gebruikt u de REST API of de SDK (C# voorbeeld met de SDK). Als u geen **Starttijd**, is de standaardwaarde **EndTime** minus één uur. Als u geen **eindtijd**, wordt de standaardwaarde huidige tijd. Alle tijden zijn UTC.

## <a name="retrieve-alerts-history"></a>Geschiedenis meldingen ophalen
Alle waarschuwingen als gebeurtenissen wilt weergeven, kunt u de logboeken van Azure Resource Manager met behulp van de volgende voorbeelden kunt opvragen.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Als u wilt weergeven in de geschiedenis van een specifieke regel voor waarschuwingen, kunt u de `Get-AzureRmAlertHistory` cmdlet, in de resource-ID van de waarschuwingsregel wordt doorgegeven.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

De `Get-AzureRmAlertHistory` cmdlet ondersteunt diverse parameters. Meer informatie, Zie de [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Informatie over waarschuwingsregels ophalen
Van de volgende opdrachten werken op een groep met de naam 'montest'.

De eigenschappen van de waarschuwingsregel weergeven:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Alle waarschuwingen voor een resourcegroep ophalen:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Alle regels instellen voor een resource doel halen. Alle waarschuwingsregels instellen, bijvoorbeeld op een VM.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`ondersteunt andere parameters. [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) Zie voor meer informatie.

## <a name="create-alert-rules"></a>Waarschuwingsregels maken
U kunt de `Add-AlertRule` cmdlet maken, bijwerken of uitschakelen van een waarschuwingsregel.

Kunt u e-mail- en webhook eigenschappen met behulp van `New-AzureRmAlertRuleEmail` en `New-AzureRmAlertRuleWebhook`, respectievelijk. In de cmdlet waarschuwingsregel deze acties te wijzen aan de eigenschap van de **Acties** van de waarschuwingsregel.

De volgende sectie bevat een voorbeeld waarin een waarschuwingsregel maken met verschillende parameters.

### <a name="alert-rule-on-a-metric"></a>De waarschuwingsregel op een metric
De volgende tabel beschrijft de parameters en waarden voor het maken van een waarschuwing met een metric.


|parameter|waarde|
|---|---|
|Naam|  simpletestdiskwrite|
|Locatie van deze waarschuwingsregel|   Oost-Verenigde Staten|
|ResourceGroup| montest|
|TargetResourceId|  /Subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig|
|MetricName van de waarschuwing die wordt gemaakt|   \Disk \PhysicalDisk (_Totaal) per seconde. Zie de `Get-MetricDefinitions` cmdlet hieronder over hoe de metrische exacte namen ophalen|
|operator|  Groter dan|
|Drempelwaarde (aantal per seconde in voor deze metriek)|    1|
|WindowSize, (indeling: mm: SS)|  00:05:00|
|lezer (statistiek van de metric die gebruikmaakt van het gemiddelde aantal, in dit geval)|  Gemiddelde|
|aangepaste e-mails (string-matrix)|'foo@example.com','bar@example.com'|
|e-mail verzenden aan eigenaren, auteurs en lezers|    -SendToServiceOwners|

Een e-actie maken

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

De actie van een Webhook maken

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

De waarschuwingsregel maken op de CPU %-metric van een klassieke VM

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

De waarschuwingsregel ophalen

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

De regel de waarschuwing cmdlet Add ook bijgewerkt als er al een waarschuwingsregel voor de opgegeven eigenschappen bestaat. Schakel een waarschuwingsregel omvatten de parameter **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Waarschuw voor logboek controlegebeurtenis

>[AZURE.NOTE] Deze functie is nog steeds in de voorbeeldweergave.

In dit scenario wordt sturen u e-mail wanneer een website is gestart in mijn abonnement in een resource-groep *abhingrgtest123*.

Een regel voor e-mailadres instellen

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Een regel webhook Setup

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

De regel maken op de gebeurtenis

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

De waarschuwingsregel ophalen

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

De `Add-AlertRule` cmdlet kunt u diverse andere parameters. Meer informatie Zie [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Een lijst met beschikbare parameters ophalen voor waarschuwingen
U kunt de `Get-AzureRmMetricDefinition` cmdlet om de lijst met alle parameters voor een bepaalde resource weer te geven.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

In het volgende voorbeeld wordt een tabel met de naam van de kosten en de eenheid voor het gegenereerd.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Een volledige lijst van beschikbare opties voor `Get-AzureRmMetricDefinition` vindt u op de [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Maken en beheren van instellingen voor automatisch schalen
Een bron, zoals een Web app VM, Cloud-Service of VM schaal ingesteld kan slechts één geconfigureerd voor het automatisch schalen-instelling hebben.
Elke instelling automatisch schalen hebben echter meerdere profielen. Bijvoorbeeld een schaal op basis van prestatie-profiel en een tweede voor een planning op basis van profiel. Elk profiel kan meerdere regels geconfigureerd voor hebben. Zie [het automatisch schalen van een toepassing](../cloud-services/cloud-services-how-to-scale.md)voor meer informatie over het automatisch schalen.

Hier zijn de stappen die we gebruiken:

1. (S) maken.
2. Profiel(en) eerder toewijzing van de regels die u hebt gemaakt om de profielen te maken.
3. Optioneel: Maak meldingen voor het automatisch schalen door webhook en e-eigenschappen te configureren.
4. Een instelling automatisch schalen met een naam voor de doelbron maken door toewijzing van profielen en berichten die u in de vorige stappen hebt gemaakt.

De volgende voorbeelden ziet u hoe u een instelling automatisch schalen voor een VM schaal ingesteld voor een Windows-besturingssysteem op basis met behulp van de metric van CPU-gebruik kunt maken.

Maak eerst een regel voor het geschaald uitbreiden met een aantal exemplaar stijging.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Maak vervolgens een regel voor schaal-in, met een daling van de telling exemplaar.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Maak vervolgens een profiel voor de regels.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Maak een webhook-eigenschap.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Maken melding van de instelling automatisch schalen, met inbegrip van e-mail en de webhook die u eerder hebt gemaakt.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Tot slot maakt u de instelling automatisch schalen om het profiel dat u hierboven hebt gemaakt.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Zie voor meer informatie over het beheren van instellingen voor automatisch schalen, [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Geschiedenis van automatisch schalen
In het volgende voorbeeld ziet u hoe u recente automatisch schalen en alert gebeurtenissen kunt bekijken. Met de zoekopdracht audit-logboek kunt u de geschiedenis automatisch schalen.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

U kunt de `Get-AzureRmAutoScaleHistory` cmdlet automatisch schalen geschiedenis ophalen.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Zie [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx)voor meer informatie.

### <a name="view-details-for-an-autoscale-setting"></a>Details weergeven voor een instelling automatisch schalen
U kunt de `Get-Autoscalesetting` cmdlet ophalen meer informatie over de instelling automatisch schalen.

In het volgende voorbeeld bevat details over alle instellingen voor automatisch schalen in de resource-groep 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

In het volgende voorbeeld bevat details over alle instellingen voor automatisch schalen in de resource-groep 'myrg1' en met name de automatisch schalen instelling met de naam 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Verwijderen van een instelling automatisch schalen
U kunt de `Remove-Autoscalesetting` cmdlet een instelling automatisch schalen te verwijderen.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Logboek profielen voor controlelogboeken beheren

U kunt een *logboek profiel* maken en gegevens van de controlelogboeken exporteren naar een opslag-account en kunt u de gegevens voor het configureren. U kunt eventueel ook de gegevens op uw Hub gebeurtenis streamen. Opmerking: deze functie momenteel in voorvertoning en u is kunt slechts één logboek profiel per abonnement maken. De volgende cmdlets kunt u met uw huidige abonnement maken en beheren van logboek-profielen. U kunt ook een bepaald abonnement. Hoewel PowerShell wordt standaard het huidige abonnement, kunt u altijd wijzigen die met behulp van `Set-AzureRmContext`. U kunt de controlelogboeken gegevens routeren naar een opslag- of gebeurtenis Hub binnen dit abonnement configureren. Gegevens worden geschreven als blob-bestanden in JSON-indeling.

### <a name="get-a-log-profile"></a>Een logboek profiel ophalen
Als u wilt uw bestaande profielen voor logboek ophalen, gebruikt u de `Get-AzureRmLogProfile` cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Een logboek profiel zonder gegevens toevoegen

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Een logboek profiel verwijderen

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Een logboek profiel met gegevensbehoud toevoegen

De **RetentionInDays -** eigenschap kunt u opgeven met het aantal dagen, als een positief geheel getal, waarbij de gegevens blijft behouden.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Logboek profiel met behoud en EventHub toevoegen
Naast routering uw gegevens naar opslag account, kunt u deze ook op een Hub gebeurtenis streamen. Houd er rekening mee dat accountconfiguratie in deze preview-versie en de opslag verplicht is maar gebeurtenis Hub configuratie optioneel is.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Diagnostische logboeken configureren
Veel Azure services bieden extra logboeken en telemetrie, Azure netwerk beveiligingsgroepen, Netwerktaakverdeling Software, sleutel kluis, Azure Search Services, waaronder en logica Apps, en kan worden geconfigureerd voor het opslaan van gegevens in uw account Azure opslag. Deze bewerking kan alleen worden uitgevoerd op een niveau van de resource en de opslag-account moet worden opgenomen in hetzelfde gebied, als de doelbron waar de instelling van de diagnose is geconfigureerd.

### <a name="get-diagnostic-setting"></a>Diagnostische instelling ophalen

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnostische instelling uitschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Diagnostische ingeschakeld zonder inhouding

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Diagnostische ingeschakeld zonder inhouding

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Diagnostische instelling zonder inhouding voor de categorie van een bepaald logboek inschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
