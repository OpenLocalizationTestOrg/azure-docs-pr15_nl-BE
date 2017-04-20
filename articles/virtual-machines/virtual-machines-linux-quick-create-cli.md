<properties
   pageTitle="Een Linux VM in Azure maken met behulp van de CLI | Microsoft Azure"
   description="Een Linux VM in Azure maken met behulp van de CLI."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Een Linux VM in Azure maken met behulp van de CLI

In dit artikel ziet u hoe een Linux virtuele machine (VM) op Azure snel implementeren met behulp van de `azure vm quick-create` opdracht in Azure opdrachtregelinterface (CLI). De `quick-create` opdracht implementeert een VM in een eenvoudige, veilige infrastructuur prototype gebruiken of snel testen van een concept. Het artikel moet:

- een Azure-account ([gratis proberen](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure CLI](../xplat-cli-install.md) vastgelegd met `azure login`.

- de modus Azure CLI _moet in_ Azure Resource Manager `azure config mode arm`.

U kunt ook snel een Linux VM implementeren met behulp van de [portal Azure](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-commands"></a>Snelle opdrachten

In het volgende voorbeeld ziet u hoe een VM CoreOS implementeren en koppelen van uw Secure Shell (SSH) sleutel (uw argumenten kunnen afwijken):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Gedetailleerde scenario

Het volgende scenario is een UbuntuLTS VM wordt geïmplementeerd, stap voor stap, met uitleg van wat elke stap doet.

## <a name="vm-quick-create-aliases"></a>VM quick-aliassen maken

Een snelle manier om te kiezen van een verdeling is toegewezen aan de meest voorkomende OS verdelingen Azure CLI aliassen gebruiken. De volgende tabel worden de aliassen (vanaf Azure CLI versie 0,10). Alle implementaties die gebruikmaken van `quick-create` standaard VMs die worden ondersteund door een solid-state drive (SSD) opslag die schijftoegang sneller aanmaken en hoge prestaties biedt. (Deze aliassen vertegenwoordigen een zeer klein deel van de beschikbare distributies op Azure. Meer afbeeldingen zoeken in de markt Azure door te [Zoeken naar een afbeelding in PowerShell](virtual-machines-linux-cli-ps-findimage.md), [op het web](https://azure.microsoft.com/marketplace/virtual-machines/)of [uw eigen aangepaste afbeelding uploaden](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Publisher | Aanbieding        | SKU         | Versie |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | meest recente  |
| CoreOS    | CoreOS    | CoreOS       | Stabiel      | meest recente  |
| Debian    | credativ  | Debian       | 8           | meest recente  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  |
| RHEL      | Red Hat    | RHEL         | 7.2         | meest recente  |
| UbuntuLTS | Canonieke | Ubuntu Server | 14.04.4-LTS | meest recente  |

De volgende secties gebruiken de `UbuntuLTS` alias voor de optie **ImageURN** (`-Q`) voor de implementatie van een 14.04.4 Ubuntu LTS-Server.

De vorige `quick-create` voorbeeld alleen wordt genoemd de `-M` vlag voor het identificeren van de openbare SSH-sleutel uploaden tijdens het uitschakelen van SSH wachtwoorden, zodat u wordt gevraagd om de volgende argumenten:

- Resourcegroepnaam (een willekeurige tekenreeks is meestal fijn voor uw eerste Azure resourcegroep.)
- VM-naam
- locatie (`westus` of `westeurope` zijn goede standaardinstellingen)
- Linux (naar Azure weet welk besturingssysteem u wilt laten)
- gebruikersnaam

In het volgende voorbeeld geeft alle waarden zodat er geen verdere vragen vereist is. Zolang er een `~/.ssh/id_rsa.pub` als een ssh-rsa public key-bestandsindeling, het werkt is:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

De uitvoer ziet er bijvoorbeeld de volgende uitvoer blokkeren:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Log in op de nieuwe VM

Log in op uw VM met behulp van het openbare IP-adres dat is vermeld in de uitvoer. Ook kunt u de volledig gekwalificeerde domeinnaam (FQDN) die wordt vermeld:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

De aanmelding ziet er ongeveer uit zoals in het volgende blok van uitvoer:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Volgende stappen

De `azure vm quick-create` opdracht is de manier om snel een VM implementeren, zodat u kunt zich bij een bash-shell aanmelden en kunnen gebruiken. Echter, met behulp van `vm quick-create` geeft u uitgebreide controle en het kunt u een complexere omgeving maken.  Voor de implementatie van een Linux VM die aangepast voor de infrastructuur, volgt u een van deze artikelen:

- [Een sjabloon Azure Resource Manager gebruiken voor het maken van een specifieke distributie](virtual-machines-linux-cli-deploy-templates.md)
- [Maak uw eigen aangepaste omgeving voor een Linux VM Azure CLI opdrachten rechtstreeks met](virtual-machines-linux-create-cli-complete.md)
- [Maak een SSH beveiligd Linux VM op Azure met behulp van sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

U kunt ook [gebruiken de `docker-machine` met diverse opdrachten om snel een VM Linux als host docker Azure-stuurprogramma](virtual-machines-linux-docker-machine.md).
