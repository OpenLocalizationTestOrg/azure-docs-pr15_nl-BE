<properties
    pageTitle="Azure CLI-opdrachten in de modus Beheer Service | Microsoft Azure"
    description="Azure opdrachtregelinterface (CLI) opdrachten in de modus Beheer Service voor het beheer van implementaties in het implementatiemodel klassiek"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure CLI-opdrachten in de modus Azure Service Management (asm)

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]U kunt ook [lezen over alle opdrachten voor bronbeheer model](virtual-machines/azure-cli-arm-commands.md), en de CLI gebruiken voor het [migreren van middelen](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) uit het klassieke het model Resource Manager.

In dit artikel wordt de syntaxis en opties voor Azure CLI-opdrachten die u meestal gebruikt maken en beheren in het implementatiemodel klassiek Azure resources. U toegang tot deze opdrachten met behulp van de CLI in Azure Service Management (asm)-modus. Dit is geen volledige informatie over en de CLI versie kan enigszins verschillende opdrachten of parameters weergeven. 

Ophalen is gestart, eerst [de Azure CLI installeren](xplat-cli-install.md) en [verbinding maken met uw abonnement Azure](xplat-cli-connect.md).

Voor de huidige opdrachtsyntaxis en opties op de opdrachtregel, typt u `azure help` kunt u help weergeven voor een specifieke opdracht `azure help [command]`. Ook vinden in de documentatie van CLI-voorbeelden voor het maken en beheren van specifieke Azure services.

Optionele parameters worden tussen vierkante haken weergegeven (bijvoorbeeld, `[parameter]`). Alle andere parameters zijn vereist.

Naast de optionele parameters opdracht specifieke hier beschreven, zijn er drie optionele parameters die kunnen worden gebruikt om gedetailleerde uitvoer als aanvraag opties en statuscodes weer te geven. De `-v` parameter biedt uitgebreide uitvoer en de `-vv` parameter geeft zelfs meer uitgebreide uitvoer. De `--json` optie wordt het resultaat in de indeling Onbewerkte json.

## <a name="setting-asm-mode"></a>Asm-modus instellen

Gebruik de volgende opdracht opdrachten in de modus Beheer van Azure CLI-Service inschakelen.

    azure config mode asm

>[AZURE.NOTE] Van de CLI Azure Resource Manager en Azure Service Management modus zijn die elkaar wederzijds uitsluiten. Dat wil zeggen kunnen niet bronnen die zijn gemaakt in een modus worden beheerd uit de andere modus.

## <a name="manage-your-account-information-and-publish-settings"></a>Publicatie-instellingen en gegevens van uw account beheren
Een manier die de CLI verbinding kunt maken met uw account is met behulp van uw abonnementsgegevens Azure. (Zie ook [verbinding maken met een Azure-abonnement van de CLI Azure](xplat-cli-connect.md) voor andere opties.) Deze informatie kan worden verkregen uit de Azure klassieke portal in een instellingenbestand publiceren die hier worden beschreven. Als een permanente lokale configuratie-instelling die de CLI wordt gebruikt voor verdere bewerkingen kunt u het bestand publiceren-instellingen importeren. U hoeft alleen voor het importeren van uw publicatie-instellingen eenmaal.

**download account [opties]**

Met deze opdracht start een browser te downloaden van het bestand .publishsettings uit de klassieke Azure portal.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**account importeren [opties] &lt;bestand >**


Met deze opdracht importeert een bestand publishsettings of een certificaat zodat deze kan worden gebruikt door het hulpprogramma in toekomstige sessies.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Het bestand publishsettings kan bevatten gegevens (naam van abonnement en ID) over meer dan één abonnement. Wanneer u het publishsettings-bestand importeert, wordt het eerste abonnement gebruikt als de standaardbeschrijving. Gebruik een ander abonnement, kunt u de volgende opdracht uitvoeren:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**account verwijderen [opties]**

Met deze opdracht verwijdert u de opgeslagen publishsettings die zijn geïmporteerd. Gebruik deze opdracht als u klaar bent met het programma op deze computer en zorgen dat het programma niet worden gebruikt met uw account in toekomstige sessies wilt.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**Accountlijst [opties]**

Lijst van de geïmporteerde abonnementen

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**account instellen [opties] &lt;abonnement&gt;**

Instellen van het huidige abonnement

###<a name="commands-to-manage-your-affinity-groups"></a>Opdrachten voor het beheren van uw affiniteit groepen

**lijst met accounts affiniteit groep [opties]**

Met deze opdracht worden de Azure affiniteit groepen.

Affiniteit groepen kan worden ingesteld wanneer een groep van virtuele machines meerdere fysieke machines. De groep affiniteit geeft fysieke machines moeten zo dicht mogelijk bij elkaar mogelijk te verminderen van netwerkvertraging.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**affiniteit-groep maken [opties] &lt;naam&gt;**

Deze opdracht maakt u een groep affiniteit

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**affiniteit rekeninggroep weergeven [opties] &lt;naam&gt;**

Met deze opdracht geeft de details van de groep affiniteit

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**account affiniteit groep verwijderen [opties] &lt;naam&gt;**

Deze opdracht verwijdert u de opgegeven affiniteit groep

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Opdrachten voor het beheren van uw account-omgeving

**lijst met accounts env [opties]**

Lijst van de account-omgevingen

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**account env weergeven [opties] [environment]**

Omgeving accountdetails weergeven

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**account env toevoegen [opties] [environment]**

Deze opdracht voegt u een omgeving aan de account

**env account instellen [opties] [environment]**

Deze opdracht stelt u de account-omgeving

**account env verwijderen [opties] [environment]**

Deze opdracht verwijdert u de opgegeven omgeving van de rekening

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Opdrachten voor het beheren van uw klassieke virtuele machines
In het volgende diagram ziet u hoe klassieke Azure virtuele machines worden gehost in de productieomgeving van de implementatie van een Azure cloud-dienst.

