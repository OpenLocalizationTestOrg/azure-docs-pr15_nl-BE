<properties
    pageTitle="Wat is HBase in HDInsight? | Microsoft Azure"
    description="Een inleiding tot de Apache HBase in HDInsight, een database NoSQL bouwen op Hadoop. Informatie over de use-cases en HBase vergelijken met andere Hadoop-clusters."
    keywords="bigtable, nosql, wat is hbase"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Wat is HBase in HDInsight: A NoSQL database die voorziet in functies zoals BigTable voor Hadoop

HBase Apache is een open source NoSQL-database die is gebaseerd op Hadoop en gemodelleerd naar Google BigTable. HBase levert RAM en sterke samenhang voor grote hoeveelheden ongestructureerde en semistructured gegevens in een database schemaless georganiseerd door de families van de kolom.

Gegevens in de rijen van een tabel zijn opgeslagen en gegevens in een rij worden gegroepeerd per kolom familie. HBase is een schemaless database in die zin dat noch het type van de gegevens die zijn opgeslagen in de bestanden als de kolommen moeten worden gedefinieerd voordat u ze gebruikt. De open source code schaalt lineair petabytes van gegevens over duizenden knooppunten te verwerken. Het vertrouwen op gegevensredundantie, batch-verwerking en andere functies die worden geleverd door gedistribueerde toepassingen in het Hadoop-ecosysteem.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Hoe is de HBase in Azure HDInsight geïmplementeerd?

HDInsight HBase wordt aangeboden als een beheerde cluster die is geïntegreerd in de Azure-omgeving. De clusters zijn geconfigureerd voor het opslaan van gegevens rechtstreeks in Azure Blob-opslag met lage latentie en hogere elasticiteit in opties voor prestaties en kosten. Hierdoor kunnen klanten op te bouwen van interactieve websites die werken met grote gegevenssets services sensor en telemetrie gegevensopslag uit miljoenen eindpunten maken en deze gegevens analyseren met Hadoop taken. HBase en Hadoop zijn goed uitgangspunt voor het project van big data in Azure; met name kan worden ingeschakeld realtime-toepassingen met grote gegevenssets werken.

De scale-out architectuur van HBase voor automatische sharding van tabellen, sterke samenhang voor leesbewerkingen en schrijfbewerkingen en automatische failover borduurt voort op de uitvoering van de HDInsight. Biedt betere prestaties door in het geheugen caching voor leesbewerkingen en hoge gegevensdoorvoer streaming voor schrijfbewerkingen. Virtueel netwerk inrichten is ook beschikbaar voor HDInsight HBase. Zie voor meer informatie [HDInsight bepaling clusters op Azure Virtual Network] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Hoe worden gegevens verwerkt in HDInsight HBase?

Gegevens kunnen worden beheerd in HBase met behulp van de `create`, `get`, `put`, en `scan` opdrachten vanuit de shell HBase. Gegevens naar de database worden geschreven met behulp van `put` en lezen met behulp van `get`. De `scan` opdracht wordt gebruikt voor het ophalen van gegevens uit meerdere rijen in een tabel. Gegevens kunnen ook worden beheerd met behulp van de API C# HBase, waarmee een clientbibliotheek op de HBase REST API. Een database HBase kan ook worden opgezocht met behulp van de component. Voor een inleiding tot deze programmering modellen, Zie [aan de slag met HBase met Hadoop in HDInsight][hbase-get-started]. CO-processors zijn ook beschikbaar, waarmee de verwerking van gegevens in de knooppunten die als host fungeren voor de database.


## <a name="scenarios-use-cases-for-hbase"></a>Scenario's: Use-cases voor HBase
De canonieke use-case voor welke BigTable (en dus ook HBase) is gemaakt is zoeken op het web. Zoekprogramma's bouwen indexen die termen verwijzen naar de webpagina's die ze bevatten. Maar er zijn veel andere use-cases HBase is geschikt voor: verschillende van die in deze sectie zijn gespecificeerd.

- Sleutel / waarde-winkel

    HBase kan worden gebruikt als een sleutel / waarde-archief en is geschikt voor het beheren van systemen van bericht. Facebook HBase voor hun berichtensysteem, en is ideaal voor het opslaan en beheren van Internet-communicatie. HBase WebTable gebruikt voor het zoeken en beheren van tabellen die worden opgehaald uit de webpagina's.

- Sensorgegevens

    HBase is handig voor het vastleggen van gegevens geleidelijk worden verzameld uit verschillende bronnen. Dit geldt ook voor sociale analytics, tijdreeks door interactieve dashboards up-to-date te houden met de trends en tellers en audit log systemen beheren. Voorbeelden zijn handelaar van Bloomberg terminal en de tijd-serie Database geopend (OpenTSDB), die worden opgeslagen en biedt toegang tot gegevens verzameld over de gezondheid van de server-systemen.

- Real-time query

    [Phoenix](http://phoenix.apache.org/) is een SQL-query-engine voor Apache HBase. Het is geopend als een stuurprogramma voor JDBC en kunt opvragen en beheren van HBase tabellen met behulp van SQL.

- HBase als een platform

    Toepassingen kunnen worden uitgevoerd op HBase door deze te gebruiken als een gegevensarchief. Voorbeelden zijn Phoenix, OpenTSDB, Kiji en Titan. Toepassingen kunnen ook integreren met HBase. Voorbeelden zijn component, varkens, Solr, Storm, Flume, Impala, Spark, ganglia semilunaria en analyse.


##<a name="next-steps"></a>Volgende stappen

- [Aan de slag met HBase met Hadoop in HDInsight][hbase-get-started]
- [HDInsight clusters op Azure Virtual Network inrichten] [hbase-provision-vnet]
- [HBase replicatie configureren in HDInsight](hdinsight-hbase-geo-replication.md)
- [Twitter sentiment met HBase in HDInsight analyseren][hbase-twitter-sentiment]
- [Maven gebruik te maken van Java-toepassingen die gebruikmaken van HBase met HDInsight (Hadoop)][hbase-build-java-maven]

##<a name="see-also"></a>Zie ook

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: Een gedistribueerd systeem voor opslag van gestructureerde gegevens](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
