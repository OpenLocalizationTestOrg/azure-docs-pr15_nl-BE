<properties
   pageTitle="Een Linux VM maken met meerdere NIC's | Microsoft Azure"
   description="Informatie over het maken van een Linux VM met meerdere NIC's die zijn gekoppeld met behulp van de sjablonen Azure CLI of Resource Manager."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Een Linux VM maken met meerdere NIC 's
U kunt een virtuele machine (VM) maken in Azure met meerdere virtuele netwerkinterfaces (NIC's) is gekoppeld. Een algemeen scenario zou zijn om met verschillende subnetten voor front-end en back-end-verbinding of een netwerk dat is toegewezen aan een controle of een back-up oplossing. In dit artikel vindt snel opdrachten voor het maken van een VM met meerdere NIC's is gekoppeld. Voor gedetailleerde informatie, met inbegrip van het maken van meerdere NIC's binnen uw eigen scripts Bash meer informatie over het [implementeren van meerdere NIC's VMs](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschillende [formaten VM](virtual-machines-linux-sizes.md) ondersteunen een wisselend aantal NIC's, zodat het formaat van uw VM dienovereenkomstig.

>[AZURE.WARNING] Wanneer u een VM maken - u de NIC's aan een bestaande VM toevoegen kunt, moet u meerdere netwerkkaarten koppelen. U kunt [een VM op basis van de oorspronkelijke virtuele schijven maken](virtual-machines-linux-copy-vm.md) en meerdere NIC's implementatie van de VM te maken.

## <a name="quick-commands"></a>Snelle opdrachten
Zorg dat u de [Azure CLI](../xplat-cli-install.md) ingelogd en Resource Manager-modus:

```bash
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myVM`.

Maak eerst een resourcegroep. In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `WestUS` locatie:

```bash
azure group create myResourceGroup -l WestUS
```

Maak een account opslag voor uw VMs. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Een virtueel netwerk om uw VMs om verbinding te maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` met het voorvoegsel van een adres van `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Maak twee subnetten virtueel netwerk - één voor front-verkeer en één voor verkeer van de back-end. In het volgende voorbeeld maakt u twee subnetten, met de naam `mySubnetFrontEnd` en `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Twee NIC's, één Netwerkkaart op de front-end-subnet en één NIC koppelen aan de back-end-subnet maken. In het volgende voorbeeld maakt u twee NIC's met de naam `myNic1` en `myNic2`, en koppelt u deze aan de subnetten:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Ten slotte maken uw VM, koppelen van de twee NIC's die u eerder hebt gemaakt. Het volgende voorbeeld wordt een VM met de naam `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Maken van meerdere NIC's met Azure CLI
Als u een met behulp van de CLI Azure VM eerder hebt gemaakt, moeten de snelle opdrachten bekend zijn. Het proces is hetzelfde NIC met één of meerdere NIC's te maken. Hier vindt u meer informatie over het [implementeren van meerdere NIC's met behulp van de CLI Azure](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), inclusief scripts, het proces van het maken van de NIC's doorlopen.

In het volgende voorbeeld maakt u twee NIC's met de naam `myNic1` en `myNic2`, met een Netwerkkaart verbinding te maken met elk subnet:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Maakt u gewoonlijk ook een [Beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) of [de belasting voor documentconversies](../load-balancer/load-balancer-overview.md) beheer en het verkeer verdelen over het VMs. Nogmaals, de opdrachten hetzelfde zijn als u werkt met meerdere netwerkkaarten. Het volgende voorbeeld wordt een netwerk beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

De NIC's verbinden met het netwerk Security Group `azure network nic set`. In het volgende voorbeeld wordt `myNic1` en `myNic2` met `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Bij het maken van de VM, nu u meerdere netwerkkaarten. In plaats daarvan met `--nic-name` voor een enkele NIC in plaats daarvan u `--nic-names` en een door komma's gescheiden lijst van NIC's bieden. U moet ook Wees voorzichtig wanneer u de grootte van de VM selecteren. Zijn er beperkingen voor het totale aantal NIC's die u aan een VM toevoegen kunt. Lees meer over [Linux VM formaten](virtual-machines-linux-sizes.md). In het volgende voorbeeld ziet u hoe meerdere NIC's en vervolgens een VM-grootte die ondersteunt het gebruik van meerdere NIC's op te geven (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Meerdere NIC's met behulp van bronbeheer sjablonen maken
Declaratieve JSON bestanden Azure Resource Manager-sjablonen gebruiken voor het definiëren van uw omgeving. Hier vindt u een [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Resource Manager-sjablonen kunnen u meerdere exemplaren van een resource tijdens de implementatie, zoals het maken van meerdere NIC's maken. U *exemplaar* geeft u het aantal exemplaren te maken:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Meer informatie over het [maken van meerdere exemplaren met *kopiëren*](../resource-group-create-multiple.md). 

U kunt ook een `copyIndex()` om vervolgens een nummer toevoegen aan de naam van een resource, kunt u voor het maken van `myNic1`, `myNic2`, enz. Hier volgt een voorbeeld van de indexwaarde toe te voegen:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Hier vindt u een volledig voorbeeld van [meerdere NIC's met behulp van bronbeheer sjablonen maken](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat [Linux VM formaten](virtual-machines-linux-sizes.md) controleren bij het maken van een VM met meerdere netwerkkaarten. Let op het maximum aantal NIC's elke grootte VM ondersteunt. 

Houd er rekening mee dat u geen extra NIC's toevoegen aan een bestaande VM, moet u de NIC's bij het implementeren van de VM. Wees voorzichtig bij het plannen van de distributie om te controleren of de vereiste verbinding met het netwerk vanaf het begin.