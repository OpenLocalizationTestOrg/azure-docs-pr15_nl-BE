<properties
   pageTitle="MATLAB clusters op virtuele machines | Microsoft Azure"
   description="Gebruik Microsoft Azure virtuele machines maken MATLAB Distributed Computing serverclusters om uw computerintensieve parallelle MATLAB werklasten worden uitgevoerd"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>MATLAB Distributed Computing serverclusters maken op Azure VMs 

Gebruik Microsoft Azure virtuele machines voor het maken van een of meer MATLAB Distributed Computing serverclusters om uw computerintensieve parallelle MATLAB werklasten worden uitgevoerd. Uw software MATLAB Distributed Computing Server installeren op een VM wilt gebruiken als een basis en een Azure quickstart sjabloon of Azure PowerShell script (beschikbaar op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) te implementeren en beheren van het cluster. Na de implementatie kunnen verbinding maken met het cluster voor het uitvoeren van de werklast. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Over MATLAB en MATLAB Distributed Computing Server 

Het platform [MATLAB](http://www.mathworks.com/products/matlab/) is geoptimaliseerd voor het oplossen van technische en wetenschappelijke problemen. Gebruikers met grootschalige simulaties en gegevensverwerking taken MATLAB kunt MathWorks parallel computing producten hun computerintensieve werklasten versnellen door te profiteren van de computerclusters en raster services. [Parallel Computing Toolbox](http://www.mathworks.com/products/parallel-computing/) kunt MATLAB gebruikers parallelize toepassingen en profiteren van multicore-processors, GPU's, en clusters te berekenen. [MATLAB Distributed Computing Server](http://www.mathworks.com/products/distriben/) kunnen gebruikers gebruikmaken van de vele computers in een compute cluster MATLAB. 


U kunt via Azure virtuele machines MATLAB Distributed Computing Server clusters met dezelfde mechanismen beschikbaar voor parallelle werk indienen als clusters voor ruimten, zoals interactieve taken, batchverwerkingen, onafhankelijke taken en communicatie taken maken. Azure gebruiken in combinatie met de MATLAB-platform heeft vele voordelen in vergelijking met het inrichten en met behulp van traditionele op-bedrijfsruimten hardware: formaat van een bereik van virtuele machine, het maken van clusters op aanvraag zodat u alleen voor de compute resources u betaalt gebruikt en de mogelijkheid voor het testen van modellen op schaal.  

## <a name="prerequisites"></a>Vereisten

* **Clientcomputer** - moet u een Windows-clientcomputer kan communiceren met Azure en MATLAB Distributed Computing servercluster na de implementatie. 

* **Azure PowerShell** - Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) op uw computer installeren. 

* **Azure abonnement** - als u niet een abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) in een paar minuten. Overweeg een pay-as-you-go abonnement of andere inkoopopties voor grotere clusters. 

* **Cores quota** - moet u mogelijk de core quota voor de implementatie van een grote cluster of meerdere MATLAB Distributed Computing Server cluster verhogen. Een target, [open een verzoek voor ondersteuning van online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) kosteloos verhogen. 

* **MATLAB, Parallel Computing Toolbox en MATLAB Distributed Computing Server licenties** - scripts wordt ervan uitgegaan dat de [MathWorks gehost License Manager](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) wordt gebruikt voor alle licenties.  

* **MATLAB Distributed Computing Server software** - wordt geïnstalleerd op een VM die wordt gebruikt als de VM-basisinstallatiekopie voor het cluster VMs. 


## <a name="high-level-steps"></a>Hoog niveau stappen

Voor het gebruik van Azure virtual machines voor het MATLAB Distributed Computing serverclusters, zijn de volgende stappen op hoog niveau vereist. Er zijn gedetailleerde instructies in de documentatie bij de quickstart sjabloon en scripts op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Maak een VM-basisinstallatiekopie**  
    * Download en installeer de software MATLAB Distributed Computing Server naar deze VM. 

    >[AZURE.NOTE]Dit proces kan enkele uren duren, maar u hoeft alleen te doen als u voor elke versie van MATLAB gebruiken.   
    
2. **Een of meer clusters maken**  
    * Gebruik de geleverde PowerShell script of de sjabloon quickstart een cluster maken uit de basisinstallatiekopie VM.   
    * Met behulp van de meegeleverde PowerShell-script waarmee u een lijst, onderbreken, hervatten clusters beheren en verwijderen van clusters. 
 
## <a name="cluster-configurations"></a>Clusterconfiguraties 

Op dit moment kunnen het script voor het cluster maken en deze sjabloon u een topologie met één MATLAB Distributed Computing Server maken. U kunt desgewenst een of meer extra clusters maken met een cluster met een ander nummer van werknemer VMs, met behulp van verschillende grootten VM, enzovoort. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB-client en cluster in Azure 

De client-knooppunt MATLAB, MATLAB Job Scheduler-knooppunt en MATLAB Distributed Computing Server "werknemer" knooppunten zijn alle geconfigureerd als Azure VMs in een virtueel netwerk, zoals in de volgende afbeelding. 

![Cluster-topologie](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Verbinding met extern bureaublad aan het knooppunt van de client voor het gebruik van het cluster. Het knooppunt van de client wordt uitgevoerd de MATLAB-client. 

* Het knooppunt van de client heeft een bestandsshare die toegankelijk is voor alle werknemers.

* MathWorks gehost License Manager wordt gebruikt voor de controle van de licentie voor alle MATLAB-software. 

* Op de werknemer VMs één MATLAB Distributed Computing Server werknemer per core wordt gemaakt, maar kunt u een willekeurig getal. 


## <a name="use-an-azure-based-cluster"></a>Gebruik van een Cluster op basis van Azure 

Als bij andere soorten MATLAB Distributed Computing serverclusters, moet u Profielbeheer Cluster in de MATLAB-client (de client VM) gebruiken voor het maken van een cluster MATLAB Job Scheduler profiel.

![Cluster Profielbeheer](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor gedetailleerde instructies voor het implementeren en beheren van MATLAB Distributed Computing serverclusters in Azure, de [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) -bibliotheek met sjablonen en scripts. 

* Ga naar de [site MathWorks](http://www.mathworks.com/) gedetailleerde documentatie over MATLAB en MATLAB Distributed Computing Server.
