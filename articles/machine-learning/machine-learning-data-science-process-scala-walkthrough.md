<properties
    pageTitle="Wetenschap van gegevens met behulp van Scala en Spark op Azure | Microsoft Azure"
    description="Het gebruik van Scala voor gecontroleerde machine learning taken met de Spark schaalbare MLlib en Spark ML pakketten op een cluster Azure HDInsight Spark."  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Wetenschap van gegevens met behulp van Scala en Spark op Azure

In dit artikel wordt beschreven hoe u Scala gebruiken voor gecontroleerde machine learning taken met de Spark schaalbare MLlib en Spark ML pakketten op een cluster Azure HDInsight Spark. Het helpt u bij de taken die het [proces van de wetenschap gegevens](http://aka.ms/datascienceprocess)vormen: gegevens via ingestie en exploratie, visualisatie, functie-engineering, modellering en model verbruik. De modellen in het artikel zijn logistiek en lineaire regressie, willekeurige forests en trees verloop versterkt (GBTs), naast de twee algemene onder toezicht staande machine learning taken:

- Regressie-probleem: voorspelling van de tip bedrag ($) voor een taxi rit
- Binaire indeling: voorspelling van tip of geen tip (1/0) voor een taxi rit

Het modelleren proces is vereist voor opleiding en evaluatie van een gegevensset test en de nauwkeurigheid van relevante parameters. In dit artikel wordt u informatie over het opslaan van deze modellen in Azure Blob-opslag en het score en evalueren van hun voorspellende prestatie. In dit artikel komen ook de meer geavanceerde onderwerpen van modellen optimaliseren met behulp van de Veegpoeder cross-validatie en hyper-parameter. De gegevens die gebruikt is een voorbeeld van de 2013 RDAM taxi rit en ritbedrag gegevensset beschikbaar op GitHub.

[Scala](http://www.scala-lang.org/), een taal die op basis van de Java virtual machine, integreert object-georiënteerde en functioneel Taalconcepten. Het is een schaalbare taal die geschikt zijn voor gedistribueerde verwerking in de cloud en op Azure Spark clusters wordt uitgevoerd.

[Spark](http://spark.apache.org/) is een open source parallel processing framework de verwerking in het geheugen ondersteunt te verhogen de prestaties van toepassingen van big data analytics. De verwerking van Spark engine is gebouwd voor snelheid en gebruiksgemak, en geavanceerde analytische. Spark van gedistribueerde berekening in het geheugen mogelijkheden maken het een goede keuze voor iteratieve algoritmen in de machine learning en grafiek berekeningen. Het pakket [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) is een uniforme set van high-level API's gebaseerd op gegevens frames die u kunnen helpen maken en afstemmen van praktische machine learning pijpleidingen. [MLlib](http://spark.apache.org/mllib/) is de Spark schaalbare machine learning bibliotheek waarvan de modelfuncties deze gedistribueerde omgeving brengt.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) is Azure gehost aangeboden Spark open source. Ook biedt ondersteuning voor laptops op het cluster Spark Jupyter Scala en interactieve query's SQL Spark transformatie, filteren en visualiseren van gegevens die zijn opgeslagen in Azure Blob-opslag kan worden uitgevoerd. De Scala codefragmenten in dit artikel die oplossingen bieden en de relevante percelen om de gegevens anders weergegeven in Jupyter-laptops die zijn geïnstalleerd op de Spark clusters worden uitgevoerd. De modellering stappen in deze onderwerpen hebben de code waarin het trainen, evalueren, opslaan en verwerken elk type model.

De procedures en de code in dit artikel zijn bedoeld voor Azure HDInsight 3.4 Spark 1.6. De code in dit artikel en in het [Notitieblok van Scala Jupyter](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) zijn echter algemeen en moet werken op een cluster Spark. De stappen van setup en beheer cluster mogelijk iets anders wat in dit artikel wordt weergegeven als u geen HDInsight Spark.

> [AZURE.NOTE] Zie [Gegevens wetenschap met motoren op HDInsight Azure](machine-learning-data-science-spark-overview.md)voor een onderwerp waarin het gebruik van Python in plaats van Scala om taken voor een end-to-end gegevens wetenschap proces te voltooien.


## <a name="prerequisites"></a>Vereisten

-   U moet een Azure-abonnement hebben. Als u nog geen één, [krijgt u een gratis proefversie van Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   U moet een Azure HDInsight 3.4 Spark 1.6 cluster op de volgende procedures uitvoeren. Als u een cluster maakt, raadpleegt u de instructies in [aan de slag: Apache Spark maken op Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Stel het cluster en de versie in het menu **Type van Cluster selecteren** .

![HDInsight type clusterconfiguratie](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Zie voor een beschrijving van de RDAM taxi rit gegevens en instructies voor het uitvoeren van code van een laptop Jupyter op het cluster Spark, de relevante secties in het [Overzicht van Data Science met motoren op HDInsight Azure](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Scala code uitvoeren vanaf een laptop Jupyter op het cluster Spark

U kunt een laptop Jupyter via de portal Azure starten. Zoek het cluster Spark op uw dashboard en klik vervolgens om de beheerpagina Voer voor uw cluster. Vervolgens klikt u op **Dashboards Cluster**en klik op **Jupyter laptop** open het notitieblok die is gekoppeld aan het cluster vonk.

![Cluster-dashboard en Jupyter-laptops](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

U ook toegang tot Jupyter-laptops op https://&lt;clusternaam&gt;.azurehdinsight.net/jupyter. *Clusternaam* vervangen door de naam van het cluster. U moet het wachtwoord voor de administrator-account voor toegang tot de Jupyter-laptops.

![Ga naar Jupyter-laptops met behulp van de naam van het cluster](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Selecteer **Scala** zien van een map met een paar voorbeelden van voorverpakte laptops die gebruikmaken van de API PySpark. Het modelleren van exploratie en score Scala.ipynb notitieblok met de codevoorbeelden voor deze suite van Spark onderwerpen is beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Het Spark-cluster, kunt u de laptop rechtstreeks van GitHub naar de laptop Jupyter server uploaden. Klik op de knop **uploaden** op de startpagina van de Jupyter. Plak de URL van de GitHub (onbewerkte inhoud) van de laptop Scala in de Verkenner en klik vervolgens op **openen**. De laptop Scala is beschikbaar op de volgende URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Setup: Spark voorinstelling en component contexten, Spark magics en Spark bibliotheken

### <a name="preset-spark-and-hive-contexts"></a>Vooraf ingestelde vonk en component-contexten

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


De Spark kernels die worden geleverd met Jupyter-laptops hebben vooraf ingestelde contexten. U hoeft niet expliciet instellen de Spark of component contexten voordat u begint te werken met de toepassing die u ontwikkelt. De vooraf ingestelde contexten zijn:

- `sc`voor SparkContext
- `sqlContext`voor HiveContext


### <a name="spark-magics"></a>Spark magics

De kernel Spark biedt enkele vooraf gedefinieerde "magics", die zijn van speciale opdrachten die u met bellen kunt `%%`. Twee van deze opdrachten worden gebruikt in de volgende voorbeelden van code.

- `%%local`Hiermee geeft u aan dat de volgende regels code lokaal worden uitgevoerd. De code moet geldige Scala-code.
- `%%sql -o <variable name>`Hiermee voert u een query component op `sqlContext`. Als de `-o` parameter wordt doorgegeven, het resultaat van de query worden behouden in de `%%local` Scala context als een frame Spark gegevens.

Voor meer informatie over de kernels voor laptops voor Jupyter en hun vooraf gedefinieerde "magics" die u aanroept met `%%` (bijvoorbeeld, `%%local`), Zie [Kernels die beschikbaar zijn voor laptops met HDInsight vonk Linux clusters op HDInsight Jupyter](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Bibliotheken importeren

Importeer de vonk, MLlib en andere bibliotheken u moet met de volgende code.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Opname van de gegevens

De eerste stap in het proces van wetenschappelijke gegevens is te nemen van de gegevens die u wilt analyseren. U overbrengen de gegevens uit externe bronnen of systemen waar het zich bevindt in de omgeving van exploratie en modellering gegevens. De gegevens die u wilt nemen is in dit artikel wordt een gekoppelde monster 0,1% van de taxi reis en tarief-bestand (opgeslagen als een bestand .tsv). De gegevens verkennen en modellering omgeving is Spark. Deze sectie bevat de code voor de volgende reeks taken voltooien:

1. Paden voor de opslag van gegevens en het model instellen.
2. Lees in de ingevoerde gegevens (opgeslagen als een bestand .tsv).
3. Definiëren van een schema voor de gegevens en de gegevens opschonen.
4. Een gegevensframe GESCHOONDE maken en deze in de cache in het geheugen.
5. De gegevens registreren als een tijdelijke tabel in SQLContext.
6. In de tabel query en de resultaten in een gegevensframe importeren.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Directory paden voor opslaglocaties instellen in Azure Blob-opslag

Spark kunt lezen en schrijven naar Azure Blob-opslag. U kunt de Spark gebruiken voor het verwerken van uw bestaande gegevens en slaan de resultaten weer in Blob-opslag.

U modellen en bestanden in de Blob-opslag, moet u het pad correct opgeven. Verwijst naar de standaardcontainer die is gekoppeld aan het cluster Spark met behulp van een pad dat met begint `wasb:///`. Andere locaties verwijzen met behulp van `wasb://`.

In het volgende voorbeeld geeft de locatie van de ingevoerde gegevens te lezen en het pad naar een Blob-opslag die is gekoppeld aan het cluster Spark waar het model moet worden opgeslagen.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Gegevens importeren, een RDD te maken en een gegevensframe volgens het schema definiëren

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 8 seconden.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>De tabel en de resultaten in een gegevensframe importeren

Vervolgens wordt de querytabel ritbedrag, passagiers en tip gegevens; een filter gegevens beschadigd en afgelegen; en meerdere rijen afdrukken.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Uitvoer:**

fare_amount|passenger_count|tip_amount|Gekantelde
-----------|---------------|----------|------
       13,5|            1.0|       2.9|   1.0
       16,0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Verkennen en visualisatie

Nadat u de gegevens naar vonk overbrengen, is de volgende stap in het proces van wetenschappelijke gegevens te krijgen van een beter begrip van de gegevens via de exploratie en visualisatie. In dit gedeelte kunt u de gegevens van de taxi controleren met behulp van SQL-query's. Importeer de resultaten in een gegevensframe uitzetten van de doelvariabelen en potentiële functies voor visuele inspectie met behulp van de functie auto-visualisatie van Jupyter.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Lokale en magic SQL gebruiken om de gegevens uitzetten

De uitvoer van een stukje code die u vanaf een laptop Jupyter uitvoert is standaard beschikbaar in het kader van de sessie die op de knooppunten van de werknemer wordt behouden. Als u wilt opslaan in de knooppunten van de werknemer voor elke berekening een reis en als de gegevens die u nodig hebt voor het berekenen van de lokaal op het serverknooppunt Jupyter (dit is het knooppunt head) beschikbaar is, kunt u de `%%local` magic het stukje code uitvoeren op de server Jupyter.

- **SQL-magic** (`%%sql`). De kernel HDInsight Spark ondersteunt gemakkelijk in line HiveQL's tegen SQLContext. De (`-o VARIABLE_NAME`) argument blijft bestaan de uitvoer van de SQL-query als een frame Pandas gegevens op de server Jupyter. Dit betekent dat het er waarschijnlijk beschikbaar zijn in de lokale modus.
- `%%local`**magic**. De `%%local` magic wordt de code uitgevoerd lokaal op de server Jupyter het hoofd knooppunt van het cluster HDInsight. Meestal gebruikt u `%%local` magic in combinatie met de `%%sql` magic met de `-o` parameter. De `-o` parameter de uitvoer van de SQL-query lokaal zou aanhouden en vervolgens `%%local` magic wordt de volgende reeks codefragment lokaal uitvoeren tegen de uitvoer van de SQL-query's die lokaal wordt bewaard.

### <a name="query-the-data-by-using-sql"></a>De gegevens met behulp van SQL-query
Deze query haalt de taxi trips door ritbedrag, aantal passagiers, en de tip.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

In de volgende code wordt de `%%local` magic wordt een lokale gegevens, sqlResults. SqlResults kunt u met behulp van matplotlib uitzetten.

> [AZURE.TIP] Lokale magic wordt meerdere keren in dit artikel gebruikt. Als uw gegevensset groot is, monsters nemen om het maken van een gegevensframe dat past in het lokale geheugen.

### <a name="plot-the-data"></a>De gegevens uitzetten

U kunt met behulp van Python code nadat het gegevensframe in de lokale context als een frame Pandas gegevens is uitzetten.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 De kernel Spark visualiseert automatisch de uitvoer van query's SQL (HiveQL) nadat u de code uitvoert. U kunt kiezen tussen verschillende soorten visualisaties:
 
- Tabel
- Cirkel
- Regel
- Gebied
- Balk

Hier is de code voor de gegevens uitzetten:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Uitvoer:**

![Tip bedrag histogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Bedrag per passagier aantal Tip](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Bedrag per bedrag ritbedrag Tip](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Functies maken en transformeren van functies en vervolgens voorbereiden voor invoer in het modelleren van functies

U hebt voor modellering op basis van een structuur van Spark ML en MLlib, doel en functies met behulp van diverse technieken, zoals binning, indexering, een hot-codering en vectorization voorbereiden. Hier vindt u de procedures in deze sectie voor:

1. Een nieuwe functie in het verkeer tijd buckets **binning** uren maken.
2. **Indexering en een hot-codering** toepassen op functies bestaan.
3. **Monster en de splitsing van de gegevensset** in opleidings- en breuken.
4. **Geef training variabelen en functies**en maak geïndexeerde of een hot gecodeerd training en testen invoer gelabelde punt veerkrachtig gedistribueerd datasets (RDDs) of gegevens-frames.
5. Automatisch **categoriseren en vectorize functies en doelen** te gebruiken als invoer voor machine learning modellen.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Een nieuwe functie maken door binning uren in tijd-buckets verkeer

Deze code ziet u het maken van een nieuwe functie met binning uur in verkeer tijd buckets en hoe u de resulterende gegevensframe in het geheugen in de cache. Waar RDDs en gegevens frames zo vaak gebruikt opslaan in cache leidt tot verbeterde uitvoeringstijden. Daarom zult u RDDs en gegevens-frames cache in verschillende stadia in de volgende procedures.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indexeren en functies bestaan van een hot-codering

De modellen en het voorspellen van de functies van de MLlib functies met het bestaan van ingevoerde gegevens worden geïndexeerd of gecodeerd vóór gebruik nodig. In deze sectie wordt beschreven hoe u indexeren of het bestaan van de functies voor invoer in de modelfuncties coderen.

U moet indexeren of coderen van uw modellen op verschillende manieren, afhankelijk van het model. Logistiek en lineaire regressie modellen vereist een hot-codering. Bijvoorbeeld kan een functie met drie categorieën worden uitgebreid in drie kolommen van de functie. Elke kolom bevat 0 of 1, afhankelijk van de categorie van een waarneming. MLlib biedt de [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) -functie voor het coderen van een hot. Dit coderingsprogramma wijst een kolom met de label indices aan een kolom van binaire vectoren met hoogstens één een-waarde. Met deze codering kunnen algoritmen die numerieke waarden functies, zoals de logistische regressie verwacht worden toegepast op voorzieningen bestaan.

Hier kunt u alleen de vier variabelen om voorbeelden zijn tekenreeksen transformeren. U kunt ook andere variabelen, zoals weekdag, vertegenwoordigd door numerieke waarden, als variabelen bestaan index.

Gebruiken voor het indexeren, `StringIndexer()`, en gebruikt voor het coderen van een hot, `OneHotEncoder()` functies van de MLlib. Hier is de code voor het indexeren en coderen functies bestaan:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 4 seconden.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Monster en de gegevensverzameling in opleidings- en breuken splitsen

Deze code maakt een willekeurige steekproef van de gegevens (in dit voorbeeld 25%). Hoewel bemonstering niet vereist voor dit voorbeeld vanwege de grootte van het gegevensbestand is, het artikel ziet u hoe u kunt een monster nemen zodat u hoe u deze gebruikt voor uw eigen problemen weet wanneer dat nodig is. Wanneer monsters groot zijn, kan dit aanzienlijke tijd besparen terwijl u modellen trainen. Vervolgens splitsen het monster een training deel (75%, in dit voorbeeld) en tests deel (in dit voorbeeld 25%) als in de indeling en het regressie model wilt gebruiken.

Een willekeurig getal (tussen 0 en 1) toevoegen aan elke rij (in een kolom "RNG") die kan worden gebruikt voor het vouwen van cross-validatie selecteren tijdens de training.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 2 seconden.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Variabele training en voorzieningen, en maak geïndexeerde of een hot gecodeerd training en input gelabelde kaders in punt RDDs of gegevens testen

Deze sectie bevat code u hoe tekst bestaan als een gegevenstype met een label punt indexeren en deze coderen ziet zodat u het kunt opleiden en testen van logistische regressie van MLlib en andere modellen voor de classificatie. Gelabelde point-objecten zijn RDDs die op een manier die door de meeste van de computer leren algoritmen in de MLlib is nodig als de ingevoerde gegevens zijn opgemaakt. Een [punt met het label](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is een lokale vector, compacte of verspreid, die is gekoppeld aan een label/antwoord.

In deze code geeft u het doel (afhankelijke) variabele en de functies die met modellen van de trein. Vervolgens maakt u geïndexeerde of een hot training en testen van punt RDDs of gegevens frames met het label input gecodeerd.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 4 seconden.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automatisch categoriseren en vectorize functies en doelen te gebruiken als invoer voor machine leren werken met modellen

Spark ML gebruiken om het doel en functies gebruiken in een boomstructuurachtige modelfuncties te categoriseren. De code heeft twee taken voltooid:

-   Maakt een doel voor de indeling voor binaire waarde 0 of 1 toewijzen aan elk gegevenspunt tussen 0 en 1 met behulp van een drempelwaarde van 0,5.
- Functies categoriseren automatisch. Als het aantal verschillende numerieke waarden voor elke functie kleiner is dan 32 is, wordt die functie gecategoriseerd.

Dit is de code voor deze twee taken.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binaire indeling model: voorspellen of een tip moet worden betaald

In dit gedeelte maakt u drie verschillende modellen van de binaire indeling om te voorspellen of een tip moet worden betaald:

- Een **logistische regressiemodel** met behulp van de vonk ML `LogisticRegression()` functie
- Een **willekeurige forest classificatie-model** met behulp van de vonk ML `RandomForestClassifier()` functie
- Een **kleurovergang prestatieverbetering classificatie boomstructuur** met behulp van de MLlib `GradientBoostedTrees()` functie

### <a name="create-a-logistic-regression-model"></a>Logistische regressie-modellen maken

Maak vervolgens een logistische regressiemodel met behulp van de vonk ML `LogisticRegression()` functie. U maakt het model bouwen code in een reeks stappen:

1. **Train het model** met één parameter ingesteld.
2. **Evalueren van het model** op een test gegevens met statistieken.
3. **Sla het model** in de Blob-opslag voor toekomstige consumptie.
4. **Het model score** tegen testgegevens.
5. **Uitzetten van de resultaten** met operationele kenmerken (ROC) curven ontvanger.

Dit is de code voor deze procedures:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Laden, score en de resultaten worden opgeslagen.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Uitvoer:**

ROC van testgegevens = 0.9827381497557599


Python op lokale Pandas gegevensframes gebruiken de ROC curve uitzetten.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Uitvoer:**

![Tip of geen tip ROC-curve](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Een willekeurige forest classificatie model maken

Maak vervolgens een model forest willekeurige indeling met behulp van de vonk ML `RandomForestClassifier()` werken en vervolgens het model op testgegevens evalueren.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Uitvoer:**

ROC van testgegevens = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>Maak een model GBT classificatie

Maak vervolgens een model GBT-indeling met behulp van MLlib `GradientBoostedTrees()` werken en vervolgens het model op testgegevens evalueren.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Uitvoer:**

Oppervlak onder de ROC curve: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Regressiemodel: tip bedrag voorspellen

In deze sectie kunt u twee typen regressie-modellen te voorspellen de tip bedrag:

- Een **lineaire regressiemodel geregulariseerd** met behulp van de vonk ML `LinearRegression()` functie. U opslaan van het model en het model van testgegevens evalueren.
- Een **regressiemodel tree - versterking van het verloop** met behulp van de vonk ML `GBTRegressor()` functie.


### <a name="create-a-regularized-linear-regression-model"></a>Een lineaire regressie geregulariseerd model maken

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 13 seconden.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Uitvoer:**

R-sqr van testgegevens = 0.5960320470835743


Vervolgens de resultaten als een gegevensframe opvragen en AutoVizWidget en matplotlib gebruiken voor het visualiseren.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

De code maakt een frame lokale gegevens uit de query-uitvoer en de gegevens worden uitgezet. De `%%local` magic wordt een lokale gegevens, `sqlResults`, waarin u kunt tekenen met matplotlib.

>[AZURE.NOTE] Deze magische vonk is meerdere keren gebruikt in dit artikel. Als de hoeveelheid gegevens te groot is, moet u een gegevens om kader te maken dat past in het lokale geheugen monster.

Percelen via Python-matplotlib maken.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Uitvoer:**

![Tip bedrag: werkelijke en voorspelde](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Maak een regressiemodel GBT

Een regressiemodel GBT maken met behulp van de vonk ML `GBTRegressor()` werken en vervolgens het model op testgegevens evalueren.

[Verloop versterkt bomen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) zijn ensembles van beslissingsbomen. GBTs trainen beslissingsstructuren iteratief tot een functie verlies te minimaliseren. U kunt GBTs gebruiken voor regressie en classificatie. Ze bestaan functies kunnen verwerken, hoeven geen functie voor schalen en nonlinearities en functie interacties kunnen vastleggen. Ook kunt u ze in een multiclass indeling.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Uitvoer:**

Testen R-sqr is: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Hulpprogramma's van geavanceerde modellering voor optimalisatie

In deze sectie kunt u machine learning-hulpprogramma's waarmee ontwikkelaars vaak voor optimalisatie van model gebruiken. U kunt met name machine learning modellen drie verschillende manieren optimaliseren met behulp van de Veegpoeder parameter en cross-validatie:

-   De gegevens opsplitsen in trein-en validatie, het model met behulp van de Veegpoeder hyper-parameter op een opleiding en het evalueren van een verzameling validatie (lineaire regressie)
-   Het model optimaliseren met behulp van cross-validatie en hyper-parameter verstrekkende met behulp van Spark ML CrossValidator functie (binaire indeling)
-   Het model optimaliseren met behulp van aangepaste code voor cross-validatie en parameter-de Veegpoeder gebruik van een machine learning functie en parameters instellen (lineaire regressie)


**Cross-validatie** is een techniek die hoe goed een model getraind voor een bekende set gegevens wordt generalize te voorspellen wat de functies van gegevenssets waarvoor deze is niet getraind. Het idee achter deze techniek is dat een model van een set gegevens bekend is opgeleid, en vervolgens de nauwkeurigheid van de voorspellingen een onafhankelijke set van gegevens wordt getest. Een algemene implementatie is een set gegevens onderverdelen in *k*-vouwen en vervolgens het model in een round robin op alle op één na het vouwen van de trein.

**Optimalisatie van de Hyper-parameter** is het probleem van het kiezen van een set van hyper-parameters voor een algoritme leren meestal met het doel van een meting van de prestaties van de algoritme op een onafhankelijke set van gegevens optimaliseren. Een hyper-parameter is een waarde die u buiten de training model procedure moet opgeven. Veronderstellingen over hyper-parameterwaarden kunnen invloed hebben op de flexibiliteit en nauwkeurigheid van het model. Beslissingsstructuren hebben bijvoorbeeld hyper-parameters, zoals de gewenste diepte en het aantal bladeren in de structuur. U moet een misclassification boete term voor een support vector machine (SVM) instellen.

Een veelgebruikte methode voor het uitvoeren van de optimalisatie van de hyper-parameter is een raster zoeken, ook wel genoemd een **parameter sweep**gebruiken. In een raster-naar wordt een volledig gezocht door de waarden van een opgegeven subset van de hyper-parameter ruimte voor een algoritme leren. Cross-validatie kunt opgeven voor het sorteren van de optimale resultaten geproduceerd door het raster zoekalgoritme performance metric. Als u cross-validatie hyper-parameter sweeping gebruikt, kunt u problemen zoals een model om trainingsgegevens overfitting beperken. Op deze manier het model behoudt de capaciteit toe te passen in de algemene set met gegevens waaruit de trainingsgegevens is uitgepakt.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Een lineaire regressiemodel met de Veegpoeder hyper-parameter optimaliseren

Vervolgens gegevens in de trein en validatie sets, gebruikt hyper-parameter op een trainingsset te optimaliseren, het model en evalueren van een verzameling validatie (lineaire regressie) verstrekkende splitsen.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Uitvoer:**

Testen R-sqr is: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Het model van de binaire indeling optimaliseren met behulp van de Veegpoeder cross-validatie en hyper-parameter

In deze sectie wordt beschreven hoe u een binaire indeling model optimaliseren met behulp van de Veegpoeder cross-validatie en hyper-parameter. Deze maakt gebruik van de vonk ML `CrossValidator` functie.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 33 seconden.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Het lineaire regressiemodel optimaliseren met behulp van aangepaste code voor cross-validatie en parameter-de Veegpoeder

Vervolgens het model met behulp van aangepaste code optimaliseren en het beste Modelparameters te identificeren met behulp van het criterium van de hoogste nauwkeurigheid. Maak het uiteindelijke model, het model van testgegevens evalueren en het model opslaan in Blob-opslag. Tot slot laadt het model testgegevens score en nauwkeurigheid te evalueren.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 61 seconden.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Spark gebouwd machine learning modellen met Scala automatisch verbruiken

Zie voor een overzicht van de onderwerpen die u stapsgewijs door de taken waaruit het proces gegevens wetenschap in Azure, [Team gegevens wetenschap proces](http://aka.ms/datascienceprocess).

[Team gegevens wetenschap proces-scenario's](data-science-process-walkthroughs.md) worden andere end-to-end-scenario's die aantonen de stappen in het proces van het Team gegevens wetenschap voor specifieke scenario's dat beschreven. De scenario's illustreren ook gereedschappen Wolk en op ruimten en diensten te combineren in een workflow of een pijpleiding een intelligente toepassing maken.

[Score Spark gebouwd machine learning modellen](machine-learning-data-science-spark-model-consumption.md) laat zien hoe u Scala code gebruiken om automatisch te laden en nieuwe gegevensgroepen score met modellen leren machine ingebouwde motoren en opgeslagen in Azure Blob-opslag. U kunt de instructies volgen en gewoon de Python-code vervangen door code in dit artikel voor geautomatiseerde consumptie Scala.
