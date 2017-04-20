<properties
    pageTitle="Het Team gegevens wetenschap proces in actie: met behulp van HDInsight Hadoop clusters op de dataset 1 TB Criteo | Microsoft Azure"
    description="Met behulp van het Team gegevens wetenschap proces voor een end-to-end scenario om een cluster HDInsight Hadoop kunnen bouwen en implementeren van een model met een groot publiek beschikbaar (1 TB)-dataset"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Het Team gegevens wetenschap proces in actie - Azure HDInsight Hadoop Clusters met behulp van een dataset 1 TB

In deze procedure wordt gedemonstreerd met het Team gegevens wetenschap proces in een end-to-end scenario met een [cluster Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) opslaan, verkennen, engineer, functie, en de voorbeeldgegevens uit een van de openbaar beschikbare [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datasets omlaag. Een binaire indeling model zijn gebaseerd op deze gegevens gebruiken we Azure Machine Learning. We zien ook hoe een van deze modellen te publiceren als een webservice.

Het is ook mogelijk met behulp van een laptop IPython taken in dit scenario wordt gepresenteerd. Het onderwerp [Criteo scenario een component een ODBC-verbinding met](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) gebruikers die u wilt proberen deze benadering dient te raadplegen.


## <a name="dataset"></a>Beschrijving van de Criteo Dataset

De Criteo gegevens is een klik voorspelling dataset die 370 GB gecomprimeerd gzip TSV-bestanden (niet-gecomprimeerd ~1.3TB), bestaande uit meer dan 4,3 miljard records. Het is afkomstig uit 24 dagen van klikt u op de beschikbare gegevens van [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). We hebben voor het gemak van wetenschappers van de gegevens zijn gegevens beschikbaar zijn voor ons om te experimenteren met uitgepakt.

Elke record in deze dataset bevat 40 kolommen:

- de eerste kolom is een labelkolom waarin wordt aangegeven of een gebruiker een **toevoegen** (waarde 1) of niet op een (waarde 0)
- volgende 13 kolommen zijn numeriek, en
- laatste 26 zijn kolommen bestaan

De kolommen zijn anoniem gemaakt en een reeks genummerde namen gebruiken: "Col1" (voor de labelkolom) te ' Col40 ' (voor de laatste kolom bestaan).            

Hier is een fragment van de eerste 20 kolommen van twee metingen (rijen) in deze dataset:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Ontbrekende waarden zijn in de kolommen numerieke en bestaan er in deze dataset. Een eenvoudige methode voor het verwerken van de ontbrekende waarden worden beschreven. Aanvullende details van de gegevens worden onderzocht wanneer ze in de tabellen van de component opgeslagen.

**Definitie:** *Doorklikken rate (CTR):* Dit is het percentage van de klikken in de gegevens. In deze dataset Criteo is de CTR ongeveer 3,3% of 0.033.

## <a name="mltasks"></a>Voorbeelden van taken die voorspelling
Twee monster voorspelling problemen komen aan bod in dit overzicht:

1. **Binaire indeling**: voorspelt of een gebruiker toevoegen geklikt:
    - Klasse 0: Geen Klik hier
    - Klasse 1: klik op

2. **Regressie**: voorspelt de kans van een ad-klik van functies.


## <a name="setup"></a>Instellen van een HDInsight Hadoop cluster voor wetenschappelijke gegevens

**Opmerking:** Dit is meestal een **Admin** -taak.

Stel uw omgeving Azure gegevens wetenschap voor anticiperende analytische oplossingen met HDInsight clusters in drie stappen:

1. [Een opslag-account maken](../storage/storage-create-storage-account.md): deze opslag rekening wordt gebruikt voor het opslaan van gegevens in Azure Blob-opslag. De gegevens die worden gebruikt in clusters HDInsight wordt hier opgeslagen.

2. [Azure HDInsight Hadoop Clusters aanpassen voor Data Science](machine-learning-data-science-customize-hadoop-cluster.md): deze stap maakt u een cluster Azure HDInsight Hadoop met 64-bits Anaconda Python 2.7 hebben geïnstalleerd op alle knooppunten. Er zijn twee belangrijke stappen (in dit onderwerp beschreven) bij het aanpassen van het cluster HDInsight voltooien.

    * De opslag-account gemaakt in stap 1 met het cluster HDInsight wanneer deze wordt gemaakt, moet u koppelen. Deze opslag rekening wordt gebruikt voor toegang tot gegevens die binnen het cluster kan worden verwerkt.

    * Aan het hoofd knooppunt van het cluster moet u externe toegang inschakelen nadat deze is gemaakt. Vergeet niet de RAS-referenties die u hier opgeeft (anders dan die voor het cluster op het werd gemaakt): u moet de volgende procedures uitvoeren.

3. [Een werkruimte Azure ML maken](machine-learning-create-workspace.md): deze Azure Machine Learning werkruimte wordt gebruikt voor het bouwen van machine learning modellen na de initiële gegevens verkennen en omlaag bemonstering op het cluster HDInsight.

## <a name="getdata"></a>Ophalen en verwerken van gegevens uit een openbare bron

De dataset [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) toegankelijk door te klikken op de koppeling, de gebruiksvoorwaarden accepteren en een naam geven. Hier ziet u een momentopname van hoe dit eruitziet:

![Criteo voorwaarden accepteren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Klik op **Doorgaan om te downloaden** voor meer informatie over de beschikbaarheid en de dataset.

De gegevens bevinden zich in een openbare locatie voor [Azure blob-opslag](../storage/storage-dotnet-how-to-use-blobs.md) : wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. De "wasb" verwijst naar de locatie van Azure Blob-opslag. 

1. De gegevens in deze openbare blob-opslag bestaat uit drie submappen van gegevens uitgepakt.

    1. De submap *Onbewerkte telling / /* bevat de eerste 21 dagen van de gegevens - dag\_00 dag\_20
    2. De submap *raw/trein/* bestaat uit een enkele dag van gegevens, dag\_21
    3. De submap *raw/test/* bestaat uit twee dagen met gegevens, dag\_22 en dag\_23

2. Voor degenen die willen beginnen met de gzip onbewerkte gegevens, deze zijn ook beschikbaar in de map *raw /* als day_NN.gz, waarbij NN van 00 tot en met 23 gaat.

Een alternatieve benadering voor verkennen en model deze gegevens die niet te downloaden van een lokale hoeft verderop in dit scenario wordt uitgelegd wanneer we component tabellen maken.

## <a name="login"></a>Log in op de cluster headnode

Gebruik voor het aanmelden bij de headnode van het cluster, de [Azure portal](https://ms.portal.azure.com) te vinden van het cluster. Klik op het pictogram van de olifant HDInsight aan de linkerkant en dubbelklik vervolgens op de naam van het cluster. Ga naar het tabblad **configuratie** , dubbelklik op het pictogram verbinding maken met aan de onderkant van de pagina en voer uw RAS-referenties. Hiermee gaat u naar de headnode van het cluster.

Dit is een typische eerste Meld u aan op de cluster headnode ziet eruit als:

![Log in op een cluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Aan de linkerkant zien we de "Hadoop opdrachtregel", die onze werkpaard voor het onderzoek van de gegevens is. We zien ook twee nuttige URL's - "Status Hadoop garens" en "Hadoop naam knooppunt". URL van het garen geeft de voortgang van het project en de naam knooppunt URL krijgt informatie over de configuratie van het cluster.

Nu we zijn ingesteld en klaar om te beginnen met het eerste gedeelte van de procedure: verkennen met behulp van de component en voorbereiden gegevens Azure Machine Learning.

## <a name="hive-db-tables"></a>Component-database en tabellen maken

Component om tabellen te maken voor onze Criteo dataset, opent u de ***opdrachtregel Hadoop*** op het bureaublad van het knooppunt head en invoeren van de map component door te voeren van de opdracht

    cd %hive_home%\bin

>[AZURE.NOTE] Alle component-opdrachten uitvoeren in dit scenario uit de opslaglocatie component / directory prompt. Dit zorgt voor problemen op pad automatisch. Gebruiken we de termen 'Component directory prompt', ' component bin / directory prompt ", en" Hadoop opdrachtregel "door elkaar.

>[AZURE.NOTE]  Elke component als query wilt uitvoeren, kan een altijd de volgende opdrachten gebruiken:

        cd %hive_home%\bin
        hive

Nadat de REPL-component wordt weergegeven met een ' component > 'ondertekenen, gewoon knippen en plakken de query wilt uitvoeren.

De volgende code maakt een database "criteo" en genereert vervolgens 4 tabellen:


* een *tabel voor het genereren van de aantallen* gebaseerd op dag dagen\_00 dag\_20,
* een *tabel gebruiken als de trein dataset* gebouwd op dag\_21, en
* twee *tabellen voor gebruik als de datasets test* gebaseerd op dag\_22 en dag\_23 respectievelijk.

We onze dataset test opgesplitst in twee verschillende tabellen omdat een van de dagen een feestdag is, en we willen graag zien als het model verschillen tussen een vakantie en niet vanuit de Doorklik-frequentie kunt detecteren.

Het script [monster & #95; component & #95; maken & #95; criteo & #95; database & #95; en & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) hier voor het gemak wordt weergegeven:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

We er rekening mee dat deze tabellen alle externe als we naar locaties Azure Blob-opslag (wasb verwijzen).

**Er zijn twee manieren component van een query waarin we nu uit te voeren.**

1. **Met behulp van de component REPL opdrachtregel**: de eerste is een opdracht "component" en kopiëren en plakken van een query op de component REPL vanaf de opdrachtregel. Voer hiertoe:

        cd %hive_home%\bin
        hive

    Nu op de opdrachtregel REPL knippen en plakken van de query wordt uitgevoerd het.

2. **Query's naar een bestand opslaan en uitvoeren van de opdracht**: de tweede is de query's opslaan in een bestand .hql ([monster & #95; component & #95; maken & #95; criteo & #95; database & #95; en & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) en vervolgens de volgende opdracht de query uit te voeren:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Bevestigen van de database en tabel maken

Vervolgens na het maken van de database met de volgende opdracht uit de opslaglocatie component bevestiging / directory prompt:

        hive -e "show databases;"

Dit geeft:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Dit bevestigt het maken van de nieuwe database, "criteo".

Als u wilt zien welke tabellen die we hebben gemaakt, we gewoon geeft de opdracht uit de opslaglocatie component / directory prompt:

        hive -e "show tables in criteo;"

Vervolgens zien we de volgende uitvoer:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>In component verkennen

We zijn nu enkele basisgegevens exploratie in component uitvoeren. We beginnen met het tellen van het aantal voorbeelden in de trein en test u gegevenstabellen.

### <a name="number-of-train-examples"></a>Aantal voorbeelden van de trein

De inhoud van het [monster & #95; component & #95; & #95 tellen; trein & #95; tabel & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) worden hieronder weergegeven:

        SELECT COUNT(*) FROM criteo.criteo_train;

Dit levert:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Ook kan een ook de volgende opdracht uit de opslaglocatie component afgeven / directory prompt:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Aantal voorbeelden in de twee test datasets test

We nu aantal het voorbeelden in de twee test datasets. De inhoud van [monster & #95; component & #95; & #95 tellen; criteo & #95; test & #95; dag & #95; 22 & #95; tabel & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) zijn hier:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Dit levert:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Zoals gebruikelijk, kunnen ook noemen we het script uit de opslaglocatie component / Active directory prompt door de opdracht:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Ten slotte, wij onderzoeken het aantal test voorbeelden in de dataset test is gebaseerd op een dag\_23.

De opdracht om dit te doen is vergelijkbaar met de zojuist aangegeven (Zie [voorbeeld & #95; component & #95; & #95 tellen; criteo & #95; test & #95; dag & #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Dit geeft:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Label distributie in de dataset trein

De verdeling van het label in de trein dataset is van belang. Om dit te zien, geven wij inhoud van het [monster & #95; component & #95; criteo & #95; & #95 label; distributie & #95; trein & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Dit geeft de verdeling van de label:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Houd er rekening mee dat het percentage positieve etiketten ongeveer 3,3% (consistent met de oorspronkelijke gegevensset is).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogram verdelingen van sommige numerieke variabelen in de dataset trein

Gebruiken we component van native ' histogram\_numerieke ' functie om erachter te komen hoe de verdeling van de numerieke variabelen eruit ziet. Hier vindt u de inhoud van het [monster & #95; component & #95; criteo & #95; histogram & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Dit levert het volgende:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

De DWARSKRACHTEN BEELD - combinatie in de component fungeert voor de productie van een SQL-achtige output in plaats van de gebruikelijke lijst exploderen. Merk op dat in deze tabel, eerste kolom komt overeen met het midden van de opslaglocatie en de tweede naar de frequentie van de opslaglocatie.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Bij benadering percentielen van sommige numerieke variabelen in de dataset trein

Ook is van belang met numerieke variabelen de berekening van de geschatte percentielen. Component de eigen "percentiel\_ongeveer" doet dit voor ons. De inhoud van [monster & #95; component & #95; criteo & #95; bij benadering & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) zijn:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Dit levert:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

We remark dat de verdeling van percentielen nauw is aan de verdeling van het histogram van een willekeurige numerieke variabele meestal verwant.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Het aantal unieke waarden voor sommige kolommen bestaan in de dataset trein zoeken

De gegevens verkennen door te gaan zoeken we nu, voor sommige kolommen bestaan het aantal unieke waarden die zij nemen. Hiertoe geven wij inhoud van [monster & #95; component & #95; criteo & #95; unieke & #95; waarden & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Dit levert:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

We u ziet dat Col15 19M unieke waarden! Met technieken zoals 'one hot codering' naïef is om te coderen dergelijke hoge dimensionale bestaan variabelen onoplosbaar. In het bijzonder we uitleggen en demonstreren van een krachtige, robuuste techniek [Leren met telt](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) aangeroepen voor het aanpakken van dit probleem efficiënt.

We beëindigen kijken naar het aantal unieke waarden voor sommige andere bestaan kolommen ook deze subsectie. De inhoud van [monster & #95; component & #95; criteo & #95; unieke & #95; waarden & #95; meerdere & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) zijn:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Dit levert:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Opnieuw zien we dat, met uitzondering van Col20, de kolommen veel unieke waarden hebben.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Collega exemplaar telt paren van het bestaan van de variabelen in de dataset trein

De tellingen collega exemplaar uit combinaties van variabelen bestaan is ook van belang. Dit kan worden bepaald met behulp van de code in [monster & #95; component & #95; criteo & #95; gepaarde & #95; bestaan & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

We reverse hun exemplaar van de tellingen sorteren en zoeken in dit geval boven de 15. Dit geeft ons:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Naar voorbeeld van de datasets voor Azure Machine Learning

Met de datasets onderzocht en aangetoond hoe we mogelijk kan dit type van alle variabelen (inclusief combinaties) we nu omlaag monster exploratie de gegevenssets zodat wij modellen in Azure Machine Learning kunt bouwen. Let erop dat het probleem dat we richten ons op: gegeven een reeks voorbeeld-kenmerken (de functie waarden uit Kol2 - Col40), wij voorspellen of Col1 0 (geen klik) of 1 (Klik hier is).

Omlaag monster onze trein en datasets op 1% van de oorspronkelijke grootte testen, gebruiken we de functie RAND() native van de component. Het volgende script [voorbeeld & #95; component & #95; criteo & #95; downsamplen & #95; trein & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) doet dit voor de trein dataset:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Het script [voorbeeld & #95; component & #95; criteo & #95; downsamplen & #95; test & #95; dag & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) doet dit allemaal voor testgegevens, dag\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Ten slotte het script [voorbeeld & #95; component & #95; criteo & #95; downsamplen & #95; test & #95; dag & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) doet dit allemaal voor testgegevens, dag\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Dit zijn gereed voor gebruik van onze omlaag bemonsterde trein en datasets voor het maken van modellen in Azure Machine Learning test.

Er is een belangrijk instrument voor de uiteindelijke voordat we gaan Azure Machine Learning, die betrekking heeft op de tabel tellen. In het volgende gedeelte van de onderliggende besproken dit in sommige details.

##<a name="count"></a>Een korte bespreking van de tabel tellen

We hebben gezien hebben bestaan die verscheidene variabelen een zeer hoge dimensionaliteit. In ons scenario presenteren we een krachtige techniek genoemd [Met telt leren](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) om deze variabelen op efficiënte en krachtige wijze te coderen. Meer informatie over deze techniek is in de koppeling.

**Opmerking:** In dit scenario richten we ons op het aantal tabellen gebruiken voor de productie van compacte representaties van functies voor hoge dimensionale bestaan. Dit is niet de enige manier om te coderen functies bestaan; voor meer informatie over andere technieken belanghebbende gebruikers kunnen uitchecken [een-hot-encoding](http://en.wikipedia.org/wiki/One-hot) en [hash-functie](http://en.wikipedia.org/wiki/Feature_hashing).

We gebruiken de gegevens in de map raw/aantal tabellen telling van het aantal gegevens bouwen. In de sectie modelleren we zien gebruikers het aantal tabellen bestaan functies maken, voor het bouwen of u kunt ook een aantal vooraf gemaakte tabel gebruiken voor hun explorations. In wat volgt, wanneer we verwijzen naar 'het aantal vooraf gemaakte tabellen', bedoelen we het aantal tabellen die we aanbieden. In de volgende sectie vindt u gedetailleerde instructies voor het openen van deze tabellen.

## <a name="aml"></a>Bouwen van een model met Azure Machine Learning

Ons model opbouwproces in Azure Machine Learning verloopt als volgt:

1. [De gegevens ophalen uit tabellen component Azure Machine Learning](#step1)
2. [Maak het experiment: opschonen van de gegevens, kiest u een cursist en featurize met het aantal tabellen](#step2)
3. [Het model van de trein](#step3)
4. [Het model van testgegevens score](#step4)
5. [Het model te evalueren](#step5)
6. [Het model als een webservice worden verbruikt publiceren](#step6)

We zijn nu gereed om modellen in Azure Machine Learning studio te maken. Onze omlaag bemonsterde gegevens worden opgeslagen als onderdeel van tabellen in het cluster. We gebruiken de module Azure Machine Learning **Gegevens importeren** om deze gegevens te lezen. De referenties voor toegang tot de account van de opslag van deze cluster vindt u in het volgende.

### <a name="step1"></a>Stap 1: Gegevens ophalen uit tabellen component Azure Machine leren met behulp van de module gegevens importeren en selecteer het voor een machine learning experiment

Begin met het selecteren van een **+ Nieuw** -> **EXPERIMENT** -> **Blanco-Experiment**. Klik in **het zoekvak linksboven op** zoeken "Gegevens importeren". Slepen en neerzetten van de module **Gegevens importeren** naar het canvas experiment (het middelste gedeelte van het scherm) om de module te gebruiken voor toegang tot gegevens.

Dit is hoe de **Gegevens importeren** tijdens het ophalen van gegevens uit de tabel component eruitziet:

![Gegevens importeren, gegevens haalt](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

De waarden van de parameters die beschikbaar zijn in de afbeelding zijn voor de module **Gegevens importeren** , maar een voorbeeld van het soort waarden die u moet opgeven. Hier ziet u enkele algemene richtlijnen voor het invullen van de parameter is ingesteld voor de module **Gegevens importeren** .

1. Kies 'Component query' voor **Gegevensbron**
2. In de **component databasequery** een eenvoudige SELECT * FROM < uw\_database\_name.your\_tabel\_naam >-voldoende is.
3. **Hcatalog server-URI**: als uw cluster 'abc' is, is dit gewoon: https://abc.azurehdinsight.net
4. **Naam van gebruikersaccount Hadoop**: de gebruikersnaam die is gekozen op het tijdstip van ingebruikneming van het cluster. (Niet de RAS-gebruikersnaam!)
5. **Hadoop wachtwoord**: het wachtwoord voor de gebruikersnaam die is gekozen op het tijdstip van ingebruikneming van het cluster. (Niet het wachtwoord voor externe toegang!)
6. **Locatie van uitvoergegevens**: kies "Azure"
7. **Azure opslag accountnaam**: het opslag-account die is gekoppeld aan het cluster
8. **Azure opslag account sleutel**: de sleutel van de opslag-account die is gekoppeld aan het cluster.
9. **Azure containernaam**: als de clusternaam is "abc", is dit gewoon "abc", meestal.


Nadat u de **Gegevens importeren** is voltooid (u ziet de groene maatstreepjes op de Module) gegevens ophalen, deze gegevens opslaan een Dataset (met een naam van uw keuze). Hoe dit eruitziet:

![Gegevens importeren, gegevens opslaan](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Met de rechtermuisknop op de poort van de output van de module **Gegevens importeren** . Dit blijkt een optie **Opslaan als dataset** en een **visualiseren** . De optie **visualiseren** wordt als erop wordt geklikt, 100 rijen van de gegevens, samen met een rechter deelvenster dat geschikt is voor sommige samenvattingsinfo. Als gegevens wilt opslaan, selecteert u **Opslaan als dataset** en volg de instructies.

Schakel de dataset opgeslagen voor gebruik in een experiment machine learning, zoek de datasets met het vak **Zoeken** in de volgende afbeelding wordt weergegeven. Typ vervolgens de naam die u gegeven dataset gedeeltelijk te openen en sleept u de dataset naar het hoofdpaneel. Slepen en neerzetten naar het hoofdpaneel geselecteerd voor gebruik in de modellering van machine learning.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] Doe dit voor zowel de trein en de datasets test. Vergeet ook niet de naam en de namen van de tabellen die u hebt opgegeven voor dit doel gebruiken. De waarden die in de afbeelding worden gebruikt, dienen uitsluitend ter illustratie purposes.* *

### <a name="step2"></a>Stap 2: Maak een eenvoudige experiment in Azure Machine Learning muisklikken voorspellen / geen klikken

Ons experiment Azure ML ziet er zo uit:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

We bekijken nu de belangrijkste onderdelen van dit experiment. Ter herinnering: we moeten onze opgeslagen trein sleept en datasets bij ons experiment canvas eerst testen.

#### <a name="clean-missing-data"></a>De ontbrekende gegevens opschonen

De module **Schoon ontbrekende gegevens** doet wat de naam al aangeeft: het opschonen van ontbrekende gegevens op een manier die de gebruiker opgegeven kunnen worden. In deze module bekijken, zien we dit:

![Schone ontbrekende gegevens](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Hier hebt alle ontbrekende waarden vervangen door een 0. Er zijn andere opties, die kunnen worden gezien door te kijken in de vervolgkeuzelijsten in de module.

#### <a name="feature-engineering-on-the-data"></a>Functie-engineering op de gegevens

Er zijn miljoenen unieke waarden voor bepaalde functies van het bestaan van grote gegevenssets. Met behulp van methoden zoals een hot-codering voor dergelijke hoge dimensionale functies bestaan die naïef is volledig onoplosbaar. In deze procedure gedemonstreerd aantal functies met behulp van ingebouwde Azure Machine Learning modules voor het genereren van compact representaties van deze hoge dimensionale variabelen bestaan. Het eindresultaat is een kleiner model, snellere tijden van training en prestatiemetingen die zeer vergelijkbaar zijn met behulp van andere technieken.

##### <a name="building-counting-transforms"></a>Gebouw transformaties tellen

Als u het aantal functies, gebruiken we de module **Bouwen tellen transformeren** , die beschikbaar is in Azure Machine Learning. De module ziet er zo uit:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Belangrijke opmerking** : we Geef In het vak **aantal kolommen** de kolommen die we voeren tellingen willen op. Dit zijn (zoals vermeld) hoge dimensionale kolommen bestaan. Bij het begin, gezegd dat de dataset Criteo 26 bestaan kolommen bevat: van Col15 naar Col40. Hier, we tellen op al deze en geef de indexcijfers (van 15 tot 40, gescheiden door komma's, zoals wordt weergegeven).

Gebruik van de module in de modus MapReduce (geschikt voor grote gegevenssets), moeten we de toegang tot een cluster HDInsight Hadoop (gebruikt voor het verkennen van de functie opnieuw kan worden gebruikt voor dit doel ook) en de referenties. De vorige cijfers illustreren de ingevulde waarden eruit (vervangt u de afbeelding met die relevant zijn voor uw eigen gebruiksvoorbeeld vervatte waarden).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

We zien hoe de invoer blob locatie invoeren in de afbeelding. Deze locatie heeft de gegevens die zijn gereserveerd voor het aantal tabellen op te maken.


Nadat deze module is voltooid, kunnen we de transformatie voor later opslaan door met de rechtermuisknop op de module en de optie **Opslaan als een transformatie** te selecteren:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

In onze architectuur experiment hierboven de dataset 'ytransform2' overeenkomt met precies een transformatie opgeslagen aantal. Voor de rest van dit experiment, nemen we aan dat de lezer een module **Bouwen tellen transformeren** op bepaalde gegevens gebruikt voor het genereren van tellingen en deze tellingen kunt aantal functies op de trein te genereren en datasets test.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Kiezen welk aantal functies om op te nemen als onderdeel van de trein en test datasets

Zodra we een aantal klaar te zetten hebben, kan de gebruiker kiezen welke functies wilt opnemen in de trein en testen met behulp van de module **Tellen de Parameters wijzigen** datasets. We gewoon te laten zien in deze module hier voor de volledigheid, maar in het belang van de eenvoud daadwerkelijk gebruik deze niet bij ons experiment.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

In dit geval zoals u kunt zien, hebt we gekozen voor het gebruik van alleen de logboek-kans en negeert de achterkant uit kolom. We kunnen ook parameters zoals de garbage bin drempel, hoeveel pseudo voorafgaande voorbeelden toe te voegen voor het vloeiend maken en of u alle Laplacian ruis of niet instellen. Zijn deze geavanceerde functies en is op gewezen dat de standaardwaarden zijn een goed uitgangspunt voor gebruikers die een nieuwe functie voor het genereren van dit type zijn.

##### <a name="data-transformation-before-generating-the-count-features"></a>Gegevenstransformatie voordat u de functies count genereert

Nu we concentreren op een belangrijk punt over onze trein te transformeren en gegevens testen voorafgaand aan het aantal functies daadwerkelijk te genereren. Opmerking Er zijn twee **R Script uitvoeren** modules gebruikt voordat we de telling transformatie op onze gegevens toepassen.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Dit is de eerste R-script:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


In dit script R we de naam van onze columns namen 'Col1' naar 'Col40'. Dit komt omdat de telling transformatie verwacht dat de namen van deze indeling.

In het tweede R-script, we de verdeling tussen klassen van positieve en negatieve saldo (respectievelijk klassen 1 en 0) door de negatieve klasse downsampling. Het R-script Hier ziet u hoe u dit doet:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

In dit eenvoudige R-script gebruiken we "pos\_neg\_ratio" instellen van het bedrag van de balans tussen de positieve en negatieve klassen. Dit is belangrijk omdat klasse onevenwicht meestal verbeteren prestatievoordelen voor indeling problemen waar de verdeling van de klasse is scheef (terughalen dat in ons geval, we 3,3% positieve klasse en 96,7% negatieve klasse hebben).

##### <a name="applying-the-count-transformation-on-our-data"></a>Het aantal transformatie toepassen op onze gegevens

We kunnen ten slotte de module **Transformatie toepassen** gebruiken het aantal transformaties toepassen op onze trein en datasets test. In deze module wordt de transformatie opgeslagen tellen als één invoer en de datasets trein of test als de andere invoer en gegevens met de functies count retourneert. Hier wordt weergegeven:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Een uittreksel van welke het aantal functies eruit

Het is leerzaam om te zien hoe het aantal functies in ons geval eruit. We zien hier een fragment van dit:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

In dit fragment zien we dat voor de kolommen die we op geteld, we de tellingen krijgen en meld u kans naast alle relevante backoffs.

We zijn nu klaar om te bouwen van een Azure Machine Learning-model met behulp van deze getransformeerde datasets. We zien hoe dit kan worden gedaan in de volgende sectie.

#### <a name="azure-machine-learning-model-building"></a>Azure Machine Learning modellen bouwen

##### <a name="choice-of-learner"></a>Keuze van de cursist

Eerst moet een cursist kiezen. We gaan een beslissingsstructuur twee klasse versterkt als onze cursist gebruiken. Hier vindt u de standaardopties voor deze cursist:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Voor ons experiment gaan we de standaardwaarden te kiezen. We er rekening mee dat de standaardinstellingen doorgaans zinvolle en een goede manier om snelle basislijnen op de prestaties. U kunt op de prestaties verbeteren door de verstrekkende parameters als u zodra u een basislijn hebt.

#### <a name="train-the-model"></a>Het model van de trein

Voor training roepen we gewoon een **Model trein** -module. De twee ingangen aan zijn de twee klasse versterkt beslissingsstructuur cursist en onze trein dataset. Dit wordt hier weergegeven:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Score van het model

Als we een ervaren model hebt, gaan we score op de dataset testen en evalueren van de prestaties. Dit doen we met behulp van de module **Score-Model** is weergegeven in de volgende afbeelding, samen met een module **Model beoordelen** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Stap 5: Evaluatie van het model

Ten slotte willen we model prestaties analyseren. Meestal is een goede indicatie voor twee klasse-indeling (binair) problemen, de AUC. Om dit, koppelen we de module **Score-Model** naar een module **Evalueren Model** voor deze. Te klikken op **visualiseren** in de module **Evalueren Model** levert een afbeelding zoals de volgende:

![Module BDT model evalueren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

In de binaire (of twee klasse) problemen met de indeling, een goede indicatie van de nauwkeurigheid van de voorspelling is het gebied onder Curve (AUC). In wat volgt, zien we onze resultaten met behulp van dit model op onze test dataset. Als u dit, met de rechtermuisknop op de poort van de output van de module **Model evalueren** en **visualiseren**.

![Module evaluatie van Model visualiseren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Stap 6: Het model als een webservice publiceren
De mogelijkheid voor het publiceren van een model Azure Machine Learning als webservices met een minimum van fuss is een zeer nuttige functie voor algemeen beschikbaar te maken. Zodra u dat hebt gedaan, kan iedereen bellen naar de webservice met de invoergegevens die zij voorspellingen voor nodig hebt, en de webservice het model gebruikt om terug te keren die voorspellingen.

Hiertoe we eerst onze opgeleide model opslaan als een object opgeleide Model. Dit wordt gedaan met de rechtermuisknop op het **Model van de trein** -module met behulp van de optie **Opslaan als ervaren Model** .

Vervolgens moet maken van input en output poorten voor onze web-service:

* een invoerpoort nodig gegevens in hetzelfde formulier als de gegevens die wij nodig voorspellingen voor
* een uitvoerpoort retourneert de Labels gescoord en de kansen.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Selecteer een paar rijen met gegevens voor de invoerpoort

Is het handig te fungeren als de invoerpoort gegevens slechts 10 rijen selecteren met een module **SQL transformatie toepassen** . Selecteer alleen deze rijen met gegevens voor onze invoerpoort met behulp van de SQL-query die hier worden weergegeven:

![Gegevens van de invoerpoort](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Web-service
We kunnen nu een klein experiment dat kan worden gebruikt voor het publiceren van onze web-service wordt uitgevoerd.

#### <a name="generate-input-data-for-webservice"></a>Genereren van gegevens die zijn ingevoerd voor de webservice

Als een stap zeroth omdat de tabel tellen groot is, we nemen een paar regels van testgegevens en uitvoergegevens genereren uit met een aantal functies. Dit kan dienen als de indeling van de ingevoerde gegevens voor de webservice. Dit wordt hier weergegeven:

![Invoergegevens BDT maken](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Nu gebruiken we voor de indeling van de ingevoerde gegevens, de uitvoer van het **Aantal Featurizer** -module. Zodra dit experiment is voltooid, de uitvoer opslaan vanuit de module **Featurizer tellen** als een Dataset. Deze Dataset wordt gebruikt om de ingevoerde gegevens in de webservice.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Experiment scoren voor publiceren webservice

We zien eerst hoe dit er uitziet. De essentiële structuur is een **Score-Model** module waarin onze opgeleide model-object en een paar regels van invoergegevens die we in de vorige stappen met behulp van de module **Tellen Featurizer** gegenereerd. Project Scored etiketten en de Score kansen gebruiken we "Kolommen in Dataset selecteren".

![Selecteer de kolommen in de Dataset](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

U ziet hoe de **Kolommen selecteren in een Dataset** -module kan worden gebruikt voor het 'uitgefilterd' gegevens uit een dataset. We zien hier de inhoud:

![Filteren met de kolommen selecteren in een Dataset-module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Als u de blauwe input en output poorten, klikt u op **webservice voorbereiden** rechts onder. Dit experiment uitgevoerd ook biedt ons de mogelijkheid om te publiceren van de webservice: klik op het pictogram **Publiceren WEBSERVICE** rechts en onder die hier worden weergegeven:

![Publiceren van webservice](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Als de webservice is gepubliceerd, krijgen we omgeleid naar een pagina die er zo uitziet:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Aan de linkerkant zien we twee koppelingen voor webservices:

* De **Aanvraag/reactie** -Service (of records) is bedoeld voor één voorspellingen en we gebruiken in deze workshop is.
* **BATCH EXECUTION** Service (BES) wordt gebruikt voor batch voorspellingen en vereist dat de ingevoerde gegevens gebruikt om voorspellingen bevinden zich in Azure Blob-opslag.

Code in C#, python en R. blik vooraf te klikken op de link **Die aanvraag/reactie** leidt ons naar een pagina die ons geeft Deze code kan eenvoudig worden gebruikt voor het aanroepen van de webservice. Houd er rekening mee dat de API-sleutel op deze pagina moet worden gebruikt voor verificatie.

Is het handig deze python code via kopiëren naar een nieuwe cel in de IPython-laptop.

We zien hier een segment van python code met de juiste API-sleutel.

![Python code](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Houd er rekening mee dat we de standaard API sleutel door van onze webservices API-sleutel vervangen. Te klikken op **uitvoeren** in deze cel in een notitieblok IPython levert het volgende antwoord:

![IPython-reactie](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

We zien dat voor twee proef voorbeelden we over (in het kader van de JSON van de python script vroegen), wij terug in de vorm antwoorden 'Scored Labels, Scored kansen'. Houd er rekening mee dat in dit geval, wij hebben gekozen voor de standaardwaarden die vooraf Ingeblikte biedt (0 voor alle numerieke kolommen en de tekenreeks "waarde" voor alle kolommen voor bestaan).

Dit concludeert onze end-to-end scenario met het afhandelen van grootschalige dataset met Azure Machine Learning. We begonnen met een terabyte van gegevens, een voorspellingsmodel gebouwd en gedistribueerd als een webservice in de cloud.
