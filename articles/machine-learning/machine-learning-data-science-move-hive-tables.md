<properties
    pageTitle="Maken en gegevens in tabellen uit de Blob-opslag Component laden | Microsoft Azure"
    description="Component tabellen maken en gegevens in de blob met tabellen Component laden"
    services="machine-learning,storage"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Maken en laden van gegevens in tabellen component van Azure blob-opslag

Dit onderwerp bevat algemene query's component die component tabellen maken en gegevens laden vanuit Azure blob-opslag. Sommige ook richtsnoeren op component-tabellen worden gepartitioneerd en over het gebruik van de geoptimaliseerde rij in kolomvorm (ORC) opmaak ter verbetering van de prestaties van query's.

Dit **menu** bevat koppelingen naar onderwerpen waarin wordt beschreven hoe u gegevens in doel omgevingen waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP) te nemen.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u beschikt over:

* Een account Azure opslag gemaakt. Als u instructies, Zie [over Azure opslag rekeningen](../storage/storage-create-storage-account.md). 
* Een aangepaste Hadoop cluster met de service HDInsight ingericht.  Als u instructies, Zie [clusters voor geavanceerde analytics Azure HDInsight Hadoop aanpassen](machine-learning-data-science-customize-hadoop-cluster.md).
* Ingeschakelde externe toegang tot het cluster, aangemeld en het opdrachtregelprogramma Hadoop-console wordt geopend. Zie [toegang tot het hoofd van Hadoop Cluster knooppunt](machine-learning-data-science-customize-hadoop-cluster.md#headnode)als u instructies nodig hebt.

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure blob-opslag
Als u een Azure virtuele machine hebt gemaakt aan de hand van de instructies in [een Azure virtual machine voor geavanceerde analytics instellen](machine-learning-data-science-setup-virtual-machine.md), dit scriptbestand moet worden gedownload naar de *C:\\gebruikers\\\<gebruikersnaam\>\\documenten\\Data Science Scripts* map op de virtuele machine. Deze component query's alleen moeten u deze aansluiten op uw eigen gegevensschema- en Azure blob opslag in de juiste velden gereed voor verzending.

Nemen we aan dat de gegevens voor tabellen van de component in **ongecomprimeerde** indeling in tabelvorm, en dat de gegevens zijn geüpload naar de standaard (of extra) container van de opslag-account die wordt gebruikt door het cluster Hadoop.

Als u oefenen op de **RDAM Taxi rit gegevens wilt**, moet u:

- **download** de 24 [RDAM Taxi rit](http://www.andresmh.com/nyctaxitrips) gegevensbestanden (12 reis bestanden en 12 ritbedrag bestanden)
- **unzip** alle bestanden in CSV-bestanden, en vervolgens
- **uploaden** om de standaard (of geschikte container) van de account Azure opslag die is gemaakt met de procedure besproken in het onderwerp [aanpassen Azure HDInsight Hadoop clusters voor geavanceerde Analytics proces en technologie](machine-learning-data-science-customize-hadoop-cluster.md) . Op deze [pagina](machine-learning-data-science-process-hive-walkthrough.md#upload)vindt u het proces van de CSV-bestanden uploaden naar de standaardcontainer voor de opslag-account.


## <a name="submit"></a>Het indienen van query's component

Component-query's kunnen worden ingediend met behulp van:

1. [Component query's via de opdrachtregel voor Hadoop in headnode van Hadoop cluster indienen](#headnode)
2. [Component-query's met de component-Editor](#hive-editor)
3. [Voer query's component met Azure PowerShell-opdrachten](#ps)

Component-query's SQL-achtige. Als u bekend met SQL bent, misschien vindt u de [component van SQL gebruikers cheats blad](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) handig.

Bij het indienen van een query component verder kunt u de bestemming van de uitvoer van de query's component, ongeacht of deze op het scherm of in een lokaal bestand op het hoofd knooppunt of een Azure blob.


###<a name="headnode"></a>1. Voer component query's via de opdrachtregel voor Hadoop in headnode van Hadoop cluster

Als de query component complex is, leidt verzendt, wordt deze direct in het head-knooppunt van de Hadoop cluster meestal tot een sneller dan het indienen met behulp van een Editor component of Azure PowerShell scripts om te schakelen.

Aanmelden bij het hoofd knooppunt van het cluster Hadoop, opent u de opdrachtregel Hadoop op het bureaublad van het knooppunt head en voer de opdracht `cd %hive_home%\bin`.

U hebben kunt op drie manieren indienen component query's in de opdrachtregel van Hadoop:

* rechtstreeks
* met behulp van .hql-bestanden
* met de opdracht component console

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Component query's rechtstreeks in Hadoop opdrachtregel indienen.

U kunt de opdracht als volgt uitvoeren `hive -e "<your hive query>;` eenvoudige component query's rechtstreeks in Hadoop opdrachtregel indienen. Hier is een voorbeeld, waarbij het rode vak geeft een overzicht van de opdracht die is ingediend door de component query en het groene vak geeft een overzicht van de uitvoer van de query voor de component.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Voer query's component in .hql bestanden

Wanneer de query component complexer is en meerdere regels heeft, is het bewerken van query's in de opdrachtregel of de opdrachtconsole component niet praktisch. Een alternatief is het gebruik van een teksteditor in het head-knooppunt van het cluster Hadoop de component query's opslaan in een .hql in een lokale directory van de hoofd-knooppunt. En vervolgens de query component in het bestand .hql kan worden ingediend met behulp van de `-f` argument als volgt:

    hive -f "<path to the .hql file>"

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Voortgang status scherm afdrukken component query onderdrukken**

Standaard, nadat de query component is ingediend in Hadoop opdrachtregel wordt de voortgang van de taak toewijzen/verminderen afgedrukt op het scherm. Als u wilt onderdrukken het scherm afdrukken van de voortgang van het project Map/verminderen, kunt u een argument `-S` ("S" in hoofdletters) in de opdracht regel als volgt:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Component-query's in de opdrachtconsole component indienen.

Kunt u ook eerst de opdrachtenconsole component opgeven met opdracht `hive` in Hadoop opdrachtregel en vervolgens component query's in de opdrachtconsole component in te dienen. Hier volgt een voorbeeld. In dit voorbeeld wordt markeren de rode vakken de opdrachten voor het invoeren van de opdrachtenconsole component en de component-query in de opdrachtconsole component, respectievelijk ingediend. Het groene vak markeert de uitvoer van de query voor de component.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

In de vorige voorbeelden uitvoer rechtstreeks de queryresultaten component op het scherm. U kunt ook de uitvoer naar een lokaal bestand schrijven op het hoofd knooppunt of een Azure blob. Vervolgens kunt u andere hulpprogramma's voor het analyseren van de uitvoer van query's component verder.

**Uitvoer van de queryresultaten component naar een lokaal bestand.**

Queryresultaten component naar een lokale map op het hoofd knooppunt uitvoeren, hebt u de query component in het Hadoop-opdrachtregel als volgt verzenden:

    hive -e "<hive query>" > <local path in the head node>

In het volgende voorbeeld wordt de uitvoer van de query component wordt geschreven naar een bestand `hivequeryoutput.txt` in de map `C:\apps\temp`.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Component-queryresultaten voor uitvoer naar een Azure blob**

Ook kunt u de queryresultaten component aan een Azure blob, binnen de standaardcontainer van het cluster Hadoop uitvoeren. De component query hiervoor is als volgt:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

In het volgende voorbeeld wordt de uitvoer van de query component wordt geschreven naar een blob-map `queryoutputdir` in de standaardcontainer van het cluster Hadoop. Hier hoeft u alleen de naam van de map, zonder de naam van de blob. Er wordt een fout gegenereerd als u map- en blob-namen, zoals `wasb:///queryoutputdir/queryoutput.txt`.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Als u de standaardcontainer van het Hadoop-cluster met Azure Opslagverkenner opent, ziet u de uitvoer van de query component zoals in de volgende afbeelding. U kunt het filter (gemarkeerd met een rood vak) toepassen om alleen de blob met de opgegeven letters in namen ophalen.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. verzenden component query's met de component-Editor

Ook kunt u de Query-Console (Editor component) door een URL van het formulier *https://&#60; Hadoop Clusternaam >.azurehdinsight.net/Home/HiveEditor* in een webbrowser. U moet zijn aangemeld in de zien deze console en dus moet u uw Hadoop cluster referenties hier.

###<a name="ps"></a>3. verzenden component query's met Azure PowerShell-opdrachten

U kunt ook PowerShell component query's indienen. Zie voor instructies [dienen component taken met PowerShell](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Component-database en tabellen maken

De component query's in de [opslagplaats van Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) worden gedeeld en kunnen worden gedownload vanaf daar.

Dit is de component-query die een tabel component maakt.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Hier vindt u de beschrijvingen van de velden die u moet aansluiten en van andere configuraties:

- **& #60; databasenaam >**: de naam van de database die u wilt maken. Als u alleen gebruiken de standaarddatabase wilt, kan de query *maken... database* worden weggelaten.
- **& #60; tabelnaam >**: de naam van de tabel die u wilt maken in de opgegeven database. Als u gebruiken de standaarddatabase wilt, de tabel rechtstreeks kan worden verwezen door *& #60; tabelnaam >* zonder & #60; databasenaam >.
- **& #60; veldscheidingsteken >**: het scheidingsteken die velden in het gegevensbestand dat moet worden geüpload naar de component-tabel.
- **& #60; regelscheidingsteken >**: het scheidingsteken die regels in het bestand.
- **& #60; opslaglocatie >**: de Azure opslaglocatie van component tabellen gegevens opslaan. Als u geen *locatie & #60; opslaglocatie >*, de database en de tabellen zijn opgeslagen in *component/magazijn/* van het onderdeel cluster standaard in de map. Als u wilt dat de opslaglocatie opgeven, is de locatie in de standaardcontainer voor de database en tabellen. Deze locatie dient te worden verwezen als de locatie ten opzichte van de standaardcontainer van het cluster in de indeling van *' wasb: / / / & #60; map 1 > / "* of *' wasb: / / / & #60; map 1 > / & #60; map 2 > /'*, enz. Nadat de query is uitgevoerd, worden de relatieve mappen binnen de standaardcontainer gemaakt.
- **TBLPROPERTIES("SKIP.header.line.Count"="1")**: als het bestand een kopregel bevat, hebt u deze eigenschap toevoegen **aan het einde** van de *tabel maken* query. Anders wordt wordt de kopregel geladen als een record aan de tabel. Als het bestand geen kop-regel, kan deze configuratie in de query worden weggelaten.

## <a name="load-data"></a>Gegevens in tabellen Component laden
Dit is de component-query waarmee u gegevens in een component-tabel laadt.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; het pad naar een blob-gegevens >**: als de blob-bestand te uploaden naar de component-tabel in de standaardcontainer van het cluster HDInsight Hadoop, de *& #60; het pad naar een blob-gegevens >* moet de indeling *' wasb: / / / & #60; directory in deze container > / & #60; blob bestandsnaam >'*. De blob-bestand kan ook worden in een extra container van het cluster HDInsight Hadoop. In dit geval *& #60; het pad naar een blob-gegevens >* moet de indeling *' wasb: / / & #60; container name>@&#60;storage accountnaam >.blob.core.windows.net/ & #60; blob bestandsnaam >'*.

    >[AZURE.NOTE] De blob-gegevens kunnen worden geüpload naar de component-tabel is in de standaard of extra container van de account van de opslag voor het cluster Hadoop. Anders mislukt de query *Gegevens laden* klagen heeft geen toegang tot de gegevens.


## <a name="partition-orc"></a>Geavanceerde onderwerpen: tabel- en component-gegevens in de indeling voor ORC gepartitioneerd

Als de gegevens te groot is, is voor het partitioneren van de tabel nuttig voor query's die alleen nodig voor het scannen van een aantal partities van de tabel. Bijvoorbeeld, is het redelijk om de logboekgegevens van een website op datums.

Naast het onderdeel tabellen worden gepartitioneerd, is ook nuttig voor het opslaan van de gegevens van de component in de indeling voor geoptimaliseerde rij in kolomvorm (ORC). Zie <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">bestanden met behulp van ORC verbetert de prestaties wanneer een component is lezen, schrijven, en verwerking van gegevens</a>voor meer informatie over het opmaken van de ORC.

### <a name="partitioned-table"></a>Gepartitioneerde tabel
Dit is de component-query die een gepartitioneerde tabel maakt en gegevens in deze geladen.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Bij het opvragen van gepartitioneerde tabellen, is het raadzaam de partitie voorwaarde toevoegen aan het **begin** van de `where` component als dit de doelmatigheid van de zoekactie aanzienlijk verbetert.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Component gegevens opslaan in de indeling voor ORC

U laden niet direct gegevens van blob-opslag naar component die is opgeslagen in de indeling voor ORC. Hier zijn de stappen die u moet uitvoeren om te laden gegevens van Azure BLOB's component tabellen opgeslagen in de indeling voor ORC.

Gegevens in een externe tabel **Opgeslagen als TEXTFILE** en laden van blob-opslag om de tabel te maken.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Een interne tabel maken met hetzelfde schema als de externe tabel in stap 1, met dezelfde veldscheidingsteken en de component-gegevens worden opgeslagen in de indeling voor ORC.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Selecteer gegevens uit de externe tabel in stap 1 en in de ORC tabel invoegen

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Als de tabel TEXTFILE *& #60; databasenaam >. & #60; tabelnaam externe textfile >* partities in stap 3 is de `SELECT * FROM <database name>.<external textfile table name>` opdracht selecteert u de variabele partitie als een veld in de geretourneerde gegevens. Invoegt beperkt houden de *& #60; databasenaam >. & #60; tabelnaam ORC >* mislukt sinds *& #60; databasenaam >. & #60; tabelnaam ORC >* hoeft niet de variabele partitie als een veld in het tabelschema. In dit geval moet u specifiek selecteert u de velden worden ingevoegd op *& #60; databasenaam >. & #60; tabelnaam ORC >* als volgt:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Veilig kunt neerzetten de *& #60; tabelnaam externe textfile >* wanneer nadat alle gegevens met behulp van de volgende query is ingevoegd in *& #60; databasenaam >. & #60; tabelnaam ORC >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Na het volgen van deze procedure hebt u een tabel met gegevens in de indeling ORC klaar voor gebruik.  
