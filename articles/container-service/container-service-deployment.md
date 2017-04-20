<properties
   pageTitle="Een cluster Azure Container Service implementeren | Microsoft Azure"
   description="Een cluster Azure Container Service implementeren met behulp van de portal Azure, Azure CLI of PowerShell."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Een cluster Azure Container Service implementeren

Azure Container Service biedt snelle implementatie van de populaire open source container clustering en orchestration oplossingen. U kunt via Azure Container Service DC/OS en Swarm Docker clusters met Azure Resource Manager sjablonen of de Azure portal implementeren. Implementatie van deze clusters met behulp van Azure Virtual Machine schaal Sets en clusters te profiteren van de aanbiedingen voor netwerk- en Azure. Als Azure Container Service, moet u een abonnement op Azure. Als u niet hebt, kunt klikt u aanmelden voor een [gratis proefperiode](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Dit document helpt u bij het implementeren van een cluster Azure Container Service via de [Azure portal](#creating-a-service-using-the-azure-portal), de [Azure opdrachtregelinterface (CLI)](#creating-a-service-using-the-azure-cli)en de [module Azure PowerShell](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Een service maken met behulp van de portal voor Azure

Aanmelden bij de Azure portal, selecteer **Nieuw**en de markt Azure **Azure Container Service**zoeken.

![Implementatie 1 maken](media/acs-portal1.png)  <br />

**Azure Container Service**te selecteren en klik op **maken**.

![Distributie 2 maken](media/acs-portal2.png)  <br />

Voer de volgende gegevens:

- **Gebruikersnaam**: dit is de gebruikersnaam die wordt gebruikt voor een rekening op elk van de virtuele machines en schaal van de virtuele machine wordt ingesteld in het cluster Azure Container Service.
- **Abonnement**: Selecteer een Azure-abonnement.
- **Resourcegroep**: een bestaande resourcegroep selecteren of een nieuwe maken.
- **Locatie**: Selecteer een Azure regio voor de implementatie van Azure Container Service.
- **Openbare SSH-sleutel**: de openbare sleutel die wordt gebruikt voor verificatie tegen Azure Container Service virtuele machines toevoegen. Het is heel belangrijk dat deze sleutel bevat geen regeleinden en wordt het voorvoegsel 'ssh-rsa' en de 'username@domain' postfix. Het bestand ziet er ongeveer zo uit: **ssh-rsa AAAAB3Nz... <>...... UcyupgH azureuser@linuxvm **. Zie voor instructies over het maken van sleutels voor Secure Shell (SSH), [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) en [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) -artikelen.

Klik op **OK** wanneer u gereed bent.

![Implementatie 3 maken](media/acs-portal3.png)  <br />

Selecteer een type Orchestration. De opties zijn:

- **DC-en OS**: een DC-en OS-cluster implementeert.
- **Swarm**: een Docker Swarm cluster implementeert.

Klik op **OK** wanneer u gereed bent.

![Implementatie 4 maken](media/acs-portal4.png)  <br />

Voer de volgende gegevens:

- **Master tellen**: het aantal modellen in het cluster.
- **Agent count**: voor Docker Swarm, dit is het aantal agenten in de schaal agent. DC-en OS is dit het aantal vertegenwoordigers in een set persoonlijke schaal. Daarnaast een set schaal openbaar gemaakt waarin een vooraf ingesteld aantal agenten. Het aantal agenten in deze schaal openbaar wordt bepaald door het aantal modellen zijn gemaakt in het cluster--één openbare agent voor één master en twee openbare agenten voor drie of vijf modellen.
- **Agent virtuele machine grootte**: de grootte van de agent virtuele machines.
- **DNS-voorvoegsel**: een wereld unieke naam die wordt gebruikt als voorvoegsel voor belangrijke delen van de FQDN-namen voor de service.

Klik op **OK** wanneer u gereed bent.

![Implementatie 5 maken](media/acs-portal5.png)  <br />

Klik op **OK** nadat de validatie service is voltooid.

![6 implementatie maken](media/acs-portal6.png)  <br />

Klik op **maken** om het implementatieproces te starten.

![7 deployment maken](media/acs-portal7.png)  <br />

Als u met de implementatie van de portal Azure pin hebt gekozen, kunt u de implementatiestatus zien.

![Implementatie 8 maken](media/acs-portal8.png)  <br />

Wanneer de installatie is voltooid, wordt het cluster Azure Container Service is klaar voor gebruik.

## <a name="create-a-service-by-using-the-azure-cli"></a>Een service maken met behulp van de CLI Azure

Een instantie van Azure Container Service maken met behulp van de opdrachtregel, moet u een abonnement op Azure. Als u niet hebt, kunt klikt u aanmelden voor een [gratis proefperiode](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). U moet ook hebben [geïnstalleerd](../xplat-cli-install.md) en [geconfigureerd](../xplat-cli-connect.md) op de CLI Azure.

Als u wilt implementeren op een DC/OS of cluster Docker Swarm, selecteert u een van de volgende sjablonen in GitHub. Houd er rekening mee dat deze sjablonen hetzelfde, met uitzondering van de standaardselectie orchestrator zijn.

* [DC-en OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Controleer vervolgens of de CLI Azure is verbonden met een abonnement op Azure. U kunt dit doen door de volgende opdracht:

```bash
azure account show
```
Als een Azure-account niet wordt geretourneerd, gebruik de volgende opdracht de CLI aanmelden bij Azure.

```bash
azure login -u user@domain.com
```

Vervolgens configureert u de Azure CLI hulpmiddelen als Azure Resource Manager wilt gebruiken.

```bash
azure config mode arm
```

Een resourcegroep Azure en Container Service cluster maken met de volgende opdracht, waarbij:

- **RESOURCE_GROUP** is de naam van de resourcegroep die u wilt gebruiken voor deze service.
- **De locatie** is de Azure regio waar de resourcegroep en Azure Container Service implementatie worden gemaakt.
- **TEMPLATE_URI** is de locatie van het bestand voor de implementatie. Zorg ervoor dat dit de Raw-bestand, niet een pointer naar de GitHub UI. Om te zoeken naar deze URL, selecteer het bestand azuredeploy.json in de GitHub en op de **Raw** -knop.

> [AZURE.NOTE] Wanneer u deze opdracht uitvoert, wordt u de shell voor implementatie parameterwaarden gevraagd.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Sjabloonparameters geven

Deze versie van de opdracht moet u interactief parameters definiëren. Als u wilt opgeven van parameters, zoals een tekenreeks JSON-indeling, kunt u doen met behulp van de `-p` overschakelen. Bijvoorbeeld:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Ook kunt u een parameterbestand JSON-indeling opgeven met behulp van de `-e` schakelen:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Om te zien een voorbeeld van de parameters-bestand met de naam `azuredeploy.parameters.json`, zoekt met de Azure Container Service-sjablonen in de GitHub.

## <a name="create-a-service-by-using-powershell"></a>Een service maken met PowerShell

U kunt ook een cluster Azure Container Service met PowerShell implementeren. Dit document is gebaseerd op de versie 1.0 [Azure PowerShell module](https://azure.microsoft.com/blog/azps-1-0/).

Selecteer een van de volgende sjablonen voor de implementatie van een DC-en OS of Docker Swarm cluster. Houd er rekening mee dat deze sjablonen hetzelfde, met uitzondering van de standaardselectie orchestrator zijn.

* [DC-en OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Voordat u een cluster maakt in uw abonnement Azure, controleert u of dat de PowerShell-sessie op Azure is ondertekend. U kunt hiervoor met de `Get-AzureRMSubscription` opdracht:

```powershell
Get-AzureRmSubscription
```

Als u aan te melden Azure, gebruikt u de `Login-AzureRMAccount` opdracht:

```powershell
Login-AzureRmAccount
```

Als u naar een nieuwe resourcegroep distribueren wilt, moet u eerst de resourcegroep. U kunt een nieuwe resourcegroep maken de `New-AzureRmResourceGroup` opdracht en geeft u een resource groep naam en locatie voor de regio:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Nadat u een resourcegroep maakt, maakt u het cluster met de volgende opdracht. De URI van de gewenste sjabloon wordt opgegeven voor de `-TemplateUri` parameter. Wanneer u deze opdracht uitvoert, wordt u PowerShell implementatie parameterwaarden gevraagd.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Sjabloonparameters geven

Als u vertrouwd met PowerShell bent, weet u dat u door de beschikbare parameters voor een cmdlet bladeren kunt met een minteken (-) te typen en vervolgens op de TAB-toets te drukken. Deze functionaliteit werkt ook met de parameters die u in de sjabloon opgeeft. Zodra u typt de naam van de sjabloon, de cmdlet de sjabloon kan worden opgehaald, parseert de parameters, en wordt de Sjabloonparameters aan de opdracht dynamisch. Hierdoor kunt heel gemakkelijk de parameterwaarden voor de sjabloon opgeven. En als u bent een vereiste parameterwaarde vergeten, PowerShell wordt u gevraagd de waarde.

Hieronder vindt u de volledige opdracht met parameters opgenomen. U kunt uw eigen waarden opgeven voor de namen van de resources.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Volgende stappen

Nu dat er een goed functionerende cluster, raadpleegt u deze documenten voor verbinding en beheer van gegevens:

- [Verbinding maken met een cluster Azure Container Service](container-service-connect.md)
- [Werken met Azure Container Service en DC/OS](container-service-mesos-marathon-rest.md)
- [Werken met Azure Container Service en Swarm Docker](container-service-docker-swarm.md)
