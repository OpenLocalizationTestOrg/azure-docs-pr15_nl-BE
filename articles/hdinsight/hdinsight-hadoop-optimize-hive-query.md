<properties
   pageTitle="Optimaliseer uw component query's sneller wordt uitgevoerd in HDInsight | Microsoft Azure"
   description="Informatie over het optimaliseren van uw component query's voor Hadoop in HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Component query's optimaliseren voor Hadoop in HDInsight

Hadoop clusters zijn standaard niet geoptimaliseerd voor prestaties. Dit artikel heeft betrekking op enkele van de meest voorkomende component prestaties optimalisatie methoden die u kunt toepassen op onze vragen.

##<a name="scale-out-worker-nodes"></a>Schaal van de knooppunten van de werknemer

Verhoging van het aantal knooppunten in een cluster werknemer kan gebruikmaken van meer mappers en verkleiningstoestellen parallel worden uitgevoerd. Er zijn twee manieren kunt u de schaal van HDInsight verbeteren:

- Op het moment van levering, kunt u het nummer van de werknemer knooppunten met behulp van de Portal Azure, Azure PowerShell of opdrachtregelinterface Cross-platform.  Zie [voorziening HDInsight clusters](hdinsight-provision-clusters.md)voor meer informatie. Het volgende scherm de werknemer knooppuntconfiguratie weergeven op de Azure Portal:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Tijdens de runtime kunt u ook de schaal van een cluster zonder opnieuw te maken in een. Dit wordt hieronder weergegeven.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Voor meer informatie over de verschillende virtuele machines die worden ondersteund door HDInsight, Zie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Tez inschakelen

