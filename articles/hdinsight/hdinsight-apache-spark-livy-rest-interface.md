<properties
    pageTitle="Spark taken op afstand met behulp van Livius | Microsoft Azure"
    description="Leren hier HDInsight clusters met vonkontsteking taken op afstand indienen."
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
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Taken met een cluster van Apache Spark op HDInsight Linux hier met vonkontsteking

Cluster van Apache Spark op Azure HDInsight bevat Livius, een REST-interface voor het verzenden van taken naar een cluster Spark op afstand. Zie voor gedetailleerde documentatie [hier](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Hier kunt u interactieve Spark houders worden uitgevoerd of taken kunnen worden uitgevoerd op een indienen. In dit artikel gesprekken voert over batchtaken indienen met behulp van Livius. Krul de onderstaande syntaxis gebruikt voor gesprekken naar het eindpunt hier REST.

**Voorwaarden:**

U hebt de volgende opties:

- Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster van Apache Spark op HDInsight Linux. Zie [clusters in Azure HDInsight Apache Spark maken](hdinsight-apache-spark-jupyter-spark-sql.md)voor instructies.

## <a name="submit-a-batch-job-the-cluster"></a>Indienen van een batchverwerking in het cluster

Voordat u een batchtaak indienen, moet u het oppervlak van de toepassing in de clusteropslag die is gekoppeld aan het cluster te uploaden. [**AzCopy**](../storage/storage-use-azcopy.md), een hulpprogramma voor de opdrachtregel, kunt u dat doen. Er zijn een heleboel andere clients die u kunt gebruiken voor het uploaden van gegevens. U vindt meer informatie over deze bij het [uploaden van gegevens voor Hadoop projecten in HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Voorbeelden**:

* Als de jar-bestand op de clusteropslag (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Als u wilt de jar-bestandsnaam en de klassenaam doorgeven als onderdeel van een invoerbestand (in dit voorbeeld input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Informatie over de batches die op het cluster worden uitgevoerd

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Voorbeelden**:

* Als u ophalen van alle batches die op het cluster worden uitgevoerd wilt:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Als u wilt ophalen van een bepaalde batch met een bepaalde batchId

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Een batchtaak verwijderen

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Voorbeeld**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livius en hoge beschikbaarheid

Hier vindt u hoge beschikbaarheid voor Spark taken die op het cluster worden uitgevoerd. Hier volgen een paar voorbeelden.

* Als de service Hier wordt afgesloten nadat u een taak met een cluster Spark heeft ingediend, blijft de taak op de achtergrond uitgevoerd. Hier is een back-up, wordt de status van de taak en de rapporten weer hersteld.

* Jupyter-laptops voor HDInsight worden aangedreven door Livius in de backend. Als u een laptop een vonk-project en de Livius-service opnieuw wordt gestart, blijven de laptop de cellen code uitvoeren. 

## <a name="show-me-an-example"></a>Laat me een voorbeeld zien

In deze sectie Bekijk wij voorbeelden over het gebruik van hier een vonk aanvraag indienen, de voortgang van de toepassing en verwijder vervolgens de taak. In dit voorbeeld we gebruiken de-toepassing is ontwikkeld in het [maken van een zelfstandige toepassing Scala en uit te voeren op een cluster HDInsight Spark](hdinsight-apache-spark-create-standalone-application.md)-artikel. De volgende stappen wordt uitgegaan van het volgende:

* U kopieert via het oppervlak van de toepassing naar de rekening van de opslag die is gekoppeld aan het cluster.
* U hebt geïnstalleerd op de computer waarop u deze stappen probeert krul.

Voer de volgende stappen.

1. Laten we eerst controleren of dat hier op het cluster wordt uitgevoerd. Dit doen we door het opvragen van een lijst van de partijen. Als dit de eerste keer dat u een taak met behulp van Livius uitvoert, moet dit nul retourneren.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Ontvangt u een uitvoer de volgende strekking:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    U ziet hoe de laatste regel in de uitvoer zegt **Totaal: 0**, die duidt op geen actieve batches.

2. Laten we nu een batchtaak indienen. In het volgende fragment wordt een input bestand (input.txt) om de jar naam te geven en de naam van de klasse als parameters gebruikt. Dit is de aanbevolen werkwijze als u deze stappen vanaf een Windows-computer uitvoert.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    De parameters in het bestand **input.txt** worden als volgt gedefinieerd:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Een uitvoer van de volgende strekking worden weergegeven:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    U ziet hoe de laatste regel van de uitvoer, zegt **staat: starten van**. Deze ook zegt, **id: 0**. Dit is de id van de batch.

3. Nu kunt u ophalen de de status van deze specifieke batch met behulp van de batch-ID.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Een uitvoer van de volgende strekking worden weergegeven:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    De uitvoer wordt nu **staat: succes**, die duidt op de taak is voltooid.

4. Als u wilt, kunt u nu de batch te verwijderen.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Een uitvoer van de volgende strekking worden weergegeven:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    De laatste regel van de output ziet u de batch is verwijderd. Als u een taak verwijdert terwijl deze wordt uitgevoerd, wordt deze in feite de taak beëindigen. Als u een taak die is voltooid is of anderszins verwijderen worden de taakgegevens volledig.

## <a name="seealso"></a>Zie ook


* [Overzicht: Apache Spark op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Motoren met Machine Learning: Spark in HDInsight voor het analyseren van gebouw temperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met vonkontsteking in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Maken en uitvoeren van toepassingen

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)

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
