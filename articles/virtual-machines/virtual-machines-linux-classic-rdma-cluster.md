<properties
 pageTitle="RDMA Linux cluster MPI-toepassingen uit te voeren | Microsoft Azure"
 description="Maak een Linux cluster van grootte, H16r, H16mr, A8 of VMs A9 bij het netwerk Azure RDMA MPI-toepassingen uitvoeren"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Instellen van een RDMA Linux cluster MPI-toepassingen uit te voeren


Informatie over het instellen van een RDMA Linux cluster in Azure met [H-serie of computerintensieve A-serie VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md) parallelle Message Passing Interface (MPI)-toepassingen uit te voeren. Dit artikel bevat stappen voor het voorbereiden van een afbeelding met een Linux HPC Intel MPI uitvoeren op een cluster. Vervolgens kunt u een cluster van VMs met behulp van deze afbeelding en een van de grootte van de RDMA kunnen Azure VM (momenteel H16r, H16mr, A8 en A9) implementeren. Het cluster met MPI toepassingen uitvoeren die efficiënt via een lage latentie communiceren, hoge doorvoer het netwerk op afstand direct memory Access-technologie (RDMA).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Cluster implementatie-opties

Methoden voor het maken van een cluster Linux RDMA met of zonder een job scheduler kunt u volgen.


* **Azure CLI-scripts** - zoals later in dit artikel gebruiken [Azure opdrachtregelinterface](../xplat-cli-install.md) (CLI) script voor de implementatie van een cluster van VMs RDMA staat. De CLI in de modus voor servicebeheer maakt de clusterknooppunten serie in het implementatiemodel klassiek dus veel computerknooppunten implementeren kan enkele minuten duren. Als u verbinding met het RDMA wanneer u het implementatiemodel klassiek, implementeren de VMs in dezelfde service cloud.

