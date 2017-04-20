<properties
    pageTitle="Het Team gegevens wetenschap proces in actie: clusters gebruiken Hadoop | Microsoft Azure"
    description="Met behulp van het Team gegevens wetenschap proces voor een end-to-end scenario die gebruikmaakt van een cluster HDInsight Hadoop kunnen bouwen en implementeren van een model met een algemeen beschikbare dataset."
    services="machine-learning,hdinsight"
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
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Het Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters gebruiken

In dit scenario gebruiken we het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md) in een end-to-end scenario met een [cluster Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) opslaan, verkennen en uitgerust met gegevens uit het openbaar [RDAM Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset engineer en de gegevens monster omlaag. Modellen van de gegevens die zijn gebouwd met Azure Machine Learning voor het verwerken van binaire en multiclass-classificatie en regressie voorspellende taken.

Zie voor een scenario waarin het afhandelen van een grotere dataset (1 terabyte) voor een soortgelijk scenario met HDInsight Hadoop clusters voor gegevensverwerking, [Team gegevens wetenschap proces - Azure HDInsight Hadoop Clusters met behulp van een dataset 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Het is ook mogelijk om met een laptop IPython taken voor het scenario met behulp van de dataset van 1 TB wordt aangeboden. Het onderwerp [Criteo scenario een component een ODBC-verbinding met](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) gebruikers die u wilt proberen deze benadering dient te raadplegen.


## <a name="dataset"></a>RDAM Taxi Trips Dataset beschrijving

De gegevens RDAM Taxi rit is ongeveer 20GB aan bestanden gecomprimeerd door komma's gescheiden waarden (CSV) (~ 48GB niet-gecomprimeerd), die bestaat uit meer dan 173 miljoen afzonderlijke trips en de tarieven voor elke reis betaald. Elke reis record bevat de locatie ophalen en inleverbibliotheek en tijd, anoniem hack (bestuurder) licentienummer en nummer straten (unieke id van de taxi). De gegevens in het jaar 2013 heeft betrekking op alle trips en wordt geleverd in de volgende twee gegevenssets voor elke maand:

1. De 'trip_data' CSV-bestanden bevatten de details van de reis, zoals het aantal passagiers, ophalen en punten dropoff, duur van de reis en duur van reis. Hier zijn een aantal voorbeeldrecords:

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

De unieke sleutel reis deelnemen aan\_gegevens en reis\_ritbedrag is samengesteld uit de velden: straten, hack\_certificaat en ophalen van\_datum/tijd.

Als u alle gegevens relevant zijn voor een bepaalde reis, volstaat het deelnemen aan met drie sleutels: "straten", "inbreken\_licentie" en "afhalen\_datum/tijd".

Wanneer we deze opslaan in tabellen component kort beschreven bepaalde meer details van de gegevens.

## <a name="mltasks"></a>Voorbeelden van taken die voorspelling
Als benaderen van gegevens, het bepalen van de aard van de voorspellingen die u wilt maken gebaseerd op de analyse helpt bij het verduidelijken van de taken die u moet opnemen in uw proces.
Hier zijn drie voorbeelden van voorspelling problemen die we in deze procedure waarvan de formulering op basis van de *tip\_bedrag*:

1. **Binaire indeling**: voorspellen of een tip betaald is voor een reis, dat wil zeggen een *tip\_bedrag* die groter is dan $0 een positief voorbeeld is, terwijl een *tip\_bedrag* is een negatief voorbeeld van $0.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiclass classificatie**: het bereik van de tip bedragen betaald voor de reis te voorspellen. We delen de *tip\_bedrag* in vijf bakken of klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressie taak**: te voorspellen, het bedrag van de tip voor een reis betaald.  


## <a name="setup"></a>Een cluster HDInsight Hadoop voor geavanceerde analytics instellen

>[AZURE.NOTE] Dit is meestal een **Admin** -taak.

U kunt een Azure-omgeving voor geavanceerde analytics die gebruikmaakt van een cluster van HDInsight in drie stappen instellen:

1. [Een opslag-account maken](../storage/storage-create-storage-account.md): deze opslag rekening wordt gebruikt voor het opslaan van gegevens in Azure Blob-opslag. De gegevens die worden gebruikt in clusters HDInsight ook bevinden zich hier.

2. [Clusters Azure HDInsight Hadoop aanpassen voor geavanceerde Analytics proces en technologie](machine-learning-data-science-customize-hadoop-cluster.md). Deze stap maakt een Azure HDInsight Hadoop cluster met 64-bits Anaconda, Python 2.7 geïnstalleerd op alle knooppunten. Er zijn twee belangrijke stappen te onthouden bij het aanpassen van uw cluster HDInsight.

    * Vergeet niet om de opslag-account gemaakt in stap 1 met het cluster HDInsight bij het maken van het te koppelen. Deze opslag rekening wordt gebruikt voor toegang tot gegevens die binnen het cluster wordt verwerkt.

    * Nadat u het cluster hebt gemaakt, kunt u externe toegang inschakelen met het hoofd knooppunt van het cluster. Ga naar het tabblad **configuratie** en klik op **Afstand inschakelen**. In deze stap geeft de referenties van de gebruiker die wordt gebruikt voor externe aanmelding.

3. [Een werkruimte Azure Machine leren maken](machine-learning-create-workspace.md): werkruimte dit Azure Machine leren wordt gebruikt om machine learning modellen te bouwen. Deze taak is gericht, na het initiële gegevens verkennen en omlaag bemonstering op basis van het cluster HDInsight.

## <a name="getdata"></a>De gegevens ophalen uit een openbare bron

>[AZURE.NOTE] Dit is meestal een **Admin** -taak.

Als u de dataset [RDAM Taxi Trips](http://www.andresmh.com/nyctaxitrips/) uit de openbare locatie, kunt u een van de methoden die worden beschreven in [Gegevens verplaatsen naar en van Azure Blob-opslag](machine-learning-data-science-move-azure-blob.md) de gegevens kopiëren naar uw computer.

Hier beschrijven we hoe AzCopy gebruiken voor het overbrengen van de bestanden die gegevens bevatten. Als u wilt downloaden en installeren van AzCopy de instructies op [Aan de slag met de AzCopy Command-Line Utility](../storage/storage-use-azcopy.md).

1. Afgeven vanaf een opdrachtprompt de volgende opdrachten AzCopy *< path_to_data_folder >* vervangen door de gewenste bestemming:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Wanneer het kopiëren is voltooid, zijn in totaal 24 ZIP-bestanden in de map data gekozen. Unzip de gedownloade bestanden naar dezelfde map op uw lokale computer. Maak een notitie van de map waar de gedecomprimeerde bestanden zich bevinden. Deze map zal worden verwezen als de *< pad\_te\_unzipped_data\_bestanden\> * is wat volgt.


## <a name="upload"></a>De gegevens uploaden naar de standaardcontainer van Azure HDInsight Hadoop cluster

>[AZURE.NOTE] Dit is meestal een **Admin** -taak.

In de volgende opdrachten van de AzCopy vervangen door de volgende parameters de werkelijke waarden die u hebt opgegeven bij het maken van het cluster Hadoop en de gegevensbestanden ritsen.

* ***& #60; path_to_data_folder >*** de map (in combinatie met het pad) op uw computer met de bestanden uitgepakt  
* ***& #60; naam van cluster Hadoop opslag >*** de opslag-account die is gekoppeld aan het cluster HDInsight
* ***& #60; standaardcontainer van Hadoop cluster >*** de standaardcontainer die wordt gebruikt door het cluster. Opmerking de naam van de standaardcontainer is doorgaans dezelfde naam als het cluster zelf. Als het cluster wordt 'abc123.azurehdinsight.net' genoemd, is de standaardcontainer abc123.
* ***& #60; opslag account sleutel >*** de sleutel voor de opslag die wordt gebruikt door het cluster

Uitvoeren vanaf een opdrachtprompt of een Windows PowerShell-venster op de computer, de volgende twee opdrachten voor AzCopy.

Met deze opdracht de gegevens van de reis naar ***nyctaxitripraw*** directory in de standaardcontainer van het cluster Hadoop geüpload.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Met deze opdracht uploadt tarief gegevens naar de map ***nyctaxifareraw*** in de standaardcontainer van het cluster Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

De gegevens moet nu in Azure Blob-opslag en gereed om te worden verbruikt in de cluster HDInsight.

## <a name="#download-hql-files"></a>Meld u aan bij het hoofd knooppunt van het cluster Hadoop en en analyse van experimentele gegevens voorbereiden

>[AZURE.NOTE] Dit is meestal een **Admin** -taak.

Voor toegang tot de hoofd-knooppunt van het cluster door bemonstering van de gegevens en analyse van experimentele gegevens, volgt u de procedure die wordt beschreven in [de kop van Hadoop Cluster knooppunt toegang](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

In dit scenario gebruiken we voornamelijk query's die zijn geschreven in een [component](https://hive.apache.org/), een querytaal met SQL-achtige voor explorations voorlopige gegevens. De component query's worden opgeslagen in bestanden met .hql. We vervolgens voorbeeldgegevens omlaag dit in Azure Machine Learning worden gebruikt voor het maken van modellen.

Ter voorbereiding van het cluster voor analyse van experimentele gegevens, downloaden we de .hql-bestanden met de betreffende component scripts [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) naar een lokale map (C:\temp) op het knooppunt head. Hiertoe opent u de **opdrachtprompt** uit binnen de head-knooppunt van het cluster en de volgende twee opdrachten:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Deze twee opdrachten wordt gedownload, alle .hql bestanden die nodig zijn in deze procedure naar een lokale map ***C:\temp & #92;*** in het head-knooppunt.

## <a name="#hive-db-tables"></a>Maken van de component database en tabellen gepartitioneerd per maand

>[AZURE.NOTE] Dit is meestal een **Admin** -taak.

We zijn nu gereed voor het maken van tabellen voor onze taxi RDAM dataset component.
In het head-knooppunt van het cluster Hadoop, opent u de ***opdrachtregel Hadoop*** op het bureaublad van het knooppunt head en typt u de map component door te voeren van de opdracht

    cd %hive_home%\bin

>[AZURE.NOTE] **Alle opdrachten van de component in dit scenario uit de bovenstaande component opslaglocatie uitvoeren / directory prompt. Dit zal zorgen voor problemen op pad automatisch. Gebruiken we de termen 'Component directory prompt', ' component bin / directory prompt ", en" Hadoop opdrachtregel ' door elkaar in dit scenario.**

Voer de volgende opdracht in Hadoop opdrachtregel van het hoofd knooppunt de component database en tabellen maken component query verzenden vanaf de opdrachtprompt van de map component:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Hier wordt de inhoud van de ***C:\temp\sample\_component\_maken\_db\_en\_tables.hql*** -bestand dat wordt gemaakt van de component database tabellen en ***nyctaxidb*** ***reis*** en het ***tarief***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Dit script component maakt twee tabellen:

* de 'trip'-tabel bevat details van de reis van elke rit (stuurprogrammagegevens, pickup tijd, reis afstand en tijden.)
* de tabel "tarief" bevat ritbedrag (ritbedrag, tip bedrag, tolgelden en toeslagen).

Als u met deze procedures als u meer hulp nodig hebt of wilt onderzoeken die alternatieve, Zie de sectie [indienen component query's rechtstreeks vanaf de opdrachtregel van Hadoop ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Laden van gegevens aan tabellen component partities

>[AZURE.NOTE] Dit is meestal een **Admin** -taak.

De dataset RDAM taxi heeft een natuurlijke partitionering per maand, die we gebruiken voor snellere verwerking en de query tijd inschakelen. De PowerShell-opdrachten eronder (uitgegeven door de component directory met behulp van de **opdrachtregel Hadoop**) laden gegevens in de "reis" en "tarief" component gepartitioneerd per maand.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Het *monster\_component\_laden\_data\_door\_partitions.hql* -bestand bevat de volgende opdrachten voor het **laden** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Opmerking een aantal query's dat component we hier in het proces van exploratie gebruiken betrekking hebben op één partitie of op een aantal partities te zoeken. Maar deze query's over de gehele gegevens kunnen worden uitgevoerd.

### <a name="#show-db"></a>Databases weergeven in de cluster HDInsight Hadoop

Voer de volgende opdracht in het Hadoop-opdrachtregel zodat de databases die zijn gemaakt in het cluster in het venster opdrachtregel Hadoop HDInsight Hadoop:

    hive -e "show databases;"

### <a name="#show-tables"></a>De component tabellen in de database nyctaxidb weergeven

Voer de volgende opdracht in het Hadoop-opdrachtregel zodat de tabellen in de database nyctaxidb:

    hive -e "show tables in nyctaxidb;"

We kunnen bevestigen dat de tabellen worden gepartitioneerd met de volgende opdracht:

    hive -e "show partitions nyctaxidb.trip;"

De verwachte output wordt hieronder weergegeven:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Op dezelfde manier kunnen we ervoor zorgen dat de tarief-tabel door de opdracht hieronder is gepartitioneerd:

    hive -e "show partitions nyctaxidb.fare;"

De verwachte output wordt hieronder weergegeven:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Verkennen en functie engineering in component

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Het verkennen en technische taken voor de gegevens in de component tabellen geladen functie kunnen worden uitgevoerd met de component-query's. Hier volgen voorbeelden van dergelijke taken dat we u doorlopen die in deze sectie:

- De top 10 records in beide tabellen weergeven.
- Ontdek de verdelingen van de gegevens van een paar velden in verschillende tijdvensters.
- Onderzoek de kwaliteit van de gegevens van de velden lengte en breedte.
- Genereren binaire en multiclass classificatie labels op basis van de **tip\_bedrag**.
- Functies genereren door het berekenen van de directe reis afstanden.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Exploratie: De top 10 records bekijken in de tabel reis

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Als u wilt zien hoe de gegevens eruit, onderzoeken we 10 records uit elke tabel. De volgende twee query's afzonderlijk uitvoeren vanaf de opdrachtprompt van de map component in de console vanaf de opdrachtregel Hadoop om de records te controleren.

De top 10 records ophalen in de tabel 'trip' van de eerste maand:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Om de top 10 records in de tabel "tarief" van de eerste maand:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Het is vaak nuttig voor het opslaan van de records in een bestand voor handige weergave. Dit wordt bereikt door een kleine wijziging aan de bovenstaande query:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Exploratie: Het aantal records in elk van de 12 partities weergeven

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Van belang is de manier waarop het aantal trips varieert gedurende het kalenderjaar. Groeperen per maand kunt u zien wat deze verdeling van trips eruit ziet.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Dit geeft ons de uitvoer:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Hier is de maand van de eerste kolom en de tweede is het aantal trips voor die maand.

We kunnen ook het totale aantal records in onze reis gegevensset tellen door de volgende opdracht bij de opdrachtprompt van de map component.

    hive -e "select count(*) from nyctaxidb.trip;"

Dit levert:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Opdrachten die vergelijkbaar is met die welke voor de gegevensset reis kunnen we component query's vanaf de opdrachtprompt van de map component voor de gegevensset ritbedrag voor het valideren van het aantal records verlenen.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Dit geeft ons de uitvoer:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Houd er rekening mee dat precies hetzelfde aantal bezoeken per maand voor beide gegevensverzamelingen wordt geretourneerd. Dit biedt de eerste validatie van de gegevens correct zijn geladen.

Telling van het totale aantal records in de gegevensset ritbedrag kan worden gedaan met behulp van de opdracht hieronder vanaf de opdrachtprompt van de map component:

    hive -e "select count(*) from nyctaxidb.fare;"

Dit levert:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Het totale aantal records in beide tabellen is ook hetzelfde. Dit zorgt voor een tweede validatie van de gegevens correct zijn geladen.

### <a name="exploration-trip-distribution-by-medallion"></a>Exploratie: Reis distributie door de straten

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

In dit voorbeeld geeft de straten (taxi nummers) met meer dan 100 trips binnen een bepaalde periode. De voordelen van de query in de gepartitioneerde tabeltoegang omdat het wordt geconditioneerd door de partitie variabele **maand**. Resultaten van de query worden geschreven naar een lokaal bestand queryoutput.tsv in `C:\temp` op het hoofd knooppunt.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Hier wordt de inhoud van *monster\_component\_reis\_aantal\_door\_medallion.hql* -bestand voor inspectie.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

De straten in de gegevensset RDAM taxi geeft een unieke cabine. We kunnen identificeren welke cab-bestanden 'bezet' worden door de vraag welke meer dan een bepaald aantal trips gemaakt in een bepaalde periode. In het volgende voorbeeld geeft de CAB-bestanden die meer dan honderd trips gemaakt in de eerste drie maanden en de query resulteert in een lokaal bestand, C:\temp\queryoutput.tsv, worden opgeslagen.

Hier wordt de inhoud van *monster\_component\_reis\_aantal\_door\_medallion.hql* -bestand voor inspectie.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Geef de onderstaande opdracht vanaf de opdrachtprompt van de map component:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Exploratie: Reis distributie door de straten en hack_license

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Bij het verkennen van een dataset, willen we vaak het aantal co-exemplaren van groepen van waarden onderzoekt. In deze sectie vindt u een voorbeeld van hoe u dit doet voor CAB-bestanden en stuurprogramma's.

Het *monster\_component\_reis\_aantal\_door\_straten\_license.hql* bestand worden de gegevensset ritbedrag op 'straten' en 'hack_license' en geeft als resultaat de tellingen van elke combinatie. Hieronder vindt u de inhoud ervan.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Deze query geeft als resultaat het CAB-bestand en bepaalde stuurprogramma combinaties gesorteerd op aflopende aantal trips.

Uitvoeren vanaf de opdrachtprompt van de map component:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Resultaten van de query naar een lokaal bestand C:\temp\queryoutput.tsv geschreven.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Exploratie: Beoordeling van de kwaliteit van de gegevens door te controleren op ongeldige lengte/latitude records

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Ongeldige of onjuiste records wieden is een gemeenschappelijke doelstelling van de analyse van experimentele gegevens. In het voorbeeld in deze sectie bepaalt of de lengte of de latitude velden een waarde ver buiten het gebied RDAM bevatten. Aangezien het waarschijnlijk is dat dergelijke records een lengte-latitude foutieve waarden hebben, willen we deze van alle gegevens die moet worden gebruikt voor de modellering te elimineren.

Hier wordt de inhoud van *monster\_component\_kwaliteit\_assessment.hql* -bestand voor inspectie.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Uitvoeren vanaf de opdrachtprompt van de map component:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Het argument *-S* is opgenomen in deze opdracht wordt de status scherm afdruk van de taken voor component kaart/verminderen onderdrukt. Dit is handig omdat het maakt het scherm afdrukken van de uitvoer van de query component beter leesbaar.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Exploratie: Klasse binaire distributies reis tips

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Het probleem voor binaire indeling beschreven in de sectie met [voorbeelden van taken die voorspelling](machine-learning-data-science-process-hive-walkthrough.md#mltasks) , is het handig om te weten of een tip of niet is opgegeven. Deze verdeling van tips is binaire:

* Tip gegeven (klasse 1, punt\_> $0 bedrag)  
* geen tip (klasse 0, tip\_bedrag = $0).

Het *monster\_component\_Gekantelde\_frequencies.hql* dit bestand hieronder weergegeven wordt.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Uitvoeren vanaf de opdrachtprompt van de map component:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Exploratie: Klasse verdelingen in de instelling van de multiclass

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Voor de classificatie multiclass probleem die worden beschreven in de sectie met [voorbeelden van taken die voorspelling](machine-learning-data-science-process-hive-walkthrough.md#mltasks) deze set gegevens ook gepaard met een natuurlijke classificatie waar willen we het bedrag van de gegeven tips te voorspellen. We kunt opslaglocaties tip bereiken definiëren in de query. Als u de klasse verdelingen voor de verschillende bereiken tip, gebruiken we de *monster\_component\_tip\_bereik\_frequencies.hql* bestand. Hieronder vindt u de inhoud ervan.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

De volgende opdracht uitvoeren vanaf de opdrachtregel Hadoop console:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Exploratie: Directe afstand tussen twee locaties van de lengtegraad Latitude berekenen

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Een maatregel van de directe afstand met stelt ons in staat om erachter te komen van het verschil tussen deze en de werkelijke reis afstand. Wij motiveren deze functie door te wijzen op dat een passagier kan waarschijnlijk minder tip als ze weten dat het stuurprogramma opzettelijk door een veel langere route getroffen.

Overzicht van de vergelijking tussen de werkelijke reis afstand en de [Haversine afstand](http://en.wikipedia.org/wiki/Haversine_formula) tussen twee punten van de geografische lengte latitude (de "" grootcirkelafstand), gebruiken we de goniometrische functies beschikbaar in component, dus:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

In de query, R de straal van de aarde in mijlen is en pi wordt geconverteerd naar radialen. Houd er rekening mee dat de lengtegraad latitude punten "gefilterd" om waarden die ver van het gebied RDAM te verwijderen.

In dit geval schrijven we onze resultaten naar een map met de naam 'queryoutputdir'. De volgorde van de onderstaande opdrachten maakt eerst deze uitvoermap en vervolgens de component-opdracht wordt uitgevoerd.

Uitvoeren vanaf de opdrachtprompt van de map component:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Resultaten van de query worden geschreven naar 9 Azure BLOB's ***queryoutputdir/000000\_0*** aan ***queryoutputdir/000008\_0*** onder de standaardcontainer van het cluster Hadoop.

Overzicht van de grootte van de afzonderlijke BLOB's uitvoeren we de volgende opdracht vanaf de opdrachtprompt van de map component:

    hdfs dfs -ls wasb:///queryoutputdir

Overzicht van de inhoud van een bepaald bestand zegt 000000\_0, we maken gebruik van Hadoop `copyToLocal` opdracht dus.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`kan erg traag is voor grote bestanden, en wordt niet aanbevolen voor gebruik met hen.  

Een groot voordeel van deze gegevens zijn opgeslagen in een Azure blob is dat besproken welke gegevens in Azure Machine Learning met behulp van de [Gegevens importeren] [ import-data] module.


## <a name="#downsample"></a>Omlaag monster gegevens en build modellen in Azure Machine Learning

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Na de fase van de analyse van experimentele gegevens gaan we nu de gegevens voor het maken van modellen in Azure Machine Learning monster omlaag. In deze sectie, we zien hoe omlaag monster met een component-query de gegevens vervolgens de [Importgegevens vanuit] [ import-data] module in Azure Machine Learning.

### <a name="down-sampling-the-data"></a>Door bemonstering van de gegevens

Er zijn twee stappen in deze procedure. Eerst we de tabellen **nyctaxidb.trip** en **nyctaxidb.fare** op drie sleutels die aanwezig in alle records zijn samenvoegen: "straten", "inbreken\_licentie", en "afhalen\_datum/tijd". We vervolgens een binaire indeling label **Gekantelde** en een label met meerdere klasse indeling genereren **tip\_klasse**.

Gebruik de pijl-omlaag kunnen bemonsterd gegevens direct vanuit de [Gegevens importeren] [ import-data] in Azure Machine Learning module is nodig voor het opslaan van de resultaten van de bovenstaande query naar een interne component-tabel. In wat volgt, we maken een interne component-tabel en de inhoud ervan met de gekoppelde en de bemonsterde gegevens invullen.

De query geldt standaard component functies rechtstreeks voor het genereren van het uur van de dag, week van het jaar, de dag van de week (1 staat voor maandag en 7 staat voor zondag) vanaf de "afhalen\_datum/tijd" veld en de rechtstreekse afstand tussen de locaties ophalen en dropoff. Gebruikers kunnen [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) raadplegen voor een volledige lijst van deze functies.

De query vervolgens monsters omlaag de gegevens zodat de resultaten van de query in de Studio Azure Machine Learning past. Slechts ongeveer 1% van de oorspronkelijke gegevensset wordt geïmporteerd in de Studio.

Hieronder vindt u de inhoud van *monster\_component\_voorbereiden\_voor\_aml\_full.hql* worden gegevens voorbereid voor model bouwen in Azure Machine Learning bestand.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Deze query uitvoeren vanaf de opdrachtprompt van de map component:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

We hebben nu een interne tabel 'nyctaxidb.nyctaxi_downsampled_dataset' kan worden geopend met de [Gegevens importeren] [ import-data] module van Azure Machine Learning. We kunnen deze dataset bovendien gebruiken voor het bouwen van Machine Learning modellen.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Gebruik de module gegevens importeren in Azure Machine Learning voor de bemonsterde omlaag gegevens

De vereisten voor de afgifte van component query's in de [Gegevens importeren] [ import-data] module van Azure Machine Learning, moeten we de toegang tot een Azure Machine Learning werkruimte en toegang tot de referenties van het cluster en de bijbehorende opslag account.

Sommige details van de [Importgegevens] [ import-data] -module en de parameters kunt invoeren:

**HCatalog server-URI**: als de clusternaam abc123 is, is dit gewoon: https://abc123.azurehdinsight.net

**Naam van gebruikersaccount Hadoop** : de naam van de gebruiker gekozen voor het cluster (**niet** de naam van de RAS-gebruiker)

**Hadoop ser wachtwoord** : het wachtwoord gekozen voor het cluster (**niet** het wachtwoord voor externe toegang)

**Locatie van uitvoergegevens** : dit zijn Azure is gekozen.

**Azure opslag accountnaam** : naam van de standaard opslag-account die is gekoppeld aan het cluster.

**Azure containernaam** : dit is de standaardnaam van de container voor het cluster en is meestal hetzelfde als de naam van het cluster. Voor een cluster "abc123" genoemd, is dit slechts abc123.

> [AZURE.IMPORTANT] **Elke tabel die we willen vragen de [Gegevens importeren] via[ import-data] in Azure Machine Learning module moet een interne tabel.** Een tip om te bepalen of een T-tabel in een database-D.db een interne tabel is is als volgt.

Geef de opdracht vanaf de opdrachtprompt van de map component:

    hdfs dfs -ls wasb:///D.db/T

Als de tabel een interne tabel is en deze is gevuld, moeten hier de inhoud weergeven. Een andere manier om te bepalen of een tabel een interne tabel is is met de Opslagverkenner Azure. Hiermee gaat u naar de standaardnaam van de container van het cluster en vervolgens filteren op de naam van de tabel. Als de tabel en de bijbehorende inhoud weergegeven, bevestigt dit is een interne tabel.

Hier is een momentopname van de component-query en de [Gegevens importeren] [ import-data] module:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Sinds onze bemonsterde gegevens bevinden zich in de standaardcontainer omlaag, de resulterende query component van Azure Machine Learning is zeer eenvoudig en is het niet een "Selecteer * uit nyctaxidb.nyctaxi\_gedownsampled\_gegevens".

De dataset kan nu worden gebruikt als uitgangspunt voor het bouwen van Machine Learning modellen.

### <a name="mlmodel"></a>Modellen in Azure Machine leren bouwen

We kunnen nu verder met de modellen bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens zijn gereed om te gebruiken in de hierboven aangeduide voorspelling-problemen aanpakken:

**1. binaire indeling**: om te voorspellen of er een tip is betaald voor een reis.

**Cursist gebruikt:** Twee klasse logistische regressie

een. Voor dit probleem, is onze label doel (of klasse) 'Gekantelde'. Onze oorspronkelijke gedownsampled dataset heeft een paar kolommen die doel lekkage voor dit experiment classificatie zijn. In het bijzonder: tip\_klasse, tip\_bedrag en totaal\_bedrag onthullen informatie over het doellabel dat niet beschikbaar is op tijd te testen. We verwijderen deze kolommen rekening gehouden met de [Kolommen selecteren in de Dataset] [ select-columns] module.

De momentopname van het hieronder ziet u ons experiment om te voorspellen of een tip voor een bepaalde reis is betaald.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Ons doel label distributies zijn voor dit experiment, ongeveer 1:1.

De momentopname die hieronder wordt weergegeven hoe de tip klasse labels voor het probleem van de binaire indeling.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Hierdoor krijgen we een AUC van 0.987 zoals in de onderstaande afbeelding.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. indeling multiclass**: om te voorspellen van het bereik van de tip bedragen betaald voor de reis, met behulp van de eerder gedefinieerde klassen.

**Cursist gebruikt:** Multiclass logistische regressie

een. Ons doel (of les) label is voor dit probleem, "tip\_klasse" die een van de vijf waarden (0,1,2,3,4) kunt nemen. Net als bij de binaire indeling hebben we een paar kolommen lekkage doel voor dit experiment. In het bijzonder: Gekantelde, tip\_bedrag Totaal\_bedrag onthullen informatie over het doellabel dat niet beschikbaar is op tijd te testen. We verwijderen deze kolommen de [Kolommen selecteren in de Dataset] [ select-columns] module.

De momentopname van het hieronder ziet u ons experiment te voorspellen in welke opslaglocatie een tip dreigt te dalen (klasse 0: tip = $0, klasse 1: > $0 en tip tip < = $5, klasse 2: > $5 en tip tip < = $10, klasse 3: > $10 en tip tip < = $20, klasse 4: tip > $20)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

We zien nu onze werkelijke test klasse verdeling ziet eruit als. We zien dat klasse 0 en klasse 1 zijn bekende, de andere klassen komen zelden voor.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Voor dit experiment gebruiken we een matrix verwarring te kijken naar onze voorspelling accuratesse. Dit wordt hieronder weergegeven.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Houd er rekening mee dat de accuratesse van onze klasse op de gangbare klassen is heel goed, het model wordt niet goed werk van 'leren' op de zeldzame klassen.


**3. regressie taak**: om te voorspellen, het bedrag van de tip voor een reis betaald.

**Cursist gebruikt:** Beslissingsstructuur gestimuleerd

een. Ons doel (of les) label is voor dit probleem, "tip\_bedrag". Ons doel lekkage in dit geval zijn: Gekantelde, tip\_klasse totaal\_bedrag; alle deze variabelen geven informatie over de tip bedrag dat is meestal niet beschikbaar op het moment testen. We verwijderen deze kolommen de [Kolommen selecteren in de Dataset] [ select-columns] module.

De momentopname belows toont ons experiment te voorspellen, het bedrag van de tip gegeven.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Regressie-problemen meten we de accuratesse van onze voorspelling door te kijken naar de gekwadrateerde fout in de voorspellingen, de determinatiecoëfficiënt en dergelijke. We zien deze hieronder.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

We zien dat over de determinatiecoëfficiënt 0.709 is, wordt wat ongeveer 71% van de variantie door onze coëfficiënten model uitgelegd.

> [AZURE.IMPORTANT] Voor meer informatie over Azure Machine Learning en het openen en gebruiken, raadpleegt u [Wat is Machine Learning?](machine-learning-what-is-machine-learning.md). Een zeer nuttige informatiebron voor het afspelen van een aantal experimenten op Azure Machine Learning Machine Learning is de [Galerie met Cortana Intelligence](https://gallery.cortanaintelligence.com/). De galerie heeft betrekking op een gamma van experimenten en biedt een grondige Inleiding in het bereik van de mogelijkheden van Azure Machine leren.

## <a name="license-information"></a>Licentie-informatie

In dit scenario van het monster en de bijbehorende scripts worden gedeeld door Microsoft onder de MIT-licentie. Controleer of u het bestand LICENSE.txt in in de directory van de voorbeeldcode op GitHub voor meer informatie.

## <a name="references"></a>Verwijzingen

• [Andrés Monroy RDAM Taxi Trips pagina downloaden](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing van RDAM Taxi rit gegevens door Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taxi RDAM en Limousine Commissie onderzoek en statistieken](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
