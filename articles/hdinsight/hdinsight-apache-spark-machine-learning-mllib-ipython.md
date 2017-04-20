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


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Machine learning: voorspellende analyse van levensmiddelen een inspectie op HDInsight Linux cluster-gegevens met behulp van MLlib met Apache Spark

> [AZURE.TIP] Deze zelfstudie is ook beschikbaar als een laptop Jupyter op een cluster Spark (Linux) die u in de HDInsight maakt. De ervaring van het notitieblok kunt u de Python fragmenten uit het notitieblok zelf uitvoeren. Als u de handleiding in een notitieblok, een vonk cluster maken, start een laptop Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), en voer vervolgens de laptop **Spark Machine Learning - voorspellende analyse van levensmiddelen controle gegevens met behulp van MLLib.ipynb** onder de map **Python** .


In dit artikel laat zien hoe met behulp van **MLLib**, Spark van ingebouwde machine learning bibliotheken, een eenvoudige voorspellende analyses uitvoeren op een open dataset. MLLib is een bibliotheek van core Spark een aantal hulpprogramma's die gebruikt voor taken van machine learning biedt worden, met inbegrip van de hulpprogramma's die geschikt zijn voor:

* Classificatie

* Regressie

* Clustering

* Onderwerp modellering

* Ontleding van de enkelvoudige waarde (SVD) en analyse van de belangrijkste component (Pso)

* Hypothese testen en het berekenen van statistieken van de steekproef

Dit artikel bevat een eenvoudige benadering van *classificatie* door middel van logistische regressie.

## <a name="what-are-classification-and-logistic-regression"></a>Wat zijn de indeling en de logistische regressie?

*Classificatie*, een zeer algemene machine learning taak, is het proces van ingevoerde gegevens sorteren in categorieën. Het is de taak van een classificatie-algoritme om erachter te komen hoe 'labels' op invoer die u toewijst. Zo kan volgens u van een machine learning algoritme die accepteert aandeleninformatie als invoer en wordt de voorraad wordt verdeeld in twee categorieën: bestanden die u moet verkopen en voorraden die u moet bewaren.

Logistische regressie is het algoritme dat u voor de indeling gebruikt. Spark van logistische regressie API is handig voor *binaire indeling*of invoergegevens ingedeeld in een van de twee groepen. Zie [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression)voor meer informatie over logistiek behalen.

Kortom levert het proces van logistische regressie een *logistieke functie* die kan worden gebruikt voor het voorspellen van de kans dat een input vector in één groep of de andere behoort.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Wat proberen we te bereiken in dit artikel?

