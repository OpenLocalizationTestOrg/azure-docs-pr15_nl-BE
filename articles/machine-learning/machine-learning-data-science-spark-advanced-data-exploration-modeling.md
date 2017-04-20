<properties
    pageTitle="Geavanceerde verkennen en modellering met vonkontsteking | Microsoft Azure"
    description="Gebruik HDInsight Spark verkennen en binaire indeling en regressie-modellen met cross-validatie en hyperparameter optimalisatie van de trein."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Geavanceerde verkennen en modellering met vonkontsteking 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

In dit scenario gebruikt HDInsight Spark verkennen en binaire indeling en regressie-modellen met cross-validatie van de trein- en optimalisatie van de hyperparameter van een steekproef van de RDAM taxi rit ritbedrag 2013 dataset. Het leidt u door de stappen van de [Procedure voor wetenschappelijke gegevens](http://aka.ms/datascienceprocess), end-to-end met behulp van een HDInsight Spark cluster voor verwerking en Azure BLOB's voor het opslaan van de gegevens en de modellen. Het proces wordt verkend en visualiseert gegevens uit een opslag Azure Blob binnengebracht en vervolgens zorgt ervoor dat de gegevens om voorspellende modellen te bouwen. Python is gebruikt om de oplossing en de relevante waarnemingspunten weergeven. Deze modellen zijn binaire indeling en regressie modelleertaken uitvoeren met de toolkit Spark MLlib bouwen. 

- De **binaire indeling** is te voorspellen of een tip wordt betaald voor de reis. 
- De **regressie** is te voorspellen, het bedrag van de op basis van andere functies tip tip. 

De stappen modellen bevatten ook code getoond hoe te trainen, evalueren en elk type model opslaan. Het onderwerp worden enkele van de grond dezelfde als onderwerp [verkennen en modellering met vonkontsteking](machine-learning-data-science-spark-data-exploration-modeling.md) . Maar er is meer 'Geavanceerde' in die wordt gebruikt tussen validatie in combinatie met verstrekkende om te trainen optimaal nauwkeurige classificatie en regressie-modellen hyperparameter. 

**Cross-validatie (AVK)** is een techniek die hoe goed opgeleid op een bekende reeks gegevens model voor het voorspellen van de functies van datasets waarop deze is niet getraind generaliseert verdwijnt. Het idee achter deze techniek is dat een model is getraind in een dataset van bekende gegevens op en vervolgens de nauwkeurigheid van de voorspellingen een onafhankelijke dataset is getest. Een algemene implementatie die hier gebruikt is een dataset verdelen in K vouwen en vervolgens het model in een round robin op alle op één na het vouwen van de trein. 

**Optimalisatie van Hyperparameter** is het probleem van het kiezen van een set hyperparameters voor een algoritme leren meestal met het doel van een meting van de prestaties van de algoritme op een onafhankelijke set van gegevens optimaliseren. **Hyperparameters** zijn waarden die buiten het model training procedure moeten worden opgegeven. Veronderstellingen over deze waarden kunnen invloed hebben op de flexibiliteit en nauwkeurigheid van de modellen. Beslissingsstructuren zijn bijvoorbeeld hyperparameters, zoals de gewenste diepte en het aantal bladeren in de structuur. Support Vector Machines (SVMs) is vereist dat een boete misclassification term. 

Een veelgebruikte methode voor het uitvoeren van de optimalisatie van de hyperparameter is hier gebruikt is het zoeken van een raster of een **parameter sweep**. Dit bestaat uit het uitvoeren van een opgegeven deelverzameling van de ruimte hyperparameter een uitgebreide zoekopdracht via de waarden voor een algoritme leren. Cross validatie kunt opgeven voor het sorteren van de optimale resultaten geproduceerd door het raster zoekalgoritme performance metric. CV gebruikt met verstrekkende helpt hyperparameter limiet problemen zoals trainingsgegevens model overfitting, zodat het model behoudt de capaciteit toe te passen in de algemene set met gegevens waaruit de trainingsgegevens is uitgepakt.

De modellen die wij gebruiken zijn logistiek en lineaire regressie, willekeurige bossen en kleurovergang gestimuleerd bomen:

- [Lineaire regressie met SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is een lineaire regressiemodel met een methode stochastische kleurovergang afkomst (SGD) en voor optimalisatie- en schalen te voorspellen wat de tip bedragen betaald. 
- Regressie [logistische regressie met LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) of "logit", is een regressiemodel dat kan worden gebruikt als de afhankelijke variabele gegevens classificatie doen bestaan. LBFGS is een quasi-Newton optimalisatie algoritme die de algoritme voor Broyden – Fletcher – Goldfarb – Shanno (BFGS) met behulp van een beperkte hoeveelheid geheugen van de computer benadert en die veelvuldig wordt gebruikt in de machine learning.
- [Willekeurige forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) zijn ensembles van beslissingsbomen.  Ze combineren veel beslissingsstructuren om het risico te beperken. Willekeurige forests worden gebruikt voor regressie en classificatie en functies bestaan kunnen verwerken en kunnen worden uitgebreid naar de instelling van de multiclass-classificatie. Ze hoeven geen functie voor schalen en kunnen niet lichtgevoeligheid vastleggen en interacties zijn uitgerust. Willekeurige bossen zijn een van de meest succesvolle machine learning modellen voor de indeling en regressie.
- [Verloop versterkt bomen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) zijn ensembles van beslissingsbomen. GBTs trainen beslissingsstructuren iteratief tot een functie verlies te minimaliseren. GBTs worden gebruikt voor regressie en classificatie aankan functies bestaan, hoeven geen functie voor schalen en kunnen niet lichtgevoeligheid vastleggen en interacties zijn uitgerust. Ze kunnen ook worden gebruikt in een multiclass indeling.

Voorbeelden CV en Hyperparameter met Modeling sweep voor het probleem van de binaire indeling worden weergegeven. Eenvoudigere voorbeelden worden (zonder de parameter maakt furore) in het belangrijkste onderwerp voor regressie taken weergegeven. Maar in de bijlage, met elastische net voor lineaire regressie en CV parameter sweep gebruiken voor willekeurige forest regressie validatie ook worden gepresenteerd. De **elastische net** is een regressie geregulariseerd bij montage van lineaire regressie-modellen die lineair de metrics L1 en L2 als sancties van de [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) en [gleuf combineert](https://en.wikipedia.org/wiki/Tikhonov_regularization) .   



>[AZURE.NOTE] Hoewel de Spark MLlib toolkit is ontworpen voor gebruik op grote gegevenssets, is hier een relatief klein monster (~ 30 Mb met rijen 170K, ongeveer 0,1% van de originele dataset RDAM) gebruikt voor het gemak. De hier opgegeven oefening efficiënt (in ongeveer 10 minuten) wordt uitgevoerd op een HDInsight-cluster met 2 knooppunten van de werknemer. Dezelfde code bevatten, met kleine wijzigingen kan worden gebruikt voor het verwerken van grotere-gegevensverzamelingen, met passende wijzigingen voor het in cache opslaan van gegevens in het geheugen en het formaat van een cluster wijzigen.


## <a name="prerequisites"></a>Vereisten

U moet een account Azure en een HDInsight Spark u behoefte aan een HDInsight 3.4 Spark 1.6 cluster voor het voltooien van deze procedure. Zie het [Overzicht van Data Science met motoren op HDInsight Azure](machine-learning-data-science-spark-overview.md) voor instructies over het aan deze vereisten voldoen. Dit onderwerp bevat ook een beschrijving van de instructies voor het uitvoeren van code van een laptop Jupyter op het cluster vonk en RDAM 2013 Taxi gegevens hier gebruikt. De **pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** -laptop met de codevoorbeelden in dit onderwerp vindt u in de [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Setup: locaties, bibliotheken en de vooraf ingestelde Spark context

Vonk kan lezen en schrijven van Blob met Azure-opslag (ook bekend als WASB). Zodat u uw bestaande gegevens opgeslagen kunnen er worden verwerkt met behulp van motoren en de resultaten opnieuw opgeslagen in WASB.

U modellen en bestanden in WASB, moet het pad correct zijn opgegeven. De standaardcontainer die is gekoppeld aan het cluster vonk kan worden verwezen met behulp van een pad dat begint met: "wasb: / / / '. Andere locaties zijn verwezen door ' wasb: / / '.

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Directory paden voor opslaglocaties instellen in WASB

In het volgende voorbeeld wordt de locatie van de gegevens te lezen en het pad voor het model opslag map waarin de uitvoer van het model is opgeslagen:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**UITVOER**

DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)


### <a name="import-libraries"></a>Bibliotheken importeren

Importeer de benodigde bibliotheken met de volgende code:

    # LOAD PYSPARK LIBRARIES
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

De kernels PySpark die worden geleverd met Jupyter-laptops hebben een vooraf ingestelde context. Zo hoeft u niet in te stellen van de vonk of component contexten expliciet voordat u begint met het werken met de toepassing die u ontwikkelt. Deze contexten zijn standaard beschikbaar voor u. Deze contexten zijn:

- SC - voor motoren 
- sqlContext - voor component

De PySpark kernel biedt bepaalde voorgedefinieerde "magics", die zijn van speciale opdrachten die u met bellen kunt %%. Er zijn twee dergelijke opdrachten die in deze codevoorbeelden worden gebruikt.

- **%% lokale** Geeft aan dat de code in de volgende regels moet lokaal worden uitgevoerd. Code moet geldige Python code.
- **%%sql -o <variable name>** Hiermee voert u een query component op de sqlContext. Als de parameter -o is doorgegeven, het resultaat van de query worden behouden in de %% lokale Python context als een DataFrame Pandas.
 

Voor meer informatie over de kernels voor laptops voor Jupyter en de vooraf gedefinieerde "magics" die ze bieden, Zie [Kernels die beschikbaar zijn voor laptops met HDInsight vonk Linux clusters op HDInsight Jupyter](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="data-ingestion-from-public-blob"></a>De opname van de gegevens uit openbare blob: 

De eerste stap in het proces van de wetenschappelijke gegevens is te nemen van de gegevens uit bronnen waar het zich in het verkennen en modelleeromgeving bevindt worden geanalyseerd. Deze omgeving is Spark in dit overzicht. Deze sectie bevat de code voor een reeks taken uitvoeren:

- het voorbeeld moet worden gemodelleerd nemen
- gelezen in invoer dataset (opgeslagen als een bestand .tsv)
- indeling en de gegevens opschonen
- maken en objecten (RDDs of gegevens frames) in het geheugen in cache
- registreren als een temp-tabel in SQL-context.

Hier is de code voor de opname van de gegevens.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
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
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
        
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd boven cel uitvoering: 276.62 seconden


## <a name="data-exploration--visualization"></a>Verkennen & visualisatie 

Zodra de gegevens zijn Spark is binnengebracht, is de volgende stap in het proces van de wetenschap gegevens de gegevens via de exploratie en de visualisatie beter inzicht te krijgen. In deze sectie, we de taxi-gegevens met behulp van SQL-query's controleren en de potentiële functies voor visuele inspectie en doelvariabelen uitzetten. We uitzetten met name de frequentie van het aantal passagiers in taxi trips, de frequentie van de bedragen van de tip en hoe tips is afhankelijk van het te betalingsbedrag en het type.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Een histogram van de passagier aantal frequenties in het monster van de taxi trips uitzetten

Deze code en de daaropvolgende fragmenten gebruiken magic SQL query uitvoeren op het monster en de lokale magic de gegevens worden uitgezet.

- **SQL magic (`%%sql`)** De kernel HDInsight PySpark ondersteunt eenvoudig in line HiveQL's tegen de sqlContext. De (-o naam_variabele) argument blijft bestaan de uitvoer van de SQL-query als een DataFrame Pandas op de server Jupyter. Dit betekent dat in de lokale modus beschikbaar is.
- De ** `%%local` magic** code lokaal uitvoeren op de server Jupyter is de headnode van het cluster HDInsight wordt gebruikt. Meestal gebruikt u `%%local` magic in combinatie met de `%%sql` magic met de parameter -o. De parameter -o zou behouden de uitvoer van de SQL-query lokaal en vervolgens %% lokale magic wordt de volgende reeks codefragment lokaal uitvoeren tegen de uitvoer van de SQL-query's die lokaal wordt behouden

De uitvoer wordt automatisch weergegeven nadat u de code uitvoert.

Deze query haalt de trips door het aantal passagiers. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Deze code maakt een lokale gegevens-frame van de query-uitvoer en de gegevens worden uitgezet. De `%%local` magic maakt u een lokale gegevens-frame, `sqlResults`, die kan worden gebruikt voor het uitzetten van matplotlib. 

>[AZURE.NOTE] Deze magische PySpark wordt meerdere keren in dit overzicht gebruikt. Als de hoeveelheid gegevens te groot is, moet u een gegevens-om kader te maken dat past in het lokale geheugen monster.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Hier is de code voor het uitzetten van de trips door de passagier tellingen

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**UITVOER**

![Frequentie van de trips per aantal passagiers](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

U kunt kiezen uit verschillende soorten visualisaties (tabel, cirkel-, lijn, gebied of balk) met behulp van het **Type** menuknoppen in het notitieblok. Het waarnemingspunt balk wordt hier weergegeven.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Een histogram van de bedragen van de tip en hoe tip bedrag verschilt per passagier tellen en ritbedrag bedragen worden uitgezet.

Een SQL-query als u monsters wilt gebruiken...
    
    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25
    

Deze cel code gebruikt de SQL-query maken drie percelen van de gegevens.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**UITVOER:** 

![Tip bedragdistributie](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Bedrag per passagier aantal Tip](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip bedrag door ritbedrag bedrag](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Functie engineering, transformatie en gegevens voorbereiden voor modellering

In deze sectie worden beschreven en de code voor de procedures voor de gegevens gereedmaakt voor gebruik in ML modellering. Dit ziet u hoe u de volgende taken uitvoeren:

- Een nieuwe functie maken door binning uren in tijd-buckets verkeer
- Indexeren en functies bestaan op hot coderen
- Gelabelde point-objecten voor de invoer in ML functies maken
- Een willekeurige onderliggende steekproef van de gegevens maken en opsplitsen in opleiding en het testen van sets
- Functie schalen
- Cacheobjecten in het geheugen


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Een nieuwe functie maken door binning uren in tijd-buckets verkeer

Deze code geeft het maken van een nieuwe functie met binning uur in verkeer tijd buckets en vervolgens hoe u de resulterende gegevensframe in het geheugen in de cache. Indien veerkrachtig gedistribueerd Datasets (RDDs) en gegevens-frames herhaaldelijk gebruikt worden, leidt opslaan in cache tot verbeterde uitvoeringstijden. Dienovereenkomstig, we de cache RDDs en gegevens-frames in verschillende stadia in het overzicht.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**UITVOER**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Indexeren en functies bestaan een hot coderen

In dit gedeelte ziet u hoe indexeren of het bestaan van de functies voor invoer in de modelfuncties coderen. De modellen en het voorspellen van de functies van de MLlib functies met het bestaan van ingevoerde gegevens worden geïndexeerd of gecodeerd vóór gebruik nodig. 

Afhankelijk van het model dat u wilt indexeren of ze op verschillende manieren coderen. Bijvoorbeeld Logistic en lineaire regressie-modellen vereisen een hot-codering, waar, bijvoorbeeld een functie met drie categorieën in drie kolommen functie, met elke met 0 of 1, afhankelijk van de categorie van een waarneming kan worden uitgebreid. MLlib biedt [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) -functie voor een hot-codering. Dit coderingsprogramma wijst een kolom met de label indices aan een kolom van binaire vectoren, met hoogstens één een-waarde. Deze codering kan algoritmen die verwachten numerieke waarden functies, zoals de logistische regressie dat, moeten worden toegepast op voorzieningen bestaan.

Hier is de code voor het indexeren en coderen functies bestaan:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd boven cel uitvoering: 3.14 seconden


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Gelabelde point-objecten voor de invoer in ML functies maken

Deze sectie bevat de code waarmee wordt aangegeven hoe tekst bestaan als een gegevenstype met een label punt indexeren en deze te coderen zodat deze kan worden gebruikt voor het trainen en test logistische regressie van MLlib en andere modellen voor de classificatie. Gelabelde point-objecten zijn veerkrachtig gedistribueerd Datasets (RDD) op een manier die door de meeste ML algoritmen in de MLlib is nodig als invoergegevens opgemaakt. Een [punt met het label](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is een lokale vector, compacte of verspreid, die is gekoppeld aan een label/antwoord.

Hier is de code voor het indexeren en tekstfuncties voor binaire indeling coderen.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Dit is de code voor het coderen en indexeren bestaan tekstfuncties voor lineaire regressie-analyse.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Een willekeurige onderliggende steekproef van de gegevens maken en opsplitsen in opleiding en het testen van sets

Deze code maakt een willekeurige steekproef van de gegevens (25% wordt hier gebruikt). Hoewel het niet vereist voor dit voorbeeld vanwege de grootte van de dataset wordt gedemonstreerd hoe u kunt een monster nemen hier zodat u hoe weet te gebruiken voor uw eigen probleem, als dat nodig is. Wanneer monsters groot zijn, bespaart dit veel tijd tijdens de training. We vervolgens splitsen het monster in een training delen (hier 75%) en een testen (hier 25%) te gebruiken in de indeling en het regressie model.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Tijd boven cel uitvoering: 0.31 seconden


### <a name="feature-scaling"></a>Functie schalen

Functie schaal, ook wel bekend als gegevens normaliseren, verzekert dat functies met veel keren waarden worden niet gezien veel wegen in de objectieve functie. De code voor het schalen van de functie gebruikt de [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) voor het schalen van de functies variantie eenheid. Het is geleverd door MLlib in een lineaire regressie met stochastische kleurovergang afkomst (SGD), een populaire algoritme voor een breed scala van andere modellen zoals geregulariseerd behalen of support vector machines (SVM) leren machine opleiding.   

>[AZURE.TIP] We hebben het algoritme LinearRegressionWithSGD gevoelig kunt u een schaal gevonden.   

Hier is de code voor de variabelen voor gebruik met de regularized SGD-algoritme lineaire schaal.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Tijd boven cel uitvoering: 11.67 seconden


### <a name="cache-objects-in-memory"></a>Cacheobjecten in het geheugen

Is de tijd voor training en testen van ML algoritmen kan worden verminderd door het frame invoergegevens objecten gebruikt voor de indeling, de regressie en geschaald functies caching.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER** 

Tijd boven cel uitvoering: 0.13 seconden


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Voorspellen of een tip wordt betaald met de modellen van de binaire indeling

In dit gedeelte ziet hoe gebruik drie modellen voor de taak van de binaire indeling van voorspellen of er een tip voor een taxi rit wordt betaald. De gepresenteerde modellen zijn:

- Logistische regressie 
- Willekeurige forest
- Kleurovergang prestatieverbetering bomen

Elk model sectie code bouwen is onderverdeeld in stappen: 

1. **Model** trainingsgegevens met één parameter ingesteld
2. **Model-evaluatie** op een gegevensset test met statistieken
3. **Model opslaan** in blob voor toekomstige consumptie

We zien hoe u cross-validatie (AVK) met parameter verstrekkende op twee manieren:

1. **Algemene** aangepaste code die kan worden toegepast op elk algoritme in MLlib en aan elke parameter wordt ingesteld in een algoritme. 
1. Met behulp van de **pySpark CrossValidator pipeline-functie**. Houd er rekening mee dat hoewel handig, op basis van onze ervaring, CrossValidator een paar beperkingen voor Spark 1.5.0 heeft: 

    - Modellen van de pijpleiding kunnen niet worden opgeslagen/bewaard voor toekomstige consumptie.
    - Kan niet worden gebruikt voor elke parameter in een model.
    - Kan niet worden gebruikt voor elke MLlib-algoritme.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Algemeen cross-validatie en hyperparameter Veegpoeder met de logistische regressie-algoritme gebruikt voor de binaire indeling

De code in dit gedeelte ziet u hoe te trainen, te evalueren en een logistische regressiemodel opslaan met [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) die worden voorspeld of een tip voor een rit in de dataset RDAM taxi rit en ritbedrag wordt betaald. Het model is getraind met behulp van cross-validatie (AVK) en hyperparameter sweeping geïmplementeerd met aangepaste code die kan worden toegepast op elk van de algoritmen learning in MLlib.   

>[AZURE.NOTE] De uitvoering van aangepaste code CV kan enkele minuten duren.

**De logistische regressiemodel met CV en de Veegpoeder hyperparameter trainen**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Coëfficiënten: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Snijpunt:-0.0111216486893

Tijd boven cel uitvoering: 14.43 seconden


**De binaire indeling model met standaard maatstelsel te evalueren**

De code in dit gedeelte ziet u hoe een logistische regressiemodel tegen een test-gegevensverzameling, met inbegrip van de tekening van de ROC-curve te evalueren.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Gebied onder PR = 0.985336538462

Oppervlak onder de ROC = 0.983383274312

Overzicht statistieken

Precisie = 0.984174341679

Intrekken = 0.984174341679

F1-Score = 0.984174341679

Tijd boven cel uitvoering: 2.67 seconden


**De ROC-curve uitzetten.**

De *predictionAndLabelsDF* is geregistreerd als een tabel, een *tmp_results*in de vorige cel. *tmp_results* kan worden gebruikt op 's en resultaten voor uitvoer in het gegevensframe sqlResults getekend. Dit is de code.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier is de code voor de voorspellingen en de ROC-curve uitzetten.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
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
    

**UITVOER**

![Logistische regressie ROC-curve voor de algemene benadering](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**Model in een blob voor toekomstige consumptie behouden**

De code in dit gedeelte ziet u hoe de logistische regressiemodel voor consumptie opslaan.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**UITVOER**

Tijd boven cel uitvoering: 34.57 seconden


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>MLlib van CrossValidator pipeline-functie gebruiken met logistische regressie (elastische regressie)-model

De code in dit gedeelte ziet u hoe te trainen, te evalueren en een logistische regressiemodel opslaan met [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) die worden voorspeld of een tip voor een rit in de dataset RDAM taxi rit en ritbedrag wordt betaald. Het model is getraind met behulp van cross-validatie (AVK) en hyperparameter sweeping geïmplementeerd met de functie MLlib CrossValidator pijplijn voor CV met parameter sweep.   


>[AZURE.NOTE] De uitvoering van deze code MLlib CV kan enkele minuten duren.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER**

Tijd boven cel uitvoering: 107.98 seconden


**De ROC-curve uitzetten.**

De *predictionAndLabelsDF* is geregistreerd als een tabel, een *tmp_results*in de vorige cel. *tmp_results* kan worden gebruikt op 's en resultaten voor uitvoer in het gegevensframe sqlResults getekend. Dit is de code.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Hier is de code voor het uitzetten van de ROC-curve.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
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


**UITVOER**

![Logistische regressie ROC-curve met behulp van MLlib CrossValidator](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Willekeurige forest classificatie

De code in dit gedeelte ziet u hoe te trainen, evalueren en opslaan van een willekeurige forest regressie die worden voorspeld of een tip voor een rit in de dataset RDAM taxi rit en ritbedrag wordt betaald.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Oppervlak onder de ROC = 0.985336538462

Tijd boven cel uitvoering: 26.72 seconden


### <a name="gradient-boosting-trees-classification"></a>Kleurovergang prestatieverbetering bomen classificatie

De code in dit gedeelte ziet u hoe te trainen, evalueren, en een kleurovergang prestatieverbetering bomen model worden voorspeld of een tip wordt betaald voor een rit in de taxi RDAM reis opslaan en ritbedrag dataset.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Oppervlak onder de ROC = 0.985336538462

Tijd boven cel uitvoering: 28.13 seconden


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Tip bedrag voorspellen met regressie-modellen (niet met AVK)

In dit gedeelte ziet hoe drie modellen voor de taak van de achteruitgang van het voorspellen van het bedrag van de tip gebruik betaald voor een taxi rit op basis van een andere tip-functies. De gepresenteerde modellen zijn:

- Lineaire regressie geregulariseerd
- Willekeurige forest
- Kleurovergang prestatieverbetering bomen

Deze modellen zijn beschreven in de inleiding. Elk model sectie code bouwen is onderverdeeld in stappen: 

1. **Model** trainingsgegevens met één parameter ingesteld
2. **Model-evaluatie** op een gegevensset test met statistieken
3. **Model opslaan** in blob voor toekomstige consumptie   


>AZURE Opmerking: Cross-validatie wordt niet gebruikt met de drie regressie-modellen in deze sectie, aangezien dit is weergegeven in de details voor de logistische regressie-modellen. Een voorbeeld met het gebruik van CV met elastisch Net voor lineaire regressie wordt geleverd in de bijlage van dit onderwerp.


>AZURE Opmerking: In onze ervaring, kunnen er problemen optreden met de convergentie van de LinearRegressionWithSGD en parameters moeten worden gewijzigd/geoptimaliseerd voor het verkrijgen van een geldig model. Schaal van variabelen aanzienlijk helpt bij de convergentie. Elastisch net regressie, weergegeven in de bijlage bij dit onderwerp, kan ook worden gebruikt in plaats van LinearRegressionWithSGD.


### <a name="linear-regression-with-sgd"></a>Lineaire regressie met SGD

De code in deze sectie wordt beschreven hoe geschaalde functies gebruiken om een lineaire regressie met stochastische kleurovergang afkomst (SGD) voor de optimalisatie van de trein, en het scoren, evalueren en sla het model in Azure Blob Storage (WASB).

>[AZURE.TIP] In onze ervaring, kunnen er problemen optreden met de convergentie van de LinearRegressionWithSGD en parameters moeten worden gewijzigd/geoptimaliseerd voor het verkrijgen van een geldig model. Schaal van variabelen aanzienlijk helpt bij de convergentie.


    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Coëfficiënten: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Onderscheppen: 0.854507624459

RMSE = 1.23485131376

R sqr = 0.597963951127

Tijd boven cel uitvoering: 38.62 seconden


### <a name="random-forest-regression"></a>Willekeurige Forest regressie

De code in dit gedeelte ziet u hoe te trainen, te evalueren en een willekeurige forest-model tip bedrag voor de RDAM taxi rit gegevens voorspelt opslaan.   

>[AZURE.NOTE] Cross-validatie met parameter verstrekkende met aangepaste code is opgenomen in het aanhangsel.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

RMSE = 0.931981967875

R sqr = 0.733445485802

Tijd boven cel uitvoering: 25.98 seconden


### <a name="gradient-boosting-trees-regression"></a>Kleurovergang prestatieverbetering bomen regressie

De code in dit gedeelte ziet u hoe te trainen, te evalueren en een kleurovergang prestatieverbetering bomen model tip bedrag voor de RDAM taxi rit gegevens voorspelt opslaan.

**Trainen en evalueren**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

RMSE = 0.928172197114

R sqr = 0.732680354389

Tijd boven cel uitvoering: 20.9 seconden


**Uitzetten**
    
*tmp_results* is geregistreerd als een component-tabel in de vorige cel. Resultaten uit de tabel worden uitgevoerd in de *sqlResults* gegevensframe getekend. Dit is de code

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier is de code voor de gegevens met behulp van de Jupyter server uitzetten.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Werkelijk-vs-voorspeld-tip-bedragen](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Aanhangsel: Extra regressie taken met meerdere validatie met parameter krijgen

Deze bijlage bevat de code waarin CV met elastisch net voor lineaire regressie doen en CV met parameter sweep met aangepaste code voor willekeurige forest regressie doen.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Cross-validatie met behulp van elastisch net voor lineaire regressie

De code in deze sectie wordt beschreven hoe met elastisch net voor lineaire regressie validatie overschrijden en het evalueren van het model met de testgegevens.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd boven cel uitvoering: 161.21 seconden

**Evalueren met R SQR metric**

*tmp_results* is geregistreerd als een component-tabel in de vorige cel. Resultaten uit de tabel worden uitgevoerd in de *sqlResults* gegevensframe getekend. Dit is de code

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Dit is de code voor de berekening van R sqr.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**UITVOER**

R sqr = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Met de parameter sweep met aangepaste code voor willekeurige forest regressie cross-validatie

De code in deze sectie wordt beschreven hoe validatie met parameter sweep met aangepaste code voor willekeurige forest regressie overschrijden en het evalueren van het model met de testgegevens.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

RMSE = 0.906972198262

R sqr = 0.740751197012

Tijd boven cel uitvoering: 69.17 seconden


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Objecten uit het geheugen en locaties afdrukken model opruimen

Gebruik `unpersist()` in het cachegeheugen opgeslagen objecten verwijderen.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


**UITVOER**

PythonRDD [122] op RDD op PythonRDD.scala: 43


**Pad naar modelbestanden moet worden gebruikt in het notitieblok verbruik printout.** Te verbruiken en een onafhankelijke gegevensset score, moet u kopiëren en plakken deze bestandsnamen in het notitieblok"verbruik".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**UITVOER**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Hoe nu verder?

Nu u regressie en classificatie modellen met de Spark MlLib hebt gemaakt, bent u klaar om te leren hoe de score en evalueren van deze modellen.

**Verbruik model:** Als u wilt weten hoe de score en evalueren van de indeling en regressie-modellen gemaakt in dit onderwerp, Zie [Score en evalueren van motoren gebouwd machine learning modellen](machine-learning-data-science-spark-model-consumption.md).
