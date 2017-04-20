<properties
    pageTitle="Fout: onvoldoende geheugen (OOM) - component instellingen | Microsoft Azure"
    description="Onvoldoende geheugenfout (OOM) van een query component in Hadoop in HDInsight gerepareerd. Het scenario voor de klant is een query in veel grote tabellen."
    keywords="van de instellingen van het geheugen fout, OOM, component"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Herstellen van een fout van geheugen (OOM) met de component geheugeninstellingen in Hadoop in Azure, HDInsight

Een van de voorkomende problemen met het gezicht van onze klanten is steeds een Out of Memory (OOM) foutmelding als met behulp van de component. Dit artikel beschrijft een scenario voor de klant en de instellingen van de component die is het raadzaam om het probleem te verhelpen.

## <a name="scenario-hive-query-across-large-tables"></a>Scenario: Component query in grote tabellen

Een klant heeft de query hieronder met behulp van de component.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Enkele nuances bevatten van deze query:

* T1 is een alias voor een grote tabel, Tabel1, die veel STRING kolomtypen.
* Andere tabellen zijn niet dat groot, maar wel een groot aantal kolommen.
* Alle tabellen samenvoegt, in sommige gevallen met meerdere kolommen in Tabel1 en anderen.

Wanneer de klant de query met behulp van component op MapReduce op een 24-knooppunt A3 cluster wordt uitgevoerd, wordt de query uitgevoerd in ongeveer 26 minuten. De klant opgemerkt dat de volgende waarschuwingen weergegeven wanneer de query is uitgevoerd met behulp van de component op MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Omdat de query uitgevoerd in ongeveer 26 minuten, de klant deze waarschuwingen genegeerd en wordt in plaats daarvan begonnen te richten op het verbeteren van deze verdere prestaties van de query.

De klant raadpleging van [query's optimaliseren component voor Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md)en besloten te gebruiken Tez execution engine. Als dezelfde query is uitgevoerd met de Tez instelling is ingeschakeld wordt de query uitgevoerd gedurende 15 minuten en vervolgens heeft de volgende fout:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

De klant vervolgens besloten te gebruiken een grotere VM (dat wil zeggen D12) een grotere VM denkt meer opslagruimte nodig zou hebben. Zelfs dan bleef de klant het probleem. De klant bereikt van het team van HDInsight voor hulp bij het opsporen van fouten in dit probleem.

## <a name="debug-the-out-of-memory-oom-error"></a>Fouten opsporen in het foutbericht onvoldoende geheugen (OOM)

Onze ondersteuning en engineering-teams bij elkaar gevonden op een van de problemen veroorzaakt door de fout van geheugen (OOM) is een [bekend probleem dat wordt beschreven in de Apache-JIRA](https://issues.apache.org/jira/browse/HIVE-8306). Van de beschrijving in de JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

We bevestigd dat **hive.auto.convert.join.noconditionaltask** inderdaad is ingesteld op **true** door te kijken onder Component site.xml bestand:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Op basis van de waarschuwing en de JIRA, is onze hypothese dat toewijzen deelnemen aan de oorzaak van de fout Java Heap ruimte OOM was. Zodat we dieper koos in dit probleem.

Zoals uiteengezet in het weblogbericht [Hadoop garen geheugeninstellingen in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), daadwerkelijk wanneer Tez execution engine is de heap schijfruimte gebruikt van de container Tez. Zie de afbeelding hieronder met een beschrijving van het geheugen van de container Tez.

![Tez container geheugen diagram: fout: onvoldoende geheugen OOM component](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Zoals het weblogbericht wordt gesuggereerd, de volgende twee geheugeninstellingen definiëren met het geheugen van de container voor de heap: **hive.tez.container.size** en **hive.tez.java.opts**. Uit onze ervaring, betekent de uitzondering OOM niet dat de container is te klein. Betekent dit dat de Java-heap-grootte (hive.tez.java.opts) te klein. Dus wanneer u OOM ziet, kunt u proberen te verhogen **hive.tez.java.opts**. Indien nodig kan het zijn dat de **hive.tez.container.size**te verhogen. De instelling van de **java.opts** moet ongeveer 80% van de **container.size**.

> [AZURE.NOTE]  De instelling **hive.tez.java.opts** moet altijd kleiner zijn dan **hive.tez.container.size**.

Aangezien een machine D12 28GB geheugen, besloten we een container grootte van 10GB (10240MB) en 80% toewijzen aan java.opts. Dit is gedaan op de component-console met de volgende instelling:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Op basis van deze instellingen, de query is uitgevoerd in minder dan tien minuten.

## <a name="conclusion-oom-errors-and-container-size"></a>Conclusie: OOM fouten en grootte van de container

Steeds een foutmelding OOM betekent niet dat de container is te klein. In plaats daarvan moet u de instellingen voor het geheugen configureren zodat de heapgrootte is toegenomen en ten minste 80% van de grootte van het systeemgeheugen container is.
