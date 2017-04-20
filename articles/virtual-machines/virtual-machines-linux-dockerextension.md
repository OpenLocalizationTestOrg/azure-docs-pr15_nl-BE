<properties
   pageTitle="Met de extensie Azure Docker VM | Microsoft Azure"
   description="Informatie over het implementeren van een omgeving Docker in Azure met behulp van bronbeheer sjablonen snel en veilig met de extensie Docker VM."
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
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Een omgeving van Docker in met de extensie Docker VM Azure maken
Docker is een populaire container management en imaging platform waarmee u snel werken met containers op Linux (en Windows ook). In Azure zijn er verschillende manieren aan uw behoeften kunt u Docker implementeren. In dit artikel de nadruk op de extensie Docker VM en Azure Resource Manager-sjablonen. 

Raadpleeg de volgende artikelen voor meer informatie over de verschillende implementatiemethoden Docker Machine en Azure Container Services, waaronder:

- Snel prototype een app, kunt u één Docker host [Docker Machine](./virtual-machines-linux-docker-machine.md)gebruiken.
- Voor grotere en meer stabiele omgeving, kunt u de uitbreiding Azure Docker VM, die ook ondersteuning voor [Docker opstellen](https://docs.docker.com/compose/overview/) voor het genereren van consistente container implementaties. In dit artikel details met de extensie Azure Docker VM.
- Bouwen van productie-ready, schaalbare omgevingen die extra hulpmiddelen voor planning en beheer bieden, kunt u een [cluster Docker Swarm op Azure Container Services](../container-service/container-service-deployment.md)implementeren.


## <a name="azure-docker-vm-extension-overview"></a>Overzicht van de extensie Azure VM Docker
De extensie Azure Docker VM installeert en configureert de daemon Docker Docker client en opstellen van Docker in uw Linux virtual machine (VM). Met de extensie Azure Docker VM, beschikt u over meer functies dan gewoon met behulp van Docker Machine of het maken van de host Docker en controle. Deze extra functies, zoals het [Opstellen van Docker](https://docs.docker.com/compose/overview/), moeten de extensie Azure Docker VM is geschikt voor de meer robuuste ontwikkelaar of productieomgevingen.

Azure Resource Manager sjablonen definiëren de volledige structuur van uw omgeving. Sjablonen kunnen u bronnen als de host Docker VMs, opslag, op rollen gebaseerde toegangscontrole RBAC () en diagnostische gegevens maken en configureren. U kunt deze sjablonen om extra implementaties op consistente wijze maken opnieuw gebruiken. Zie [Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md)voor meer informatie over bronbeheer Azure en sjablonen. 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Een sjabloon met de extensie Azure Docker VM implementeren
We gebruiken een quickstart-template voor het maken van een Ubuntu VM met de extensie Azure Docker VM installeren en configureren van de host Docker. U kunt de sjabloon hier bekijken: [eenvoudige implementatie van een Ubuntu VM met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

U moet de [Meest recente Azure CLI](../xplat-cli-install.md) geïnstalleerd en geregistreerd bij het gebruik van de modus Resource Manager als volgt:

```
azure config mode arm
```

Distribueer de sjabloon met behulp van de CLI Azure, de URI van de sjabloon op te geven. In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `WestUS` locatie. Uw eigen Resourcegroepnaam en locatie als volgt gebruiken:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Beantwoord de vragen uw opslag account een naam, een gebruikersnaam en wachtwoord, en bieden een DNS-naam. De uitvoer is vergelijkbaar met het volgende voorbeeld:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Het CLI Azure geeft u de opdrachtprompt nadat er maar een paar seconden, maar de host Docker is nog steeds worden gemaakt en geconfigureerd door de extensie Azure Docker VM. Duurt het enkele minuten duren voordat de installatie te voltooien. U kunt details bekijken over de Docker host status met behulp van de `azure vm show` opdracht.

Het volgende voorbeeld wordt de status van de VM met de naam `myDockerVM` (de standaardnaam van de sjabloon - niet deze naam wijzigen) in de bronnengroep met de naam `myResourceGroup`. Geef de naam van de resourcegroep die u in de vorige stap hebt gemaakt:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

De uitvoer van de `azure vm show` opdracht is vergelijkbaar met het volgende voorbeeld:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Bij de bovenkant van de output ziet u de `ProvisioningState` van de VM. Wanneer dit wordt `Succeeded`, de implementatie is voltooid en kunt u SSH voor VM.

Tegen het einde van de uitvoer van de `FQDN` de FQDN-naam van de host Docker wordt weergegeven. Deze FQDN is wat u kunt SSH naar uw host Docker in de resterende stappen.


## <a name="deploy-your-first-nginx-container"></a>De eerste container met nginx implementeren
Nadat de implementatie is voltooid, SSH naar uw nieuwe Docker host van uw lokale computer. Voer uw eigen gebruikersnaam en de FQDN-naam als volgt:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Nadat u bent aangemeld met de host Docker, voeren we een container nginx:

```
sudo docker run -d -p 80:80 nginx
```

De uitvoer is vergelijkbaar met het volgende voorbeeld de nginx afbeelding gedownload en een container is gestart:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Controleer de status van de containers op de host Docker als volgt uitgevoerd:

```
sudo docker ps
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waaruit blijkt dat de container nginx is uitgevoerd en TCP-poorten 80 en 443 en doorgestuurd:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Als u wilt uw container in actie zien, opent u een webbrowser en voert u de FQDN-naam van de host Docker:

![Actieve ngnix container](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure VM Docker extensie sjabloonverwijzing
In het vorige voorbeeld maakt gebruik van een bestaande sjabloon voor quickstart. U kunt ook de extensie Azure Docker VM implementeren met uw eigen sjablonen Resource Manager. Voeg hiertoe de volgende Resource Manager-sjablonen, definiëren de `vmName` van de VM op de juiste manier:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Hier vindt u meer gedetailleerde stapsgewijze instructies over het gebruik van sjablonen Resource Manager door te lezen [Overzicht Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).


## <a name="next-steps"></a>Volgende stappen
Wilt u mogelijk [de daemon Docker TCP-poort configureren](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), [Docker beveiliging](https://docs.docker.com/engine/security/security/)begrijpen of implementeren van containers met [Docker opstellen](https://docs.docker.com/compose/overview/). Zie de [GitHub project](https://github.com/Azure/azure-docker-extension/)voor meer informatie over de uitbreiding Azure Docker VM zelf.

Lees meer informatie over de extra implementatieopties Docker in Azure:

- [Docker Machine met de Azure stuurprogramma gebruiken](./virtual-machines-linux-docker-machine.md)  
- [Aan de slag met Docker en opstellen voor het definiëren en uitvoeren van een toepassing met meerdere container op een Azure virtuele machine](virtual-machines-linux-docker-compose-quickstart.md).
- [Een cluster Azure Container Service implementeren](../container-service/container-service-deployment.md)
