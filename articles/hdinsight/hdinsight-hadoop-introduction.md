 <properties
    pageTitle="Inleiding tot het Hadoop - wat is Hadoop op HDInsight? | Microsoft Azure"
    description="Kennismaking met Hadoop, gedistribueerde verwerking van big data en analyse en de onderdelen van het Hadoop-ecosysteem in de cloud op HDInsight."
    keywords="grote gegevensanalyse, hadoop, wat is hadoop, hadoop technologiestack, hadoop-ecosysteem-Inleiding"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Een inleiding tot het Hadoop-ecosysteem op Azure HDInsight

 Dit artikel bevat een inleiding tot Hadoop op Azure HDInsight, haar ecosysteem en big data. Meer informatie over de onderdelen Hadoop, gemeenschappelijke terminologie en scenario's voor gegevensanalyse groot.

## <a name="what-is-hadoop-on-hdinsight"></a>Wat is Hadoop op HDInsight?

Hadoop verwijst naar een ecosysteem van open source-software is een raamwerk voor gedistribueerde verwerking, opslag en analyse van grote gegevenssets in clusters van computers. Azure HDInsight maakt het Hadoop-onderdelen van de distributie **Hortonworks Data Platform (HDP)** beschikbaar in de cloud, en implementeert en bepalingen beheerde clusters met hoge betrouwbaarheid en beschikbaarheid.  

Apache Hadoop is het oorspronkelijke project van open source voor grote gegevens verwerken. Volgende is de ontwikkeling van verwante software en hulpprogramma's die deel uitmaken van de stack Hadoop-technologie, met inbegrip van Apache component, HBase Apache Apache Spark en vele andere. Overzicht [van het Hadoop-ecosysteem in HDInsight](#overview) voor meer informatie.

## <a name="what-is-big-data"></a>Wat is big data?

Grote gegevens beschrijft een groot tekstgedeelte van digitale informatie uit de tekst in een Twitter feed aan de sensor gegevens van industriële apparatuur naar informatie over klanten bladeren en aankopen op een website. Grote gegevens worden historische (wat betekent dat opgeslagen gegevens) of real-time (wat betekent gestreamd rechtstreeks van de bron). Grote gegevens worden verzameld in de-escaleren volumes op de luchtsnelheden steeds sneller, en een uitbreiding diverse indelingen.

Voor grote gegevens inzichtelijker intelligence of inzicht te verstrekken, moet u de relevante gegevens te verzamelen en de juiste vragen stellen. U moet ook controleren of dat de gegevens zijn toegankelijk, gereinigd, geanalyseerd en vervolgens weergegeven in een handige manier. Dat is waar grote gegevensanalyse op Hadoop in HDInsight kan helpen.

## <a name="overview"></a>Overzicht van het Hadoop-ecosysteem in HDInsight

HDInsight is een verdeling van de wolk op Microsoft Azure van de snel groeiende Apache Hadoop-technologie-stack voor grote gegevensanalyse. Het bevat implementaties van Apache Spark, HBase, Storm, varkens, component, Sqoop, Oozie, Ambari, enzovoort. HDInsight is ook geïntegreerd met de business intelligence (BI) tools zoals Power BI, Excel, SQL Server Analysis Services en SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase Spark, Storm en aangepaste clusters

