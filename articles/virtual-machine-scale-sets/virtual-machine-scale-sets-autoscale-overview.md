<properties
    pageTitle="Automatische schaling en virtuele machine schalen sets | Microsoft Azure"
    description="Informatie over het automatisch schalen van virtuele machines in een schaal met diagnostische gegevens en bronnen automatisch schalen."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automatische schaling en virtuele machine schalen sets

Automatische schaling van virtuele machines in een schaal, is het maken of verwijderen van machines in de set om te voldoen aan de prestatie-eisen. Als het volume van het werk groeit, mogelijk een toepassing extra bronnen te kunnen taken effectief uit te voeren.

Automatisch schalen is een geautomatiseerd proces gemakkelijk overheadkosten voor beheer. Overhead verminderd, hoeft u niet voortdurend prestaties van het systeem controleren of het beheren van bronnen bepalen. Schaal is een elastisch proces. Meer bronnen kunnen worden toegevoegd als de belasting wordt verhoogd, maar als vraag afneemt resources kosten minimaliseren en onderhouden van de prestaties kunnen worden verwijderd.

Automatische schaling op een schaal instellen met behulp van een sjabloon Azure Resource Manager, Azure PowerShell, Azure CLI of de Azure portal instellen.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Een schaal met behulp van bronbeheer sjablonen instellen

In plaats van een sjabloon die alle bronnen in een enkele, gecoördineerde werking implementeren en beheren van elke bron van uw toepassing afzonderlijk te gebruiken. Toepassingsbronnen zijn gedefinieerd in de sjabloon en implementatie parameters worden opgegeven voor verschillende omgevingen. De sjabloon bestaat uit JSON en expressies waarmee u waarden voor de implementatie samenstellen. Kijk voor meer informatie, [sjablonen ontwerpen Azure Resource Manager](../resource-group-authoring-templates.md).

In de sjabloon, kunt u het element capaciteit opgeven:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Capaciteit geeft het aantal virtuele machines in de set. U kunt de capaciteit handmatig wijzigen door het implementeren van een sjabloon met een andere waarde. Als u een sjabloon wijzigt alleen de capaciteit implementeert, kunt u alleen de SKU-element met de bijgewerkte inhoud opnemen.

De capaciteit van de schaal instellen met behulp van de combinatie van de bron autoscaleSettings en de uitbreiding van de diagnostische gegevens automatisch wijzigen.

### <a name="configure-the-azure-diagnostics-extension"></a>De extensie Azure diagnostische gegevens configureren

Automatisch schalen is alleen mogelijk als de parameters-collectie is voltooid op elke virtuele machine in de schaal. De uitbreiding van de diagnostische gegevens van Azure biedt controle en diagnose mogelijkheden die voldoet aan de behoeften van de collectie Parameters van de bron automatisch schalen. U kunt de extensie installeren als onderdeel van de sjabloon Resource Manager.

In dit voorbeeld worden de variabelen weergegeven die worden gebruikt in de sjabloon voor het configureren van de uitbreiding van de diagnostische gegevens:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parameters worden verstrekt wanneer de sjabloon wordt geïmplementeerd. In dit voorbeeld wordt worden de naam van de account van de opslag waar gegevens worden opgeslagen en de naam van de schaal waarop gegevens worden verzameld geleverd. In dit voorbeeld Windows Server, alleen het aantal threads prestatiemeteritem ook verzameld. De beschikbare prestatiemeteritems in Windows of Linux kunnen worden gebruikt voor het verzamelen van diagnostische informatie en kunnen worden opgenomen in de configuratie van de uitbreiding.

In dit voorbeeld wordt de definitie van de uitbreiding in de sjabloon:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Wanneer de uitbreiding van de diagnostische gegevens wordt uitgevoerd, worden de gegevens verzameld in een tabel die zich bevindt in de opslag-account die u opgeeft. U kunt de verzamelde gegevens vinden in de tabel WADPerformanceCounters:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>De bron van de autoScaleSettings configureren

De bron van de autoscaleSettings gebruikt de informatie uit de uitbreiding van de diagnostische gegevens om te beslissen of u wilt vergroten of verkleinen van het aantal virtuele machines in de schaal.

In dit voorbeeld ziet u de configuratie van de resource in de sjabloon:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

In het bovenstaande voorbeeld worden twee regels gemaakt voor de automatische schaling acties te definiëren. De eerste regel definieert de schalen actie en de tweede regel definieert de schaal in actie. Deze waarden zijn beschikbaar in de regels:

