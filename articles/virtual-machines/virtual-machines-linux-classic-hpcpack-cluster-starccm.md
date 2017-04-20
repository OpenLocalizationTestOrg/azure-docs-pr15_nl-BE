<properties
 pageTitle="Uitvoeren van de STER-CCM + met HPC Pack op Linux VMs | Microsoft Azure"
 description="Een Pack van Microsoft HPC-cluster op Azure implementeren en uitvoeren van een STER-computerknooppunten CCM + project op meerdere Linux via een netwerk van RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Uitvoeren van de STER-CCM + met Microsoft HPC Pack op een RDMA Linux cluster in Azure
Dit artikel ziet u de implementatie van een cluster Microsoft HPC Pack op Azure en run een [CD adapco STAR-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) taak op meerdere Linux computerknooppunten die onderling zijn verbonden door middel van InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack biedt functies voor het uitvoeren van een groot aantal grootschalige HPC en parallelle toepassingen, inclusief toepassingen MPI in clusters van Microsoft Azure virtuele machines. HPC Pack ondersteunt ook waarop Linux HPC-toepassingen op Linux-computerknooppunt VMs die worden geïmplementeerd in een Pack HPC-cluster. Een inleiding tot het gebruik van Linux computerknooppunten met HPC Pack, kunt u [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Instellen van een Pack HPC-cluster
De implementatie van HPC Pack IaaS scripts downloaden vanaf het [Downloadcentrum](https://www.microsoft.com/en-us/download/details.aspx?id=44949) en lokaal uitpakken.

Azure PowerShell is een vereiste. PowerShell is niet geconfigureerd op de lokale computer, lees het artikel van [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Op het moment van schrijven van dit zijn de Linux-afbeeldingen van Azure Marketplace (met daarin de InfiniBand-stuurprogramma's voor Azure) voor 12 SLES en 6.5 CentOS CentOS 7.1. Dit artikel is gebaseerd op het gebruik van SLES 12. Voor het ophalen van de naam van alle Linux afbeeldingen die ondersteuning bieden voor HPC in de markt, kunt u de volgende PowerShell-opdracht uitvoeren:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

De uitvoer bevat de locatie waarin deze afbeeldingen beschikbaar zijn en de naam van de afbeelding (**imagenaam**) hoger in de sjabloon voor de implementatie wordt gebruikt.

Voordat u het cluster implementeert, moet u een sjabloonbestand HPC Pack implementatie samenstellen. Omdat we een klein cluster waarvoor ze bedoeld wordt het hoofd knooppunt de domeincontroller en hosten van een lokale SQL-database.

De volgende sjabloon wordt een hoofd knooppunt implementeren, maakt u een XML-bestand met de naam **MyCluster.xml**en de waarden van **SubscriptionId**, **StorageAccount**, **VMName**, **locatie**en **servicenaam** vervangen door uw eigen.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Start het head-knooppunt maken door de PowerShell-opdracht uit te voeren in een verhoogde opdrachtprompt:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Na 20 tot 30 minuten, moet het hoofd knooppunt worden. Kunt u deze via de portal Azure door te klikken op het pictogram van de **verbinding** van de virtuele machine.

Mogelijk moet u uiteindelijk oplossen van de DNS-doorstuurserver. Hiertoe start u DNS-beheer.

1.  Klik met de rechtermuisknop op de naam van de server in DNS-beheer, selecteer **Eigenschappen**en klik vervolgens op het tabblad **doorstuurservers** .

2.  Klik op de knop **bewerken** als u wilt verwijderen van alle doorstuurservers en klik vervolgens op **OK**.

3.  Zorg ervoor dat het selectievakje **aanbevolen basisservers gebruiken als er geen doorstuurservers** is geselecteerd en klik op **OK**.

## <a name="set-up-linux-compute-nodes"></a>Linux-computerknooppunten instellen
U implementeren de compute nodes van Linux met behulp van dezelfde sjabloon distributie die u gebruikt om het hoofd knooppunt te maken.

Kopieer het bestand **MyCluster.xml** van uw lokale computer naar het knooppunt head en de **NodeCount** -code bijwerken met het aantal knooppunten dat u wilt implementeren (< = 20). Zorg ervoor dat voldoende cores beschikbaar zijn in uw Azure quotum omdat elke instantie A9 16 cores in uw abonnement verbruiken. Als u wilt meer VMs in hetzelfde budget gebruiken, kunt u exemplaren van de A8 (8 cores) in plaats van de A9.

Op het knooppunt head, de implementatie van HPC Pack IaaS scripts te kopiëren.

Voer de volgende Azure PowerShell-opdrachten in een verhoogde opdrachtprompt:

1.  **Toevoegen-AzureAccount** verbinding maken met uw abonnement Azure worden uitgevoerd.

2.  Als er meerdere abonnementen, **Get-AzureSubscription** als ze worden uitgevoerd.

3.  Een standaardabonnement ingesteld met de **xxxx selecteren-AzureSubscription - SubscriptionName-standaard** opdracht.

4.  Uitvoeren van **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** starten implementatie van Linux computerknooppunten.

    ![Implementatie van de hoofd-node in actie][hndeploy]

Open het hulpprogramma HPC Pack Cluster Manager. Na enkele minuten wordt computerknooppunten Linux regelmatig weergegeven in een lijst met computerknooppunten cluster. Met de modus klassieke implementatie VMs IaaS opeenvolgend gemaakt. Dus als het aantal knooppunten belangrijk is, kunt alle ingezet getting nemen een aanzienlijke hoeveelheid tijd.

![Linux-knooppunten in Cluster Manager van HPC Pack][clustermanager]

Nu dat alle knooppunten actief in het cluster, worden er aanvullende infrastructuur instellingen te maken.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Instellen van een Azure bestandsshare voor knooppunten met Windows en Linux
U kunt de Azure File service scripts, toepassingspakketten en gegevensbestanden worden opgeslagen. Azure bestand biedt mogelijkheden voor CIFS op Azure Blob-opslag als een blijvende opslag. Let erop dat dit niet de meest schaalbare oplossing is, maar dit is de eenvoudigste en speciale VMs niet nodig.

Maak een bestandsshare van Azure door de instructies in het artikel [aan de slag met Azure bestandsopslag op Windows](..\storage\storage-dotnet-how-to-use-files.md).

De naam van de account van uw opslag bewaren als **saname**, de bestandsshare-naam als de **sharenaam**en de sleutel van de account opslag als **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Koppel de Azure bestandsshare op het knooppunt head
Open een opdrachtprompt en voer de volgende opdracht voor het opslaan van de referenties in de kluis van de lokale computer:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Klik, als u wilt koppelen op de bestandsshare Azure, uitvoeren:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>De bestandsshare Azure koppelen op Linux-computerknooppunten
Een handig hulpmiddel dat wordt geleverd met HPC Pack is het hulpprogramma clusrun. U kunt dit opdrachtregelprogramma dezelfde opdracht op een reeks computerknooppunten gelijktijdig worden uitgevoerd. In ons geval wordt het gebruikt voor de bestandsshare Azure koppelen en behouden van het overleven van de computer opnieuw hebt opgestart.
Voer de volgende opdrachten in een verhoogde opdrachtprompt op het knooppunt head.

De gekoppelde map maken:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

De bestandsshare Azure koppelen:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

De share mount aanhouden:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installeren van de STER-CCM +
Azure VM exemplaren A8 en A9 bieden InfiniBand en RDMA mogelijkheden. De kernel-stuurprogramma's van deze mogelijkheden zijn beschikbaar voor Windows Server 2012 R2, SUSE 12 6.5 CentOS en CentOS 7.1 afbeeldingen op Marketplace Azure. MPI Microsoft en Intel MPI (versie 5.x) zijn de twee MPI-bibliotheken die deze stuurprogramma's in Azure ondersteunen.

CD adapco STAR-CCM + 11.x vrijgeven en hoger wordt meegeleverd met Intel MPI versie 5.x, zodat InfiniBand-ondersteuning voor Azure opgenomen is.

Ophalen van de Linux64 STER-CCM + pakket vanaf de [CD adapco portal](https://steve.cd-adapco.com). In ons geval hebben we versie 11.02.010 in gemengde precisie gebruikt.

Klik op het knooppunt hoofd in de **/hpcdata** Azure bestandsshare, maken een shellscript met de naam **setupstarccm.sh** met de volgende inhoud. Dit script wordt uitgevoerd op elk computerknooppunt voor het instellen van de STER-CCM + lokaal.

#### <a name="sample-setupstarcmsh-script"></a>Voorbeeldscript voor setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Nu voor het instellen van de STER-CCM + op alle uw Linux computerknooppunten, open een opdrachtprompt en voer de volgende opdracht uit:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Terwijl de opdracht wordt uitgevoerd, kunt u het CPU-gebruik controleren met behulp van de warmte-kaart van Cluster Manager. Na enkele minuten alle knooppunten moeten correct zijn ingesteld.

## <a name="run-star-ccm-jobs"></a>Uitvoeren van de STER-CCM + taken
HPC Pack STAR uitvoeren wordt gebruikt om de mogelijkheden van job scheduler-CCM + taken. Hiervoor moeten we de ondersteuning van een aantal scripts die worden gebruikt voor de taak starten en uitvoeren van STAR-CCM +. De ingevoerde gegevens wordt bewaard op de bestandsshare Azure eerste voor eenvoud.

De volgende PowerShell script wordt gebruikt om een STER in de wachtrij-CCM + project. Het duurt drie argumenten:

*  Naam van het model

*  Het aantal knooppunten moet worden gebruikt

*  Het aantal cores op elk knooppunt moet worden gebruikt

Omdat STAR-CCM + vult de geheugenbandbreedte, is het meestal beter met minder cores per computerknooppunten en nieuwe knooppunten toe te voegen. Het exacte aantal cores per knooppunt, is afhankelijk van de processorfamilie en de snelheid van interconnect.

De knooppunten zijn exclusief voor de taak toegewezen en kunnen niet worden gedeeld met andere taken. De taak wordt niet rechtstreeks als een taak MPI gestart. Het **runstarccm.sh** shellscript zal MPI starten.

Het model van de invoer en het script **runstarccm.sh** worden opgeslagen in de **/hpcdata** -share die eerder is gekoppeld.

Logboekbestanden worden benoemd met de taak-ID en worden opgeslagen in de **/hpcdata delen**, samen met de STER-CCM + uitvoerbestanden.


#### <a name="sample-submitstarccmjobps1-script"></a>Voorbeeldscript voor SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
**Runner.java** vervangen door je favoriete STER-CCM + Java model starten en registratie code.

#### <a name="sample-runstarccmsh-script"></a>Voorbeeldscript voor runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

In onze test hebben we een Power-On-Demand licentie-token gebruikt. Voor die u moet de variabele **$CDLMD_LICENSE_FILE** ingesteld op **1999@flex.cd-adapco.com** en de sleutel in de optie **- podkey** van de opdracht.

Na een initialisatie script extracten--van de **$CCP_NODES_CORES** omgevingsvariabelen die HPC Pack--de lijst van knooppunten aan het bouwen van een hostfile die gebruikmaakt van het startprogramma voor MPI. Deze hostfile wordt de lijst met namen van de compute nodes die worden gebruikt voor het project, één naam per regel bevatten.

De indeling van **$CCP_NODES_CORES** volgt deze notatie:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Waar:

* `<Number of nodes>`het aantal knooppunten dat is toegewezen aan deze taak is.

* `<Name of node_n_...>`is de naam van elk knooppunt is toegewezen aan deze taak.

* `<Cores of node_n_...>`het aantal cores op het knooppunt is toegewezen aan deze taak is.

Het aantal kernen (**$NBCORES**) wordt ook berekend op basis van het aantal knooppunten (**$NBNODES**) en het aantal cores per knooppunt (geleverd als parameter **$NBCORESPERNODE**).

Voor de MPI-opties zijn die worden gebruikt in combinatie met Intel MPI op Azure:

*   `-mpi intel`Intel MPI opgeven.

*   `-fabric UDAPL`Azure InfiniBand woorden gebruiken.

*   `-cpubind bandwidth,v`voor een optimale bandbreedte voor MPI met STER-CCM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Intel MPI met Azure InfiniBand werken en het vereiste aantal cores per knooppunt ingesteld.

*   `-batch`Start de STER-CCM + in de batchmodus met geen gebruikersinterface.


Een taak starten, zorg er ten slotte dat de knooppunten actief zijn en on line in Clusterbeheer. Via een PowerShell-opdrachtprompt, voert u deze:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Stoppen met knooppunten
Later op nadat u met de tests bent klaar, kunt u de volgende opdrachten voor HPC Pack PowerShell stoppen en starten van knooppunten:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Volgende stappen
Probeer andere Linux-workloads te voeren. Zie bijvoorbeeld:

* [NAMD uitvoeren met Microsoft HPC Pack op Linux-computerknooppunten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [OpenFOAM met Microsoft HPC Pack uitvoeren op een cluster RDMA Linux in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