[Apache Tez](http://hortonworks.com/hadoop/tez/) is een motor alternatieve uitvoering aan de MapReduce-engine:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez verloopt sneller omdat:

- Gestuurde acyclische grafiek (DAG) als in de engine MapReduce één taak uitvoeren, de DAG die wordt vermeld, moet elke set van mappers wordt gevolgd door een reeks verkleiningstoestellen. Dit zorgt ervoor dat meerdere MapReduce taken om te worden gesponnen uit voor elke query component. Tez heeft geen dergelijke beperking en complexe DAG kunnen worden verwerkt als één taak taak starten overhead dus tot een minimum beperken.
- **Onnodige Avoids schrijft** Door meerdere projecten worden gesponnen voor dezelfde query component in de MapReduce-engine, wordt de uitvoer van elke taak naar HDFS geschreven voor intermediaire gegevens. Aangezien Tez aantal taken voor elke component query kan beperkt om te voorkomen dat onnodige schrijven.
- **Minimizes opstarten vertragingen** Tez kan beter opstarten vertraging beperken door vermindering van het aantal mappers nodig is om te beginnen en ook verbetering van optimalisatie in.
- **Gebruik van containers** Wanneer mogelijk Tez kan hergebruiken containers om ervoor te zorgen dat vertraging als gevolg van containers gestart wordt verminderd.
- **Continue optimalisatie technieken** Traditioneel is optimalisatie gedaan tijdens de compilatie. Maar er meer informatie over de invoer beschikbaar is waardoor voor betere optimalisatie tijdens runtime. Tez wordt continu optimalisatie technieken waarmee het plan nog in de fase uitvoering optimaliseren.

Voor meer informatie over deze concepten, klik [hier](http://hortonworks.com/hadoop/tez/)

Kunt u elke query component Tez ingeschakeld door het voorvoegsel van de query met de volgende instelling:

    set hive.execution.engine=tez;

Voor HDInsight op basis van Windows-clusters moet Tez worden ingeschakeld op het moment van levering. Hier volgt een voorbeeld van Azure PowerShell script voor het inrichten van een cluster Hadoop met Tez ingeschakeld:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux-gebaseerde HDInsight clusters hebben Tez is standaard ingeschakeld.
    

## <a name="hive-partitioning"></a>Component partitioneren

I/o-bewerking is de grote bottleneck voor het uitvoeren van query's component. De prestaties kunnen worden verbeterd als de hoeveelheid gegevens die moeten worden gelezen kan worden verkleind. Component query's scannen gehele component tabellen standaard. Dit is ideaal voor query's zoals tabel scant voor query's die alleen nodig voor het scannen van een kleine hoeveelheid gegevens (bijvoorbeeld query's filteren), maakt dit echter onnodige overhead. Component partitionering kunt u query's component voor toegang tot de benodigde hoeveelheid gegevens in tabellen component.

Partitionering van de component wordt geïmplementeerd door de onbewerkte gegevens ordenen in nieuwe mappen met elk een eigen map - waar de partitie wordt gedefinieerd door de gebruiker die partitie. In het volgende diagram ziet u een component-tabel op de kolom *jaar*partitioneren. Voor elk jaar wordt een nieuwe map gemaakt.

![partitioneren][image-hdi-optimize-hive-partitioning_1]

Sommige partitionering overwegingen:

- **Doet niet onder partitie** - partitionering voor kolommen met slechts enkele waarden kunnen leiden tot zeer weinig partities. Zoals wordt partitionering van geslacht slechts twee partities worden gemaakt (mannelijk en vrouwelijk) te maken, dus alleen verminderen de latentie met maximaal de helft.

- **Te weinig niet partitioneren** - op het andere uiterste maken van een partitie op een kolom met een unieke waarde (bijvoorbeeld gebruikersnaam), waardoor meerdere partities een zware belasting op de namenode van het cluster veroorzaakt als er voor het verwerken van de grote hoeveelheid mappen.

- **Gegevens scheef** - verstandig uw sleutel partitionering kiezen zodat alle partities zelfs grootte zijn. Een voorbeeld is gepartitioneerd in *staat* kan tot gevolg hebben dat het aantal records in Californië zijn bijna 30 x van Vermont vanwege het verschil in de populatie.

Een als partitietabel wilt maken, gebruikt u de component *Door gepartitioneerd* :

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Zodra de gepartitioneerde tabel is gemaakt, kunt u partitioneren van statische of dynamische partities maken.

- **Statische partitionering** betekent dat er al een laptopgeheugen van gegevens in de juiste mappen kunt u de component partities handmatig op basis van de locatie van de map vragen. Dit wordt weergegeven in het onderstaande stukje code.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dynamische partitionering** betekent dat component partities automatisch voor u te maken. Aangezien we al de partitionering tabel uit de staging-tabel hebt gemaakt, is moeten we de gegevens naar de gepartitioneerde tabel invoegen, zoals hieronder wordt weergegeven:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Zie [Tabellen gepartitioneerd](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables)voor meer informatie.

##<a name="use-the-orcfile-format"></a>Gebruik de notatie ORCFile

Component ondersteunt verschillende bestandsindelingen. Bijvoorbeeld:

- **Tekst**: dit is de standaardbestandsindeling en werkt met de meeste scenario's
- **Avro**: geschikt voor interoperabiliteit scenario's
- **ORC/parketvloeren**: geschikt voor prestaties

ORC (geoptimaliseerd rij in kolomvorm)-indeling is een zeer efficiënte manier voor het opslaan van gegevens van de component. Vergeleken met andere indelingen, heeft ORC de volgende voordelen:

- ondersteuning voor complexe gegevenstypen, zoals datum/tijd en complexe en semi-gestructureerde typen
- maar liefst 70% compressie
- indexen van elke 10.000 rijen waarmee u rijen overslaan
- een duidelijke daling van de runtime-uitvoering

Zodat de indeling voor ORC maakt u een tabel met de component *opgeslagen als ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Vervolgens plaatst u gegevens aan de tabel ORC uit de staging-tabel. Bijvoorbeeld:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

U kunt meer lezen over de ORC-indeling [hier](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization kan component voor het verwerken van een partij van 1024 rijen bij elkaar in plaats van het verwerken van één rij tegelijk. Dit betekent dat zijn eenvoudige bewerkingen sneller uitgevoerd omdat minder interne code moet worden uitgevoerd.

Vectorization voorvoegsel zodat uw query component met de volgende instellingen:

    set hive.vectorized.execution.enabled = true;

Zie voor meer informatie [Vectorized query kan worden uitgevoerd](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Andere methoden van optimalisatie

Er zijn meer optimalisatie-methoden die u, bijvoorbeeld overwegen kunt:

- **Component bucketing:** een techniek waarmee het cluster of segment grote reeksen gegevens optimaliseren van de prestaties van query's.
- **Deelnemen aan optimalisatie:** optimalisatie van de component in de uitvoering van de query wilt verbeteren van de efficiëntie van joins en verminderen de noodzaak voor de gebruiker aanbevolen basisservers. Zie [lid worden van optimalisatie](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization)voor meer informatie.
- **verkleiningstoestellen vergroten**

##<a id="nextsteps"></a>Volgende stappen
In dit artikel, kunt u verschillende veelgebruikte component query optimalisatie methoden hebt geleerd. Voor meer informatie, Zie de volgende artikelen:

- [Apache-component in de HDInsight gebruiken](hdinsight-use-hive.md)
- [Vlucht vertraging gegevens analyseren met behulp van de component in HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Twitter analyseren met behulp van de component in HDInsight](hdinsight-analyze-twitter-data.md)
- [Met de Query component Console op Hadoop in HDInsight sensorgegevens analyseren](hdinsight-hive-analyze-sensor-data.md)
- [Component gebruiken met HDInsight voor het analyseren van Logboeken van websites](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
