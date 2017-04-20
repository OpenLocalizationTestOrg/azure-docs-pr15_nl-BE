<properties 
    pageTitle="Externe-pakketten gebruiken met Jupyter laptops in clusters van Apache Spark op HDInsight | Azure"
    description="Stapsgewijze instructies voor het configureren van Jupyter-laptops beschikbaar met clusters HDInsight Spark externe Spark pakketten gebruiken." 
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
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Externe-pakketten gebruiken met Jupyter laptops in clusters van Apache Spark op HDInsight Linux

>[AZURE.NOTE] Dit artikel is van toepassing op Spark 1.5.2 op HDInsight 3.3 en Spark 1.6.2 op 3.4 HDInsight. 

Informatie over het configureren van een laptop voor Jupyter in Apache Spark cluster op HDInsight (Linux) gebruik van externe, Gemeenschap bijgedragen pakketten die geen out-of-the-box opgenomen in het cluster. 

U kunt de [Maven opslagplaats](http://search.maven.org/) voor de complete lijst van pakketten die beschikbaar zijn op te zoeken. U kunt ook een lijst van beschikbare pakketten ophalen uit andere bronnen. Een complete lijst van pakketten communautaire bijdrage is bijvoorbeeld beschikbaar op [Spark pakketten](http://spark-packages.org/).

In dit artikel leert u hoe u het pakket [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) met de laptop Jupyter.

##<a name="prerequisites"></a>Vereisten

U hebt de volgende opties:

- Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster van Apache Spark op HDInsight Linux. Zie [clusters in Azure HDInsight Apache Spark maken](hdinsight-apache-spark-jupyter-spark-sql.md)voor instructies.

## <a name="use-external-packages-with-jupyter-notebooks"></a>Externe-pakketten gebruiken met Jupyter-laptops 

1. De [Azure Portal](https://portal.azure.com/)van de startboard, klik op de tegel voor uw cluster Spark (als u deze aan de startboard vastgemaakt). U kunt ook navigeren naar het cluster onder **Alle bladeren** > **HDInsight Clusters**.   

2. Van de bladeserver Spark-cluster op **Snelkoppelingen**en klik **Jupyter laptop**van de bladeserver **Cluster Dashboard** . Geef desgevraagd de referenties van de beheerder voor het cluster.

    > [AZURE.NOTE] U kan ook de laptop Jupyter voor uw cluster bereiken via de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van het cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **motoren**.

    ![Een nieuwe laptop voor Jupyter maken] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Een nieuwe laptop voor Jupyter maken")

3. Een nieuw notitieblok wordt gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok boven en voert u een naam.

    ![Geef een naam op voor de laptop] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Geef een naam op voor de laptop")

4. U gebruikt de `%%configure` magic het notitieblok voor het gebruik van een externe-pakket te configureren. Zorg ervoor dat u belt in notitieblokken die externe pakketten gebruiken, de `%%configure` magic in de eerste cel van de code. Dit zorgt ervoor dat de kernel is geconfigureerd voor het gebruik van het pakket voordat de sessie wordt gestart.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Als u voor het configureren van de kernel in de eerste cel vergeet, kunt u de `%%configure` met de `-f` de parameter, maar dat de sessie opnieuw en alle voortgang zal verloren gaan.

5. In het bovenstaande fragment `packages` een lijst met coördinaten in een centrale opslagplaats voor Maven maven verwacht. In dit fragment, `com.databricks:spark-csv_2.10:1.4.0` de maven-coördinaat voor **spark csv** -pakket. Hier ziet u hoe u de coördinaten voor een pakket samenstellen.

    een. Zoek het pakket in de opslagplaats van Maven. Voor deze zelfstudie gebruiken we [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Verzamelen van de opslagplaats, de waarden voor de **groeps-id**, **ArtifactId**en **versie**.

    ![Gebruik externe pakketten met Jupyter-laptop] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Gebruik externe pakketten met Jupyter-laptop")

    c. Samenvoegen van de drie waarden, gescheiden door een dubbele punt (**:**).

        com.databricks:spark-csv_2.10:1.4.0

6. Voer de code-cel met de `%%configure` magic. Dit stelt de onderliggende sessie hier gebruik van het pakket dat u hebt opgegeven. U kunt nu het pakket, gebruiken in de daarop volgende cellen in het notitieblok, zoals hieronder wordt weergegeven.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Vervolgens kunt u uitvoeren de fragmenten, zoals hieronder wordt weergegeven voor het bekijken van de dataframe die u in de vorige stap hebt gemaakt.

        df.show()

        df.select("Time").count()


## <a name="seealso"></a>Zie ook


* [Overzicht: Apache Spark op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Motoren met Machine Learning: Spark in HDInsight voor het analyseren van gebouw temperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met vonkontsteking in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Maken en uitvoeren van toepassingen

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Taken op afstand uitvoeren op een vonk cluster met behulp van Livius](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Hulpprogramma's en -extensies

* [HDInsight extra Plugin voor IntelliJ IDEE maken en indienen van Spark Scala applicatons gebruiken](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Gebruik HDInsight extra Plugin voor IntelliJ IDEE Spark toepassingen op afstand fouten opsporen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Zeppelin notitieblokken gebruiken met een cluster motoren op HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernels die beschikbaar zijn voor de laptop in een cluster voor HDInsight Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Jupyter op uw computer installeren en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)
