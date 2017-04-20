<properties
    pageTitle="Een Linux VM aanpassen tijdens het maken van met cloud-init | Microsoft Azure"
    description="Met behulp van cloud-init een Linux VM aanpassen tijdens het maken."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Met behulp van cloud-init een Linux VM aanpassen tijdens het maken van

Dit artikel wordt beschreven hoe u een wolk init script de pakketten van de update is geïnstalleerd, de hostnaam instellen en beheren van gebruikersaccounts.  De wolk init-scripts worden tijdens het maken van VM in Azure CLI genoemd.  Het artikel moet:

- een Azure-account ([gratis proberen](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure CLI](../xplat-cli-install.md) vastgelegd met `azure login`.

- de modus Azure CLI _moet in_ Azure Resource Manager `azure config mode arm`.

## <a name="quick-commands"></a>Snelle opdrachten

Een cloud-init.txt script maken dat de hostnaam wordt ingesteld, werkt u alle pakketten en voegt een gebruiker sudo naar Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Een resourcegroep VMs in starten als u wilt maken.

```bash
azure group create myResourceGroup westus
```

Maak een Linux VM met cloud init te configureren tijdens het opstarten.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Gedetailleerde scenario

### <a name="introduction"></a>Inleiding

Wanneer u een nieuwe Linux VM start, krijgt u een standaard Linux VM met niets aangepaste of gereed is voor uw behoeften. [Cloud-init](https://cloudinit.readthedocs.org) is een standaardmethode voor het invoeren van een script of de configuratie-instellingen in die Linux VM zoals deze voor de eerste keer wordt opgestart.

Op Azure, er zijn drie manieren een te wijzigen naar een Linux VM zoals deze wordt geïmplementeerd of opgestart.

- Invoeren met behulp van cloud init scripts.
- Scripts met de [Extensie VMAccess](virtual-machines-linux-using-vmaccess-extension.md)-Azure invoeren.
- Azure met een sjabloon wolk init.
- Azure met een sjabloon [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Om te ' injecteren ' scripts op elk gewenst moment na het opstarten:

- SSH opdrachten rechtstreeks uitvoeren
- Injecteer scripts met de [Extensie VMAccess](virtual-machines-linux-using-vmaccess-extension.md)Azure imperatively of in een sjabloon Azure
- Configuratie management tools zoals Ansible, zout, Chef en Puppet.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Beschikbaarheid van cloud-init in Azure VM quick-afbeelding aliassen maken:

| Alias     | Publisher | Aanbieding        | SKU         | Versie | cloud-init |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7.2         | meest recente  | geen         |
| CoreOS    | CoreOS    | CoreOS       | Stabiel      | meest recente  | Ja        |
| Debian    | credativ  | Debian       | 8           | meest recente  | geen         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  | geen         |
| RHEL      | RedHat    | RHEL         | 7.2         | meest recente  | geen         |
| UbuntuLTS | Canonieke | UbuntuServer | 14.04.4-LTS | meest recente  | Ja        |

Microsoft werkt samen met onze partners om opgenomen en werkt in de afbeeldingen die ze aan Azure leveren cloud-init.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Een wolk init script toe te voegen aan het maken van de VM met de CLI Azure

Om een wolk init-script wordt gestart bij het maken van een VM in Azure, het cloud init-bestand met behulp van de CLI Azure opgeven `--custom-data` overschakelen.

Een resourcegroep VMs in starten als u wilt maken.

```bash
azure group create myResourceGroup westus
```

Maak een Linux VM met cloud init te configureren tijdens het opstarten.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Cloud-init script voor de hostnaam van een Linux VM maken

Een van de eenvoudigste en meest belangrijke instellingen voor alle Linux VM is de hostnaam. We kunt gemakkelijk instellen met cloud init met dit script.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Voorbeeld van de wolk init script met de naam `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Tijdens het opstarten van de VM deze wolk init script wordt de hostnaam op `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Aanmelding en controleer of de hostnaam van de nieuwe VM.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Maken van een wolk init script om te werken Linux

Beveiliging wilt u uw Ubuntu VM bijwerken voor het eerst wordt opgestart.  Met behulp van cloud-init die kunt doen we dat met het volgende script, afhankelijk van de Linux-distributie die u gebruikt.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Voorbeeld van de wolk init script `cloud_config_apt_upgrade.txt` voor de Debian-familie

```bash
#cloud-config
apt_upgrade: true
```

Nadat Linux heeft opgestart, alle geïnstalleerde pakketten worden bijgewerkt via `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Aanmelding en controleer of alle pakketten zijn bijgewerkt.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Maken van een wolk init script om een gebruiker toevoegen aan Linux

Een van de eerste taken op elke nieuwe Linux VM is een gebruiker toevoegen voor uzelf of om te voorkomen dat met behulp van `root`. SSH sleutels zijn best practices voor beveiliging en bruikbaarheid en ze worden toegevoegd aan de `~/.ssh/authorized_keys` -bestand met deze wolk init script.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Voorbeeld van de wolk init script `cloud_config_add_users.txt` voor Debian-familie

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Nadat Linux heeft opgestart, worden de weergegeven gebruikers gemaakt en toegevoegd aan de groep sudo.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Aanmelding en controleer of de nieuwe gebruiker.

```bash
ssh myVM
cat /etc/group
```

Uitvoer

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Volgende stappen

Cloud-init wordt steeds een standaardmethode voor het wijzigen van uw VM Linux op te starten. Azure is voor VM-uitbreidingen waarmee u uw LinuxVM op te starten of terwijl deze wordt uitgevoerd wijzigen. U kunt bijvoorbeeld de Azure VMAccessExtension SSH-of Gebruikersgegevens opnieuw instellen tijdens het uitvoeren van de VM. Met cloud-init moet u het wachtwoord opnieuw wilt opstarten.

[Over functies en uitbreidingen van de virtuele machine](virtual-machines-linux-extensions-features.md)

[Beheren van gebruikers, SSH en controleer of het herstellen van schijven op Azure Linux VMs met de extensie VMAccess](virtual-machines-linux-using-vmaccess-extension.md)
