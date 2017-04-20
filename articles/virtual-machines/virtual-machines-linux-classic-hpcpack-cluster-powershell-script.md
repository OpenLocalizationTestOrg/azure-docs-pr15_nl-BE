<properties
   pageTitle="PowerShell script Linux HPC-cluster implementeren | Microsoft Azure"
   description="Een PowerShell script uitvoeren om te implementeren, een Pack van Linux HPC-cluster in Azure virtuele machines"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Maak een Linux high performance computing (HPC)-cluster met het implementatiescript HPC Pack IaaS

De implementatie van HPC Pack IaaS PowerShell script voor de implementatie van een volledige HPC-cluster voor Linux werklasten in Azure virtuele machines worden uitgevoerd. Het cluster bestaat uit een deel uitmaakt van een Active Directory head knooppunt met Windows Server en Microsoft HPC Pack en computerknooppunten die worden uitgevoerd op een van de Linux-distributies worden ondersteund door HPC Pack. Zie [een Windows HPC-cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)voor de implementatie van een Pack HPC-cluster in werkbelasting voor Windows Azure. U kunt ook een sjabloon Azure Resource Manager een Pack HPC-cluster implementeren. Zie voor een voorbeeld, [maken een HPC-cluster met Linux computerknooppunten](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Voorbeeld configuratiebestand

Het volgende configuratiebestand maakt een nieuwe domeincontroller en het domein-forest en een Pack HPC-cluster 1 hoofd knooppunt met lokale databases en 10 Linux-computerknooppunten met implementeren. Alle cloud services worden direct in de Oost-Azië-locatie gemaakt. De Linux compute nodes worden gemaakt in 2 cloud services en 2 opslag (dat wil zeggen _MyLnxCN-0001_ aan _MyLnxCN-0005_ in _MyLnxCNService01_ en _mylnxstorage01_) en _MyLnxCN-0006_ aan _MyLnxCN-0010_ in _MyLnxCNService02_ en _mylnxstorage02_. De compute nodes worden gemaakt van een Linux-afbeelding van OpenLogic CentOS versie 7.0. 

Vervangen door uw eigen waarden voor de abonnementsnaam van uw en de namen en de service.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Het oplossen van problemen

* **Fout 'VNet niet bestaat,** -als u het script HPC Pack IaaS implementatie voor de implementatie van meerdere clusters in Azure onder één abonnement tegelijkertijd uitvoert, een of meer installaties mislukken met de fout ' VNet *VNet\_naam* bestaat niet '.
Als deze fout optreedt, is opnieuw uitvoeren van het script voor de installatie is mislukt.

* **Problematische toegang tot het Internet vanaf de Azure virtueel netwerk** - als u een Pack voor HPC-cluster met een nieuwe domeincontroller maken met behulp van een script voor de implementatie, of als u handmatig een hoofd knooppunt VM tot domeincontroller promoveren, treden problemen op het VMs in de Azure virtueel netwerk verbinding met het Internet. Dit kan gebeuren als een doorstuurserver DNS-server automatisch geconfigureerd op de domeincontroller en deze DNS-server doorstuurservers niet goed is opgelost.

    Omzeilen van dit probleem te melden bij de domeincontroller en beide verwijderen de doorstuurserver configuratie-instelling of een doorstuurserver geldige DNS-server configureren. U doet dit door in Serverbeheer op **hulpprogramma's** >
    **DNS** DNS-beheer openen en dubbelklik vervolgens op **doorstuurservers**.
    
## <a name="next-steps"></a>Volgende stappen

* Zie [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) voor informatie over ondersteunde Linux-distributies, verplaatsen van gegevens en het verzenden van taken naar een Pack HPC-cluster met Linux computerknooppunten.
* Voor de zelfstudies waarmee het script een cluster maken en uitvoeren van een Linux HPC werkbelasting, Zie:
    * [NAMD uitvoeren met Microsoft HPC Pack op Linux-computerknooppunten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [OpenFOAM uitvoeren met Microsoft HPC Pack op Linux-computerknooppunten in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Uitvoeren van de STER-CCM + met Microsoft HPC Pack op Linux computerknooppunten in Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
