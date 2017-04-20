<properties
    pageTitle="Schaalbare Data Science in Azure gegevens Lake: een end-to-end scenario | Microsoft Azure"
    description="Het gebruik van Azure gegevens meer gegevens te verkennen en binaire indeling taken uitvoeren op een dataset."  
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
    ms.date="09/19/2016"
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Schaalbare Data Science in Azure gegevens Lake: een end-to-end-overzicht

In dit scenario wordt met Azure gegevens Lake verkennen en taken van de binaire indeling van een steekproef van de taxi RDAM reis en ritbedrag dataset om te voorspellen of een tip worden betaald door een tarief. Het leidt u door de stappen van de [Procedure voor het Team gegevens wetenschap](http://aka.ms/datascienceprocess)end-to-end, van gegevens oplevert als model voor opleiding en voor de implementatie van een webservice die in het model wordt gepubliceerd.


### <a name="azure-data-lake-analytics"></a>Azure gegevens Lake Analytics

De [Microsoft Azure gegevens Lake](https://azure.microsoft.com/solutions/data-lake/) heeft alle mogelijkheden kunt u gemakkelijk gegevens wetenschappers vereist voor het opslaan van gegevens van elke grootte, vorm en snelheid en gegevensverwerking en geavanceerde machine leren modelleren met hoge schaalbaarheid op een kosteneffectieve wijze uitvoeren.   U betaalt op basis van per taak alleen wanneer de gegevens daadwerkelijk wordt verwerkt. Azure gegevens Lake Analytics U SQL bevat, een taal die de declaratieve aard van SQL met de expressieve voordelen van C# voor schaalbare overvloeiingen verdeeld query-mogelijkheden. Deze kunt u ongestructureerde gegevens verwerken volgens schema op lezen, invoegen van aangepaste logica en door gebruiker gedefinieerde functies (UDF's), en uitbreidbaarheid zodat u probleemloos korrelig controle over het uitvoeren van schaal bevat. Zie voor meer informatie over de ontwerpfilosofie achter U SQL, [Visual Studio blog boeken](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Gegevens Lake Analytics is ook een belangrijk onderdeel van Cortana Analytics-pakket en het werkt met Azure SQL Data Warehouse, Power BI en Data Factory. Dit geeft u een volledig cloud big data en geavanceerde analytics platform.

In dit scenario begint met het beschrijven van de voorwaarden en middelen die nodig zijn voor het voltooien van de taken met gegevens Lake Analytics die het proces van de wetenschappelijke gegevens en hoe ze te installeren. Vervolgens worden de gegevens verwerken stappen beschreven die U SQL gebruikt en wordt afgesloten door het gebruik van Python en component weer te geven met Azure Machine Learning Studio kunnen bouwen en implementeren van de voorspellende modellen. 


### <a name="u-sql-and-visual-studio"></a>U SQL en Visual Studio

In dit scenario raadt U SQL-scripts voor het verwerken van de gegevensset bewerken met Visual Studio. De U SQL-scripts zijn die hier worden beschreven en die in een afzonderlijk bestand. Het proces omvat ingesting, verkennen en bemonstering van de gegevens. Ook ziet u hoe u een taak U SQL-scripts uitvoeren vanaf de portal Azure. Component tabellen worden gemaakt voor de gegevens in een gekoppeld cluster HDInsight ter vergemakkelijking van het gebouw en de implementatie van een model van de binaire indeling in Azure Machine Learning Studio.  


### <a name="python"></a>Python

In dit scenario bevat ook een sectie ziet u hoe te bouwen en implementeren van een voorspellende model met Python met Azure Machine Learning Studio.  We voorzien van een laptop Jupyter de Python scripts voor de volgende stappen in dit proces. De laptop bevat een code voor een extra functie engineering stappen en modellen bouw zoals multiclass classificatie en regressie model naast de binaire indeling model hier beschreven. De regressie is te voorspellen, het bedrag van de op basis van andere functies tip tip. 


### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning Studio wordt gebruikt voor het bouwen en implementeren van de voorspellende modellen. Dit wordt gedaan met behulp van twee benaderingen: eerst met behulp van Python scripts en vervolgens met component tabellen op een HDInsight (Hadoop)-cluster.


### <a name="scripts"></a>Scripts

Alleen de belangrijkste stappen worden in deze procedure beschreven. U kunt het volledige **U SQL-script** en de **Laptop Jupyter** downloaden van [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Vereisten

Voordat u deze onderwerpen, hebt u het volgende:

- Een abonnement op Azure. Als u nog geen een, Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- (Aanbevolen) Visual Studio 2013 en 2015. Als u nog geen een van deze versies zijn geïnstalleerd, kunt u een gratis Community edition downloaden vanaf [hier](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Klik op de knop **downloaden Gemeenschap 2015** onder het gedeelte Visual Studio. 

>[AZURE.NOTE] U kunt ook de Azure Portal Azure gegevens meer query's verzenden in plaats van Visual Studio gebruiken. We bieden instructies hierover zowel met Visual Studio en de portal in de sectie **gegevens met U SQL proces**. 

- Aanmelding voor Azure gegevens Lake Preview

>[AZURE.NOTE] U moet krijgen goedkeuring Azure gegevens Lake winkel (ADLS) en Azure gegevens Lake Analytics (ADLA) als deze services in het voorbeeld zijn gebruikt. U wordt gevraagd aan te melden bij het maken van uw eerste ADLS of ADLA. Sigh van, klik op **aanmelden om te bekijken**, lees de gebruiksrechtovereenkomst en klik op **OK**. Hier is bijvoorbeeld de pagina ADLS aanmelding:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Wetenschap-omgeving voor Azure gegevens meer gegevens voorbereiden
Als voorbereiding op het milieu van de wetenschap gegevens voor dit scenario, maken de volgende bronnen:

- Azure Lake gegevensarchief (ADLS) 
- Azure gegevens Lake Analytics (ADLA)
- Azure Blob opslag account
- Azure Machine Learning Studio account
- Azure Lake hulpmiddelen voor Visual Studio (aanbevolen)

Deze sectie bevat instructies voor het maken van elk van deze bronnen. Als u component om tabellen te gebruiken met Azure Machine Learning in plaats van Python, voor het opbouwen van een model, moet u ook een HDInsight (Hadoop)-cluster inrichten. Deze alternatieve procedure in de desbetreffende sectie hieronder beschreven.
<br/>
>AZURE. Opmerking de **Azure Lake gegevensarchief** kunt maken op afzonderlijk of als u de **Azure gegevens Lake Analytics** als de standaard opslag maakt. Instructies voor het maken van elk van deze bronnen afzonderlijk hieronder wordt verwezen, maar de gegevens meer opslagruimte account hoeft niet afzonderlijk worden gemaakt.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Maak een Azure-gegevensarchief Lake

Een ADLS maken vanaf de [Portal Azure](http://portal.azure.com). Zie [een cluster van HDInsight met Lake gegevensarchief maken met behulp van Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)voor meer informatie. Zorg ervoor dat het instellen van de identiteit van de AAD Cluster in het blad **gegevensbron** van de **Optionele configuratie** blade daar beschreven. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Een Azure gegevens Lake Analytics-account maken
Een ADLA-account maken vanaf de [Portal Azure](http://portal.azure.com). Zie voor meer informatie [Zelfstudie: aan de slag met Azure gegevens Lake Analytics met Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Maak een account Azure Blob-opslag
Een Azure Blob opslag-account maken vanaf de [Portal Azure](http://portal.azure.com). Zie voor meer informatie, het maken van een sectie opslag in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md).
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Een Azure Machine Learning Studio account instellen
Omhoog/naar Azure Machine Learning Studio aanmelden vanaf de pagina [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) . Klik op de knop **nu aan de slag** en kies vervolgens een "Werkruimte vrij" of "Standaard werkruimte". Daarna kun je voor het maken van experimenten in Azure ML Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Installeren van Azure Lake Gegevenshulpmiddelen (aanbevolen)
Azure gegevens Lake's installeren voor uw versie van Visual Studio van [Azure Lake hulpmiddelen voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Nadat de installatie voltooid is, opent u Visual Studio. Hier ziet u de gegevens Lake het menu boven aan het tabblad. Azure resources moeten worden weergegeven in het linkerdeelvenster wanneer u op uw account Azure inloggen.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>De dataset RDAM Taxi Trips
De gegevensset die we hier gebruikt is een openbaar beschikbare dataset-- [RDAM Taxi Trips dataset](http://www.andresmh.com/nyctaxitrips/). De gegevens RDAM Taxi reis bestaat uit 20GB gecomprimeerd CSV-bestanden (~ 48GB niet-gecomprimeerd), vastleggen van meer dan 173 miljoen afzonderlijke trips en de tarieven voor elke reis betaald. Elke reis record bevat de pickup en inleverbibliotheek locaties en tijden, anoniem hack (bestuurder) licentienummer en het nummer van de straten (unieke id van de taxi). De gegevens in het jaar 2013 heeft betrekking op alle trips en wordt geleverd in de volgende twee gegevenssets voor elke maand:

 - Trip_data CSV bevat details van de reis, zoals het aantal passagiers, ophalen en punten dropoff, duur van de reis en duur van reis. Hier zijn een aantal voorbeeldrecords:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - 'trip_fare' CSV bevat details van de voor elke reis, zoals betalingstype bedrag ritbedrag, toeslag en belastingen, tips en tolgelden, betaalde reissom en het totale bedrag betaald. Hier zijn een aantal voorbeeldrecords:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel reis deelnemen aan\_gegevens en reis\_ritbedrag bestaat uit de volgende drie velden: straten, hack\_licentie en ophalen van\_datum/tijd. De ruwe CSV-bestanden toegankelijk is vanuit een openbare opslag van Azure blob. Het U SQL-script voor deze koppeling is in de sectie [Join reis en ritbedrag tabellen](#join) .

## <a name="process-data-with-u-sql"></a>Verwerken van gegevens met U SQL

De gegevensverwerking taken in deze sectie worden beschreven zijn ingesting, kwaliteitscontrole, verkennen en bemonstering van de gegevens. We zien ook hoe reis en ritbedrag tabellen koppelen. In het laatste gedeelte wordt uitgevoerd een script U SQL-taak vanaf de portal Azure. Hier vindt koppelingen naar elke onderafdeling:

- [Opname van de gegevens: in openbare blob-gegevens lezen](#ingest)
- [Gegevens kwaliteit controle](#quality)
- [Verkennen](#explore)
- [Reis- en ritbedrag tabellen koppelen](#join)
- [Bemonstering van gegevens](#sample)
- [U SQL-taken uitvoeren](#run)

De U SQL-scripts zijn die hier worden beschreven en die in een afzonderlijk bestand. U kunt de volledige **U SQL-scripts** downloaden van [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Voor het uitvoeren van de U-SQL, Visual Studio openen, klikt u op **Bestand--> Nieuw Project-->**, kiest **U SQL-Project**, naam en opslaan naar een map.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Het is mogelijk met behulp van de Portal Azure U-SQL in plaats van Visual Studio worden uitgevoerd. U kunt navigeren naar de resource Azure gegevens Lake Analytics op de portal en query's worden rechtstreeks als geïllustreerde in de volgende afbeelding.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Ingestie van gegevens: Gegevens uit openbare blob gelezen

De locatie van de gegevens in de Azure blob wordt verwezen als **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** en kunnen worden geëxtraheerd met behulp van **Extractors.Csv()**. Vervangen door uw eigen naam van de container en de naam van opslag in de volgende scripts voor container_name@blob_storage_account_name in het adres wasb. Aangezien de namen van bestanden in dezelfde indeling, gebruiken we **reis\_data_ {\*\}CSV** in alle 12 reis bestanden te lezen. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Omdat er in de eerste rij koppen, moeten we de koppen verwijderen en kolomtypen in juiste eenheden. We kunnen ofwel de verwerkte gegevens Azure Lake gegevensopslag met **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ of Azure Blob-opslag met account opslaan **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Op dezelfde manier kunnen we lezen in het ritbedrag gegevenssets. Klik met de rechtermuisknop Azure Lake gegevensarchief, kunt u uw gegevens in **Azure Portal--> Data Explorer** of **File Explorer** binnen Visual Studio wilt bekijken. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Gegevens kwaliteit controle

Nadat u tabellen reis en ritbedrag zijn gelezen, kunnen gegevens kwaliteit controle plaatsvinden op de volgende manier. Het resulterende CSV-bestanden kunnen worden uitgevoerd naar Azure Blob-opslag of Azure Lake gegevensarchief. 

Zoek het nummer van de medallions en het unieke nummer van medallions:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

De medallions die meer dan 100 trips had zoeken:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Zoeken die ongeldige records in pickup_longitude:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Zoeken naar ontbrekende waarden voor sommige variabelen:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Verkennen

We kunt bepaalde gegevens te verkennen om een beter begrip van de gegevens doen.

De verdeling van de fooien en niet-Gekantelde trips vinden:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

De verdeling van het bedrag van de tip met de waarden van de licht-donkerscheiding te vinden: 0,5,10 en 20 euro.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Basisstatistieken van reis afstand vinden:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Zoek de percentielen van reis afstand:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Reis- en ritbedrag tabellen koppelen

Reis- en ritbedrag tabellen kunnen worden verbonden door straten, hack_license en pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Bereken het aantal records, gemiddelde tip bedrag, de variatie van de tip bedrag, percentage van Gekantelde trips voor elk niveau van het aantal passagiers.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Bemonstering van gegevens

Eerst selecteren we willekeurig 0,1% van de gegevens uit de gekoppelde tabel:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

We doen vervolgens toepassing stratificatie bemonstering door binaire variabele tip_class:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>U SQL-taken uitvoeren

Wanneer u klaar bent U SQL-scripts, kunt u deze verzenden naar de server met uw Azure gegevens Lake Analytics-account. Klik op de knop **verzenden** klikt u op **Gegevens Lake** **Taak verzenden**, selecteer uw **Analytics-Account**en **parallellisme**kiezen.  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Wanneer het project met succes is voldaan, wordt de status van de taak weergegeven in Visual Studio voor het toezicht. Nadat de taak is voltooid, kunt u zelfs tijdens het proces van de uitvoering van taak herhalen en vinden het knelpunt stappen uit om de efficiëntie van uw project te verbeteren. U kunt ook naar de Portal te controleren van de status van uw taken U SQL Azure gaan.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Controleer nu de uitvoerbestanden in Azure Blob-opslag of Azure Portal. De toepassing van stratificatie voorbeeldgegevens gebruiken we voor onze modellen in de volgende stap.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Bouwen en implementeren van modellen in Azure Machine Learning

Ingegaan op twee opties die beschikbaar zijn voor u om gegevens op te halen in Azure Machine Learning te bouwen en 

- In de eerste optie de bemonsterde gegevens gebruiken die is geschreven voor een Azure Blob (in de **steekproef van gegevens** stap hierboven) en Python gebruiken voor het bouwen en implementeren van de modellen van Azure Machine Learning. 
- In de tweede optie query u de gegevens in Azure gegevens Lake rechtstreeks met een query component. Deze optie moet u een nieuw HDInsight-cluster maken of gebruik een bestaand cluster van HDInsight waar de component tabellen verwijzen naar de gegevens van de Taxi NY in Azure Lake gegevensopslag.  Zowel deze opties hieronder besproken. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Optie 1: Gebruik Python kunnen bouwen en implementeren van de computer leren modellen

Om te bouwen en implementeren van machine learning modellen met Python, een laptop Jupyter te maken op uw lokale computer of in Azure Machine Learning Studio. De Jupyter-laptop die op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) bevat de volledige code te verkennen, het visualiseren van gegevens, technische functie, modellering en implementatie. We zien in dit artikel wordt alleen de modellering en implementatie. 

### <a name="import-python-libraries"></a>Python bibliotheken importeren

Script-bestand de volgende Python pakketten nodig zijn om uit te voeren van het monster Jupyter Notebook of de Python. Deze pakketten zijn vooraf geïnstalleerd als u de service AzureML laptop.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Lezen in de gegevens van de blob

- Verbindingsreeks   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Lezen als tekst

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Namen van kolommen toevoegen en de afzonderlijke kolommen

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Sommige kolommen wijzigen in numeriek

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Machine learning modellen bouwen

Wij bouwen hier een binaire indeling model om te voorspellen of een reis is Gekantelde of niet. In de laptop Jupyter vindt u andere twee modellen: multiclass indeling en regressie-modellen.

- Eerst moet u maken dummy variabelen die kunnen worden gebruikt in scikit-informatie over modellen

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Kader voor de modellering gegevens maken

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Training en 60-40 splitsen testen

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logistische regressie in opleiding set

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Score van gegevensverzameling testen

        Y_test_pred = logit_fit.predict(X_test)

- Evaluatie statistieken berekenen

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Web Service API bouwen en verbruiken Python

U wilt mogelijk maken de computer leren model nadat deze is opgebouwd. We gebruiken hier de binaire logistieke model als voorbeeld. Zorg ervoor dat de scikit-informatie over de versie op de lokale computer is 0.15.1. U hebt geen zorgen te maken over dit als u Azure ML studio service.

- Uw werkruimte referenties van Azure ML studio instellingen zoeken. Klik op **Instellingen**in Azure Machine Learning Studio --> **naam** --> **Vergunning Tokens**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Web-Service maken

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Web-Servicereferenties ophalen

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Bel Web service API. U moet 5-10 seconden wachten na de vorige stap.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Optie 2: Maken en implementeren van modellen rechtstreeks in Azure Machine Learning

Azure Machine Learning Studio gegevens rechtstreeks vanuit Azure Lake gegevensarchief kunt lezen en vervolgens worden gebruikt voor het maken en implementeren van modellen. Deze benadering wordt een component-tabel die op de Azure Lake gegevensarchief verwijst. Hiervoor is vereist dat een afzonderlijke HDInsight Azure-cluster worden ingericht, op de component-tabel wordt gemaakt. De volgende secties worden deze procedure. 

### <a name="create-an-hdinsight-linux-cluster"></a>Een HDInsight Linux Cluster maken

Maak een HDInsight Cluster (Linux) vanaf de [Portal Azure](http://portal.azure.com). Zie de sectie **een cluster HDInsight met toegang tot de gegevensopslag Lake Azure maken** in [een cluster HDInsight met Lake gegevensarchief maken met behulp van Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)voor meer informatie.

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Component-tabel maken in de HDInsight

We maken nu de tabellen in het HDInsight-cluster met behulp van de gegevens in de vorige stap in Azure Lake gegevensopslag worden gebruikt in Azure Machine Learning Studio component. Ga naar de HDInsight cluster hebt gemaakt. Klik op **Instellingen** --> **Eigenschappen** --> **Cluster AAD identiteit** --> **ADLS Access**, zorg ervoor dat uw account Azure Lake gegevensarchief wordt toegevoegd in de lijst met lezen, schrijven en uitvoeren van rechten. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Klik op **Dashboard** naast de knop **Instellingen** en een venster wordt weergegeven. Klik op **Component-weergave** in het bovenste rechterhoek van de pagina en u ziet de **Query Editor**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


In de volgende component scripts voor het maken van een tabel te plakken. De locatie van de gegevensbron in Azure Lake gegevensarchief verwijzing op deze manier is: **adl://data_lake_store_name.azuredatalakestore.net:443/mapnaam/bestandsnaam**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Als de query is voltooid, ziet u de resultaten als volgt:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Bouwen en implementeren van modellen in Azure Machine Learning Studio

We zijn nu klaar om te bouwen en implementeren van een model die worden voorspeld of een tip wordt betaald met Azure Machine Learning. De toepassing van stratificatie voorbeeldgegevens is klaar om te worden gebruikt in deze binaire indeling (tip of niet) probleem. De voorspellende modellen met multiclass-indeling (tip_class) en regressie (tip_amount) kunnen ook worden gebouwd en geïmplementeerd met Azure Machine Learning Studio, maar hier we alleen zien hoe verwerken met behulp van het model van de binaire indeling.

1. De gegevens ophalen in Azure ML met behulp van de module **Gegevens importeren** in de sectie **gegevensinvoer en uitvoer** . Zie de pagina met naslaginformatie [module gegevens importeren](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) voor meer informatie.
2. Selectiequery **Component** als **gegevensbron** in het deelvenster **Eigenschappen** .
3. Het volgende script in de component in de **component databasequery** editor plakken

        select * from nyc_stratified_sample;

4. Voer de URI van de HDInsight cluster (dit kunt u vinden in Azure Portal), Hadoop referenties, locatie van de gegevens over de output, en de naam van sleutel-naam/container Azure opslag.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Een voorbeeld van een binaire indeling experiment lezen van gegevens uit de tabel component wordt weergegeven in de onderstaande afbeelding.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Nadat het experiment is gemaakt, klikt u op **Webservice instellen** --> **Anticiperende Web Service**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Uitvoeren van de automatisch gemaakte scoren experiment, wanneer dit is voltooid, klikt u op de **Web-Service implementeren**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Het dashboard web service wordt kort weergegeven:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Samenvatting

U hebt een data science omgeving voor het bouwen van schaalbare oplossingen voor end-to-end in Azure gegevens Lake gemaakt door de procedure is voltooid. Deze omgeving is gebruikt voor het analyseren van grote openbare dataset, laat dit door de canonieke stappen van het proces gegevens wetenschap, uit de overname gegevens door middel van training model en voor de implementatie van het model als een webservice. U SQL is gebruikt om te verwerken, te verkennen en de voorbeeldgegevens. Python en component zijn met Azure Machine Learning Studio kunnen bouwen en implementeren van voorspellende modellen gebruikt.

## <a name="whats-next"></a>Hoe nu verder?

Het pad van de training voor het [Team gegevens wetenschap proces (TDSP)](http://aka.ms/datascienceprocess) biedt koppelingen naar onderwerpen met een beschrijving van elke stap in het proces van geavanceerde analytics. Er zijn een aantal scenario's gespecificeerd op de pagina [Team gegevens wetenschap proces scenario's](data-science-process-walkthroughs.md) waarin het gebruik van bronnen en services in verschillende analytics anticiperende scenario:

- [Het Team gegevens wetenschap proces in actie: SQL Data Warehouse gebruiken](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Het Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters gebruiken](machine-learning-data-science-process-hive-walkthrough.md)
- [Het Team gegevens wetenschap proces: SQL Server met behulp](machine-learning-data-science-process-sql-walkthrough.md)
- [Overzicht van de gegevens wetenschap met vonkontsteking in Azure, HDInsight](machine-learning-data-science-spark-overview.md)

