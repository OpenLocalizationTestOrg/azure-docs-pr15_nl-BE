<properties 
    pageTitle="Aangepaste bibliotheken met een cluster HDInsight Spark gebruiken voor het analyseren van Logboeken website | Microsoft Azure" 
    description="Gebruik aangepaste bibliotheken met een cluster HDInsight Spark website logs analyseren" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Met behulp van een aangepaste bibliotheek met Apache Spark cluster op HDInsight Linux website-logboekbestanden analyseren

Dit notitieblok laat zien hoe met behulp van een aangepaste bibliotheek met motoren op HDInsight logboekgegevens analyseren. De aangepaste bibliotheek we gebruiken is een Python bibliotheek **iislogparser.py**genoemd.

> [AZURE.TIP] Deze zelfstudie is ook beschikbaar als een laptop Jupyter op een cluster Spark (Linux) die u in de HDInsight maakt. De ervaring van het notitieblok kunt u de Python fragmenten uit het notitieblok zelf uitvoeren. Als u de handleiding in een notitieblok, een vonk cluster maken, start een laptop Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), en voer vervolgens de laptop **analyseren met vonkontsteking, met een aangepaste library.ipynb zich** in de map **PySpark** .

**Voorwaarden:**

U hebt de volgende opties:

- Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster van Apache Spark op HDInsight Linux. Zie [clusters in Azure HDInsight Apache Spark maken](hdinsight-apache-spark-jupyter-spark-sql.md)voor instructies.

## <a name="save-raw-data-as-an-rdd"></a>Onbewerkte gegevens opslaan als een RDD

