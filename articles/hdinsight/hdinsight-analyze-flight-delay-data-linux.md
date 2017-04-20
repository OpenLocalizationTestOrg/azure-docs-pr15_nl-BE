<properties 
    pageTitle="Vlucht vertraging gegevens analyseren met de component op Linux-gebaseerde HDInsight | Microsoft Azure" 
    description="Leren werken met component vluchtgegevens op Linux-gebaseerde HDInsight analyseren en vervolgens de gegevens exporteren naar een SQL-Database met behulp van Sqoop." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Vlucht vertraging gegevens analyseren met behulp van de component in HDInsight

Meer informatie over het analyseren van vlucht vertraging gegevens met behulp van de component op Linux-gebaseerde HDInsight en vervolgens de gegevens exporteren naar Azure SQL-Database met behulp van Sqoop.

> [AZURE.NOTE] Terwijl de individuele onderdelen van dit document kunnen worden gebruikt met HDInsight voor Windows-gebaseerde clusters (Python en component bijvoorbeeld) veel stappen zijn specifiek voor Linux gebaseerde clusters. Zie voor instructies die met een Windows-gebaseerde clusters werkt [analyseren vertraging vluchtgegevens met behulp van de component in HDInsight](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Een HDInsight-cluster__. Zie [aan de slag met Hadoop met de component in HDInsight op Linux](hdinsight-hadoop-linux-tutorial-get-started.md) voor stapsgewijze instructies voor het maken van een nieuwe HDInsight op basis van Linux-cluster.

- __Azure SQL-Database__. U gebruikt een Azure SQL-database als gegevensarchief bestemming. Als u een SQL-Database niet al hebt, raadpleegt u [zelfstudie voor SQL-Database: een SQL-database maken in minuten](../sql-database/sql-database-get-started.md).

- __Azure CLI__. Zie als u de CLI Azure niet hebt geïnstalleerd, [Installeer en configureer de CLI Azure](../xplat-cli-install.md) voor meer stappen.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>De vluchtgegevens downloaden

1. Ga naar [onderzoek en innovatieve technologie voor beheer, Bureau of Transportation Statistics][rita-website].
2. Selecteer op de pagina de volgende waarden:

  	| Naam | Waarde |
  	| ---- | ---- |
  	| Jaar filter | 2013 |
  	| Periode filteren | Januari |
  	| Velden | Jaar, FlightDate, UniqueCarrier, vervoerders, FlightNum, OriginAirportID, oorsprong, OriginCityName, OriginState, DestAirportID, doel, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Alle andere velden wissen |

3. Klik op **downloaden**. 

##<a name="upload-the-data"></a>De gegevens uploaden

1. Gebruik de volgende opdracht in het zip-bestand uploaden naar het hoofd clusterknooppunt van HDInsight:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    __Bestandsnaam__ vervangen door de naam van het zip-bestand. Vervang __gebruikersnaam__ met de SSH-aanmelding voor het cluster HDInsight. CLUSTERNAAM vervangen door de naam van het cluster HDInsight.
    
    > [AZURE.NOTE] Als u een wachtwoord gebruikt voor verificatie van uw SSH-aanmelding, wordt u gevraagd om het wachtwoord. Als u een openbare sleutel gebruikt, moet u mogelijk gebruik van de `-i` parameter en geeft u het pad naar de bijbehorende persoonlijke sleutel. Bijvoorbeeld `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Nadat het uploaden is voltooid, kunt u verbinding met het cluster via SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Raadpleeg de volgende artikelen voor meer informatie over het gebruik van SSH in Linux-gebaseerde HDInsight:
    
    * [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Zodra verbonden, gebruikt u de volgende Pak het ZIP-bestand:

        unzip FILENAME.zip
    
    Dit wordt opgehaald door een CSV-bestand dat is ongeveer 60MB groot.
    
4. Gebruik het volgende om een nieuwe map maken op WASB (de gedistribueerde gegevensopslag gebruikt door HDInsight) en kopieer het bestand:

    hdfs dfs - mkdir -p /tutorials/flightdelays/data hdfs dfs-put FILENAME.csv zelfstudies/flightdelays/data /
    
##<a name="create-and-run-the-hiveql"></a>Maken en uitvoeren van de HiveQL

Gebruik de volgende stappen uit om gegevens te importeren uit het CSV-bestand in een component-tabel met de naam __vertragingen__.

1. Gebruik de volgende maken en bewerken van een nieuw bestand met de naam __flightdelays.hql__:

        nano flightdelays.hql
        
    Als de inhoud van dit bestand, gebruikt u het volgende:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Gebruik __Ctrl + X__en __Y__ in het bestand wilt opslaan.

3. Gebruik de volgende component starten en uitvoeren van het bestand __flightdelays.hql__ :

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] In dit voorbeeld wordt `localhost` wordt gebruikt omdat u bent verbonden met het hoofd knooppunt van het cluster HDInsight, dat is waar HiveServer2 wordt uitgevoerd.

4. Gebruik de volgende opdracht als u een interactieve sessie met Beeline te openen:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Wanneer u ontvangt de `jdbc:hive2://localhost:10001/>` wordt gevraagd, gebruikt u de volgende gegevens op te halen uit de gegevens van geïmporteerde vlucht vertraging.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Dit wordt een lijst van steden die vertragingen weer, samen met de gemiddelde vertraging geconstateerd ophalen en opslaan op `/tutorials/flightdelays/output`. Sqoop zal later de gegevens lezen van deze locatie en exporteren naar Azure SQL-Database.

6. Invoeren om af te sluiten Beeline `!quit` bij de opdrachtprompt.

## <a name="create-a-sql-database"></a>Een SQL-Database maken

Als u al een SQL-Database, kunt u de naam van de server moet ophalen. U vindt dit in de [Portal Azure](https://portal.azure.com) __SQL-Databases__te selecteren en vervolgens filteren op de naam van de database die u wilt gebruiken. De naam van de server wordt vermeld in de kolom __SERVER__ .

Als u nog geen SQL-Database, gebruikt u de informatie in [zelfstudie voor SQL-Database: een SQL-database maken in minuten](../sql-database/sql-database-get-started.md) maken. U moet de naam van de server gebruikt voor de database opslaan.

##<a name="create-a-sql-database-table"></a>Een SQL-databasetabel maken

> [AZURE.NOTE] Er zijn tal van manieren verbinding maken met een SQL-Database een tabel te maken. De volgende stappen gebruikt [FreeTDS](http://www.freetds.org/) uit het cluster HDInsight.

1. Met SSH verbinding maken met het cluster op basis van Linux-HDInsight en de volgende stappen uitvoeren vanaf de SSH-sessie.

3. Gebruik de volgende opdracht FreeTDS installeren:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Zodra de FreeTDS is geïnstalleerd, gebruik de volgende opdracht voor verbinding met de Database van SQL server. __Servernaam__ vervangen door de naam van de Database van SQL server. __AdminLogin__ en __adminPassword__ vervangen door de aanmelding voor SQL-Database. __Databasenaam__ vervangen door de naam van de database.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Uitvoer van de volgende strekking wordt weergegeven:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Op de `1>` wordt gevraagd, voert u de volgende regels:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Wanneer de `GO` instructie is ingevoerd, wordt de vorige overzichten worden geëvalueerd. Hiermee maakt u een nieuwe tabel met de naam __vertragingen__, met een gegroepeerde index (vereist voor SQL-Database).

    Om te controleren of de tabel is gemaakt, gebruikt u het volgende:

        SELECT * FROM information_schema.tables
        GO

    Uitvoer van de volgende strekking worden weergegeven:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Voer `exit` op de `1>` vragen om het hulpprogramma tsql af te sluiten.
    
##<a name="export-data-with-sqoop"></a>Gegevens exporteren met Sqoop

2. Gebruik de volgende opdracht om te controleren of de Sqoop ziet de SQL-Database:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dit moet een lijst met databases, met inbegrip van de database die u in de tabel vertraging in eerder retourneren.

3. De volgende opdracht gebruiken om gegevens te exporteren uit hivesampletable aan de mobiledata-tabel:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Dit geeft de opdracht Sqoop verbinding maken met SQL-Database, de database met de tabel vertraging en gegevens exporteren uit de wasbs: / / / zelfstudies/flightdelays/output (waar we opgeslagen de uitvoer van de query component eerder) aan de tabel vertraging.

4. Nadat de opdracht is voltooid, gebruikt u de volgende verbinding maken met de database via TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zodra verbonden, gebruikt u de volgende instructies om te controleren dat de gegevens zijn geëxporteerd naar de tabel mobiledata:
    
        SELECT * FROM delays
        GO

    Hier ziet u een overzicht van de gegevens in de tabel. Type `exit` om af te sluiten van het hulpprogramma tsql.

##<a id="nextsteps"></a>Volgende stappen

U weet nu hoe een bestand te uploaden naar Azure Blob opslag, het vullen van een component-tabel met de gegevens uit de Azure Blob-opslag onderdeel query's uitvoeren en Sqoop gebruiken om gegevens te exporteren uit HDFS naar Azure SQL-databases. Voor meer informatie, Zie de volgende artikelen:

* [Aan de slag met HDInsight][hdinsight-get-started]
* [Gebruik component met HDInsight][hdinsight-use-hive]
* [Gebruik Oozie met HDInsight][hdinsight-use-oozie]
* [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop]
* [Varken met HDInsight gebruiken][hdinsight-use-pig]
* [MapReduce Java-programma's ontwikkelen voor HDInsight][hdinsight-develop-mapreduce]
* [Ontwikkelen van Python Hadoop streaming programma's voor HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
