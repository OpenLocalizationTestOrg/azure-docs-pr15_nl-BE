<properties
 pageTitle="Een HPC Pack head-knooppunt maken in een VM Azure | Microsoft Azure"
 description="Informatie over de Azure portal en het implementatiemodel Resource Manager gebruiken voor het maken van een hoofd Microsoft HPC Pack-knooppunt in een VM Azure."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Het hoofd knooppunt van een cluster HPC Pack in een Azure VM met een Marketplace-afbeelding maken


Een [afbeelding van Microsoft HPC Pack virtuele machine](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) uit de markt Azure en de Azure portal gebruik te maken van de hoofd-knooppunt van een HPC-cluster. Deze afbeelding HPC Pack VM is gebaseerd op Windows Server 2012 R2 Datacenter met HPC Pack 2012 R2 Update 3 vooraf geïnstalleerd. Dit knooppunt hoofd gebruiken om een bewijs van de implementatie van HPC Pack in Azure concept. Vervolgens kunt u computerknooppunten toevoegen aan het cluster aan HPC werklasten worden uitgevoerd.



>[AZURE.TIP]Om een volledige HPC Pack cluster in Azure met het hoofd knooppunt en computerknooppunten implementeert, is het raadzaam een geautomatiseerde methode te gebruiken. Opties omvatten de [HPC Pack IaaS implementatiescript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) en de sjabloon [Pack HPC-cluster voor werkbelasting van Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) Resource Manager. Zie [Opties voor het cluster in Azure Pack HPC](virtual-machines-windows-hpcpack-cluster-options.md) voor extra sjablonen. 


## <a name="planning-considerations"></a>Overwegingen bij de planning

In de volgende afbeelding ziet u het hoofd HPC Pack-knooppunt in een Active Directory-domein in een virtueel netwerk van Azure implementeren.

![Hoofd knooppunt HPC Pack][headnode]

* **Active Directory-domein** - het HPC Pack head knooppunt deel van een Active Directory-domein in Azure uitmaakt voordat u de HPC-services op de VM. Zoals in dit artikel, voor een bewijs van concept implementatie, kunt u de VM u maken voordat u begint met het HPC-services als een domeincontroller voor het knooppunt head promoveren. Een andere mogelijkheid is om een afzonderlijke domeincontroller en forest in Azure waarmee u deelnemen aan het hoofd knooppunt VM te implementeren.

* **Azure virtueel netwerk** - wanneer u het implementatiemodel Resource Manager gebruiken voor de implementatie van het knooppunt head, opgeven of een Azure virtueel netwerk maken. U kunt het virtuele netwerk gebruiken als u wilt deelnemen aan het hoofd knooppunt aan een bestaand Active Directory-domein. U moet ook het bericht later VMs-computerknooppunt toevoegen aan het cluster.

    
## <a name="steps-to-create-the-head-node"></a>Stappen voor het maken van de hoofd-knooppunt

Volgende hoofdstappen de Azure portal met een Azure VM voor het hoofd knooppunt HPC Pack maakt met behulp van de Resource Manager-implementatiemodel zijn. 


1. Als u een nieuw Active Directory-forest in Azure maken met afzonderlijke domeincontroller VMs wilt, is een optie met een [sjabloon Resource Manager](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Voor een eenvoudig bewijs van concept implementatie is het fijn deze stap overslaat en het hoofd knooppunt VM zelf configureert als domeincontroller. Deze optie wordt verderop in dit artikel beschreven.
    
2. Klik op het [HPC Pack 2012 R2 op Windows Server 2012 R2-pagina](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) in de markt Azure, **virtuele Machine maken**. 

3. Selecteer het model van de deployment **Resource Manager** en klik op **maken**in de portal op de pagina **HPC Pack 2012 R2 op Windows Server 2012 R2** .

    ![Afbeelding van de HPC-Pack][marketplace]

4. Configureren van de instellingen en de VM te maken via de portal. Bent u nieuw in Azure, volgt u de zelfstudie voor [maken van een virtuele Windows-computer in de portal Azure](virtual-machines-windows-hero-tutorial.md). Voor een bewijs van concept implementatie, kunt u meestal de standaardwaarde accepteren of aanbevolen instellingen.

    >[AZURE.NOTE]Als u deelnemen aan het hoofd knooppunt aan een bestaand Active Directory-domein in Azure wilt, moet dat u het virtuele netwerk voor het domein opgeven bij het maken van de VM.
       
4. Maken nadat u de VM en de VM wordt uitgevoerd, [de VM verbinding](virtual-machines-windows-connect-logon.md) met extern bureaublad. 

5. Lid worden van de VM aan een bestaand domeinforest of een domein-forest op de VM zelf maken.

    * Als u de VM in een Azure virtuele netwerk met een bestaand domeinforest gemaakt, lid worden van de VM aan het forest met standaardhulpprogramma's van Serverbeheer of met Windows PowerShell. Start opnieuw op.

    * Als u de VM gemaakt in een nieuw virtueel netwerk (zonder een bestaand domeinforest), bevordering van de VM vervolgens als domeincontroller. Gebruik standaard stappen installeren en configureren van de functie Active Directory Domain Services op het hoofdkantoor knooppunt. Zie voor gedetailleerde stappen [voor het installeren van een nieuwe Windows Server 2012 Active Directory-Forest](https://technet.microsoft.com/library/jj574166.aspx).

5. Nadat de VM wordt uitgevoerd en is gekoppeld aan een Active Directory-forest, als volgt de HPC Pack services starten:

    een. Verbinding maken met het hoofd knooppunt VM met een domeinaccount dat lid is van de lokale groep Administrators. Gebruik bijvoorbeeld de administrator-account die u instelt wanneer u het hoofd VM-knooppunt gemaakt.

    b. Windows PowerShell starten als beheerder voor een standaardconfiguratie head knooppunt en typ het volgende:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Het kan enkele minuten duren voordat de HPC Pack services te starten.

    Voor extra head knooppunt configuratie-opties, typt u `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Volgende stappen

* U kunt nu werken met het hoofd knooppunt van het cluster HPC Pack. Bijvoorbeeld start HPC Cluster Manager en de [Implementatie takenlijst](https://technet.microsoft.com/library/jj884141.aspx)voltooien.
* [Als u wilt verhogen het cluster berekenen capaciteit op aanvraag, Azure burst knooppunten](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) toevoegen in een cloud-service. 

* Probeer de werkbelasting van een test uitgevoerd op het cluster. Zie voor een voorbeeld, de HPC Pack [aan de slag](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
