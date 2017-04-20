<properties 
   pageTitle="Azure automatisering Runbook typen"
   description="Beschrijving van de verschillende soorten runbooks die u in Azure automatisering en overwegingen waarmee u rekening houden gebruiken kunt moet bij het bepalen van welk type u. "
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Azure automatisering runbook typen

Azure automatisering ondersteunt vier typen runbooks die kort worden beschreven in de volgende tabel.  In de volgende secties bevatten meer informatie over elk type met inbegrip van overwegingen op welk.


| Type |  Beschrijving |
|:---|:---|
| [Grafische](#graphical-runbooks) | Op basis van Windows PowerShell en grafische editor gemaakt en volledig in bewerkte in Azure portal. | 
| [Grafische Workflow voor PowerShell](#graphical-runbooks) | Op basis van Windows PowerShell Workflow en gemaakt en bewerkt volledig in de grafische editor in Azure portal. 
| [PowerShell](#powershell-runbooks) | Tekst runbook op basis van de Windows PowerShell-script.
| [PowerShell Workflow](#powershell-workflow-runbooks) | Tekst runbook op basis van Windows PowerShell-Workflow. |


## <a name="graphical-runbooks"></a>Grafische runbooks

[Grafische](automation-runbook-types.md#graphical-runbooks) en grafische PowerShell Workflow runbooks worden gemaakt en bewerkt met de grafische editor in de portal Azure.  U kunt deze naar een bestand exporteren en vervolgens importeren in een andere account voor automatisering, maar u kunt maken of bewerken met een ander hulpprogramma.  Grafische runbooks PowerShell code genereren, maar niet direct weergeven of wijzigen van de code. Grafische runbooks kan niet worden geconverteerd naar een van de [tekstindelingen](automation-runbook-types.md), noch kan een runbook tekst worden geconverteerd naar een grafische indeling. Grafische runbooks kunnen worden geconverteerd naar grafische PowerShell Workflow runbooks tijdens de invoer en vice versa.

### <a name="advantages"></a>Voordelen

- Runbooks maken met een minimale kennis van [PowerShell](automation-powershell-workflow.md).
- Visueel management processen vertegenwoordigen.
- Andere runbooks bevatten als onderliggende runbooks op hoog niveau werkstromen maken.


### <a name="limitations"></a>Beperkingen

- Runbook buiten Azure portal kan niet worden bewerkt.
- Mogelijk een Code activiteit met PowerShell code voor het uitvoeren van complexe logica.
- Niet weergeven of de PowerShell code die is gemaakt door de grafische workflow rechtstreeks te bewerken. Houd er rekening mee dat u de code die u in alle activiteiten van de Code maakt kunt bekijken.


## <a name="powershell-runbooks"></a>PowerShell runbooks

PowerShell runbooks zijn gebaseerd op Windows PowerShell.  U kunt de code van de runbook met de teksteditor in Azure portal rechtstreeks bewerken.  U kunt ook alle off line teksteditor en [de runbook importeren](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure automatisering gebruiken.

### <a name="advantages"></a>Voordelen

- Alle complexe logica met PowerShell code zonder de extra complexiteit van PowerShell Workflow te implementeren. 
- Runbook sneller dan runbooks grafische of PowerShell Workflow wordt gestart omdat het niet hoeft te worden gecompileerd voordat u.

### <a name="limitations"></a>Beperkingen

- Moet bekend zijn met PowerShell-scripts.
- [Parallelle verwerking](automation-powershell-workflow.md#parallel-processing) kan niet worden gebruikt voor meerdere acties parallel.
- Niet [controlepunten](automation-powershell-workflow.md#checkpoints) gebruiken runbook in geval van een fout weer.
- PowerShell Workflow runbooks en grafische runbooks kan alleen worden opgenomen als de onderliggende runbooks met behulp van de cmdlet Start AzureAutomationRunbook die een nieuwe taak maakt.

### <a name="known-issues"></a>Bekende problemen
Hieronder vindt u actuele bekende problemen met PowerShell runbooks.

- PowerShell runbooks niet mogelijk een niet-gecodeerde [variabele activa](automation-variables.md) met een null-waarde niet ophalen.
- PowerShell runbooks niet ophalen een [variabele activa](automation-variables.md) *~* in de naam.
- Get-proces in een lus in een PowerShell runbook loopt vast na ongeveer 80 keer. 
- Een runbook PowerShell kan mislukken als u probeert een zeer grote hoeveelheid gegevens in één keer naar de uitvoerstroom schrijven.   U kunt dit probleem meestal omzeilen door het uitvoeren van alleen de informatie die u nodig hebt bij het werken met grote objecten.  Bijvoorbeeld, in plaats van iets als *Get-Process*uitvoeren, kunt u alleen de vereiste velden met *Get-Process uitvoeren | Selecteer de procesnaam, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell Workflow runbooks

PowerShell Workflow runbooks zijn tekst-runbooks op basis van [Windows PowerShell-Workflow](automation-powershell-workflow.md).  U kunt de code van de runbook met de teksteditor in Azure portal rechtstreeks bewerken.  U kunt ook alle off line teksteditor en [de runbook importeren](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure automatisering gebruiken.

### <a name="advantages"></a>Voordelen

- Alle complexe logica met PowerShell Workflow code implementeren.
- [Controlepunten](automation-powershell-workflow.md#checkpoints) runbook in geval van een fout weer gebruiken.
- [Parallelle verwerking](automation-powershell-workflow.md#parallel-processing) gebruikt voor meerdere acties parallel.
- Andere grafische runbooks en runbooks PowerShell Workflow kan bevatten als onderliggende runbooks op hoog niveau werkstromen maken.


### <a name="limitations"></a>Beperkingen

- De auteur moet bekend zijn met PowerShell Workflow.
- Runbook moet omgaan met de extra complexiteit van PowerShell Workflow zoals [gedeserialiseerd objecten](automation-powershell-workflow.md#code-changes).
- Runbook duurt langer dan PowerShell runbooks starten omdat deze moet worden gecompileerd voordat u.
- PowerShell runbooks kan alleen worden opgenomen als de onderliggende runbooks met behulp van de cmdlet Start AzureAutomationRunbook die een nieuwe taak maakt.


## <a name="considerations"></a>Overwegingen met betrekking tot

U moet rekening gehouden met de volgende aanvullende overwegingen bij het bepalen van welk type u voor een bepaalde runbook.

- U kan runbooks van grafische converteren naar tekst type of vice versa.
- Er gelden beperkingen met runbooks van verschillende typen als een onderliggende runbook.  Zie [onderliggende runbooks in Azure automatisering](automation-child-runbooks.md) voor meer informatie.

  
## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het ontwerpen van grafische runbook, [grafische ontwerpen in Azure automatisering](automation-graphical-authoring-intro.md)
- Om het verschil tussen PowerShell en PowerShell workflows voor runbooks, Zie [Learning Windows PowerShell Workflow](automation-powershell-workflow.md)
- Zie voor meer informatie over het maken of importeren van een Runbook [maken of importeren van een Runbook](automation-creating-importing-runbook.md)