- **metricName** - deze waarde is hetzelfde als de prestatiemeteritems die u hebt gedefinieerd in de variabele wadperfcounter voor de uitbreiding van de diagnostische gegevens. In het bovenstaande voorbeeld wordt de teller voor het aantal threads gebruikt.  
- **metricResourceUri** - deze waarde is de bron-id van de virtuele machine schaal set. Deze id bevat de naam van de resourcegroep, de naam van de provider van de resource en de naam van de schaal instelt om te schalen.
- **timeGrain** : deze waarde is de granulatie van de metrische gegevens die worden verzameld. In het voorgaande voorbeeld worden gegevens verzameld op een interval van één minuut. Deze waarde wordt gebruikt met de waarde voor timeWindow.
- **Statistieken** – met deze waarde bepaalt hoe de cijfers voor de automatische schaling actie worden gecombineerd. De mogelijke waarden zijn: gemiddelde, Min, Max.
- **waarde voor timeWindow** : deze waarde is de tijd waarin gegevens worden verzameld. Deze moet tussen 5 minuten en 12 uur.
- **TimeAggregation van** – deze waarde bepaalt hoe de gegevens die worden verzameld over de tijd moeten worden gecombineerd. De standaardwaarde is de gemiddelde. De mogelijke waarden zijn: gemiddelde, Minimum, Maximum, laatste, totaal, telling.
- **operator** : deze waarde is de operator die wordt gebruikt voor het vergelijken van de metrische gegevens en de drempel. De mogelijke waarden zijn: is gelijk aan NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **drempel** – dit is de waarde die de schaal actie activeert. Zorg voor een voldoende verschil tussen de drempel voor de actie van de schalen en de drempel voor de schaal in actie. Als u de waarden hetzelfde te zijn, verwacht het systeem constant wijzigen, waardoor uitvoering van een actie van de schaal. Bijvoorbeeld werkt instellen op 600 threads in het vorige voorbeeld niet.
- **richting** : deze waarde bepaalt de actie die wordt uitgevoerd wanneer de drempelwaarde wordt bereikt. De mogelijke waarden zijn verhoging of verlaging.
- **type** : deze waarde is het soort actie dat moet worden uitgevoerd en moet worden ingesteld op ChangeCount.
- **waarde** : deze waarde is het aantal virtuele machines die worden toegevoegd aan of verwijderd uit de set schaal. Deze waarde moet 1 of hoger.
- **cooldown** : deze waarde is de tijdsduur sinds de laatste actie schaal wachten voordat de volgende actie plaatsvindt. Deze waarde moet liggen tussen één minuut en één week.

Afhankelijk van de prestatiemeteritems die u gebruikt, worden bepaalde elementen in de sjabloonconfiguratie anders gebruikt. In het voorgaande voorbeeld de Prestatiemeter aantal threads is de drempelwaarde is 650 voor een actie van de schalen en de drempelwaarde is 550 voor de schaal in actie. Als u een item zoals % processortijd, is de drempelwaarde ingesteld op het percentage CPU-gebruik die een actie van de schaal bepaalt.

Wanneer een belasting wordt gemaakt op de virtuele machines die een schaal actie activeert, is de capaciteit van de set wordt verhoogd op basis van de waarde in de sjabloon. In een schaal instellen waarbij de capaciteit is ingesteld op 3 en de actie-schaalwaarde ingesteld op 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Wanneer de belasting gemaakt die ervoor zorgt het gemiddelde aantal threads dat gaan boven de drempel van 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Een scale-out gestart die ervoor zorgt dat de capaciteit van de set met één wordt verhoogd:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

En een virtuele machine wordt toegevoegd aan de set schaal:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Na een cooldown van 5 minuten, het gemiddelde aantal threads op de computers meer dan 600 blijft een andere machine toegevoegd aan de set. Het gemiddelde aantal threads onder 550 blijft, de capaciteit van de set schaal wordt verminderd met een als een computer wordt verwijderd uit de set.

## <a name="set-up-scaling-using-azure-powershell"></a>Schalen met Azure PowerShell instellen

Ga voor voorbeelden van het gebruik van PowerShell autoscaling instellen Bekijk [Azure Monitor PowerShell snelle start monsters](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Schalen met Azure CLI instellen

Ga voor voorbeelden van het gebruik van Azure CLI autoscaling instellen Bekijk [Azure Monitor platforms CLI snelle start monsters](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Schalen met de Azure portal instellen

U ziet een voorbeeld van het gebruik van de portal Azure autoscaling instellen, Bekijk [maken een virtuele Machine schaal instellen met behulp van de portal Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Schaal acties onderzoeken

- [Azure portal]() - op dit moment kun je een beperkte hoeveelheid gegevens die via de portal.
- [Azure Resource Explorer]() - dit programma is het meest geschikt voor het verkennen van de huidige status van uw set schaal. Volg dit pad en ziet u de weergave van het exemplaar van de schaal set die u hebt gemaakt: abonnementen > {uw abonnement} > resourceGroups > {de resourcegroep} > providers > Microsoft.Compute > virtualMachineScaleSets > {uw set schaal} > virtualMachines
- Azure PowerShell - Gebruik deze opdracht om bepaalde informatie te verkrijgen:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Verbinding maken met de virtuele machine van jumpbox net zoals u zou doen met een andere computer en klikt u op afstand toegang de virtuele machines in de schaal instellen tot voor afzonderlijke processen.

## <a name="next-steps"></a>Volgende stappen

- Bekijk [automatisch schalen machines in een virtuele Machine schaal ingesteld](virtual-machine-scale-sets-windows-autoscale.md) voor een voorbeeld van het maken van een schaal instelt met automatisch schalen is geconfigureerd.
- Voorbeelden van controlefuncties in [Azure Monitor PowerShell snelle start monsters](../monitoring-and-diagnostics/insights-powershell-samples.md) Azure-Monitor
- Informatie over de voorzieningen voor kennisgeving in [Acties voor het verzenden van e-mail en webhook meldingen in Azure Monitor automatisch schalen gebruiken](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Meer informatie over het [Gebruik controle meldt zich bij het verzenden van e-mail en webhook meldingen in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Meer informatie over [Geavanceerde automatisch schalen scenario's](./virtual-machine-scale-sets-advanced-autoscale.md).
