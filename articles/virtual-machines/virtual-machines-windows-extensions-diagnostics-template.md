<properties
    pageTitle="Maak een virtuele Windows-machine met bewaking en diagnostiek met Azure Resource Manager sjabloon | Microsoft Azure"
    description="Een sjabloon Azure resource manager gebruiken voor het maken van een nieuwe virtuele machine voor Windows Azure diagnostics extensie."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Een virtuele Windows-machine met bewaking en diagnostiek met Azure Resource Manager-sjabloon maken

De uitbreiding van de diagnostische gegevens van Azure biedt de bewaking en diagnostische mogelijkheden op een Windows Azure virtuele machine gebaseerd. Door de uitbreiding als onderdeel van de sjabloon azure resource manager kunt u deze mogelijkheden op de virtuele machine. [Azure Resource Manager sjablonen ontwerpen met de VM-extensies](virtual-machines-windows-extensions-authoring-templates.md) Zie voor meer informatie over het toevoegen van een extensie als onderdeel van de sjabloon voor een virtuele machine. In dit artikel wordt beschreven hoe u de extensie Azure diagnostische gegevens kunt toevoegen aan een sjabloon windows virtuele machine.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>De uitbreiding Azure diagnostische gegevens voor de definitie van VM resource toevoegen 

Als de extensie van de diagnostische gegevens op een virtuele Windows-computer wilt inschakelen, moet u de extensie toevoegen als een VM-bron in de sjabloon Resource manager.

Voor een eenvoudige Resource Manager gebaseerde virtuele Machine de uitbreiding configuratie toevoegen aan de matrix van *resources* voor de virtuele Machine: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Een andere algemene conventie is de uitbreiding configuratie toevoegen op het hoofdknooppunt van de bronnen van de sjabloon in plaats van het onder het knooppunt van de virtuele machine resources definiëren. Met deze aanpak hebt u expliciet een hiërarchische relatie tussen de uitbreiding en de virtuele machine met de *naam* en het *type* waarden opgeven. Bijvoorbeeld: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

De extensie is altijd gekoppeld aan de virtuele machine, kunt u rechtstreeks rechtstreeks onder het knooppunt van de virtuele machine resource definiëren of op het niveau van base definiëren en de hiërarchische naamgevingsconventie gebruiken om te koppelen aan de virtuele machine.

De configuratie van de extensies voor virtuele Machine schaal Sets is opgegeven in de eigenschap *extensionProfile* van de *VirtualMachineProfile*.
   
Identificeert de extensie Azure diagnostische gegevens van de *publisher* -eigenschap met de waarde van **Microsoft.Azure.Diagnostics** en de eigenschap *type* met de waarde van **IaaSDiagnostics** .

De waarde van de eigenschap *name* kan worden gebruikt om te verwijzen naar de extensie in de resourcegroep. Instelt op **Microsoft.Insights.VMDiagnosticsSettings** , wordt deze kan gemakkelijk worden geïdentificeerd door de Azure klassieke portal portal ervoor te zorgen dat de controle grafieken correct in de klassieke Azure portal uitgeschakeld.

De *typeHandlerVersion* geeft de versie van de extensie die u wilt gebruiken. *AutoUpgradeMinorVersion* als secundaire versie op **true** zorgt ervoor dat u wordt de meest recente secundaire versie van de extensie die beschikbaar is. Het wordt nadrukkelijk aanbevolen dat *autoUpgradeMinorVersion* altijd om **waar** te zijn zodat u altijd de meest recente uitbreiding van de beschikbare diagnostische gegevens gebruiken met de nieuwe functies en bugfixes altijd ingesteld. 

