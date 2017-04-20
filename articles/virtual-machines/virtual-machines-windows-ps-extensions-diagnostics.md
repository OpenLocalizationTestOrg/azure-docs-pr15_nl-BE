<properties
    pageTitle="PowerShell gebruiken om in een virtuele machine met Windows Azure Diagnostics | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Informatie over het gebruik van PowerShell Azure Diagnostics inschakelen in een virtuele machine met Windows"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>PowerShell gebruiken om in een virtuele machine met Windows Azure diagnostische gegevens

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure diagnostische gegevens bestaat de mogelijkheid in Azure waarmee het verzamelen van diagnostische gegevens op een gedistribueerde toepassing. U kunt de uitbreiding van de diagnostische gegevens verzamelen van diagnostische gegevens, zoals de toepassingslogboeken of prestatiemeteritems van een Azure VM (virtual machine) met Windows. In dit artikel wordt beschreven hoe u Windows PowerShell gebruiken om te schakelen van de uitbreiding van de diagnostische gegevens voor een VM. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) op de vereisten die nodig zijn voor dit artikel.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>De diagnose-extensie inschakelen als u het implementatiemodel Resource Manager

Tijdens het maken van een Windows VM via het implementatiemodel Azure Resource Manager door de uitbreiding configuratie toe te voegen aan de sjabloon Resource Manager kunt u de extensie diagnostics inschakelen. Zie [een virtuele Windows-computer met bewaking en diagnostiek met behulp van de sjabloon Azure Resource Manager maken](virtual-machines-windows-extensions-diagnostics-template.md).

Als u wilt dat de uitbreiding van de diagnostische gegevens op een bestaande VM die is gemaakt via het implementatiemodel Resource Manager, kunt u de cmdlet [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShell zoals hieronder wordt weergegeven.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* is het pad naar het bestand met de configuratie van de diagnostische gegevens in XML, zoals beschreven in het [voorbeeld](#sample-diagnostics-configuration) hieronder.  

Als het configuratiebestand van de diagnostische gegevens van een **StorageAccount** -element met de naam van een opslag, wordt in het *Set-AzureRMVMDiagnosticsExtension* script automatisch de extensie diagnostics diagnostische gegevens verzenden naar opslag account ingesteld. Dit werkt, moet de account voor de opslag van het abonnement op dezelfde als de VM.

Als er geen **StorageAccount** is opgegeven in de configuratie van diagnostische gegevens, moet u in de *StorageAccountName* -parameter doorgeven aan de cmdlet. Als de *StorageAccountName* -parameter is opgegeven, wordt de cmdlet altijd gebruikt de opslag-account die is opgegeven in de parameter en niet die is opgegeven in het configuratiebestand van de diagnostische gegevens.

Als de diagnostische gegevens opslag account zich in een ander abonnement van de VM, moet u expliciet in de *StorageAccountName* en *StorageAccountKey* parameters doorgeven aan de cmdlet. De parameter *StorageAccountKey* is niet nodig als de diagnostische gegevens opslag account in het hetzelfde abonnement als de cmdlet automatisch kunt opvragen en de waarde van de sleutel instellen om de uitbreiding van de diagnostische gegevens. Echter, als de diagnostische gegevens opslag account in een ander abonnement, vervolgens de cmdlet mogelijk niet automatisch ophalen van de sleutel en moet u de sleutel tot en met de parameter *StorageAccountKey* expliciet op te geven.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Zodra de diagnose-extensie is ingeschakeld op een VM, kunt u de huidige instellingen met behulp van de cmdlet [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) .

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

De cmdlet retourneert *PublicSettings*, waarin de XML-configuratie in een indeling met Base64-codering. Het XML-bestand worden gelezen, moet u dit decoderen.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

De cmdlet [Verwijderen AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) kan worden gebruikt voor de uitbreiding van de diagnostische gegevens verwijderen uit de VM.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>De diagnose-extensie inschakelen als u het implementatiemodel klassiek

U kunt de cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) in een VM die u door het implementatiemodel klassiek maakt een diagnose-extensie inschakelen. In het volgende voorbeeld ziet u hoe een nieuwe VM via de klassieke implementatiemodel maken met de extensie diagnostics is ingeschakeld.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Als u wilt dat de uitbreiding van de diagnostische gegevens op een bestaande VM die is gemaakt via het implementatiemodel klassiek, de cmdlet [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) eerst te gebruiken om de VM-configuratie. Werk vervolgens de VM-configuratie om de uitbreiding van de diagnostische gegevens opnemen met behulp van de cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) . Ten slotte de bijgewerkte configuratie voor VM toepassen met behulp van [Update AzureVM](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Configuratie van diagnostische monster

De volgende XML kan worden gebruikt voor de diagnostische gegevens van openbare configuratie met behulp van de bovenstaande scripts. Deze configuratie monster, verschillende prestatiemeteritems voor de diagnostische gegevens opslag rekening, en fouten in de toepassing, beveiliging en systeemkanalen in de gebeurtenislogboeken van Windows en eventuele fouten uit de infrastructuur diagnostische logboeken worden overgebracht.

De configuratie moet worden bijgewerkt met het volgende:

- De *resourceID* -kenmerk van het element **maatstaven** moet worden bijgewerkt met de resource-ID voor de VM.
    - De resource-ID kan worden samengesteld volgens het volgende patroon: "/ abonnementen / {*abonnements-ID voor het abonnement met de VM*} /resourceGroups/ {*de naam resourcegroup voor VM*} / {*de naam VM*} providers/Microsoft.Compute/virtualMachines/".
    - Bijvoorbeeld, als de abonnement-ID voor het abonnement waar de VM met **11111111-1111-1111-1111-111111111111**, de naam van de resource voor de resourcegroep **MyResourceGroup**en de naam van de VM **MyWindowsVM**is, zou moet de waarde voor *resourceID* zijn:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Voor meer informatie over hoe u statistieken worden gegenereerd op basis van de prestaties tellers en statistieken configuratie, Zie [Diagnostische gegevens Azure metrics tabel in opslag](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- Het element **StorageAccount** moet worden bijgewerkt met de naam van de rekening van diagnostische gegevens opslag.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer advies over het gebruik van de Azure diagnosemogelijkheden en andere technieken voor het oplossen van problemen, [Diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Schema voor diagnostiek configuraties](https://msdn.microsoft.com/library/azure/mt634524.aspx) worden beschreven de verschillende XML-configuraties voor de uitbreiding van de diagnostische gegevens.
