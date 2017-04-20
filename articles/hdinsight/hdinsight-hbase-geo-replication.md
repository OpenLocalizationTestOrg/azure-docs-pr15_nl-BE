<properties 
   pageTitle="Configureer de replicatie tussen twee datacenters HBase | Microsoft Azure" 
   description="Informatie over het HBase replicatie tussen twee datacenters en over de use-cases voor replicatie cluster configureren." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>HBase geo-replicatie configureren in HDInsight

> [AZURE.SELECTOR]
- [VPN-verbindingen configureren](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS configureren](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replicatie configureren](hdinsight-hbase-geo-replication.md) 
 
Informatie over het configureren HBase replicatie tussen twee datacenters. Sommige gevallen gebruiken voor replicatie cluster omvatten:

- Back-up en noodherstel
- Het samenvoegen van gegevens
- Verdeling van de geografische gegevens
- On line gegevens ingestie gecombineerd met analytics voor off line gegevens

Cluster-replicatie gebruikt een bron-push-methodologie. Een cluster HBase kan een bron, bestemming, of beide rollen tegelijk kunt uitvoeren. Asynchrone replicatie en het doel van de replication is uiteindelijk consistentie. Als de bron een bewerking naar de familie van een kolom met replicatie is ingeschakeld ontvangt, wordt die bewerken doorgegeven aan alle clusters van bestemming. Wanneer gegevens worden gerepliceerd uit een cluster naar het andere, worden het bron-cluster en alle clusters die de gegevens al zijn gevoederd bijgehouden om te voorkomen dat replicatie lussen. Voor meer informatie configureert In deze zelfstudie, u een bron-en doel-replicatie.  Zie voor andere clustertopologieën [Apache HBase Reference Guide](http://hbase.apache.org/book.html#_cluster_replication).

Dit is het derde deel van de reeks:

- [Een VPN-verbinding tussen twee virtuele netwerken configureren][hdinsight-hbase-replication-vnet]
- [DNS configureren voor de virtuele netwerken][hdinsight-hbase-replication-dns]
- HBase geo-replicatie (deze zelfstudie) configureren

In het volgende diagram ziet u de twee virtuele netwerken en de netwerkverbinding die u hebt gemaakt in [een VPN-verbinding tussen twee virtuele netwerken configureren] [ hdinsight-hbase-geo-replication-vnet] en [DNS configureren voor de virtuele netwerken][hdinsight-hbase-replication-dns]: 

![HDInsight HBase replicatie virtuele netwerkdiagram][img-vnet-diagram]

## <a id="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**.

    Als PowerShell scripts uitvoeren, moet u Azure PowerShell als administrator uitvoeren en de uitvoering van beleid ingesteld op *RemoteSigned*. Zie Werken met de cmdlet Set-uitvoeringsbeleid.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Twee Azure virtueel netwerk met een VPN-verbinding met en DNS is geconfigureerd**.  Zie [een VPN-verbinding tussen twee Azure virtuele netwerken configureren]voor meer informatie,[hdinsight-hbase-replication-vnet], en [DNS configureren tussen twee netwerken van Azure virtual][hdinsight-hbase-replication-dns].


    Voordat u PowerShell scripts, moet dat u verbonden bent met uw Azure-abonnement met de volgende cmdlet:

        Add-AzureAccount

    Als er meerdere Azure abonnementen, gebruikt u de volgende cmdlet instellen van het huidige abonnement:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Bepaling HBase clusters in HDInsight

In [een VPN-verbinding tussen twee Azure virtuele netwerken configureren][hdinsight-hbase-replication-vnet], u hebt een virtueel netwerk in een Europa gemaakt datacenter en een virtueel netwerk in een Amerikaanse Datacenter. De twee virtuele netwerk zijn aangesloten via VPN. In deze sessie, wordt u een cluster HBase in elk van de virtuele netwerken inrichten. Verderop in deze zelfstudie brengt u een van de clusters HBase voor het repliceren van de andere HBase cluster.

De klassieke Portal Azure ondersteunt geen provisioning HDInsight clusters met aangepaste configuratie-opties. Bijvoorbeeld, *hbase.replication* ingesteld op *true*. Als u de waarde in het configuratiebestand als een cluster wordt ingericht, verliest u de instelling nadat het cluster is wordt reimaged. Zie voor meer informatie [verstrekken Hadoop clusters in HDInsight][hdinsight-provision]. Een van de opties voor het inrichten van de cluster HDInsight met aangepaste opties Azure PowerShell gebruikt.


**Inrichten van een HBase Cluster in Contoso-VNet-EU** 

1. Open Windows PowerShell ISE vanuit uw werkstation.
2. Stel de variabelen aan het begin van het script en vervolgens het script uitvoeren.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Inrichten van een HBase Cluster in Contoso-VNet-US** 

- Hetzelfde script met de volgende waarden gebruiken:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Omdat u al hebt aangesloten op uw account Azure, moet u niet meer met het uitvoeren van de volgende comlets:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Voorwaardelijke doorstuurserver voor DNS configureren

In [DNS configureren voor de virtuele netwerken][hdinsight-hbase-replication-dns], DNS-servers voor twee netwerken is geconfigureerd. HBase clusters hebben verschillende domeinachtervoegsels. Zo moet u extra DNS voorwaardelijke doorstuurservers configureren.

Voorwaardelijke doorstuurserver configureren, moet u weten de domeinachtervoegsels HBase twee clusters. 

**De domeinachtervoegsels van twee clusters HBase zoeken**

1. RDP in **Contoso HBase EU**.  Zie voor instructies [de clusters beheren Hadoop in HDInsight met de klassieke Portal Azure][hdinsight-manage-portal]. Het is eigenlijk een headnode0 van het cluster.
2. Open een Windows PowerShell-console of vanaf de opdrachtprompt.
3. **Ipconfig**uitvoeren en schrijf **verbindingsspecifiek DNS-achtervoegsel**.
4. U moet de RDP-sessie niet sluiten.  U moet het bericht later Domeinnaamomzetting testen.
5. Herhaal dezelfde stappen uit om erachter te komen van de **verbindingsspecifieke DNS-achtervoegsel** van **Contoso HBase VS**.


**Voor het configureren van DNS-doorstuurservers**
 
1.  RDP in **DNS de Contoso-EU**. 
2.  Klik op de Windows-toets linksonder.
2.  Klik op **Systeembeheer**.
3.  Klik op **DNS**.
4.  Vouw in het linkerdeelvenster **DSN** **Contoso DNS EU**.
5.  Klik met de rechtermuisknop op **Voorwaardelijke doorstuurservers**en klik vervolgens op **Nieuwe voorwaardelijke doorstuurserver**. 
5.  Voer de volgende gegevens:
    - **DNS-domein**: Voer het DNS-achtervoegsel van de Contoso-HBase-US. Bijvoorbeeld: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **IP-adressen van de masterservers**: 10.2.0.4, namelijk de Contoso-DNS-Verenigde Staten van IP-adres invoeren. Controleer of de IP. De DNS-server kan een ander IP-adres hebben.
6.  Druk op **ENTER**en klik vervolgens op **OK**.  Nu kun je de Contoso-DNS-Verenigde Staten van IP-adres van DNS-Contoso EU oplossen.
7.  Herhaal de stappen voor het toevoegen van een voorwaardelijke doorstuurserver voor DNS om de DNS-service op de Contoso-DNS-US virtuele machine met de volgende waarden:
    - **DNS-domein**: Voer het DNS-achtervoegsel van de Contoso-HBase-EU. 
    - **IP-adressen van de masterservers**: 10.2.0.4, namelijk de Contoso-DNS-EU-IP-adres invoeren.

**Domeinnaamomzetting testen**

1. Overschakelen naar het venster RDP Contoso HBase EU.
2. Open een opdrachtprompt.
3. De opdracht ping uitvoeren:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    De ICM-protocol is ingeschakeld op de knooppunten van de werknemer van de clusters HBase

4. Sluit niet de RDP-sessie. Toch moet u deze later in de zelfstudie.
5. Herhaal dezelfde stappen uit om de headnode0 van de Contoso-HBase-EU van Contoso-HBase-US ping.

>[AZURE.IMPORTANT] DNS moet werken voordat u met de volgende stappen verdergaat.

## <a name="enable-replication-between-hbase-tables"></a>Replicatie tussen HBase tabellen inschakelen

Nu kunt u een voorbeeldtabel HBase maken, replicatie inschakelen en test deze met enkele gegevens. De voorbeeldtabel met u heeft twee kolom families: persoonlijke en Office. 

In deze zelfstudie brengt u het cluster HBase Europa als bron en het cluster, het Amerikaanse HBase cluster als het cluster bestemming.

HBase tabellen met dezelfde namen en kolom families op de bron- en doel-clusters maken zodat het cluster bestemming waar u weet voor het opslaan van gegevens ontvangt. Zie voor meer informatie over het gebruik van de HBase-shell [aan de slag met Apache HBase in HDInsight][hdinsight-hbase-get-started].

**Voor het maken van een tabel HBase op HBase de Contoso-EU**

1. Overschakelen naar het venster **Contoso HBase EU** RDP.
2. Vanaf het bureaublad, klikt u op **de opdrachtregel Hadoop**.
2. De map naar de basismap HBase wijzigen:

        cd %HBASE_HOME%\bin
3. Open de shell HBase:

        hbase shell
4. Maak een tabel HBase:

        create 'Contacts', 'Personal', 'Office'
5. Sluit niet de RDP-sessie noch het opdrachtregelvenster Hadoop. Toch moet u ze later in de zelfstudie.
    
**Voor het maken van een tabel HBase op HBase Contoso VS**

- Herhaal dezelfde stappen te maken van dezelfde tabel op HBase Contoso VS.


**Contoso HBase VS toevoegen als een replicatie-peer**

1. Overschakelen naar het venster van de RDP **Contso HBase_EU** .
2. Vanuit het venster HBase shell, voeg toe het cluster bestemming (Contoso HBase VS) als een peer, bijvoorbeeld:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    In het voorbeeld is het domeinachtervoegsel *contoso-hbase-us.d4.internal.cloudapp.net*. U moet bijwerken zodat het overeenkomt met het domeinachtervoegsel van het cluster ons HBase. Zorg ervoor dat er geen spaties tussen de hostnamen.

**Elke kolom familie worden gerepliceerd op de bron-cluster configureren**

1. Configureren voor elke kolom familie worden gerepliceerd vanuit het venster HBase shell van de **Contso HBase EU** RDP-sessie:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Massaal uploaden van gegevens naar de tabel HBase**

Voorbeeld van een gegevensbestand is geüpload naar een openbare Azure Blob-container met de volgende URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

De inhoud van het bestand:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

U kunt het bestand uploaden naar uw cluster HBase en importeer de gegevens vanuit.

1. Overschakelen naar het venster **Contoso HBase EU** RDP.
2. Vanaf het bureaublad, klikt u op **de opdrachtregel Hadoop**.
3. De map naar de basismap HBase wijzigen:

        cd %HBASE_HOME%\bin

4. uploaden van de gegevens:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Controleer of de dat gegevensreplicatie plaatsvindt

U kunt controleren dat of de replicatie plaatsvindt door het scannen van de tabellen uit beide clusters met de volgende HBase shell-opdrachten:

        Scan 'Contacts'


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe HBase replicatie configureren via twee datacenters. Voor meer informatie over HDInsight en HBase, Zie:

- [HDInsight-servicepagina](https://azure.microsoft.com/services/hdinsight/)
- [HDInsight documentatie](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Aan de slag met Apache HBase in HDInsight][hdinsight-hbase-get-started]
- [HDInsight HBase-overzicht][hdinsight-hbase-overview]
- [HBase clusters op Azure Virtual Network inrichten][hdinsight-hbase-provision-vnet]
- [Realtime Twitter sentiment met HBase analyseren][hdinsight-hbase-twitter-sentiment]
- [Analyseren van sensorgegevens met Storm en HBase in HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
