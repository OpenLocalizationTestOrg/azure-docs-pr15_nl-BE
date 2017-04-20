<properties
    pageTitle="Hadoop, HBase, Storm of Spark clusters maken op Linux in HDInsight met behulp van de portal | Microsoft Azure"
    description="Informatie over het maken van Hadoop, HBase, Storm of Spark clusters op Linux voor HDInsight met behulp van een webbrowser en de Azure preview portal."
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


#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Linux gebaseerde clusters in HDInsight met de Azure portal maken

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

De Azure portal is een beheer-hulpprogramma voor services en bronnen die worden gehost in de cloud met Microsoft Azure. In dit artikel leert u het maken van HDInsight met Linux gebaseerde clusters met behulp van de portal.

## <a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Een moderne webbrowser__. De Azure portal gebruikt HTML5 en Javascript en mogelijk niet goed werken in oudere webbrowsers.

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-clusters"></a>Clusters maken

De portal Azure beschrijft de meeste clustereigenschappen. Azure Resource Manager sjabloon kunt u een groot aantal details verbergen. Zie voor meer informatie [in HDInsight met behulp van bronbeheer Azure sjablonen maken Linux-gebaseerde Hadoop](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

1. Log in om de [Azure portal](https://portal.azure.com).

2. Klik op **Nieuw**, klik op **Analytics gegevens**en klik op **HDInsight**.

    ![Een nieuw cluster in Azure portal maken] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Een nieuw cluster in Azure portal maken")
3. Voer de **Naam van het Cluster**: deze naam moet uniek zijn.
4. Klik op **cluster Type selecteren**en selecteer:

    - **Cluster-Type**: als u niet wat weet te kiezen, selecteert u **Hadoop**. Het is de meest populaire clustertype.

        > [AZURE.IMPORTANT] HDInsight clusters komen in verschillende typen die met de technologie die het cluster is afgestemd corresponderen voor werkbelasting. Er is geen ondersteunde methode voor het maken van een cluster dat een combinatie van meerdere typen, zoals Storm en HBase op één cluster. 

    - **Besturingssysteem**: **Linux**selecteren.
    - **Versie**: de standaardversie gebruiken als u niet wat weet te kiezen. Zie voor meer informatie [HDInsight cluster versies](hdinsight-component-versioning.md).
    - **Cluster-Tier**: Azure HDInsight biedt de big data wolk aanbiedingen in twee categorieën: laag Standard en Premium-laag. Zie [lagen Cluster](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers)voor meer informatie.
    
    ![HDInsight premium tier configuratie](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)

4. Klik op **abonnement** schakelt de Azure abonnement die wordt gebruikt voor het cluster.

5. Klik op **Resourcegroep** om een bestaande resourcegroep selecteren of klik op **Nieuw** om een nieuwe resourcegroep maken

    > [AZURE.NOTE] Deze vermelding wordt standaard een van de bestaande resourcegroepen, als deze beschikbaar zijn.

6. Klik op **referenties** en voer een wachtwoord voor de admin-gebruiker. U moet ook een **SSH-gebruikersnaam** en een **wachtwoord** of een **Openbare sleutel**die wordt gebruikt voor het verifiëren van de gebruiker SSH invoeren. Met een openbare sleutel is de aanbevolen werkwijze. Klik op **selecteren** onderaan de configuratie van de referenties op te slaan.

    ![Cluster referenties] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.3.png "Cluster referenties")

    Zie voor meer informatie over het gebruik van SSH in HDInsight, een van de volgende artikelen:

    * [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)


7. Klik op **Gegevensbron** als u een bestaande gegevensbron voor het cluster of een nieuwe maken.

    ![Gegevensbron blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.4.png "Configuratie van de gegevensbron opgeven")

    Momenteel kunt u een Account Azure opslag als gegevensbron voor een HDInsight-cluster. Gebruik de volgende te begrijpen van de posten op de **Gegevensbron** blade.

    - **Selectie-methode**: deze worden ingesteld **van alle abonnementen** bladeren van rekeningen van de opslag van uw abonnementen inschakelen. Als u wilt opgeven voor de **Opslag** en de **Toegangstoets** van een bestaande account voor opslag, moet u deze instellen op de **Toegangstoets** .

    - **Opslag account selecteren / nieuwe**: klik op **opslag account selecteren** om te bladeren en selecteer een bestaande opslag-account die u wilt koppelen aan het cluster. Of klik op **Nieuw** om een nieuwe opslag-account te maken. Gebruik het veld om het invoeren van de naam van de account van de opslag. Een groen vinkje wordt weergegeven als de naam beschikbaar is.

    - **Kies standaardcontainer**: Gebruik deze optie voert u de naam van de standaardcontainer te gebruiken voor het cluster. Terwijl u hier een naam invoert, wordt aangeraden met dezelfde naam als het cluster, zodat u gemakkelijk herkennen kunt dat de container wordt gebruikt voor deze specifieke clusterhost.

    - **Locatie**: de geografische regio die de account voor de opslag, of wordt gemaakt in.

        > [AZURE.IMPORTANT] De locatie voor de standaard-gegevensbron te selecteren, wordt de locatie van het cluster HDInsight ook ingesteld. De cluster- en gegevensbron moet zich bevinden in hetzelfde gebied.
        
    - **Cluster AAD identiteit**: te configureren, maakt u het cluster toegankelijk is voor de Azure gegevens Lake winkels op basis van de configuratie van AAD.

    Klik op **selecteren** om op te slaan van de configuratie van de gegevensbron.

8. Klik op **Knooppunt prijzen lagen** informatie weergegeven over de knooppunten die voor dit cluster wordt gemaakt. Stel het aantal knooppunten werknemer die u nodig hebt voor het cluster. De geschatte kosten van het cluster wordt weergegeven in het blad.

    ![Knooppunt prijzen lagen blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.5.png "Geef het aantal knooppunten in het servercluster")
    
    > [AZURE.IMPORTANT] Als u van plan op meer dan 32 werknemer knooppunten te maken van het cluster of bent door de schaal van het cluster nadat het is gemaakt, selecteert u de grootte van een hoofd knooppunt met minimaal 8 cores en 14GB ram.
    >
    > Zie voor meer informatie op het knooppunt formaten en bijbehorende kosten, [prijzen HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

    Klik op **selecteren** om op te slaan van de prijzen knooppuntconfiguratie.

9. Klik op **Optionele configuratie** selecteert u de versie van het cluster, evenals andere optionele instellingen configureren zoals lid worden van een **Virtueel netwerk**, een **Externe Metastore** instellen voor het opslaan van gegevens voor component- en Oozie, scriptacties gebruiken voor het aanpassen van een cluster als u wilt aangepaste onderdelen installeren of extra opslagruimte accounts gebruiken voor het cluster.

    * **Virtueel netwerk**: Selecteer een Azure virtual network en het subnet als plaats voor het cluster in een virtueel netwerk.  

        ![Virtueel netwerk blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.6.png "Geef virtuele netwerkdetails")

        Zie voor meer informatie over het gebruik van HDInsight met een virtueel netwerk, met inbegrip van specifieke configuratievereisten voor het virtuele netwerk [mogelijkheden HDInsight uitbreiden met behulp van een virtueel netwerk Azure](hdinsight-extend-hadoop-virtual-network.md).

    * Klik op de **Metastores van externe** SQL-database die u gebruiken wilt voor het opslaan van de component en Oozie metagegevens die zijn gekoppeld aan het cluster opgeven.
    
        > [AZURE.NOTE] Metastore configuratie is niet beschikbaar voor HBase clustertypen.

        ![Aangepaste metastores blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.7.png "Externe metastores opgeven")

        Voor metagegevens van **een bestaande SQL DB voor component gebruiken** , klikt u op **Ja**en selecteert u een SQL-database geeft u de gebruikersnaam en het wachtwoord voor de database. Herhaal deze stappen als u **Gebruik een bestaande SQL DB voor metagegevens van Oozie**. Klik op **selecteren** totdat u terug in de **Optionele configuratie** blade bent.

        >[AZURE.NOTE] De Azure SQL-database die wordt gebruikt voor de metastore moet toestaan verbinding met andere Azure diensten, met inbegrip van Azure HDInsight. Klik op de naam van de server op het dashboard Azure SQL-database, aan de rechterkant. Dit is de server waarop een exemplaar van de SQL-database wordt uitgevoerd. Zodra u op de serverweergave, klikt u op **configureren**en vervolgens **Azure Services**, klikt u op **Ja**en klik op **Opslaan**.

        &nbsp;

        > [AZURE.IMPORTANT] Bij het maken van een metastore, gebruik niet de naam van een database met streepjes of afbreekstreepjes, omdat dit leiden het ontwikkelingsproces van een cluster tot kan worden uitgevoerd.

    * **Scriptacties** als u een aangepast script gebruiken voor het aanpassen van een cluster als het cluster wordt gemaakt. Zie voor meer informatie over scriptacties [aanpassen HDInsight clusters met actie Script](hdinsight-hadoop-customize-cluster-linux.md). Op het blad scriptacties bieden u de details in de schermopname.

        ![Script actie blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.8.png "De actie script opgeven")

    * Klik op **Gekoppelde Accounts voor opslag** extra opslagruimte accounts koppelen aan het cluster opgeven. In het blad **Azure opslag sleutels** **een sleutel opslag toevoegen**, klikt u op en selecteert u een bestaande account voor opslag of maak een nieuwe account.

        ![Extra opslagruimte blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.9.png "Geef extra opslagruimte accounts")

        Nadat u een cluster hebt gemaakt, kunt u extra opslagruimte accounts toevoegen.  Zie [HDInsight aanpassen Linux gebaseerde clusters met actie Script](hdinsight-hadoop-customize-cluster-linux.md).

        Klik op **selecteren** totdat u terug in het blad **HDInsight nieuwe cluster bent** .
        
        Naast het Blob storage-account, kunt u ook Azure gegevens Lake winkels koppelen. De configuratie kan worden door configureren AAD uit de gegevensbron waarop u de standaardaccount voor opslag en standaardcontainer geconfigureerd.

10. Op het **Nieuwe Cluster van HDInsight** -blade zorgen **vastmaken aan de Startboard** is geselecteerd en klik vervolgens op **maken**. Dit maakt het cluster en een tegel voor het toevoegen aan de Startboard van uw portal Azure. Het pictogram geeft aan dat het cluster wordt ingericht en het pictogram HDInsight verandert als inrichten is voltooid.

  	| Tijdens het inrichten | Inrichten is voltooid |
  	| ------------------ | --------------------- |
  	| ![Indicator op de startboard wordt ingericht](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioning.png) | ![Ingerichte cluster naast elkaar](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioned.png) |

    > [AZURE.NOTE] Het duurt enige tijd voor het cluster moet worden gemaakt, meestal ongeveer 15 minuten. De tegel op de Startboard of de vermelding van de **kennisgevingen** aan de linkerkant van de pagina gebruiken om te controleren op het inrichtingsproces.

11. Zodra het proces is voltooid, klikt u op de tegel voor het cluster uit de Startboard voor het starten van de cluster-blade. De cluster-bladeserver biedt essentiële informatie over de cluster, zoals de naam, de groep waartoe het behoort, de locatie, het besturingssysteem, de URL voor het cluster dashboard, enz.

    ![Cluster-blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.Cluster.Blade.png "Eigenschappen van cluster")

    Gebruik de volgende te begrijpen van de pictogrammen aan de bovenkant van deze blade, en in de sectie **Essentials** :

    * **Instellingen** en **Alle instellingen**: het blad van de **Instellingen** voor het cluster waarmee u toegang tot de gedetailleerde configuratiegegevens van het cluster weergegeven.

    * **Dashboard** **Dashboard Cluster**en **URL**: dit zijn alle manieren om toegang te krijgen tot het cluster-dashboard is een webportaal uit te voeren taken op het cluster.

    * **Secure Shell**: informatie die nodig is voor toegang tot het cluster via SSH.

    * **Verwijderen**: Hiermee verwijdert u het cluster HDInsight.

    * **QuickStart** (![pictogram wolk en thunderbolt = quickstart](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)): hierin wordt informatie weergegeven die u aan de slag met HDInsight.

    * **Gebruikers** (![pictogram gebruikers](./media/hdinsight-hadoop-create-linux-cluster-portal/users.png)): Hiermee kunt u machtigingen instellen voor _beheer van portal_ van dit cluster voor andere gebruikers op uw abonnement Azure.

        > [AZURE.IMPORTANT] Dit _alleen_ van invloed op toegang en machtigingen voor dit cluster in Azure portal en heeft geen invloed op wie kan verbinding maken met of taken aan het cluster HDInsight.

    * **Tags** (![pictogram label](./media/hdinsight-hadoop-create-linux-cluster-portal/tags.png)): Tags kunt u instellen van sleutel/waarde-paren voor het definiëren van een aangepaste taxonomie van de cloud-services. U kan bijvoorbeeld een sleutel met de naam __project__maken en een gemeenschappelijke waarde vervolgens gebruiken voor alle services die zijn gekoppeld aan een specifiek project.

##<a name="customize-clusters"></a>Aanpassen van clusters

- Zie [clusters van HDInsight aanpassen met behulp van de Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
- Zie [HDInsight aanpassen Linux gebaseerde clusters met actie Script](hdinsight-hadoop-customize-cluster-linux.md).

##<a name="delete-the-cluster"></a>Het cluster te verwijderen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Volgende stappen

Nu dat u een HDInsight-cluster hebt gemaakt, gebruikt u de volgende voor meer informatie over het werken met het cluster:

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

###<a name="spark-clusters"></a>Spark-clusters

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren op een vonk cluster met behulp van Livius](hdinsight-apache-spark-livy-rest-interface.md)
* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)
* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)
