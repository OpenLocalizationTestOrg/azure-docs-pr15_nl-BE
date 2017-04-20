<properties
    pageTitle="Hadoop, HBase, Storm of Spark clusters maken op Linux in HDInsight met Azure PowerShell | Microsoft Azure"
    description="Informatie over het maken van Hadoop, HBase, Storm of Spark clusters op Linux voor HDInsight met Azure PowerShell."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Linux gebaseerde clusters maken in HDInsight met Azure PowerShell

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell is een krachtige scriptomgeving die u gebruiken kunt voor het automatiseren van de implementatie en het beheer van uw werkbelasting in Microsoft Azure. Dit document bevat informatie over het maken van een HDInsight op basis van Linux-cluster met Azure PowerShell. Het bevat tevens een voorbeeldscript.

> [AZURE.NOTE] Azure PowerShell is alleen beschikbaar op Windows-clients. Als u een client voor Linux, Unix, of Mac OS X, Zie [een CLI Azure met HDInsight op basis van Linux-cluster maken](hdinsight-hadoop-create-linux-clusters-azure-cli.md) voor meer informatie over het maken van een cluster met behulp van de CLI Azure.

## <a name="prerequisites"></a>Vereisten
Hiervoor hebt u het volgende voordat u deze procedure uitvoert:

- Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Zie voor meer informatie over het gebruik van Azure PowerShell met HDInsight de [HDInsight beheren met behulp van PowerShell](hdinsight-administer-use-powershell.md). Zie [HDInsight cmdlet referentie](https://msdn.microsoft.com/library/azure/dn858087.aspx)voor de lijst van HDInsight Windows PowerShell-cmdlets.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Clusters maken

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Als u wilt een HDInsight-cluster met Azure PowerShell maakt, moet u de volgende procedures uitvoeren:

- Maak een brongroep Azure
- Maak een account Azure opslag
- Een container Azure Blob maken
- Maak een cluster van HDInsight

De twee meest belangrijke parameters die u instellen moet voor het maken van Linux-clusters zijn die het type besturingssysteem en de details van de gebruiker SSH:

- Zorg ervoor dat u de parameter **- Waardereeksen** als **Linux**.
- Voor het gebruik van SSH voor externe sessies op de clusters, kunt u het wachtwoord van de gebruiker SSH of de openbare SSH-sleutel. Als u het wachtwoord van de gebruiker SSH en de openbare SSH-sleutel opgeeft, wordt de sleutel genegeerd. Als u de SSH-sleutel gebruiken voor externe sessies wilt, moet u een lege SSH wachtwoord wanneer daarom wordt gevraagd voor een. Zie voor meer informatie over het gebruik van SSH met HDInsight, een van de volgende artikelen:

    * [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

Het volgende script laat zien hoe u een nieuw cluster maken:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

De waarden die u voor **$clusterCredentials opgeeft** worden gebruikt voor het maken van de Hadoop-gebruikersaccount voor de cluster. Met deze account verbinding maken met het cluster.

De waarden die u voor **$sshCredentials opgeeft** worden gebruikt om de gebruiker SSH voor het cluster te maken. Gebruik deze account een SSH-sessie op het cluster starten en uitvoeren van taken.

> [AZURE.IMPORTANT] In dit script, moet u het nummer van de werknemer knooppunten in het cluster. Als u van plan bent te gebruiken meer dan 32 knooppunten van werknemer (hetzij door het cluster na het maken van schalen of maken van het cluster), moet u ook de grootte van een hoofd knooppunt opgeven met minimaal 8 cores en 14 GB RAM-geheugen.
>
> Zie voor meer informatie op het knooppunt formaten en bijbehorende kosten, [prijzen HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Het kan maken van een cluster maximaal 20 minuten duren.

In het volgende voorbeeld wordt getoond hoe een extra opslagruimte toevoegen:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Aanpassen van clusters

- Zie [clusters van HDInsight aanpassen met behulp van de Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Zie [Windows aanpassen op basis van HDInsight clusters met actie Script](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Het cluster te verwijderen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gemaakt met een HDInsight-cluster, moet u de volgende bronnen gebruiken voor meer informatie over het werken met het cluster.

### <a name="hadoop-clusters"></a>Hadoop clusters

* [Gebruik component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase clusters

* [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Ontwikkelen van Java-toepassingen voor HBase op HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-clusters

* [Ontwikkelen van Java topologieën voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md)
* [Python-componenten gebruiken in de Storm op HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementeren en topologieën met Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-clusters

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren op een vonk cluster met behulp van Livius](hdinsight-apache-spark-livy-rest-interface.md)
* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)
* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)
