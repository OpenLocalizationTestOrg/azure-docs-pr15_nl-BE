<properties
    pageTitle="Overzicht van Data Science met vonkontsteking in Azure HDInsight | Microsoft Azure"
    description="De toolkit Spark MLlib brengt aanzienlijke machine learning modellering mogelijkheden aan de gedistribueerde omgeving voor HDInsight."
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

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Overzicht van Data Science met vonkontsteking in Azure, HDInsight

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Dit pakket van onderwerpen zien hoe HDInsight Spark met gemeenschappelijke gegevens wetenschap taken uitvoeren zoals ingestie van gegevens, technische functie modelleren en model-evaluatie. De gegevens die gebruikt is een voorbeeld van de 2013 RDAM taxi rit en ritbedrag dataset. De modellen gebouwd zijn logistiek en lineaire regressie, willekeurige bossen en kleurovergang gestimuleerd bomen. De onderwerpen weergeven ook het opslaan van deze modellen in Azure blob-opslag (WASB) en het score en evalueren van hun voorspellende prestatie. Meer geavanceerde onderwerpen besproken hoe modellen kunnen worden getraind met behulp van de Veegpoeder cross-validatie en hyper-parameter. In dit onderwerp wordt ook beschreven hoe het Spark-cluster moet u de stappen in de drie scenario's instellen. 

[Spark](http://spark.apache.org/) is een open source parallelle verwerking die ondersteuning biedt voor verwerking om zo de prestaties van analytische toepassingen van big data in het geheugen. Spark verwerkings-engine is gebouwd voor snelheid en gebruiksgemak, en geavanceerde analytische. Spark van gedistribueerde berekening in het geheugen mogelijkheden maken het een goede keuze voor iteratieve algoritmen in de machine learning en grafiek berekeningen. [MLlib](http://spark.apache.org/mllib/) is Spark van schaalbare machine learning bibliotheek die modelfuncties deze gedistribueerde omgeving brengt. 

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) is open source Spark Azure gehoste aangeboden. Ook is er ondersteuning voor **laptops Jupyter PySpark** op de cluster Spark Spark SQL-interactieve query's voor het transformeren van filtering en visualiseren van gegevens die zijn opgeslagen in Azure BLOB's (WASB) kan worden uitgevoerd. PySpark is de Python API voor motoren. De codefragmenten die oplossingen bieden en de relevante percelen om de gegevens hier uitgevoerd in Jupyter-laptops die zijn geïnstalleerd op de clusters Spark anders weergeven. De stappen van het model in deze onderwerpen bevatten code die laat hoe trainen zien, evalueren, opslaan en verwerken elk type model. 

De procedures en de code die u in dit scenario is voor HDInsight 3.4 Spark 1.6. De code hier en in de laptops is echter algemeen en moet werken aan een cluster vonk. Als u geen van HDInsight Spark, de clusterinstallatie gebruikmaakt en management stappen enigszins afwijken van wat hier wordt weergegeven.

## <a name="prerequisites"></a>Vereisten

