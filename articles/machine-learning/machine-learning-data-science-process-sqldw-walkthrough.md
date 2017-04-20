<properties
    pageTitle="Het Team gegevens wetenschap proces in actie: met behulp van SQL Data Warehouse | Microsoft Azure"
    description="Geavanceerde Analytics proces en de technologie in actie"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Het Team gegevens wetenschap proces in actie: SQL Data Warehouse gebruiken

In deze zelfstudie bekijken we u bij het maken en implementeren van een machine leren model met behulp van SQL Data Warehouse (DW SQL) voor een openbaar beschikbare dataset--de dataset [RDAM Taxi Trips](http://www.andresmh.com/nyctaxitrips/) . De binaire indeling model gebouwd worden voorspeld of een tip wordt betaald voor een reis en modellen voor de multiclass-indeling en regressie worden ook besproken die de verdeling van de betaalde bedragen tip voorspellen.

De procedure volgt de werkstroom [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . We zien hoe een science gegevensomgeving setup is uitgerust met laden van de gegevens in SQL DW, en hoe gebruiken SQL DW of een notitieblok IPython verkennen van de gegevens en de engineering model. We vervolgens hoe bouwen en implementeren van een model met Azure Machine Learning.


## <a name="dataset"></a>De dataset RDAM Taxi Trips

De gegevens RDAM Taxi reis bestaat uit 20GB gecomprimeerd CSV-bestanden (~ 48GB niet-gecomprimeerd), vastleggen van meer dan 173 miljoen afzonderlijke trips en de tarieven voor elke reis betaald. Elke reis record bevat de pickup en inleverbibliotheek locaties en tijden, anoniem hack (bestuurder) licentienummer en het nummer van de straten (unieke id van de taxi). De gegevens in het jaar 2013 heeft betrekking op alle trips en wordt geleverd in de volgende twee gegevenssets voor elke maand:

1. Het bestand **trip_data.csv** bevat details van de reis, zoals het aantal passagiers, ophalen en punten dropoff, duur van de reis en duur van reis. Hier zijn een aantal voorbeeldrecords:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Het bestand **trip_fare.csv** bevat de details van de voor elke reis, zoals betalingstype bedrag ritbedrag, toeslag en belastingen, tips en tolgelden, betaalde reissom en het totale bedrag betaald. Hier zijn een aantal voorbeeldrecords:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De **unieke sleutel** reis deelnemen aan\_gegevens en reis\_ritbedrag bestaat uit de volgende drie velden:

- straten,
- inbreken\_licentie en
- Afhalen\_datum/tijd.

## <a name="mltasks"></a>Adres van de drie soorten taken voorspelling

We formuleren drie voorspelling problemen op basis van de *tip\_bedrag* ter illustratie van de drie soorten de modellering van taken:

1. **Binaire indeling**: om te voorspellen of er een tip is betaald voor een rit, dat wil zeggen een *tip\_bedrag* die groter is dan $0 een positief voorbeeld is, terwijl een *tip\_bedrag* is een negatief voorbeeld van $0.

2. **Multiclass classificatie**: voorspellen van het bereik van de tip voor de reis betaald. We delen de *tip\_bedrag* in vijf bakken of klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressie taak**: te voorspellen, het bedrag van de tip voor een reis betaald.  


## <a name="setup"></a>De omgeving van de wetenschap Azure gegevens voor geavanceerde analytics instellen

Ga als volgt te werk om in te stellen uw omgeving Azure Data Science.

**Maak uw eigen account Azure blob-opslag**

- Wanneer u uw eigen Azure blob-opslag inrichten, een geo-locatie kiezen voor uw Azure blob-opslag in of zo dicht mogelijk tot **Centraal Zuid-Amerikaanse**, dat is waar de Taxi RDAM-gegevens opgeslagen. De gegevens worden gekopieerd met behulp van AzCopy uit de openbare blob storage container naar een container in uw eigen account opslag. Hoe dichter uw Azure blob-opslag South Central ons, des te sneller deze taak (stap 4) wordt voltooid.
- Als u wilt uw eigen account Azure opslag maken, volgt u de stappen op [Azure over opslag rekeningen](../storage/storage-create-storage-account.md). Zorg ervoor dat notities te maken over de waarden voor de volgende opslag referenties als ze later in deze procedure nodig.

  - **De naam van opslag**
  - **Opslag Account sleutel**
  - **Naam van container** (die u wilt dat de gegevens worden opgeslagen in de Azure blobopslag)

**Uw exemplaar van Azure SQL DW inrichten.**
Volg de documentatie bij het [maken van een SQL-datawarehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) inrichten van een exemplaar van SQL Data Warehouse. Zorg ervoor dat u notities maken op de volgende SQL Data Warehouse-referenties die worden gebruikt in de volgende stappen.

  - **Naam**: <server Name>. database.windows.net
  - **De naam SQLDW (Database)**
  - **Gebruikersnaam**
  - **Wachtwoord**

**Installeer Visual Studio 2015 en hulpmiddelen voor SQL Server-gegevens.** Zie voor meer informatie de [installatie van Visual Studio 2015 en/of SSDT (SQL Server Data Tools) voor SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Verbinding maken met uw DW Azure SQL met Visual Studio.** Zie stap 1 & 2 in [verbinding maken met Azure SQL Data Warehouse met Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md)voor meer informatie.

>[AZURE.NOTE] De volgende SQL-query uitvoeren op de database die u hebt gemaakt in uw SQL Data Warehouse (in plaats van de query die in stap 3 van het onderwerp verbinding maken) maken **een hoofdsleutel**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Een werkruimte Azure Machine Learning onder uw Azure abonnement maken.** Zie [een Azure Machine Learning werkruimte maken](machine-learning-create-workspace.md)voor instructies.

## <a name="getdata"></a>Laden van de gegevens in SQL Data Warehouse

Open een Windows PowerShell-opdracht-console. Voer de volgende PowerShell downloaden van voorbeeld van SQL-opdrachten scriptbestanden die wij met u delen op Github naar een lokale map die u met de parameter opgeeft *-DestDir*. Kunt u de waarde van de parameter *-DestDir* naar een lokale map. Als niet *- DestDir* bestaat, wordt deze gemaakt door het PowerShell-script.

>[AZURE.NOTE] Mogelijk moet u **als Administrator uitvoeren** bij het uitvoeren van de volgende PowerShell script als de map *DestDir* Administrator-bevoegdheden moet te maken of om ernaar te schrijven.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Na de geslaagde uitvoering wordt de huidige werkmap *- DestDir*. Wees scherm zien zoals hieronder:

![][19]

In de *DestDir -*de volgende PowerShell script in de administrator-modus worden uitgevoerd:

    ./SQLDW_Data_Import.ps1

Wanneer het PowerShell-script wordt uitgevoerd voor de eerste keer, wordt u gevraagd om invoer van de gegevens uit de SQL Azure DW en uw account Azure blob-opslag. Wanneer deze PowerShell-script is voltooid wordt voor de eerste keer de referenties u invoer zijn geschreven naar een configuratiebestand SQLDW.conf in de huidige werkmap. De toekomstige uitvoering van dit scriptbestand PowerShell heeft de optie voor het lezen van dat alle parameters in dit configuratiebestand nodig. Als u bepaalde instellingen wilt wijzigen, kunt u de invoerparameters op het scherm bij de prompt door dit configuratiebestand verwijderen en de parameterwaarden invoeren wanneer daarom wordt gevraagd of de parameterwaarden wijzigen door het bewerken van het bestand SQLDW.conf in de directory *- DestDir* .

>[AZURE.NOTE] Schema naam om conflicten te voorkomen met artikelen die al in uw SQL Azure DW, bij het lezen van parameters rechtstreeks uit het bestand SQLDW.conf, wordt een willekeurig getal met 3 cijfers toegevoegd aan de naam van het schema uit het bestand SQLDW.conf als de naam van het standaard schema voor elke serie. De PowerShell-script kan u vragen voor de naam van een schema: de naam kan naar goeddunken van de gebruiker worden opgegeven.

Dit bestand **PowerShell script** voert de volgende taken:

- **Gedownload en geïnstalleerd, AzCopy**, als AzCopy niet is geïnstalleerd

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Gegevens naar uw account privé blob-opslag** van de openbare blob met AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Geladen gegevens met behulp van Polybase (door het uitvoeren van LoadDataToSQLDW.sql) om uw SQL Azure DW** van uw persoonlijke blob storage rekening met de volgende opdrachten.

    - Een schema maken

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Een referentie binnen het bereik van database maken

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Een externe gegevensbron maken voor een blob storage Azure

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Een externe bestandsindeling voor een CSV-bestand maken. Gegevens zijn gedecomprimeerd en velden worden gescheiden met het teken.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Externe ritbedrag en tabellen voor RDAM taxi dataset reis in Azure blob-opslag maken.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Laden van gegevens uit externe tabellen in Azure blob-opslag naar SQL Data Warehouse

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Maak een gegevenstabel monster (NYCTaxi_Sample) en gegevens invoegen van SQL-query's op de reis en ritbedrag tabellen te selecteren. (Sommige stappen van deze procedure moet deze voorbeeldtabel gebruikt.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

De geografische locatie van uw opslag rekeningen beïnvloedt laadtijden.

>[AZURE.NOTE] Afhankelijk van de geografische locatie van uw persoonlijke blob storage-account, het kopiëren van gegevens uit een openbare blob aan uw account voor de particuliere opslag kan duren ongeveer 15 minuten of zelfs langer, en het proces van het laden van de gegevens van uw opslag-account aan uw SQL Azure DW kan duurt ongeveer 20 minuten of langer.  

U moet beslissen wat betekenen als er dubbele bestanden van bron- en doeladressen.

>[AZURE.NOTE] Als het CSV-bestanden worden gekopieerd van de openbare blob-opslag aan uw persoonlijke blob storage account bestaat al in uw account privé blob storage, AzCopy u wordt gevraagd of u wilt overschrijven. Als u niet wilt overschrijven, invoerwaarden **n** wanneer daarom wordt gevraagd. Als u overschrijven **alle** van hen, de invoer **een wilt** wanneer daarom wordt gevraagd. U kunt ook invoer **y** om afzonderlijk CSV-bestanden overschrijven.

![#21 uitzetten][21]

U kunt uw eigen gegevens gebruiken. Als de gegevens in uw lokale machine in uw toepassing van het echte leven, kunt u AzCopy op lokale gegevens uploaden naar uw persoonlijke Azure blob-opslag. U hoeft alleen te wijzigen van **de locatie,** `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in opdracht van de AzCopy van de PowerShell script-bestand naar de lokale map die de gegevens bevat.


>[AZURE.TIP] Als uw gegevens al in uw persoonlijke Azure blob-opslag in de toepassing van het echte leven, kunt u het AzCopy overslaan in het PowerShell script en de gegevens rechtstreeks uploaden naar Azure SQL DW. Dit vereist extra bewerkingen van het script aan te passen dat naar de indeling van uw gegevens.


Deze Powershell-script ook aansluit in de informatie Azure SQL DW de exploratie voorbeeldbestanden SQLDW_Explorations.sql, SQLDW_Explorations.ipynb en SQLDW_Explorations_Scripts.py zodat deze drie bestanden klaar om te worden getest direct zijn nadat het PowerShell-script is voltooid.

Na een geslaagde uitvoering ziet u scherm zoals hieronder:

![][20]

## <a name="dbexplore"></a>Verkennen en functie engineering in Azure SQL Data Warehouse

In deze sectie uitvoeren wij verkennen en de functie voor het genereren van SQL-query's uitvoeren tegen Azure SQL DW rechtstreeks met behulp van **Visual Studio-hulpmiddelen voor gegevens**. Alle SQL-query's gebruikt in deze sectie vindt u in het script met de naam *SQLDW_Explorations.sql*. Dit bestand is al gedownload naar uw lokale map door het PowerShell-script. U kunt het ook vanuit [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql)ophalen. Maar het bestand in de Github heeft niet de informatie Azure SQL DW is aangesloten.

Verbinding maken met uw Azure SQL DW met DW SQL-aanmeldingsnaam en wachtwoord met behulp van Visual Studio en opent u de **SQL Object Explorer** bevestigen dat de database en de tabellen hebt geïmporteerd. Het bestand *SQLDW_Explorations.sql* ophalen.

>[AZURE.NOTE] Een parallelle gegevens magazijn (PDW) query als editor wilt openen, gebruikt u de opdracht **Nieuwe Query** terwijl uw PDW is geselecteerd in de **SQL Object Explorer**. De standaard SQL-query editor wordt niet ondersteund door PDW.

Hier vindt u het type gegevens te verkennen en functie generatie uitgevoerd in deze sectie:

- Ontdek de verdelingen van de gegevens van een paar velden in verschillende tijdvensters.
- Onderzoek de kwaliteit van de gegevens van de velden lengte en breedte.
- Genereren binaire en multiclass classificatie labels op basis van de **tip\_bedrag**.
- Genereren van functies en reis afstanden compute/vergelijken.
- Voeg de twee tabellen en pak een steekproef die wordt gebruikt om modellen te bouwen.

### <a name="data-import-verification"></a>Controle van gegevens importeren

Deze query's bieden een snelle controle van het aantal rijen en kolommen in de tabellen gevuld met eerder Polybase van parallelle bulk importeren,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Output:** U moet 173,179,759 rijen en kolommen op 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Exploratie: Reis distributie door de straten

In dit voorbeeldquery geeft medallions (taxi nummers) die meer dan 100 trips voltooid binnen een opgegeven periode. De query zouden kunnen profiteren van de toegang tot de gepartitioneerde tabel omdat deze is geconditioneerd door het partitieschema van **pickup\_datum/tijd**. Bij het controleren van de volledige gegevensset maakt ook gebruik van de gepartitioneerde tabel en/of scan indexeren.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Output:** De query moet een tabel met rijen geven de 13,369 medallions (taxi's) en het nummer van de reis voltooid door ze in 2013 retourneren. De laatste kolom bevat het aantal trips voltooid.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Exploratie: Reis distributie door de straten en hack_license

In dit voorbeeld geeft de medallions (taxi nummers) en hack_license getallen (stuurprogramma's) die voltooid van meer dan 100 trips binnen een opgegeven periode.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Output:** De query moet een tabel retourneren met 13,369 rijen geven de 13,369 auto en stuurprogramma-id's die meer dat 100 trips in 2013 hebt voltooid. De laatste kolom bevat het aantal trips voltooid.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Evaluatie van kwaliteit: Controleer de records met onjuiste lengte en/of latitude

Hierbij wordt gekeken naar als een van de velden lengte en/of latitude een ongeldige waarde bevatten (radiaal graden moeten liggen tussen-90 en 90), of (0, 0) coördinaten.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Output:** De query retourneert 837,467 trips met ongeldige velden voor de geografische lengte en/of latitude.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Exploratie: Gekantelde versus niet Gekantelde trips distributie

In dit voorbeeld wordt gezocht naar het aantal trips zijn Gekantelde versus het aantal wanneer geen punt zijn in een bepaalde periode (of in de volledige gegevensset als die betrekking hebben op het volledige jaar zoals deze hier is ingesteld). Deze verdeling komt overeen met de verdeling van de binaire label later worden gebruikt voor de modellering van de binaire indeling.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Output:** De query moet Gekantelde de volgende tip frequenties voor het jaar 2013: 90,447,622 en 82,264,709 niet Gekantelde retourneren.

### <a name="exploration-tip-classrange-distribution"></a>Exploratie: Tip klasse-bereik distributie

In dit voorbeeld berekent de verdeling van de tip bereiken in een bepaalde periode (of de volledige gegevensset als die betrekking hebben op het volledige jaar). Dit is de verdeling van de label-klassen die later worden gebruikt voor de modellering van de multiclass-classificatie.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Uitvoer:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Exploratie: Berekenen en vergelijken reis afstand

In dit voorbeeld wordt het ophalen en inleverbibliotheek lengtegraad en latitude naar SQL Geografie verwijst, berekent de afstand van de reis met SQL Geografie punten verschil en geeft als resultaat een aselecte steekproef van de resultaten van de vergelijking. In het voorbeeld beperkt de resultaten van geldige coördinaten alleen met de kwaliteit beoordeling gegevensquery waarvoor eerder.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>SQL-functies met functie-engineering

SQL-functies kunnen soms een efficiënte optie voor engineering functie zijn. In dit scenario hebben we een SQL-functie voor het berekenen van de rechtstreekse afstand tussen de locaties ophalen en dropoff gedefinieerd. U kunt de volgende SQL-scripts uitvoeren in **Visual Studio Tools-gegevens**.

Hier wordt de SQL-script waarmee wordt gedefinieerd met de functie distance.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Hier volgt een voorbeeld van de functie voor het genereren van functies in uw SQL-query:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Output:** Deze query wordt een tabel (met 2,803,538 rijen) gegenereerd met afhalen en dropoff Latitude-systemen en lengten en de bijbehorende directe afstandstabel in mijlen. Hier zijn de resultaten voor de eerste 3 rijen:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Gegevens voorbereiden voor modellen bouwen

De volgende query joins de **nyctaxi\_reis** en **nyctaxi\_ritbedrag** tabellen, genereert een binaire indeling label **Gekantelde**, een indeling met meerdere klasse label **tip\_klasse**, en een monster wordt geëxtraheerd uit de volledige samengevoegde dataset. De bemonstering gebeurt door het ophalen van een subset van de trips gebaseerd op tijd ophalen.  Deze query kan worden gekopieerd en vervolgens geplakt in de [Azure Machine Learning Studio](https://studio.azureml.net) [Gegevens importeren] rechtstreeks[ import-data] -module voor ingestie rechtstreeks gegevens uit de SQL-database-instantie in Azure. De query sluit de records met onjuiste (0, 0) coördinaten.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Wanneer u klaar bent om te gaan naar Azure Machine Learning, kan u:  

1. De uiteindelijke SQL-query te pakken en de voorbeeldgegevens kopiëren plakken de query rechtstreeks in [Importeren gegevens] opslaan[ import-data] module in Azure Machine Learning, of
2. De bemonsterde en kunstmatige gegevens die u wilt gebruiken voor het bouwen in een nieuwe tabel met SQL DW model en de nieuwe tabel in de [Gegevens importeren] gebruiken[ import-data] module in Azure Machine Learning. De PowerShell-script in een eerdere stap heeft dit voor u gedaan. U kunt lezen rechtstreeks uit deze tabel in de module gegevens importeren.


## <a name="ipnb"></a>Verkennen en functie engineering in IPython-laptop

In deze sectie, we verkennen en functie genereren met behulp van beide Python wordt uitgevoerd en SQL-query's op de SQL-DW eerder hebt gemaakt. Een monster IPython-laptop met de naam **SQLDW_Explorations.ipynb** en een scriptbestand Python **SQLDW_Explorations_Scripts.py** zijn gedownload naar uw lokale map. Ze zijn ook beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Deze twee bestanden zijn identiek in Python scripts. De Python script-bestand is verleend in het geval er geen een laptop IPython-server. Deze twee voorbeeldbestanden Python zijn onder **2.7 Python**ontworpen.

De benodigde informatie aan Azure SQL DW in de steekproef IPython laptop en de Python scriptbestand gedownload naar uw lokale computer is aangesloten door het PowerShell script eerder. Zij zijn uitvoerbaar zonder enige wijziging.

Als u al een werkruimte AzureML hebt ingesteld, kunt u rechtstreeks het monster laptop IPython uploaden naar de service AzureML IPython laptop en waarop deze wordt gestart. Hier zijn de stappen om te uploaden naar AzureML IPython-laptop-service:

1. Log in op uw werkruimte AzureML 'Studio' aan de bovenkant, en op "Laptops" aan de linkerkant van de webpagina.

    ![#22 uitzetten][22]

2. Klik op 'Nieuw' in de linker benedenhoek van de pagina en selecteer "Python 2". Vervolgens een naam in het notitieblok en klikt u op het vinkje om de nieuwe IPython-laptop leeg te maken.

    ![#23 uitzetten][23]

3. Klik op het symbool 'Jupyter' op de linker bovenhoek van de nieuwe IPython-laptop.

    ![#24 uitzetten][24]

4. Slepen en neerzetten van het monster IPython laptop naar de pagina **structuur** van uw service AzureML IPython laptop Klik op **uploaden**. Vervolgens het monster laptop IPython worden geüpload naar de service AzureML IPython-laptop.

    ![#25 uitzetten][25]

Script-bestand de volgende Python pakketten nodig zijn om uit te voeren van het monster IPython-laptop of de Python. Deze pakketten zijn vooraf geïnstalleerd als u de service AzureML IPython-laptop.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

De aanbevolen volgorde bij het bouwen van geavanceerde analytische oplossingen op AzureML met grote gegevens is als volgt:

- In een kleine steekproef van de gegevens in een kader voor de gegevens in het geheugen gelezen.
- Sommige visualisaties en explorations met behulp van de bemonsterde gegevens uitvoeren.
- Experimenteer met de functie engineering met behulp van de gegevens van de steekproef.
- Python gebruiken om SQL-query's rechtstreeks tegen de SQL DW groter verkennen, gegevens manipuleren en functie engineering.
- De steekproefgrootte is geschikt voor het bouwen van Azure Machine Learning model bepalen.

De volgende elementen zijn enkele verkennen, data visualisatie en voorbeelden technische functie. Meer gegevens explorations vindt u in het monster IPython laptop en het voorbeeldbestand voor Python script.

### <a name="initialize-database-credentials"></a>Databasereferenties initialiseren

De verbindingsinstellingen voor de database in de volgende variabelen initialiseren:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Databaseverbinding maken

Hier is de verbindingsreeks waarmee de verbinding met de database.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rapport aantal rijen en kolommen in tabel < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Totale aantal rijen = 173179759  
- Totaal aantal kolommen = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Rapport aantal rijen en kolommen in tabel < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Totale aantal rijen = 173179759  
- Totaal aantal kolommen = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Alleen in een klein voorbeeld uit de SQL Data Warehouse-Database

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Tijd voor het lezen van dat de voorbeeldtabel is 14.096495 seconden.  
Het aantal rijen en kolommen opgehaald = (1000, 21).

### <a name="descriptive-statistics"></a>Beschrijvende statistiek

U bent nu gereed voor de bemonsterde gegevens verkennen. We beginnen met het kijken naar enkele Beschrijvende statistiek voor de **reis\_afstand** (of andere velden die u wilt opgeven).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Visualisatie: Voorbeeld van grafiek

Vervolgens kijken we de BoxPlot voor de reis afstand visualiseren van de quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 uitzetten][1]

### <a name="visualization-distribution-plot-example"></a>Visualisatie: Voorbeeld van de verdeling waarnemingspunt

Percelen die de distributie en een histogram voor de bemonsterde reis afstand te visualiseren.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Uitzetten van #2][2]

### <a name="visualization-bar-and-line-plots"></a>Visualisatie: Staaf en lijn getekend

In dit voorbeeld we bin de reis afstand in vijf opslaglocaties en visualiseren van de resultaten van binning.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

We kunnen uitzetten van de bovenstaande verdeling van de opslaglocatie in een bar of lijn uitgezet met:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 uitzetten][3]

en

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 uitzetten][4]

### <a name="visualization-scatterplot-examples"></a>Visualisatie: Voorbeelden Scatterplot

We zien waarnemingspunt spreiding tussen **reis\_keer\_in\_sec.** en **reis\_afstand** om te zien of er een correlatie

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 uitzetten][6]

We kunnen op dezelfde manier controleren of de relatie tussen **rate\_code** en **reis\_afstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 uitzetten][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Verkennen van de bemonsterde gegevens met behulp van SQL-query's in een notitieblok IPython

In deze sectie besproken verdelingen van gegevens met behulp van de bemonsterde gegevens die is vastgelegd in de nieuwe tabel die we eerder hebben gemaakt. Houd er rekening mee dat soortgelijke explorations kunnen worden uitgevoerd met behulp van de oorspronkelijke tabellen.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Exploratie: Rapport aantal rijen en kolommen in de tabel bemonsterde

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Exploratie: Gekantelde/niet omzeild distributie

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Exploratie: Tip klasse distributie

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Exploratie: De verdeling van de tip tekenen per klasse

    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 uitzetten][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Exploratie: Dagelijkse distributie van trips

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Exploratie: Reis verdeling per straten

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Exploratie: Reis distributie door de straten en hack-licentie

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Exploratie: Reis verdeling

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Exploratie: Reis afstand verdeling

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Exploratie: Betaling type verdeling

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Controleer of de uiteindelijke vorm van de featurized-tabel

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Modellen in Azure Machine leren bouwen

We gaan nu verder met de modellen bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens zijn gereed om te worden gebruikt in de voorspelling problemen geïdentificeerd eerder, namelijk:

1. **Binaire indeling**: om te voorspellen of er een tip is betaald voor een reis.

2. **Multiclass classificatie**: voorspellen van het bereik van de tip betaald volgens de eerder gedefinieerde klassen.

3. **Regressie taak**: te voorspellen, het bedrag van de tip voor een reis betaald.  



Om te beginnen met de oefening modellering, log in op uw werkruimte **Azure Machine Learning** . Als u nog niet een machine learning werkruimte hebt gemaakt, ziet u [een Azure ML werkruimte maken](machine-learning-create-workspace.md).

1. Zie aan de slag met Azure Machine Learning [Wat is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Log in op [Azure Machine Learning Studio](https://studio.azureml.net).

3. De introductiepagina van Studio biedt een schat aan informatie, video's, zelfstudies, koppelingen naar de verwijzing Modules en andere bronnen. Neem contact op met de [Azure Machine Informatiepunt documentatie](https://azure.microsoft.com/documentation/services/machine-learning/)voor meer informatie over Azure Machine Learning.

Een typische training experiment bestaat uit de volgende stappen uit:

1. Een **+ Nieuw** experiment maken.
2. De gegevens ophalen in Azure ML.
3. Vooraf verwerken, transformeren en de gegevens desgewenst bewerken.
4. Functies worden gegenereerd wanneer dat nodig is.
5. De gegevens opsplitsen in datasets training, validatie/testen (of afzonderlijke gegevenssets voor elke).
6. Selecteer een of meer machine learning algoritmen afhankelijk van het leren-probleem op te lossen. Bijvoorbeeld, binaire indeling, multiclass classificatie en regressie.
7. Een of meer modellen met behulp van de dataset training trainen.
8. Score dataset validatie met behulp van de opgeleide modellen.
9. Evaluatie van de modellen berekent u de relevante parameters voor het probleem van leren.
10. Fijn afstemmen van de modellen en selecteer het beste model te implementeren.

In deze oefening, hebben we al verkend engineering van de gegevens in een gegevensmagazijn SQL en besloot op de grootte van de steekproef te nemen in Azure ML. Hier volgt de procedure voor het maken van een of meer van de voorspellende modellen:

1. Azure ML met behulp van de [Gegevens importeren] de gegevens ophalen[ import-data] module beschikbaar in de sectie **gegevensinvoer en uitvoer** . Zie de [Gegevens importeren] voor meer informatie[ import-data] module pagina.

    ![Gegevens importeren met Azure ML][17]

2. Selecteer **Azure SQL-Database** als **gegevensbron** in het deelvenster **Eigenschappen** .

3. De DNS-naam van de database invoeren in het veld **naam van de databaseserver** . Indeling:`tcp:<your_virtual_machine_DNS_name>,1433`

4. De **naam van de Database** invoeren in het bijbehorende veld.

5. Voer de *SQL-gebruikersnaam* in van de **Server gebruikersnaam**en het *wachtwoord* in het **wachtwoord van Server**.

6. Schakel het selectievakje **een servercertificaat te accepteren** .

7. In het tekstgebied **databasequery** bewerken monsters plakken op de query die de nodige haalt databasevelden (met inbegrip van alle berekende velden, zoals de labels) of pijl-omlaag die de gegevens op de gewenste steekproefgrootte.

Een voorbeeld van een binaire indeling experiment lezen gegevens rechtstreeks vanuit de SQL Data Warehouse-database is in de onderstaande afbeelding (Vergeet niet om de tabel namen nyctaxi_trip en nyctaxi_fare vervangen door de naam van het schema en de tabelnamen die u in uw scenario gebruikt). Soortgelijke experimenten kunnen worden samengesteld voor de multiclass-indeling en regressie problemen.

![Azure ML trein][10]

> [AZURE.IMPORTANT] In de gegevens modelleren extractie en bemonstering query voorbeelden vermeld in de voorgaande secties, **alle labels voor de oefeningen drie modellen zijn opgenomen in de query**. Een belangrijk (vereist) in elk van de oefeningen modelleren is uit te **sluiten** de onnodige labels voor de twee problemen en eventuele andere **doel lekt**. Bijvoorbeeld wanneer u de binaire indeling, gebruikt u de label **Gekantelde** en uitsluiten van de velden **tip\_klasse**, **punt\_bedrag**, en **Totaal\_bedrag**. Zijn doel lekkage aangezien zij de tip impliceren betaald.
>
> Wilt uitsluiten van alle overbodige kolommen of doel lekt, mag u de [Kolommen selecteren in de Dataset] [ select-columns] module of de [Metagegevens bewerken][edit-metadata]. Zie voor meer informatie [Kolommen selecteren in de Dataset] [ select-columns] en [Metagegevens bewerken] [ edit-metadata] verwijzen naar pagina's.

## <a name="mldeploy"></a>Modellen in Azure Machine Learning implementeren

Als uw model klaar is, kunt u het gemakkelijk implementeren als een webservice rechtstreeks van het experiment. Zie voor meer informatie over het implementeren van Azure ML webservices [implementeren een webservice Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Als u wilt implementeren op een nieuwe webservice, moet u:

1. Een score experiment maken.
2. Implementeren van de webservice.

Klik op **Maken SCOREN EXPERIMENTEREN** in de onderste actiebalk om een score experiment uit een opleiding **voltooid** experiment.

![Azure scoren][18]

Azure Machine Learning probeert te maken van een score experiment op basis van de onderdelen van het experiment training. Het programma zal met name:

1. De opgeleide model opslaan en verwijderen van de trainingsmodules model.
2. Een logische **invoerpoort** overeenkomen met het schema van de verwachte ingevoerde gegevens identificeren.
3. Een logische **uitvoerpoort** voor de verwachte web service-uitvoerschema herkennen.

Wanneer het scorepatroon experiment wordt gemaakt, bekijken en maken zo nodig aanpassen. Een typische aanpassing is de dataset invoer en/of de query vervangen door een exclusief label velden, zoals deze is alleen beschikbaar als de service wordt genoemd. Het is een goede gewoonte om de grootte van de dataset invoer en/of de query beperken tot enkele records, net voldoende om aan te geven de invoer schema. Voor de uitvoerpoort voor alle invoervelden uitsluiten en alleen de **Labels gescoord** en **Kansen gescoord** in de uitvoer op basis van de [Kolommen selecteren in de Dataset] is[ select-columns] module.

Een voorbeeld van een experiment scoren wordt geleverd in de onderstaande afbeelding. Als u klaar bent om te implementeren, klik op **Publiceren WEBSERVICE** in de onderste actiebalk.

![Azure ML publiceren][11]


## <a name="summary"></a>Samenvatting
Om een samenvatting van wat we hebben gedaan in deze zelfstudie scenario, hebt u gemaakt een Azure wetenschap gegevensomgeving, met een grote openbare dataset gewerkt door het Team gegevens wetenschap, van gegevens oplevert als model voor opleiding en voor de implementatie van een webservice Azure Machine leren fotograferen.

### <a name="license-information"></a>Licentie-informatie

In dit scenario van het monster en de bijbehorende scripts en IPython notebook(s) gedeeld door Microsoft onder de MIT-licentie. Controleer of u het bestand LICENSE.txt in in de directory van de voorbeeldcode op GitHub voor meer informatie.

## <a name="references"></a>Verwijzingen

• [Andrés Monroy RDAM Taxi Trips pagina downloaden](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing van RDAM Taxi rit gegevens door Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taxi RDAM en Limousine Commissie onderzoek en statistieken](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
