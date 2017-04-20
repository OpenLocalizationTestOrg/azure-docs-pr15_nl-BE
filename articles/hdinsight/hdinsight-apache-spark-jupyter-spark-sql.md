<properties
    pageTitle="Maak een cluster motoren op HDInsight Linux en SQL Spark van Jupyter voor interactieve analyse | Microsoft Azure"
    description="Stapsgewijze instructies voor het snel maken van een vonk Apache cluster in HDInsight en vervolgens SQL Spark van Jupyter-laptops interactieve query's worden uitgevoerd."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Aan de slag: Apache Spark cluster maakt op HDInsight Linux en interactieve query's met vonkontsteking SQL uitvoeren

Informatie over het maken van een cluster van Apache Spark in HDInsight en [Jupyter](https://jupyter.org) laptop met interactieve Spark SQL-query's uitvoeren op het cluster vonk.

   ![Aan de slag met behulp van Apache Spark in HDInsight] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Met behulp van Apache Spark in HDInsight zelfstudie aan de slag. Geïllustreerd stappen: Maak een account voor opslag; maken van een cluster. Spark SQL-instructies uitvoeren")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Vereisten

- **Azure een abonnement**. Voordat u deze zelfstudie hebt u een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **A Secure Shell (SSH) client**: Linux, Unix en OS X-systemen opgegeven een SSH client via de `ssh` opdracht. Voor Windows-systemen, wordt aangeraden [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Secure Shell (SSH) toetsen (optioneel)**: U kunt de SSH-account gebruikt om verbinding met het cluster met behulp van een wachtwoord of een openbare sleutel te beveiligen. Met een wachtwoord, wordt u snel op weg en gebruik deze optie als u wilt snel een cluster maken en uitvoeren van sommige testtaken. Met behulp van een sleutel is veiliger, maar deze aanvullende instellingen vereist. U kunt deze aanpak gebruikt bij het maken van een cluster van productie. In dit artikel gebruiken we de aanpak van het wachtwoord. Raadpleeg de volgende artikelen voor instructies over het maken en gebruiken van SSH-sleutels met HDInsight:

    -  Van een Linux-computer - [Gebruik SSH met Linux-gebaseerde HDInsight (Hadoop) van Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Vanaf een computer met Windows - [Gebruik SSH met Linux-gebaseerde HDInsight (Hadoop) van Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] In dit artikel wordt een sjabloon Azure resource manager gebruikt voor het maken van een cluster Spark met [Azure Storage Blobs als de clusteropslag](hdinsight-hadoop-use-blob-storage.md). U kunt ook een vonk cluster die [Azure Lake gegevensopslag](../data-lake-store/data-lake-store-overview.md) als een extra opslagruimte, naast Azure Storage Blobs als de standaard opslag gebruikt maken. Zie [een cluster van HDInsight met Lake gegevensarchief maken](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)voor instructies.

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Een cluster maken

In dit gedeelte vindt u een HDInsight versie 3.4 cluster (Spark versie 1.6.1) met behulp van een sjabloon Azure resource manager. Zie voor meer informatie over de versies HDInsight en hun SLA's [HDInsight component versiebeheer](hdinsight-component-versioning.md). Zie voor andere methoden voor het maken van cluster [-clusters maken HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klik op de onderstaande afbeelding om de sjabloon te openen in de Portal Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De sjabloon bevindt zich in een openbare blob container *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Voer de volgende gegevens van de bladeserver Parameters:

    - **Clusternaam**: Voer een naam voor het Hadoop-cluster dat u wilt maken.
    - **Cluster-aanmeldingsnaam en wachtwoord**: beheerder de aanmeldingsnaam van de standaard is.
    - **SSH-gebruikersnaam en wachtwoord**.
    
    Schrijf deze waarden.  U moet ze later in de zelfstudie.

    > [AZURE.NOTE] SSH wordt gebruikt om op afstand toegang tot de HDInsight-cluster met behulp van een opdrachtregel. De gebruikersnaam en het wachtwoord dat u hier gebruikt, wordt gebruikt wanneer een verbinding met het cluster via SSH. Ook moet de SSH-gebruikersnaam uniek zijn, zoals een gebruikersaccount wordt gemaakt op alle clusterknooppunten van HDInsight. De volgende zijn enkele van de accountnamen gereserveerd voor gebruik door de services op het cluster en kan niet worden gebruikt als de naam van de gebruiker SSH:
    >
    > Root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, garen, mapred, hbase, component, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.

    > Zie voor meer informatie over het gebruik van SSH in HDInsight, een van de volgende artikelen:

    > * [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3. Klik op **OK** om op te slaan van de parameters.

4. uit de **aangepaste implementatie** blade, klikt u op de vervolgkeuzelijst **resourcegroep** en klik vervolgens op **Nieuw** om een nieuwe resourcegroep maken. De resourcegroep is een container waarin de account afhankelijk opslag en andere gekoppelde bron van het cluster worden gegroepeerd.

5. op **juridische voorwaarden**en klik vervolgens op **maken**.

6. Klik op **maken**. Ziet u een nieuwe tegel met de titel Submitting implementatie voor de implementatie van de sjabloon. Duurt ongeveer 20 minuten over het maken van het cluster en de SQL-database.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Met behulp van een laptop Jupyter Spark SQL-query's uitvoeren

In dit gedeelte kunt u Jupyter laptop Spark SQL-query's uitvoeren op het cluster vonk. HDInsight Spark clusters bieden twee kernels die u met de Jupyter-laptop gebruiken kunt. Dit zijn:

* **PySpark** (voor toepassingen die zijn geschreven in Python)
* **Spark** (voor toepassingen die zijn geschreven in Scala)

In dit artikel gebruikt u de kernel PySpark. U kunt lezen in het artikel [Kernels op Jupyter-laptops met vonkontsteking HDInsight clusters beschikbaar](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) in detail over de voordelen van het gebruik van de kernel PySpark. Enkele belangrijke voordelen van het gebruik van de kernel PySpark zijn echter:

* U hoeft niet voor te stellen de contexten vonk en component. Deze automatisch voor u ingesteld.
* U kunt de magics cel, zoals `%%sql`, rechtstreeks uw SQL-component of query's worden uitgevoerd, zonder enige voorafgaande codefragmenten.
* De uitvoer voor de query's SQL of de component wordt automatisch weergegeven.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Jupyter notitieblok maken met kernel PySpark 

1. De [Azure Portal](https://portal.azure.com/)van de startboard, klik op de tegel voor uw cluster Spark (als u deze aan de startboard vastgemaakt). U kunt ook navigeren naar het cluster onder **Alle bladeren** > **HDInsight Clusters**.   

2. Uit het cluster Spark blade, klikt u op **Cluster Dashboard**en klik op **Jupyter laptop**. Geef desgevraagd de referenties van de beheerder voor het cluster.

    > [AZURE.NOTE] U kan ook de laptop Jupyter voor uw cluster bereiken via de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van het cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuwe laptop voor Jupyter maken] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Een nieuwe laptop voor Jupyter maken")

3. Een nieuw notitieblok wordt gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok boven en voert u een naam.

    ![Geef een naam op voor de laptop] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Geef een naam op voor de laptop")

4. Omdat u een laptop met behulp van de kernel PySpark gemaakt, hoeft u niet alle contexten expliciet maken. Vonk en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel in de code uitvoert. U kunt starten door het importeren van de typen die nodig zijn voor dit scenario. Hiertoe het volgende codefragment plakken in een cel en druk op **SHIFT + ENTER**.

        from pyspark.sql.types import *
        
    Telkens wanneer u een taak in Jupyter uitvoert, wordt de titel van uw web browser venster status **(bezet)** en de titel van het notitieblok weergeven. U ziet ook een gevulde cirkel naast de tekst **PySpark** in de rechterbovenhoek. Nadat de taak is voltooid, wordt dit gewijzigd in een lege cirkel.

     ![Status van een taak van de laptop Jupyter] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status van een taak van de laptop Jupyter")

4. Voorbeeldgegevens worden geladen in een tijdelijke tabel. Wanneer u een cluster Spark in HDInsight maakt, wordt het voorbeeldgegevensbestand, **hvac.csv**, aan de account gekoppeld opslag onder **\HdiSamples\HdiSamples\SensorSampleData\hvac**gekopieerd.

    In het volgende codevoorbeeld te plakken in een lege cel en druk op **SHIFT + ENTER**. In dit voorbeeld registreert de gegevens naar een tijdelijke tabel **voor installaties**genoemd.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Aangezien u een kernel PySpark gebruikt, kunt u nu rechtstreeks uitvoeren een SQL-query op de tijdelijke tabel **voor installaties** die u zojuist hebt gemaakt met behulp van de `%%sql` magic. Voor meer informatie over de `%%sql` magic, evenals andere magics beschikbaar met de kernel PySpark Zie [Kernels op Jupyter-laptops met vonkontsteking HDInsight clusters beschikbaar](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Zodra de taak is voltooid, wordt standaard de volgende uitvoer in tabelvorm weergegeven.

    ![Uitvoer van de tabel van het resultaat van de query] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Uitvoer van de tabel van het resultaat van de query")

    Ook ziet u de resultaten in ook andere visualisaties. Een vlakgrafiek voor hetzelfde resultaat zou uitzien als volgt uit.

    ![Gebied grafiek van het resultaat van de query] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Gebied grafiek van het resultaat van de query")


6. Nadat u klaar bent met het uitvoeren van de toepassing, moet u afsluiten van de laptop om bronnen vrij te geven. Hiertoe klikt u in het menu **bestand** op de laptop, klikt u op **sluiten en stoppen**. Deze wordt afgesloten en sluit de laptop.

##<a name="delete-the-cluster"></a>Het cluster te verwijderen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Zie ook


* [Overzicht: Apache Spark op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Motoren met Machine Learning: Spark in HDInsight voor het analyseren van gebouw temperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met vonkontsteking in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Toepassing inzicht telemetrie analyse van gegevens met vonkontsteking in HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Maken en uitvoeren van toepassingen

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Taken op afstand uitvoeren op een vonk cluster met behulp van Livius](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Hulpprogramma's en -extensies

* [HDInsight extra Plugin voor IntelliJ IDEE maken en indienen van Spark Scala applicatons gebruiken](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Gebruik HDInsight extra Plugin voor IntelliJ IDEE Spark toepassingen op afstand fouten opsporen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Zeppelin notitieblokken gebruiken met een cluster motoren op HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernels die beschikbaar zijn voor de laptop in een cluster voor HDInsight Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Externe-pakketten gebruiken met Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter op uw computer installeren en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