In deze sectie gebruiken we de [Jupyter](https://jupyter.org) laptop die is gekoppeld aan een cluster van Apache Spark in HDInsight uit te voeren taken die uw onbewerkte voorbeeldgegevens en deze opslaan in een component-tabel. De voorbeeldgegevens is een CSV-bestand (hvac.csv) beschikbaar op alle clusters op standaard.

Zodra uw gegevens als een component-tabel is opgeslagen, in de volgende sectie verbinding we met de component-tabel met BI-hulpmiddelen zoals Power BI en Tableau.

1. De [Azure Portal](https://portal.azure.com/)van de startboard, klik op de tegel voor uw cluster Spark (als u deze aan de startboard vastgemaakt). U kunt ook navigeren naar het cluster onder **Alle bladeren** > **HDInsight Clusters**.   

2. Uit het cluster Spark blade, klikt u op **Cluster Dashboard**en klik op **Jupyter laptop**. Geef desgevraagd de referenties van de beheerder voor het cluster.

    > [AZURE.NOTE] U kan ook de laptop Jupyter voor uw cluster bereiken via de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van het cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuwe laptop voor Jupyter maken] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Een nieuwe laptop voor Jupyter maken")

3. Een nieuw notitieblok wordt gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok boven en voert u een naam.

    ![Geef een naam op voor de laptop] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Geef een naam op voor de laptop")

4. Omdat u een laptop met behulp van de kernel PySpark gemaakt, hoeft u niet alle contexten expliciet maken. Vonk en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel in de code uitvoert. U kunt beginnen door het importeren van de typen die vereist voor dit scenario zijn. Het volgende codefragment plakken in een lege cel en druk vervolgens op **SHIFT + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Maak een RDD met behulp van het logboek voorbeeldgegevens al beschikbaar op het cluster. U kunt toegang krijgen tot de gegevens in de standaard opslag-account die is gekoppeld aan het cluster op **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Ophalen van een monster logboek instellen om te controleren de vorige stap is voltooid.

        logs.take(5)

    Een uitvoer van de volgende strekking worden weergegeven:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Met behulp van een library met custom Python logboekgegevens analyseren

7. De eerste paar regels bevat de koptekstgegevens in de uitvoer, en elke resterende regel overeenkomt met het schema dat wordt beschreven in die header. Bij het parseren van deze logboeken kan ingewikkeld zijn. We gebruiken dus een aangepaste Python library (**iislogparser.py**) die bij het parseren van deze logboeken veel eenvoudiger maakt. Deze bibliotheek is standaard opgenomen in uw cluster motoren op HDInsight op **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Deze bibliotheek is echter niet in de `PYTHONPATH` zodat we kunnen niet worden gebruikt met behulp van een instructie importeren als `import iislogparser`. Gebruik deze bibliotheek, we moeten verspreiden onder alle knooppunten van de werknemer. In het volgende fragment wordt uitgevoerd.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`biedt een functie `parse_log_line` die retourneert `None` als een regel een rij met kolomkoppen is en geeft als een exemplaar van resultaat de `LogLine` klasse als er een regel wordt aangetroffen. Gebruik de `LogLine` klasse alleen de regels van het logboek van de RDD uitpakken:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Ophalen enkele uitgepakte log regels te controleren dat de stap is voltooid.

        logLines.take(2)

    De uitvoer is vergelijkbaar met het volgende:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. De `LogLine` klasse, heeft op zijn beurt enkele nuttige methoden zoals `is_error()`, die of een logboekvermelding een foutcode heeft geretourneerd. Gebruik deze optie voor het berekenen van het aantal fouten in de uitgepakte log regels en meld u vervolgens alle fouten naar een ander bestand.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Hier ziet u de volgende uitvoer:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. U kunt ook **Matplotlib** gebruiken om een visualisatie van de gegevens samen te stellen. Als u isoleren van de oorzaak van de aanvragen die lange tijd worden uitgevoerd wilt, wilt u mogelijk de bestanden zoeken die de meeste tijd gemiddeld dienst te nemen.
Het onderstaande stukje haalt de bovenste 25 bronnen die de meeste tijd aan een aanvraag heeft genomen.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Hier ziet u de volgende uitvoer:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. U kunt ook presenteert deze informatie in de vorm van een grafiek. Als eerste stap voor het maken van een waarnemingspunt laat ons maakt eerst een tijdelijke tabel **AverageTime**. De tabel de logboeken op tijd om te zien of op een willekeurig moment een ongebruikelijke latentie pieken zijn gegroepeerd.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Vervolgens kunt u de volgende SQL-query als u alle records in de tabel **AverageTime** uitvoeren.

        %%sql -o averagetime
        SELECT * FROM AverageTime

    De `%%sql` magic gevolgd door `-o averagetime` zorgt ervoor dat de uitvoer van de query lokaal wordt bewaard op de server Jupyter (meestal de headnode van het cluster). De uitvoer wordt behouden als een dataframe [Pandas](http://pandas.pydata.org/) met de opgegeven naam **averagetime**.

    Hier ziet u de volgende uitvoer:

    ![SQL query-uitvoer] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "SQL query-uitvoer")

    Voor meer informatie over de `%%sql` magic, evenals andere magics beschikbaar met de kernel PySpark Zie [Kernels op Jupyter-laptops met vonkontsteking HDInsight clusters beschikbaar](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. U kunt nu Matplotlib, een bibliotheek die wordt gebruikt om de visualisatie van gegevens, samen te gebruiken voor het maken van een grafiek. Aangezien het waarnemingspunt moet worden gemaakt van de lokaal permanente **averagetime** dataframe, het codefragment moet beginnen met de `%%local` magic. Dit zorgt ervoor dat de code op de Jupyter-server lokaal wordt uitgevoerd.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Hier ziet u de volgende uitvoer:

    ![Uitvoer Matplotlib] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Uitvoer Matplotlib")

16. Nadat u klaar bent met het uitvoeren van de toepassing, moet u afsluiten van de laptop om bronnen vrij te geven. Hiertoe klikt u in het menu **bestand** op de laptop, klikt u op **sluiten en stoppen**. Deze wordt afgesloten en sluit de laptop.
    

## <a name="seealso"></a>Zie ook


* [Overzicht: Apache Spark op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Motoren met Machine Learning: Spark in HDInsight voor het analyseren van gebouw temperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

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
