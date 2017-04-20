<properties
    pageTitle="Verticaal schalen Azure VM schaal sets | Microsoft Azure"
    description="Het verticaal schalen van een virtuele Machine in reactie op de monitoring van waarschuwingen met Azure automatisering"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Verticale automatisch schalen met virtuele Machine schaal instellen

In dit artikel wordt beschreven hoe verticaal schalen Azure [Virtual Machine schaal Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) met of zonder reprovisioning. Raadpleeg [verticaal schalen Azure virtuele machine met Azure automatisering](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md)voor verticale schaling van VMs die niet in de schaal wordt ingesteld.

Verticaal schalen, ook wel _vergroten_ en _verkleinen_, betekent het vergroten of verkleinen van de virtuele machine (VM) formaten in reactie op een werkbelasting. Vergelijk dit met [Horizontaal schalen](./virtual-machine-scale-sets-autoscale-overview.md), ook wel genoemd _geschaald uitbreiden_ en de _schaal in_, waarbij het aantal VMs afhankelijk van de werkbelasting wordt gewijzigd.

Reprovisioning betekent een bestaande VM verwijderen en vervangen door een nieuwe. Wanneer u vergroten of van VMs in een VM schaal ingesteld verkleinen, in sommige gevallen wilt u het formaat van de bestaande VMs en bewaren van gegevens, terwijl in andere gevallen u moet voor de implementatie van nieuwe VMs van het nieuwe formaat. Beide gevallen worden beschreven.

Verticaal schalen is handig wanneer:

- Een service die is gebaseerd op virtuele machines is onder gebruikt (bijvoorbeeld in het weekend). Het verkleinen van VM kunt maandelijkse kosten verminderen.
- VM vergroten om met een grotere vraag zonder dat er extra VMs.

U kunt instellen verticaal schalen worden geactiveerd op basis van de metrische waarschuwingen op basis van uw VM schaal instellen. Wanneer de waarschuwing wordt geactiveerd deze gebeurtenis wordt gestart een webhook dat een runbook die u kunt de schaal schaal instellen omhoog of omlaag triggers. Verticaal schalen kan worden geconfigureerd door de volgende stappen:

1. Maak een account Azure automatisering met uitvoeren als mogelijkheid.
2. Azure automatisering verticale schaal runbooks voor VM schaal Sets importeren in uw abonnement.
3. Een webhook toevoegen aan uw runbook.
4. Een waarschuwing toevoegen aan uw VM schaal instellen met een melding van webhook.

> [AZURE.NOTE] Verticale autoscaling kan alleen plaatsvinden binnen bepaalde grenzen van VM formaten. De specificaties van elke omvang te vergelijken voordat u besluit te schalen van het ene naar het andere (hoger getal geeft niet altijd aan grotere VM). U kunt schalen tussen de volgende formaten:

>| VM-formaten paar schalen |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Een Azure automatisering-Account maken met mogelijkheid voor uitvoeren als

Het eerste wat dat u moet doen is een Azure automatisering account maken die is van de runbooks gebruikt host voor het schalen van de exemplaren VM schaal ingesteld. [Azure automatisering](https://azure.microsoft.com/services/automation/) introduceerde onlangs de 'Uitvoeren als'-functie waardoor de instelling van de Service Principal voor het automatisch uitvoeren van de runbooks van een gebruiker namens heel eenvoudig. U kunt meer lezen over dit in het artikel hieronder:

* [Runbooks met Azure uitvoeren als-account verifiëren](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Azure automatisering verticale schaal runbooks importeren in uw abonnement

De runbooks die nodig zijn voor het verticaal schalen van uw VM schaal Sets zijn al gepubliceerd in de galerie met Azure automatisering Runbook. Als u wilt importeren stappen in uw abonnement op de in dit artikel:

* [Runbook en module galerijen voor Azure automatisering](../automation/automation-runbook-gallery.md)

Kies de optie Galerij bladeren in het menu Runbooks:

![Runbooks moeten worden geïmporteerd][runbooks]

De runbooks die moeten worden geïmporteerd, worden weergegeven. De runbook op basis van of u verticale schaling met of zonder reprovisioning wilt selecteren:

![Galerie Runbooks][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook

Zodra u hebt geïmporteerd toevoegen de runbooks u moet een webhook aan de runbook, zodat deze kan worden geactiveerd door een waarschuwing van een VM schaal ingesteld. De details van het maken van een webhook voor de Runbook worden in dit artikel beschreven:

* [Azure automatisering webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Zorg ervoor dat u de webhook URI kopiëren voordat u het dialoogvenster webhook te sluiten, want u dit in de volgende sectie moet.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Een waarschuwing toevoegen aan uw VM schaal instellen

Hieronder vindt u een PowerShell script ziet u hoe u een melding wilt toevoegen aan een VM schaal ingesteld. Raadpleeg het volgende artikel in de naam van de metric de waarschuwing op te starten: [Azure Monitor autoscaling gemeenschappelijke maatstaven](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Het is raadzaam een redelijke termijn de waarschuwing venster configureren om te voorkomen dat die verticaal schalen en alle bijbehorende service wordt onderbroken, maar al te vaak. U kunt een venster van minimaal 20-30 minuten of meer. U kunt horizontaal schalen als u wilt vermijden.

Raadpleeg de volgende artikelen voor meer informatie over het maken van waarschuwingen:

* [Azure PowerShell Monitor quick start-voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure Monitor platforms CLI quick start-voorbeelden](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Samenvatting

Dit artikel werd eenvoudige voorbeelden van verticaal schalen. Met deze bouwstenen - automatisering-account, runbooks, webhooks, waarschuwingen - kunt u een rijk scala aan gebeurtenissen met een aangepaste reeks acties.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
