<properties
    pageTitle="Maken en uploaden van een aangepaste afbeelding voor Linux | Microsoft Azure"
    description="Maken en uploaden van een virtuele harde schijf (VHD) naar Azure met een aangepaste Linux-afbeelding met het implementatiemodel Resource Manager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Uploaden en een Linux VM van aangepaste installatiekopie maken

In dit artikel wordt beschreven hoe u een virtuele harde schijf (VHD) uploaden naar het implementatiemodel Resource Manager met Azure en Linux VMs van deze aangepaste afbeelding maken. Met deze functie kunt u installeren en configureren van een Linux-distro op uw vereisten en maakt u met deze VHD snel Azure virtuele machines (VMs).

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende gegevens in de sectie snel de base opdrachten een VM uploaden naar Azure. Meer gedetailleerde informatie en context voor elke stap vindt u de rest van het document, [begint hier](#requirements).

Zorg dat u [De CLI Azure](../xplat-cli-install.md) ingelogd en Resource Manager-modus:

```bash
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myimages`.

Maak eerst een resourcegroep. In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `WestUs` locatie:

```bash
azure group create myResourceGroup --location "WestUS"
```

Een opslag-account voor het opslaan van uw virtuele schijven maken. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Een overzicht van de toegangstoetsen voor uw opslag-account. Maak een notitie van `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Maak een container in uw opslag-account met behulp van de opslag-sleutel die u hebt aangeschaft. Het volgende voorbeeld wordt een container met de naam `myimages` met de waarde van de opslag van `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Ten slotte de VHD uploaden naar de container die u hebt gemaakt. Het lokale pad naar de VHD onder `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

U kunt nu een VM maken vanuit uw geüploade virtuele schijf [met behulp van de sjabloon voor een Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). U kunt ook de CLI gebruiken door te geven van de URI naar de schijf (`--image-urn`). Het volgende voorbeeld wordt een VM met de naam `myVM` met behulp van de virtuele schijf hebt geüpload:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

De bestemming opslag account is hetzelfde als waar u de virtuele schijf om te uploaden. U moet ook opgeven of antwoord wordt gevraagd voor de extra parameters vereist door de `azure vm create` opdracht zoals virtueel netwerk, openbaar IP-adres, gebruikersnaam en SSH-sleutels. U kunt meer lezen over de [beschikbare parameters voor CLI Resource Manager](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Vereisten
U moet u de volgende stappen:

- **Linux-besturingssysteem is geïnstalleerd in het VHD-bestand** - een [Azure geviseerd Linux distributie](virtual-machines-linux-endorsed-distros.md) installeren (of informatie [voor distributies niet bekrachtigd](virtual-machines-linux-create-upload-generic.md)) tot een virtuele schijf in de indeling van de VHD. Meerdere hulpprogramma's bestaan om een VM en VHD te maken:
    - Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), bekommeren VHD gebruiken als indeling voor uw afbeeldingen. Indien nodig, kunt u met behulp van [een afbeelding omzet in](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) `qemu-img convert`.
    - U kunt ook Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] De nieuwere VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een VM maakt, geeft u VHD als de indeling. Indien nodig, kunt u omzetten VHDX schijven VHD via [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure ondersteunt bovendien geen dynamische VHD's, uploaden, dus u deze schijven converteren naar statische VHD's moet voordat u uploadt. Dynamische schijven converteren tijdens het uploaden naar Azure kunt u hulpprogramma's zoals [Azure VHD-hulpprogramma's voor gaan](https://github.com/Microsoft/azure-vhd-utils-for-go) .

- Gemaakt op basis van uw aangepaste installatiekopie VMs moeten zich bevinden op dezelfde rekening als de afbeelding zelf opslag
    - Een opslag- en container voor het opslaan van uw aangepaste installatiekopie en de gemaakte VMs maken
    - Nadat u alle uw VMs hebt gemaakt, kunt u veilig uw afbeelding verwijderen

Zorg dat u [De CLI Azure](../xplat-cli-install.md) ingelogd en Resource Manager-modus:

```bash
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Voorbereiden van het image te uploaden

Azure ondersteunt diverse Linux-distributies (Zie [Geviseerd distributies](virtual-machines-linux-endorsed-distros.md)). De volgende artikelen begeleiden u bij het voorbereiden van de diverse Linux-distributies die worden ondersteund op Azure:

- **[Op basis van centOS verdelingen](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - niet-geviseerd verdelingen](virtual-machines-linux-create-upload-generic.md)**

Zie ook de **[Opmerkingen bij de installatie Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** voor meer algemene tips over het voorbereiden van images Linux van Azure.

> [AZURE.NOTE] De [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) is van toepassing op VMs waarop Linux wordt uitgevoerd alleen als een van de geviseerde distributies wordt gebruikt met de configuratiegegevens van de opgegeven onder versies ondersteund in [Linux op Azure-Endorsed verdelingen](virtual-machines-linux-endorsed-distros.md).


## <a name="create-a-resource-group"></a>Een resourcegroep maken
Resourcegroepen samenbrengen logisch de Azure bronnen ter ondersteuning van uw virtuele machines, zoals virtuele netwerken en opslag. Lees meer over [Azure resourcegroepen hier](../azure-resource-manager/resource-group-overview.md). Voordat u uw aangepaste installatiekopie uploaden en VMs maakt, moet u eerst een resourcegroep maken. 

In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `WestUS` locatie:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Maak een account voor opslag
VMs worden opgeslagen als BLOB's pagina binnen een opslag-account. Lees meer over [hier Azure blob-opslag](../storage/storage-introduction.md#blob-storage). U maakt een account opslag voor uw aangepaste schijfkopie en VMs. Een VMs die u van uw aangepaste installatiekopie maken moeten op dezelfde rekening opslag als afbeelding.

Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount` in de resourcegroep hebt gemaakt:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Lijst opslag account sleutels
Azure genereert twee 512 bits toegangstoetsen voor elke rekening voor opslag. Deze toegangstoetsen worden gebruikt bij het verifiëren van de opslag-account, zoals schrijf-bewerkingen uitvoeren. Meer informatie over het [beheren van toegang tot opslag hier](../storage/storage-create-storage-account.md#manage-your-storage-account). U kunt weergeven met access-toetsen de `azure storage account keys list` opdracht.

Bekijk de toegangstoetsen voor de opslag-account die u hebt gemaakt:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

De uitvoer is vergelijkbaar met:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Maak een notitie van `key1` als u deze gebruiken wilt voor interactie met uw account voor opslag in de volgende stappen.

## <a name="create-a-storage-container"></a>Maak een opslag container
Op dezelfde manier dat u verschillende mappen voor het indelen van uw lokale bestandssysteem logisch maakt maken u containers in een opslag-account voor het ordenen van uw virtuele schijven en afbeeldingen. Een opslag-account kan een willekeurig aantal containers bevatten. 

Het volgende voorbeeld wordt een container met de naam `myimages`, geven de toegangstoets verkregen in de vorige stap (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>VHD uploaden
Nu kunt u daadwerkelijk uw aangepaste installatiekopie uploaden. Als met alle virtuele schijven gebruikt door VMs, u uploaden en opslaan van uw aangepaste schijfkopie als een blob pagina.

Geef uw toegangssleutel, de container die u hebt gemaakt in de vorige stap en het pad naar de aangepaste installatiekopie op uw lokale computer:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>VM van een aangepaste installatiekopie maken
Wanneer u uw aangepaste schijfkopie VMs maakt, geef de URI op de afbeelding van de schijf. Ervoor te zorgen dat de bestemming opslag account overeenkomsten waarin de aangepaste installatiekopie is opgeslagen. U kunt uw VM met de Azure CLI of bronnenbeheerder JSON-sjabloon maken.


### <a name="create-a-vm-using-the-azure-cli"></a>Maak een met behulp van de CLI Azure VM
U geeft de `--image-urn` parameter met de `azure vm create` opdracht om te verwijzen naar uw aangepaste installatiekopie. Ervoor te zorgen dat `--storage-account-name` overeenkomt met de opslag account waar u de aangepaste installatiekopie is opgeslagen. U hoeft niet dezelfde container als de aangepaste schijfkopie gebruiken voor het opslaan van uw VMs. Zorg ervoor dat u andere containers op dezelfde manier als de eerdere stappen voordat u de aangepaste schijf afbeeldingen uploadt.

Het volgende voorbeeld wordt een VM met de naam `myVM` uit uw aangepaste schijfkopie:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Nog steeds moet u opgeven of het antwoord wordt gevraagd voor de extra parameters vereist door de `azure vm create` opdracht zoals virtueel netwerk, openbaar IP-adres, gebruikersnaam en SSH-sleutels. Meer informatie over de [beschikbare parameters voor CLI Resource Manager](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Een VM met behulp van een JSON-sjabloon maken
Azure Resource Manager-sjablonen zijn JavaScript Object Notation (JSON) bestanden die u wilt bouwen milieu definieert. De sjablonen zijn onderverdeeld verschillende providers zoals compute of netwerk. U kunt bestaande sjablonen gebruiken of uw eigen schrijven. Meer informatie over het [gebruik van Resource Manager en sjablonen](../azure-resource-manager/resource-group-overview.md).

In de `Microsoft.Compute/virtualMachines` provider van de sjabloon die u hebt een `storageProfile` het knooppunt dat de configuratiegegevens voor uw VM bevat. Zijn de twee belangrijkste parameters voor het bewerken van de `image` en `vhd` URI's die verwijzen naar uw aangepaste schijfkopie en uw nieuwe VM virtuele schijf. Hier volgt een voorbeeld van de JSON voor het gebruik van een aangepaste schijfkopie:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

U kunt [deze bestaande](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) sjabloon een VM uit een aangepaste afbeelding maken of meer informatie over het [maken van uw eigen sjablonen Azure Resource Manager](../resource-group-authoring-templates.md). 

Wanneer u een sjabloon hebt, maakt u uw VMs met behulp van de `azure group deployment create` opdracht. Geef de URI van de JSON-sjabloon met de `--template-uri` parameter:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Als u een JSON-bestand dat lokaal is opgeslagen op uw computer hebt, kunt u de `--template-file` parameter in plaats daarvan:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt voorbereid en de aangepaste virtuele schijf geüpload, kunt u meer informatie over het [gebruik van Resource Manager en sjablonen](../azure-resource-manager/resource-group-overview.md). U kunt ook [een gegevensschijf](virtual-machines-linux-add-disk.md) toevoegen aan uw nieuwe VMs. Als u toepassingen op uw VMs waartoe u toegang moet hebben, moet u poorten moet [openen en eindpunten](virtual-machines-linux-nsg-quickstart.md).