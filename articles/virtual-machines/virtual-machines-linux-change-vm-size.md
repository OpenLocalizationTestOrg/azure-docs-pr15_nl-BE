<properties
   pageTitle="Hoe u het formaat van een Linux VM | Microsoft Azure"
   description="Het vergroten of verkleinen van een virtuele machine van Linux, door de grootte van de VM."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Hoe u het formaat van een Linux VM

## <a name="overview"></a>Overzicht 

Nadat u een virtuele machine (VM) inrichten, kunt u de schaal de VM omhoog of omlaag door de [grootte van de VM][vm-sizes]. In sommige gevallen moet u eerst de VM opheffen. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op het cluster hardware die als host voor de VM fungeert.

In dit artikel ziet u hoe het formaat van een Linux VM met behulp van de [CLI Azure][azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.


## <a name="resize-a-linux-vm"></a>Het formaat van een Linux-VM 

Als u een VM, kunt u de volgende stappen uitvoeren.

1. Voer de volgende opdracht in de CLI. Met deze opdracht worden de VM-formaten die beschikbaar zijn op de hardware-cluster waarin de VM wordt gehost.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Als de gewenste grootte wordt weergegeven, kunt u de volgende opdracht om het formaat van de VM te uitvoeren.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    De VM wordt opnieuw opgestart tijdens dit proces. Na de herstart wordt de bestaande OS en data schijven opnieuw toegewezen. Alles wat op de tijdelijke schijf worden verloren.

    Gebruik de `--enable-boot-diagnostics` optie kunt [opstarten diagnostische gegevens][boot-diagnostics], aan te melden alle fouten met betrekking tot het opstarten.

3. Als het gewenste formaat niet wordt vermeld, voert u de volgende opdrachten de VM, de toewijzing het formaat en de VM start.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Dynamische IP-adressen toegewezen aan de VM VM vrijgeven ook worden vrijgegeven. De schijven van OS en de gegevens worden niet beïnvloed.
   
## <a name="next-steps"></a>Volgende stappen

Uitvoeren van meerdere exemplaren van de VM voor extra schaalbaarheid, en schaalt. Zie voor meer informatie [automatisch schalen Linux machines in een virtuele Machine schaal ingesteld][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md