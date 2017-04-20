<properties
    pageTitle="Maak een kopie van uw Linux Azure VM | Microsoft Azure"
    description="Informatie over het maken van een kopie van uw Azure Linux virtuele machine in het implementatiemodel Resource Manager"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Een kopie maken van een Linux virtuele machine op Azure


In dit artikel wordt beschreven hoe u een kopie maken van uw Azure VM (virtual machine) waarop het implementatiemodel Resource Manager met Linux. Eerst u via het besturingssysteem en de gegevensschijven kopiëren naar een nieuwe container klikt en vervolgens de netwerkbronnen instellen en de nieuwe virtuele machine maken.

U kunt ook [uploaden en een VM uit aangepaste schijfkopie te maken](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Voordat u begint

Te zorgen dat u de volgende vereisten voordat u de stappen uit:

- U hebt de [Azure CLI] (.. / cli-xplat-install.md) gedownload en geïnstalleerd op uw computer. 

- U moet ook bepaalde informatie over uw bestaande Azure Linux VM:

| Informatie over VM | Waar u het |
|------------|-----------------|
| VM-naam | `azure vm list` |
| Naam van de resourcegroep | `azure vm list` |
| Locatie | `azure vm list` |
| De naam van opslag | `azure storage account list -g <resourceGroup>` |
| Naam van container | `azure storage container list -a <sourcestorageaccountname>` |
| Naam van de gegevensbron VM VHD-bestand | `azure storage blob list --container <containerName>` |



- U moet enkele keuzes maken over uw nieuwe VM:   <br> -Naam van container   <br> VM - naam   <br> VM - grootte   <br> vNet - naam   <br> -SubNet naam   <br> IP - naam   <br> Naam - NIC
    

## <a name="login-and-set-your-subscription"></a>Inloggen en instellen van uw abonnement

1. Aanmelden bij de CLI.
        
        azure login

2. Zorg ervoor dat bronbeheer modus.
    
        azure config mode arm

3. Stel het juiste abonnement. U kunt een Accountlijst met azure-voor een overzicht van al uw abonnementen.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Stop de VM 

Stop en toewijzing van de bron VM. U kunt de lijst met azure vm gebruiken om een lijst van alle de VMs in uw abonnement en hun resource namen.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Kopieer de VHD


Kunt u de VHD uit de opslag van de bron naar de bestemming met behulp van de `azure storage blob copy start`. In dit voorbeeld gaan we de VHD kopiëren naar dezelfde rekening opslag, maar een andere container.

Om de VHD kopiëren naar een andere container op dezelfde rekening van opslag, typt u:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Het virtuele netwerk ingesteld voor de nieuwe VM

Instellen van een virtueel netwerk- en NIC voor uw nieuwe VM. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Maak de nieuwe VM 

Nu kunt u een VM vanuit uw geüploade virtuele schijf [met behulp van een sjabloon resource manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) of via de CLI maken door de URI naar de schijf gekopieerd door te typen:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Volgende stappen

Zie meer informatie over het beheren van uw nieuwe virtuele machine met Azure CLI, [Azure CLI-opdrachten voor de Azure Resource Manager](azure-cli-arm-commands.md).
