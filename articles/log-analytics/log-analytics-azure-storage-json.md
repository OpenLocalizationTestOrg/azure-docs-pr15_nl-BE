<properties
    pageTitle="Azure diagnostische logboeken met Analytics logboek analyseren | Microsoft Azure"
    description="Logboek Analytics kan de logboeken van Azure services die Azure diagnostische logboeken om blob-opslag in JSON-indeling lezen."
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


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Azure diagnostische logboeken met Analytics logboek analyseren

Logboek Analytics kunt verzamelen de logboeken voor de volgende Azure services die [Diagnostische logboeken Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) blob-opslag in JSON-indeling schrijft:

+ Automatisering (voorbeeld)
+ Sleutel kluis (voorbeeld)
+ Application Gateway (voorbeeld)
+ Network Security Group (voorbeeld)

In de volgende secties kunt u doorlopen met PowerShell aan:

+ Analytics voor het verzamelen van de logboeken van de opslag voor elke resource logboekbestanden configureren  
+ Het logboek Analytics oplossing voor de Azure-service inschakelen

Voordat het logboek Analytics kunt verzamelen van gegevens voor deze bronnen, moet Azure diagnostische gegevens worden ingeschakeld. Gebruik de `Set-AzureRmDiagnosticSetting` cmdlet logboekregistratie inschakelen.

Raadpleeg de volgende artikelen voor meer informatie over het inschakelen van de registratie van diagnostische gegevens:

+ [Sleutel kluis](../key-vault/key-vault-logging.md)
+ [Application Gateway](../application-gateway/application-gateway-diagnostics.md)
+ [Network Security Group](../virtual-network/virtual-network-nsg-manage-log.md)

Deze documentatie bevat ook details op:

+ Het verzamelen van gegevens oplossen
+ Het verzamelen van gegevens te stoppen

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Analytics voor het verzamelen van diagnostische logboeken Azure logboekbestanden configureren

Het verzamelen van Logboeken voor deze services en het inschakelen van de oplossing voor het visualiseren van de logboeken wordt uitgevoerd met behulp van PowerShell scripts.

In het volgende voorbeeld kunt zich aanmelden op alle ondersteunde bronnen

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Sommige bronnen is het mogelijk om de vorige configuratie uitvoeren vanuit de Azure portal met behulp van de stappen die worden beschreven in [overzicht van Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs).

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Analytics voor het verzamelen van diagnostische logboeken Azure logboekbestanden configureren