HDInsight biedt voor Apache Hadoop, Spark, HBase of Storm-clusterconfiguraties. Of u kunt [clusters met scriptacties aanpassen](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: biedt betrouwbare gegevensopslag [HDFS](#hdfs)en een eenvoudige [MapReduce](#mapreduce) programmeermodel te verwerken en analyseren van gegevens in parallel.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: een parallelle verwerking framework ondersteunt de verwerking in het geheugen te verhogen de prestaties van toepassingen, grote gegevensanalyse, Wek werken voor SQL-gegevens, streaming en machine learning. Zie [Overzicht: Wat is Apache Spark in HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: een NoSQL database gebouwd op Hadoop met willekeurige toegang en sterke samenhang voor grote hoeveelheden niet-gestructureerde en semi-gestructureerde gegevens - potentieel miljarden rijen kolommen miljoenen keren. Overzicht [van HBase op HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>**: een berekening van gedistribueerde, real-time systeem voor het snel verwerken van grote stromen van gegevens. Storm wordt aangeboden als een beheerde cluster in HDInsight. Raadpleeg [real-time sensorgegevens analyseren met behulp van Storm en Hadoop](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Voorbeeldscripts voor aanpassing

Scriptacties zijn scripts die worden uitgevoerd tijdens het inrichten van de cluster en kunnen worden gebruikt voor het installeren van aanvullende onderdelen op het cluster. Dit zijn Bash scripts voor Linux gebaseerde clusters.

De volgende voorbeeldscripts worden geleverd door het team van HDInsight:

* [Tint](hdinsight-hadoop-hue-linux.md): een verzameling van webtoepassingen die worden gebruikt voor interactie met een cluster. Linux clusters.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): grafiek verwerken van model relaties tussen dingen of mensen.

* [R](hdinsight-hadoop-r-scripts-linux.md): een open source-taal en omgeving voor statistische berekeningen die worden gebruikt in de machine learning.

* [Solr](hdinsight-hadoop-solr-install-linux.md): een platform voor het zoeken van onderneming schaal waarmee zoeken in volledige tekst van de gegevens.

Zie voor meer informatie over het ontwikkelen van uw eigen acties Script [scriptactie ontwikkelen met HDInsight](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Wat zijn de hulpprogramma's en onderdelen van Hadoop?

De volgende onderdelen en hulpprogramma's zijn opgenomen in de HDInsight-clusters.

* **[Ambari](#ambari)**: inrichten, beheer, toezicht en hulpprogramma's voor het Cluster.

* **[Avro](#avro)** (Microsoft .NET-bibliotheek voor Avro): serialisatie van gegevens voor de Microsoft .NET-omgeving.

* **[Component & HCatalog](#hive)**: Structured Query Language (SQL)-zoals het uitvoeren van query's en een tabel- en management-laag.

* **[Mahout](#mahout)**: leren werken met toepassingen voor schaalbare machine.

* **[MapReduce](#mapreduce)**: Legacy kader voor Hadoop distributed verwerking en het beheer. Zie [GAREN](#yarn), het kader van de nieuwe resource.

* **[Oozie](#oozie)**: beheer van de Workflow.

* **[Phoenix](#phoenix)**: relationele databaselaag via HBase.

* **[Varken](#pig)**: eenvoudiger scripting voor MapReduce transformaties.

* **[Sqoop](#sqoop)**: gegevens importeren en exporteren.

* **[Tez](#tez)**: kunt u gegevensintensieve processen doeltreffend uit te voeren op de schaal.

* **[GAREN](#yarn)**: deel van de volgende generatie van MapReduce software framework en Hadoop core library.

* **[ZooKeeper](#zookeeper)**: coördinatie van de processen voor gedistribueerde systemen.

> [AZURE.NOTE] Zie voor informatie over specifieke onderdelen en versie-informatie [Hadoop onderdelen, versioning, en dienstverlening in HDInsight][component-versioning]

### <a name="ambari"></a>Ambari

Apache-Ambari is voor het inrichten, beheren en controleren van clusters van Apache Hadoop. Het bevat een intuïtieve verzameling hulpprogramma's de operator en een robuuste set van API's die de complexiteit van Hadoop, vereenvoudiging van de werking van clusters te verbergen. Linux-gebaseerde HDInsight clusters bieden zowel de Ambari web UI en de REST API voor Ambari, terwijl de Windows-gebaseerde clusters bieden een subset van de REST-API. Weergaven van Ambari op HDInsight clusters kunt invoegtoepassing UI-mogelijkheden.

Zie [HDInsight beheren van clusters met Ambari](hdinsight-hadoop-manage-ambari.md) (alleen voor Linux) [Monitor Hadoop clusters in HDInsight met de Ambari-API](hdinsight-monitor-use-ambari-api.md)en <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API reference</a>.

### <a name="avro"></a>Avro (bibliotheek van Microsoft .NET voor Avro)

De bibliotheek van Microsoft .NET voor Avro implementeert de Apache Avro interchange-indeling voor serialisatie voor het milieu van Microsoft .NET compact binaire gegevens. <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> wordt gebruikt voor het definiëren van een taal agnostic schema dat interoperabiliteit van talen beleggingsrendement, zin gegevens geserialiseerd in één taal in een andere kunnen worden gelezen. Gedetailleerde informatie over de indeling kan worden gevonden in het < doel = _ "lege" href = "http://avro.apache.org/docs/current/spec.html" > Apache Avro specificatie</a>.
De indeling van de Avro-bestanden ondersteunt het programmeermodel gedistribueerde MapReduce. De bestanden zijn 'splitsbare", wat betekent dat u kunt zoeken naar elk punt in een bestand en start het lezen van een bepaald blok. Als u wilt weten hoe, Zie [geserialiseerd gegevens met de Microsoft .NET-bibliotheek voor de Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Hadoop Distributed bestand System (HDFS) is een gedistribueerd bestandssysteem die met MapReduce en garens, de kern van het Hadoop-ecosysteem is. HDFS is het standaard bestandssysteem voor clusters van Hadoop op HDInsight.

### <a name="hive"></a>Component & HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache-component</a> is software voor magazijn gegevens gebaseerd op Hadoop waarmee u kunt opvragen en beheren van grote datasets in gedistribueerde opslag met behulp van een SQL-achtige taal HiveQL genoemd. Component, zoals varkens, is een abstractie op MapReduce. Wanneer uitgevoerd, zet component om query's in een reeks taken MapReduce. Component is conceptueel dichter bij een relationele database management systeem dan geslachte varkens, en is daarom geschikt voor gebruik met meer gestructureerde gegevens. Ongestructureerde gegevens is varken een betere keuze. Zie [component met Hadoop in HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache-HCatalog</a> is een tabel- en management-laag voor Hadoop waarin gebruikers met een relationele weergave van gegevens. In HCatalog, kunt u lezen en schrijven van bestanden in een indeling waarvoor een SerDe component (serializer deserializer) kan worden geschreven.

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> is een schaalbare bibliotheek van machine learning algoritmen die worden uitgevoerd op Hadoop. Met de beginselen van statistiek, leren machine learning toepassingen systemen voor meer gegevens en met het verleden resultaten toekomstige gedrag bepalen. Zie [genereren film aanbevelingen met Mahout op Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
Het kader van de oude software voor Hadoop is MapReduce voor het schrijven van toepassingen met grote gegevenssets proces parallel batch. Een taak MapReduce splitst grote gegevenssets en organiseert u de gegevens in de sleutel / waarde-paren voor verwerking.

[GAREN](#yarn) is het raamwerk Hadoop nieuwe resource manager en de toepassing en MapReduce 2.0 genoemd. MapReduce taken uitvoeren op garens.

Zie voor meer informatie over MapReduce, <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> in de Hadoop Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Oozie van Apache</a> is een workflow coördinatie-systeem dat Hadoop taken worden beheerd. Het is geïntegreerd in de stack Hadoop en ondersteunt Hadoop taken voor MapReduce, varkens, component en Sqoop. Het kan ook worden gebruikt voor het plannen van taken die specifiek zijn voor een systeem, zoals Java-programma's of shell-scripts. Zie [Oozie voor gebruik met Hadoop](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> is een relationele databaselaag via HBase. Phoenix bevat een JDBC-stuurprogramma waarmee gebruikers kunnen opvragen en beheren van SQL-tabellen rechtstreeks. Phoenix zet query's en andere instructies in native NoSQL API-aanroepen - in plaats van met behulp van MapReduce - zodat sneller toepassingen NoSQL winkels. Zie [Gebruik Apache Phoenix en eekhoorn HBase clusters](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Varken
<a  target="_blank" href="http://pig.apache.org/">Apache varken</a> is een high-level platform waarmee u complexe MapReduce transformaties op zeer grote gegevenssets uitvoeren met behulp van een eenvoudige scripttaal varken Latijn. Varken zet de Latijnse varken scripts zodat deze binnen het Hadoop wordt uitgevoerd. U kunt door de gebruiker gedefinieerde functies (UDF's) uit te breiden Latijn varken maken. Zie [Gebruik varken met Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache-Sqoop</a> is een hulpprogramma dat overdrachten bulk-gegevens tussen Hadoop en relationele databases die een SQL- of andere winkels gestructureerde gegevens zo efficiënt mogelijk. Zie [Sqoop voor gebruik met Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> is een application framework gebaseerd op Hadoop garens die complexe, acyclische grafieken van algemene gegevensverwerking wordt uitgevoerd. Het is een meer flexibele en krachtige opvolger van de MapReduce framework waarmee data-intensieve processen, zoals component efficiënter worden uitgevoerd op een schaal. Zie ["Apache Tez gebruiken voor betere prestaties", in gebruik component en HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>GARENS
GARENS van Apache is de volgende generatie van MapReduce (MapReduce 2.0 of MRv2) en ondersteunt gegevensverwerking scenario's buiten de batchverwerking MapReduce met meer schaalbaarheid en real-time verwerking. GARENS biedt bronnenbeheer en het kader van een gedistribueerde toepassing. MapReduce taken uitvoeren op garens.

Zie informatie over GAREN, <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (GAREN)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> coördineert processen in grote gedistribueerde systemen door middel van een gedeelde hiërarchische naamruimte van gegevens registers (znodes). Znodes bevat kleine hoeveelheden meta-informatie die nodig is voor het coördineren van processen: status, locatie en configuratie.

## <a name="programming-languages-on-hdinsight"></a>Programmeertalen op HDInsight

HDInsight clusters--Hadoop, HBase, Storm en Spark clusters ondersteunen verschillende programmeertalen, maar sommige worden niet standaard geïnstalleerd. Voor bibliotheken, modules of pakketten niet standaard geïnstalleerd, door een scriptactie te gebruiken om het onderdeel te installeren. Zie [scripts actie te ontwikkelen met HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Standaard ondersteuning voor programmeren

HDInsight-standaard ondersteuning voor clusters:

* Java

* Python

Extra talen kunnen worden geïnstalleerd met behulp van scriptacties: [ontwikkeling van de actie Script met HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java virtual machine (JVM) talen

Veel talen dan Java kunnen worden uitgevoerd met een Java virtual machine (JVM); Sommige van deze talen wordt uitgevoerd mogelijk echter extra onderdelen zijn geïnstalleerd op het cluster.

Deze talen JVM gebaseerd worden op HDInsight-clusters ondersteund:

* Clojure

* Jython (Python voor Java)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-specifieke talen

HDInsight clusters ondersteunen de volgende talen die specifiek voor het Hadoop-ecosysteem zijn:

* Latijn voor varkens taken varken

* HiveQL voor component-functies en SparkSQL


## <a name="advantage"></a>Voordelen van Hadoop in de cloud

Als onderdeel van het ecosysteem Azure cloud biedt Hadoop in HDInsight een aantal voordelen, onder hen:

* Automatisch inrichten van Hadoop-clusters. HDInsight clusters zijn veel gemakkelijker te maken dan het handmatig configureren van Hadoop-clusters. Zie voor meer informatie [verstrekken Hadoop clusters in het HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* The art Hadoop onderdelen. Zie voor details [Hadoop onderdelen, versioning, en dienstverlening in HDInsight][component-versioning].

* Hoge betrouwbaarheid en beschikbaarheid van clusters. Zie [beschikbaarheid en betrouwbaarheid van Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md) voor meer informatie.

* Efficiënte en voordelige gegevensopslag met Azure Blob-opslag, een optie voor het Hadoop-compatibel. Zie [Gebruik Azure Blob-opslag met Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) voor meer informatie.

* Integratie met andere Azure diensten, met inbegrip van [Web apps](https://azure.microsoft.com/documentation/services/app-service/web/) en [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/).

* Aanvullende VM maten en typen voor het uitvoeren van HDInsight clusters. Zie [onderdelen van Hadoop, versiebeheer, en dienstverlening in HDInsight] [ component-versioning] voor meer informatie.

* Schaal van het cluster. Cluster schalen kunt u het aantal knooppunten van een cluster met actieve HDInsight wijzigen zonder te verwijderen of opnieuw te maken.

* Ondersteuning voor virtuele netwerken. HDInsight clusters kunnen worden gebruikt met Azure virtueel netwerk ter ondersteuning van isolatie van bronnen voor cloud of hybride-scenario's die cloud resources die in uw datacenter te koppelen.

* Lage kosten-vermelding. Start een [gratis proefversie](https://azure.microsoft.com/free/)of Raadpleeg [HDInsight prijsgegevens](https://azure.microsoft.com/pricing/details/hdinsight/).

Meer informatie over de voordelen van Hadoop in HDInsight, Zie de [pagina Azure onderdelen voor HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard en Premium HDInsight

HDInsight biedt een grote gegevens wolk aanbiedingen in twee categorieën, Standard en Premium. HDInsight Standard biedt een cluster onderneming schaal waarmee organisaties hun werkbelasting grote gegevens uitvoeren. HDInsight Premium is gebaseerd op die en biedt geavanceerde analytische en -mogelijkheden voor een HDInsight-cluster. Zie voor meer informatie, [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Bronnen voor meer informatie over big data-analyse, Hadoop en HDInsight

Bouwen op deze inleiding op Hadoop in de cloud en grote gegevensanalyse in de onderstaande bronnen.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop-documentatie voor HDInsight

* [HDInsight-documentatie](https://azure.microsoft.com/documentation/services/hdinsight/): de documentatiepagina voor Hadoop op Azure HDInsight met koppelingen naar artikelen, video's en meer bronnen.

* [Aan de slag met Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): een quick start-zelfstudie voor clusters HDInsight Hadoop inrichten en monster component query's uitvoeren.

* [Aan de slag met vonkontsteking in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): een quick start-zelfstudie voor het maken van een cluster Spark en interactieve Spark SQL-query's uitvoeren.

* [Gebruik R Server op HDInsight](hdinsight-hadoop-r-server-get-started.md): R-Server gebruiken in HDInsight Premium.

* [Bepaling HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md): informatie over het inrichten van een cluster HDInsight Hadoop via de portal Azure, Azure CLI of Azure PowerShell.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: meer informatie over de Apache Hadoop software library, een raamwerk waarmee de gedistribueerde verwerking van grote datasets in clusters van computers.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: meer informatie over de architectuur en het ontwerp van de Hadoop Distributed File System, het primaire systeem gebruikt door toepassingen die Hadoop.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce-zelfstudie</a>: meer informatie over het programmeren kader voor het schrijven van Hadoop-toepassingen die snel grote hoeveelheden gegevens tegelijkertijd in grote clusters van computerknooppunten verwerken.


### <a name="microsoft-business-intelligence"></a>Microsoft business intelligence

Bekende business intelligence (BI) tools - zoals Excel, PowerPivot, SQL Server Analysis Services en SQL Server Reporting Services - ophalen, analyseren en gegevens die zijn geïntegreerd met HDInsight met de Power Query in of het ODBC-stuurprogramma Microsoft component.

Deze BI-hulpprogramma's kunt u uw big data analyse:

* [Excel verbinden aan Hadoop met Power Query](hdinsight-connect-excel-power-query.md): informatie over het aansluiten van Excel naar de opslag van Azure rekening waarin de gegevens die zijn gekoppeld aan het cluster HDInsight met Microsoft Power Query voor Excel. Windows-werkstation nodig is. Als u werkt met Windows - of Linux-gebaseerde clusters.

* [Hadoop met component ODBC-stuurprogramma voor de Microsoft Excel verbinding](hdinsight-connect-excel-hive-odbc-driver.md): informatie over het importeren van gegevens uit de HDInsight met het ODBC-stuurprogramma Microsoft component. Windows-werkstation nodig is. Als u werkt met Windows - of Linux-gebaseerde clusters.

* [Microsoft Cloud-Platform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informatie over Power BI voor Office 365, download de evaluatieversie van SQL Server en SharePoint Server 2013 en SQL Server BI instellen.

* [SQL Server analyseservices](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
