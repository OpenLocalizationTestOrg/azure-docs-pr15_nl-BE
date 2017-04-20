<properties
 pageTitle="Automatisch schalen HPC Pack clusterknooppunten | Microsoft Azure"
 description="Automatisch vergroten of verkleinen van het aantal HPC Pack clusterknooppunten compute in Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatisch vergroot of verkleind de clusterbronnen HPC Pack in Azure volgens de werkbelasting van het cluster




Als u Azure 'burst'-knooppunten in het cluster HPC Pack implementeert, of u een Pack voor HPC-cluster in Azure VMs maakt, kunt u een manier om automatisch groter of kleiner het aantal Azure compute bronnen zoals knooppunten of cores op basis van de huidige werkbelasting op het cluster. Hiermee kunt u uw Azure middelen efficiënter gebruiken en beheren van hun kosten.
Stel hiervoor de Pack HPC-cluster eigenschap **AutoGrowShrink**. U kunt ook de **AzureAutoGrowShrink.ps1** HPC PowerShell script uitvoeren dat HPC Pack is geïnstalleerd.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ook kunnen op dit moment alleen automatisch groeien en verkleinen HPC Pack-computerknooppunten met een Windows-besturingssysteem.

## <a name="set-the-autogrowshrink-cluster-property"></a>Stel de eigenschap AutoGrowShrink cluster

### <a name="prerequisites"></a>Vereisten

* **HPC Pack 2012 R2 Update 2 of hoger cluster** - het hoofd clusterknooppunt kan worden geïmplementeerd in ruimten of in een VM Azure. Zie [een cluster van hybride met HPC Pack instellen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) om aan de slag met een hoofd knooppunt op gebouwen en Azure 'burst'-knooppunten. Zie het [HPC Pack IaaS implementatiescript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) kan snel een Pack HPC-cluster in Azure VMs.


