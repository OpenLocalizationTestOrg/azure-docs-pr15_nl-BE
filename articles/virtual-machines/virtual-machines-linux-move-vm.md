<properties
    pageTitle="Verplaatsen van een Linux VM | Microsoft Azure"
    description="Verplaats een VM Linux aan een andere Azure abonnement of bron in het implementatiemodel Resource Manager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Een VM Linux aan een andere abonnement of een resource verplaatsen

Dit artikel helpt u bij het verplaatsen van een Linux VM tussen resourcegroepen of abonnementen. Een VM verplaatsen tussen abonnementen kan handig zijn als u in een persoonlijk abonnement een VM hebt gemaakt en wilt verplaatsen naar een abonnement van uw bedrijf.

> [AZURE.NOTE] Nieuwe resource-id's worden gemaakt als onderdeel van de verhuizing. Zodra de VM is verplaatst, moet u de hulpprogramma's en scripts die de nieuwe resource-id's bijwerken. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Gebruik de CLI Azure een VM verplaatsen 

Om met succes een VM verplaatst, moet u de VM en alle ondersteunende resources verplaatsen. Gebruik de opdracht **show azure groep** om de resources in een resourcegroep en hun id. Hiermee pipe de uitvoer van deze opdracht naar een bestand, zodat u kunt kopiëren en plakken van de id's in latere opdrachten.

    azure group show <resourceGroupName>

Als u een VM en de bijbehorende bronnen aan een andere resource, gebruikt de opdracht CLI **azure resource verplaatsen** . In het volgende voorbeeld ziet u hoe een VM en de meest voorkomende bronnen moet verplaatsen. We gebruiken de parameter **-i** en in een door komma's gescheiden lijst (zonder spaties) van id's voor de bronnen te verplaatsen.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Als u wilt dat de VM en de bijbehorende bronnen verplaatsen naar een ander abonnement, voegt u de **--bestemming subscriptionId & #60; destinationSubscriptionID & #62;** parameter geeft u het doelabonnement.

Als u vanaf de opdrachtprompt op een computer met Windows werkt, moet u toevoegen een **$** voor de namen van variabelen als u ze kunt declareren. Dit is niet nodig in Linux.

U wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resource. Type **Y** om te bevestigen dat u wilt verplaatsen van de resources.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten bronnen verplaatsen tussen resourcegroepen en abonnementen. Zie de [bronnen naar een nieuwe resourcegroep of abonnement verplaatsen](../resource-group-move-resources.md)voor meer informatie.    