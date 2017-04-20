<properties
    pageTitle="Beschikbaarheid van Hadoop clusters in HDInsight | Microsoft Azure"
    description="HDInsight implementeert maximaal beschikbare en betrouwbare clusters met een extra knooppunt head."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Beschikbaarheid en betrouwbaarheid van Windows gebaseerde Hadoop clusters in HDInsight


>[AZURE.NOTE] De stappen in dit document zijn specifiek voor de HDInsight op basis van Windows-clusters. Zie als u een Linux-gebaseerde clusters, [beschikbaarheid en betrouwbaarheid van Hadoop Linux gebaseerde clusters in het HDInsight](hdinsight-high-availability-linux.md) voor Linux-specifieke informatie.

HDInsight kunnen klanten diverse typen cluster, voor andere data analytics werkbelasting te implementeren. Cluster vandaag aangeboden worden clusters voor query en analyse werklasten HBase clusters voor NoSQL werkbelasting en de Storm clusters voor real-time gebeurtenis processorbelasting Hadoop. Er zijn verschillende rollen voor de verschillende knooppunten binnen een bepaald cluster-type. Bijvoorbeeld:



- Hadoop clusters voor HDInsight worden geïmplementeerd met twee rollen:
    - Hoofd-knooppunt (2 knooppunten)
    - Gegevensknooppunt (minimaal 1 knooppunt)

- HBase clusters voor HDInsight worden geïmplementeerd met de drie rollen:
    - Hoofd-servers (knooppunten 2)
    - Regio-servers (minimaal 1 knooppunt)
    - Master/Zookeeper knooppunten (knooppunten 3)

- Storm-clusters voor HDInsight worden geïmplementeerd met de drie rollen:
    - Nimbus knooppunten (knooppunten 2)
    - Supervisor servers (minimaal 1 knooppunt)
    - Zookeeper knooppunten (knooppunten 3)

Standaard implementaties van Hadoop clusters hebben meestal één knooppunt head. HDInsight Hiermee verwijdert u dit potentieel risico met het toevoegen van een secundaire head knooppunt/HEAD server/Nimbus knooppunt verhogen de beschikbaarheid en betrouwbaarheid van de service die nodig zijn voor het beheren van de werkbelasting. Deze knooppunten head knooppunten/dier servers/Nimbus probleemloos beheren van storingen in knooppunten werknemer zijn ontworpen, maar eventuele storingen van services op het knooppunt head master zou ertoe leiden dat het cluster niet meer werken.


[ZooKeeper](http://zookeeper.apache.org/ ) knooppunten (ZKs) zijn toegevoegd en worden gebruikt voor leader election head knooppunten en om te verzekeren dat de werknemer als failover uitvoeren naar het secundaire head knooppunt (hoofd knooppunt1) wanneer de kop actief knooppunt (hoofd Node0) inactief weet knooppunten en gateways (GWs).

![Diagram van de uiterst betrouwbare head knooppunten in de uitvoering HDInsight Hadoop.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Status van actieve head knooppunt service controleren
Om te bepalen welke hoofd knooppunt actief is en controleer de status van de services die worden uitgevoerd op dat knooppunt head, moet u het Hadoop-cluster met behulp van Remote Desktop Protocol (RDP). Zie voor de instructies RDP [clusters beheren Hadoop in HDInsight met behulp van de portal Azure](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Zodra u verbinding met het cluster hebt gemaakt, dubbelklikt u op het pictogram **Hadoop Service beschikbaar** op het bureaublad ophalen status over welk knooppunt hoofd van de Namenode, Jobtracker, Templeton, Oozieservice, Hiveserver2 en Metastore services actief of voor services 3.0 HDI, Namenode, resourcemanager, Server geschiedenis, Templeton, Oozieservice, Metastore en Hiveserver2.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Op het screenshot is het actieve knooppunt hoofd *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Logboekbestanden van Access op het secundaire head

Voor toegang tot project zich aanmeldt op het secundaire head in het geval dat is geworden het hoofd actief knooppunt, surfen op het JobTracker UI werkt nog steeds als voor de primaire actieve knooppunt. Voor toegang tot JobTracker, moet u verbinding met het cluster Hadoop via RDP zoals beschreven in de vorige sectie. Eenmaal de knooppunten in het cluster, dubbelklikt u op het pictogram **Hadoop naam knooppunt Status** op het bureaublad en klik vervolgens op in de **Logboeken van de NameNode** naar de map van Logboeken op het secundaire head.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Grootte kop knooppunt configureren
De hoofd-knooppunten zijn toegewezen als grote virtuele machines (VMs) standaard. Deze grootte is voldoende voor het beheer van de meeste Hadoop taken op het cluster worden uitgevoerd. Er zijn echter scenario's waarvoor extra grote VMs voor de hoofd-knooppunten. Een voorbeeld is wanneer het cluster heeft voor het beheren van een groot aantal kleine Oozie taken.

Extra grote VMs kunnen worden geconfigureerd met behulp van Azure PowerShell-cmdlets of de HDInsight SDK.

Het maken en inrichten van een cluster met Azure PowerShell wordt beschreven in [HDInsight beheren met PowerShell](hdinsight-administer-use-powershell.md). De configuratie van een extra grote kop knooppunt, moet de toevoeging van de `-HeadNodeVMSize ExtraLarge` -parameter voor de `New-AzureRmHDInsightcluster` cmdlet gebruikt in deze code.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Het verhaal lijkt voor de SDK. Het maken en inrichten van een cluster met behulp van de SDK wordt beschreven in [HDInsight .NET SDK gebruiken](hdinsight-provision-clusters.md#sdk). De configuratie van een extra grote kop knooppunt, moet de toevoeging van de `HeadNodeSize = NodeVMSize.ExtraLarge` -parameter voor de `ClusterCreateParameters()` methode die in deze code wordt gebruikt.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Volgende stappen

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Verbinding maken met clusters van HDInsight met RDP](hdinsight-administer-use-management-portal.md#rdp)
- [Met behulp van HDInsight .NET SDK](hdinsight-provision-clusters.md#sdk)
