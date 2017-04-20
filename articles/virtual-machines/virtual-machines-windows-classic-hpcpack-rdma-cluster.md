<properties
 pageTitle="Instellen van een cluster met Windows RDMA MPI-toepassingen uit te voeren | Microsoft Azure"
 description="Informatie over het maken een Pack van Windows HPC-cluster met een grootte van H16r, H16mr, A8 of VMs A9 bij het netwerk Azure RDMA MPI apps uitvoeren."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Instellen van een cluster met Windows RDMA met HPC Pack MPI-toepassingen uit te voeren

Instellen van een cluster van Windows RDMA in Azure met [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) en [H-serie of computerintensieve exemplaren van een reeks](virtual-machines-windows-a8-a9-a10-a11-specs.md) parallelle Message Passing Interface (MPI)-toepassingen uit te voeren. Bij het instellen van de RDMA staat, op basis van Windows Server knooppunten in een cluster HPC Pack, communiceert toepassingen voor MPI efficiënt een lage latentie netwerk hoge doorvoersnelheid in Azure die is gebaseerd op externe direct memory access (RDMA)-technologie.

Als u MPI werklasten worden uitgevoerd op Linux VMs die toegang het netwerk RDMA Azure wilt tot, Zie [een cluster van Linux-RDMA MPI-toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>Implementatieopties Pack HPC-cluster
Microsoft HPC Pack is een hulpprogramma voor zover zonder extra kosten voor het maken van HPC-clusters in ruimten of in Azure Windows of Linux HPC toepassingen uitvoeren. HPC Pack bevat een runtimeomgeving voor de Microsoft-implementatie van het bericht wordt doorgegeven Interface voor Windows (MS-MPI). In combinatie met RDMA kunnen exemplaren waarop een ondersteunde Windows-besturingssysteem wordt uitgevoerd, biedt HPC Pack een efficiënte optie MPI Windows toepassingen die toegang hebben tot het netwerk Azure RDMA uit te voeren. 

Dit artikel introduceert twee scenario's en koppelingen naar gedetailleerde richtlijnen voor het instellen van een cluster Winodws RDMA met Microsoft HPC Pack. 

* Scenario 1. Implementatie van intensieve werknemer rol exemplaren (PaaS)

* Scenario 2. Computerknooppunten in intensieve VMs (IaaS) implementeren

Zie voor algemene vereisten voor het gebruik van computer-intensieve exemplaren met Windows, [H-serie en intensieve A-serie VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scenario 1. Implementatie van intensieve werknemer rol exemplaren (PaaS)

Uit een bestaand Pack HPC-cluster toevoegen extra compute resources in Azure werknemer rol gevallen (Azure knooppunten) uitgevoerd in een cloud-service (PaaS). Deze functie, ook wel 'burst naar Azure"van HPC Pack, ondersteunt een aantal formaten voor de rol van werknemer exemplaren. Wanneer u de Azure knooppunten toevoegt, gewoon opgeven een van de RDMA-compatibele formaten.

Overwegingen en stappen voor burst aan RDMA kunnen volgen Azure exemplaren van een bestaande (meestal op-ruimten) cluster. Soortgelijke procedures werknemer rol exemplaren toevoegen aan een HPC Pack head-knooppunt dat wordt geïmplementeerd in een Azure VM gebruiken.

>[AZURE.NOTE] Zie [een cluster van hybride met HPC Pack instellen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)voor een zelfstudie met burst naar Azure Pack HPC. Let op de overwegingen in de volgende stappen uit die van toepassing zijn op RDMA kan Azure knooppunten.

![Burst naar Azure][burst]

### <a name="steps"></a>Werk

4. **Implementeren en configureren van een hoofd knooppunt HPC Pack 2012 R2**

    Het meest recente HPC-installatiepakket downloaden vanaf het [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Zie [HPC Pack Getting Started Guide](https://technet.microsoft.com/library/jj884144.aspx) en [Burst Azure werknemer exemplaren met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)voor de vereisten en instructies voor te bereiden voor een package-burst Azure-implementatie.

5. **Een certificaat configureren in het abonnement Azure**

    Configureer een certificaat beveiliging van de verbinding tussen het hoofd-knooppunt en de Azure. Zie [scenario's voor het configureren van het certificaat Azure Management voor HPC Pack](http://technet.microsoft.com/library/gg481759.aspx)voor opties en procedures. HPC Pack installeert voor test-implementaties, een standaard Microsoft HPC Azure Management certificaat kunt u snel naar uw abonnement Azure uploaden.

6. **Maak een nieuwe wolk service en een account voor opslag**

    De klassieke Azure portal gebruik te maken van een cloud-service en een opslag-account voor de implementatie in een regio waar de RDMA kunnen exemplaren beschikbaar zijn.

7. **Een knooppunt Azure-sjabloon maken**

    Gebruik de Wizard knooppunt HPC Cluster Manager maken. Zie [een knooppunt Azure-sjabloon maken](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) in 'Stappen te implementeren Azure knooppunten met Microsoft HPC Pack'.

    Voor de eerste tests is het raadzaam een handmatige beschikbaarheid-beleid te configureren in de sjabloon.

8. **Knooppunten toevoegen aan het cluster**

    Gebruik de Wizard knooppunten toevoegen HPC Cluster Manager. Zie [Azure knooppunten toevoegen aan het Windows HPC-Cluster](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add)voor meer informatie.

    Wanneer de grootte van de knooppunten op te geven, selecteert u een van de grootte van de RDMA geschikt exemplaar.
    
    >[AZURE.NOTE]In elke burst naar Azure-implementatie met de intensieve exemplaren HPC Pack automatisch een minimum van 2 RDMA kunnen instanties (zoals A8) geïmplementeerd als proxy knooppunten, naast de Azure werknemer rol exemplaren die u opgeeft. De proxy-knooppunten gebruiken cores die zijn toegewezen aan het abonnement en gelden de tarieven en de rol van werknemer Azure exemplaren.

9. **(Gegevens leveren) start de knooppunten te brengen en on line taken uitvoeren**

    De knooppunten te selecteren en gebruik de actie **Start** HPC Cluster Manager. Bij het inrichten is voltooid, selecteert u de knooppunten en de **On line brengen** actie HPC Cluster Manager. De knooppunten zijn taken uitvoeren.

10. **Taken aan het cluster**

    HPC Pack taak indiening's gebruiken om het cluster taken uitvoeren. Zie [Microsoft HPC Pack: Project Management](http://technet.microsoft.com/library/jj899585.aspx).

11. **Stoppen (identiteitsgegevens) de knooppunten**

    Wanneer u klaar bent met het uitvoeren van taken, off line nemen van de knooppunten en de **Stop** actie HPC Cluster Manager.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scenario 2. Computerknooppunten in intensieve VMs (IaaS) implementeren

In dit scenario wordt u het hoofd knooppunt HPC Pack implementeert en cluster compute nodes op VMs deel uitmaakt van een Active Directory-domein in een virtueel netwerk van Azure. HPC Pack biedt een aantal [implementatieopties in Azure VMs](virtual-machines-linux-hpcpack-cluster-options.md), met inbegrip van geautomatiseerde implementatie scripts en Azure quickstart sjablonen. Als u bijvoorbeeld de overwegingen en de onderstaande stappen u helpen de meeste van dit proces automatiseren met het [implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) .

![Cluster in Azure VMs][iaas]



### <a name="steps"></a>Werk

1. **De hoofd-knooppunt van een cluster maken en knooppunt VMs berekenen door het implementatiescript HPC Pack IaaS wordt uitgevoerd op een clientcomputer**

    De HPC Pack IaaS implementatiescript downloaden vanaf het [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922).

    Het configuratiebestand script maken om voor te bereiden op de clientcomputer, en het script wordt uitgevoerd, Zie [maken van een HPC-Cluster met het implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Houd rekening met de volgende aanvullende overwegingen voor de implementatie van RDMA kunnen computerknooppunten:
    
    * **Virtueel netwerk** - Geef een nieuw virtueel netwerk in een regio waar de RDMA geschikt exemplaar u wilt gebruiken beschikbaar is.

    * **Het besturingssysteem Windows Server** - ondersteuning van RDMA verbindingen, het opgeven van een Windows Server 2012 R2 of Windows Server 2012 besturingssysteem voor de compute node VMs.

    * **Cloud services** - implementatie van uw hoofd knooppunt in een cloud-service en de computerknooppunten in een andere cloud-service het beste.

    * **Grootte van het knooppunt Head** - voor dit scenario kunt u een grootte van ten minste A4 (Extra groot) voor de hoofd-knooppunt.

    * **Uitbreiding van de HpcVmDrivers** - script voor de implementatie de Azure VM-Agent en de extensie HpcVmDrivers wordt automatisch geïnstalleerd wanneer u de grootte van A8 of A9 computerknooppunten met een Windows-besturingssysteem implementeert. HpcVmDrivers worden stuurprogramma's geïnstalleerd op de compute node VMs zodat ze verbinding met het netwerk RDMA maken kunnen.

    * **Configuratie van het cluster** - script voor de implementatie stelt automatisch het cluster HPC Pack 5-topologie (alle knooppunten op het bedrijfsnetwerk). Deze topologie is vereist voor alle implementaties van HPC Pack cluster in VMs. De netwerktopologie van het cluster niet later wijzigen.

2. **Breng de compute nodes on line taken uitvoeren**

    Selecteer de knooppunten en de **On line brengen** actie HPC Cluster Manager. De knooppunten zijn taken uitvoeren.

3. **Taken aan het cluster**

    Verbinding maken met het knooppunt statutaire taken of een computer op ruimten instellen om dit te doen. Zie voor informatie [Verzenden taken naar een HPC-cluster in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Off line nemen van de knooppunten en stoppen (toewijzing) ze**

    Wanneer u klaar bent met het uitvoeren van taken, worden de knooppunten die off line HPC Cluster Manager. Azure beheerprogramma's vervolgens gebruiken ze om af te sluiten.



## <a name="run-mpi-applications-on-the-cluster"></a>MPI-toepassingen worden uitgevoerd op het cluster

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Voorbeeld: Mpipingpong uitgevoerd op een Pack van HPC-cluster

Om te controleren of een HPC Pack-implementatie van de RDMA kunnen exemplaren, de opdracht HPC Pack **mpipingpong** op het cluster worden uitgevoerd. **mpipingpong** verzendt pakketten van gegevens tussen paren knooppunten die herhaaldelijk latentie en doorvoer metingen berekend en statistische gegevens voor het netwerk RDMA-toepassingen. In dit voorbeeld ziet een typische patroon voor het uitvoeren van een taak MPI (in dit geval **mpipingpong**) met behulp van de clusteropdracht **mpiexec** .

In dit voorbeeld wordt ervan uitgegaan dat u Azure knooppunten toegevoegd in een "burst naar Azure"-configuratie (Zie[Scenario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Als u op een cluster van Azure VMs HPC Pack hebt geïmplementeerd, moet u de opdrachtsyntaxis van de extra omgevingsvariabelen netwerk verkeer op het netwerk RDMA instellen om op te geven een ander knooppunt groep wijzigen.


Mpipingpong op het cluster uitvoeren:


1. Open een opdrachtprompt op het knooppunt hoofd of op een client correct is geconfigureerd.

2. Als u wilt schatten latentie tussen knooppunten in een burst Azure-implementatie van 4 knooppunten, typ de volgende opdracht in te dienen van een taak als u wilt mpipingpong met een kleine pakketgrootte en een groot aantal iteraties:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    De opdracht retourneert de ID van de taak die is ingediend.

    Als u het Pack HPC-cluster geïmplementeerd op Azure VMs, geeft u een groep met knooppunt geïmplementeerd berekenen knooppunt VMs in een enkele cloud-service geïmplementeerd en de opdracht **mpiexec** als volgt wijzigen:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Wanneer de taak is voltooid, weergave van de uitvoer (in dit geval, de uitvoer van de taak, taak 1), typt u het volgende

    ```
    task view <JobID>.1
    ```

    waar &lt; *JobID* &gt; is de ID van de taak die is ingediend.

    Latentie resultaten met de volgende strekking worden opgenomen in de uitvoer.

    ![Ping pong latentie][pingpong1]

4. Als u wilt schatten doorvoersnelheid tussen paren van Azure burst knooppunten, typ de volgende opdracht in te dienen van een taak als u wilt **mpipingpong** met een groot pakketgrootte en een klein aantal iteraties:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    De opdracht retourneert de ID van de taak die is ingediend.

    Een Pack van HPC-cluster geïmplementeerd op Azure VMs de opdracht zoals vermeld in stap 2 te wijzigen.

5. Wanneer de taak is voltooid, weergave van de uitvoer (in dit geval, de uitvoer van de taak, taak 1), typt u het volgende:

    ```
    task view <JobID>.1
    ```

  Doorvoerresultaten met de volgende strekking worden opgenomen in de uitvoer.

  ![Ping pong doorvoer][pingpong2]


### <a name="mpi-application-considerations"></a>Overwegingen met betrekking tot MPI-toepassing


Hieronder volgen enkele overwegingen voor MPI-toepassingen met HPC Pack in Azure. Sommige gelden alleen voor implementaties van Azure knooppunten (werknemer rol exemplaren in een configuratie met "burst naar Azure" toegevoegd).

* Werknemer rol exemplaren in een cloud-service worden regelmatig door de service zonder voorafgaande kennisgeving door Azure (bijvoorbeeld voor systeemonderhoud of in geval die een exemplaar niet). Een exemplaar wordt door de service terwijl deze een MPI-taak wordt uitgevoerd, de instantie gegevens verliest als wordt de status wanneer deze werd eerst geïmplementeerd, waardoor de taak MPI mislukken. De meer knooppunten te gebruiken voor één taak MPI en hoe langer de taak wordt uitgevoerd, hoe groter de kans dat een van de instanties wordt door de service tijdens een taak wordt uitgevoerd. U moet ook overwegen als u een enkel knooppunt in de installatie als een bestandsserver aanwijzen.


* Om uit te voeren taken MPI in Azure, er geen gebruik van de RDMA kunnen exemplaren. U kunt elk exemplaar formaat dat wordt ondersteund door HPC Pack gebruiken. De RDMA kunnen exemplaren worden echter aanbevolen voor het uitvoeren van relatief grote MPI taken die gevoelig zijn voor de latentie en de bandbreedte van het netwerk dat de knooppunten verbindt. Als u andere grootten latentie en bandbreedte gevoelige MPI taken uitvoeren, kunt u het beste kleine projecten, waarin een taak wordt uitgevoerd op alleen een paar knooppunten wordt uitgevoerd.

* Toepassingen die worden gebruikt in Azure instanties zijn de licentievoorwaarden die zijn gekoppeld aan de toepassing. Neem contact op met de leverancier van een commerciële toepassing voor licenties of andere beperkingen voor het uitvoeren in de cloud. Niet alle leveranciers bieden "pay-as-you-go"-licentiëring.


* Azure exemplaren nodig verdere setup toegang op ruimten knooppunten, aandelen en licentieservers. U kunt bijvoorbeeld zodat de Azure knooppunten voor toegang tot een licentieserver op gebouwen Azure virtueel netwerk van site naar site configureren.


* MPI-toepassingen op Azure exemplaren worden uitgevoerd, elk MPI toepassing registreren met Windows Firewall op de exemplaren met de opdracht **hpcfwutil** . Hierdoor MPI communicatie vindt plaats op een poort die dynamisch wordt toegewezen door de firewall.

    >[AZURE.NOTE] U kunt ook een firewall-uitzondering opdracht automatisch worden uitgevoerd op alle nieuwe Azure knooppunten die worden toegevoegd aan het cluster configureren voor burst met Azure-installaties. Nadat u de opdracht **hpcfwutil** uitvoeren en controleren of de toepassing werkt, kunt u de opdracht toevoegen aan een opstartscript voor de Azure knooppunten. Voor meer informatie, Zie [gebruik van een opstartscript voor Azure knooppunten](https://technet.microsoft.com/library/jj899632.aspx).



* De omgevingsvariabele CCP_MPI_NETMASK cluster HPC Pack gebruikt voor het opgeven van een bereik van geldige adressen voor MPI communicatie. Starten in de HPC Pack 2012 R2, de omgevingsvariabele CCP_MPI_NETMASK cluster geldt alleen voor communicatie tussen knooppunten in het servercluster domein behoren compute MPI (in ruimten of in Azure VMs). De variabele wordt door knooppunten toegevoegd in een package-burst op Azure configuratie genegeerd.


* MPI-taken uitvoeren niet via Azure exemplaren die in verschillende cloud-services (bijvoorbeeld in een package-burst op Azure implementaties met een ander knooppunt sjablonen of computerknooppunten VM in Azure geïmplementeerd in meerdere cloud services) zijn geïmplementeerd. Als er meerdere Azure knooppunt implementaties die worden gestart met een ander knooppunt, sjablonen, moet de taak MPI uitvoeren op slechts één reeks knooppunten Azure.


* Wanneer u Azure knooppunten aan cluster toevoegen en deze on line te brengen, wil de HPC Job Scheduler-Service onmiddellijk taken starten op de knooppunten. Als slechts een gedeelte van uw werkbelasting op Azure uitvoeren kunt, moet u bijwerken of sjablonen van project om te bepalen welke functietypen kunnen worden uitgevoerd op Azure maken. Bijvoorbeeld, om ervoor te zorgen dat projecten ingediend met een sjabloon alleen uitgevoerd op Azure knooppunten, de eigenschap van het knooppuntgroepen toevoegen aan de sjabloon en selecteer AzureNodes de vereiste waarde. U kunt aangepaste groepen voor uw Azure knooppunten maken met de cmdlet Add-HpcGroup HPC PowerShell.


## <a name="next-steps"></a>Volgende stappen

* Als alternatief voor het gebruik van HPC Pack met de service Azure Batch MPI-toepassingen uitvoeren op knooppunten in Azure compute van beheerde toepassingen te ontwikkelen. Zie [taken voor gebruik met meerdere instanties Message Passing Interface (MPI)-toepassingen in Azure Batch uit te voeren](../batch/batch-mpi.md).

* Als u Linux MPI toepassingen uitvoeren die toegang het netwerk RDMA Azure wilt tot, Zie [een cluster van Linux-RDMA MPI-toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png