* **Voor een cluster met een head-knooppunt in Azure** - als u een script voor de implementatie van de HPC Pack IaaS het cluster maakt, kunt u de eigenschap **AutoGrowShrink** cluster inschakelen door de optie AutoGrowShrink in het configuratiebestand van het cluster. Zie voor meer informatie de documentatie bij het [script downloaden](https://www.microsoft.com/download/details.aspx?id=44949). 

    De eigenschap **AutoGrowShrink** cluster ook inschakelen nadat u het cluster hebt geïmplementeerd met behulp van HPC PowerShell-opdrachten in de volgende sectie wordt beschreven. Als voorbereiding voor deze eerst de volgende stappen:
    1. Een certificaat Azure beheer configureren op het knooppunt head en in het abonnement op Azure. U kunt voor een testinstallatie gebruiken standaard Microsoft HPC Azure zelf-ondertekend certificaat die HPC Pack wordt geïnstalleerd op het knooppunt head en gewoon dat certificaat uploaden naar uw abonnement Azure. Zie de [richtlijnen van de TechNet Library](https://technet.microsoft.com/library/gg481759.aspx)voor opties en werk.
    2. Voer **regedit** op het knooppunt head, Ga naar HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo en voeg een nieuwe tekenreekswaarde. De naam van de waarde voor 'Vingerafdruk' en de waardegegevens voor de blauwdruk van het certificaat in stap 1.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC PowerShell-opdrachten voor het instellen van de eigenschap AutoGrowShrink

Volgende zijn HPC PowerShell-opdrachten kunt u **AutoGrowShrink** instellen en afstemmen met extra parameters het gedrag van het monster. Zie [parameters AutoGrowShrink](#AutoGrowShrink-parameters) verderop in dit artikel voor de volledige lijst van de instellingen. 

U kunt deze opdrachten uitvoeren, HPC PowerShell op het clusterknooppunt head te starten als administrator.

**De eigenschap AutoGrowShrink inschakelen**

    Set-HpcClusterProperty –EnableGrowShrink 1

**De eigenschap AutoGrowShrink uitschakelen**

    Set-HpcClusterProperty –EnableGrowShrink 0

**De grow-interval in minuten wijzigen**

    Set-HpcClusterProperty –GrowInterval <interval>

**Het verkleinen interval in minuten wijzigen**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**De huidige configuratie van de AutoGrowShrink weergeven**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink-parameters

AutoGrowShrink-parameters die u wijzigen kunt met de opdracht **Set-HpcClusterProperty** zijn.

* **EnableGrowShrink** - Switch of de eigenschap **AutoGrowShrink** uit te schakelen.
* **ParamSweepTasksPerCore** - aantal parametrische sweep taken één groeien. De standaardwaarde is één core per taak groeien. 
 
    >[AZURE.NOTE] **ParamSweepTasksPerCore** HPC Pack QFE KB3134307 gewijzigd in **TasksPerResourceUnit**. Het is gebaseerd op het taaktype en knooppunt, socket of core.
    
* **GrowThreshold** - drempel van taken in de wachtrij voor het starten van automatische groei. De standaardwaarde is 1, wat dat betekent als er 1 of meer taken in de wachtrij staat automatisch groeien knooppunten.
* **GrowInterval** - Interval in minuten voor het starten van automatische groei. Het standaardinterval is 5 minuten.
* **ShrinkInterval** - Interval in minuten voor het starten van automatische verkleinen. Het standaardinterval is 5 minuten. |
* **ShrinkIdleTimes** - continu controles wilt verkleinen om aan te geven van dat de knooppunten actief zijn. De standaardwaarde is 3 keer. Als de **ShrinkInterval** 5 minuten is, controleert HPC Pack bijvoorbeeld of het knooppunt niet actief is om de 5 minuten. Als de knooppunten in de inactieve status na 3 continu gecontroleerd (15 minuten), verkleind HPC Pack dat knooppunt.
* **ExtraNodesGrowRatio** - extra percentage van knooppunten Message Passing Interface (MPI) taken groei. De standaardwaarde is 1, wat betekent dat HPC Pack knooppunten 1% voor MPI taken groeit. 
* **GrowByMin** - Switch om aan te geven of het autogrow-beleid is gebaseerd op de minimaal vereiste bronnen voor de taak. De standaardwaarde is false, wat betekent dat HPC Pack knooppunten voor projecten op basis van de maximale vereiste bronnen voor de taken groeit.
* **SoaJobGrowThreshold** - drempel van binnenkomende SOA-aanvragen voor het starten van het automatische groeien proces. De standaardwaarde is 50000.  
    
    >[AZURE.NOTE] Deze parameter wordt ondersteund in de HPC Pack 2012 R2 Update 3.
    
* **SoaRequestsPerCore** -aantal binnenkomende SOA-aanvragen een core groeien. De standaardwaarde is 20000.  

    >[AZURE.NOTE] Deze parameter wordt ondersteund in de HPC Pack 2012 R2 Update 3.

### <a name="mpi-example"></a>Voorbeeld van MPI

Standaard HPC Pack groeit 1% extra knooppunten voor MPI-projecten (**ExtraNodesGrowRatio** is ingesteld op 1). De reden is dat MPI mogelijk meerdere knooppunten en de taak kan alleen worden uitgevoerd als alle knooppunten gereed zijn. Zodra Azure knooppunten, af en toe één knooppunt kan meer tijd nodig dan andere veroorzaakt door andere knooppunten actief is tijdens het wachten op dat knooppunt klaar starten. Door de groeiende extra knooppunten, HPC Pack vermindert de wachttijd voor deze resource en potentieel bespaart u kosten. Als u wilt verhogen van het percentage van de extra knooppunten voor MPI taken (bijvoorbeeld 10%), een vergelijkbaar met opdracht uitvoeren

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-voorbeeld

Standaard **SoaJobGrowThreshold** is ingesteld op 50000 en **SoaRequestsPerCore** is ingesteld op 200000. Als u één taak van SOA met 70000 aanvragen indient, zal er één taak en binnenkomende aanvragen zijn 70000. In dit geval groeit HPC Pack 1 core voor de taak in de wachtrij en voor binnenkomende verzoeken (70000 50000) zal toenemen / core 20000 = 1, dus in totaal 2 cores voor deze taak SOA zal toenemen.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Voer het script AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Vereisten

* **HPC Pack 2012 R2 Update 1 of hoger cluster** - het **AzureAutoGrowShrink.ps1** script is geïnstalleerd in de map % CCP_HOME % bin. Het hoofd clusterknooppunt kan worden geïmplementeerd in ruimten of in een VM Azure. Zie [een cluster van hybride met HPC Pack instellen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) om aan de slag met een hoofd knooppunt op gebouwen en Azure 'burst'-knooppunten. Zie het [HPC Pack IaaS implementatiescript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) kan snel een Pack HPC-cluster in Azure VMs of een [Azure quickstart sjabloon](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/)gebruiken.

* **Azure PowerShell 0.8.12** - die momenteel in het script zijn afhankelijk van deze specifieke versie van Azure PowerShell. Als u een nieuwere versie op het hoofd knooppunt uitvoert, is het wellicht Azure PowerShell downgraden naar [versie 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) voor het uitvoeren van het script. 

* **Voor een cluster met knooppunten Azure burst** - het script uitvoeren op een clientcomputer waarop HPC Pack is geïnstalleerd, of op het hoofd knooppunt. Als op een clientcomputer uitvoert, ervoor te zorgen dat u de variabele $env instellen: CCP_SCHEDULER goed om te verwijzen naar het hoofd knooppunt. De knooppunten Azure 'burst' is toegevoegd aan het cluster, maar zij kunnen worden in de staat niet geïmplementeerd.


* **Voor een cluster dat is geïmplementeerd in Azure VMs** - het script uitvoeren op het hoofd knooppunt VM, omdat deze afhankelijk van de **Begin-en HpcIaaSNode.ps1 is** en **Stop HpcIaaSNode.ps1** scripts die zijn er geïnstalleerd. Bovendien vereisen een certificaat Azure management of instellingenbestand publiceert deze scripts (Zie [beheren knooppunten in een cluster HPC Pack in Azure berekenen](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Zorg ervoor dat alle de compute node VMs u moet al zijn toegevoegd aan het cluster. Zij kunnen worden in de staat gestopt.

### <a name="syntax"></a>Syntaxis

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parameters

 * **NodeTemplates** - namen van het knooppunt sjablonen definiëren het bereik voor de knooppunten te vergroten of verkleinen. Als u niet opgeeft (de standaardwaarde is @()), alle knooppunten in de groep **AzureNodes** -knooppunt in het bereik zijn als **NodeType** een waarde van AzureNodes heeft en alle knooppunten in de groep **ComputeNodes** -knooppunt in het bereik als **NodeType** een waarde van ComputeNodes heeft.

 * **JobTemplates** - namen van de Project-sjablonen voor het definiëren van het bereik voor de knooppunten te laten groeien.

 * **NodeType** - het type knooppunt dat u wilt vergroten of verkleinen. Ondersteunde waarden zijn:

     * **AzureNodes** – voor knooppunten in een bedrijf op Azure PaaS (burst) of Azure IaaS cluster.

     * **ComputeNodes** - voor VMs-computerknooppunt alleen in een cluster IaaS Azure.

* **NumOfQueuedJobsPerNodeToGrow** - aantal wachtrijtaken nodig om te groeien één knooppunt.

* **NumOfQueuedJobsToGrowThreshold** - de drempelwaarde voor aantal wachtrijtaken de grow-proces te starten.

* **NumOfActiveQueuedTasksPerNodeToGrow** - het aantal actieve taken in de wachtrij moet één knooppunt groeien. Als **NumOfQueuedJobsPerNodeToGrow** met een waarde groter dan 0 is opgegeven, wordt deze parameter genegeerd.

* **NumOfActiveQueuedTasksToGrowThreshold** - de drempelwaarde voor aantal actieve taken in de wachtrij de grow-proces te starten.

* **NumOfInitialNodesToGrow** - het minimale aantal knooppunten om te groeien als alle knooppunten in het bereik **Niet is geïmplementeerd** of **gestopt (Deallocated)**.

* **GrowCheckIntervalMins** - het interval in minuten tussen controles om te groeien.

* **ShrinkCheckIntervalMins** - het interval in minuten tussen de controles te verkleinen.

* **ShrinkCheckIdleTimes** - het aantal continue verkleinen controles (gescheiden door een **ShrinkCheckIntervalMins**) om aan te geven van de knooppunten actief zijn.

* **UseLastConfigurations** - de vorige configuraties in het argument-bestand opgeslagen.

* **ArgFile**- de naam van het argument-bestand gebruikt voor het opslaan en bijwerken van de configuratie het script uitgevoerd.

* **LogFilePrefix** - voorvoegsel van de naam van het logboekbestand. U kunt een pad opgeven. Standaard wordt het logboek geschreven naar de huidige werkmap.

### <a name="example-1"></a>Voorbeeld 1

In het volgende voorbeeld wordt de Azure burst knooppunten geïmplementeerd met de AzureNode standaard sjabloon die u wilt vergroten of verkleinen, automatisch geconfigureerd. Als alle knooppunten in eerste instantie in de lidstaat **Niet is geïmplementeerd** , worden ten minste 3 knooppunten gestart. Als het aantal taken in de wachtrij groter is dan 8, start het script knooppunten totdat hun aantal groter is dan de verhouding tussen de taken in de wachtrij te **NumOfQueuedJobsPerNodeToGrow**. Als een knooppunt wordt actief in 3 maal niet actief wordt gevonden, wordt deze gestopt.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Voorbeeld 2

In het volgende voorbeeld wordt de Azure compute node VMs geïmplementeerd met de ComputeNode standaard sjabloon die u wilt vergroten of verkleinen, automatisch geconfigureerd.
De taken die zijn geconfigureerd door de standaardsjabloon voor de taak draagwijdte de van de belasting op het cluster. Als alle knooppunten in eerste instantie worden gestopt, worden ten minste 5 knooppunten gestart. Als het aantal actieve taken in de wachtrij groter is dan 15, start het script knooppunten totdat hun aantal groter is dan de verhouding van de actieve taken in de wachtrij te **NumOfActiveQueuedTasksPerNodeToGrow**. Als een knooppunt is actief in 10 maal niet actief wordt gevonden, wordt deze gestopt.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
