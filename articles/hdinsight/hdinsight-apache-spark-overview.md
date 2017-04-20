<properties 
    pageTitle="Een overzicht van Apache Spark in HDInsight | Microsoft Azure" 
    description="Een introductie van Apache Spark in HDInsight en scenario's waarin de motoren op HDInsight in uw toepassingen te gebruiken." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Overzicht: Apache Spark op HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> is een open source parallelle verwerking die ondersteuning biedt voor verwerking om zo de prestaties van analytische toepassingen van big data in het geheugen. Spark verwerkings-engine is gebouwd voor snelheid en gebruiksgemak, en geavanceerde analytische. Spark van berekening van de in het geheugen mogelijkheden maken het een goede keuze voor iteratieve algoritmen in de machine learning en grafiek berekeningen. Spark is ook compatibel met Azure Blob-opslag (WASB), zodat de bestaande gegevens opgeslagen in Azure via vonk kan gemakkelijk worden verwerkt.

Wanneer u een cluster Spark in HDInsight maakt, maakt u Azure compute bronnen met vonkontsteking geïnstalleerd en geconfigureerd. Alleen duurt ongeveer tien minuten voor het maken van een cluster Spark in HDInsight. De gegevens worden verwerkt, wordt opgeslagen in Azure Blob-opslag. Zie [Gebruik Azure Blob-opslag met HDInsight][hdinsight-storage].

![Apache Spark op Azure HDInsight] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark op Azure HDInsight")


