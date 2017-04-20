<properties
   pageTitle="MapReduce en extern bureaublad met Hadoop in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van extern bureaublad verbinding maken met Hadoop op HDInsight en MapReduce taken uitvoeren."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>MapReduce in Hadoop op HDInsight met extern bureaublad gebruiken

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u hoe u via Extern bureaublad verbinding maken met een Hadoop op de cluster HDInsight en MapReduce taken uitvoeren met de opdracht Hadoop.

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een cluster van Windows-gebaseerde HDInsight (Hadoop op HDInsight)

* Een clientcomputer met Windows 10, Windows 8 of Windows 7

##<a id="connect"></a>Verbinding met extern bureaublad

Extern bureaublad inschakelen voor het cluster HDInsight en vervolgens verbinding maken met het door de instructies op [verbinding maken met clusters van HDInsight met RDP](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hadoop"></a>Gebruik de opdracht Hadoop

Wanneer u met het bureaublad voor het cluster HDInsight verbonden bent, gebruik dan de volgende stappen voor het uitvoeren van een taak MapReduce met behulp van de opdracht Hadoop:

1. Starten vanaf het bureaublad HDInsight de **Hadoop-opdrachtregel**. Hiermee opent u een nieuwe opdrachtprompt in de **c:\apps\dist\hadoop-&lt;versienummer >** directory.

    > [AZURE.NOTE] Het versienummer wordt gewijzigd als Hadoop wordt bijgewerkt. De omgevingsvariabele **HADOOP_HOME** kan worden gebruikt om het pad te vinden. Bijvoorbeeld `cd %HADOOP_HOME%` mappen naar de map Hadoop, zonder dat u weet dat het versienummer wordt gewijzigd.

2. Voor het gebruik van de opdracht **Hadoop** een voorbeeld MapReduce taak uit te voeren, gebruikt u de volgende opdracht:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Hiermee start u de klasse **wordcount** , die is opgenomen in het bestand **hadoop-mapreduce-examples.jar** in de huidige map. Als invoer, wordt het document **wasbs://example/data/gutenberg/davinci.txt** gebruikt en de uitvoer wordt opgeslagen op: **wasbs: / / / voorbeeld/data/WordCountOutput**.

    > [AZURE.NOTE] Zie voor meer informatie over deze taak MapReduce en de voorbeeldgegevens <a href="hdinsight-use-mapreduce.md">Gebruik MapReduce in HDInsight Hadoop</a>.

2. De taak genereert details zoals deze wordt verwerkt en de gegevens worden geretourneerd met de volgende strekking wanneer de taak voltooid is:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wanneer de taak voltooid is, gebruikt u de volgende opdracht om de output bestanden opgeslagen op **wasbs://example/data/WordCountOutput**:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Dit moet worden twee bestanden, **_SUCCESS** en **deel-r-00000**weergegeven. De **deel-r-00000** -bestand bevat de uitvoer voor deze taak.

    > [AZURE.NOTE] Sommige taken MapReduce kunnen de resultaten splitsen in meerdere bestanden van **deel-r-###** . Als dit het geval is, gebruikt u de ### achtervoegsel om aan te geven de volgorde van de bestanden.

4. Weergave van de uitvoer, gebruikt u de volgende opdracht:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Er verschijnt een lijst van de woorden die zijn opgenomen in het bestand **wasbs://example/data/gutenberg/davinci.txt** , samen met het aantal keren dat elk woord is opgetreden. Hier volgt een voorbeeld van de gegevens die worden opgenomen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Samenvatting

Zoals u zien kunt, biedt de opdracht Hadoop gemakkelijk MapReduce taken uitvoeren op een cluster van HDInsight en bekijk de uitvoer van de taak.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce op HDInsight Hadoop gebruiken](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)
