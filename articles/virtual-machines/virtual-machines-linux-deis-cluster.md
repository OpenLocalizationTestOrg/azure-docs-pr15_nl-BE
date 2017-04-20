<properties
   pageTitle="Implementeren van een 3-node cluster Deis | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe u een 3-node cluster op Azure met behulp van een sjabloon Azure Resource Manager Deis"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Implementeren van een 3-node cluster Deis

Dit artikel begeleidt u bij het inrichten van een [Deis](http://deis.io/) cluster op Azure. Dit omvat alle stappen van het maken van de noodzakelijke certificaten distribueren en schalen een voorbeeldtoepassing **gaan** op de nieuw ingerichte cluster.

In het volgende diagram ziet u de architectuur van het geïmplementeerde systeem. Een systeembeheerder wordt beheerd in het cluster met behulp van hulpprogramma's zoals **deis** en **deisctl**Deis. Verbindingen worden tot stand gebracht via een Azure taakverdeling die de verbindingen naar een van de lidstaten knooppunten in het cluster doorstuurt. De clients toegang tot toepassingen via alsmede de taakverdeling geïmplementeerd. In dit geval de taakverdelingsvoorziening stuurt het verkeer naar een router NET die meer verkeer naar de bijbehorende Docker containers die worden gehost op het cluster routs Deis.

  ![Architectuurdiagram van gedistribueerde Desis cluster](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Door de volgende stappen uitvoeren, hebt u het volgende nodig:

 * Een abonnement op Azure active. Als u er geen hebt, krijgt u een gratis spoor op [azure.com](https://azure.microsoft.com/).
 * Een werk of school-id gebruiken Azure resourcegroepen. Als u een persoonlijke account en log in met een Microsoft-id hebt, moet u [een werk-id van uw persoonlijk een maken](virtual-machines-windows-create-aad-work-id.md).
 * Een--afhankelijk van uw clientbesturingssysteem--de [Azure PowerShell](../powershell-install-configure.md) of de [CLI voor Mac, Linux en Windows Azure](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL wordt gebruikt voor het genereren van de benodigde certificaten.
 * Een client Git zoals [Git Bash](https://git-scm.com/).
 * Als u wilt testen in de voorbeeldtoepassing, ook moet u een DNS-server. U kunt alle DNS-servers of services die ondersteuning bieden voor A-records voor jokertekens gebruiken.
 * Een computer uit te voeren Deis clienthulpprogramma's. U kunt een lokale computer of een virtuele machine. U kunt deze hulpprogramma's uitvoeren op bijna elke Linux-distributie, maar de volgende instructies gebruiken Ubuntu.

## <a name="provision-the-cluster"></a>Bepaling van het cluster

In dit gedeelte kunt u een sjabloon [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) van de open-source bibliotheek [azure-quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates). U zult eerst kopiëren naar de sjabloon. Vervolgens maakt u een nieuw SSH-sleutelpaar voor verificatie. En vervolgens gaat u een nieuwe id voor u cluster configureren. En ten slotte kunt u het Shell-script of de PowerShell-script voor het cluster inrichten.

1. De opslagplaats klonen: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Ga naar de map:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Maak een nieuwe SSH-sleutelpaar met behulp van ssh-keygen:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Een certificaat met de persoonlijke sleutel van bovenstaande genereren:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Ga naar [https://discovery.etcd.io/new](https://discovery.etcd.io/new) voor het genereren van een nieuw cluster token welke ziet er iets als:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Elk cluster CoreOS moet een unieke token uit deze gratis service. Zie [CoreOS documentatie](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) voor meer informatie.

6. Wijzig het bestand **cloud config.yaml** ter vervanging van de bestaande **discovery** token met het nieuwe token:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Wijzig het bestand **azuredeploy-parameters.json** : het certificaat dat u hebt gemaakt in stap 4 in een teksteditor openen. Kopieer alle tekst tussen `----BEGIN CERTIFICATE-----` en `-----END CERTIFICATE-----` in de **sshKeyData** -parameter (u moet alle tekens voor nieuwe regels verwijderen).

8. Wijzig de parameter **newStorageAccountName** . Dit is de rekening van de opslag voor VM OS schijven. Deze naam is uniek.

9. Wijzig de parameter **publicDomainName** . Dit zal deel uitmaken van de DNS-naam die is gekoppeld aan de load balancer openbare IP. De uiteindelijke FQDN hebben de indeling van _[de waarde van deze parameter]_. _[regio]_. cloudapp.azure.com. Bijvoorbeeld, als u de naam als de deishbai32 opgeven en de resourcegroep is geïmplementeerd in de regio West VS, vervolgens de definitieve FQDN-naam voor de verdeling van de belasting worden deishbai32.westus.cloudapp.azure.com.

10. De parameterbestand opslaan. En kunt u het cluster via Azure PowerShell richt:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  of Azure CLI

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Nadat de resourcegroep is ingericht, kunt u de bronnen in de groep zien op Azure klassieke portal. Zoals in de volgende schermafdruk, bevat de resourcegroep een virtueel netwerk met drie VMs, die lid zijn van dezelfde set beschikbaarheid. De groep bevat tevens een load balancer heeft een bijbehorende openbare IP.

  ![De ingerichte resourcegroep op Azure klassieke portal](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>De client installeren

U moet **deisctl** om te bepalen uw cluster Deis. Hoewel de deisctl wordt automatisch geïnstalleerd op alle clusterknooppunten, is een goede gewoonte om deisctl op een afzonderlijke administratieve machine gebruiken. Bovendien, omdat alle knooppunten met alleen particuliere IP-adressen zijn geconfigureerd, moet u SSH tunneling via de taakverdeling met een openbaar IP-adres, verbinding maken met het knooppunt computers gebruiken. Hieronder volgen de stappen voor het instellen van deisctl op een aparte Ubuntu fysieke of virtuele machine.

1. Installatie deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Uw persoonlijke sleutel met ssh-agent toevoegen:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Deisctl configureren:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

De sjabloon bepaalt de binnenkomende NAT regels die 2223 aan exemplaar 1, 2224 exemplaar 2 en 2225 exemplaar 3 toewijzen. Dit biedt redundantie voor het gebruik van het hulpprogramma deisctl. U kunt deze regels op Azure klassieke portal onderzoeken:

![NAT regels inzake de verdeling van de belasting](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] De sjabloon ondersteunt momenteel alleen 3 knooppunten bevatten. Dit komt door een beperking in Azure Resource Manager sjabloon NAT regel definiëren die lus syntaxis niet ondersteunt.

## <a name="install-and-start-the-deis-platform"></a>Installeer en start de Deis platform

Nu kunt u deisctl te installeren en start de Deis platform:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Vanaf het platform duurt (ongeveer 10 minuten). Vooral kan de builder-service te starten lang duren. En soms duurt een paar pogingen om te slagen: als de bewerking lijkt te reageren, kunt u `ctrl+c` aan het einde van de uitvoering van de opdracht en probeer het opnieuw.

U kunt `deisctl list` om te controleren of alle services worden uitgevoerd:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Gefeliciteerd! Nu u een lopend hebt Deis clsuter op Azure! Daarna laten we een voorbeeld van een toepassing aan het cluster in actie zien gaan implementeren.

## <a name="deploy-and-scale-a-hello-world-application"></a>Distribueren en schalen van een toepassing Hallo wereld

De volgende stappen laten zien hoe implementeren van een "Hello World" Ga toepassing aan het cluster. De stappen zijn gebaseerd op [Deis documentatie](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Voor het bewerkingsplan net goed te laten werken, moet u een jokerteken A record voor uw domein aan te wijzen op het openbare IP-adres van de taakverdeling. Het volgende screenshot toont de A-record voor de registratie van een monster domein op GoDaddy:

    ![Godaddy A-record](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Installatie deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Maak een nieuwe SSH-sleutel en voegt u de openbare sleutel op GitHub (natuurlijk kunt u ook opnieuw gebruiken uw bestaande sleutels). Een nieuw sleutelpaar SSH kunt maken:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Id_rsa.pub of de openbare sleutel van uw keuze, toevoegen aan de GitHub. U kunt dit doen met behulp van de sleutel toevoegen SSH-knop in uw SSH sleutels Configuratiescherm:

  ![Sleutel Github](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Een nieuwe gebruiker registreren:

        deis register http://deis.[your domain]
<p />
6. De SSH-sleutel toevoegen:

        deis keys:add [path to your SSH public key]
  <p />      
7. Een toepassing maken.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.De git push Docker afbeeldingen moeten worden ontwikkeld en geïmplementeerd, duurt een paar minuten moet worden geactiveerd. Uit Mijn ervaring, soms loopt stap 10 (Pushing afbeelding naar persoonlijke bibliotheek). Wanneer dit gebeurt, kunt u het proces stoppen, verwijdert u de toepassing met behulp van ' deis apps: vernietigen: een <application name> ` to remove the application and try again. You can use `apps:list deis' op de naam van uw toepassing. Als alles werkt, ziet u ongeveer het volgende aan het einde van de uitvoer van de opdracht:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Controleer of de toepassing werkt:

        curl -S http://[your application name].[your domain]
  U ziet:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Schaal 3 exemplaren van de toepassing:

        deis scale cmd=3
<p />
11. U kunt desgewenst deis info om te bekijken van details van uw toepassing. De volgende uitgangen zijn van mijn installatie van toepassingen:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Volgende stappen

In dit artikel u heeft gelopen door de stappen voor het inrichten van een nieuw cluster op Azure met behulp van een sjabloon Azure Resource Manager Deis. De sjabloon biedt ondersteuning voor redundantie in gereedschap verbindingen zoals taakverdeling voor gedistribueerde toepassingen. De sjabloon wordt ook voorkomen op lid-knooppunten, kostbare openbare IP-adresbronnen worden opgeslagen en biedt een beter beveiligde omgeving host voor toepassingen met behulp van openbare IP-adressen. Voor meer informatie, Zie de volgende artikelen:

[Azure Resource Manager-overzicht] [resource-group-overview]  
[Het gebruik van de CLI Azure] [azure-command-line-tools]  
[Met behulp van Azure PowerShell met Azure Resource Manager] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
