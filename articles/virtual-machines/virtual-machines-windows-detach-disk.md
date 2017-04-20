<properties
    pageTitle="Loskoppelen van een schijf met gegevens uit een Windows VM | Microsoft Azure"
    description="Informatie over loskoppelen van een schijf met gegevens vanaf een virtuele machine in het implementatiemodel Resource Manager met Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Het ontkoppelen van een schijf met gegevens vanaf een virtuele machine onder Windows


Wanneer u een schijf met gegevens die gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u het eenvoudig loskoppelen. Dit verwijdert van de schijf van de virtuele machine, maar deze niet verwijderen uit de opslag. 

> [AZURE.WARNING] Als u een schijf die niet automatisch verwijderd loskoppelen. Als u bent geabonneerd op Premium opslag, moet u blijven gelden de tarieven van de opslag van de schijf. Raadpleeg voor meer informatie [voor prijzen en facturering wanneer u Premium opslag](../storage/storage-premium-storage.md#pricing-and-billing). 

Als u wilt dat de bestaande gegevens op de schijf om opnieuw te gebruiken, kunt u het opnieuw koppelen op dezelfde virtuele machine of een ander.  


## <a name="detach-a-data-disk-using-the-portal"></a>Loskoppelen van een schijf met gegevens met behulp van de portal

1. Selecteer de **virtuele Machines**in de portal hub.

2. Selecteer de virtuele machine met de gegevensschijf die u wilt loskoppelen en klik vervolgens op **alle instellingen**.

3. Selecteer in het blad **Instellingen** **schijven**.

4. Selecteer in het blad **schijven** schijf met de gegevens die u wilt loskoppelen.

5. In het blad voor de gegevensschijf, klikt u op **verbreken**.


    ![Schermafdruk waarin de knop loskoppelen.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

De schijf blijft opgeslagen, maar is niet langer gekoppeld aan een virtuele machine.


## <a name="detach-a-data-disk-using-powershell"></a>Loskoppelen van een schijf met gegevens met PowerShell

De eerste opdracht wordt in dit voorbeeld wordt de virtuele machine met de naam **MyVM07** in de resourcegroep voor **RG11** met de cmdlet Get-AzureRmVM. De opdracht wordt de virtuele machine in de variabele **$VirtualMachine** opgeslagen. 

De tweede opdracht verwijdert u de schijf met gegevens met de naam DataDisk3 uit de virtuele machine. 

De laatste opdracht werkt u de status van de virtuele machine voor het voltooien van het proces van het verwijderen van de gegevensschijf.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Zie voor meer informatie [Verwijderen AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Volgende stappen

Als u wilt dat de gegevensschijf opnieuw gebruiken, kunt u alleen [koppelen aan een andere VM](virtual-machines-windows-attach-disk-portal.md)
