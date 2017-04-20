<properties
pageTitle="Poorten die worden gebruikt door HDInsight | Azure"
description="Een lijst met poorten die worden gebruikt door Hadoop services op HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Poorten en gebruikt door HDInsight URI 's

Dit document bevat een overzicht van de poorten die worden gebruikt door Hadoop services op HDInsight op basis van Linux-clusters. Het biedt tevens informatie over de poorten die worden gebruikt voor verbinding met het cluster via SSH.

## <a name="public-ports-vs-non-public-ports"></a>Openbare poorten versus niet-openbare poorten

Linux-gebaseerde HDInsight clusters beschrijft slechts drie poorten openbaar op internet. 22, 23 en 443. Deze worden veilig toegang krijgen tot het cluster via SSH en services die via het beveiligde HTTPS-protocol gebruikt.

Intern HDInsight door verschillende Azure virtuele Machines (knooppunten binnen het cluster) is geïmplementeerd op een virtueel netwerk Azure. Van binnen het virtuele netwerk toegankelijk poorten niet beschikbaar zijn via het internet. Bijvoorbeeld, als u verbinding met een van de hoofd-knooppunten via SSH maakt, uit het hoofd knooppunt u kunt vervolgens rechtstreeks toegang tot services die worden uitgevoerd op de clusterknooppunten.

> [AZURE.IMPORTANT] Wanneer u een cluster HDInsight maken als u een virtueel netwerk Azure niet als een configuratieoptie opgeeft, wordt er een gemaakt; echter koppelen niet u andere computers (bijvoorbeeld andere Azure virtuele Machines of de clientcomputer ontwikkeling) aan deze virtuele netwerk automatisch gemaakt. 

