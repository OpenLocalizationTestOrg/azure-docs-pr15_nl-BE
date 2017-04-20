<properties
   pageTitle="PowerShell script Windows HPC-cluster implementeren | Microsoft Azure"
   description="Een PowerShell script uitvoeren om te implementeren, een Pack van Windows HPC-cluster in Azure virtuele machines"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Windows high performance computing (HPC)-cluster maken met het implementatiescript HPC Pack IaaS

De implementatie van HPC Pack IaaS voor de implementatie van een volledige HPC-cluster voor werkbelasting in Azure virtuele machines Windows PowerShell-script wordt uitgevoerd. Het cluster bestaat uit een deel uitmaakt van een Active Directory head knooppunt met Windows Server en Microsoft HPC Pack en extra vensters berekenen van bronnen die u opgeeft. Als u implementeren een Pack HPC-cluster in Azure voor Linux werkbelasting wilt, Zie [een Linux HPC-cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). U kunt ook een sjabloon Azure Resource Manager een Pack HPC-cluster implementeren. Zie voor voorbeelden van [een HPC-cluster maken](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) en [maken een HPC-cluster met een aangepaste afbeelding knooppunt te berekenen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Voorbeeld van de configuratiebestanden

In de volgende voorbeelden vervangen door uw eigen waarden voor uw abonnements-Id of naam en de namen en de service.

### <a name="example-1"></a>Voorbeeld 1

De volgende configuratiebestand implementeert een Pack HPC-cluster met een head-knooppunt met lokale databases en vijf knooppunten waarop het besturingssysteem Windows Server 2012 R2 berekenen. Alle cloud services worden direct in de VS West locatie gemaakt. Het hoofd knooppunt fungeert als domeincontroller van het domein-forest.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Voorbeeld 2

De volgende configuratiebestand implementeert een Pack HPC-cluster in een bestaand domeinforest. Het cluster heeft 1 hoofd knooppunt met lokale databases en 12 compute nodes met de extensie BGInfo VM is toegepast.
Automatische installatie van Windows-updates is uitgeschakeld voor alle VMs in het domein-forest. Alle cloud services worden direct in de Oost-Azië-locatie gemaakt. De compute nodes in drie cloud services en drie opslag accounts worden gemaakt: _MyHPCCN-0001_ aan _MyHPCCN-0005_ in _MyHPCCNService01_ en _mycnstorage01_; _MyHPCCN-0006_ naar _MyHPCCN0010_ in _MyHPCCNService02_ en _mycnstorage02_; (en _MyHPCCN-0011_ aan _MyHPCCN-0012_ in _MyHPCCNService03_ en _mycnstorage03_). De compute nodes worden gemaakt van een bestaande persoonlijke afbeelding van een compute node vastgelegd. De automatische vergroten of verkleinen-service is ingeschakeld met de standaard vergroten of verkleinen van intervallen.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Voorbeeld 3

De volgende configuratiebestand implementeert een Pack HPC-cluster in een bestaand domeinforest. Het cluster bevat één hoofd knooppunt, een databaseserver met een schijf van 500 GB gegevens 2 broker knooppunten waarop het besturingssysteem Windows Server 2012 R2 en vijf computerknooppunten waarop het besturingssysteem Windows Server 2012 R2. De service cloud MyHPCCNService wordt gemaakt in de groep affiniteit *MyIBAffinityGroup*en de andere wolk diensten worden gemaakt in de groep affiniteit *MyAffinityGroup*. De HPC Job Scheduler REST API en HPC-webportaal zijn ingeschakeld op het hoofd knooppunt.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Voorbeeld 4

De volgende configuratiebestand implementeert een Pack HPC-cluster in een bestaand domeinforest. Het cluster heeft twee hoofd knooppunt met lokale databases en twee Azure knooppunt sjablonen zijn gemaakt voor Azure knooppunt sjabloon _AzureTemplate1_drie grootte normaal Azure knooppunten zijn gemaakt. Een scriptbestand op het hoofd knooppunt wordt uitgevoerd nadat het hoofd knooppunt is geconfigureerd.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Het oplossen van problemen


* **Fout 'VNet niet bestaat,** -als u het script uitvoeren om te implementeren meerdere clusters in Azure gelijktijdig onder één abonnement, een of meer installaties mislukken met de fout ' VNet *VNet\_naam* bestaat niet '.
Als deze fout optreedt, wordt het script voor de mislukte installatie opnieuw uitvoeren.

* **Problematische toegang tot het Internet vanaf de Azure virtueel netwerk** - als u een cluster met een nieuwe domeincontroller maakt met behulp van een script voor de implementatie, of als u handmatig een hoofd knooppunt VM tot domeincontroller promoveren, treden problemen op het VMs verbinden met het Internet. Dit probleem kan optreden als een doorstuurserver DNS-server automatisch geconfigureerd op de domeincontroller en deze DNS-server doorstuurservers niet goed is opgelost.

    Omzeilen van dit probleem te melden bij de domeincontroller en beide verwijderen de doorstuurserver configuratie-instelling of een doorstuurserver geldige DNS-server configureren. Als u deze instelling wilt Klik in Serverbeheer op **Extra** >
    **DNS** DNS-beheer openen en dubbelklik vervolgens op **doorstuurservers**.

* **Probleem RDMA netwerk van intensieve VMs** - als u Windows Server compute toevoegt of knooppunt VMs met een grootte van RDMA kan bijvoorbeeld A8 of A9 broker, er kunnen problemen optreden die VMs verbinden met het netwerk van de toepassing RDMA. Een reden dat dit probleem optreedt, is als de extensie HpcVmDrivers is niet juist geïnstalleerd wanneer het VMs worden toegevoegd aan het cluster. Bijvoorbeeld de extensie in de installatie staat vast.

    Om dit probleem te omzeilen, moet u eerst de status van de extensie in het VMs controleren. Als de extensie niet goed is geïnstalleerd, verwijdert u de knooppunten van de HPC-cluster en de knooppunten vervolgens opnieuw toevoegen. U kunt bijvoorbeeld VMs-computerknooppunt toevoegen door het uitvoeren van het script toevoegen HpcIaaSNode.ps1 op het hoofd knooppunt.
    
## <a name="next-steps"></a>Volgende stappen

* Probeer de werkbelasting van een test uitgevoerd op het cluster. Zie voor een voorbeeld, de HPC Pack [aan de slag](https://technet.microsoft.com/library/jj884144).

* Zie voor een zelfstudie script de cluster implementeren en uitvoeren van een HPC-werkbelasting [aan de slag met een Pack HPC-cluster in Azure naar Excel- en SOA-werklasten worden uitgevoerd](virtual-machines-windows-excel-cluster-hpcpack.md).

* Probeer te starten, stoppen, voegen hulpmiddelen HPC-Pack en computerknooppunten verwijderen uit een cluster dat u maakt. Zie [knooppunten in een cluster HPC Pack in Azure berekent u beheren](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Zie [taken HPC verzenden vanaf een computer in ruimten met een Pack HPC-cluster in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)ophalen instellen van taken verzenden aan het cluster vanaf een lokale computer.
