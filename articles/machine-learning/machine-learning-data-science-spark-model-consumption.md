<properties
    pageTitle="Score van motoren gebouwd machine learning modellen | Microsoft Azure"
    description="Het score learning modellen die zijn opgeslagen in Azure Blob Storage (WASB)."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="score-spark-built-machine-learning-models"></a>Score van motoren gebouwd machine learning modellen 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

In dit onderwerp wordt beschreven hoe u de machine learning (ML) modellen die zijn gebouwd met behulp van een MLlib worden geladen en opgeslagen in Azure Blob-opslag (WASB) en hoe deze score met datasets die ook zijn opgeslagen in WASB. Ziet het vooraf verwerken van de ingevoerde gegevens, functies met behulp van de functies indexing en codering in de MLlib toolkit transformeren en het maken van een object met labels punt gegevens die kan worden gebruikt als invoer voor het scoren van de ML-modellen. De modellen die zijn gebruikt voor het scoren van bevatten lineaire regressie, logistische regressie, willekeurige Forest modellen en verloop waarbij de structuur modellen.


## <a name="prerequisites"></a>Vereisten

1. U moet een account Azure en een HDInsight Spark u behoefte aan een HDInsight 3.4 Spark 1.6 cluster voor het voltooien van deze procedure. Zie het [Overzicht van Data Science met motoren op HDInsight Azure](machine-learning-data-science-spark-overview.md) voor instructies over het aan deze vereisten voldoen. Dit onderwerp bevat ook een beschrijving van de instructies voor het uitvoeren van code van een laptop Jupyter op het cluster vonk en RDAM 2013 Taxi gegevens hier gebruikt. De **pySpark-machine-learning-data-science-spark-model-consumption.ipynb** -laptop met de codevoorbeelden in dit onderwerp vindt u in de [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Ook moet u de computer leren werken met modellen die hier worden gescoord door te werken door de [gegevens te verkennen en modellering met vonkontsteking](machine-learning-data-science-spark-data-exploration-modeling.md) onderwerp.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Setup: locaties, bibliotheken en de vooraf ingestelde Spark context

Vonk kan lezen en schrijven een Azure opslag Blob (WASB). Zodat u uw bestaande gegevens opgeslagen kunnen er worden verwerkt met behulp van motoren en de resultaten opnieuw opgeslagen in WASB.

U modellen en bestanden in WASB, moet het pad correct zijn opgegeven. De standaardcontainer die is gekoppeld aan het cluster vonk kan worden verwezen met behulp van een pad dat begint met: *' wasb / / "*. In het volgende voorbeeld wordt de locatie van de gegevens te lezen en het pad voor het model opslag map waarin de uitvoer van het model is opgeslagen. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Directory paden voor opslaglocaties instellen in WASB

Modellen worden opgeslagen in: "wasb: / / / gebruiker/remoteuser/NYCTaxi/modellen". Als dit pad niet correct is ingesteld, worden modellen niet geladen voor het scoren.

De scored resultaten zijn opgeslagen: "wasb: / / / gebruiker/remoteuser/NYCTaxi/ScoredResults". Als het pad naar de map onjuist is, worden geen resultaten in die map opgeslagen.   


>[AZURE.NOTE] De locatie van de pad kunnen worden gekopieerd en geplakt in de tijdelijke aanduidingen in deze code uit de resultaten van de laatste cel van de **machine-learning-data-science-spark-data-exploration-modeling.ipynb** -laptop.   


Dit is de code paden instellen: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**UITVOER:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Bibliotheken importeren

Spark context instellen en benodigde bibliotheken met de volgende code te importeren

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Vooraf ingestelde Spark context en PySpark magics

De kernels PySpark die worden geleverd met Jupyter-laptops hebben een vooraf ingestelde context. Zo hoeft u niet in te stellen van de vonk of component contexten expliciet voordat u begint met het werken met de toepassing die u ontwikkelt. Dit zijn standaard beschikbaar voor u. Deze contexten zijn:

- SC - voor motoren 
- sqlContext - voor component

De PySpark kernel biedt bepaalde voorgedefinieerde "magics", die zijn van speciale opdrachten die u met bellen kunt %%. Er zijn twee dergelijke opdrachten die in deze codevoorbeelden worden gebruikt.

- **%% lokale** Opgegeven dat de code in de volgende regels lokaal wordt uitgevoerd. Code moet geldige Python code.
- **%% sql -o<variable name>** 
- Hiermee voert u een query component op de sqlContext. Als de parameter -o is doorgegeven, het resultaat van de query worden behouden in de %% lokale Python context als een dataframe Pandas.
 

Voor meer informatie over de kernels voor laptops voor Jupyter en de vooraf gedefinieerde "magics" die ze bieden, Zie [Kernels die beschikbaar zijn voor laptops met HDInsight vonk Linux clusters op HDInsight Jupyter](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Consumptie van gegevens en het maken van een opgeschoonde data frame

Deze sectie bevat de code voor een reeks taken die zijn vereist voor het nemen van de gegevens die moeten worden gescoord. In een gekoppelde monster 0,1% van de taxi reis en ritbedrag-bestand (opgeslagen als een bestand .tsv) en indeling de gegevens lezen en maakt vervolgens een schone data frame.

De taxi rit en ritbedrag bestanden is verbonden op basis van de procedure in de: [het Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters met](machine-learning-data-science-process-hive-walkthrough.md) onderwerp.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd boven cel uitvoering: 46.37 seconden


## <a name="prepare-data-for-scoring-in-spark"></a>Gegevens voorbereiden voor het scoren in vonk 

In dit gedeelte ziet u hoe om te indexeren, coderen en schalen functies bestaan in gecontroleerde MLlib leren algoritmen worden gebruikt voor de indeling en regressie voor te bereiden.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Functie transformatie: index en coderen bestaan functies voor invoer in modellen voor het scoren van 

In dit gedeelte ziet u hoe indexeren bestaan met behulp van een `StringIndexer` en coderen van functies en `OneHotEncoder` in de modellen ingevoerd.

De [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) codeert een string-kolom met gegevenslabels aan een kolom van het label indices. De indices worden per label frequenties besteld. 

Een kolom met de label indices de [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) toegewezen aan een kolom van binaire vectoren, met hoogstens één een-waarde. Deze codering kan algoritmen die continue gewaardeerde functies, zoals de logistische regressie verwachten, moeten worden toegepast op voorzieningen bestaan.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd boven cel uitvoering: 5,37 seconden


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>RDD objecten maken met functie-arrays voor invoer in modellen

Deze sectie bevat de code waarmee wordt aangegeven hoe tekst bestaan als een object RDD indexeren en deze een hot coderen zodat het trainen en testen van logistische regressie van MLlib en op basis van de structuur modellen kan worden gebruikt. De geïndexeerde gegevens worden opgeslagen in de objecten [Veerkrachtig gedistribueerd Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) . Dit zijn de elementaire onttrekking in vonk. Een RDD-object vertegenwoordigt een onveranderbare gepartitioneerde verzameling elementen die kunnen worden bediend op in samenspraak met vonkontsteking.

Bevat ook code waarin wordt uitgelegd hoe u de schaal van gegevens met de `StandardScalar` die door de MLlib voor gebruik in een lineaire regressie met stochastische kleurovergang afkomst (SGD), een populaire algoritme voor een breed scala van modellen van machine learning training. De [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) wordt gebruikt voor het schalen van de functies variantie eenheid. Functie schaal, ook wel bekend als gegevens normaliseren, verzekert dat functies met veel keren waarden worden niet gezien veel wegen in de objectieve functie. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd boven cel uitvoering: 11.72 seconden


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Logistische regressiemodel met score en uitvoer naar een blob-opslaan

De code in dit gedeelte ziet u hoe een logistische regressiemodel dat is opgeslagen in Azure blob-opslag laden en gebruiken om te voorspellen of een tip wordt betaald op reis taxi, deze score met standaard indeling metrics, opslaan en weergeven van de resultaten naar een blobopslag. De scored resultaten worden opgeslagen in de RDD-objecten. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:**

Tijd boven cel uitvoering: 19.22 seconden


## <a name="score-a-linear-regression-model"></a>Score van een lineaire regressiemodel

We [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) gebruikt voor het trainen van een lineaire regressiemodel met behulp van stochastische kleurovergang afkomst (SGD) voor optimalisatie te voorspellen hoeveel tip betaald. 

De code in dit gedeelte ziet u hoe een lineaire regressiemodel laden vanuit Azure blob-opslag en sla de resultaten terug naar de blob score geschaald variabelen gebruiken.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER:**

Tijd boven cel uitvoering: 16.63 seconden


## <a name="score-classification-and-regression-random-forest-models"></a>Score van de classificatie en regressie willekeurige Forest modellen

De code in dit gedeelte ziet u hoe de indeling opgeslagen laden en regressie willekeurige Forest modellen opgeslagen in Azure blob-opslag score van hun prestaties met standaard classificatie en regressie maatregelen en sla de resultaten terug naar blob-opslag.

[Willekeurige forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) zijn ensembles van beslissingsbomen.  Ze combineren veel beslissingsstructuren om het risico te beperken. Willekeurige forests kunnen verwerken functies bestaan, worden uitgebreid tot de instelling van de multiclass-classificatie, hoeven geen functie voor schalen en kunnen niet lichtgevoeligheid vastleggen en interacties zijn uitgerust. Willekeurige bossen zijn een van de meest succesvolle machine learning modellen voor de indeling en regressie.

[Spark.mllib](http://spark.apache.org/mllib/) ondersteunt willekeurige forests voor binaire en multiclass-classificatie en voor regressie, met functies voor continue en bestaan. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:**

Tijd boven cel uitvoering: 31.07 seconden


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Score van de classificatie en regressie verloop waarbij de structuur modellen

De code in deze sectie ziet u hoe u de indeling en regressie verloop waarbij de structuur modellen van Azure blob-opslag te laden en vervolgens de resultaten opslaan naar een blob-opslag score van hun prestaties met standaard classificatie en regressie maatregelen. 

**Spark.mllib** ondersteunt de GBTs voor de binaire indeling en voor regressie, met functies voor continue en bestaan. 

[Kleurovergang prestatieverbetering bomen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) zijn ensembles van beslissingsbomen. GBTs trainen beslissingsstructuren iteratief tot een functie verlies te minimaliseren. GBTs functies bestaan kan verwerken, hoeven geen functie voor schalen en kunnen niet lichtgevoeligheid vastleggen en interacties zijn uitgerust. Ze kunnen ook worden gebruikt in een multiclass indeling.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**UITVOER:**

Tijd boven cel uitvoering: 14.6 seconden


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Objecten uit het geheugen opschonen en bestandslocaties gescoord afdrukken

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**UITVOER:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Spark modellen gebruiken via een webinterface

Spark biedt een mechanisme om te dienen op afstand batchtaken of interactieve query's via een REST-interface met een component die hier genoemd. Hier is standaard ingeschakeld op de cluster HDInsight Spark. Zie voor meer informatie over hier: [Spark verzenden taken op afstand met behulp van Livius](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

U kunt hier een taak die batch-scores op afstand stuurt een bestand dat is opgeslagen in een Azure blob en schrijft de resultaten vervolgens naar een andere blob. U doet dit door uploaden u de Python script  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) op de label van het cluster vonk. U kunt een hulpprogramma zoals **Microsoft Azure Opslagverkenner** of **AzCopy** Kopieer het script naar de blob cluster. In ons geval uploaden we het script naar ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] De toegangstoetsen die u nodig hebt kunt u vinden op de portal voor de opslag die is gekoppeld aan het cluster vonk. 


Eenmaal geüpload naar deze locatie, is dit script wordt uitgevoerd binnen het cluster Spark in een gedistribueerde context. Het laadt het model en voorspellingen op invoerbestanden op basis van het model worden uitgevoerd.  

U kunt dit script extern aanroepen door een eenvoudige HTTPS/REST aanvraag op hier.  Hier volgt een curl-opdracht om de HTTP-aanvraag voor het extern aanroepen van het script Python samen te stellen. CLUSTERLOGIN, CLUSTERPASSWORD, de CLUSTERNAAM vervangen door de juiste waarden voor uw cluster vonk.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

U kunt elke taal op het externe systeem roepen Spark via Livius door een eenvoudige HTTPS verbinding met basisverificatie.   


>[AZURE.NOTE] Zou het niet handig de aanvragen Python bibliotheek gebruiken bij het maken van deze oproep HTTP, maar het is momenteel niet geïnstalleerd in Azure functies standaard. Zodat oudere HTTP-bibliotheken gebruikt.   


Dit is de Python-code voor de HTTP-oproep:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


U kunt ook deze Python code toevoegen naar [Azure-functies](https://azure.microsoft.com/documentation/services/functions/) voor het starten van een vonk taak indiening scores een blob gebaseerd op verschillende gebeurtenissen zoals een timer, maken of bijwerken van een blob. 

U kunt desgewenst een code gratis Clientervaring gebruiken de [Azure logica Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) om op te roepen de Spark batch scoren door een HTTP-actie op de **Logica Apps Designer** definiëren en instellen van de parameters. 

- Maak vanuit Azure portal, een nieuwe logica App door te selecteren **+ Nieuw** -> **Web + Mobile** -> **App logica**. 
- Om van de **Logica Apps Designer**, voer de naam van de App logica en plannen van App-Service.
- Selecteer een actie HTTP en voert u de parameters die worden weergegeven in de volgende afbeelding:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Hoe nu verder? 

**Cross-validatie en hyperparameter verstrekkende**: Zie [Geavanceerde verkennen en modellering met vonkontsteking](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) op hoe de modellen kunnen worden getraind met behulp van de Veegpoeder cross-validatie en hyper-parameter.