Om extra computers toevoegen aan het virtuele netwerk, moet u eerst het virtuele netwerk maken en opgeven bij het maken van het cluster HDInsight. Zie voor meer informatie, de [mogelijkheden van de HDInsight uitbreiden met behulp van een virtueel netwerk Azure](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Openbare poorten

Alle knooppunten in een cluster HDInsight bevinden zich in een virtueel netwerk Azure en zijn niet rechtstreeks toegankelijk vanaf het internet. Een gateway openbare internettoegang biedt tot de volgende poorten die gebruikt in alle HDInsight clustertypen worden.

| Service | Poort | Protocol | Beschrijving |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Clients verbinding met sshd op de primaire headnode. Zie [werken met SSH met Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Clients verbinding met sshd op het randknooppunt (alleen HDInsight Premium). Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Clients verbinding met sshd op de secundaire headnode. Zie [werken met SSH met Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari web UI. Zie [HDInsight beheren met behulp van de Ambari Web UI](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari REST API. Zie [HDInsight beheren met behulp van de API Ambari REST](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog REST API. Zie [component met krul](hdinsight-hadoop-use-pig-curl.md), [varken met krul gebruiken](hdinsight-hadoop-use-pig-curl.md), [Gebruik MapReduce met krul](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Maakt verbinding met een component met behulp van ODBC. Zie [Excel HDInsight met het stuurprogramma Microsoft ODBC-verbinding](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Maakt verbinding met een component via JDBC. Zie [verbinding maken met de component in HDInsight met behulp van de component JDBC-stuurprogramma](hdinsight-connect-hive-jdbc-driver.md) |

Zijn beschikbaar voor een specifiek clustertypen:

| Service | Poort | Protocol |Clustertype | Beschrijving |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase REST API. Zie [aan de slag met HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livius | 443 | HTTPS |  Spark | Spark REST API. Zie [indienen Spark taken op afstand met behulp van Livius](hdinsight-apache-spark-livy-rest-interface.md) |
| Storm | 443 | HTTPS | Storm | Storm web UI. Zie [implementeren en beheren van Storm topologieën op HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Verificatie

Alle services die openbaar toegankelijk op het internet moeten worden geverifieerd:

| Poort | Referenties |
| ---- | ----------- |
| 22 of 23 | De referenties van de gebruiker SSH tijdens het maken van het cluster opgegeven |
| 443 | De aanmeldingsnaam (standaard: admin,) en het wachtwoord die zijn ingesteld tijdens het maken van het cluster |

## <a name="non-public-ports"></a>Niet-openbare poorten

> [AZURE.NOTE] Sommige services zijn alleen beschikbaar op een specifiek clustertypen. HBase is bijvoorbeeld alleen beschikbaar op HBase clustertypen.

### <a name="hdfs-ports"></a>HDFS poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode web UI | Hoofd-knooppunten | 30070 | HTTPS | On line gebruikersinterface voor de huidige status weergeven |
| NameNode metadata service | hoofd knooppunten | 8020 | IPC | Metagegevens van het systeem 
| DataNode | Alle knooppunten van de werknemer | 30075 | HTTPS | Web UI status weergeven, Logboeken, enz. |
| DataNode | Alle knooppunten van de werknemer | 30010 | &nbsp; | Overdracht van gegevens |
| DataNode | Alle knooppunten van de werknemer | 30020 | IPC | Bewerkingen van metagegevens |
| Secundaire NameNode | Hoofd-knooppunten | 50090 | HTTP | Controlepunt voor metagegevens van NameNode |

### <a name="yarn-ports"></a>GARENS van poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Resource Manager web UI | Hoofd-knooppunten | 8088 | HTTP | On line gebruikersinterface voor bronbeheer |
| Resource Manager web UI | Hoofd-knooppunten | 8090 | HTTPS | On line gebruikersinterface voor bronbeheer |
| Resource Manager admin interface | hoofd knooppunten | 8141 | IPC | Voor toepassing inzendingen (component, server component, varkens, enz.) |
| Scheduler Resource Manager | hoofd knooppunten | 8030 | HTTP | Beheerinterface |
| De toepassingsinterface Resource Manager | hoofd knooppunten | 8050 | HTTP |Adres van de interface voor het beheer van toepassingen |
| NodeManager | Alle knooppunten van de werknemer | 30050 | &nbsp; | Het adres van de manager van de container |
| NodeManager web UI | Alle knooppunten van de werknemer | 30060 | HTTP | Resource manager-interface |
| Adres van de tijdlijn | Hoofd-knooppunten | 10200 | RPC | De tijdlijn RPC service. |
| Tijdlijn web UI | Hoofd-knooppunten | 8181 | HTTP | De tijdlijn service web UI |

### <a name="hive-ports"></a>Component-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Hoofd-knooppunten | 10001 | Thrift | Service voor programmatisch component (Thrift/JDBC) |
| HiveServer | Hoofd-knooppunten | 10000 | Thrift | Service voor programmatisch component (Thrift/JDBC) |
| Component Metastore | Hoofd-knooppunten | 9083 | Thrift | Service voor programmatisch component metagegevens (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat server | Hoofd-knooppunten | 30111 | HTTP | Web API op HCatalog en andere Hadoop-services |

### <a name="mapreduce-ports"></a>MapReduce-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Hoofd-knooppunten | 19888 | HTTP | MapReduce JobHistory web UI |
| JobHistory | Hoofd-knooppunten | 10020 | &nbsp; | MapReduce JobHistory server |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Overdrachten tussenliggende kaart-uitgang verkleiningstoestellen aanvragen |

### <a name="oozie"></a>Oozie

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie server | Hoofd-knooppunten | 11000 | HTTP | URL voor de service Oozie |
| Oozie server | Hoofd-knooppunten | 11001 | HTTP | Poort voor Oozie admin |

### <a name="ambari-metrics"></a>Statistieken voor Ambari

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Tijdlijn (geschiedenis van toepassing) | Hoofd-knooppunten | 6188 | HTTP | De tijdlijn service web UI |
| Tijdlijn (geschiedenis van toepassing) | Hoofd-knooppunten | 30200 | RPC | De tijdlijn service web UI |

### <a name="hbase-ports"></a>HBase poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Hoofd-knooppunten | 16000 | &nbsp; | &nbsp; |
| HMaster info Web UI | Hoofd-knooppunten | 16010 | HTTP | De poort voor het web Master HBase UI |
| Regio-server | Alle knooppunten van de werknemer | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | De poort die clients gebruiken om verbinding met ZooKeeper |

### <a name="kafka-ports"></a>Kafka-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Makelaar  | Werknemer knooppunten | 9092 | [Kafka-kabelprotocol](http://kafka.apache.org/protocol.html) | Gebruikt voor de clientcommunicatie |
| &nbsp; | Zookeeper knooppunten | 2181 | &nbsp; | De poort die clients gebruiken om verbinding met Zookeeper |