* **Sjablonen voor azure Resource Manager** - ook kunt u de Resource Manager implementatiemodel voor de implementatie van een cluster die is verbonden met het netwerk RDMA van VMs RDMA staat. U kunt [uw eigen sjabloon maken](../resource-group-authoring-templates.md), of de [Azure quickstart sjablonen](https://azure.microsoft.com/documentation/templates/) voor sjablonen die zijn ingebracht door Microsoft of de Gemeenschap voor de implementatie van de oplossing die u wilt controleren. Sjablonen voor Resource Manager kunnen bieden een snelle en betrouwbare manier om een Linux cluster implementeren. Als u verbinding met het RDMA wanneer u het implementatiemodel Resource Manager, de VMs in bepaalde beschikbaarheid te implementeren.

* **HPC Pack** - een Pack van Microsoft HPC-cluster maken in Azure en RDMA kunnen computerknooppunten waarop een ondersteunde Linux-distributie voor toegang tot het netwerk RDMA toevoegen. Zie [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Voorbeeld van de implementatiestappen in klassiek model

De volgende stappen laten zien hoe met behulp van de CLI Azure een VM SUSE Linux Enterprise Server (SLES) 12 SP1 HPC van Azure Marketplace implementeren, aanpassen en een aangepaste VM-afbeelding maken. Gebruik de installatiekopie vervolgens script voor de implementatie van een cluster van VMs RDMA staat. 

>[AZURE.TIP]Gebruik vergelijkbare stappen voor de implementatie van een cluster van RDMA kunnen VMs op basis van andere ondersteunde HPC afbeeldingen op Marketplace Azure. Enkele stappen kunnen verschillen, zoals vermeld. Intel MPI is bijvoorbeeld opgenomen en geconfigureerd in slechts enkele van deze afbeeldingen. En als u een VM SLES 12 HPC in plaats van een VM SLES 12 SP1 HPC implementeert, moet u de RDMA-stuurprogramma's bijwerken. Zie voor meer informatie [over de A8, A9, A10, en A11 intensieve exemplaren](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Vereisten

*   **Clientcomputer** - moet u een Mac, Linux of Windows gebaseerde client computer kan communiceren met Azure. Deze stappen wordt ervan uitgegaan dat u een Linux-client.

*   **Azure abonnement** - als u niet een abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) in een paar minuten. Overweeg een pay-as-you-go abonnement of andere inkoopopties voor grotere clusters. 

*   **Beschikbaarheid voor VM-grootte** - zijn RDMA staat momenteel het volgende exemplaar afmetingen: H16r, H16mr, A8 en A9. [Producten per regio](https://azure.microsoft.com/regions/services/) controleren beschikbaar in Azure regio's. 

*   **Cores quota** - moet u mogelijk het quotum van de boormonsters voor de implementatie van een cluster van intensieve VMs verhogen. Bijvoorbeeld, moet u ten minste 128 cores als u implementeren 8 A9 VMs wilt, zoals in dit artikel. Uw abonnement kan ook beperken het aantal cores, die u in bepaalde families VM omvang implementeren kunt, met inbegrip van de H-reeks. Voor het aanvragen van een quotum verhogen, [open een verzoek voor ondersteuning van online klant](../azure-supportability/how-to-create-azure-support-request.md) kosteloos. 

*   **Azure CLI** - [installeren](../xplat-cli-install.md) de CLI Azure en [verbinding maken met uw abonnement Azure](../xplat-cli-connect.md) vanaf de clientcomputer.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Stap 1. Inrichten van een SLES 12 SP1 HPC-VM

Aanmelden bij Azure met de CLI Azure, en voer `azure config list` om te bevestigen dat de uitvoer servicebeheer-modus toont. Als dat niet het geval is, wordt met deze opdracht de modus instellen:


    azure config mode asm


Typ het volgende als u alle abonnementen die u mag gebruiken:


    azure account list

De huidige actieve abonnement wordt aangeduid met `Current` ingesteld op `true`. Als dit abonnement niet degene die u gebruiken wilt voor het maken van het cluster, stelt u de juiste abonnements-Id als het actieve abonnement:

    azure account set <subscription-Id>

Overzicht van de algemeen beschikbare afbeeldingen SLES 12 SP1 HPC in Azure, een vergelijkbaar met de volgende opdracht wordt uitgevoerd, ervan uitgaande dat de shell-omgeving ondersteunt **grep**:


    azure vm image list | grep "suse.*hpc"

Nu een VM RDMA kan met een afbeelding SLES 12 SP1 HPC inrichten door het uitvoeren van een opdracht met de volgende strekking:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

waar

* De grootte (A9 in dit voorbeeld) een van de formaten VM RDMA geschikt is.

* De externe SSH-poortnummer (22 in dit voorbeeld de standaard SSH wordt) is een geldig poortnummer. Het interne nummer van de SSH-poort is ingesteld op 22.

* Een nieuwe wolk service is gemaakt in de Azure gebied dat is opgegeven door de locatie. Geef een locatie waarin u kiest de VM-grootte beschikbaar is.

* Op dit moment de naam van de afbeelding SLES 12 SP1 kan worden `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` of `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` voor ondersteuning met prioriteit SUSE (extra kosten van toepassing).

### <a name="step-2-customize-the-vm"></a>Stap 2. Aanpassen van de VM

Nadat de VM is voltooid wordt ingericht, SSH voor VM met behulp van de VM externe IP-adres (of DNS-naam) en de externe poort nummer u geconfigureerd en aan te passen. Zie voor details voor verbinding [het aanmelden bij een virtuele Machine wordt uitgevoerd op Linux](virtual-machines-linux-mac-create-ssh-keys.md). Opdrachten uitvoeren als de gebruiker die u hebt geconfigureerd op de VM, tenzij de toegang tot de hoofdmap is vereist om een stap te voltooien.

>[AZURE.IMPORTANT]Microsoft Azure biedt geen toegang tot de hoofdmap te Linux VMs. Administratieve om toegang te krijgen wanneer verbonden als een gebruiker op de VM, uitvoeren van opdrachten met behulp van `sudo`.

* **Updates** - updates installeren met behulp van **zypper**. U kunt ook hulpprogramma's voor NFS installeren. 

    >[AZURE.IMPORTANT]In een VM SLES 12 SP1 HPC is het raadzaam dat u de kernel-updates die de oorzaak van problemen met de RDMA Linux stuurprogramma's niet toepast.

* **Intel MPI** - de installatie van Intel MPI op de VM SLES 12 SP1 HPC voltooien door de volgende opdracht uit te voeren:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Geheugen vergrendelen** - codes voor MPI het beschikbare geheugen vergrendelen voor RDMA, toevoegen of wijzigen van de volgende instellingen in het bestand /etc/security/limits.conf. (U moet toegang tot de hoofdmap om dit bestand te bewerken.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Voor testdoeleinden kunt u ook een memlock instellen op onbeperkt. Bijvoorbeeld: `<User or group name>    hard    memlock unlimited`. Zie voor meer informatie, [Bekend van de beste methoden voor de instelling vergrendeld geheugengrootte](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **SSH-sleutels voor SLES VMs** - genereren SSH sleutels vertrouwensrelatie tot stand brengen voor de gebruikersaccount van de compute nodes in het cluster SLES tijdens het uitvoeren van taken MPI. (Als u een CentOS-gebaseerde HPC VM hebt geïmplementeerd, niet Volg deze stap. Zie de instructies verderop in het artikel passwordless SSH vertrouwensrelatie tussen de clusterknooppunten instellen nadat u de installatiekopie vastleggen en het cluster implementeren.) 

    Voer de volgende opdracht om de SSH-sleutels maken. Wanneer u wordt gevraagd om invoer, druk op Enter om de sleutels op de standaardlocatie zonder een wachtwoordzin te genereren.

        ssh-keygen

    De openbare sleutel toevoegen aan het bestand authorized_keys voor bekende openbare sleutels.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    Bewerken in de ~/.ssh map, of maak het bestand 'config'. Geef het IP-adresbereik van het particuliere netwerk die u wilt gebruiken in Azure (10.32.0.0/16 in dit voorbeeld):

        host 10.32.0.*
        StrictHostKeyChecking no

    U kunt ook een overzicht van het particuliere netwerk-IP-adres van elke VM in het cluster als volgt:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Configureren van `StrictHostKeyChecking no` een potentieel beveiligingsrisico kunt maken wanneer u een specifiek IP-adres of -bereik is niet opgegeven.

* **Toepassingen** - installeert u alle toepassingen die u nodig hebt op deze VM of andere aanpassingen uitvoeren voordat u de installatiekopie vastleggen.

### <a name="step-3-capture-the-image"></a>Stap 3. De installatiekopie vastleggen

Voor het vastleggen van de afbeelding, moet u eerst de volgende opdracht uitvoeren in Linux VM. Met deze opdracht de VM deprovisions maar behoudt gebruikersaccounts en SSH-sleutels die u hebt ingesteld.

```
sudo waagent -deprovision
```

Van uw computer, voert u de volgende Azure CLI-opdrachten voor het vastleggen van de afbeelding. Zie [een klassieke Linux virtuele machine als een afbeelding vastleggen](virtual-machines-linux-classic-capture-image.md) voor meer informatie.  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

De VM-afbeelding wordt vastgelegd voor het gebruik en de VM wordt verwijderd nadat u deze opdrachten uitvoeren. U hebt nu uw aangepaste installatiekopie implementeren van een cluster.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Stap 4. Een cluster met de installatiekopie implementeren

Wijzig de volgende Bash script met de juiste waarden voor uw omgeving en uitvoeren vanaf de clientcomputer. Omdat Azure de VMs serie in het implementatiemodel klassiek implementeert, duurt het een paar minuten de 8 A9 VMs voorgesteld in dit script implementeren.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Overwegingen voor een CentOS HPC-cluster

Als u instellen op een op basis van een van de afbeeldingen op basis van CentOS HPC in de markt Azure in plaats van 12 SLES voor HPC-cluster wilt, voert u de algemene stappen in de vorige sectie. Opmerking de volgende verschillen wanneer u creëren en configureren van de VM:

1. Intel MPI is al geïnstalleerd op een VM uit een afbeelding op basis van CentOS HPC ingericht. 

2. Instellingen voor het geheugen vergrendelen zijn al in /etc/security/limits.conf-bestand van de VM toegevoegd.

2. Genereren geen SSH sleutels op de VM u inrichten voor het vastleggen. Wij raden het instellen van verificatie op basis van gebruiker nadat u de cluser geïmplementeerd. Zie de volgende sectie.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Passwordless SSH vertrouwen op het cluster instellen

CentOS-gebaseerde HPC-cluster worden twee methoden voor het instellen van een vertrouwensrelatie tussen de compute nodes: host-gebaseerde verificatie en verificatie op basis van gebruiker. Host-gebaseerde verificatie is buiten het bereik van dit artikel en in het algemeen moet worden uitgevoerd via een script extensie tijdens de implementatie. Verificatie op basis van gebruiker is handig voor het instellen van een vertrouwensrelatie na de implementatie en vereist het genereren en delen van SSH sleutels tussen de compute nodes in het cluster. Deze methode is algemeen bekend als passwordless SSH-aanmelding en is vereist bij het uitvoeren van taken MPI. 

Een voorbeeldscript van de communautaire bijdrage is beschikbaar op [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) eenvoudig verificatie op een CentOS-gebaseerde HPC-cluster in te schakelen. Downloaden en gebruiken van dit script met behulp van de volgende stappen uit. U kunt ook dit script wijzigen of gebruiken een andere methode vast te stellen tussen de clusterknooppunten compute passwordless SSH-verificatie.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Het script wordt uitgevoerd, moet u het voorvoegsel van het subnet-IP-adressen weet. Het voorvoegsel opvragen door de volgende opdracht uit te voeren op een van de clusterknooppunten. De uitvoer ziet er ongeveer uit zoals in 10.1.3.5 en het voorvoegsel is de/m 10.1.3 gedeelte.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Voer nu het script uit met drie parameters: de algemene gebruikersnaam op de compute nodes, het gemeenschappelijk wachtwoord voor de gebruiker op de compute nodes en het subnetvoorvoegsel dat door de vorige opdracht is geretourneerd.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Dit script doet het volgende:

* Een map gemaakt op de hostknooppunt met de naam .ssh, die is vereist voor passwordless inloggen. 
* Een configuratiebestand gemaakt in de .ssh directory geeft passwordless inloggen om te kunnen aanmelden vanaf elk knooppunt in het cluster. 
* Worden bestanden gemaakt met de namen van de nodes en IP-adressen voor alle knooppunten in het cluster. Deze bestanden blijven nadat het script is uitgevoerd voor later gebruik. 
* Hiermee maakt u een persoonlijke en openbare sleutelpaar voor elk clusterknooppunt met inbegrip van de hostknooppunt en worden posten gemaakt in het bestand authorized_keys.

>[AZURE.WARNING]Dit script wordt uitgevoerd, kan een potentieel beveiligingsrisico maken. Zorg ervoor dat de openbare-sleutelgegevens in ~/.ssh niet wordt verstrekt.


## <a name="configure-intel-mpi"></a>Intel MPI configureren

MPI-toepassingen op Linux-RDMA Azure uitgevoerd, moet u bepaalde specifieke Intel MPI omgevingsvariabelen configureren. Hier volgt een voorbeeldscript Bash voor het configureren van de variabelen die nodig zijn voor het uitvoeren van een toepassing. Wijzig het pad naar mpivars.sh voor uw installatie van Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

De indeling van het bestand met de host is als volgt. Een regel voor elk knooppunt in het cluster wilt toevoegen. Geef de particuliere IP-adressen van de VNet eerder hebt gedefinieerd, niet door DNS-namen. Voor twee hosts IP-adressen 10.32.0.1 en 10.32.0.2 bevat het bestand bijvoorbeeld de volgende:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI worden uitgevoerd op een eenvoudig cluster met twee knooppunten

Als u nog niet hebt gedaan, eerst instellen de omgeving voor Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Een eenvoudige MPI-opdracht uitvoeren

Een eenvoudige MPI-opdracht uitvoeren op een van de compute nodes dat MPI correct is geïnstalleerd en communiceren kan tussen ten minste dat twee knooppunten berekenen, weergeven. De volgende **mpirun** opdracht de opdracht **hostname** op twee knooppunten wordt uitgevoerd.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
De uitvoer moet staan de namen van de knooppunten die u hebt doorgegeven als invoer voor `-hosts`. Bijvoorbeeld wordt een opdracht **mpirun** met twee knooppunten uitvoer weergegeven met de volgende strekking:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Een benchmark MPI uitvoeren

De volgende Intel MPI-opdracht wordt uitgevoerd een benchmark pingpong om te controleren of de configuratie van het cluster en de verbinding met het netwerk RDMA.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Op een werkende cluster met twee knooppunten ziet u uitvoer van de volgende strekking. Op het netwerk RDMA Azure verwachten latentie op of onder 3 microseconden voor het bericht maximaal 512 bytes het formaat.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Volgende stappen

* Probeer te implementeren en uw MPI Linux-toepassingen uitvoert op uw Linux-cluster.

* Zie de [documentatie van Intel MPI Library](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) voor richtlijnen op Intel MPI.

* [Quickstart-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) probeert te maken van een Lustre Intel cluster met behulp van een afbeelding op basis van CentOS HPC. Voor meer informatie, Zie deze [blog posten](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
