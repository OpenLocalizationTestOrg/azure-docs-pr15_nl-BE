<properties
 pageTitle="Beheren van de clusterknooppunten compute HPC Pack | Microsoft Azure"
 description="Meer informatie over PowerShell script hulpmiddelen voor het toevoegen, verwijderen, starten en stoppen van HPC Pack clusterknooppunten compute in Azure"
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

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Het aantal en de beschikbaarheid van de computerknooppunten in een cluster HPC Pack in Azure beheren

Als u een Pack voor HPC-cluster in Azure VMs hebt gemaakt, kunt u manieren eenvoudig toevoegen, verwijderen, (dient) starten of stoppen (identiteitsgegevens) een aantal compute knooppunt VMs in het cluster. Deze taken doet Azure PowerShell scripts die zijn geïnstalleerd op de kop VM-knooppunt worden uitgevoerd. Deze scripts kunnen u het aantal en de beschikbaarheid van de clusterbronnen HPC Pack bepalen zodat u kosten kunt bepalen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Vereisten

* **Pack HPC-cluster in Azure VMs** - een Pack HPC-cluster maken in het implementatiemodel klassiek met behulp van ten minste HPC Pack 2012 R2 Update 1. U kunt bijvoorbeeld de implementatie automatiseren met behulp van de huidige afbeelding HPC Pack VM in de Marketplace Azure en een Azure PowerShell-script. Zie voor informatie en vereisten [maken een HPC-Cluster met het implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Na de implementatie, vinden het knooppunt van beheerscripts in de map % CCP\_HOME % bin-map op de hoofd-knooppunt. Elk van de scripts moet u uitvoeren als beheerder.

* **Azure instellingen bestand of management-certificaat publiceren** - u hoeft alleen een van de volgende opties in het head-knooppunt:

    * **De Azure import instellingenbestand publiceren**. Hiertoe de volgende Azure PowerShell cmdlets worden uitgevoerd op het knooppunt head:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Het certificaat Azure management op het knooppunt head configureren**. Als u het cer-bestand importeren in het certificaatarchief van de CurrentUser\My en voer vervolgens de volgende Azure PowerShell-cmdlet voor uw Azure-omgeving (AzureCloud of AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>VMs-computerknooppunt toevoegen

Compute-knooppunten toevoegen aan het script **Toevoegen HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxis
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parameters

* **Servicenaam** - naam van de cloud-service die nieuwe computerknooppunt VMs aan worden toegevoegd.

* **ImageName** - procesnaam Azure VM, die kan worden verkregen via de Azure klassieke portal of Azure PowerShell-cmdlet **Get-AzureVMImage**. De afbeelding moet voldoen aan de volgende eisen voldoen:

    1. Een Windows-besturingssysteem moet worden geïnstalleerd.

    2. HPC Pack moet zijn geïnstalleerd in de rol van de compute-knooppunt.

    3. De afbeelding moet een persoonlijke afbeelding in de categorie gebruikers, niet openbaar Azure VM-afbeelding.

* **Aantal** - aantal computerknooppunt VMs moet worden toegevoegd.

* **InstanceSize** - grootte van de compute node VMs.

* **DomainUserName** - domeinnaam, die wordt gebruikt voor de nieuwe VMs toevoegen aan het domein.

* **DomainUserPassword** - wachtwoord van de domeingebruiker.

* **NodeNameSeries** (optioneel) - patroon voor de compute nodes benoemen. Moet de notatie &lt; *basis\_naam*&gt;&lt;*Start\_nummer*&gt;%. Bijvoorbeeld: MyCN % 10% betekent een reeks de namen van de compute nodes vanaf MyCN11. Als u niet opgeeft, wordt het script het geconfigureerde knooppunt naming-reeks in de HPC-cluster.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt 20 grootte groot computerknooppunt VMs in de cloud service *hpcservice1*, op basis van de VM afbeelding *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>VMs-computerknooppunt verwijderen

-Computerknooppunten met het script **Verwijderen HpcIaaSNode.ps1** verwijderen.

### <a name="syntax"></a>Syntaxis

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parameters

* **Naam** - de namen van de knooppunten van het cluster worden verwijderd. Jokertekens worden ondersteund. De naam van de parameter is de naam. U kunt geen parameters voor de **naam** en het **knooppunt** opgeven.

* **Knooppunt** - object voor de knooppunten moet worden verwijderd, de HpcNode die kunnen worden verkregen via de HPC-PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is een knooppunt. U kunt geen parameters voor de **naam** en het **knooppunt** opgeven.

* **DeleteVHD** (optioneel) - als u op de gekoppelde schijven verwijderd voor de VMs die worden verwijderd.

* **Werking** (optioneel) - instellen om offline HPC-knooppunten voordat u ze verwijdert.

* **Bevestigen** (optioneel) - vragen om bevestiging voordat de opdracht wordt uitgevoerd.

* **WhatIf** - instelling om te beschrijven wat er zou gebeuren als u de opdracht zonder daadwerkelijk uitvoeren van de opdracht uitgevoerd.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld zorgt ervoor dat off line knooppunten met namen die beginnen *HPCNode-CN -* en ze verwijdert u de knooppunten en hun gekoppelde schijven.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Start de VMs-computerknooppunt

Start-computerknooppunten met het script **Start HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxis

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parameters

* **Naam** - de namen van de knooppunten van het cluster moet worden gestart. Jokertekens worden ondersteund. De naam van de parameter is de naam. U kunt geen parameters voor de **naam** en het **knooppunt** opgeven.

* **Knooppunt**- object voor de knooppunten moet worden gestart, de HpcNode die kunnen worden verkregen via de HPC-PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is een knooppunt. U kunt geen parameters voor de **naam** en het **knooppunt** opgeven.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt de knooppunten gestart met namen die beginnen *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>VMs-computerknooppunt stoppen

Stop-computerknooppunten met het script **Stoppen HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxis

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parameters


* **Naam**- de namen van de knooppunten van het cluster moet worden gestopt. Jokertekens worden ondersteund. De naam van de parameter is de naam. U kunt geen parameters voor de **naam** en het **knooppunt** opgeven.

* **Knooppunt** - object voor de knooppunten worden gestopt, de HpcNode die kunnen worden verkregen via de HPC-PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is een knooppunt. U kunt geen parameters voor de **naam** en het **knooppunt** opgeven.

* **Werking** (optioneel) - instelling forceren offline HPC knooppunten voordat deze wordt gestopt.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld zorgt ervoor dat off line knooppunten met namen die beginnen *HPCNode-CN -* en stopt de knooppunten.

– De naam HPCNodeCN - Stop-HPCIaaSNode.ps1 *-kracht

## <a name="next-steps"></a>Volgende stappen

* Als u een manier om automatisch groter of kleiner de clusterknooppunten op basis van de huidige werkbelasting van projecten en taken op het cluster, Zie [automatisch vergroot of verkleind de clusterbronnen HPC Pack in Azure, afhankelijk van de werkbelasting van het cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
