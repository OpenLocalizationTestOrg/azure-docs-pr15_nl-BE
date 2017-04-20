<properties
 pageTitle="Voorbeeld van de Apache Storm topologieën op HDInsight | Microsoft Azure"
 description="Een lijst met voorbeeld Storm topologieën gemaakt en getest met Apache Storm op HDInsight, met inbegrip van algemene topologieën voor C# en Java en het werken met gebeurtenis-Hubs."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="08/23/2016"
 ms.author="larryfr"/>

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Voorbeeld van de Storm toplogies en onderdelen voor Apache Storm op HDInsight

Hier volgt een lijst van voorbeelden gemaakt en wordt onderhouden door Microsoft voor gebruik met Apache Storm op HDInsight. Deze voorbeelden behandelen diverse onderwerpen, van het maken van eenvoudige C# en Java topologieën met Azure services zoals gebeurtenis Hubs, DocumentDB, Power BI, SQL-Database, HBase op HDInsight en opslag van Azure samen te werken. Enkele voorbeelden ziet u ook het werken met niet-Azure of zelfs niet-Microsoft-technologieën, zoals SignalR en Socket.IO

| Beschrijving                                                                                             | Laat zien                                         | Taal/Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Schrijven naar Azure Lake gegevensopslag van Apache Storm](hdinsight-storm-write-data-lake-store.md) | Schrijven naar Azure Lake gegevensarchief | Java |
| [Bron van gebeurtenis Hub Spout en bout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Bron van de gebeurtenis Hub Spout en bout | Java |
| [Ontwikkelen van Java gebaseerde topologieën voor Apache Storm op HDInsight][5797064f]                                 | Maven                                                | Java                       |
| [Ontwikkelen van C# topologieën voor Apache Storm op HDInsight met behulp van Visual Studio][16fce2d1]                     | Extra HDInsight voor Visual Studio                    | C#, Java                   |
| [Meerdere gegevensstromen in een C# Storm topologie maken][ec5a4064]                                         | Meerdere streams                                     | C#                         |
| [Twitter volgen van trends onderwerpen met Storm op HDInsight bepalen][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Gebeurtenissen laten verwerken Azure gebeurtenis Hubs met Storm op HDInsight (C#)][844d1d81]                                | Gebeurtenis Hubs                                           | C# en Java                |
| [Gebeurtenissen laten verwerken Azure gebeurtenis Hubs met Storm op HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Gebeurtenis Hubs | Java |
| [Power Bi gebruiken om te visualiseren van gegevens uit een topologie Storm][94d15238]                              | Power BI                                             | C#                         |
| [Analyseren van sensorgegevens met Storm en HBase in HDInsight][ab894747]                                     | Gebeurtenis Hubs, HBase, Socket.IO, dashboard Web          | C#, Java, JavaScript, HTML |
| [Verwerken van gegevens over het voertuig sensor van gebeurtenis Hubs met Storm op HDInsight][246ee964]                        | Gebeurtenis Hubs, DocumentDb, opslag Azure Blob (WASB)    | C#, Java                   |
| [Extraheren, transformeren en laden (ETL) van Azure gebeurtenis Hubs op HBase, met Storm op HDInsight][b4b68194] | Gebeurtenis Hubs, HBase                                    | C#                         |
| [Sjabloon voor C# Storm topologie project voor het werken met Azure services van Storm op HDInsight][ce0c02a2]  | Gebeurtenis Hubs, DocumentDb, SQL-Database, HBase, SignalR | C#, Java                   |
| [Schaalbaarheid-benchmarks voor het lezen van Azure gebeurtenis Hubs met Storm op HDInsight][d6c540e3]           | Doorvoer van berichten afneemt, gebeurtenis Hubs, SQL-Database         | C#, Java                   |
| [Gebeurtenissen met Storm en HBase in HDInsight correleren](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Python gebruiken met Storm op HDInsight](hdinsight-storm-develop-python-topology.md) | Python-onderdelen met Java en Clojure gebaseerd Storm topologieën | Python |

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Apache Storm op HDInsight][2b8c3488]

* [Meer informatie over het implementeren en beheren van Storm topologieën met Storm op HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Informatie over het maken een Storm op de cluster HDInsight en het Dashboard Storm voorbeeld topologieën implementeren."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Informatie over het implementeren en beheren met behulp van het Dashboard web gebaseerde Storm en Storm UI of de HDInsight's voor Visual Studio topologieën."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Informatie over het maken van topologieën Storm C# met behulp van de HDInsight's voor Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Informatie over Storm topologieën maken in Java, Maven, via een eenvoudige wordcount topologie maken."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Laat zien hoe gegevens schrijven naar Power BI van een topologie met C#, wordt een grafiek en dashboard maken van de gegevens."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Ziet u een elementaire Storm topologie die een wordcount, geïmplementeerd in C# wordt uitgevoerd. Dit toont ook aan meerdere gegevensstromen binnen C#-topologie maken."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Leren lezen en schrijven van gegevens van Azure gebeurtenis Hubs met Storm op HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Informatie over het gebruik van Apache Storm op HDInsight voor het verwerken van sensorgegevens van Azure gebeurtenis Hubs visualiseren met behulp van D3.js en (optioneel), slaat u deze op HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Leren werken met Trident voor het maken van een topologie Storm die volgen van trends onderwerpen (op basis van hashtags,) bepaalt op Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Documenten van Azure DocumentDB voor gegevens verwijzen naar informatie over het gebruik van een topologie Storm om berichten te lezen van Azure gebeurtenis Hubs, lezen en opslaan van gegevens in de opslag van Azure."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Verschillende topologieën zodat doorvoer tonen bij het lezen van Azure gebeurtenis Hubs en met Apache Storm op HDInsight SQL-database opslaan."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Informatie over het Azure gebeurtenis Hubs, statistische gegevens lezen en de gegevens transformeren en vervolgens opslaan op HBase op HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Dit project bevat sjablonen voor spouts, bouten en topologieën voor interactie met verschillende Azure services zoals gebeurtenis Hubs, DocumentDB en SQL-Database."
 
