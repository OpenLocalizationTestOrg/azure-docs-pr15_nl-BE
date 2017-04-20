<properties
   pageTitle="Een zelfstandige cluster maken met Azure VMs met Windows | Microsoft Azure"
   description="Informatie over het maken en beheren van een cluster Azure Service Fabric op Azure virtuele machines waarop Windows Server wordt uitgevoerd."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Maak een cluster met drie knooppunten zelfstandige Service Fabric met Azure virtuele machines met Windows Server

In dit artikel wordt beschreven hoe een cluster op Windows gebaseerde Azure virtuele machines (VMs), met behulp van het zelfstandige installatieprogramma voor Fabric-Service voor Windows Server maken. Dit is een speciaal geval van [maken en beheren van een cluster waarop Windows Server](service-fabric-cluster-creation-for-windows-server.md) waar de VMs zijn [Azure VMs waarop Windows Server wordt uitgevoerd](../virtual-machines/virtual-machines-windows-hero-tutorial.md), maar u niet [een Azure cloud-gebaseerde Service Fabric '-cluster](service-fabric-cluster-creation-via-portal.md)maakt. Het verschil is dat de zelfstandige Service Fabric '-cluster gemaakt door de volgende stappen wordt volledig beheerd door u, terwijl de Azure cloud-gebaseerde Service Fabric clusters worden beheerd en bijgewerkt door de resource Fabric Service provider.


## <a name="steps-to-create-the-standalone-cluster"></a>Stappen voor het maken van het cluster zelfstandige

1. Aanmelden bij de Azure portal en maak een nieuwe Windows Server 2012 R2 Datacenter VM in een resourcegroep. Lees het artikel [een Windows VM in Azure portal maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor meer informatie.
2. Een paar meer Windows Server 2012 R2 Datacenter VMs aan dezelfde resourcegroep toevoegen. Zorg ervoor dat elk van de VMs heeft dezelfde gebruikersnaam en hetzelfde wachtwoord als gemaakt. Eenmaal gemaakt, ziet u alle drie de VMs in hetzelfde virtuele netwerk.
3. Verbinding maken met elk van de VMs en uitschakelen in Windows Firewall met de [Server Manager, de lokale Server dashboard](https://technet.microsoft.com/library/jj134147.aspx). Dit zorgt ervoor dat het netwerkverkeer tussen de computers met elkaar kan communiceren. Verbinding gemaakt met elke computer en het IP-adres verkrijgen door een opdrachtprompt te openen en te typen `ipconfig`. Ook ziet u het IP-adres van elke computer door de resource virtueel netwerk voor de resourcegroep selecteren in de portal Azure.
4. Verbinding maken met een van de VMs en testen dat u de twee VMs kunt pingen.
5. Verbinding maken met een van de VMs en [de zelfstandige Service Fabric downloaden voor Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) in een nieuwe map op de computer en het pakket uitpakken.
6. Het bestand *ClusterConfig.Unsecure.MultiMachine.json* in Kladblok openen en bewerken van elk knooppunt met de drie IP-adressen van de machines. Wijzig de naam van het cluster aan de bovenkant en sla het bestand.  Hieronder vindt u een (gedeeltelijke) voorbeeld van het manifest van het cluster.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Open een [venster PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Ga naar de map waarin u het gedownloade standalone installer-pakket wordt uitgepakt en het manifestbestand van het cluster opgeslagen. Voer de volgende PowerShell-opdracht.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. U ziet de PowerShell uitvoeren, verbinding maken met elke computer en een cluster maken. Na ongeveer een minuut, u controleren kunt of het cluster operationeel is door verbinding te maken met de Service Fabric Explorer op een van de IP-adres van de computer bijvoorbeeld met behulp van `http://10.7.0.5:19080/Explorer/index.html`. Aangezien dit een autonome cluster met Azure VMs, om u [de VMs Azure certificaten](service-fabric-windows-cluster-x509-security.md) distribueren of instellen van een van de computers als een [domeincontroller voor Windows Server Active Directory (AD) voor Windows-verificatie](service-fabric-windows-cluster-windows-security.md), net zoals u in gebouwen doen zou te beveiligen.


## <a name="next-steps"></a>Volgende stappen
- [Zelfstandige Service Fabric clusters maken op Windows Server of Linux](service-fabric-deploy-anywhere.md)
- [Toevoegen of verwijderen van knooppunten aan een cluster van zelfstandige Service Fabric](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Configuratie-instellingen voor een zelfstandige Windows-cluster](service-fabric-cluster-manifest.md)
- [Een zelfstandige cluster op Windows met behulp van Windows-beveiliging veilig](service-fabric-windows-cluster-windows-security.md)
- [Een zelfstandige cluster op Windows X509 met beveiligde certificaten](service-fabric-windows-cluster-x509-security.md)
