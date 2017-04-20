
<properties
   pageTitle="Maak een volledige Linux-omgeving met behulp van de CLI Azure | Microsoft Azure"
   description="Opslag, een VM Linux een virtueel netwerk en subnet, een taakverdeling, een NIC, een openbaar IP-adres en een beveiligingsgroep netwerk van de grond omhoog met behulp van de CLI Azure maken."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Maak een volledige Linux-omgeving met behulp van de CLI Azure

In dit artikel bouwen we een eenvoudig netwerk met een load balancer en een paar VMs die handig voor de ontwikkeling en eenvoudige computing zijn. Bekijken we het proces van opdracht tot twee werkdagen, veilige Linux VMs-waarmee u verbinding overal op het Internet maken kunt. U kunt vervolgens op verplaatsen naar de meer complexe netwerken en omgevingen.

Langs de manier leert u de afhankelijkheidshiërarchie dat het implementatiemodel Resource Manager u biedt en over hoeveel energie er biedt. Nadat u hoe het systeem is gebouwd zien, u kunt opnieuw maken veel sneller met behulp van [Bronbeheer Azure-sjablonen](../resource-group-authoring-templates.md). Ook nadat u hebt geleerd hoe de onderdelen van uw omgeving in elkaar passen, makkelijker maken van sjablonen automatiseren.

De omgeving bevat:

- Twee VMs binnen een set beschikbaarheid.
- Een taakverdeling met een regel-taakverdeling op poort 80.
- Network group (NSG) regels voor uw VM beveiligen tegen ongewenst verkeer.

