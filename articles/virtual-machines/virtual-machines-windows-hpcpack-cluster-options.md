<properties
 pageTitle="Opties voor Windows HPC Pack cluster in de cloud | Microsoft Azure"
 description="Meer informatie over opties met Microsoft HPC Pack maken en beheren van een Windows high performance computing (HPC)-cluster in de Azure cloud"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Opties met HPC Pack maken en beheren van een Windows HPC-cluster in Azure

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dit artikel richt zich op de opties voor het maken van clusters van HPC Pack om Windows werklasten worden uitgevoerd. Er zijn ook opties voor het maken van clusters [Linux HPC werkbelasting met HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md)uitvoeren.


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Een Pack van HPC-cluster uitgevoerd in Azure VMs

### <a name="azure-templates"></a>Azure-sjablonen

* (Marktplaats) [Cluster HPC Pack voor Windows werkbelasting](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Marktplaats) [Pack HPC-cluster voor Excel werkbelasting](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Quickstart) [Een HPC-cluster maken](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Quickstart) [Een HPC-cluster met aangepaste compute knooppunt afbeelding maken](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure VM-images

* [HPC Pack op Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [Computerknooppunt HPC Pack voor Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack berekenen knooppunt met Excel op Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>Implementatiescript voor PowerShell

* [Een HPC-cluster maken met het implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Zelfstudies

* [Zelfstudie: Aan de slag met een Pack HPC-cluster in Azure naar Excel- en SOA-werklasten worden uitgevoerd](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Handmatige implementatie met Azure portal

* [Het hoofd knooppunt van een cluster HPC Pack in een VM Azure instellen](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Clusterbeheer

* [Computerknooppunten in een cluster HPC Pack in Azure beheren](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Vergroten of verkleinen van Azure compute bronnen in een Pack HPC-cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Verzenden taken naar een cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Project management in de HPC-Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Werknemer rol knooppunten toevoegen aan een cluster van HPC Pack


* [Burst-Azure werknemer exemplaren met HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)

* [Zelfstudie: Een hybride cluster met HPC Pack in Azure instellen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Azure 'burst'-knooppunten toevoegen aan een HPC Pack head knooppunt in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integreren met Azure Batch 

* [Burst-Azure batch met HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA clusters voor MPI werkbelasting maken

* [Instellen van een cluster met Windows RDMA met HPC Pack MPI-toepassingen uit te voeren](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