**Wilt u aan de slag met Apache Spark op Azure HDInsight?** Zie [QuickStart: een cluster Spark op HDInsight Linux maken en uitvoeren van voorbeeldtoepassingen Jupyter met](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Zie voor een lijst met bekende problemen en beperkingen in verband met de huidige versie, de [bekende problemen van Apache Spark in Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Waarom Spark op Azure HDInsight gebruiken? 

Azure HDInsight biedt een volledig beheerde service van Spark. Voordelen van het gebruik van motoren op HDInsight zijn:

| Functie                             | Beschrijving       |
|-------------------------------------|-------------------|
| Het gemak van het maken van clusters            | Kunt u een nieuw cluster met motoren op HDInsight met de Azure Management Portal of Azure PowerShell .NET SDK HDInsight minuten. Zie [aan de slag met vonkontsteking cluster in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Gebruiksgemak                     | Spark in HDInsight clusters bevat Jupyter-laptops vooraf geconfigureerd. U kunt deze voor interactieve verwerking en visualisatie. De URL's voor de https://CLUSTERNAME.azurehdinsight.net/jupyter is. __CLUSTERNAAM__ vervangen door de naam van het cluster Spark HDInsight.|
| REST API 's                       | Spark in HDInsight bevat [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), een REST-API gebaseerd Spark Project server op afstand indienen en actieve taken controleren. |
| Ondersteuning voor Azure Lake gegevensarchief | Motoren op HDInsight kan worden geconfigureerd voor het gebruik van Azure Lake gegevensarchief als een extra opslagruimte. Zie voor meer informatie over gegevensopslag Lake, [Overzicht van Azure Lake gegevensarchief](../data-lake-store/data-lake-store-overview.md).
| Integratie met Azure services | Motoren op HDInsight wordt geleverd met een verbindingslijn met Azure gebeurtenis Hubs. Klanten kunnen bouwen streaming toepassingen met behulp van de gebeurtenis Hubs, naast [Kafka](http://kafka.apache.org/), die al beschikbaar is als onderdeel van een vonk. |
| Ondersteuning voor Server R  | U kunt een R-Server in het cluster HDInsight Spark gedistribueerde R berekeningen uitvoeren met de snelheden die met een cluster Spark toegezegde instellen. Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)voor meer informatie.   |
| Integratie met IntelliJ IDEE  | U kunt de HDInsight Plugin voor IntelliJ maken en indienen van een cluster HDInsight Spark-toepassingen. Zie [Gebruik HDInsight extra Plugin voor IntelliJ IDEE Spark om toepassingen te maken voor HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin.md)voor meer informatie. |
| Gelijktijdige query 's              | Spark in HDInsight ondersteunt gelijktijdige query's. Hierdoor kunnen meerdere query's van één gebruiker of meerdere query's van de verschillende gebruikers en toepassingen delen dezelfde clusterbronnen. |
| Caching op SSD                 | U kunt cachegegevens in het geheugen of in SSD aangesloten op de clusterknooppunten. Biedt de beste prestaties van query's opslaan in de cache in het geheugen, maar kan erg duur zijn; opslaan in cache in SSD biedt een geweldige optie voor het verbeteren van de queryprestaties hoeft te maken van een cluster met een grootte die nodig is voor de gehele dataset in het geheugen passen.|
| Integratie met BI-hulpprogramma 's       | Motoren voor HDInsight biedt aansluitingen voor BI-hulpprogramma's zoals [Power BI](http://www.powerbi.com/) en [Tableau](http://www.tableau.com/products/desktop) voor data analytics.|
| Bibliotheken met vooraf geladen Anaconda        | Spark clusters op HDInsight worden geleverd met vooraf geïnstalleerd exemplaar van Anaconda-bibliotheken. [Anaconda](http://docs.continuum.io/anaconda/) biedt bijna 200 bibliotheken voor machine learning, analyse van gegevens, visualisatie, enz.|
| Schaalbaarheid                     | Maar u het aantal knooppunten in het cluster tijdens het maken opgeven kunt, kunt u vergroten of verkleinen van het cluster overeenkomt met de belasting. Alle HDInsight clusters kunnen u het aantal knooppunten in het cluster wijzigen. Spark clusters kunnen ook worden weggehaald zonder verlies van gegevens, omdat alle gegevens opgeslagen in Azure Blob-opslag. |
| 24 x 7 support                    | Motoren op HDInsight wordt geleverd met 24 x 7-ondersteuning op bedrijfsniveau en een SLA van up-time van 99,9%.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Wat zijn de use-cases voor motoren op HDInsight?

Apache Spark in HDInsight kunnen de volgende belangrijke scenario's.

### <a name="interactive-data-analysis-and-bi"></a>Interactieve gegevensanalyse en BI

[Een zelfstudie bekijken](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark in HDInsight worden gegevens opgeslagen in Azure BLOB's. Zakelijke deskundigen en besluitvormers kunnen analyseren en rapporten samenstellen die gegevens en interactieve rapporten van de geanalyseerde gegevens maken met Microsoft Power BI. Analisten kunnen starten vanaf de ongestructureerde/semi gestructureerde gegevens in Azure opslag, een schema voor de gegevens met behulp van laptops definiëren en vervolgens samen met Microsoft Power BI gegevensmodellen. Spark in HDInsight ondersteunt ook een aantal programma's van derden BI zoals Tableau, Qlikview en SAP Lumira waardoor het een ideaal platform voor data-analisten, zakelijke deskundigen en besluitvormers.

### <a name="iterative-machine-learning"></a>Iteratieve Machine Learning

[Een zelfstudie bekijken: voorspellen temperaturen uisng HVAC-gegevens maken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Een zelfstudie bekijken: voedsel inspectieresultaten voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark is voorzien van [MLlib](http://spark.apache.org/mllib/), een bibliotheek gebaseerd op Spark leren machine. Daarnaast bevat de motoren op HDInsight ook Anaconda, een Python verdeling met een groot aantal pakketten voor machine learning. Combineer dit met een ingebouwde ondersteuning voor laptops voor Jupyter en u hebt een van de allerbeste omgeving voor het maken van machine learning-toepassingen.  

### <a name="streaming-and-real-time-data-analysis"></a>Streaming en real-time data-analyse

[Een zelfstudie bekijken](hdinsight-apache-spark-eventhub-streaming.md)

Real-time data-analyse wordt gebruikt voor scenario's, variërend van verkorting van inzicht in gegevens door de gegevens te verwerken zoals belandt, met een true streaming oplossing bouwen. Spark in HDInsight biedt een uitgebreide ondersteuning voor real-time analytics oplossingen. Terwijl de vonk al connectoren aan de consumptie van gegevens uit allerlei bronnen, zoals Kafka, Flume, Twitter, ZeroMQ of TCP-sockets, Spark in HDInsight eersteklas wordt ondersteuning toegevoegd voor ingesting gegevens van Azure gebeurtenis Hubs. Gebeurtenis Hubs zijn het meest gebruikte wachtrijservice op Azure. Met een out-of-the-box ondersteuning voor gebeurtenis Hubs Wek maakt in HDInsight een ideaal platform voor het bouwen van real-time analytics pijpleiding.

##<a name="next-steps"></a>Welke onderdelen worden opgenomen als onderdeel van een cluster Spark?

Spark in HDInsight bevat de volgende onderdelen die beschikbaar op de clusters op standaard zijn.

- [Spark Core](https://spark.apache.org/docs/1.5.1/). Spark Core, SQL Spark, Spark streaming API's, GraphX en MLlib bevat.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter-laptop](https://jupyter.org)

Spark in HDInsight biedt ook een [ODBC-stuurprogramma](http://go.microsoft.com/fwlink/?LinkId=616229) voor connectiviteit met vonkontsteking clusters in HDInsight van BI-hulpprogramma's zoals Microsoft Power BI en Tableau.

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

Beginnen met het maken van een cluster Spark op HDInsight Linux. Zie [QuickStart: een cluster Spark op HDInsight Linux maken en uitvoeren van voorbeeldtoepassingen Jupyter met](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Volgende stappen

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Motoren met Machine Learning: Spark in HDInsight voor het analyseren van gebouw temperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met vonkontsteking in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Maken en uitvoeren van toepassingen

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Taken op afstand uitvoeren op een vonk cluster met behulp van Livius](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Hulpprogramma's en -extensies

* [HDInsight extra Plugin voor IntelliJ IDEE maken en indienen van Spark Scala applicatons gebruiken](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Gebruik HDInsight extra Plugin voor IntelliJ IDEE Spark toepassingen op afstand fouten opsporen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Zeppelin notitieblokken gebruiken met een cluster motoren op HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernels die beschikbaar zijn voor de laptop in een cluster voor HDInsight Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Externe-pakketten gebruiken met Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter op uw computer installeren en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
