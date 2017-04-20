<properties 
    pageTitle="Gebruik van Apache Spark machine learning toepassingen op HDInsight | Microsoft Azure" 
    description="Stapsgewijze instructies voor het gebruik van laptops met Apache Spark machine learning-toepassingen te bouwen" 
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
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Machine Learning-toepassingen uitvoeren op clusters van Apache Spark op HDInsight Linux

Informatie over het maken van een toepassing met behulp van een cluster van Apache Spark in HDInsight leren machine. Dit artikel wordt beschreven hoe u met de laptop Jupyter beschikbaar met het cluster te bouwen en testen van de toepassing. De toepassing gebruikt de HVAC.csv voorbeeldgegevens die beschikbaar is op alle clusters op standaard.

**Voorwaarden:**

U hebt de volgende opties:

- Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster van Apache Spark op HDInsight Linux. Zie [clusters in Azure HDInsight Apache Spark maken](hdinsight-apache-spark-jupyter-spark-sql.md)voor instructies. 

##<a name="data"></a>De gegevens weergeven

Voordat we beginnen met het samenstellen van de toepassing, laat het ons inzicht in de structuur van de gegevens en het soort analyse doen we op de gegevens. 

In dit artikel gebruiken we het voorbeeld **HVAC.csv** bestand dat beschikbaar is in de opslag Azure-account die u aan het cluster HDInsight gekoppeld. Het bestand is binnen de opslag-account op **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Download en open het CSV-bestand als u een momentopname van de gegevens.  

![Momentopname van de gegevens voor installaties] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Momentopname van de gegevens voor installaties")

De gegevens wordt de temperatuur van het doel en de werkelijke temperatuur van een gebouw met HVAC systemen geïnstalleerd. Gaan we ervan uit de kolom **systeem** vertegenwoordigt de ID van het systeem en de kolom **SystemAge** het aantal jaren dat het HVAC-systeem bij het gebouw is.

Deze gegevens gebruiken we om te voorspellen of een gebouw wordt hotter of colder op basis van de temperatuur van het doel, een systeem-ID en system leeftijd.

##<a name="app"></a>Een machine learning-toepassing met behulp van een MLlib schrijven

In deze toepassing gebruiken we een pijpleiding Spark ML voor het uitvoeren van de indeling van een document. In de pipeline, we het document splitsen in woorden, zet de woorden in een numerieke functie vector en ten slotte een voorspellingsmodel met behulp van de functie vectoren en labels maken. Voer de volgende stappen uit om de toepassing te maken.

1. De [Azure Portal](https://portal.azure.com/)van de startboard, klik op de tegel voor uw cluster Spark (als u deze aan de startboard vastgemaakt). U kunt ook navigeren naar het cluster onder **Alle bladeren** > **HDInsight Clusters**.   

2. Uit het cluster Spark blade, klikt u op **Cluster Dashboard**en klik op **Jupyter laptop**. Geef desgevraagd de referenties van de beheerder voor het cluster.

    > [AZURE.NOTE] U kan ook de laptop Jupyter voor uw cluster bereiken via de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van het cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuwe laptop voor Jupyter maken] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Een nieuwe laptop voor Jupyter maken")

3. Een nieuw notitieblok wordt gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok boven en voert u een naam.

    ![Geef een naam op voor de laptop] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Geef een naam op voor de laptop")

3. Omdat u een laptop met behulp van de kernel PySpark gemaakt, hoeft u niet alle contexten expliciet maken. Vonk en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel in de code uitvoert. U kunt beginnen door het importeren van de typen die vereist voor dit scenario zijn. Het volgende codefragment plakken in een lege cel en druk vervolgens op **SHIFT + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. U moet nu de gegevens worden geladen (hvac.csv) het parseren en gebruiken voor het trainen van het model. Voor dit definieert u een functie die controleert of de werkelijke temperatuur van het gebouw hoger dan de temperatuur van het doel is. Als de werkelijke temperatuur hoger is, is het gebouw hot, aangeduid met de waarde **1.0**. Als de werkelijke temperatuur lager is, is het gebouw koud, aangeduid met de waarde **0,0**. 

    Het volgende codefragment plakken in een lege cel en druk op **SHIFT + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Configureer de Spark machine learning pijplijn uit drie fasen bestaat: Basisvormherkenning, hashingTF en lr. Zie voor meer informatie over wat een pijpleiding is en hoe het werkt <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pijpleiding</a>.

    Het volgende codefragment plakken in een lege cel en druk op **SHIFT + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. De pijpleiding naar het document opleiding past. Het volgende codefragment plakken in een lege cel en druk op **SHIFT + ENTER**.

        model = pipeline.fit(training)

7. Controleer of het document training checkpoint uw voortgang met de toepassing. Het volgende codefragment plakken in een lege cel en druk op **SHIFT + ENTER**.

        training.show()

    Dit geeft de uitvoer van de volgende strekking:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Ga terug en controleer of de uitvoer ten opzichte van het ruwe CSV-bestand. Bijvoorbeeld heeft de eerste rij van het CSV-bestand deze gegevens:

    ![Momentopname van de gegevens voor installaties] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Momentopname van de gegevens voor installaties")

    U ziet hoe de werkelijke temperatuur lager is dan het doel temperatuur voorgesteld dat het gebouw koud is. Daarom is de waarde voor de **label** in de eerste rij in de uitvoer van de training **0,0**, wat betekent dat het gebouw is niet warm.

8.  Voorbereiden van een set gegevens voor het uitvoeren van de opgeleide model tegen. Hiertoe zou geven we op een systeem-ID en system leeftijd (aangeduid als **SystemInfo** in de uitvoer van de training) en het model zou voorspellen of het gebouw met die ID van het systeem en het systeem leeftijd hotter zijn zou (aangeduid met 1.0) of koeler (aangeduid met 0,0).

    Het volgende codefragment plakken in een lege cel en druk op **SHIFT + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Ten slotte, voorspellingen van de testgegevens. Het volgende codefragment plakken in een lege cel en druk op **SHIFT + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Een uitvoer van de volgende strekking worden weergegeven:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    In de eerste rij in de voorspelling, ziet u dat voor een systeem voor installaties met ID 20 en 25 jaar oud systeem, het gebouw zal hot (**voorspelling = 1,0**). De eerste waarde voor DenseVector (0.49999) komt overeen met de voorspelling 0,0 en de tweede waarde (0.5001) komt overeen met de voorspelling 1.0. In de uitvoer, zelfs als de tweede waarde slechts marginaal hoger is, het model ziet u **voorspelling = 1,0**.

11. Nadat u klaar bent met het uitvoeren van de toepassing, moet u afsluiten van de laptop om bronnen vrij te geven. Hiertoe klikt u in het menu **bestand** op de laptop, klikt u op **sluiten en stoppen**. Deze wordt afgesloten en sluit de laptop.
           

##<a name="anaconda"></a>Gebruik scikit Anaconda-bibliotheek voor Machine leren leren

Clusters van Apache Spark op HDInsight opnemen Anaconda bibliotheken. Hierbij zijn ook de **scikit-informatie over** voor machine learning. De bibliotheek bevat ook verschillende gegevenssets kunt u toepassingen rechtstreeks vanaf een laptop Jupyter monster. Voor voorbeelden van het gebruik van de scikit-bibliotheek informatie, Zie [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Zie ook

* [Overzicht: Apache Spark op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

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

* [Externe-pakketten gebruiken met Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter op uw computer installeren en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
