<properties
 pageTitle="HPC-Pack Linux cluster opties in de cloud | Microsoft Azure"
 description="Meer informatie over opties met Microsoft HPC Pack maken en beheren van een Linux high performance computing (HPC)-cluster in de Azure cloud"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Opties met HPC Pack maken en beheren van een HPC-cluster in Azure voor Linux werkbelasting

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

In dit artikel ligt de nadruk op Opties HPC Pack met Linux werklasten worden uitgevoerd. Er zijn ook opties voor het uitvoeren van [Windows HPC werkbelasting met HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Een Pack van HPC-cluster uitgevoerd in Azure VMs

### <a name="azure-templates"></a>Azure-sjablonen


* (Marktplaats) [Pack HPC-cluster voor Linux werkbelasting](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Quickstart) [Een HPC-cluster met Linux maken computerknooppunten](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>Implementatiescript voor PowerShell

* [Maak een Linux HPC-cluster met het implementatiescript HPC Pack IaaS](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Zelfstudies

* [Zelfstudie: Aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Zelfstudie: NAMD met Microsoft HPC Pack uitvoeren onder Linux computerknooppunten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Zelfstudie: Run OpenFOAM met Microsoft HPC Pack op een cluster RDMA Linux in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Zelfstudie: Run STAR-CCM + met Microsoft HPC Pack op een RDMA Linux cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Clusterbeheer

* [Verzenden taken naar een cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Project management in de HPC-Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA clusters voor MPI werkbelasting maken

* [Zelfstudie: Run OpenFOAM met Microsoft HPC Pack op een cluster RDMA Linux in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Instellen van een RDMA Linux cluster MPI-toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md)

