<properties
 pageTitle="Over intensieve VMs met Windows | Microsoft Azure"
 description="Achtergrondinformatie en overwegingen bij het gebruik van de Azure H-serie en A8, A9, A10 en A11 intensieve formaten voor Windows VMs en cloud services"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Over H-serie en intensieve A-serie VMs

Hier volgt achtergrondinformatie en enkele overwegingen bij het gebruik van de nieuwere Azure H-reeks en de eerdere A8, A9, A10 en A11 exemplaren, ook bekend als *intensieve* exemplaren. In dit artikel de nadruk op deze exemplaren voor Windows VMs. Dit artikel is ook beschikbaar voor [Linux VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md).


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Toegang tot het netwerk RDMA

U kunt clusters van RDMA kunnen Windows Server-exemplaren maken en implementeren van een van de ondersteunde MPI-implementaties kunnen profiteren van het netwerk RDMA Azure. Dit netwerk met lage latentie, hoge gegevensdoorvoer wordt MPI alleen voor verkeer gereserveerd.

* **Besturingssysteem**
    * **Virtuele machines** - Windows Server 2012 R2 Windows Server 2012
    * **Cloud services** - Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 Gast OS-familie

* **MPI** - MPI Microsoft (MS-MPI) 2012 R2 of later, Intel MPI Library 5.x

MPI-implementaties ondersteund de Microsoft netwerk Direct interface gebruiken voor de communicatie tussen instanties. Zie [instellen van een cluster met Windows RDMA met HPC Pack MPI-toepassingen uit te voeren](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) en [Gebruik meerdere exemplaren taken Message Passing Interface (MPI)-toepassingen in Azure Batch](../batch/batch-mpi.md) voor installatieopties en configuratiestappen monster.


>[AZURE.NOTE]Op RDMA kunnen intensieve VMs, moet de extensie HpcVmDrivers toegevoegd aan de VMs Windows netwerk apparaat stuurprogramma's te installeren die nodig zijn voor een RDMA-verbinding. In de meeste toepassingen, wordt automatisch de extensie HpcVmDrivers toegevoegd. Als u de extensie uzelf toevoegen, Zie [beheren van VM-extensies](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Overwegingen voor HPC Pack en Windows

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)van Microsoft gratis HPC-cluster en project management-oplossing is niet vereist voor het gebruik van de intensieve exemplaren met Windows Server. Het is echter één optie voor u maakt een compute cluster in Azure MPI Windows gebaseerde toepassingen en andere werkbelasting HPC uit te voeren. HPC Pack 2012 R2 en hogere versies bevatten een runtimeomgeving voor MS-MPI die het netwerk Azure RDMA wanneer geïmplementeerd op RDMA kunnen VMs kunt gebruiken.

Zie [een cluster van Windows RDMA met HPC Pack MPI-toepassingen uit te voeren](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)voor meer informatie en checklists voor de intensieve exemplaren HPC Pack op Windows Server.




## <a name="next-steps"></a>Volgende stappen

* Zie [virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) en [prijzen van Cloud-Services](https://azure.microsoft.com/pricing/details/cloud-services/)voor meer informatie over beschikbaarheid en prijzen van de intensieve formaten.

* Zie [voor virtuele machines](virtual-machines-linux-sizes.md)voor opslagcapaciteit en details van de schijf.

* Zie [een cluster van Windows RDMA met HPC Pack MPI-toepassingen uit te voeren](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)om te beginnen distributie en het gebruik van computer-intensieve exemplaren met HPC Pack op Windows.

* Zie voor informatie over het gebruik van instanties A8 en A9 MPI-toepassingen uitvoeren met de batchverwerking Azure, [Gebruik meerdere exemplaren taken Message Passing Interface (MPI)-toepassingen in Azure Batch uit te voeren](../batch/batch-mpi.md).