De *Instellingen* -element bevat configuratie-eigenschappen voor de extensie die u kunt instellen en lezen van de extensie (ook wel openbare configuratie genoemd). De eigenschap *xmlcfg* bevat de XML-gebaseerde configuratie voor de diagnostische logboeken, prestatiemeteritems enz die worden verzameld door de agent diagnostiek. Zie [Schema van diagnostische gegevens configureren](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor meer informatie over het XML-schema zelf. Een veelvoorkomende truc is het werkelijke XML-configuratie opslaan als een variabele in de sjabloon Azure Resource Manager en vervolgens samenvoegen en base64 coderen stelt de waarde voor *xmlcfg*. Zie de sectie over [variabelen diagnostische gegevens](#diagnostics-configuration-variables) voor meer informatie over de XML-gegevens opslaan in variabelen. De eigenschap *storageAccount* geeft de naam van de account van de opslag die diagnostische gegevens worden overgebracht. 
 
De eigenschappen in de *protectedSettings* (ook wel persoonlijke configuratie genoemd) kunnen instellen, maar kunnen niet worden gelezen terug na wordt ingesteld. De aard van het alleen-schrijven van *protectedSettings* is het handig voor het opslaan van geheimen, net als de sleutel opslag account waarin de diagnostische gegevens worden geschreven.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Diagnostische gegevens opslag account opgeven als parameters 

In het bovenstaande van diagnostische extensie json fragment neemt twee parameters, *existingdiagnosticsStorageAccountName* en *existingdiagnosticsStorageResourceGroup* geeft u de diagnostische gegevens opslag account diagnostische gegevens worden opgeslagen. De diagnostische gegevens opslag account opgeven als een parameter het maakt gemakkelijk de diagnostische gegevens opslag account wijzigen in verschillende omgevingen bv. u kunt een account te gebruiken verschillende diagnostische gegevens opslag voor het testen en een voor de productie-implementatie.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Het is raadzaam een diagnostische gegevens opslag account opgeven in een andere bronnengroep bevinden dan de bronnengroep voor de virtuele machine. Een groep kan worden beschouwd als een implementatie-eenheid met een eigen levensduur, een virtuele machine kan worden geïmplementeerd en ingezet als nieuwe configuraties wordt bijgewerkt dat deze maar u wilt doorgaan met diagnostische gegevens op te slaan in dezelfde opslag account via deze virtuele machine-implementaties. Met de account is opslag in een andere resource, kunt de opslag account om gegevens uit verschillende virtuele machine implementaties, zodat u gemakkelijk problemen in de verschillende versies te accepteren.

>[AZURE.NOTE] Als u een sjabloon windows virtuele machine van Visual Studio maken kan de standaardaccount opslag worden ingesteld op opslag dezelfde account gebruiken, waarbij de virtuele machine VHD is geüpload. Dit is de initiële instellingen van de VM te vereenvoudigen. U moet de sjabloon voor het gebruik van een verschillende opslag-account die kan worden doorgegeven als een parameter opnieuw factor. 

## <a name="diagnostics-configuration-variables"></a>Diagnostische gegevens van de variabelen
 
In het bovenstaande van diagnostische extensie json fragment definieert een variabele *accountid* ter vereenvoudiging van het ophalen van de sleutel opslag account voor de opslag van diagnostische gegevens:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


De eigenschap *xmlcfg* voor de uitbreiding van de diagnostische gegevens wordt gedefinieerd met behulp van meerdere variabelen die worden samengevoegd. De waarden van deze variabelen zijn in xml, zodat ze correct worden overgeslagen tijdens het instellen van de variabelen json nodig.

De volgende tabel ziet de diagnostische configuratie xml die standaard niveau systeemprestatiemeteritems en sommige windows-gebeurtenislogboeken en diagnostische infrastructuur logboeken worden verzameld. Voorafgegaan is en correct opgemaakt zodat de configuratie in de sectie variabelen van de sjabloon rechtstreeks kan worden geplakt. Zie het [Schema van diagnostische gegevens configureren](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor een meer menselijke leesbare voorbeeld van de configuratie van xml.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Het maatstelsel definitie XML-knooppunt in de bovenstaande configuratie is een belangrijke configuratie-element wordt gedefinieerd hoe de prestatiemeteritems die eerder in het XML-knooppunt *PerformanceCounter* gedefinieerd worden samengevoegd en opgeslagen. 

> [AZURE.IMPORTANT] Deze statistieken station de grafieken en waarschuwingen in de portal Azure.  Het knooppunt **Statistieken** met *resourceID* en **MetricAggregation** moet worden opgenomen in de configuratie van diagnostische voor uw VM als u wilt zien van de meetgegevens VM in Azure portal. 

Hier volgt een voorbeeld van de xml voor definities voor statistieken: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Het kenmerk *resourceID* is een unieke aanduiding voor de virtuele machine op uw abonnement. Controleer of u de functies subscription() en resourceGroup() zodat de sjabloon automatisch bijgewerkt met deze waarden op basis van het abonnement en de resourcegroep die u levert.

Als u meerdere virtuele Machines in een lus maakt hebt u de waarde *resourceID* met een copyIndex()-functie om elke afzonderlijke VM correct onderscheid te vullen. De waarde van de *xmlCfg* kan worden bijgewerkt voor de ondersteuning van dit als volgt:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

De waarde van de MetricAggregation van *PT1H* en *PT1M* geven aan een samenvoeging in een minuut en een samenvoeging van een uur.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics tabellen in opslag

De bovenstaande cijfers configuratie genereert tabellen in uw account diagnostische gegevens opslag met de volgende naamgeving:

- **WADMetrics** : standaardvoorvoegsel voor alle tabellen van de WADMetrics
- **PT1H** of **PT1M** : geeft aan dat de tabel statistische gegevens meer dan 1 uur of 1 minuut bevat
- **P10D** : geeft aan dat de tabel gegevens bevat voor 10 dagen waarop de tabel begonnen met het verzamelen van gegevens
- **V2S** : tekenreeksconstante
- **JJJJMMDD** : de datum waarop de tabel begonnen met het verzamelen van gegevens

Voorbeeld: *WADMetricsPT1HP10DV2S20151108* bevat metrische gegevens samengevoegd in een uur voor 10 dagen vanaf 11-Nov-2015    

Elke tabel WADMetrics bevat de volgende kolommen:

- **PartitionKey**: het partitionkey is samengesteld op basis van de waarde van *resourceID* ter identificatie van de VM-bron. voor bv.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : de indeling volgt <Descending time tick>:<Performance Counter Name>. De berekening van de aflopende tick is max tijd tikken min de tijd van het begin van de periode van de samenvoeging. Bv. Als de bemonsteringsperiode gestart op 10-Nov-2015 UTC 00:00Hrs en vervolgens de berekening: DateTime.MaxValue.Ticks - (nieuwe DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Tikken). Voor het geheugen beschikbare bytes Prestatiemeter de rijsleutel eruit zal zien, zoals: 2519551871999999999__:005CMemory:005CAvailable:0020 Bytes
- **CounterName** : Is de naam van de Prestatiemeter. Dit komt overeen met de *counterSpecifier* die is gedefinieerd in het XML-configuratie.
- **Maximum** : de maximale waarde van het prestatiemeteritem gedurende de periode van de samenvoeging.
- **Minimum** : de laagste waarde van het prestatiemeteritem gedurende de periode van de samenvoeging.
- **Totaal** : de som van alle waarden van het prestatiemeteritem gerapporteerd over de periode van de samenvoeging.
- **Aantal** : het totale aantal waarden aangegeven voor de Prestatiemeter.
- **Gemiddeld** : het gemiddelde (totaal/aantal) waarde van het prestatiemeteritem gedurende de aggregatie.


## <a name="next-steps"></a>Volgende stappen

- Zie voor een volledige voorbeeldsjabloon van een virtuele Windows-computer met de extensie diagnostics [201-vm-controle-diagnose-extensie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- De resource manager-sjabloon met [Azure PowerShell](virtual-machines-windows-ps-manage.md) of een [opdrachtregel Azure](virtual-machines-linux-cli-deploy-templates.md) implementeren
- Meer informatie over [sjablonen voor schrijven Azure Resource Manager](../resource-group-authoring-templates.md)







