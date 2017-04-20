<properties
 pageTitle="Burst-knooppunten toevoegen aan een cluster HPC Pack | Microsoft Azure"
 description="Meer informatie over het uitbreiden van een Pack HPC-cluster in Azure op verzoek door werknemer rol exemplaren die worden uitgevoerd in een cloud-service toe te voegen"
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
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>On-demand 'burst'-knooppunten toevoegen aan een cluster HPC Pack in Azure



Als u een in Azure [Pack voor Microsoft HPC](https://technet.microsoft.com/library/cc514029) -cluster hebt ingesteld, kunt u een manier om snel schaal van de capaciteit van het cluster omhoog of omlaag, zonder behoud van een aantal vooraf geconfigureerde computerknooppunt VMs. In dit artikel wordt beschreven hoe u on-demand 'burst'-knooppunten (werknemer rol exemplaren die worden uitgevoerd in een cloud-service) toevoegen als compute-bronnen naar een knooppunt head in Azure. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Burst-knooppunten][burst]

De stappen in dit artikel kunt u snel Azure knooppunten toevoegen aan een cloud-gebaseerde HPC Pack head knooppunt VM voor een test of het bewijs van concept implementatie. Op hoog niveau stappen zijn hetzelfde als de stappen uit om "burst naar Azure" toe te voegen compute cloud capaciteit aan een cluster van lokalen HPC Pack. Zie voor een zelfstudie, [stelt u een hybride compute cluster met Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Zie voor gedetailleerde richtlijnen en aandachtspunten voor productie-implementaties, [Burst naar Azure Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Vereisten

* **HPC Pack head knooppunt geïmplementeerd in een VM Azure** - kunt u een zelfstandige hoofd knooppunt VM of die deel uitmaakt van een grotere cluster. Zie een zelfstandig knooppunt hoofd maken [een HPC Head-knooppunt in een VM Azure Pack distribueren](virtual-machines-windows-hpcpack-cluster-headnode.md). Zie [Opties voor het maken en beheren van een Windows HPC-cluster in Azure Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md)voor geautomatiseerde HPC Pack cluster opties voor distributie.

    >[AZURE.TIP] Als u het [implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) gebruikt voor het maken van het cluster in Azure, neemt u Azure burst-knooppunten in de geautomatiseerde implementatie. Zie de voorbeelden in dit artikel.

* **Azure abonnement** - Azure knooppunten toevoegen kunt u het implementeren van het hoofd knooppunt VM, dezelfde-abonnement of een ander abonnement (of abonnementen).

* **Cores quota** - wellicht te verhogen van de quota van cores, vooral als u kiest voor de implementatie van verschillende Azure knooppunten met multicore-formaat. Een target, [open een verzoek voor ondersteuning van online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) kosteloos verhogen.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Stap 1: Maak een cloud-service en een opslag-account voor de knooppunten Azure

Gebruik de Azure klassieke portal of gelijkwaardig gereedschap in de volgende bronnen die nodig zijn voor de implementatie van uw Azure knooppunten configureren:

* Een nieuwe Azure cloud-service
* Een nieuwe account met Azure opslag

>[AZURE.NOTE] Niet opnieuw gebruiken van een bestaande service cloud in uw abonnement. 

**Overwegingen met betrekking tot**

* Configureer een aparte cloud service voor elk knooppunt Azure-sjabloon die u wilt maken. Echter, kunt u dezelfde opslag account voor meerdere sjablonen van het knooppunt.

* Het is raadzaam de service cloud en de opslag-account voor de distributie te in dezelfde regio Azure vinden.




## <a name="step-2-configure-an-azure-management-certificate"></a>Stap 2: Een certificaat Azure beheer configureren

Azure knooppunten als compute resources toevoegen, moet u een certificaat op de hoofd-knooppunt en uploaden op de Azure abonnement gebruikt voor de implementatie van een bijbehorend certificaat.

In dit scenario kunt u de **Standaard HPC Azure certificaat** dat HPC Pack installeert en configureert automatisch op het knooppunt head. Dit certificaat is handig voor het testen van de toepassing en het bewijs van concept implementaties. Upload het bestand C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer uit het hoofd VM-knooppunt aan het abonnement voor het gebruik van dit certificaat. Klik op **Instellingen**voor het uploaden van het certificaat in de [Azure klassieke portal](https://manage.windowsazure.com) > **Beheer van certificaten**.

Zie [scenario's voor het configureren van het certificaat Azure Management voor Azure Burst-implementaties](http://technet.microsoft.com/library/gg481759.aspx)voor extra opties voor het configureren van het certificaat.

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Stap 3: Azure knooppunten aan het cluster implementeren



De stappen voor het toevoegen en Azure knooppunten starten in dit scenario zijn doorgaans hetzelfde als de stappen met een hoofd knooppunt op gebouwen. Voor meer informatie, Zie de volgende secties in [stappen voor het implementeren van Azure knooppunten met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Een knooppunt Azure-sjabloon maken

* Azure knooppunten toevoegen aan het Windows HPC-cluster

* Start (levering) de Azure knooppunten

Nadat u toevoegt en de knooppunten te starten, zijn ze gereed voor u gebruikt om een cluster taken uitvoeren.

Als u problemen ondervindt bij het implementeren van Azure knooppunten, Zie [Problemen met implementaties van Azure knooppunten met Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Volgende stappen

* Voor het gebruik van de grootte van een intensieve exemplaar voor de knooppunten burst, Zie de overwegingen in [over VMs van H-serie en intensieve A-serie](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Als u wilt automatisch groter of kleiner de Azure computerbronnen volgens de werkbelasting cluster Zie [automatisch vergroot of verkleind Azure compute bronnen in een Pack HPC-cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
