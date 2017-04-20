<properties
    pageTitle="Installatiekopie van algemene Azure VM VM | Microsoft Azure"
    description="Meer informatie over het vastleggen van een VM-afbeelding uit een gegeneraliseerde Azure VM gemaakt in het implementatiemodel Resource Manager"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Hoe u een installatiekopie van een gegeneraliseerde Azure VM VM


In dit artikel wordt beschreven hoe u Azure PowerShell gebruiken voor het maken van een afbeelding van een gegeneraliseerde Azure VM. U kunt de installatiekopie vervolgens gebruiken voor het maken van een andere VM. Het image bevat de OS-schijf en gegevensschijven die zijn gekoppeld aan de virtuele machine. De afbeelding bevat geen virtuele netwerkbronnen, dus u deze resources instellen moet bij het maken van de nieuwe VM. 


## <a name="prerequisites"></a>Vereisten

- U moet al [de VM generalized](virtual-machines-windows-generalize-vhd.md). Een VM noemt, verwijdert u alle uw persoonlijke gegevens, onder andere en zorgt ervoor dat de machine moet worden gebruikt als een afbeelding.

- Moet u Azure PowerShell versie 1.0.x of hoger zijn geïnstalleerd. Als u al PowerShell nog niet hebt geïnstalleerd, Lees [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor installatie-instructies.


## <a name="log-in-to-azure-powershell"></a>Log in op Azure PowerShell

1. Azure PowerShell openen en inloggen bij uw account Azure.

    ```powershell
    Login-AzureRmAccount
    ```

    Een pop-upvenster opent u uw Azure accountreferenties opgeven.

2. De abonnement-id's ophalen voor uw abonnementen beschikbaar.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Instellen van het juiste abonnement met de abonnement-ID.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Toewijzing van de VM en stelt de algemene staat       

1. Toewijzing van de VM-bronnen.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    De *Status* van de VM in Azure portal verandert **gestopt** **gestopt (opgeheven)**.

2. Stel de status van de virtuele machine op **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Controleer de status van de VM. De sectie **OSState/generalized** voor VM hebt **DisplayStatus** ingesteld op **VM generalized**.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>De afbeelding maken 

1. Kopieer de installatiekopie van de virtuele machine naar de doelcontainer opslag met behulp van deze opdracht. De afbeelding wordt gemaakt op dezelfde rekening als de oorspronkelijke virtuele machine opslag. De `-Path` parameter wordt een kopie van de sjabloon JSON lokaal opgeslagen. De `-DestinationContainerName` parameter is de naam van de container die u wilt uw afbeeldingen bevatten. Als de container niet bestaat, wordt deze voor u gemaakt.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Kun je de URL van uw afbeelding in de sjabloon voor het JSON. Ga naar de **bronnen**van de > **storageProfile** > **osDisk** > **afbeelding** > sectie van de**uri** voor het volledige pad van de afbeelding. De URL van de afbeelding eruitziet: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    U kunt ook controleren of de URI in de portal. De afbeelding wordt gekopieerd naar een container met de naam **system** in uw account voor opslag. 


## <a name="next-steps"></a>Volgende stappen

- U kunt nu [een VM uit de installatiekopie maken](virtual-machines-windows-create-vm-generalized.md).