We hebben een PowerShell script-module waarmee twee cmdlets om te helpen bij het configureren van logboek Analytics verstrekt:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`u wordt gevraagd om invoer en kan eenvoudig configuraties instellen
2. `Add-AzureDiagnosticsToLogAnalytics`de middelen om te controleren als invoer neemt en vervolgens Analytics logboek configureert  

### <a name="pre-requisites"></a>Minimumvereisten

1. Azure PowerShell met versie 1.0.8 of nieuwer van de cmdlets operationele inzichten.
  - [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)
  - Controleer of uw versie van cmdlets:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnostische logboekregistratie is geconfigureerd voor de Azure resource die u wilt controleren. Gebruik `Set-AzureRmDiagnosticSetting` of Raadpleeg de [Logboek-Analytics gebruiken voor het verzamelen van gegevens uit accounts Azure opslag](log-analytics-azure-storage.md) voor het inschakelen van diagnostische gegevens.
3. Een [Logboek Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) werkruimte  
4. De module AzureDiagnosticsAndLogAnalytics PowerShell
  - De module [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) van galerie PowerShell downloaden

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Optie 1: De van interactieve configuratiescripts uitvoeren

PowerShell openen en uitvoeren:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

U bent een lijst met beschikbare opties te zien en gezien gevraagd uw selectie te maken.
U wordt gevraagd te selecteren voor elk van de volgende opties:

+ Brontypen (Azure Services) voor het verzamelen van Logboeken
+ Exemplaren van de resource voor het verzamelen van Logboeken
+ Logboek Analytics werkruimte om de gegevens te verzamelen

Nadat dit script is uitgevoerd, worden er records in logboek Analytics ongeveer 30 minuten nadat de nieuwe diagnostische gegevens worden geschreven naar de opslag. Als er records zijn niet beschikbaar na dit tijdstip verwijzen naar de onderstaande sectie over problemen oplossen.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Optie 2: Een lijst met resources maken en ze doorgeven aan de cmdlet configuratie

U kunt een lijst met resources die Azure diagnostics is ingeschakeld en de resources vervolgens doorgeven aan de cmdlet configuratie maken.

U kunt meer informatie over de cmdlet zien door te voeren `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Zoeken naar meer details op OMS [Logboek Analytics PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Als bron en werkruimte in verschillende Azure abonnementen, wisselen zo nodig met behulp van`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Nadat dit script is uitgevoerd, worden er records in logboek Analytics ongeveer 30 minuten nadat de nieuwe diagnostische gegevens worden geschreven naar de opslag. Als er records zijn niet beschikbaar na dit tijdstip verwijzen naar de onderstaande sectie over problemen oplossen.  

>[AZURE.NOTE] De configuratie is niet zichtbaar in de portal Azure. U kunt controleren of configuratie met behulp van de `Get-AzureRmOperationalInsightsStorageInsight` cmdlet.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Analytics logboek stoppen vanaf Azure diagnoselogboeken verzamelen

Het logboek Analytics als configuratie wilt verwijderen voor een resource, gebruikt u de `Remove-AzureRmOperationalInsightsStorageInsight` cmdlet.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Configuratie voor Azure diagnostische logboeken oplossen

*Probleem*

Het volgende foutbericht wordt weergegeven bij het configureren van een resource die is naar de klassieke opslag registreren:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Resolutie*

Log in op de API voor het implementatiemodel klassiek met`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Het verzamelen van gegevens voor Azure diagnostische logboeken oplossen

Als u gegevens voor uw Azure resource in logboek Analytics niet ziet, kunt u de volgende stappen:

+ Controleer of de gegevens die naar de rekening voor opslag
+ Controleer of dat het logboek Analytics oplossing voor de service is ingeschakeld.
+ Of Log Analytics is geconfigureerd om te lezen uit de opslag
+ De sleutel opslag account bijwerken

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Controleer of de gegevens naar de opslag rekening stroomt

U kunt controleren als gegevens naar de opslag rekening met een hulpmiddel waarmee u bladeren in Azure opslag (bijvoorbeeld Visual Studio) of via PowerShell stroomt.

De bron is geconfigureerd om te zoeken naar de rekening van de opslag om te melden voor het gebruik van de volgende PowerShell:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

De opslag container gebruikt door Azure diagnostische gegevens heeft een naam met een indeling van:  

`insights-logs-<Category>`

Zie [opslag cmdlets gebruiken om een container voor recente gegevens](../storage/storage-powershell-guide-full.md) voor meer informatie over het weergeven van de inhoud van de opslag-account.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Controleer of dat het logboek Analytics oplossing voor de service is ingeschakeld.

Als u `Add-AzureDiagnosticsToLogAnalyticsUI`, de juiste oplossing voor logboek Analytics automatisch voor u is ingeschakeld.

Als u wilt controleren of er een oplossing is ingeschakeld, voert u de volgende PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Als de oplossing niet is ingeschakeld, kunt u met behulp van de volgende PowerShell inschakelen:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Als u wilt zoeken naar de naam van de oplossing voor elk resourcetype wilt inschakelen, gebruikt u de volgende PowerShell (deze variabele is beschikbaar zodra u de module hebt geïmporteerd):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Of Log Analytics is geconfigureerd om te lezen uit de opslag

Als u extra Azure Resources toevoegt, moet u Diagnostische logboekregistratie voor deze inschakelen en logboek Analytics voor hen configureren.
Gebruik de volgende PowerShell te controleren welke bronnen en opslag rekeningen logboek Analytics is geconfigureerd voor het verzamelen van Logboeken voor:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>De sleutel voor de opslag wordt bijgewerkt

Als u de sleutel voor de opslag account wijzigt, moet de configuratie van het logboek Analytics ook worden bijgewerkt met de nieuwe sleutel.
U kunt de configuratie van het logboek Analytics bijwerken met een nieuwe sleutel met de volgende PowerShell:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

De opslag inzicht om naam te zoeken, gebruik de `Get-AzureRmOperationalInsightsStorageInsight` cmdlet, zoals in eerdere voorbeelden.

## <a name="next-steps"></a>Volgende stappen

- [Blob-opslag voor IIS gebruik en de tabelopslag voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) voor het lezen van de logboeken voor Azure services die schrijven diagnostische gegevens voor de tabelopslag of de IIS-logboeken geschreven naar een blob-opslag.
- [Inschakelen oplossingen](log-analytics-add-solutions.md) bieden inzicht in de gegevens.
- [Zoekopdrachten gebruiken](log-analytics-log-searches.md) om de gegevens te analyseren.