![Azure technische Diagram](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Nieuw** maakt u het station in blob-opslag (dat wil zeggen, e:\ in het diagram); een schijf al gemaakt, maar niet-gekoppelde **koppelen** is gekoppeld aan een virtuele machine.

**VM maken [opties] &lt;DNS-naam > &lt;afbeelding > &lt;gebruikersnaam > [wachtwoord]**

Met deze opdracht maakt een Azure virtuele machine. Elke virtuele machine (vm) wordt standaard gemaakt in een eigen cloud-service. U kunt opgeven dat een virtuele machine moet worden toegevoegd aan een bestaande service cloud door gebruik te maken van de optie - c zoals hier beschreven.

De vm opdracht, zoals de klassieke Azure portal maakt, maakt alleen de virtuele machines in de productieomgeving implementeren. Er is geen optie voor het maken van een virtuele machine in de testomgeving voor de implementatie van een cloud-service. Als uw abonnement geen een bestaande account voor Azure opslag, de opdracht gemaakt.

U kunt een locatie opgeven via de--locatieparameter of kunt u een groep affiniteit via de--parameter affiniteit groep. Als geen van beide is opgegeven, wordt u gevraagd om een uit een lijst met geldige locaties.

Het opgegeven wachtwoord moet 8 123 tekens lang zijn en voldoen aan de complexiteitsvereisten van het besturingssysteem dat u voor deze virtuele machine gebruikt.

Als u van plan bent om SSH te gebruiken voor het beheren van een geïmplementeerde Linux virtuele machine (zoals meestal het geval), moet u SSH inschakelen via de optie e - als u de virtuele machine maakt. Het is niet mogelijk om te schakelen SSH nadat u de virtuele machine hebt gemaakt.

Windows virtuele machines kunnen later RDP inschakelen door poort 3389 toe te voegen als een eindpunt.

De volgende optionele parameters worden voor deze opdracht ondersteund:

**- c,--verbinding** de virtuele machine binnen een reeds gemaakte implementatie in een hostingservice te maken. Als niet - vmname met deze optie wordt gebruikt, wordt de naam van de nieuwe virtuele machine wordt automatisch gegenereerd.<br />
**-n,--vm-naam** Geef de naam van de virtuele machine. Deze parameter wordt standaard hosting servicenaam. Als niet - vmname is opgegeven, de naam voor de nieuwe virtuele machine wordt gegenereerd als &lt;-servicenaam >&lt;id >, waarbij &lt;id > is het aantal bestaande virtuele machines in de service plus 1. Bijvoorbeeld, als u deze opdracht gebruikt om een virtuele machine toevoegen aan een hostingservice MijnService met een bestaande virtuele machine, de nieuwe virtuele machine met de naam MyService2.<br />
**-u,--blob-url** Geef de doel-URL blob storage waartegen de systeemschijf virtuele machine maken. <br />
**-z,--vm-grootte** Geef de grootte van de virtuele machine. Geldige waarden zijn: 'ExtraSmall', 'Klein', "Medium", 'Groot', "ExtraLarge", "A5", "A6", "A7", 'A8', "A9", "A10", 'A11', "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , 'Standard_G3', 'Standard_G4', 'Standard_G55'. De standaardwaarde is 'Klein'. <br />
**-r** RDP-verbindingen toevoegen aan een virtuele Windows-computer. <br />
**-e,--ssh** SSH verbinding toegevoegd aan een virtuele Windows-computer. <br />
**-t,--ssh-cert** Hiermee geeft u het certificaat SSH. <br />
**-s** Het abonnement <br />
**-o,--communautaire** De opgegeven afbeelding is een afbeelding van de Gemeenschap <br />
**-w** De naam van het virtuele netwerk <br/>
**-l,--locatie** geeft de locatie (bijvoorbeeld ' North Central ons'). <br />
Hiermee geeft u de groep affiniteit **- een,--affiniteit groep** .<br />
**-w,--virtuele-netwerknaam** Hiermee geeft u het virtuele netwerk waarop de nieuwe virtuele machine toevoegen. Virtuele netwerken kunnen worden ingesteld en beheerd vanuit Azure klassieke portal.<br />
**-b,--subnet-namen** Hier worden de subnetnamen voor het toewijzen van de virtuele machine.

In dit voorbeeld is MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB een afbeelding die door het platform. Zie voor meer informatie over besturingssysteemkopieën vm lijst met afbeeldingen.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**VM maken van &lt;DNS-naam > &lt;rol bestand >**

Met deze opdracht maakt een Azure virtuele machine uit een bestand JSON rol.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**VM lijst [opties]**

Met deze opdracht worden de Azure virtuele machines. De optie--json geeft aan dat de resultaten worden weergegeven in de indeling raw JSON.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**lijst van de locatie van VM [opties]**

Met deze opdracht worden alle beschikbare Azure account locaties.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**VM weergeven [opties] &lt;naam >**

Deze opdracht geeft u gegevens over een Azure VM. De optie--json geeft aan dat de resultaten worden weergegeven in de indeling raw JSON.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**VM verwijderen [opties] &lt;naam >**

Met deze opdracht verwijdert u een Azure virtuele machine. Met deze opdracht verwijdert standaard niet de Azure blob waaruit u de schijf en de gegevensschijf zijn gemaakt. Voor het verwijderen van de blob en de virtuele machine waarop het is gebaseerd, geeft u de optie -b.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**VM start [options] &lt;naam >**

Met deze opdracht wordt een Azure virtuele machine gestart.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**VM start [options] &lt;naam >**

Met deze opdracht wordt opnieuw gestart met een Azure virtuele machine.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**VM afsluiten [opties] &lt;naam >**

Met deze opdracht een Azure virtuele machine wordt afgesloten. U kunt de optie -p geeft de compute-bron worden niet vrijgegeven op afsluiten.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**VM vastleggen &lt;vm naam > &lt;doel Procesnaam >**

Met deze opdracht wordt er een afbeelding Azure virtuele machine.

De afbeelding van een virtuele machine kan alleen worden vastgelegd als de status van de virtuele machine **gestopt**. De virtuele machine afgesloten voordat u verdergaat.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**VM exporteren [opties] &lt;vm naam > &lt;pad naar bestand >**

Met deze opdracht exporteren een afbeelding Azure virtuele machine naar een bestand

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Opdrachten voor het beheren van uw Azure VM eindpunten
In het volgende diagram ziet u de architectuur van een typische implementatie van meerdere exemplaren van een klassieke virtuele machine. In dit voorbeeld is de poort 3389 open op elke virtuele machine (voor toegang van RDP). Er is ook een intern IP-adres (bijvoorbeeld 168.55.11.1) op elke virtuele machine die wordt gebruikt door de verdeling van de belasting voor het routeren van netwerkverkeer naar de virtuele machine. Deze interne IP-adres kan ook worden gebruikt voor communicatie tussen virtuele machines.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Externe aanvragen voor de virtuele machines cursisten met een load balancer. Om deze reden worden niet aanvragen tegen een bepaalde virtuele machine voor implementaties met meerdere virtuele machines opgegeven. Voor implementaties met meerdere virtuele machines, moet de poorttoewijzing worden geconfigureerd tussen de virtuele machines (vm-poort) en de verdeling van de belasting (lb-poort).

**VM-eindpunt maken &lt;vm naam > &lt;lb poort > [vm-poort]**

Met deze opdracht maakt een eindpunt van de virtuele machine. U kunt ook -u of--enable direct-server terug te geven of om directe server als resultaat dit eindpunt, standaard uitgeschakeld.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**vm eindpunt maken-meerdere [opties] &lt;vm naam > &lt;lb poort > [:&lt;vm poort > [:&lt;protocol > [:&lt;inschakelen direct-server return > [:&lt;lb setnaam > [:&lt;sonde protocol > [:&lt;sonde poort > [:&lt;sonde pad > [:&lt;interne lb-naam >]]] {1 -*}**

Maak meerdere vm eindpunten.

**VM-eindpunt verwijderen [opties] &lt;vm naam > &lt;-de naam >**

Met deze opdracht verwijdert u een eindpunt van de virtuele machine.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**lijst van VM eindpunt &lt;vm naam >**

Met deze opdracht worden alle virtuele machine eindpunten. De optie--json geeft aan dat de resultaten worden weergegeven in de indeling raw JSON.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**VM eindpunt update [opties] &lt;vm naam > &lt;-de naam >**

Deze opdracht wordt een eindpunt vm bijgewerkt naar nieuwe waarden met behulp van deze opties.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**VM-eindpunt weergeven [opties] &lt;vm naam >**

Met deze opdracht geeft de details van de eindpunten op een vm

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Opdrachten voor het beheren van uw afbeeldingen Azure virtuele machine

Virtuele machine afbeeldingen zijn opnamen van een reeds geconfigureerde virtuele machines die kunnen worden gerepliceerd als vereist.

**lijst met afbeeldingen VM [opties]**

Met deze opdracht wordt een lijst met afbeeldingen van de virtuele machine. Er zijn drie typen afbeeldingen: afbeeldingen gemaakt door Microsoft, die worden voorafgegaan door 'MSFT', gemaakt door derden, die worden voorafgegaan door de naam van de leverancier en u afbeeldingen afbeeldingen maken. Om afbeeldingen te maken, moet u een bestaande virtuele machine vastleggen of een afbeelding maken van een aangepaste VHD geüpload naar een blobopslag. Zie voor meer informatie over het gebruik van een aangepaste VHD vm-afbeelding maken.
De optie--json geeft aan dat de resultaten worden weergegeven in de indeling raw JSON.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**VM afbeelding weergeven [opties] &lt;naam >**

Met deze opdracht geeft de details van de afbeelding voor een virtuele machine.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**VM afbeelding verwijderen [opties] &lt;naam >**

Deze opdracht verwijdert u de afbeelding voor een virtuele machine.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**VM-afbeelding maken &lt;naam > [bronpad]**

Met deze opdracht maakt een installatiekopie van de virtuele machine. De aangepaste VHD-bestanden worden geüpload om blob-opslag en vervolgens de afbeelding van de virtuele machine wordt gemaakt vanaf daar. Vervolgens u deze afbeelding van de virtuele machine maakt een virtuele machine. De locatie en OS-parameters zijn vereist.

>[AZURE.NOTE]Deze opdracht ondersteunt momenteel alleen vaste VHD bestanden zijn geüpload. Gebruik dynamische VHD-bestand uploaden, de [Azure VHD-hulpprogramma's voor gaan](https://github.com/Microsoft/azure-vhd-utils-for-go).

Sommige systemen bestand process-descriptor grenzen opgelegd. Als deze limiet wordt overschreden, wordt het hulpprogramma een bestandsfout descriptor-limiet. U kunt de opdracht opnieuw met de p - uitvoeren &lt;getal > parameter voor het verlagen van het maximum aantal parallelle uploads. De standaard maximum aantal parallelle uploads is 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Opdrachten voor het beheren van uw gegevensschijven Azure virtuele machine

Gegevensschijven zijn VHD-bestanden in een blobopslag die door een virtuele machine kan worden gebruikt. Zie voor meer informatie over hoe u gegevensschijven worden geïmplementeerd om blob-opslag, de Azure technische diagram hierboven.

De opdrachten voor het koppelen van gegevensschijven (azure vm schijf koppelen en azure vm schijf koppelen-nieuwe) een logische eenheid (LUN) toewijzen aan de gegevensschijf van de gekoppelde, zoals vereist door de SCSI-protocol. De eerste schijf met gegevens die zijn gekoppeld aan een virtuele machine LUN 0 wordt toegewezen, de volgende toegewezen LUN 1, enzovoort.

Wanneer u een schijf met gegevens met de azure vm schijf ontkoppelen opdracht loskoppelen, gebruikt u de &lt;lun&gt; parameter om aan te geven welke schijf los te koppelen.

>[AZURE.NOTE] U moet altijd in omgekeerde volgorde gegevensschijven los beginnen met het hoogste nummer LUN is toegewezen. De SCSI-Linux laag biedt geen ondersteuning voor loskoppelen van een LUN met lagere nummers terwijl er een LUN hogere nummers is nog steeds gekoppeld. U moet bijvoorbeeld geen LUN 0 loskoppelen als LUN 1 is nog steeds gekoppeld.

**VM schijf weergeven [opties] &lt;naam >**

Deze opdracht geeft u informatie over een schijf Azure.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**VM Schijflijst [opties] [vm-naam]**

Deze opdracht lijsten Azure schijven of schijven die zijn gekoppeld aan een opgegeven virtuele machine. Als u met de parameter voor een virtuele machine uitvoert, wordt alle schijven die zijn gekoppeld aan de virtuele machine. 1 LUN wordt gemaakt met de virtuele machine en andere vermelde schijven afzonderlijk zijn gekoppeld.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Deze opdracht zonder de parameter voor een virtuele machine wordt uitgevoerd, retourneert alle schijven.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**VM schijf verwijderen [opties] &lt;naam >**

Met deze opdracht wordt een Azure-schijf uit een persoonlijke bibliotheek verwijderd. De schijf moet worden losgekoppeld van de virtuele machine voordat deze wordt verwijderd.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**VM schijf maken &lt;naam > [bronpad]**

Met deze opdracht worden geüpload en registreert een Azure-schijf. --blob-url,--locatie, of--affiniteit groep moet worden opgegeven. Als u deze opdracht gebruikt met [bronpad], het VHD-bestand opgegeven is geüpload en wordt een afbeelding gemaakt. U kunt deze afbeelding vervolgens koppelen aan een virtuele machine met behulp van vm schijf koppelen.

Sommige systemen bestand process-descriptor grenzen opgelegd. Als deze limiet wordt overschreden, wordt het hulpprogramma een bestandsfout descriptor-limiet. U kunt de opdracht opnieuw met de p - uitvoeren &lt;getal > parameter voor het verlagen van het maximum aantal parallelle uploads. De standaard maximum aantal parallelle uploads is 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**VM schijf uploaden [opties] &lt;bron pad > &lt;blob-url > &lt;opslag account toetsen >**

Met deze opdracht kunt u een schijf vm uploaden

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**VM schijf koppelen &lt;vm naam > &lt;schijf Procesnaam >**

Met deze opdracht koppelt een bestaande schijf in blob-opslag met een bestaande virtuele machine in een cloud-service geïmplementeerd.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**VM schijf koppelen-nieuwe &lt;vm naam > &lt;grootte in gb > [blob-url]**

Met deze opdracht koppelt een schijf met gegevens een Azure virtual machine. 20 is in dit voorbeeld wordt de grootte van de nieuwe schijf in gigabyte moet worden gekoppeld. Desgewenst kunt u een blob-URL als het laatste argument expliciet opgeven van de blob doel te maken. Als u niet een blob-URL opgeeft, wordt automatisch een blob object gegenereerd.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**VM schijf los &lt;vm naam > &lt;lun >**

Met deze opdracht wordt een schijf met gegevens die zijn gekoppeld aan een Azure virtuele machine. &lt;LUN > identificeert de schijf worden losgekoppeld. Als u een lijst met schijven die is gekoppeld aan een schijf voordat u deze loskoppelt, vm schijf-lijst gebruiken &lt;vm-name >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Opdrachten voor het beheren van uw Azure cloud services

Azure cloud services zijn toepassingen en services op het web-rollen en functies van de werknemer. De volgende opdrachten kunnen worden gebruikt voor het beheren van Azure cloud services.

**service maken [opties] &lt;servicenaam >**

Deze opdracht maakt u een cloud-service

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**service weergeven [opties] &lt;servicenaam >**

Deze opdracht geeft u de details van een Azure cloud-service

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**lijst van service [opties]**

Met deze opdracht geeft Azure cloud services.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**service verwijderen [opties] &lt;naam >**

Met deze opdracht verwijdert u een Azure cloud-service.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Wilt verwijderen, gebruikt u de `-q` parameter.


## <a name="commands-to-manage-your-azure-certificates"></a>Opdrachten voor het beheren van uw certificaten Azure

Azure service certificaten zijn gekoppeld aan uw account Azure SSL-certificaten. Voor meer informatie over Azure certificaten Zie [Certificaten beheren](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**lijst van service-cert [opties]**

Met deze opdracht worden de certificaten Azure.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**Service certificaat maken &lt;dns-voorvoegsel > &lt;bestand > [wachtwoord]**

Met deze opdracht worden een certificaat. Niets in het wachtwoord wordt gevraagd voor certificaten die niet beveiligd met een wachtwoord zijn.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**Service certificaat verwijderen [opties] &lt;vingerafdruk >**

Met deze opdracht verwijdert u een certificaat.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Opdrachten voor het beheren van uw web-apps

Een Azure web app is een configuratie van Internet toegankelijk door URI. Webtoepassingen in virtuele machines worden gehost, maar u hoeft niet te denken over de details van het maken en implementeren van de virtuele machine zelf. Deze gegevens worden verwerkt voor u door Azure.

**lijst met sites [opties]**

Met deze opdracht geeft een overzicht van uw web-apps.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**site instellen [opties] [naam]**

Deze opdracht stelt u de configuratie-opties voor uw web app [naam]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**site deploymentscript [opties]**

Deze opdracht genereert u een script voor aangepaste implementatie

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**site maken [opties] [naam]**

Met deze opdracht maakt een web app en de lokale map.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] De naam moet uniek zijn. U kunt een site maken met dezelfde DNS-naam als een bestaande site.

**site bladeren [opties] [naam]**

Deze opdracht opent uw web app in een browser.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**site weergeven [opties] [naam]**

Deze opdracht geeft u de details voor een web app.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**site verwijderen [opties] [naam]**

Met deze opdracht verwijdert u een web app.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **site swap [opties] [naam]**

Met deze opdracht worden omgewisseld voor twee sleuven voor web app.

Deze opdracht ondersteunt de volgende extra opties:

**- q of **--stille **: niet om bevestiging gevraagd. Deze optie gebruiken in geautomatiseerde scripts.


**site-start [options] [naam]**

Deze opdracht start een web app.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**site-stop [opties] [naam]**

Deze opdracht stopt u een web app.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**site opnieuw [opties] [naam]**

Met deze opdracht stopt en start vervolgens een opgegeven web app.

Deze opdracht ondersteunt de volgende extra opties:

**--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.


**lijst van site-locatie [opties]**

Met deze opdracht worden de locaties van uw web app.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Opdrachten voor het beheren van uw app instellingen van webtoepassing

**lijst met sites appsetting [opties] [naam]**

Met deze opdracht wordt de instelling van de app toegevoegd aan de web app.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**appsetting site toevoegen [opties] &lt;keyvaluepair > [naam]**

Met deze opdracht wordt een instelling app toegevoegd aan uw web app als een sleutel-waardepaar.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**site appsetting verwijderen [opties] &lt;sleutel > [naam]**

Deze opdracht wordt de instelling van de opgegeven app verwijderd uit de web app.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**site appsetting weergeven [opties] &lt;sleutel > [naam]**

Deze opdracht geeft u de details van de instelling van de opgegeven app

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Opdrachten voor het beheren van uw web app-certificaten

**cert sitelijst [opties] [naam]**

Deze opdracht geeft u een lijst met de certificaten voor het web app.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**site certificaat toevoegen [opties] &lt;certificaat pad > [naam]**

**site certificaat verwijderen [opties] &lt;vingerafdruk > [naam]**

**site-cert weergeven [opties] &lt;vingerafdruk > [naam]**

Deze opdracht geeft u de details van het certificaat

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Opdrachten voor het beheren van uw web app-verbindingsreeksen

**lijst van de connectionstring-site [opties] [naam]**

**site connectionstring toevoegen [opties] &lt;connectionname > &lt;waarde > &lt;type > [naam]**

**site connectionstring verwijderen [opties] &lt;connectionname > [naam]**

**site connectionstring weergeven [opties] &lt;connectionname > [naam]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Opdrachten voor het beheren van uw web app standaarddocumenten

**lijst van site-defaultdocument [opties] [naam]**

**defaultdocument site toevoegen [opties] &lt;document > [naam]**

**defaultdocument site verwijderen [opties] &lt;document > [naam]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Opdrachten voor het beheren van uw web app-implementaties

**lijst van site-implementatie [opties] [naam]**

**website-implementatie weergeven [opties] &lt;commitId > [naam]**

**site-implementatie opnieuw distribueren [opties] &lt;commitId > [naam]**

**site-implementatie github [opties] [naam]**

**site-implementatie gebruiker instellen [opties] [gebruikersnaam] [wachtwoord]**

###<a name="commands-to-manage-your-web-app-domains"></a>Opdrachten voor het beheren van uw domeinen web app

**lijst met sites domein [opties] [naam]**

**site domein toevoegen [opties] &lt;dn > [naam]**

**site domein verwijderen [opties] &lt;dn > [naam]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Opdrachten voor het beheren van uw web app-Handlertoewijzingen

**lijst van site-handler [opties] [naam]**

**site-handler toevoegen [opties] &lt;extensie > &lt;processor > [naam]**

**site-handler verwijderen [opties] &lt;extensie > [naam]**

###<a name="commands-to-manage-your-web-jobs"></a>Opdrachten voor het beheren van uw website-vacatures

**lijst met site [opties] [naam]**

Met deze opdracht een lijst alle web-taken onder een web app.

Deze opdracht ondersteunt de volgende extra opties:

+ **--type taak** &lt;type taak >: optioneel. Het type van de webjob. Geldige waarde is "geactiveerd" of "continue". Standaard webjobs van alle typen als resultaat.
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

**site-taak weergeven [opties] &lt;taaknaam > &lt;taaktype > [naam]**

Deze opdracht geeft u de details van een specifieke web-project.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;-taaknaam >: vereist. De naam van de webjob.
+ **--type taak** &lt;type taak >: vereist. Het type van de webjob. Geldige waarde is "geactiveerd" of "continue".
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

**site verwijderen [opties] &lt;taaknaam > &lt;taaktype > [naam]**

Deze opdracht verwijdert u het opgegeven web project.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;-naam > vereist. De naam van de webjob.
+ **--type taak** &lt;type taak > vereist. Het type van de webjob. Geldige waarde is "geactiveerd" of "continue".
+ **q -** of **--stille**: niet om bevestiging gevraagd. Deze optie gebruiken in geautomatiseerde scripts.
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

**site taak uploaden [opties] &lt;taaknaam > &lt;taaktype > <jobFile> [naam]**

Deze opdracht verwijdert u het opgegeven web project.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;-taaknaam >: vereist. De naam van de webjob.
+ **--type taak** &lt;type taak >: vereist. Het type van de webjob. Geldige waarde is "geactiveerd" of "continue".
+ **--job-bestand** &lt;job-bestand >: vereist. Het project-bestand.
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

**site-taak start [options] &lt;taaknaam > &lt;taaktype > [naam]**

Met deze opdracht wordt het opgegeven web project gestart.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;-taaknaam >: vereist. De naam van de webjob.
+ **--type taak** &lt;type taak >: vereist. Het type van de webjob. Geldige waarde is "geactiveerd" of "continue".
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

**einde van de taak site [opties] &lt;taaknaam > &lt;taaktype > [naam]**

Deze opdracht stopt u de taak van het opgegeven web. Alleen van continue taken kunnen worden gestopt.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;-taaknaam >: vereist. De naam van de webjob.
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

###<a name="commands-to-manage-your-web-jobs-history"></a>Opdrachten voor het beheren van uw taken webgeschiedenis

**site geschiedenislijst [opties] [taaknaam] [naam]**

Deze opdracht geeft u een overzicht van de uitgevoerde taak opgegeven web.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;-taaknaam >: vereist. De naam van de webjob.
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

**site geschiedenis weergeven [opties] [taaknaam] [runId] [naam]**

Met deze opdracht haalt de details van de taak worden uitgevoerd voor het opgegeven web project.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;-taaknaam >: vereist. De naam van de webjob.
+ **--run-id** &lt;run-id >: optioneel. De id van de geschiedenis uitvoeren. Als niet wordt opgegeven, tonen de nieuwste uitvoeren.
+ **--sleuf** &lt;sleuf >: de naam van de sleuf op te starten.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Opdrachten voor het beheren van uw web app diagnostische gegevens

**locatie logboek downloaden [opties] [naam]**

Download een ZIP-bestand met diagnostische gegevens van uw web app.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**locatie logboek staart [opties] [naam]**

Met deze opdracht maakt verbinding met de terminal de logboek-streaming-service.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**site-logboekset [opties] [naam]**

Met deze opdracht configureert u de diagnostische opties voor uw web app.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Opdrachten voor het beheren van uw web app opslagplaatsen

**website bibliotheek filiaal [opties] &lt;branch > [naam]**

**site bibliotheek verwijderen [opties] [naam]**

**site-opslagplaats synchroniseren [opties] [naam]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Opdrachten voor het beheren van uw app schalen

**schaalmodus site [opties] &lt;modus > [naam]**

**site schaal exemplaren [opties] &lt;exemplaren > [naam]**


## <a name="commands-to-manage-azure-mobile-services"></a>Opdrachten voor het beheren van Azure Mobile Services

Azure Mobile Services combineert een aantal Azure services waarmee back-end-mogelijkheden voor uw toepassingen. Opdrachten voor mobiele Services zijn onderverdeeld in de volgende categorieën:

+ [Opdrachten voor het beheren van mobiele service-exemplaren](#Mobile_Services)
+ [Opdrachten voor het beheren van de configuratie van de mobiele service](#Mobile_Configuration)
+ [Opdrachten voor het beheren van mobiele-tabellen](#Mobile_Tables)
+ [Opdrachten voor het beheren van mobiele-scripts](#Mobile_Scripts)
+ [Opdrachten voor het beheren van geplande taken](#Mobile_Jobs)
+ [Opdrachten voor het schalen van een mobiele service](#Mobile_Scale)

De volgende opties van toepassing op opdrachten voor de meeste mobiele Services:

+ **-h** of **--help**: weergave-informatie over het gebruik uitvoer.
+ **-s `<id>` ** of **--abonnement `<id>` **: gebruik van een bepaald abonnement, opgegeven als `<id>`.
+ **v -** of **--verbose**: uitgebreide uitvoer schrijven.
+ **--json**: JSON schrijft uitvoer.

### <a name="Mobile_Services"></a>Opdrachten voor het beheren van mobiele service-exemplaren

**mobiele locaties [opties]**

Met deze opdracht worden de geografische locaties ondersteund door mobiele Services.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**mobiele maken [opties] [servicenaam] [sqlAdminUsername] [sqlAdminPassword]**

Met deze opdracht maakt een mobiele service samen met een SQL-Database en server.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **- r `<sqlServer>` ** of **--sqlServer `<sqlServer>` **: gebruiken van een bestaande SQL-Database-server worden opgegeven als `<sqlServer>`.
+ **-d `<sqlDb>` ** of **--sqlDb `<sqlDb>` **: gebruiken van bestaande SQL-database, opgegeven als `<sqlDb>`.
+ **-l `<location>` ** of **--locatie `<location>` **: het maken van de service op een specifieke locatie, opgegeven als `<location>`. Azure mobiele locaties als u de beschikbare locaties worden uitgevoerd.
+ **--sqlLocation `<location>` **: maken van de SQL-server in een specifieke `<location>`; Standaard is de locatie van de mobiele service.

**mobiele verwijderen [opties] [servicenaam]**

Deze opdracht verwijdert u een mobile-service en de SQL-Database en server.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **d-** of **--deleteData**: alle gegevens van deze mobiele service uit de database verwijderen.
+ **-een** of **deleteAll--**: verwijderen van de server en de SQL-Database.
+ **q -** of **--stille**: niet om bevestiging gevraagd. Deze optie gebruiken in geautomatiseerde scripts.

**mobiele lijst [opties]**

Met deze opdracht geeft een overzicht van uw mobiele services.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**mobiele weergeven [opties] [servicenaam]**

Deze opdracht geeft u gegevens over een mobiele service.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**mobiele opnieuw [opties] [servicenaam]**

Met deze opdracht wordt opnieuw gestart met een mobiele service-exemplaar.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**mobiele log [opties] [servicenaam]**

Deze opdracht retourneert de mobiele service, Logboeken, alle log-typen worden uitgefilterd maar `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **- r `<query>` ** of **-query `<query>` **: het opgegeven logboek query wordt uitgevoerd.
+ **-t `<type>` ** of **-type `<type>` **: de geretourneerde Logboeken filteren op post `<type>`, die kan zijn `information`, `warning`, of `error`.
+ **-k `<skip>` ** of **--overslaan `<skip>` **: Hiermee slaat u het aantal rijen dat in `<skip>`.
+ **-p `<top>` ** of **--top `<top>` **: geeft als resultaat een bepaald aantal rijen, opgegeven door `<top>`.

> [AZURE.NOTE] De **--query** parameter voorrang op **--type**, **--overslaan**, en **--boven**.

**mobiele herstellen [opties] [unhealthyservicename] [healthyservicename]**

Met deze opdracht herstelt een beschadigde mobiele service door deze te verplaatsen naar een gezonde mobiele service in een andere regio.

Deze opdracht ondersteunt de volgende extra opties:

**q -** of **--stille**: de prompt voor bevestiging van herstel.

**mobiele sleutel opnieuw genereren [opties] [servicenaam] [type]**

Met deze opdracht worden opnieuw gegenereerd door de sleutel voor de mobiele toepassing.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Typen sleutels zijn `master` en `application`.

> [AZURE.NOTE] Wanneer u sleutels opnieuw genereren, is het mogelijk dat clients die werken met de oude sleutel geen toegang tot uw mobiele service. Wanneer u de toepassingstoets opnieuw genereren, moet u uw app bijwerken met de nieuwe sleutelwaarde.

**mobiele sleutelset [opties] [servicenaam] [type] [waarde]**

Deze opdracht stelt de sleutel mobile-service op een specifieke waarde.


### <a name="Mobile_Configuration"></a>Opdrachten voor het beheren van de configuratie van de mobiele service

**lijst van mobiele config [opties] [servicenaam]**

Met deze opdracht geeft een overzicht van de configuratieopties voor een mobiele service.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**mobiele config [opties] [servicenaam] [toets] ophalen**

Met deze opdracht haalt een specifieke configuratie-optie voor een mobiele service, in dit geval dynamische schema.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**mobiele configuratieset [opties] [servicenaam] [toets] [waarde]**

Deze opdracht stelt u een specifieke configuratie-optie voor een mobiele service, in dit geval dynamische schema.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Opdrachten voor het beheren van mobiele-tabellen

**lijst van mobiele tabel [opties] [servicenaam]**

Met deze opdracht worden alle tabellen in uw mobiele service.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**mobiele tabel weergeven [opties] [servicenaam] [tablename]**

Deze opdracht geeft u details over een bepaalde tabel als resultaat gegeven.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**mobiele tabel maken [opties] [servicenaam] [tablename]**

Met deze opdracht wordt een tabel gemaakt.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-p `&lt;permissions>` ** of **--machtigingen `&lt;permissions>` **: door komma's gescheiden lijst met `<operation>` = `<permission>` paren, waarbij `<operation>` is `insert`, `read`, `update`, of `delete` en `&lt;permissions>` is `public`, `application` (standaard), `user`, of `admin`.

**mobiele gegevens lezen [opties] [servicenaam] [tablename] [query]**

Met deze opdracht leest gegevens uit een tabel.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-k `<skip>` ** of **--overslaan `<skip>` **: Hiermee slaat u het aantal rijen dat in `<skip>`.
+ **-t `<top>` ** of **--top `<top>` **: geeft als resultaat een bepaald aantal rijen, opgegeven door `<top>`.
+ **l -** of **--lijst**: geeft als resultaat de gegevens in een lijst.

**mobiele update voor [opties] [servicenaam] [tablename]**

Met deze opdracht wijzigt machtigingen voor het verwijderen van een tabel alleen voor beheerders.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-p `&lt;permissions>` ** of **--machtigingen `&lt;permissions>` **: door komma's gescheiden lijst met `<operation>` = `<permission>` paren, waarbij `<operation>` is `insert`, `read`, `update`, of `delete` en `&lt;permissions>` is `public`, `application` (standaard), `user`, of `admin`.
+ **--deleteColumn `<columns>` **: door komma's gescheiden lijst van de kolommen te verwijderen, als `<columns>`.
+ **q -** of **--stille**: kolommen verwijderd zonder dat om bevestiging wordt gevraagd.
+ **--addIndex `<columns>` **: door komma's gescheiden lijst met kolommen in de index wilt opnemen.
+ **--deleteIndex `<columns>` **: door komma's gescheiden lijst met kolommen moeten worden uitgesloten van de index.

**mobiele tabel verwijderen [opties] [servicenaam] [tablename]**

Met deze opdracht verwijdert een tabel.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Hiermee geeft u de parameter - q om de tabel zonder bevestiging verwijderen. Dit doen om te voorkomen dat het blokkeren van automatiseringsscripts.

**mobiele gegevens afkappen [opties] [servicenaam] [tablename]**

Met deze opdracht verwijdert u alle rijen met gegevens uit de tabel.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Opdrachten voor het beheer van scripts

Opdrachten in deze sectie worden gebruikt om de server-scripts die deel uitmaken van een mobile-service beheren. Zie [werken met serverscripts in mobiele Services](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md)voor meer informatie.

**Scriptlijst van mobiele [opties] [servicenaam]**

Met deze opdracht worden de geregistreerde scripts, inclusief scripts voor zowel de tabel als de scheduler.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**mobiele script download [opties] [servicenaam] [scriptnaam]**

Met deze opdracht het script invoegen uit de tabel TodoItem downloadt naar een bestand met de naam `todoitem.insert.js` in de `table` submap.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-p `<path>` ** of **--pad `<path>` **: de locatie in het bestand waarin u het script opslaat waarin de huidige werkmap de standaardinstelling is.
+ **-f `<file>` ** of **-bestand `<file>` **: de naam van het bestand waarin het script opslaan.
+ **-o** of **--overschrijven**: een bestaand bestand overschrijven.
+ **-c** of **--console**: het script naar de console in plaats van naar een bestand schrijven.

**mobiele script uploaden [opties] [servicenaam] [scriptnaam]**

Met deze opdracht worden geüpload een script met de naam `todoitem.insert.js` van de `table` submap.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

De naam van het bestand moet worden samengesteld uit de namen en de werking. Het moet zich bevinden in de tabel submap van de locatie waar de opdracht wordt uitgevoerd. Ook kunt u de **-f `<file>` ** of **-bestand `<file>` ** parameter geeft u een andere bestandsnaam en pad naar het bestand met het script dat u wilt registreren.


**mobiele script verwijderen [opties] [servicenaam] [scriptnaam]**

Met deze opdracht verwijdert u het bestaande script voor invoegen uit de tabel TodoItem.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Opdrachten voor het beheren van geplande taken

Opdrachten in deze sectie worden gebruikt voor het beheren van geplande taken die deel uitmaken van een mobile-service. Zie [taken plannen](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx)voor meer informatie.

**mobiele taaklijst [opties] [servicenaam]**

Met deze opdracht geeft u een overzicht van geplande taken.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**mobiele maken [opties] [servicenaam] [taaknaam]**

Deze opdracht maakt u een taak met de naam `getUpdates` die is gepland per uur.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-i `<number>` ** of **--interval `<number>` **: taakinterval voor de, als een geheel getal. De standaardwaarde is `15`.
+ **-u `<unit>` ** of **--intervalUnit `<unit>` **: de eenheid voor het _interval_, die een van de volgende waarden:
    + **minuut** (standaard)
    + **uur**
    + **dag**
    + **maand**
    + **geen** (taken op aanvraag)
+ **-t `<time>`** **--startTime `<time>` ** De begintijd van de eerste uitvoering van het script in de ISO-notatie. De standaardwaarde is `now`.

> [AZURE.NOTE] Nieuwe taken worden gemaakt in een uitgeschakeld omdat een script moet nog steeds worden geüpload. Gebruik de opdracht **mobiele script uploaden** om een script en de opdracht **mobiel project bijwerken** om de taak te uploaden.

**mobiele taak update [opties] [servicenaam] [taaknaam]**

De volgende opdracht wordt de uitgeschakelde `getUpdates` taak.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-i `<number>` ** of **--interval `<number>` **: taakinterval voor de, als een geheel getal. De standaardwaarde is `15`.
+ **-u `<unit>` ** of **--intervalUnit `<unit>` **: de eenheid voor het _interval_, die een van de volgende waarden:
    + **minuut** (standaard)
    + **uur**
    + **dag**
    + **maand**
    + **geen** (taken op aanvraag)
+ **-t `<time>`** **--startTime `<time>` ** De begintijd van de eerste uitvoering van het script in de ISO-notatie. De standaardwaarde is `now`.
+ **- a `<status>` ** of **: status `<status>` **: de taakstatus, kan `enabled` of `disabled`.

**mobiele verwijderen [opties] [servicenaam] [taaknaam]**

Met deze opdracht verwijdert u de geplande taak getUpdates van de takenlijst van server.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Verwijderen van een taak verwijderd de geüploade script ook.

### <a name="Mobile_Scale"></a>Opdrachten voor het schalen van een mobiele service

Opdrachten in deze sectie worden gebruikt voor het schalen van een mobiele service. Zie [schalen van een mobiele service](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx)voor meer informatie.

**mobiele schaal [opties] [servicenaam] weergeven**

Deze opdracht geeft u gegevens over de schaal met inbegrip van de huidige berekenen en het aantal exemplaren.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**mobiele schaal wijzigen [opties] [servicenaam]**

Deze opdracht wijzigt de schaal van de mobiele service van gratis premium-modus.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **- c `<mode>` ** of **--computeMode `<mode>` **: de compute-modus moet een `Free` of `Reserved`.
+ **-i `<count>` ** of **--numberOfInstances `<count>` **: het aantal exemplaren in gereserveerde modus gebruikt.

> [AZURE.NOTE] Wanneer u een compute-modus instelt op `Reserved`, uw mobiele services in hetzelfde gebied in de premium-modus uitvoeren.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Voorbeeld-functies inschakelen voor uw mobiele Service-opdrachten

**lijst van mobiele voorbeeld [opties] [servicenaam]**

Deze opdracht geeft u de voorbeeld-functies die beschikbaar zijn op de opgegeven service en of ze zijn ingeschakeld.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**mobiele voorvertoning inschakelen [opties] [servicenaam] [onderdeel]**

Met deze opdracht kunt de opgegeven voorbeeldfunctie voor een mobiele service. Eenmaal is ingeschakeld, kunnen niet voor een mobiele service voorbeeld functies worden uitgeschakeld.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Opdrachten voor het beheren van uw mobiele service API 's

**mobiele api lijst [opties] [servicenaam]**

Deze opdracht geeft u een lijst van mobiele service aangepaste API's die u hebt gemaakt voor uw mobiele service.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**mobiele api maken [opties] [servicenaam] [apiname]**

Hiermee maakt u een aangepaste mobiele service-API

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Deze opdracht ondersteunt de volgende extra opties:

**-p** of **--machtigingen** &lt;machtigingen >: een door komma's gescheiden lijst met &lt;methode > =&lt;machtiging > paren.

**mobiele api update [opties] [servicenaam] [apiname]**

Deze opdracht werkt u de aangepaste opgegeven mobiele service-API.

Deze opdracht ondersteunt de volgende extra opties:

Deze opdracht ondersteunt de volgende extra opties:

+ **-p** of **--machtigingen** &lt;machtigingen >: een door komma's gescheiden lijst met &lt;methode > =&lt;machtiging > paren.
+ **-f** of **--force**: aangepaste wijzigingen in het metagegevensbestand machtigingen overschrijft.

**mobiele api [opties] [servicenaam] [apiname] verwijderen**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Deze opdracht verwijdert u het opgegeven mobiele service aangepaste API.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Opdrachten voor het beheren van de instellingen van de mobiele toepassing app

**lijst van mobiele appsetting [opties] [servicenaam]**

Deze opdracht geeft u de instellingen voor mobiele app voor de opgegeven service.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**mobiele appsetting toevoegen [opties] [servicenaam] [naam] [waarde]**

Met deze opdracht wordt de instelling van een aangepaste toepassing voor uw mobiele service toegevoegd.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**mobiele appsetting verwijderen [opties] [servicenaam] [naam]**

Met deze opdracht verwijdert u de instelling van de opgegeven toepassing voor uw mobiele service.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**mobiele appsetting weergeven [opties] [servicenaam] [naam]**

Met deze opdracht verwijdert u de instelling van de opgegeven toepassing voor uw mobiele service.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Hulpprogramma voor lokale instellingen beheren

Lokale instellingen zijn uw abonnement-ID en naam standaardaccount voor opslag.

**lijst van de config [opties]**

Met deze opdracht wordt de configuratie-instellingen weergegeven.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**configuratieset [opties] &lt;naam&gt;,&lt;waarde&gt;**

Met deze opdracht wijzigt een configuratie-instelling.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Opdrachten voor het beheren van Service Bus

Deze opdrachten gebruiken voor het beheren van uw account Service Bus

**SB naamruimte selectievakje [opties] &lt;naam >**

Controleer of een service bus naamruimte juridische en beschikbaar is.

**SB-naamruimte maken &lt;naam > &lt;locatie >**

Hiermee maakt u een naamruimte Service Bus.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**SB-naamruimte verwijderen &lt;naam >**

Een naamruimte verwijderen.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**lijst van SB-naamruimte**

Een overzicht van alle naamruimten gemaakt voor uw account.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**Vestigingsoverzicht SB-naamruimte**

Een lijst weergeven met alle locaties van de naamruimte beschikbaar.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**SB naamruimte weergeven &lt;naam >**

Meer informatie over een specifieke naamruimte weergegeven.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**SB-naamruimte controleren &lt;naam >**

Controleer of de naamruimte beschikbaar is.

## <a name="commands-to-manage-your-storage-objects"></a>Opdrachten voor het beheren van uw objecten voor gegevensopslag

###<a name="commands-to-manage-your-storage-accounts"></a>Opdrachten voor het beheren van uw opslag rekeningen

**lijst met opslag [opties]**

Deze opdracht geeft u de rekeningen voor opslag op uw abonnement.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**opslag-account weergeven [opties]<name>**

Met deze opdracht wordt informatie weergegeven over de opgegeven opslag account waaronder de eigenschappen URI en account.

**opslag-account maken [opties]<name>**

Met deze opdracht maakt de account van een opslag op basis van de opgegeven opties.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **e -** of **--label** &lt;label >: het label voor de opslag.
+ **-d** of **-Beschrijving** &lt;omschrijving >: de account omschrijving opslag.
+ **l -** of **--locatie** &lt;naam >: de geografische regio waar de opslag-account te maken.
+ **-een** of **--affiniteit groep** &lt;naam >: de affiniteit groep waaraan de opslag-account gekoppeld. 
+ **--type**: geeft het type account maken: een standaardopslag met redundantie-optie (LRS/ZRS/GRS/RAGRS) of Premium opslag (PLRS).

**opslag-account instellen [opties]<name>**

Deze opdracht werkt u de rekening voor opslag opgegeven.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **e -** of **--label** &lt;label >: het label voor de opslag.
+ **-d** of **-Beschrijving** &lt;omschrijving >: de account omschrijving opslag.
+ **l -** of **--locatie** &lt;naam >: de geografische regio waar de opslag-account te maken.
+ **--type**: het nieuwe type account aangeeft: een standaardopslag met redundantie-optie (LRS/ZRS/GRS/RAGRS) of Premium opslag (PLRS).

**opslag account verwijderen [opties]<name>**

Deze opdracht verwijdert u de account voor opslag opgegeven.

Deze opdracht ondersteunt de volgende extra opties:

**q -** of **--stille**: niet om bevestiging gevraagd. Deze optie gebruiken in geautomatiseerde scripts.

###<a name="commands-to-manage-your-storage-account-keys"></a>Opdrachten voor het beheren van uw opslag account sleutels

**Toetsen voor opslag account lijst [opties]<name>**

Met deze opdracht worden de primaire en secundaire sleutels voor de opslag van opgegeven account.

**opslag account sleutels vernieuwen [opties]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Opdrachten voor het beheren van uw opslag container

**opslag container lijst [opties] [voorvoegsel]**

Deze opdracht geeft u de lijst met opslag container voor een account voor opslag opgegeven. De opslag-account wordt opgegeven door de verbindingsreeks of de sleutel opslag account naam en account.

Deze opdracht ondersteunt de volgende extra opties:

+ **p -** of **-voorvoegsel** &lt;prefix >: voorvoegsel van de opslag container.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opslag-opdracht wordt uitgevoerd in de foutopsporingsmodus.

**opslag container weergeven [opties] [container]**
**opslag container maken [opties] [container]**

Met deze opdracht maakt een opslag container voor de opslag van opgegeven account. De opslag-account wordt opgegeven door de verbindingsreeks of de sleutel opslag account naam en account.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **p -** of **-voorvoegsel** &lt;prefix >: voorvoegsel van de opslag container.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag
+ **--debug**: de opslag-opdracht wordt uitgevoerd in de foutopsporingsmodus.

**opslag container verwijderen [opties] [container]**

Deze opdracht verwijdert u de opgegeven opslag container. De opslag-account wordt opgegeven door de verbindingsreeks of de sleutel opslag account naam en account.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **p -** of **-voorvoegsel** &lt;prefix >: voorvoegsel van de opslag container.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opslag-opdracht wordt uitgevoerd in de foutopsporingsmodus.

**opslag container instellen [opties] [container]**

Deze opdracht stelt ACL (toegangsbeheerlijst) voor de opslag container. De opslag-account wordt opgegeven door de verbindingsreeks of de sleutel opslag account naam en account.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **p -** of **-voorvoegsel** &lt;prefix >: voorvoegsel van de opslag container.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opslag-opdracht wordt uitgevoerd in de foutopsporingsmodus.

###<a name="commands-to-manage-your-storage-blob"></a>Opdrachten voor het beheren van uw opslag blob

**lijst van de blob Storage [opties] [container] [voorvoegsel]**

Met deze opdracht wordt een lijst van de blobs opslag in de container voor opslag opgegeven.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **p -** of **-voorvoegsel** &lt;prefix >: voorvoegsel van de opslag container.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opslag-opdracht wordt uitgevoerd in de foutopsporingsmodus.

**blob Storage [opties] [container] [blob] weergeven**

Deze opdracht geeft u de details van de blob opslag opgegeven.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **p -** of **-voorvoegsel** &lt;prefix >: voorvoegsel van de opslag container.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opdracht opslag in foutopsporing wordt uitgevoerd.

**blob Storage [opties] [container] [blob] verwijderen**

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **b -** of **--blob** &lt;blobName >: de naam van de blob storage te verwijderen.
+ **q -** of **--stille**: verwijdert de opgegeven blob Storage zonder bevestiging.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opdracht opslag in foutopsporing wordt uitgevoerd.

**uploaden van de blob Storage [opties] [bestand] [container] [blob]**

Met deze opdracht wordt het opgegeven bestand uploaden naar de blob specified\ opslag.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **b -** of **--blob** &lt;blobName >: de naam van de blob storage te uploaden.
+ **-t** of **--blobtype** &lt;blobtype >: het type van de blob storage: pagina of blokelement.
+ **p -** of **-Eigenschappen** &lt;eigenschappen >: de eigenschappen van de blob opslag van geüploade bestand. Eigenschappen zijn de sleutel = waarde paar s en gescheiden door plaats. Beschikbare eigenschappen zijn contentType, contentEncoding, contentLanguage en cacheControl.
+ **-m** of **metagegevens--** &lt;metagegevens >: de metagegevens van de blob storage voor geüploade bestand. Metagegevens zijn sleutel = waarde-paren een d van elkaar gescheiden door een puntkomma (;).
+ **--concurrenttaskcount** &lt;concurrenttaskcount >: het maximum aantal gelijktijdige upload aanvragen.
+ **q -** of **--stille**: de opgegeven blob Storage zonder bevestiging overschrijven.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opdracht opslag in foutopsporing wordt uitgevoerd.

**downloaden van de blob Storage [opties] [container] [blob] [doel]**

Met deze opdracht downloads de blob storage opgegeven.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **b -** of **--blob** &lt;blobName >: de naam van de blob storage.
+ **-d** of **--bestemming** [doel]: het downloaden van bestand of map doelpad.
+ **m -** of **--checkmd5**: het selectievakje md5sum voor het gedownloade bestand.
+ **--concurrenttaskcount** &lt;concurrenttaskcount > het maximum aantal gelijktijdige upload aanvragen
+ **q -** of **--stille**: het doelbestand overschreven zonder bevestiging.
+ **-een** of **--rekening met de naam** &lt;accountnaam >: de naam van de opslag.
+ **-k** of **--rekening sleutel** &lt;accountKey >: de sleutel opslag account.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingstekenreeks opslag.
+ **--debug**: de opdracht opslag in foutopsporing wordt uitgevoerd.

## <a name="commands-to-manage-sql-databases"></a>Opdrachten voor het beheren van SQL-Databases

Deze opdrachten gebruiken voor het beheren van uw Azure SQL-Databases

###<a name="commands-to-manage-sql-servers"></a>Opdrachten voor het beheren van SQL-Servers.

Deze opdrachten gebruiken voor het beheren van uw SQL-Servers

**maken van SQL server &lt;administratorLogin > &lt;administratorPassword > &lt;locatie >**

Maak een databaseserver

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**weergeven van SQL server &lt;naam >**

Serverdetails weergeven.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**lijst met SQL server**

De lijst met servers te verkrijgen.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**SQL server verwijderen &lt;naam >**

Een server verwijderen

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Opdrachten voor het beheren van SQL-Databases

Deze opdrachten gebruiken voor het beheren van uw SQL-Databases.

**SQL db maken [opties] &lt;serverName > &lt;databaseName > &lt;administratorPassword >**

Hiermee maakt u een database-instantie

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL db weergeven [opties] &lt;serverName > &lt;databaseName > &lt;administratorPassword >**

Informatie over de database worden weergegeven.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**lijst met SQL db [opties] &lt;serverName > &lt;administratorPassword >**

Een overzicht van de databases.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**SQL db verwijderen [opties] &lt;serverName > &lt;databaseName > &lt;administratorPassword >**

Hiermee verwijdert u een database.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Opdrachten voor het beheren van uw firewall-regels van SQL Server

Deze opdrachten gebruiken voor het beheren van uw firewall-regels van SQL Server

**SQL-firewallrule maken [opties] &lt;serverName > &lt;Naamregel > &lt;startIPAddress > &lt;endIPAddress >**

Een firewallregel maken voor een SQL-Server.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL-firewallrule weergeven [opties] &lt;serverName > &lt;Naamregel >**

Firewall regeldetails weergegeven.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**lijst met SQL-firewallrule [opties] &lt;serverName >**

Een overzicht van de firewall-regels.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL-firewallrule verwijderen [opties] &lt;serverName > &lt;Naamregel >**

Met deze opdracht verwijdert u een firewallregel.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Opdrachten voor het beheren van uw virtuele netwerken

Deze opdrachten gebruiken om uw virtuele netwerken beheren

**vnet netwerk maken [opties] &lt;locatie >**

Een virtueel netwerk maken.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**netwerk weergeven voor vnet &lt;naam >**

Details weergeven van een virtueel netwerk.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**lijst met vnet**

Een overzicht van alle bestaande virtuele netwerken.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**verwijderen netwerk vnet &lt;naam >**

Hiermee verwijdert u de opgegeven virtuele netwerk.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**netwerk exporteren [-pad]**

Geavanceerde configuratie kunt u de netwerkconfiguratie van uw lokaal exporteren. De geëxporteerde configuratie bevat instellingen voor DNS-server, instellingen virtueel netwerk, lokaal netwerk site-instellingen en andere instellingen.

**netwerk importeren [-pad]**

Een lokaal netwerkconfiguratie importeren.

**netwerk DNS-server registreren [opties] &lt;dnsIP >**

Registreren van een DNS-server die u wilt gebruiken voor naamomzetting in uw netwerkconfiguratie.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**lijst met DNS-server**

Een overzicht van alle DNS-servers die zijn geregistreerd in uw netwerkconfiguratie.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**de registratie van DNS-netwerk server [opties] &lt;dnsIP >**

Hiermee verwijdert u de vermelding van een DNS-server van de netwerkconfiguratie.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

