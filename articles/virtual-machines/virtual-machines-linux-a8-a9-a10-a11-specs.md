<properties
 pageTitle="Over intensieve VMs met Linux | Microsoft Azure"
 description="Achtergrondinformatie en overwegingen bij het gebruik van de H-serie en A8, A9, A10 en A11 intensieve formaten voor Linux VMs"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Over H-serie en intensieve A-serie VMs 

Hier volgt achtergrondinformatie en enkele overwegingen bij het gebruik van de nieuwere Azure H-reeks en de eerdere A8, A9, A10 en A11 formaten, ook bekend als *intensieve* exemplaren. Dit artikel is gericht op gebruik van deze formaten voor Linux VMs. Dit artikel is ook beschikbaar voor [Windows VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Toegang tot het netwerk RDMA

Kunt u clusters van RDMA kan Linux VMs dat uitvoeren een van de volgende ondersteunde HPC Linux-distributies en ondersteunde MPI implementatie kunnen profiteren van het netwerk RDMA Azure. Zie [een cluster van Linux-RDMA MPI-toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md) voor installatieopties en monster configuratiestappen worden uitgevoerd.

* **Verdelingen** - moet u VMs implementeren van RDMA kunnen SUSE Linux Enterprise Server (SLES) of CentOS OpenLogic gebaseerde HPC afbeeldingen op Marketplace Azure. Alleen de volgende afbeeldingen op Marketplace ondersteunt de benodigde stuurprogramma's voor Linux RDMA:

    * 12 SLES SP1 voor HPC SLES 12 SP1 voor HPC (Premium)
    
    * 12 SLES voor HPC SLES 12 voor HPC (Premium)
    
    * 7.1 HPC op basis van centOS
    
    * 6.5 HPC op basis van centOS
    
    >[AZURE.NOTE]Voor VMs H-serie, wordt aangeraden een SLES 12 SP1 voor HPC afbeelding ofwel CentOS-7.1 HPC installatiekopie.
    >
    >Kernel-updates zijn op de CentOS-gebaseerde HPC-afbeeldingen uitgeschakeld in het configuratiebestand **yum** . Dit is omdat de stuurprogramma's voor Linux RDMA als een RPM-pakket worden gedistribueerd en updates voor stuurprogramma's niet werken mogelijk als de kernel wordt bijgewerkt.

* **MPI** - Intel MPI Library 5.x

    Afhankelijk van de Marketplace-afbeelding gekozen, afzonderlijke licenties, installatie of configuratie van Intel MPI nodig zijn, als volgt: 
    
    * **SLES 12 SP1 voor afbeelding HPC** - installatie de Intel MPI-pakketten op de VM worden verdeeld door de volgende opdracht uit te voeren:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **12 SLES voor afbeelding HPC** - moet u afzonderlijk registreren om te downloaden en installeren van Intel MPI. Zie de [installatiehandleiding voor Intel MPI Library](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **Afbeeldingen op basis van centOS HPC** - Intel MPI 5.1 is al geïnstalleerd.  

    MPI taken uitvoeren op geclusterde VMs is aanvullende configuratie nodig. Op een cluster van VMs moet u bijvoorbeeld naar het opbouwen van vertrouwen tussen de compute nodes. Zie [een cluster van Linux-RDMA MPI-toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md)voor de normale instellingen.


## <a name="considerations-for-hpc-pack-and-linux"></a>Overwegingen voor HPC Pack en Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)van Microsoft gratis HPC-cluster en project management-oplossing biedt een optie voor het gebruik van de intensieve exemplaren met Linux. De laatste releases van HPC Pack 2012 R2 ondersteuning van diverse Linux-distributies uit te voeren op computerknooppunten geïmplementeerd in Azure VMs, beheerd door een Windows Server-head knooppunt. Met RDMA kan Linux computerknooppunten met Intel MPI, HPC Pack plannen en Linux MPI toepassingen uitvoeren die toegang het netwerk van RDMA tot. Om te beginnen Zie [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Aandachtspunten bij een netwerk topologie

* Eth1 is op RDMA ingeschakeld Linux VMs in Azure gereserveerd voor RDMA-netwerkverkeer. Wijzig de Eth1 instellingen of gegevens in het configuratiebestand verwijzen naar dit netwerk niet. Eth0 is gereserveerd voor reguliere Azure netwerkverkeer.

* IP over InfiniBand (IB) wordt niet ondersteund in Azure. Alleen RDMA via IB wordt ondersteund.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA updates voor stuurprogramma's voor SLES 12

Nadat u een VM op basis van een SLES 12 HPC-image maakt, moet u mogelijk de RDMA-stuurprogramma's op de VMs voor verbinding met het netwerk RDMA bijwerken. 

>[AZURE.IMPORTANT]Deze stap is **vereist** voor 12 SLES voor HPC VM-implementaties in alle gebieden van Azure. 
>Deze stap is niet nodig als u een SLES 12 SP1 voor HPC, 7.1 HPC op basis van CentOS of CentOS gebaseerde 6.5 HPC VM implementeren. 

Voordat u de stuurprogramma's bijwerken, stop **zypper** processen of processen die de SUSE repo-databases op de VM te vergrendelen. Anders kunnen de stuurprogramma's niet goed bijgewerkt.  

De stuurprogramma's voor Linux RDMA op elke VM, werkt een van de volgende sets van Azure CLI-opdrachten vanaf de clientcomputer.

**In het klassieke implementatiemodel ingericht en SLES 12 voor HPC-VM**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**12 voor HPC VM SLES ingericht in het implementatiemodel Resource Manager**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Het duurt enige tijd de stuurprogramma's installeren en de opdracht krijgt zonder dat de uitvoer. Na de update uw VM wordt opnieuw opgestart en is gereed voor gebruik in enkele minuten.

### <a name="sample-script-for-driver-updates"></a>Voorbeeldscript voor stuurprogramma-updates

Als u een cluster van 12 SLES voor HPC VMs, kunt u de stuurprogrammaupdate script op alle knooppunten in het cluster. Het volgende script werkt bijvoorbeeld de stuurprogramma's in een cluster met 8 knooppunten.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Volgende stappen

* Zie [virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux)voor meer informatie over beschikbaarheid en prijzen van de intensieve formaten.

* Zie [voor virtuele machines](virtual-machines-linux-sizes.md)voor opslagcapaciteit en details van de schijf.

* Zie [een cluster van Linux-RDMA MPI-toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md)om te beginnen distributie en het gebruik van intensieve formaten met RDMA op Linux.


