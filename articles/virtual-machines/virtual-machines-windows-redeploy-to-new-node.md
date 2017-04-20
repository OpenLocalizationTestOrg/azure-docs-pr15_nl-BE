<properties 
    pageTitle="Implementeer Windows virtuele machines | Microsoft Azure" 
    description="Beschrijving van het implementeren van virtuele machines Windows om RDP-verbindingsproblemen te verhelpen." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Implementeren van nieuwe Azure knooppunt virtuele machine

Als u moeilijkheden zijn gericht kan problemen met extern bureaublad verbinding of toepassing toegang tot Windows Azure virtuele machine (VM), VM opnieuw distribueren helpen. Wanneer u opnieuw een VM, VM verplaatst naar een nieuw knooppunt binnen de infrastructuur van Azure en deze vervolgens weer in, behouden alle configuratie-opties en bijbehorende resources bevoegdheden. In dit artikel wordt beschreven hoe u te implementeren een VM met Azure PowerShell of de Azure portal.

> [AZURE.NOTE] Nadat u een VM implementeren, de tijdelijke schijf gaat verloren en dynamische IP-adressen die zijn gekoppeld aan een virtueel netwerkinterface worden bijgewerkt. 

## <a name="using-azure-powershell"></a>Azure PowerShell gebruiken

Zorg ervoor dat u beschikt over de meest recente Azure PowerShell 1.x op uw computer zijn geïnstalleerd. Zie voor meer informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Gebruik deze opdracht Azure PowerShell naar de virtuele machine opnieuw implementeren:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw VM problemen ondervindt, kunt u Help-informatie over [het oplossen van RDP-verbindingen](virtual-machines-windows-troubleshoot-rdp-connection.md) of [gedetailleerde RDP stappen voor probleemoplossing](virtual-machines-windows-detailed-troubleshoot-rdp.md)vinden. Als u geen toegang een toepassing op uw VM wordt uitgevoerd tot, kunt u ook de [toepassing het oplossen van problemen](virtual-machines-windows-troubleshoot-app-connection.md)lezen.