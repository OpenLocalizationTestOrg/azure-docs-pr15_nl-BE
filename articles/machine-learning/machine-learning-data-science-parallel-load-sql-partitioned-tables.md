<properties 
    pageTitle="Grote hoeveelheden gegevens importeren met behulp van SQL-partitietabellen parallelle | Microsoft Azure" 
    description="Parallelle grote hoeveelheden gegevens importeren met behulp van SQL-partitietabellen" 
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
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Parallelle grote hoeveelheden gegevens importeren met behulp van SQL-partitietabellen

Dit document wordt beschreven hoe u gepartitioneerde tabel(len) voor snelle parallelle bulk importeren van gegevens naar een SQL Server-database maken. Voor grote laden/overdracht van gegevens naar een SQL-database, kan het importeren van gegevens naar de SQL DB en de daaropvolgende query's worden verbeterd met behulp van _gepartitioneerd tabellen en weergaven_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Een nieuwe database en een set van bestandsgroepen maken

- [Een nieuwe database maken](https://technet.microsoft.com/library/ms176061.aspx) (als het niet bestaat)
- Bestandsgroepen database toevoegen aan de database waarin de gepartitioneerde fysieke bestanden

  Opmerking: U kunt dit doen met de [DATABASE maken](https://technet.microsoft.com/library/ms176061.aspx) als nieuwe of [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) als de database al bestaat

- (Indien nodig) een of meer bestanden toevoegen aan elke bestandsgroep database

 > [AZURE.NOTE] Geef de bestandsgroep doel waarin gegevens voor deze partitie en de fysieke database bestand namen waar de gegevens van de bestandsgroep worden opgeslagen.
 
In het volgende voorbeeld wordt een nieuwe database gemaakt met drie bestandsgroepen dan de primaire of met een fysiek bestand in elk logboek groepen. De databasebestanden zijn gemaakt in de standaardmap voor SQL Server-gegevens, zoals ingesteld in de SQL Server-exemplaar. Zie voor meer informatie over de standaardbestandslocaties, [Locaties voor standaard en benoemde exemplaren van SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Een gepartitioneerde tabel maken

Gepartitioneerde tabellen volgens het gegevensschema, gekoppeld aan de database gemaakt in de vorige stap bestandsgroepen maken. Wanneer gegevens geïmporteerd naar de gepartitioneerde tabel(len) bulk worden, records over verdeeld, volgens een partitieschema bestandsgroepen zoals hieronder beschreven.

**Een als partitietabel wilt maken, moet u:**

- [De partitiefunctie van een maken](https://msdn.microsoft.com/library/ms187802.aspx) waarin het bereik van waarden/grenzen moeten worden opgenomen in elke afzonderlijke partitietabel, bijvoorbeeld, partities per maand beperken (sommige\_datum/tijd\_veld) in het jaar 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Een partitieschema maken](https://msdn.microsoft.com/library/ms179854.aspx) die elk bereik partitie in de partitiefunctie wordt toegewezen aan een fysieke bestandsgroep bv.:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Tip: Als u wilt controleren of de bereiken van kracht in elke partitie op basis van de functie/schema, de volgende query uitvoeren:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [De gepartitioneerde tabel maken](https://msdn.microsoft.com/library/ms174979.aspx) (s) op het gegevensschema van uw, en geeft u de partitie stelsel en beperking veld voor het partitioneren van de tabel, bijvoorbeeld:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Zie voor meer informatie, [Maak gepartitioneerd tabellen en indexen](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>De gegevens voor elke afzonderlijke partitietabel bulkimport

- U kunt de BCP BULK INSERT of andere methoden zoals [SQL Server Migration Wizard](http://sqlazuremw.codeplex.com/). Het voorbeeld gebruikt de BCP-methode.

- [De database wijzigen](https://msdn.microsoft.com/library/bb522682.aspx) transactielogboeken schema wijzigen in BULK_LOGGED tot een minimum beperken van de overhead van de logboekfunctie, bijv.:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Sneller laden van gegevens, de bulkimportbewerkingen tegelijkertijd te starten. Zie voor tips over het bespoedigen bulk importeren van grote gegevens in SQL Server-databases, [1 TB in minder dan 1 uur laden](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

De volgende PowerShell script is een voorbeeld van de parallelle gegevens te laden met behulp van BCP.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Maak indexen joins en prestaties van query's optimaliseren

- Als u gegevens voor modellering wordt ophaalt uit meerdere tabellen, indexen maken op de join-toetsen om de join-prestaties te verbeteren.

- [Indexen maken](https://technet.microsoft.com/library/ms188783.aspx) (geclusterde of niet-geclusterde) gericht op de dezelfde bestandsgroep voor elke partitie, voor bv.:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
of,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] U kunt de indexen voor bulk importeren van de gegevens maken. Index maken voor het importeren van grote hoeveelheden langzamer laden van de gegevens.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Geavanceerde Analytics proces en de technologie in actie-voorbeeld

Zie bijvoorbeeld een end-to-end scenario met Cortana Analytics proces met een openbare dataset [Cortana Analytics proces in actie: met behulp van SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
 
