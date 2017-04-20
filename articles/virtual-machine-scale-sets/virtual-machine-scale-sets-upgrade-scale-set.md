<properties
    pageTitle="Implementeren van een app op de virtuele machine schaal sets | Microsoft Azure"
    description="Implementeren van een app op de virtuele machine schaal sets"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Upgraden van een virtuele machine schaal instellen

In dit artikel wordt beschreven hoe u kunt zetten een update van het OS op de schaal van een Azure VM zonder enige uitvaltijd. In deze context moet een OS update versie of SKU van het besturingssysteem wijzigen of de URI van een aangepaste afbeelding wijzigen. Zonder uitvaltijd bijwerken van virtuele machines in een of in groepen (zoals één domein voor fouttolerantie tegelijk) in plaats van allemaal tegelijk worden bijgewerkt. Door dit te doen, kan elke virtuele machines die worden niet bijgewerkt blijven uitvoeren.

Om misverstanden te voorkomen, zullen we onderscheiden drie soorten OS-update die u wilt uitvoeren:

- De versie of SKU van een besturingssysteemkopie wijzigen. Wijzigen van bijvoorbeeld Ubuntu versie 14.04.2-LTS van 14.04.201506100 naar 14.04.201507060, of het wijzigen van de Ubuntu 15.10/nieuwste SKU te 16.04.0-LTS/latest. In dit scenario worden behandeld in dit artikel.

- De URI die naar een nieuwe versie van een aangepaste installatiekopie verwijst wijzigen gebouwd (**Eigenschappen** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **afbeelding** > **uri**). In dit scenario worden behandeld in dit artikel.

- Herstellen van het besturingssysteem uit binnen een virtuele machine (voorbeelden hiervan zijn een beveiligingspatch installeren en uitvoeren van Windows Update). In dit scenario wordt ondersteund, maar niet in dit artikel worden gedekt.

De eerste twee opties zijn ondersteunde eisen in dit artikel bedoelde. U moet een nieuwe schaal instellen voor het uitvoeren van de derde optie maken.

Virtuele machine schaal sets die zijn geïmplementeerd als onderdeel van een cluster [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) vallen niet hier.

De basis voor het wijzigen van de OS-versie/SKU van een besturingssysteemkopie of de URI van een aangepaste afbeelding ziet er als volgt:

1. Het model met een virtuele machine schaal krijgen.

2. Wijziging in de versie, de SKU of de URI-waarde in het model.

3. Het model bijwerken.

4. Een aanroep van *manualUpgrade* op de virtuele machines in de schaal doen. Deze stap is alleen van toepassing als *upgradePolicy* is ingesteld op **handmatig** in uw set schaal. Als deze is ingesteld op **automatisch**, de virtuele machines opgewaardeerd in één keer ertoe leidt dat uitvaltijd.


Deze achtergrondinformatie in gedachten, laten we zien hoe u de versie van een schaal in PowerShell en met de REST API kan bijwerken. Deze voorbeelden betrekking op het geval van een besturingssysteemkopie, maar in dit artikel vindt u dit proces voor een aangepaste afbeelding aan te passen.

## <a name="powershell"></a>PowerShell##

Dit voorbeeld werkt een Windows VM schaal ingesteld op de nieuwe versie 4.0.20160229. Na het bijwerken van het model, wordt een exemplaar van update één virtuele machine tegelijk.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Als u de URI voor een aangepaste afbeelding niet te wijzigen, een platform-versie van de afbeelding bijwerken wilt, vervangen door de regel 'set van de nieuwe versie' ongeveer zo uit:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>De REST-API

Hier volgen enkele voorbeelden gebruiken de Azure REST API implementeert de update van een OS-versie van Python. Zowel de lichte [azurerm](https://pypi.python.org/pypi/azurerm) -bibliotheek van Azure REST API-wrapper-functies voor GET op schaal set model, gevolgd door een PUT met een bijgewerkte model gebruikt. Zij ook bekijken virtuele machine exemplaren weergaven voor het identificeren van de virtuele machines door update domein.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) is een Python-script dat wordt gebruikt voor het bijwerken van het besturingssysteem op een actieve virtuele machine schaal zetten een update domein per keer instellen.

![Vmssupgrade-script voor virtuele machines of een domein bijwerken kiezen](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Met dit script kunt u specifieke virtuele machines te werken of geef een domein bijwerken te kiezen. Het ondersteunt een platform-versie van de afbeelding wijzigen of de URI van een aangepaste afbeelding wijzigen.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is een algemene editor voor virtuele machine schaal stelt dat de status van de virtuele machine als een heatmap waarbij één rij vertegenwoordigt één update domein. U kunt onder andere het model voor een set schaal bijwerken met een nieuwe versie, de SKU of de aangepaste afbeelding URI en kies vervolgens een fout met betrekking tot domeinen upgraden. Als u dit doet, worden de virtuele machines in dat domein update bijgewerkt naar het nieuwe model. Ook kunt u een rolling upgrade op basis van de grootte van de partij van uw keuze doen.  

Het volgende screenshot toont een model van een schaal instelt voor Ubuntu 14.04-2LTS versie 14.04.201507060. Veel meer opties zijn toegevoegd aan dit programma omdat dit screenshot is genomen.

![Model van een schaal instelt voor Ubuntu 14.04-2LTS Vmsseditor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Nadat u **een upgrade uitvoert** en **Details ophalen**op, start de virtuele machines in een UD 0 bij te werken.

![Vmsseditor tonen update wordt uitgevoerd](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
