<properties
    pageTitle="Gebruikt u PowerShell maken en configureren van een werkruimte logboek Analytics | Microsoft Azure"
    description="Meld u Analytics gebruikt gegevens van servers in uw locatie op of cloud-infrastructuur. U kunt computergegevens van Azure opslag wanneer gegenereerd door Azure diagnostische gegevens verzamelen."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Logboek Analytics met PowerShell beheren

U kunt [Log Analytics PowerShell cmdlets] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) voor diverse functies in logboek Analytics uitvoeren vanaf een opdrachtregel of als onderdeel van een script.  Voorbeelden van taken die u met PowerShell uitvoeren kunt zijn:

+ Een werkruimte maken
+ Toevoegen of verwijderen van een oplossing
+ Importeren en exporteren van opgeslagen zoekopdrachten
+ Maak een computergroep
+ Verzameling van IIS-logboeken van computers met Windows geïnstalleerd agent inschakelen
+ Het verzamelen van prestatiemeteritems van Linux en Windows-computers
+ Gebeurtenissen verzamelen van syslog op Linux computers 
+ Gebeurtenissen verzamelen van de Windows-gebeurtenislogboeken
+ Aangepaste gebeurtenislogboeken verzamelen
+ De agent logboek analytics toevoegen aan een Azure virtuele machine
+ Analytics logboek om gegevens te indexeren verzameld met Azure diagnostische gegevens configureren


Dit artikel bevat twee codevoorbeelden waarin enkele van de functies die u vanuit PowerShell uitvoeren kunt illustreren.  U kunt verwijzen naar de [Log Analytics PowerShell cmdlet referentie] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) voor andere functies.

> [AZURE.NOTE] Logboek Analytics werd voorheen operationele inzichten die daarom is de naam die in de cmdlets genoemd.

## <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken in uw werkruimte logboek Analytics PowerShell, moet u hebben:

+ Een abonnement op Azure en 
+ Uw werkruimte Azure logboek Analytics gekoppeld aan uw abonnement op Azure.

Als u een OMS-werkruimte hebt gemaakt, maar het is niet nog hebt gekoppeld voor een Azure abonnement kunt u de koppeling:

+ In de portal voor Azure
+ In de portal OMS of 
+ De Get-AzureRmOperationalInsightsLinkTargets en een nieuw AzureRmOperationalInsightsWorkspace-cmdlets gebruiken.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Maken en configureren van een logboek Analytics werkruimte

Het volgende script voorbeeld ziet u hoe:

1.  Een werkruimte maken
2.  Lijst met beschikbare oplossingen
3.  Oplossingen aan de werkruimte toevoegen.
4.  Importeren van opgeslagen zoekopdrachten
5.  Export opgeslagen zoekopdrachten
6.  Maak een computergroep
7.  Verzameling van IIS-logboeken van computers met Windows geïnstalleerd agent inschakelen
8.  Verzamelen van prestatiemeteritems voor logische schijf van Linux-computers (% Inodes gebruikt; Gratis MB; % Gebruikte ruimte; Schijf-overdrachten per seconde; Schijf lezen per seconde; Schijf schrijven per seconde)
9.  Syslog-gebeurtenissen verzamelen van Linux-computers
10. Fout en waarschuwing gebeurtenissen verzamelen in het toepassingslogboek van Windows-computers
11. Prestatiemeteritems voor geheugen beschikbare Mbytes verzamelen van Windows-computers
12. Een aangepaste logboek verzamelen 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Logboek Analytics indexeren Azure diagnostische gegevens configureren 

Voor agentless toezicht op Azure resources, moeten de middelen Azure diagnostics ingeschakeld en geconfigureerd voor het schrijven naar een opslag. Logboek Analytics kunt wordt geconfigureerd voor het verzamelen van de logboeken van de opslag-account. Bronnen die u moet doen van de hiervoor beschreven configuratie voor omvat:

+ Klassieke cloud services (web- en werknemer rollen)
+ Service fabric-clusters
+ Netwerk-beveiligingsgroepen
+ Sleutel kluizen en 
+ Toepassingsgateways

U kunt ook PowerShell een logboek Analytics werkruimte configureren in één Azure abonnement voor het verzamelen van Logboeken van andere Azure abonnementen.

Het volgende voorbeeld wordt getoond hoe:

1.  De bestaande accounts voor opslag en locaties die logboek Analytics indexeert gegevens uit
2.  Een configuratie te lezen van een opslag-account maken
3.  De nieuwe configuratie om gegevens te indexeren bijwerken vanaf andere locaties
4.  De nieuwe configuratie verwijderen

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Volgende stappen

- [Revisie Log Analytics PowerShell-cmdlets](http://msdn.microsoft.com/library/mt188224.aspx) voor meer informatie over het gebruik van PowerShell voor de configuratie van logboek Analytics.

