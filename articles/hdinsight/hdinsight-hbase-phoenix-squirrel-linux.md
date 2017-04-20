<properties 
   pageTitle="Apache gebruikt Phoenix en eekhoorn in HDInsight | Microsoft Azure" 
   description="Informatie over Phoenix Apache gebruiken in HDInsight en het installeren en configureren van eekhoorn op uw werkstation verbinding maken met een cluster HBase in HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Phoenix Apache gebruiken met HBase Linux gebaseerde clusters in HDInsight  

Informatie over [Phoenix Apache](http://phoenix.apache.org/) gebruiken in HDInsight en SQLLine gebruiken. Zie voor meer informatie over Phoenix [Phoenix in 15 minuten of minder](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Zie voor de grammatica Phoenix [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Zie voor de versiegegevens voor de Phoenix in HDInsight, [Wat is er nieuw in de versies van Hadoop cluster geleverd door HDInsight?] [hdinsight-versions].

##<a name="use-sqlline"></a>SQLLine gebruiken
[SQLLine](http://sqlline.sourceforge.net/) is een opdrachtregelprogramma voor het uitvoeren van SQL. 

###<a name="prerequisites"></a>Vereisten
Voordat u SQLLine gebruiken kunt, hebt u het volgende:

- **Cluster A HBase in HDInsight**. Voor meer informatie over de voorziening HBase cluster, Zie [aan de slag met Apache HBase in HDInsight][hdinsight-hbase-get-started].
- **Verbinding maken met het cluster HBase via remote desktop protocol**. Zie voor instructies [de clusters beheren Hadoop in HDInsight met de klassieke Portal Azure][hdinsight-manage-portal].


Wanneer u verbinding met een HBase-cluster maakt, moet u verbinding maken met een van de zoohouder. Elk cluster HDInsight heeft 3 zoohouder. 

**Om erachter te komen dat de host-naam Zookeeper**

1. Ambari openen door te bladeren naar **https://<ClusterName>. azurehdinsight.net**.
2. HTTP (cluster) gebruikersnaam en wachtwoord invoeren om in te loggen.
3. Klik op **ZooKeeper** in het linkermenu. Er wordt 3 **ZooKeeper Server** weergegeven.
4. Klik op een van de **Server ZooKeeper** vermeld. Zoeken naar de **hostnaam**in het deelvenster Overzicht. Dit is vergelijkbaar met *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine gebruiken**

1. Verbinding maken met het cluster via SSH. Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) of [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md) voor instructies, afhankelijk van uw computer OS.

2. Voer de volgende opdrachten uit te voeren SQLLine van SSH:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Voer de volgende opdrachten voor het maken van een tabel HBase en sommige gegevens invoegen:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Zie [handleiding voor SQLLine](http://sqlline.sourceforge.net/#manual) en [Phoenix grammatica](http://phoenix.apache.org/language/index.html)voor meer informatie.


 
##<a name="next-steps"></a>Volgende stappen
In dit artikel, kunt u gebruiken Apache Phoenix in HDInsight hebt geleerd.  Zie voor meer informatie

- [Overzicht HDInsight HBase][hdinsight-hbase-overview]: HBase is een Apache open source NoSQL database gebouwd op Hadoop met willekeurige toegang en sterke samenhang voor grote hoeveelheden ongestructureerde en semistructured-gegevens.
- [HBase clusters op Azure Virtual Network inrichten][hdinsight-hbase-provision-vnet]: virtueel netwerk integratie, HBase clusters worden geïmplementeerd op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen rechtstreeks met HBase communiceren kunnen.
- [Replicatie configureren HBase in HDInsight](hdinsight-hbase-geo-replication.md): informatie over het configureren van replicatie HBase over twee Azure datacenters. 
- [Analyseren van Twitter sentiment met HBase in HDInsight][hbase-twitter-sentiment]: leren gaat real-time [sentiment analyse](http://en.wikipedia.org/wiki/Sentiment_analysis) van grote gegevens HBase in een cluster Hadoop in HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