U kunt vonk sommige voorspellende analyses uitvoeren op levensmiddelen controle gegevens (**Food_Inspections1.csv**) die is verkregen via de [DataPortal-Den Haag](https://data.cityofchicago.org/). Deze dataset bevat informatie over voedsel inspecties die zijn uitgevoerd in Chicago, met inbegrip van informatie over elke inrichting levensmiddelen die werd onderzocht, de overtredingen die zijn gevonden (indien aanwezig) en de resultaten van de inspectie. Het CSV-bestand is al beschikbaar in de opslag-account die is gekoppeld aan het cluster op **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

In de volgende stappen kunt u een model om te zien wat nodig is voor goedgekeurd of afgekeurd een inspectie voedsel ontwikkelen. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Beginnen met het samenstellen van een machine learning-toepassing met behulp van een MLlib

1. De [Azure Portal](https://portal.azure.com/)van de startboard, klik op de tegel voor uw cluster Spark (als u deze aan de startboard vastgemaakt). U kunt ook navigeren naar het cluster onder **Alle bladeren** > **HDInsight Clusters**.   

2. Uit het cluster Spark blade, klikt u op **Cluster Dashboard**en klik op **Jupyter laptop**. Geef desgevraagd de referenties van de beheerder voor het cluster.

    > [AZURE.NOTE] U kan ook de laptop Jupyter voor uw cluster bereiken via de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van het cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuwe laptop voor Jupyter maken] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Een nieuwe laptop voor Jupyter maken")

3. Een nieuw notitieblok wordt gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok boven en voert u een naam.

    ![Geef een naam op voor de laptop] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Geef een naam op voor de laptop")

3. Omdat u een laptop met behulp van de kernel PySpark gemaakt, hoeft u niet alle contexten expliciet maken. Vonk en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel in de code uitvoert. U kunt beginnen met het bouwen van uw machine toepassing leren door het importeren van de typen die nodig zijn voor dit scenario. Hiertoe plaatst u de cursor in de cel en drukt u op **SHIFT + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Een invoer dataframe samenstellen

Gebruiken we `sqlContext` voor het uitvoeren van transformaties van gestructureerde gegevens. De eerste taak is de voorbeeldgegevens ((**Food_Inspections1.csv**)) in een vonk SQL *dataframe*laden. 

1. Omdat de onbewerkte gegevens in een CSV-indeling, moeten we de context vonk op het trekken van elke regel van het bestand in het geheugen als niet-gestructureerde tekst; Gebruik vervolgens de Python CSV-bibliotheek parseren van elke regel afzonderlijk. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. Nu hebben we het CSV-bestand als een RDD. Laat ons een rij uit de RDD te begrijpen van het schema van de gegevens ophalen.


        inspections.take(1)


    Hier ziet u de volgende uitvoer:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. De bovenstaande uitvoer geeft ons een idee van het schema van het invoerbestand; het bestand bevat de naam van elke instelling, het type van de inrichting, het adres, de gegevens van de inspecties en de locatie, onder andere. Laten we een paar kolommen selecteren die zal nuttig zijn voor onze voorspellende analyse en de resultaten groeperen als een dataframe, we gebruiken om een tijdelijke tabel te maken.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. We hebben nu een *dataframe* `df` waarop we onze analyse kunt uitvoeren. We hebben ook een tijdelijke tabel call **CountResults**. 4 kolommen van belang in de dataframe hebt opgenomen: **id**, **naam**, **resultaten**en **overtredingen**. 
    
    We krijgen een kleine steekproef van de gegevens:

        df.show(5)

    Hier ziet u de volgende uitvoer:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Inzicht in de gegevens

1. Laten we beginnen met een idee van wat onze dataset bevat. Wat zijn bijvoorbeeld de verschillende waarden in de kolom **resultaten** ?


        df.select('results').distinct().show()

    
    Hier ziet u de volgende uitvoer:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Een snelle weergave kunt u ons de reden over de verdeling van deze resultaten. Al hebben we de gegevens in een tijdelijke tabel **CountResults**. U kunt de volgende SQL-query uitvoeren op de tabel om een beter begrip van hoe de resultaten worden verspreid.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    De `%%sql` magic gevolgd door `-o countResultsdf` zorgt ervoor dat de uitvoer van de query lokaal wordt bewaard op de server Jupyter (meestal de headnode van het cluster). De uitvoer wordt behouden als een dataframe [Pandas](http://pandas.pydata.org/) met de opgegeven naam **countResultsdf**.
    
    Hier ziet u de volgende uitvoer:
    
    ![SQL query-uitvoer] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "SQL query-uitvoer")

    Voor meer informatie over de `%%sql` magic, evenals andere magics beschikbaar met de kernel PySpark Zie [Kernels op Jupyter-laptops met vonkontsteking HDInsight clusters beschikbaar](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. U kunt ook Matplotlib, een bibliotheek die wordt gebruikt voor weergave van gegevens, samen voor het maken van een grafiek. Aangezien het waarnemingspunt moet worden gemaakt van de lokaal permanente **countResultsdf** dataframe, het codefragment moet beginnen met de `%%local` magic. Dit zorgt ervoor dat de code op de Jupyter-server lokaal wordt uitgevoerd.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Hier ziet u de volgende uitvoer:

    ![Output resultaat](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. U ziet dat er 5 verschillende resultaten die een inspectie kan hebben:
    
    * Bedrijf niet vinden 
    * Mislukt
    * Doorgeven
    * PSS met voorwaarden, en
    * Buiten het bedrijf 

    Wij ontwikkelen een model dat het resultaat van een inspectie levensmiddelen kan raden schendingen van het gegeven. Aangezien de logistische regressie is een binaire indeling, is het verstandig om onze gegevens in twee categorieën: **mislukt** en **doorgeven**. Een "doorgeven met voorwaarden" is nog steeds een stap, dus als we het model van de trein, we worden beschouwd als de twee resultaten gelijkwaardig zijn. Gegevens met de andere resultaten ("Business niet vinden", "Out of Business") worden niet nuttig is, worden deze verwijderd uit onze trainingsset. Dit moet geen probleem aangezien deze twee categorieën een zeer klein percentage van de resultaten toch vormen.

5. Laat het ons dan gerust en converteren van onze bestaande dataframe (`df`) in een nieuwe dataframe waar elke inspectie wordt voorgesteld als een combinatie van een label-overtredingen. In ons geval, een label van `0.0` duidt op een fout, een label van `1.0` vertegenwoordigt een succes en een label van `-1.0` sommige resultaten naast deze twee vertegenwoordigt. We zullen deze andere resultaten bij het berekenen van de nieuwe gegevensframe uitfilteren.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Laten we één rij uit de gegevens met een label om te zien hoe deze eruit worden opgehaald.


        labeledData.take(1)


    Hier ziet u de volgende uitvoer:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Een logistische regressiemodel van de invoer dataframe maken

Onze laatste taak is het gelabelde gegevens converteren naar een indeling die kan worden geanalyseerd door middel van logistische regressie. De invoer voor een logistische regressie-algoritme moet een reeks *vector-paren label-functie*, waar de vector"functie" is een vector van getallen die de input punt op een bepaalde manier vertegenwoordigt. We moeten dus een manier om te converteren van de kolom 'overtredingen' semi-gestructureerde en bevat een groot aantal opmerkingen in vrije tekst, naar een matrix van reële getallen die een machine gemakkelijk kan begrijpen. 

Een standaard machine learning aanpak voor de verwerking van natuurlijke taal is een "index" voor elk woord afzonderlijk toewijzen en vervolgens een vector met de computer leren algoritme, dat de relatieve frequentie van dat woord in de tekst van de waarde van elke index doorgeven. 

MLLib biedt een eenvoudige manier om deze bewerking niet uitvoeren. Eerst wij zult "basisvormen" elke tekenreeks overtredingen als u afzonderlijke woorden in elke tekenreeks en vervolgens gebruiken we een `HashingTF` elke reeks tokens converteren naar een vector functie, die vervolgens kan worden doorgegeven aan de logistische regressie-algoritme om een model samen te stellen. We zullen alle stappen in de reeks met een 'pipeline' uitvoeren.


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Het model op een afzonderlijke test dataset evalueren

Gebruiken we het model dat we eerder hebt gemaakt om te *voorspellen* wat de resultaten van de nieuwe controles worden, op basis van de overtredingen die zijn waargenomen. We dit model in de dataset **Food_Inspections1.csv**opgeleid. Laat ons een tweede dataset **Food_Inspections2.csv**, *evalueren* de sterkte van dit model op nieuwe gegevens gebruiken. Deze tweede gegevensverzameling (**Food_Inspections2.csv**) zijn al in de standaard opslag container die is gekoppeld aan het cluster.

1. Het onderstaande codefragment maakt een nieuwe dataframe, **predictionsDf** met de door het model gegenereerde voorspelling. Het codefragment maakt ook een tijdelijke tabel **die voorspellingen** op basis van de dataframe.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Hier ziet u de volgende uitvoer:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Bekijk een van de voorspellingen. In dit fragment worden uitgevoerd:

        predictionsDf.take(1)

    De voorspelling voor het eerste item in de gegevensset test wordt weergegeven.

3. De `model.transform()` methode zal de dezelfde transformatie toepassen op nieuwe gegevens met hetzelfde schema en een voorspelling van hoe de gegevens classificeren aankomen. Wij kunnen nu enkele eenvoudige statistieken om een idee van hoe nauwkeurig de voorspellingen zijn:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    De uitvoer ziet er als volgt uit:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Met behulp van logistische regressie met vonkontsteking geeft ons een nauwkeurig model van de relatie tussen overtredingen beschrijvingen in het Engels en een bepaald bedrijfsproces zou doorgeven of afbreken van een inspectie van levensmiddelen. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Maak een visuele weergave van de voorspelling

We kunnen een definitieve visualisatie om ons te helpen nu reden over de resultaten van deze test maken. 

1. Begin met het uitpakken van de verschillende voorspellingen en de resultaten van de **voorspellingen** van een tijdelijke tabel gemaakt. De volgende query's moeten u de uitvoer als *true_positive*, *false_positive*, *true_negative*en *false_negative*. In de volgende query's we uitschakelen visualisatie met `-q` en ook de uitvoer op te slaan (met behulp van `-o`) als dataframes die vervolgens kunnen worden gebruikt met de `%%local` magic. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Ten slotte het volgende fragment gebruiken voor het genereren van het waarnemingspunt **Matplotlib**gebruiken.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Hier ziet u de volgende uitvoer.
    
    ![Voorspelling uitvoer](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    In deze grafiek verwijst een "positief" resultaat naar de inspectie eten, terwijl een negatief resultaat heeft betrekking op een inspectie doorgegeven.

## <a name="shut-down-the-notebook"></a>Sluit de laptop

Nadat u klaar bent met het uitvoeren van de toepassing, moet u afsluiten van de laptop om bronnen vrij te geven. Hiertoe klikt u in het menu **bestand** op de laptop, klikt u op **sluiten en stoppen**. Deze wordt afgesloten en sluit de laptop.


## <a name="seealso"></a>Zie ook


* [Overzicht: Apache Spark op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Motoren met Machine Learning: Spark in HDInsight voor het analyseren van gebouw temperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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