![Overzicht van de Basic-omgeving](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Om deze aangepaste omgeving maakt, moet u de meest recente [Azure CLI](../xplat-cli-install.md) modus Resource Manager (`azure config mode arm`). U moet ook een hulpmiddel bij het parseren van JSON. In dit voorbeeld wordt de [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende gegevens in de sectie snel de base opdrachten een VM uploaden naar Azure. Meer gedetailleerde informatie en de context voor elke stap kan worden gevonden in de rest van het document, begint [hier](#detailed-walkthrough).

Zorg dat u [De CLI Azure](../xplat-cli-install.md) ingelogd en Resource Manager-modus:

```bash
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen zijn `myResourceGroup`, `mystorageaccount`, en `myVM`.

De resourcegroep maken. In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `westeurope` locatie:

```bash
azure group create -n myResourceGroup -l westeurope
```

Controleer of de bronnengroep met behulp van de JSON parser:

```bash
azure group show myResourceGroup --json | jq '.'
```

De opslag-account maken. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount` (de naam van de opslag moet uniek zijn, dus Geef uw eigen unieke naam):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Controleer of de account van de opslag met behulp van de JSON parser:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Het virtuele netwerk maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Een subnet maken. Het volgende voorbeeld wordt een subnet genaamd `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Controleer of het virtuele netwerk en subnet met behulp van de JSON parser:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Maak een openbare IP-adres. Het volgende voorbeeld wordt een openbaar IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns` (de DNS-naam moet uniek zijn, dus Geef uw eigen unieke naam):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Maak de taakverdeling. Het volgende voorbeeld wordt een taakverdeling met de naam `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Maak een front-end-IP-adresgroep voor de taakverdeling en het openbare IP koppelen. Het volgende voorbeeld wordt een front-end-IP-toepassingen met de naam `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Maken van de back-end-IP-adresgroep voor de taakverdeling. In het volgende voorbeeld maakt u een back-end-IP-toepassingen met de naam `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Inkomende SSH-netwerk adres netwerkadresomzetting (NAT) regels maken voor de taakverdeling. In het volgende voorbeeld maakt u twee regels voor load balancer, `myLoadBalancerRuleSSH1` en `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Maak het web binnenkomende NAT regels voor de taakverdeling. Het volgende voorbeeld wordt een load balancer regel met de naam`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

De load balancer gezondheid sonde maken. Het volgende voorbeeld wordt een TCP-sonde met de naam `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

De taakverdeling, IP-adresgroepen en NAT-regels controleren met behulp van de JSON parser:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

De eerste netwerkkaart (NIC) maken. Vervang de `#####-###-###` secties met uw eigen Azure abonnement-ID. Abonnement-ID wordt vermeld in de uitvoer van `jq` bij het onderzoek van de bronnen die u maakt. U kunt ook uw abonnements-ID met bekijken `azure account list`. 

Het volgende voorbeeld wordt een NIC met de naam `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Maak de tweede NIC. Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Controleer of de twee NIC's met behulp van de JSON parser:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Maak de groep netwerk. Het volgende voorbeeld wordt een netwerk beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Twee regels voor binnenkomende verbindingen voor de netwerk-beveiligingsgroep toevoegen. In het volgende voorbeeld maakt u twee regels, `myNetworkSecurityGroupRuleSSH` en `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Controleer of de netwerk-beveiligingsgroep en regels voor binnenkomende verbindingen met behulp van de JSON parser:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

De beveiligingsgroep netwerk verbinden met twee netwerkadapters:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Maak de beschikbaarheid. Het volgende voorbeeld wordt de beschikbaarheid van een set met de naam `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

De eerste Linux VM maken. Het volgende voorbeeld wordt een VM met de naam `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Maak de tweede Linux VM. Het volgende voorbeeld wordt een VM met de naam `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Gebruik de JSON parser te controleren dat alles wat is gemaakt:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

De nieuwe omgeving exporteren naar een sjabloon om snel nieuwe exemplaren opnieuw te maken:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Gedetailleerde scenario
De gedetailleerde stappen die volgen wordt uitgelegd wat elke opdracht doet voor het opzetten van uw omgeving. Deze concepten zijn nuttig bij het samenstellen van uw eigen aangepaste omgevingen voor ontwikkeling of productie.

Zorg dat u [De CLI Azure](../xplat-cli-install.md) ingelogd en Resource Manager-modus:

```bash
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen zijn `myResourceGroup`, `mystorageaccount`, en `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Resourcegroepen maken en kies distributie locaties

Azure resourcegroepen zijn logische implementatie entiteiten met configuratie-informatie en metagegevens om het logische beheer van implementaties van de resource. In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `westeurope` locatie:

```bash
azure group create --name myResourceGroup --location westeurope
```

Uitvoer:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Maak een account voor opslag

U moet accounts opslag voor uw VM-schijven en schijven aanvullende gegevens die u wilt toevoegen. Opslag accounts maakt u bijna onmiddellijk nadat u resourcegroepen maken.

Hier gebruiken we de `azure storage account create` opdracht geven de locatie van de rekening, de resourcegroep die besturingselementen en het type van de gewenste ondersteuning voor opslag. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Uitvoer:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Onze resourcegroep bekijken met behulp van de `azure group show` opdracht, we gebruiken de [jq](https://stedolan.github.io/jq/) samen met de `--json` Azure CLI-optie. (U kunt **jsawk** of een taal-bibliotheek die u wilt parseren de JSON.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Als u wilt analyseren de opslag-account met behulp van de CLI, moet u eerst de accountnamen en toetsen instellen. De naam van de account van de opslag in het volgende voorbeeld vervangen door een naam die u kiest:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

U kunt vervolgens uw opslaggegevens gemakkelijk bekijken:

```bash
azure storage container list
```

Uitvoer:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en subnet maken

Vervolgens gaat u nodig hebt voor het maken van een virtueel netwerk uitgevoerd in Azure en een subnet waarin u uw VMs kunt maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` met de `192.168.0.0/16` -adresprefix:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Uitvoer:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Nogmaals, we gebruiken de optie--json van `azure group show` en **jq** om te zien hoe we onze middelen bouwen. We hebben nu een `storageAccounts` resource en een `virtualNetworks` bron.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Nu maken we een subnet in de `myVnet` virtueel netwerk waarop het VMs worden geïmplementeerd. We gebruiken de `azure network vnet subnet create` command, samen met de middelen die we al hebben gemaakt: de `myResourceGroup` groep en de `myVnet` virtueel netwerk. In het volgende voorbeeld voegen we het subnet genaamd `mySubnet` met het voorvoegsel van subnet adres van `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Uitvoer:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Omdat het subnet logisch binnen het virtuele netwerk is, zoekt we het subnet met een andere opdracht. De opdracht die we gebruiken is `azure network vnet show`, maar wij onderzoeken de JSON-uitvoer met behulp van **jq**.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Uitvoer:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Maak een openbaar IP-adres (PIP)

Nu maken we het openbare IP-adres (PIP) die we aan de verdeling van de belasting toewijzen. Hiermee kunt u verbinding maken met uw VMs vanaf het Internet met behulp van de `azure network public-ip create` opdracht. Omdat de standaard-adres dynamisch is, maken we een benoemde DNS-vermelding in het domein van **cloudapp.azure.com** met behulp van de `--domain-name-label` optie. Het volgende voorbeeld wordt een openbaar IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns`. Als de DNS-naam moet uniek zijn, biedt uw eigen unieke DNS-naam:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Uitvoer:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Het openbare IP-adres is ook een bron op het hoogste niveau, zodat u met zien kunt `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

U kunt meer informatie over resource, zoals de volledig gekwalificeerde domeinnaam (FQDN) van het subdomein, met behulp van de volledige onderzoeken `azure network public-ip show` opdracht. De openbare IP-adresbron logisch is toegewezen, maar als een specifiek adres nog niet is toegewezen. Als u een IP-adres, gaat u moet een taakverdeling die we nog niet hebt gemaakt.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Uitvoer:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Een load balancer en IP-adresgroepen maken
Wanneer u een load balancer maakt, kunt u verkeer verdelen over meerdere VMs. Ook biedt redundantie voor uw toepassing door het uitvoeren van meerdere VMs die op aanvragen van gebruikers bij onderhoud of zware belasting reageren. Het volgende voorbeeld wordt een taakverdeling met de naam `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Uitvoer:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Onze taakverdeling is vrij leeg, dus laten we enkele IP-adresgroepen maken. Wij willen voor onze taakverdeling - één voor de voorkant en één voor de back-end twee IP-adresgroepen maken. De front-IP-adresgroep is openbaar zichtbaar. Het is ook de locatie waar we de ogen die we eerder hebben gemaakt toewijzen. Vervolgens gebruiken we de back-end-toepassingen als locatie voor onze VMs verbinding maken met. Op die manier het verkeer door de verdeling van de belasting op het VMs kan stromen.

Eerst gaan we onze front-IP-adresgroep maken. Het volgende voorbeeld wordt een front-end-toepassingen met de naam `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Uitvoer:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

We gebruikt de `--public-ip-name` switch doorgeven de `myPublicIP` die we eerder hebben gemaakt. Het openbare IP-adres toewijzen aan de taakverdeling, kunt u uw VMs bereiken via het Internet.

Vervolgens gaan we onze tweede IP-adresgroep maken voor onze back-end-verkeer. In het volgende voorbeeld maakt u een back-end-toepassingen met de naam `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Uitvoer:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Kunnen we zien hoe onze taakverdeling doet door te kijken met `azure network lb show` en onderzoek van de JSON-uitvoer:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Uitvoer:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Taakverdeling NAT regels maken
Als u verkeer dat via onze taakverdeling, moeten we netwerkadres netwerkadresomzetting (NAT) regels maken die binnenkomend of uitgaand acties opgeven. U kunt opgeven van het protocol te gebruiken en vervolgens externe poorten toewijzen aan interne poorten naar wens. Voor onze omgeving, zullen we enkele regels maken die SSH toestaan via onze taakverdeling aan onze VMs. U stelt TCP-poorten 4222 en 4223 om te leiden via TCP-poort 22 op onze VMs (die we later maken). Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleSSH1` TCP-poort 4222 toewijzen aan poort 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Uitvoer:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Herhaal de procedure voor de tweede regel voor NAT voor SSH. Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleSSH2` TCP-poort 4223 toewijzen aan poort 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Laten we ook doorgaan en maken van een NAT regel voor TCP-poort 80 voor webverkeer, aansluiting van de regel tot onze IP-adresgroepen. Als we de regel moet een IP-adresgroep, in plaats van een afzonderlijk, een regel aan onze VMs aansluiting aansluiten kunnen we toevoegen of verwijderen van VMs van de IP-adresgroep. De taakverdeling automatisch aangepast voor de doorstroming van het verkeer. Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleWeb` TCP-poort 80 voor poort 80 toewijzen:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Uitvoer:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Maak een load balancer gezondheid sonde

Gezondheid probe regelmatig controles op het VMs achter onze taakverdeling om te controleren of ze werken en reageren op aanvragen zoals gedefinieerd. Als dit niet het geval is, ze zijn verwijderd uit de bewerking om ervoor te zorgen dat gebruikers worden niet wordt doorgestuurd naar deze. Hier kunt u aangepaste controles voor de gezondheid sonde, intervallen en time-outwaarden. Zie voor meer informatie over gezondheid sondes, [Load Balancer sondes](../load-balancer/load-balancer-custom-probe-overview.md). Het volgende voorbeeld wordt een TCP gezondheid voortgangstekst benoemde `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Uitvoer:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

We hebben opgegeven, met een interval van 15 seconden voor onze controles. Wij kunnen een maximum van vier sondes (1 minuut) voordat de taakverdeling van oordeel is dat de host niet meer werkt missen.

## <a name="verify-the-load-balancer"></a>Controleer of de verdeling van de belasting
De load balancer-configuratie is nu klaar. Hier vindt u de stappen die hebt genomen:

1. Maakte een taakverdeling.
2. Vervolgens een IP-adresgroep front-end gemaakt en een openbaar IP-adres toegewezen.
3. Vervolgens kunt u een back-end-IP-adresgroep die VMs verbinding met maken kunnen gemaakt.
4. Daarna kunt u NAT regels waardoor SSH aan het VMs voor beheer, samen met een regel waarmee TCP-poort 80 voor onze web app gemaakt.
5. Ten slotte kunt u een sonde gezondheid geregeld wordt gecontroleerd of de VMs toegevoegd. Deze sonde gezondheid zorgt ervoor dat gebruikers niet proberen toegang te krijgen tot een VM die niet meer functioneren of inhoud.

Eens kijken wat de taakverdeling er nu uit:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Uitvoer:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Maak een NIC met Linux VM

NIC's zijn via programmacode beschikbaar, omdat hun gebruik kunt u regels toepassen. U kunt ook meerdere hebben. In de volgende `azure network nic create` opdracht u de Netwerkkaart met de load back-end-IP-groep aansluiten en koppelen aan de NAT regel SSH verkeer toestaan.
 
Vervang de `#####-###-###` secties met uw eigen Azure abonnement-ID. Abonnement-ID wordt vermeld in de uitvoer van `jq` bij het onderzoek van de bronnen die u maakt. U kunt ook uw abonnements-ID met bekijken `azure account list`. 

Het volgende voorbeeld wordt een NIC met de naam `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Uitvoer:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Door het onderzoeken van de resource direct kunt u de details bekijken. U bekijkt de resource met behulp van de `azure network nic show` opdracht:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Uitvoer:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

We maken nu de tweede Netwerkkaart, in aansluiting op onze back-end-IP-adresgroep opnieuw. Deze regel de tweede keer NAT SSH verkeer is toegestaan. Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Een netwerk-beveiligingsgroep en regels maken

Nu maken we een netwerkgroep voor beveiliging en de regels voor binnenkomende verbindingen die voor de toegang tot de NIC. Een beveiligingsgroep netwerk kan worden toegepast op een Netwerkkaart of een subnet. U definieert regels voor het bepalen van de doorstroming van het verkeer in en uit uw VMs. Het volgende voorbeeld wordt een netwerk beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

We voegen de binnenkomende regel voor de NSG toestaan binnenkomende verbindingen op poort 22 (voor ondersteuning van SSH). Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRuleSSH` TCP-poort 22 toestaan:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nu voegen we de binnenkomende regel voor de NSG toestaan binnenkomende verbindingen op poort 80 (voor ondersteuning van Internet-verkeer). Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRuleHTTP` waarmee TCP-poort 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] De binnenkomende regel is een filter voor binnenkomende netwerkverbindingen. In dit voorbeeld binden we de NSG aan de virtuele NIC VMs, wat betekent dat elke aanvraag naar poort 22 wordt doorgegeven aan de Netwerkkaart op onze VM. Deze binnenkomende regel over een netwerkverbinding is en niet over een eindpunt, dat is wat het is over klassieke implementaties. Als u een poort opent, moet u laat de `--source-port-range` ingesteld op '\*' (de standaardwaarde) te accepteren van binnenkomende aanvragen vanaf **de** poort aanvraagt. Poorten zijn meestal dynamisch.

## <a name="bind-to-the-nic"></a>Binden aan het NIC

De NIC's afhankelijk maken van de NSG. We moeten onze NIC's met onze groep netwerk verbinding te maken. Beide opdrachten aansluiten van onze NIC's uitvoeren:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Maak een set beschikbaarheid
Beschikbaarheid wordt help spread uw VMs in fout met betrekking tot domeinen en domeinen upgraden. We maken een instellen voor uw VMs beschikbaarheid. Het volgende voorbeeld wordt de beschikbaarheid van een set met de naam `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Fout met betrekking tot domeinen definiëren een groep van virtuele machines die een gemeenschappelijke bron- en stopcontact delen. Standaard worden de virtuele machines die worden geconfigureerd in uw set beschikbaarheid voor maximaal drie domeinen van fouttolerantie gescheiden. Het idee is een hardwareprobleem in één van de domeinen van deze fout heeft geen invloed op elke VM met uw app. VMs verdeeld Azure automatisch over de domeinen veroorzaakt bij het plaatsen van hen in een set van beschikbaarheid.

Domeinen upgraden geven aan groepen van virtuele machines en de onderliggende fysieke hardware die tegelijkertijd kan worden gestart. De volgorde waarin upgraden domeinen worden opgestart misschien niet opeenvolgende tijdens gepland onderhoud, maar slechts één upgrade opnieuw is gestart op een tijdstip. Nogmaals, Azure automatisch verdeelt de VMs voor upgraden domeinen bij het plaatsen van een site beschikbaar.

Meer informatie over het [beheren van de beschikbaarheid van VMs](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>De Linux VMs maken

U kunt de opslag- en netwerkbronnen ter ondersteuning van VMs Internet toegankelijk hebt gemaakt. Nu we de VMs maken en ze met een SSH-sleutel die geen wachtwoord zijn beveiligd. In dit geval gaan we een Ubuntu VM op basis van de meest recente LTS maken. Wij vinden dat informatie over afbeeldingen met behulp van `azure vm image list`, zoals beschreven in [het zoeken van afbeeldingen voor Azure VM](virtual-machines-linux-cli-ps-findimage.md).

We een afbeelding hebt geselecteerd met de opdracht `azure vm image list westeurope canonical | grep LTS`. In dit geval gebruiken we `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Voor het laatste veld geven we `latest` , zodat in de toekomst we altijd de meest recente versie. (De tekenreeks we gebruiken `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Deze stap is bekend voor iedereen die al is gemaakt door een ssh openbare en persoonlijke sleutel van rsa pair op Linux of Mac met behulp van **ssh-keygen-t rsa -b 2048**. Als er geen sleutelparen die een certificaat uw `~/.ssh` directory, kunt u deze maken:

- Automatisch, met behulp van de `azure vm create --generate-ssh-keys` optie.
- Handmatig, met behulp van [de instructies voor het zelf maken](virtual-machines-linux-mac-create-ssh-keys.md).

U kunt ook de `--admin-password` methode voor het verifiëren van de SSH-verbindingen nadat de VM is gemaakt. Deze methode is doorgaans minder veilig.

We maken de VM doordat onze bronnen en informatie, samen met de `azure vm create` opdracht:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Uitvoer:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Kunt u uw VM direct met behulp van de standaard SSH-sleutels. Zorg ervoor dat u de juiste poort opgeven omdat we via de taakverdeling geven. (Voor onze eerste VM we de NAT regel instellen voor het doorsturen van poort 4222 naar onze VM):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Uitvoer:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Ga verder en maak uw tweede VM op dezelfde manier:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

En u kunt nu de `azure vm show myResourceGroup myVM1` de opdracht om te controleren wat u hebt gemaakt. U werkt op dit moment uw Ubuntu VMs achter een load balancer in Azure die u inloggen kunt op alleen met uw SSH-sleutelpaar (omdat wachtwoorden worden uitgeschakeld). Nginx of httpd installeren, implementeren van een web app en het verkeer Zie stroom door de verdeling van de belasting op beide van het VMs.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Uitvoer:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Het milieu als een sjabloon exporteren
Nu dat u hebt gemaakt van deze omgeving, wat gebeurt er als u wilt een extra ontwikkelomgeving maken met dezelfde parameters of een productie-omgeving die overeenkomt met het? Resource Manager gebruikt de JSON-sjablonen die u de parameters voor uw omgeving definieert. U kunt samenstellen uit de hele omgeving door te verwijzen naar deze sjabloon JSON. U kunt [handmatig JSON-sjablonen maken](../resource-group-authoring-templates.md) of een bestaande omgeving om de JSON-sjabloon maken voor u exporteren:

```bash
azure group export --name myResourceGroup
```

Deze opdracht maakt u de `myResourceGroup.json` bestand in de huidige werkmap. Wanneer u een omgeving met deze sjabloon maakt, wordt u gevraagd alle resourcenamen van de, met inbegrip van de namen voor de taakverdeling, VMs of netwerkinterfaces. U kunt deze namen in het sjabloonbestand vullen door de `-p` of `--includeParameterDefaultValue` -parameter voor de `azure group export` opdracht die eerder is aangegeven. Bewerk de template JSON als de resourcenamen of [een parameters.json-bestand maken](../resource-group-authoring-templates.md#parameters) waarmee de resourcenamen wilt opgeven.

Een omgeving maken van uw sjabloon:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

U kunt Lees [meer over het implementeren van sjablonen](../resource-group-template-deploy-cli.md). Informatie over hoe u stapsgewijs omgevingen bijwerken, gebruikt u het parameterbestand en toegang tot sjablonen geven vanaf een enkele locatie.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te gaan werken met meerdere netwerkonderdelen en VMs. U kunt in deze omgeving monster bouwen van uw toepassing met behulp van de hoofdonderdelen die hier.