1. Je moet een Azure-abonnement hebben. Als u nog geen een, Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. u moet een cluster HDInsight 3.4 Spark 1.6 voor het voltooien van deze procedure. Zie de instructies in om een [aan de slag: maken van Apache Spark op Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Het cluster en de versie is opgegeven in het menu **Type van Cluster selecteren** . 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Zie de [wetenschap gegevens met behulp van Scala met vonkontsteking op Azure](machine-learning-data-science-process-scala-walkthrough.md)voor een onderwerp waarin wordt uitgelegd hoe u Scala in plaats van Python gebruiken om taken voor een end-to-end gegevens wetenschap proces te voltooien.

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>De gegevens RDAM 2013 Taxi

De gegevens RDAM Taxi rit is ongeveer 20 GB aan bestanden gecomprimeerd door komma's gescheiden waarden (CSV) (~ 48 GB niet-gecomprimeerd), die bestaat uit meer dan 173 miljoen afzonderlijke trips en de tarieven voor elke reis betaald. Elke reis record bevat de pick up en afgiftepunt en tijd, anoniem hack (bestuurder) licentienummer, en nummer straten (unieke id van de taxi). De gegevens in het jaar 2013 heeft betrekking op alle trips en wordt geleverd in de volgende twee gegevenssets voor elke maand:

1. De 'trip_data' CSV-bestanden bevatten de details van de reis, zoals het aantal passagiers, afhalen en dropoff verwijst, duur en de duur van reis krachtvoertuigen. Hier zijn een aantal voorbeeldrecords:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. De 'trip_fare' CSV-bestanden bevatten gegevens van de voor elke reis, zoals betalingstype bedrag ritbedrag, toeslag en belastingen, tips en tolgelden, betaalde reissom en de totaal betaalde bedrag. Hier zijn een aantal voorbeeldrecords:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5


We hebben genomen een monster van 0,1% van deze bestanden en lid van de reis\_gegevens en reis\_ritbedrag CVS bestanden in een enkele dataset wilt gebruiken als de dataset input voor dit scenario. De unieke sleutel reis deelnemen aan\_gegevens en reis\_ritbedrag is samengesteld uit de velden: straten, hack\_certificaat en ophalen van\_datum/tijd. Elke record van de dataset bevat de volgende kenmerken die een reis RDAM Taxi:


|Veld| Korte beschrijving
|------|---------------------------------
| straten |Taxi anoniem straten (taxi unieke id)
| hack_license |    Anoniem Hackney vervoer licentienummer
| vendor_id |   Taxi leverancier-id
| rate_code | RDAM taxi tarief van tarief
| store_and_fwd_flag | Opslaan en doorsturen vlag
| pickup_datetime | Datum en tijd ophalen
| dropoff_datetime | Dropoff datum & tijd
| pickup_hour | Uur afhalen
| pickup_week | Week van het jaar ophalen
| dag van de week | Weekday (bereik 1-7)
| passenger_count | Aantal passagiers in een taxi rit
| trip_time_in_secs | Reis tijd in seconden
| trip_distance | Reis afstand in mijlen afgelegd
| pickup_longitude | Lengtegraad afhalen
| pickup_latitude | Latitude afhalen
| dropoff_longitude | Dropoff lengte
| dropoff_latitude | Dropoff latitude
| direct_distance | Afstand tussen de pick up directe en dropoff locaties
| payment_type | Betalingstype (cas, creditcard enz.)
| fare_amount | Ritbedrag bedrag in
| toeslag | Toeslag
| mta_tax | MTA-belasting
| tip_amount | Tip bedrag
| tolls_amount | Tolgelden bedrag
| total_amount | Totaalbedrag
| Gekantelde | Gekantelde (0-1 voor Nee of Ja)
| tip_class | Tip-klasse (0: $0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Code uitvoeren vanaf een laptop Jupyter op het cluster Spark 

U kunt de laptop Jupyter vanaf de portal Azure starten. Zoeken naar uw cluster Spark op uw dashboard en klik op beheerpagina invoeren voor het cluster. De laptop die is gekoppeld aan het cluster Spark, klikt u op **Dashboards Cluster** -> **Jupyter laptop** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

U kunt ook zoeken naar de ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** voor toegang tot de Jupyter-laptops. De CLUSTERNAAM deel van deze URL vervangen door de naam van uw eigen cluster. U moet het wachtwoord voor uw account admin toegang te krijgen tot de laptops.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Selecteer PySpark om te zien van een map met daarin een paar voorbeelden van voorverpakte laptops die gebruikmaken van de API PySpark. De laptops die de codevoorbeelden voor deze suite van Spark onderwerp bevatten zijn beschikbaar op [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


Het Spark-cluster, kunt u de laptops van Github rechtstreeks naar de laptop Jupyter server uploaden. Klik op de knop **uploaden** in het rechterdeel van het scherm op de introductiepagina van uw Jupyter. De Bestandsverkenner van een wordt geopend. Hier kunt u de URL Github (onbewerkte inhoud) van het notitieblok plakken en klik op **openen**. De PySpark-laptops zijn beschikbaar op de volgende URL's:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Ziet u de bestandsnaam op uw Jupyter lijst met bestanden met de knop **uploaden** opnieuw. Klik op deze knop **uploaden** . Nu hebt u de laptop geïmporteerd. Herhaal deze stappen als u wilt uploaden van de andere laptops van dit scenario.

> [AZURE.TIP] U kunt met de rechtermuisknop op de koppelingen in de browser en selecteer **Koppeling kopiëren** naar de URL github ruwe inhoud ophalen. U kunt deze URL in Jupyter uploaden explorer het dialoogvenster bestand kunt plakken.

U kunt nu het volgende doen:

- De code bekijken door te klikken op de laptop.
- Elke cel uitvoeren door op **SHIFT + ENTER**te drukken.
- Het hele notitieblok uitvoeren door te klikken op de **cel** -> **uitvoeren**.
- Gebruik de automatische weergave van query's.

> [AZURE.TIP] De kernel PySpark visualiseert automatisch de uitvoer van query's SQL (HiveQL). U worden kunt kiezen uit verschillende soorten visualisaties (tabel, cirkel-, lijn, gebied of balk) met behulp van het **Type** menuknoppen in het notitieblok vermeld:

![Logistische regressie ROC-curve voor de algemene benadering](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Hoe nu verder?

Nu u een cluster HDInsight Spark zijn ingesteld en de Jupyter-laptops zijn geüpload, bent u klaar om te werken door de onderwerpen die met de drie PySpark-laptops overeenkomen. Ze tonen hoe uw gegevens te verkennen en maken en verbruiken modellen. De geavanceerde exploratie en modellering laptop wordt cross-validatie, hyper-parameter verstrekkende, opnemen en het model van de evaluatie. 

**Verkennen en modellering met vonkontsteking:** Verken de dataset en maken, score, en evalueren van de machine modellen te leren door te werken via het onderwerp [binaire indeling en regressie-modellen voor de gegevens met de toolkit Spark MLlib maken](machine-learning-data-science-spark-data-exploration-modeling.md) .

**Verbruik model:** Als u wilt weten hoe de score van de classificatie en regressie-modellen gemaakt in dit onderwerp, Zie [Score en evalueren van motoren gebouwd machine learning modellen](machine-learning-data-science-spark-model-consumption.md).

**Cross-validatie en hyperparameter verstrekkende**: Zie [Geavanceerde verkennen en modellering met vonkontsteking](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) op hoe de modellen kunnen worden getraind met behulp van de Veegpoeder cross-validatie en hyper-parameter

