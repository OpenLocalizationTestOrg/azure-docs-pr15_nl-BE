<properties
    pageTitle="Verschillende manieren voor het maken van een Linux VM | Microsoft Azure"
    description="Informatie over de verschillende manieren voor het maken van een virtuele machine van Linux op Azure, waaronder koppelingen naar hulpprogramma's en zelfstudies voor elke methode."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Verschillende manieren voor het maken van een virtuele Linux machine in Azure

U hebt de flexibiliteit in Azure een Linux virtuele machine (VM) met hulpprogramma's en workflows om u vertrouwd te maken. Dit artikel bevat een overzicht van deze verschillen en voorbeelden voor het maken van uw Linux VMs.


## <a name="azure-cli"></a>Azure CLI 

De CLI Azure is beschikbaar op platforms die via een npm-pakket, pakketten geleverde distro of Docker container. Meer informatie over [het installeren en configureren van de CLI Azure](../xplat-cli-install.md). De volgende zelfstudies bieden voorbeelden over het gebruik van de CLI Azure. Lees elk artikel voor meer informatie over de CLI quick start-opdrachten weergegeven:

- [Maak een Linux VM uit de CLI Azure voor dev en test](virtual-machines-linux-quick-create-cli.md)
    - Het volgende voorbeeld wordt een CoreOS VM met een openbare sleutel met de naam `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Een beveiligde Linux VM met behulp van een Azure-sjabloon maken](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - In het volgende voorbeeld wordt een VM met behulp van een sjabloon die is opgeslagen op GitHub gemaakt:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Maak een volledige Linux-omgeving met behulp van de CLI Azure](virtual-machines-linux-create-cli-complete.md)
    - Bevat een load balancer en meerdere VMs maken in een set van beschikbaarheid.

- [Een schijf toevoegen aan een Linux VM](virtual-machines-linux-add-disk.md)
    - In het volgende voorbeeld wordt een 5Gb schijf toegevoegd aan een bestaande VM met de naam `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure portal

De [Azure portal](https://portal.azure.com) kunt u snel een VM maken omdat er niets te installeren op uw systeem. De Azure portal gebruiken voor het maken van de VM:

- [Maak een Linux VM met de Azure portal](virtual-machines-linux-quick-create-portal.md) 
- [Een schijf met de Azure portal koppelen](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Besturingssysteem en de opties in de installatiekopie
Wanneer een VM maken, kiest u een afbeelding op basis van het besturingssysteem dat u wilt uitvoeren. Azure en haar partners bieden veel afbeeldingen, waarvan sommige toepassingen en hulpprogramma's die vooraf geïnstalleerd zijn. Of uploaden van een van uw eigen afbeeldingen (Zie [de volgende sectie](#use-your-own-image)).

### <a name="azure-images"></a>Azure afbeeldingen
Gebruik de `azure vm image` CLI-opdrachten om te zien wat er door publisher en distro release builds beschikbaar is.

Een overzicht van beschikbare uitgevers als volgt:

```bash
azure vm image list-publishers --location WestUS
```

Een overzicht van beschikbare producten (aanbiedingen) voor een bepaalde uitgever als volgt:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Een overzicht van beschikbare SKU's (distro releases) van een opgegeven aanbod als volgt:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Voor een bepaalde release volgt een overzicht van alle beschikbare afbeeldingen:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Zie voor meer voorbeelden van de bladeren en het gebruik van de beschikbare afbeeldingen, [navigeren en selecteer Azure VM afbeeldingen met de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).

De `azure vm quick-create` en `azure vm create` -opdrachten kunt u snel toegang tot de meest voorkomende distros en hun laatste releases aliassen hebben. Met behulp van aliassen is vaak sneller dan de uitgever, aanbieding, SKU en versie opgeven telkens wanneer die u een VM maken:

| Alias     | Publisher | Aanbieding        | SKU         | Versie |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | meest recente  |
| CoreOS    | CoreOS    | CoreOS       | Stabiel      | meest recente  |
| Debian    | credativ  | Debian       | 8           | meest recente  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  |
| RHEL      | RedHat    | RHEL         | 7.2         | meest recente  |
| SLES      | SLES      | SLES         | 12-SP1      | meest recente  |
| UbuntuLTS | Canonieke | UbuntuServer | 14.04.4-LTS | meest recente  |

### <a name="use-your-own-image"></a>Uw eigen afbeelding gebruiken

Als u specifieke aanpassingen nodig hebt, kunt u een afbeelding op basis van een bestaande Azure VM door het *vastleggen van* deze VM. U kunt ook een afbeelding gemaakt op ruimten uploaden. Raadpleeg de volgende artikelen voor meer informatie over ondersteunde distros en hoe u uw eigen afbeeldingen gebruiken:

- [Azure geviseerd verdelingen](virtual-machines-linux-endorsed-distros.md)

- [Informatie voor niet-geviseerd verdelingen](virtual-machines-linux-create-upload-generic.md)

- [Een virtuele Linux machine als een bronnenbeheerder sjabloon vastleggen](virtual-machines-linux-capture-image.md).
    - Voorbeeld van de opdrachten voor het vastleggen van een bestaande VM Quick-start:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Volgende stappen

- Maak een Linux VM vanuit de [portal](virtual-machines-linux-quick-create-portal.md)met de [CLI](virtual-machines-linux-quick-create-cli.md)of met behulp van een [sjabloon Azure Resource Manager](virtual-machines-linux-cli-deploy-templates.md).

- Na het maken van een Linux VM, [toevoegen van een schijf met gegevens](virtual-machines-linux-add-disk.md).

- Snelle stappen voor het [opnieuw instellen van een wachtwoord of SSH-sleutels en gebruikers beheren](virtual-machines-linux-using-vmaccess-extension.md)
