<properties 
    pageTitle="Opnieuw lanceren Linux virtuele Machines | Microsoft Azure" 
    description="Beschrijving van het implementeren van virtuele machines Linux om SSH verbindingsproblemen te verhelpen." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Implementeren van nieuwe Azure knooppunt virtuele machine

Als er zijn die in moeilijkheden verkeren probleemoplossing SSH of -toepassing toegang tot een Azure VM (virtual machine), opnieuw de VM kan helpen. Wanneer u opnieuw een VM, VM verplaatst naar een nieuw knooppunt binnen de infrastructuur van Azure en deze vervolgens weer in, behouden alle configuratie-opties en bijbehorende resources bevoegdheden. In dit artikel wordt beschreven hoe u te implementeren een VM met Azure CLI of de Azure portal.

> [AZURE.NOTE] Nadat u een VM implementeren, de tijdelijke schijf gaat verloren en dynamische IP-adressen die zijn gekoppeld aan een virtueel netwerkinterface worden bijgewerkt. 


## <a name="using-azure-cli"></a>Met behulp van Azure CLI

Zorg ervoor dat u beschikt over de [Meest recente Azure CLI geïnstalleerd](../xplat-cli-install.md) op uw computer en in de Resource Manager (`azure config mode arm`).

Gebruik de volgende opdracht Azure CLI te implementeren van uw virtuele machine:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

U kunt de status van de wijziging van de VM zien als deze door het proces opnieuw distribueren. De `PowerState` van de VM wordt 'Gestart' Updating, 'Start' en tot slot 'uitgevoerd' als deze door het proces van opnieuw te distribueren naar een nieuwe host. Controleer de status van de VMs binnen een groep met:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw VM problemen ondervindt, kunt u Help-informatie over [het oplossen van SSH-verbindingen](virtual-machines-linux-troubleshoot-ssh-connection.md) of [gedetailleerde SSH stappen voor probleemoplossing](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md)vinden. Als u geen toegang een toepassing op uw VM wordt uitgevoerd tot, kunt u ook de [toepassing het oplossen van problemen](virtual-machines-linux-troubleshoot-app-connection.md)lezen.