<properties
 pageTitle="OpenFOAM met HPC Pack uitvoeren op Linux VMs | Microsoft Azure"
 description="Een Pack van Microsoft HPC-cluster op Azure implementeren en een taak OpenFOAM uitvoeren op meerdere Linux-computerknooppunten via een netwerk van RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>OpenFoam met Microsoft HPC Pack uitvoeren op een cluster RDMA Linux in Azure

In dit artikel laat zien hoe OpenFoam in Azure virtuele machines worden uitgevoerd. Hier kunt implementeren u een cluster Microsoft HPC Pack met computerknooppunten Linux op Azure en het uitvoeren van taken met Intel MPI een [OpenFoam](http://openfoam.com/) . U kunt RDMA kunnen Azure VMs voor de compute nodes, zodat de compute nodes via het netwerk Azure RDMA communiceren. Andere opties voor het uitvoeren van OpenFoam in Azure opnemen volledig geconfigureerde commerciële afbeeldingen beschikbaar in de markt, zoals de UberCloud van [2.3 op CentOS 6 OpenFoam](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)en door te voeren op [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (voor Open veld bewerking en manipulatie) is pakket een open source computational fluid dynamics (CFD)-software veel in de techniek en wetenschap in commerciële en academische organisaties gebruikt wordt. Het bevat hulpprogramma's voor meshing, met name snappyHexMesh, een parallelized mesher voor complexe CAD-geometrieën en vóór en na verwerking. Bijna alle processen parallel uitgevoerd, zodat gebruikers kunnen profiteren van computerhardware beschikken.  

Microsoft HPC Pack biedt functies voor het uitvoeren van grootschalige HPC en parallelle toepassingen, inclusief toepassingen MPI in clusters van virtuele machines van Microsoft Azure. HPC Pack ondersteunt ook actief Linux HPC-toepassingen op Linux VMs geïmplementeerd in een Pack HPC-cluster knooppunt berekenen. Zie [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) voor een inleiding tot het gebruik van Linux-computerknooppunten met HPC Pack.

>[AZURE.NOTE] In dit artikel ziet u hoe een Linux MPI werkbelasting met HPC Pack wordt uitgevoerd. Wordt ervan uitgegaan dat u enigszins bekend zijn met Linux Systeembeheer en werklast MPI op Linux-clusters met hebt. Als u versies van MPI en OpenFOAM die afwijken van de instellingen in dit artikel wordt weergegeven, is het mogelijk enkele stappen voor installatie en configuratie te wijzigen. 

## <a name="prerequisites"></a>Vereisten

*   **Computerknooppunten Pack HPC-cluster met Linux RDMA staat** - een Pack HPC-cluster implementeren met grootte A8, A9, H16r of H16rm, Linux-computerknooppunten met behulp van een [sjabloon Azure Resource Manager](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) of een [Azure PowerShell-script](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Zie [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) voor de voorwaarden en stappen voor beide opties. Als u de optie PowerShell script, Zie het voorbeeld configuratiebestand in de voorbeeldbestanden die aan het einde van dit artikel. Deze configuratie wordt gebruikt voor de implementatie van een HPC Pack Azure gebaseerde cluster dat bestaat uit een hoofd grootte A8 Windows Server 2012 R2-knooppunt en 2 grootte A8 SUSE Linux Enterprise Server 12-computerknooppunten. Vervangen door de juiste waarden voor uw abonnement en service namen. 

    **Meer dingen om te weten**

    *   Zie voor Linux RDMA netwerk vereisten in Azure, [H-serie en intensieve A-serie VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Als u de optie Powershell script-implementatie, implementeren van alle de Linux compute nodes binnen een cloud-service gebruik van verbinding met het RDMA.

    *   Na de implementatie van de knooppunten van Linux, verbinding maken via SSH administratieve taken uitvoeren. De details van de SSH-verbinding voor elke Linux VM in Azure portal vinden.  
        
*   **Intel MPI** - OpenFOAM uitgevoerd op HPC voor SLES 12 computerknooppunten in Azure, moet u de Intel MPI Library 5 runtime installeren vanaf de [site Intel.com](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 is voorgeïnstalleerd op CentOS gebaseerde HPC-afbeeldingen).  Installeer Intel MPI op uw Linux-computerknooppunten in een latere stap indien nodig. Als voorbereiding voor deze stap nadat u hebt geregistreerd met Intel, volg de link in de e-mail bevestiging naar de bijbehorende webpagina. Kopieer vervolgens de downloadkoppeling voor het bestand .tgz voor de juiste versie van Intel MPI. Dit artikel is gebaseerd op Intel MPI versie 5.0.3.048.

*   **OpenFOAM bron Pack** - Download de software OpenFOAM bron Pack voor Linux van de [OpenFOAM Foundation-site](http://openfoam.org/download/2-3-1-source/). Dit artikel is gebaseerd op de bron Pack versie 2.3.1 downloaden als OpenFOAM 2.3.1.tgz. Volg de instructies verderop in dit artikel, uitpakken en compileren van OpenFOAM op de Linux compute nodes.

*   **EnSight** (optioneel) - Zie de resultaten van de simulatie van OpenFOAM, downloaden en installeren van het programma voor visualisatie en analyse van [EnSight](https://www.ceisoftware.com/download/) . Informatie over licentieverlening en download zijn op de website EnSight.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Wederzijds vertrouwen tussen computerknooppunten instellen

Een taak tussen knooppunt wordt uitgevoerd op meerdere knooppunten voor Linux vereist de knooppunten die vertrouwen elkaar ( **rsh** of **ssh**). Wanneer u het Pack HPC-cluster met het implementatiescript Microsoft HPC Pack IaaS, het script worden automatisch ingesteld permanente wederzijds vertrouwen voor de administrator-account die u opgeeft. Tijdelijke wederzijds vertrouwen tussen de knooppunten instellen wanneer een taak wordt toegewezen aan deze voor gebruikers die geen beheerder u in het domein van het cluster maakt, en de relatie wordt verwijderd nadat de taak voltooid is. Als u wilt instellen voor elke gebruiker worden vertrouwd, bieden een RSA-sleutelpaar aan het cluster waarop HPC Pack voor de vertrouwensrelatie.

### <a name="generate-an-rsa-key-pair"></a>Een RSA-sleutelpaar genereren

Het is gemakkelijk voor het genereren van een RSA-sleutelpaar, dat een openbare sleutel en een persoonlijke sleutel bevat met de opdracht Linux **ssh-keygen** .

1.  Aanmelden bij een computer met Linux.

2.  Voer de volgende opdracht:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Druk op **Enter** om de standaardinstellingen te gebruiken totdat de opdracht is voltooid. Voer geen een wachtwoordzin hier; Wanneer u wordt gevraagd een wachtwoord in te voeren, drukt u op **Enter**.

    ![Een RSA-sleutelpaar genereren][keygen]

3.  Wijzig de map in de ~/.ssh map. De persoonlijke sleutel wordt opgeslagen in id_rsa en de openbare sleutel in id_rsa.pub.

    ![Persoonlijke en openbare sleutels][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Het sleutelpaar toevoegen aan het cluster HPC Pack
1.  Een extern bureaublad verbinding maken met uw hoofd knooppunt met de HPC Pack administrator-account (de beheerdersaccount u instellen bij het uitvoeren van een script voor de implementatie).

2. Standaard procedures voor Windows Server gebruik te maken van een domeingebruikersaccount in Active Directory-domein van het cluster. Gebruik bijvoorbeeld het hulpprogramma Active Directory: gebruikers en Computers op de hoofd-knooppunt. De voorbeelden in dit artikel wordt ervan uitgegaan dat u een gebruiker met de naam hpclab\hpcuser gemaakt.

3.  Maak een bestand met de naam C:\cred.xml en kopieer de RSA-sleutel gegevens erin. Er is een voorbeeld van een cred.xml-bestand aan het einde van dit artikel.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Open een opdrachtprompt en voer de volgende opdracht stelt u de referenties voor de account hpclab\hpcuser. Met de parameter **extendeddata** kunt u de naam van het C:\cred.xml dat die u hebt gemaakt voor de belangrijkste gegevens doorgeeft.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Deze opdracht is voltooid zonder de uitvoer. Nadat de referenties voor de gebruikersaccounts die u nodig hebt voor het uitvoeren van taken, het bestand cred.xml op een veilige locatie bewaren of verwijderen.

5.  Als u de RSA-sleutelpaar op een van de knooppunten Linux gegenereerd, moet u de sleutels verwijderen nadat u ze met. HPC Pack vindt een bestaand id_rsa bestand of id_rsa.pub bestand, dan wordt het wederzijds vertrouwen niet ingesteld.

>[AZURE.IMPORTANT] Wordt niet aanbevolen een Linux-taak wordt uitgevoerd als een beheerder van een cluster op een gedeelde cluster omdat een taak ingediend door een beheerder wordt uitgevoerd onder de root-account op de Linux-knooppunten. Een taak die is ingediend door een gebruiker die geen beheerder wordt echter uitgevoerd onder een lokale gebruikersaccount voor Linux met dezelfde naam als de gebruiker van de taak. In dit geval stelt HPC Pack wederzijds vertrouwen voor deze gebruiker Linux op de knooppunten die zijn toegewezen aan de taak. U kunt de gebruiker Linux handmatig op de Linux-knooppunten instellen voordat u de batchverwerking of HPC Pack maakt de gebruiker automatisch wanneer de taak wordt verzonden. Als de gebruiker maakt op HPC Pack, verwijderd HPC Pack nadat de taak is voltooid. Om beveiligingsrisico's verminderen, verwijderd HPC Pack de sleutels na de taakvoltooiing.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Een bestandsshare voor Linux knooppunten instellen

Nu een standaard SMB-share in een map op het hoofd knooppunt ingesteld. Koppel de gedeelde map op de knooppunten Linux zodat de knooppunten Linux toegang te krijgen tot bestanden met een algemeen pad. Als u wilt, kunt u een andere optie, zoals een share bestanden Azure - aanbevolen voor vele scenario's- of een NFS-share voor bestandsdeling. Zie het bestand delen van informatie en gedetailleerde stappen in [aan de slag met Linux computerknooppunten in een Cluster van HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Maak een map op het knooppunt head en deze voor iedereen te delen door het instellen van machtigingen voor lezen/schrijven. Bijvoorbeeld C:\OpenFOAM delen op het knooppunt in het hoofd als \\ \\SUSE12RDMA HN\OpenFOAM. *SUSE12RDMA HN* is hier de hostnaam van de hoofd-knooppunt.

2.  Open een Windows PowerShell-venster en voer de volgende opdrachten:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

De eerste opdracht maakt u een map met de naam /openfoam op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt u de gedeelde map //SUSE12RDMA-HN/OpenFOAM op de Linux-knooppunten met dir_mode en file_mode-bits ingesteld op 777. De *gebruikersnaam* en het *wachtwoord* in de opdracht moeten worden de referenties van een gebruiker op het knooppunt head.

>[AZURE.NOTE]De "\`" symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`," betekent dat de "," (komma teken) is een onderdeel van de opdracht.

## <a name="install-mpi-and-openfoam"></a>MPI en OpenFOAM installeren

OpenFOAM als een taak MPI op het netwerk RDMA uitgevoerd, moet u OpenFOAM compileren met de Intel MPI-bibliotheken. 

Voer eerst enkele opdrachten **clusrun** Intel MPI bibliotheken te installeren (indien nog niet geïnstalleerd) en OpenFOAM op uw Linux-knooppunten. Het hoofd knooppunt share eerder geconfigureerd voor het delen van installatiebestanden tussen de knooppunten Linux gebruiken.

>[AZURE.IMPORTANT]Deze installatie- en compileren stappen zijn voorbeelden. U moet enige kennis van Linux Systeembeheer te zorgen dat afhankelijke compilers en bibliotheken correct zijn geïnstalleerd. Wellicht moet u bepaalde omgevingsvariabelen of andere instellingen voor uw versie van Intel MPI en OpenFOAM. Zie voor meer informatie, [Intel MPI Library voor Linux Installation Guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) en [OpenFOAM bron Pack installatie](http://openfoam.org/download/2-3-1-source/) voor uw omgeving.


### <a name="install-intel-mpi"></a>Intel MPI installeren

Het gedownloade installatiepakket voor Intel MPI (l_mpi_p_5.0.3.048.tgz in dit voorbeeld) in C:\OpenFoam op het hoofd knooppunt zodanig opslaan dat de knooppunten Linux toegang dit bestand in /openfoam tot. **Clusrun** Intel MPI library installeren op alle knooppunten van de Linux wordt uitgevoerd.

1.  De volgende opdrachten kopieert u het installatiepakket en pak het /opt/intel op elk knooppunt.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Intel MPI Library stil installeren, via een silent.cfg-bestand. In de voorbeeldbestanden die aan het einde van dit artikel vindt u een voorbeeld. Plaats dit bestand in de gedeelde map /openfoam. Zie voor meer informatie over het bestand silent.cfg [Intel MPI Library voor Linux Installation Guide - stille installatie](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Zorg ervoor dat u uw silent.cfg bestand opslaan als een tekstbestand met Linux regeleinden heb (alleen LF, niet CR LF). Deze stap zorgt u ervoor dat deze wordt uitgevoerd correct op de Linux-knooppunten.

3.  Intel MPI Library installeren in de stille modus.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>MPI configureren

Voor het testen, moet u de volgende regels toevoegen aan de /etc/security/limits.conf op elk van de knooppunten voor Linux:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


De Linux-knooppunten opnieuw nadat u het bestand limits.conf bijwerken. Gebruik bijvoorbeeld de volgende opdracht voor **clusrun** :

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Start opnieuw op en zorg ervoor dat de gedeelde map is gekoppeld als /openfoam.

### <a name="compile-and-install-openfoam"></a>Compileren en installeren van OpenFOAM

Het gedownloade installatiepakket voor OpenFOAM bron Pack (OpenFOAM-2.3.1.tgz in dit voorbeeld) om C:\OpenFoam op het hoofd knooppunt zodanig opslaan dat Linux knooppunten toegang dit bestand in /openfoam tot. Opdrachten voor het compileren van OpenFOAM op alle knooppunten in de Linux **clusrun** wordt uitgevoerd.


1.  Maken van een map /opt/OpenFOAM op elk knooppunt Linux en pak deze het bronpakket kopiëren naar deze map.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Voor het compileren van OpenFOAM met de Intel MPI Library, eerst enkele omgevingsvariabelen instellen voor zowel Intel MPI en OpenFOAM. Gebruik een bash-script genaamd settings.sh om de variabelen. In de voorbeeldbestanden die aan het einde van dit artikel vindt u een voorbeeld. Plaats dit bestand (opgeslagen met regeleinden voor Linux) in de gedeelde map /openfoam. Dit bestand bevat ook instellingen voor de runtimes MPI en OpenFOAM waarmee u later een OpenFOAM taak uit te voeren.

3. Afhankelijke pakketten die nodig zijn voor het compileren van OpenFOAM installeren. Afhankelijk van uw distributie Linux moet u mogelijk eerst een opslagplaats toevoegen. Voer **clusrun** opdrachten met de volgende strekking:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Indien nodig, SSH voor elk knooppunt Linux om de opdrachten te bevestigen dat zij goed worden uitgevoerd.

4.  Voer de volgende opdracht voor het compileren van OpenFOAM. Het compilatie proces duurt enige tijd en genereert een grote hoeveelheid logboekgegevens naar de standaarduitvoer, dus de optie **/ interleaved** gebruiken om de interleaved uitvoer weer te geven.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]De "\`" symbool in de opdracht is een escapeteken voor PowerShell. "\`&" betekent de "&" is een onderdeel van de opdracht.

## <a name="prepare-to-run-an-openfoam-job"></a>Voorbereiding op het uitvoeren van een taak OpenFOAM

Nu bereid een MPI project genaamd sloshingTank3D, die een van de monsters van OpenFoam, op twee knooppunten voor Linux uit te voeren. 

### <a name="set-up-the-runtime-environment"></a>De runtime-omgeving instellen

Om voor te stellen de runtimeomgevingen MPI en OpenFOAM op de Linux-knooppunten, moet u de volgende opdracht uitvoeren in een Windows PowerShell-venster op het knooppunt head. (Deze opdracht is alleen geldig voor SUSE Linux.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

Gebruik de knooppunt head share die u eerder hebt geconfigureerd voor het delen van bestanden tussen de knooppunten van de Linux (gekoppeld als /openfoam).

1.  SSH aan een van uw Linux computerknooppunten.

2.  Voer de volgende opdracht om de runtime-omgeving van OpenFOAM in te stellen als u dit nog niet hebt gedaan.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Het monster sloshingTank3D kopiëren naar de gedeelde map en navigeer naar het.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Wanneer u de standaardparameters van dit voorbeeld, kost het tientallen minuten uit te voeren, zodat u sommige parameters kunt zodat deze sneller kunnen wijzigen. Een eenvoudige keuze is de tijd stap variabelen deltaT en writeInterval in het systeem/controlDict bestand te wijzigen. Dit bestand slaat alle ingevoerde gegevens met betrekking tot de controle van de tijd- en lezen en schrijven van oplossingsgegevens. Bijvoorbeeld, kon u de waarde van deltaT van 0,05 op 0,5 en de waarde van de writeInterval van 0,05 tot 0,5.

    ![Wijzig de variabelen in stap][step_variables]

5.  Gewenste waarden opgeven voor de variabelen in het systeem/decomposeParDict-bestand. In dit voorbeeld worden twee Linux knooppunten elk met 8 cores, dus numberOfSubdomains ingesteld op 16 en n van hierarchicalCoeffs tot (1 1 16), wat betekent dat parallel OpenFOAM met 16 processen. Zie voor meer informatie [OpenFOAM gebruikershandleiding: 3,4-toepassingen uitgevoerd in parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Ontleden van processen][decompose]

6.  Voer de volgende opdrachten uit de map sloshingTank3D op de voorbeeldgegevens voorbereiden.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Klik op het knooppunt hoofd ziet u dat de voorbeeldbestanden worden gekopieerd naar de C:\OpenFoam\sloshingTank3D. (C:\OpenFoam is de gedeelde map op het knooppunt head).

    ![Op het knooppunt head-gegevensbestanden][data_files]

### <a name="host-file-for-mpirun"></a>Hostbestand voor mpirun

In deze stap maakt u een hostbestand (een lijst met computerknooppunten) **mpirun** wordt gebruikt.

1.  Maakt een bestand met de naam hostfile onder /openfoam, zodat u dit bestand kan worden bereikt op /openfoam/hostfile op alle knooppunten van Linux op een van de knooppunten Linux.

2.  De namen van de nodes Linux naar dit bestand schrijven. In dit voorbeeld wordt bevat het bestand de volgende namen:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]U kunt dit bestand ook op C:\OpenFoam\hostfile op het hoofd knooppunt maken. Als u deze optie kiest, opslaan als een tekstbestand met Linux regeleinden (alleen LF, niet CR LF). Dit zorgt ervoor dat deze wordt uitgevoerd correct op de Linux-knooppunten.

    **Bash script wrapper**

    Als u veel Linux knooppunten hebt en u wilt dat uw taak uit te voeren op slechts enkele, is het niet verstandig een vaste hostbestand te gebruiken omdat u niet welke knooppunten weet, worden toegewezen aan de taak. In dit geval een bash script-wrapper voor **mpirun** voor het maken van het bestand met de host automatisch schrijven. U kunt een voorbeeld van de bash script wrapper genaamd hpcimpirun.sh aan het einde van dit artikel vinden en sla deze op als /openfoam/hpcimpirun.sh. Dit voorbeeldscript doet het volgende:

    1.  De omgevingsvariabelen voor **mpirun**en sommige opdrachtparameters toevoeging stelt de MPI-taak wordt uitgevoerd via het netwerk RDMA. In dit geval wordt de volgende variabelen:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = weergeeft van een-v2-ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Maakt een hostbestand afhankelijk van de omgeving variabele $CCP_NODES_CORES die door het hoofd HPC-knooppunt is ingesteld wanneer de taak wordt geactiveerd.

        De indeling van $CCP_NODES_CORES volgt deze notatie:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        waar

        * `<Number of nodes>`-het aantal knooppunten dat is toegewezen aan deze taak.  
        
        * `<Name of node_n_...>`-de naam van elk knooppunt is toegewezen aan deze taak.
        
        * `<Cores of node_n_...>`-het aantal cores op het knooppunt is toegewezen aan deze taak.

        Bijvoorbeeld, als de taak twee knooppunten moet worden uitgevoerd, is $CCP_NODES_CORES vergelijkbaar met
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Roept de opdracht **mpirun** en voegt twee parameters op de opdrachtregel.

        * `--hostfile <hostfilepath>: <hostfilepath>`-het pad van het bestand met de host het script maakt

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-een omgevingsvariabele ingesteld door het HPC Pack head knooppunt het nummer van de totale cores zijn toegewezen aan deze taak bevat. In dit geval geeft het aantal processen voor **mpirun**aan.


## <a name="submit-an-openfoam-job"></a>Een OpenFOAM-taak

Nu kunt u een taak HPC Cluster Manager verzenden. U moet het script hpcimpirun.sh doorgeven in de opdrachtregels voor sommige van de projecttaken.

1. Verbinding maken met uw hoofd clusterknooppunt en HPC-Cluster Manager starten.

2. **In de Resource Management**, ervoor te zorgen dat de compute nodes van Linux in de **on line** staat. Als dat niet het geval is, selecteert u deze en klikt u op **On line brengen**.

3.  Klik op **Nieuwe taak**in **Project Management**.

4.  Voer een naam voor de taak, zoals _sloshingTank3D_.

    ![Taakdetails][job_details]

5.  In **Project resources**, kiest u het type resource als 'Knooppunt' en de minimale ingesteld op 2. Deze configuratie wordt de taak uitgevoerd op twee knooppunten van Linux, die allemaal acht cores in dit voorbeeld.

    ![Project-resources][job_resources]

6. Klik op **Taken bewerken** in de navigatiebalk aan de linkerkant en klik vervolgens op **toevoegen** om een taak aan de taak toevoegen. Vier taken toevoegen aan het project met de volgende regels en instellingen.

    >[AZURE.NOTE]Met `source /openfoam/settings.sh` stelt de OpenFOAM en MPI runtime-omgevingen, zodat elk van de volgende taken voordat u de opdracht OpenFOAM aangeroepen.

    *   **Taak 1**. **DecomposePar** voor het genereren van gegevensbestanden voor het uitvoeren van **interDyMFoam** parallel worden uitgevoerd.
    
        *   Een van de knooppunten aan de taak toewijzen

        *   **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Werkmap** - openfoam/sloshingTank3D
        
        Zie de volgende afbeelding. U configureren de resterende taken op dezelfde manier.

        ![Details van taak 1][task_details1]

    *   **Taak 2**. **InterDyMFoam** voor het berekenen van het monster parallel uitgevoerd.

        *   Twee knooppunten aan de taak wilt toewijzen.

        *   **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Werkmap** - openfoam/sloshingTank3D

    *   **Taak 3**. **ReconstructPar** om de mappen uit de map processor_N_ tijd sets samenvoegen in één set worden uitgevoerd.

        *   Een van de knooppunten aan de taak toewijzen

        *   **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Werkmap** - openfoam/sloshingTank3D

    *   **Taak 4**. **FoamToEnsight** ter aanvulling van het OpenFOAM resultaat-bestanden converteren naar de indeling van de EnSight en de EnSight-bestanden plaatsen in een map met de naam Ensight in de map met de aanvraag worden uitgevoerd.

        *   Twee knooppunten aan de taak wilt toewijzen.

        *   **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Werkmap** - openfoam/sloshingTank3D

6.  Afhankelijkheden toevoegen aan deze taken in oplopende taakvolgorde.

    ![Afhankelijkheden van taken][task_dependencies]

7.  Klik op **verzenden** om te worden uitgevoerd.

    Standaard verzendt de HPC Pack de taak als uw huidige account van de aangemelde gebruiker. Nadat u op **verzenden**klikt, verschijnt er een dialoogvenster waarin u de gebruikersnaam en het wachtwoord invoeren.

    ![Project referenties][creds]

    Onder bepaalde omstandigheden onthoudt HPC Pack u de gebruikersgegevens invoeren voordat u dit dialoogvenster niet weergeven. Voer de volgende opdracht bij een opdrachtprompt en vervolgens dienen de taak zodat HPC Pack opnieuw weergeven.

    ```
    hpccred delcreds
    ```

8.  De taak heeft uit tientallen minuten tot enkele uren op basis van de parameters die u hebt ingesteld voor het monster. In de warmte-kaart ziet u de taak die wordt uitgevoerd op de knooppunten van Linux. 

    ![Warmte-kaart][heat_map]

    Op elk knooppunt, zijn acht processen gestart.

    ![Linux-processen][linux_processes]

9.  Wanneer de taak is voltooid, vindt u de resultaten van het project in mappen onder C:\OpenFoam\sloshingTank3D en de logboekbestanden op C:\OpenFoam.


## <a name="view-results-in-ensight"></a>Resultaten weergeven in EnSight

Eventueel [EnSight](https://www.ceisoftware.com/) gebruiken om te visualiseren en analyseren van de resultaten van het project OpenFOAM. Zie voor meer informatie over visualisatie en animatie in de EnSight, deze [video guide](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Nadat u EnSight op het hoofd knooppunt hebt geïnstalleerd, start u deze.

2.  Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    U ziet een tank in de viewer.

    ![Tank in EnSight][tank]

3.  Een **Isosurface** van **internalMesh**maken en kies vervolgens de variabele **alpha_water**.

    ![Een isosurface maken][isosurface]

4.  Stel de kleur voor **Isosurface_part** in de vorige stap hebt gemaakt. Bijvoorbeeld instellen dat aan het water blauw.

    ![Isosurface kleur bewerken][isosurface_color]

5.  Een **Iso-volume** van **wanden** maken door de **wanden** te selecteren in het deelvenster **onderdelen** en **Isosurfaces** klikt u op de werkbalk.

6.  In het dialoogvenster Selecteer **Type** als **Isovolume** en de Min van **Isovolume bereik** ingesteld op 0,5. Klik op **maken met geselecteerde onderdelen**om de isovolume.

7.  Stel de kleur voor **Iso_volume_part** in de vorige stap hebt gemaakt. Bijvoorbeeld instellen aan het water diep blauw.

8.  Stel de kleur voor de **muren**. Deze bijvoorbeeld ingesteld op Transparant wit.

9. Klik op **afspelen** om te zien van de resultaten van de simulatie.

    ![Resultaat van de tank][tank_result]

## <a name="sample-files"></a>Voorbeeldbestanden

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Voorbeeld XML-configuratiebestand voor cluster implementeren door PowerShell script

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Voorbeeld van bestand cred.xml

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Voorbeeld van bestand silent.cfg MPI installeren

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Voorbeeldscript voor settings.sh

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Voorbeeldscript voor hpcimpirun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
