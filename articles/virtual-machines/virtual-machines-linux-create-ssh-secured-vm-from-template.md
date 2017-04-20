<properties
    pageTitle="Maak een Linux VM met behulp van een sjabloon Azure | Microsoft Azure"
    description="Maak een Linux VM op Azure met behulp van een sjabloon Azure Resource Manager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Maak een Linux VM met behulp van een sjabloon Azure

In dit artikel wordt beschreven hoe u snel een virtuele Linux Machine op Azure implementeren met behulp van een sjabloon Azure.  Het artikel moet:

- een Azure-account ([gratis proberen](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure CLI](../xplat-cli-install.md) vastgelegd met `azure login`.

- de modus Azure CLI _moet in_ Azure Resource Manager `azure config mode arm`.

U kunt ook snel een sjabloon Linux VM implementeren met behulp van de [portal Azure](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Overzicht snelle opdracht

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Gedetailleerde scenario

Sjablonen kunt u met de instellingen die u wilt aanpassen tijdens het starten, zoals gebruikersnamen en hostnamen VMs op Azure maken. Voor dit artikel, zijn we een Azure-sjabloon met behulp van een Ubuntu VM samen met een netwerk security group (NSG) starten met de poort open 22 voor SSH.

Azure Resource Manager-sjablonen zijn JSON bestanden die kunnen worden gebruikt voor eenvoudige eenmalige taken als het starten van een Ubuntu VM zoals gedaan in dit artikel.  Azure sjablonen kunnen ook worden gebruikt om complexe Azure configuraties van de hele omgeving werkt, bijvoorbeeld een stapel van de implementatie testen, dev of productie samen te stellen.

## <a name="create-the-linux-vm"></a>De Linux VM maken

In het volgende voorbeeld ziet u hoe aanroepen `azure group create` een resourcegroep maken en implementeren van een beveiligde SSH Linux VM op hetzelfde moment met behulp van [deze sjabloon Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Vergeet niet dat in uw voorbeeld moet u de namen die uniek voor uw omgeving zijn. In dit voorbeeld wordt `myResourceGroup` als de naam van de resource, en `myVM` als de naam van de VM.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

De uitvoer ziet er bijvoorbeeld de volgende uitvoer blokkeren:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Dit voorbeeld gebruikt een VM met de `--template-uri` parameter.  U kunt ook downloaden of lokaal een sjabloon maken en doorgeven van de sjabloon met behulp van de `--template-file` parameter een pad naar het sjabloonbestand als argument. De CLI Azure vraagt u om de parameters die nodig zijn door de sjabloon.

## <a name="next-steps"></a>Volgende stappen

Zoek in de [galerie met sjablonen](https://azure.microsoft.com/documentation/templates/) om te ontdekken welke app frameworks vervolgens implementeren.
