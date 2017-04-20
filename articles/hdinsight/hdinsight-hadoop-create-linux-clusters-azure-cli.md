<properties
    pageTitle="Hadoop, HBase of Storm clusters maken op Linux in HDInsight met behulp van de CLI platforms Azure | Microsoft Azure"
    description="Informatie over het maken HDInsight Linux gebaseerde clusters met behulp van de CLI platforms Azure, Azure Resource Manager sjablonen en de Azure REST API. U kunt opgeven welk clustertype (Hadoop, HBase of Storm) of scripts gebruiken om aangepaste onderdelen te installeren..."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Linux gebaseerde clusters maken in met behulp van de CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

De CLI Azure is een opdrachtregelprogramma van cross-platform waarmee u Azure Services beheren. Het kan worden gebruikt, met Azure Resource management sjablonen, voor het maken van een HDInsight-cluster met opslag gekoppelde accounts en andere services.

Azure Resource Management sjablonen worden JSON-documenten die een beschrijving geven van een __resourcegroep__ en alle bronnen in deze (bijvoorbeeld HDInsight). Deze benadering op basis van een sjabloon kunt u de resources die u nodig hebt voor HDInsight in een sjabloon opgeven. U kunt ook wijzigingen in de groep als geheel via __implementaties__die wijzigingen toepassen op de hele groep beheren.

De stappen in dit document doorlopen van het proces van het maken van een nieuw HDInsight-cluster met behulp van de CLI Azure en een sjabloon.

> [AZURE.IMPORTANT] Het aantal knooppunten werknemer (4) de stappen in dit document gebruiken voor een cluster HDInsight. Als u van plan bent meer dan 32 knooppunten van de werknemer (tijdens het maken van het cluster of schalen van het cluster), selecteert u de grootte van een hoofd knooppunt met minimaal 8 cores en 14 GB ram.
>
> Zie voor meer informatie op het knooppunt formaten en bijbehorende kosten, [prijzen HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure CLI__. De stappen in dit document zijn laatst getest met Azure CLI versie 0.10.1.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Log in op uw abonnement Azure

Volg de stappen beschreven in [verbinding maken met een Azure-abonnement van de Azure-opdrachtregelinterface (CLI Azure)](../xplat-cli-connect.md) en verbinding maken met uw abonnement via de methode voor de __aanmelding__ .

##<a name="create-a-cluster"></a>Een cluster maken

De volgende stappen moeten worden uitgevoerd vanaf een opdrachtprompt, shell of een sessie na het installeren en configureren van de CLI Azure.

1. Gebruik de volgende opdracht om uw abonnement op Azure te verifiëren:

        azure login

    U wordt gevraagd uw naam en wachtwoord opgeven. Als er meerdere abonnementen voor Azure, `azure account set <subscriptionname>` voor het instellen van het abonnement met de Azure CLI-opdrachten.

3. Overschakelen naar de modus van Azure Resource Manager met de volgende opdracht:

        azure config mode arm

4. Een resourcegroep maken. Deze resourcegroep bevat het cluster HDInsight en opslag-account is gekoppeld.

        azure group create groupname location
        
    * __Groupname__ vervangen door een unieke naam voor de groep. 
    * __Locatie__ vervangen door het geografische gebied dat u wilt maken van de groep in. 
    
        Gebruik voor een lijst met geldige locaties, de `azure location list` opdracht en voert u een van de locaties in de kolom __naam__ .

5. Maak een account voor opslag. Deze account opslag wordt gebruikt als de standaard opslag voor het cluster HDInsight.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * __Groupname__ vervangen door de naam van de groep in de vorige stap hebt gemaakt.
     * __Locatie__ vervangen door dezelfde locatie gebruikt in de vorige stap. 
     * __Storagename__ vervangen door een unieke naam voor de account van de opslag.
     
     > [AZURE.NOTE] Voor meer informatie over de parameters die worden gebruikt in deze opdracht, gebruikt u `azure storage account create -h` help voor deze opdracht weergeven.

5. De sleutel die wordt gebruikt voor toegang tot de opslag-account worden opgehaald.

        azure storage account keys list -g groupname storagename
        
    * __Groupname__ vervangen door de naam van de resource.
    * __Storagename__ vervangen door de naam van de account van de opslag.
    
    Opslaan in de gegevens die worden geretourneerd, de waarde van de __sleutel__ voor __key1__.

6. Maak een cluster HDInsight.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * __Groupname__ vervangen door de naam van de resource.

    * __Hadoop__ vervangen door het soort cluster dat u wilt maken. Bijvoorbeeld, `Hadoop`, `HBase`, `Storm` of `Spark`.

        > [AZURE.IMPORTANT] HDInsight clusters komen in verschillende typen die met de technologie die het cluster is afgestemd corresponderen voor werkbelasting. Er is geen ondersteunde methode voor het maken van een cluster dat een combinatie van meerdere typen, zoals Storm en HBase op één cluster. 

    * __Locatie__ vervangen door dezelfde locatie gebruikt in de vorige stappen.

    * __Storagename__ vervangen door de naam van de opslag.

    * __Storagekey__ vervangen door de sleutel die is verkregen in de vorige stap. 

    * Voor de `--defaultStorageContainer` parameter, gebruiken als u dezelfde naam gebruikt voor het cluster.

    * __Admin__ en __httppassword__ vervangen door de naam en het wachtwoord die u gebruiken wilt wanneer de toegang tot het cluster via HTTPS.

    * __Sshuser__ en __sshuserpassword__ vervangen door de gebruikersnaam en het wachtwoord dat u gebruiken wilt bij de toegang tot het cluster met behulp van SSH

    Het kan enkele minuten duren voordat het cluster maken proces is voltooid. Meestal ongeveer 15.

##<a name="next-steps"></a>Volgende stappen

Nu dat u hebt gemaakt met een HDInsight-cluster met behulp van de CLI Azure, gebruikt u de volgende voor meer informatie over het werken met het cluster:

###<a name="hadoop-clusters"></a>Hadoop clusters

* [Gebruik component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase clusters

* [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Ontwikkelen van Java-toepassingen voor HBase op HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm-clusters

* [Ontwikkelen van Java topologieën voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md)
* [Python-componenten gebruiken in de Storm op HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementeren en topologieën met Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology-linux.md)
