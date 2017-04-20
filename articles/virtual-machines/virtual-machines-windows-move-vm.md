<properties
    pageTitle="Verplaatsen van een Windows VM | Microsoft Azure"
    description="Verplaatsen van een Windows VM aan een andere Azure abonnement of bron in het implementatiemodel Resource Manager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Een Windows VM aan een andere Azure-abonnement of een resource verplaatsen 

Dit artikel helpt u bij het verplaatsen van een Windows VM tussen resourcegroepen of abonnementen. Verplaatsen tussen abonnementen kan handig zijn als u een VM hebt gemaakt in een persoonlijk abonnement en wilt verplaatsen naar een abonnement van uw bedrijf om te gaan met uw werk.

> [AZURE.NOTE] Nieuwe resource-id's wordt gemaakt als onderdeel van de verplaatsing. Zodra de VM is verplaatst, moet u voor het bijwerken van uw programma's en scripts die de nieuwe resource-id. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Gebruik Powershell om een VM

Als u een virtuele machine naar een andere groep verplaatst, moet u om ervoor te zorgen dat u ook alle afhankelijke bronnen verplaatsen. De cmdlet verplaatsen AzureRMResource gebruiken, moet u de naam en het type bron. U kunt ophalen van de cmdlet zoeken naar AzureRMResource.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Als u wilt verplaatsen van een VM moet meerdere resources verplaatsen. We kunnen alleen afzonderlijke variabelen voor elke resource te maken en deze vermelden. In het volgende voorbeeld bevat het merendeel van de algemene resources voor een VM, maar u kunt toevoegen als nodig is.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Het om bronnen te verplaatsen naar een ander abonnement, zijn de **DestinationSubscriptionId -** parameter. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



U wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resources. Type **Y** om te bevestigen dat u wilt verplaatsen van de resources.

  
## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten bronnen verplaatsen tussen resourcegroepen en abonnementen. Zie de [bronnen naar een nieuwe resourcegroep of abonnement verplaatsen](../resource-group-move-resources.md)voor meer informatie.    