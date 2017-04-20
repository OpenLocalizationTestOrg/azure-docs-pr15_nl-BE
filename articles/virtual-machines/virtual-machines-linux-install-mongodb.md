<properties
   pageTitle="MongoDB installeren op een Linux VM | Microsoft Azure"
   description="Informatie over het installeren en configureren van MongoDB op een virtuele Linux machine in het implementatiemodel Resource Manager met Azure."
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
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Installeren en configureren van MongoDB op een Linux VM in Azure
[MongoDB](http://www.mongodb.org) is een populaire open-source, krachtige NoSQL database. In dit artikel wordt beschreven hoe u te installeren en configureren van MongoDB op een Linux VM in Azure het implementatiemodel Resource Manager gebruiken. Voorbeelden hoe die gegevens worden weergegeven voor:

- [Handmatig installeren en configureren van een eenvoudige MongoDB-exemplaar](#manually-install-and-configure-mongodb-on-a-vm)
- [Een eenvoudige MongoDB-exemplaar met behulp van de sjabloon voor een Resource Manager maken](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Een complexe MongoDB een laptopgeheugen cluster met replica's wordt ingesteld met behulp van een Resource Manager-sjabloon maken](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Vereisten
Dit artikel is het volgende vereist:

- een Azure-account ([gratis proberen](https://azure.microsoft.com/pricing/free-trial/)).
- [Azure CLI](../xplat-cli-install.md) bent aangemeld`azure login`
- de Azure CLI *moet zijn* in de modus met behulp van bronbeheer Azure`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Handmatig installeren en configureren van MongoDB op een VM
MongoDB [installatie-instructies](https://docs.mongodb.com/manual/administration/install-on-linux/) voor waaronder Red Hat Linux-distros / CentOS, SUSE, Ubuntu en Debian. In het volgende voorbeeld wordt een `CoreOS` met een SSH-sleutel opgeslagen op VM `.ssh/azure_id_rsa.pub`. Beantwoord de vragen voor opslag, accountnaam en DNS-naam admin referenties:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Meld u aan voor het openbare IP-adres weergegeven aan het einde van de voorgaande stap voor VM maken VM:

```bash
ssh ops@40.78.23.145
```

Voor het toevoegen van de installatiebronnen voor MongoDB, maken een `yum` opslagplaatsbestand als volgt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Open het bestand MongoDB repo voor bewerken. Voeg de volgende regels:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Installeren met behulp van MongoDB `yum` als volgt:

```bash
sudo yum install -y mongodb-org
```

SELinux wordt standaard afgedwongen op CentOS afbeeldingen waardoor u toegang tot MongoDB. Hulpprogramma's voor Groepsbeleidsbeheer installeren en configureren van SELinux zodat MongoDB om te rijden op de standaard TCP-poort 27017 als volgt. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

De MongoDB-service als volgt starten:

```bash
sudo service mongod start
```

Controleer of de installatie van MongoDB door verbinding te maken met de lokale `mongo` client:

```bash
mongo
```

Nu de MongoDB instantie testen door gegevens toe te voegen en vervolgens te zoeken:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Desgewenst configureren MongoDB automatisch wordt gestart tijdens het opnieuw opstarten van een systeem:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Basic MongoDB exemplaar op CentOS met behulp van een sjabloon maken
Op een enkele CentOS VM met behulp van de volgende sjabloon Azure quickstart van Github, kunt u een eenvoudige MongoDB-exemplaar maken. Deze sjabloon wordt de extensie aangepast Script voor Linux toe te voegen een `yum` bibliotheek naar de zojuist gemaakte CentOS VM en installeer MongoDB.

- [Basic MongoDB exemplaar op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

In het volgende voorbeeld wordt een groep gemaakt met de naam `myResourceGroup` in de `WestUS` regio. Voer uw eigen waarden als volgt:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] De CLI Azure keert u terug naar de opdrachtprompt binnen een paar seconden om de implementatie te maken, maar de installatie en configuratie duurt enkele minuten. Controleer de status van de implementatie met `azure group deployment show myResourceGroup`, dienovereenkomstig invoeren van de naam van de resourcegroep. Wachten tot de `ProvisioningState` 'Geslaagd' bevat voordat u SSH voor VM.

Zodra de installatie voltooid, SSH voor VM is. Het IP-adres van uw VM met behulp van de `azure vm show` opdracht als volgt:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Aan het einde van de uitvoer, de `Public IP address` wordt weergegeven. SSH aan uw VM met het IP-adres van uw VM:

```bash
ssh ops@138.91.149.74
```

Controleer of de installatie van MongoDB door verbinding te maken met de lokale `mongo` client als volgt:

```bash
mongo
```

Nu de instantie testen door gegevens toe te voegen en als volgt zoeken:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Een complexe MongoDB een laptopgeheugen Cluster op CentOS met behulp van een sjabloon maken
U kunt een complexe MongoDB een laptopgeheugen cluster met behulp van de volgende sjabloon Azure quickstart van Github. Deze sjabloon volgt de [Aanbevolen procedures voor MongoDB een laptopgeheugen cluster](https://docs.mongodb.com/manual/core/sharded-cluster-components/) om redundantie en hoge beschikbaarheid. De sjabloon maakt twee shards, met drie knooppunten in de replicaset. Een config server replicaset drie knooppunten ook gemaakt, plus twee `mongos` router servers om te voorzien in de shards consistentie in toepassingen.

- [MongoDB Sharding Cluster op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Dit complexe MongoDB een laptopgeheugen cluster implementeert, moet meer dan 20 cores, die gewoonlijk is de standaard core telling per regio voor een abonnement. Open een verzoek voor Azure ondersteuning te verhogen van uw aantal core.

In het volgende voorbeeld wordt een groep gemaakt met de naam `myResourceGroup` in de `WestUS` regio. Voer uw eigen waarden als volgt:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] De CLI Azure keert u terug naar de opdrachtprompt binnen een paar seconden om de implementatie te maken, maar de installatie en configuratie kunnen overnemen een uur te voltooien. Controleer de status van de implementatie met `azure group deployment show myResourceGroup`, de naam van de resourcegroep dienovereenkomstig aan te passen. Wachten tot de `ProvisioningState` 'Geslaagd' bevat voordat u verbinding maakt met het VMs.


## <a name="next-steps"></a>Volgende stappen
In deze voorbeelden u het exemplaar MongoDB lokaal in de VM. Als u verbinden met de MongoDB-instantie van een andere VM of netwerk wilt, moet u ervoor zorgen de juiste [netwerk groep regels worden gemaakt](virtual-machines-linux-nsg-quickstart.md).

Zie [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)voor meer informatie over het gebruik van sjablonen maken.

De bronnenbeheerder Azure-sjablonen gebruiken de extensie aangepast Script downloaden en uitvoeren van scripts op uw VMs. Zie [werken met de extensie Azure aangepast Script Linux virtuele Machines](virtual-machines-linux-extensions-customscript.md)voor meer informatie.