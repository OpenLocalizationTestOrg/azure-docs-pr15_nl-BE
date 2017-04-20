<properties
 pageTitle="NAMD met Microsoft HPC Pack op Linux VMs | Microsoft Azure"
 description="Een Pack van Microsoft HPC-cluster op Azure implementeren en uitvoeren van een simulatie van NAMD met charmrun op meerdere Linux-computerknooppunten"
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
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>NAMD uitvoeren met Microsoft HPC Pack op Linux-computerknooppunten in Azure

In dit artikel laat zien hoe een werkbelasting Linux high performance computing (HPC) op Azure virtuele machines worden uitgevoerd. Hier u een cluster [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) op Azure met computerknooppunten Linux instellen en uitvoeren van een simulatie van de [NAMD](http://www.ks.uiuc.edu/Research/namd/) om te berekenen en de structuur van een grote biomoleculaire visualiseren.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (voor moleculaire Dynamics Nanoscale programma) is een parallelle moleculaire dynamics-pakket dat is ontworpen voor krachtige simulatie van grote biomoleculaire systemen met miljoenen atomen. Voorbeelden van deze systemen zijn virussen, cel structuren en grote eiwitten. NAMD worden geschaald naar honderden cores voor typische simulaties met meer dan 500.000 cores voor de grootste simulaties.

* **Microsoft HPC Pack** biedt functies voor het uitvoeren van grootschalige HPC en parallelle toepassingen in clusters van computers in de bedrijfsruimten of Azure virtuele machines. Oorspronkelijk ontwikkeld als een oplossing voor een standaardwerkbelasting Windows HPC, HPC Pack nu ondersteunt Linux HPC-toepassingen uitvoert op Linux VMs van knooppunt in een cluster HPC Pack geïmplementeerd berekenen. Zie [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) voor een introductie.

Zie voor andere opties voor Linux HPC werklasten worden uitgevoerd in Azure, [technische bronnen voor batch en high-performance computing](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Vereisten

* **Computerknooppunten Pack HPC-cluster met Linux** - implementatie van een cluster HPC Pack met computerknooppunten Linux op Azure met behulp van een [sjabloon Azure Resource Manager](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) of een [Azure PowerShell-script](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Zie [aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) voor de voorwaarden en stappen voor beide opties. Als u de optie PowerShell script, Zie het voorbeeld configuratiebestand in de voorbeeldbestanden die aan het einde van dit artikel. Dit bestand wordt een Azure-gebaseerde HPC Pack cluster dat bestaat uit een hoofd knooppunt met Windows Server 2012 R2 en vier grote CentOS 6.6 grootte-computerknooppunten geconfigureerd. Dit bestand aanpassen voor uw omgeving.


* **NAMD software en zelfstudie bestanden** - NAMD Download software voor Linux op de site [NAMD](http://www.ks.uiuc.edu/Research/namd/) (registratie verplicht). Dit artikel is gebaseerd op versie 2.10 NAMD en het archief [Linux-x86_64 (64-bits Intel/AMD met Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) gebruikt. Ook de [NAMD, zelfstudiebestanden](http://www.ks.uiuc.edu/Training/Tutorials/#namd)downloaden. De downloads zijn bestanden .tar en moet u een Windows-hulpprogramma voor het uitpakken van de bestanden op het clusterknooppunt head. De bestanden wilt uitpakken, volgt u de aanwijzingen verderop in dit artikel. 

* **VMD** (optioneel) - de resultaten van het project NAMD downloaden en installeren van het programma voor moleculaire visualisatie [VMD](http://www.ks.uiuc.edu/Research/vmd/) op een computer van uw keuze. De huidige versie is 1.9.2. Zie de VMD downloaden van de site aan de slag.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Wederzijds vertrouwen tussen computerknooppunten instellen
Een taak tussen knooppunt wordt uitgevoerd op meerdere knooppunten voor Linux vereist de knooppunten die vertrouwen elkaar ( **rsh** of **ssh**). Wanneer u het Pack HPC-cluster met het implementatiescript Microsoft HPC Pack IaaS, het script worden automatisch ingesteld permanente wederzijds vertrouwen voor de administrator-account die u opgeeft. Voor gebruikers die geen beheerder u in het domein van het cluster maakt, moeten tijdelijke wederzijds vertrouwen tussen de knooppunten instellen wanneer u een taak aan hen is toegewezen. De relatie vervolgens te vernietigen nadat de taak voltooid is. Bieden hiervoor voor elke gebruiker een RSA-sleutelpaar aan het cluster HPC Pack wordt de vertrouwensrelatie tot stand brengen. Voer de instructies.

### <a name="generate-an-rsa-key-pair"></a>Een RSA-sleutelpaar genereren
Het is gemakkelijk voor het genereren van een RSA-sleutelpaar, dat een openbare sleutel en een persoonlijke sleutel bevat met de opdracht Linux **ssh-keygen** .

1.  Aanmelden bij een computer met Linux.

2.  Voer de volgende opdracht:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Druk op **Enter** om de standaardinstellingen te gebruiken totdat de opdracht is voltooid. Voer geen een wachtwoordzin hier; Wanneer u wordt gevraagd een wachtwoord in te voeren, drukt u op **Enter**.

    ![Een RSA-sleutelpaar genereren][keygen]

3.  Wijzig de map in de ~/.ssh map. De persoonlijke sleutel wordt opgeslagen in id_rsa en de openbare sleutel in id_rsa.pub.

    ![Persoonlijke en openbare sleutels][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Het sleutelpaar toevoegen aan het cluster HPC Pack
1.  [Verbinding maken met extern bureaublad](virtual-machines-windows-connect-logon.md) naar het hoofd knooppunt VM met behulp van het domein met de referenties vermeld wanneer u het cluster (bijvoorbeeld hpc\clusteradmin) geïmplementeerd. U beheren het cluster vanaf het knooppunt head.

2. Standaard procedures voor Windows Server gebruik te maken van een domeingebruikersaccount in Active Directory-domein van het cluster. Gebruik bijvoorbeeld het hulpprogramma Active Directory: gebruikers en Computers op de hoofd-knooppunt. De voorbeelden in dit artikel wordt ervan uitgegaan dat u een gebruiker met de naam hpcuser in het domein hpclab (hpclab\hpcuser).

3. De domeingebruiker toevoegen aan het Pack HPC-cluster als een cluster-gebruiker. Zie voor instructies [toevoegen of verwijderen, gebruikers van cluster](https://technet.microsoft.com/library/ff919330.aspx).

2.  Maak een bestand met de naam C:\cred.xml en kopieer de RSA-sleutel gegevens erin. In de voorbeeldbestanden die aan het einde van dit artikel vindt u een voorbeeld.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Open een opdrachtprompt en voer de volgende opdracht stelt u de referenties voor de account hpclab\hpcuser. Met de parameter **extendeddata** kunt u de naam van het bestand C:\cred.xml dat u hebt gemaakt voor de belangrijkste gegevens doorgeeft.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Deze opdracht is voltooid zonder de uitvoer. Nadat de referenties voor de gebruikersaccounts die u nodig hebt voor het uitvoeren van taken, het bestand cred.xml op een veilige locatie bewaren of verwijderen.

5.  Als u de RSA-sleutelpaar op een van de knooppunten Linux gegenereerd, moet u de sleutels verwijderen nadat u ze met. HPC Pack stelt geen wederzijds vertrouwen als er een bestaand id_rsa-bestand of het bestand id_rsa.pub.

>[AZURE.IMPORTANT] Wordt niet aanbevolen een Linux-taak wordt uitgevoerd als een beheerder van een cluster op een gedeelde cluster omdat een taak ingediend door een beheerder wordt uitgevoerd onder de root-account op de Linux-knooppunten. Een taak die is ingediend door een gebruiker zonder beheerdersmachtigingen wordt uitgevoerd onder een lokale gebruikersaccount voor Linux met dezelfde naam als de gebruiker van de taak. In dit geval stelt HPC Pack wederzijds vertrouwen voor deze gebruiker Linux op de knooppunten die zijn toegewezen aan de taak. U kunt de gebruiker Linux handmatig op de Linux-knooppunten instellen voordat u de batchverwerking of HPC Pack maakt de gebruiker automatisch wanneer de taak wordt verzonden. Als de gebruiker maakt op HPC Pack, verwijderd HPC Pack nadat de taak is voltooid. Om beveiligingsrisico, worden de sleutels verwijderd nadat de taak is voltooid op de knooppunten.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Een bestandsshare voor Linux knooppunten instellen

Nu een SMB-bestandsshare instellen en koppelen van de gedeelde map op alle knooppunten van Linux kunnen de Linux-knooppunten toegang krijgen tot bestanden met een algemeen pad NAMD. Hieronder volgen de stappen voor het koppelen van een gedeelde map op het knooppunt head. Een share wordt aanbevolen voor distributies zoals CentOS 6.6 die momenteel de Azure File-service niet ondersteunen. Als de knooppunten Linux een bestandsshare Azure ondersteunt, Zie [Azure bestandsopslag met Linux gebruiken](../storage/storage-how-to-use-files-linux.md). Zie voor extra bestand opties voor delen met HPC Pack, [aan de slag met Linux computerknooppunten in een Cluster van HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Maak een map op het knooppunt head en deze voor iedereen te delen door het instellen van machtigingen voor lezen/schrijven. In dit voorbeeld wordt \\ \\CentOS66HN\Namd is de naam van de map waar CentOS66HN de hostnaam van de hoofd-knooppunt is.

2. Maak een submap met de naam namd2 in de gedeelde map. In namd2, maakt u een nieuwe submap met de naam namdsample.

3. Pak de NAMD bestanden in de map met behulp van een Windows-versie van **tar** of een ander Windows-hulpprogramma dat werkt op .tar archieven. 
    * Uitpakken van het tar-archief NAMD op \\ \\CentOS66HN\Namd\namd2.
    
    * Pak de bestanden van de zelfstudie onder \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Open een Windows PowerShell-venster en voer de volgende opdrachten voor het koppelen van de gedeelde map op de Linux-knooppunten.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

De eerste opdracht maakt u een map met de naam /namd2 op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt u de gedeelde map //CentOS66HN/Namd/namd2 naar de map met dir_mode en file_mode-bits ingesteld op 777. De *gebruikersnaam* en het *wachtwoord* in de opdracht moeten worden de referenties van een gebruiker op het knooppunt head.

>[AZURE.NOTE]De "\`" symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`," betekent dat de "," (komma teken) is een onderdeel van de opdracht.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Maak een Bash-script voor het uitvoeren van een taak NAMD

De NAMD moet *een knooppuntenbestand voor **charmrun** om te bepalen van het aantal knooppunten te gebruiken bij het starten van de processen van de NAMD* . U gebruikt een Bash script die compatibel is met deze knooppuntenbestand **charmrun** en de knooppuntenbestand genereert. Vervolgens kunt u een taak NAMD in HPC Cluster Manager die dit script aanroept indienen.

Met behulp van een teksteditor van uw keuze, een Bash-script maken in de /namd2 map met de programmabestanden van NAMD en geef deze de naam hpccharmrun.sh. Kopieer het voorbeeld hpccharmrun.sh script aan het einde van dit artikel bedoelde en Ga naar [indienen een taak NAMD](#submit-a-namd-job)voor een snelle drukproef van begrip.

>[AZURE.TIP] Sla het script als een tekstbestand met Linux regeleinden (alleen LF, niet CR LF). Dit zorgt ervoor dat deze wordt uitgevoerd correct op de Linux-knooppunten.

Hieronder vindt u meer informatie over de werking van dit bash-script. 

1.  Sommige variabelen definiëren.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Knooppunt informatie ophalen uit de omgevingsvariabelen. $NODESCORES bevat een lijst met woorden splitsen van $CCP_NODES_CORES. $COUNT is de grootte van de $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    De indeling voor de variabele $CCP_NODES_CORES is als volgt:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Deze variabele geeft het totale aantal knooppunten knooppuntnamen en het aantal cores op elk knooppunt dat aan de taak is toegewezen. Bijvoorbeeld, als de taak 10 cores moet worden uitgevoerd, lijkt de waarde van $CCP_NODES_CORES op:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Als de variabele $CCP_NODES_CORES is niet ingesteld, start u direct **charmrun** . (Dit alleen gebeurt wanneer u dit script rechtstreeks op uw Linux-knooppunten uitvoert.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Of maak een knooppuntenbestand voor **charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  **Charmrun** worden uitgevoerd met de knooppuntenbestand, krijgen de status van het resultaat en de knooppuntenbestand aan het einde verwijderen.

    ${CCP_NUMCPUS} is een andere variabele instellen door het hoofd knooppunt HPC Pack. Het aantal totale cores zijn toegewezen aan deze taak wordt opgeslagen. We gebruiken deze geeft het aantal processen voor charmrun.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Sluit af met de status van het resultaat **charmrun** .

    ```
    exit ${RTNSTS}
    ```



Dit is de informatie in het knooppuntenbestand dat het script wordt gegenereerd:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Bijvoorbeeld:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Een NAMD-taak verzenden

U bent nu gereed voor een NAMD taak HPC Cluster Manager verzenden.

1.  Verbinding maken met uw hoofd clusterknooppunt en HPC-Cluster Manager starten.

2.  **Resource Management**zorgen ervoor dat de compute nodes van Linux in de **on line** staat. Als dat niet het geval is, selecteert u deze en klikt u op **On line brengen**.

2.  Klik op **Nieuwe taak**in **Project Management**.

3.  Voer een naam voor de taak, zoals *hpccharmrun*.

    ![Nieuwe HPC-taak][namd_job]

4.  Op de pagina **Details van** onder **Resources in Project**, selecteert u het type resource als **knooppunt** en het **Minimum** is ingesteld op 3. , wij de taak uitgevoerd op drie Linux knooppunten en elk knooppunt heeft vier cores.

    ![Project-resources][job_resources]

5. Klik op **Taken bewerken** in de navigatiebalk aan de linkerkant en klik vervolgens op **toevoegen** om een taak aan de taak toevoegen.    


6. Stel op de pagina **i/o-apparaten en de Details van** de volgende waarden:

    * **Vanaf de opdrachtregel** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] De voorgaande opdrachtregel is een enkele opdracht zonder regeleinden. Loopt deze op meerdere regels onder de **opdrachtregel**worden weergegeven.

    * **Werkmap** - /namd2

    * **Minimum** - 3

    ![Taakdetails][task_details]

    >[AZURE.NOTE] De werkmap ingestelde hier omdat **charmrun** probeert om te navigeren naar dezelfde werkmap op elk knooppunt. Als de werkmap niet is ingesteld, start HPC Pack de opdracht in de map met een willekeurige naam gemaakt op een van de knooppunten voor Linux. Hierdoor wordt de volgende fout op de andere knooppunten: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` opgeven om te voorkomen dat dit probleem, een pad naar de map die toegankelijk is voor alle knooppunten als de werkmap.

5.  Klik op **OK** en klik vervolgens op **verzenden** om te worden uitgevoerd.

    Standaard verzendt de HPC Pack de taak als uw huidige account van de aangemelde gebruiker. Het dialoogvenster vraagt u mogelijk om de gebruikersnaam en het wachtwoord invoeren nadat u op **verzenden**klikt.

    ![Project referenties][creds]

    Onder bepaalde omstandigheden onthoudt HPC Pack u de gebruikersgegevens invoeren voordat u dit dialoogvenster niet weergeven. Voer de volgende opdracht bij een opdrachtprompt en vervolgens dienen de taak zodat HPC Pack opnieuw weergeven.

    ```command
    hpccred delcreds
    ```

6.  De taak duurt enkele minuten duren.

7.  Zoeken naar het logboek taak op \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log en de uitvoerbestanden in \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Eventueel VMD om de resultaten van uw project weer te starten. De uitvoer van de stappen voor het visualiseren van de NAMD bestanden (in dit geval, een ubiquitin eiwit molecuul in een bol water) vallen buiten het bestek van dit artikel. Zie [Zelfstudie voor NAMD](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) voor meer informatie.

    ![Project resultaten][vmd_view]

## <a name="sample-files"></a>Voorbeeldbestanden

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Voorbeeld XML-configuratiebestand voor cluster implementeren door PowerShell script

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Voorbeeldscript voor hpccharmrun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
