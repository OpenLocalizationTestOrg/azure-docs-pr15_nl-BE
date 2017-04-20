<properties
   pageTitle="MapReduce- en SSH-verbinding met Hadoop in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van Hadoop op HDInsight MapReduce-taken uitvoeren met SSH."
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

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>MapReduce gebruiken in combinatie met Hadoop op HDInsight met SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u hoe u met de Secure Shell (SSH) verbinding maken met een Hadoop op de cluster HDInsight en MapReduce taken vervolgens indienen met behulp van Hadoop-opdrachten.

> [AZURE.NOTE] Als u al bekend bent met Hadoop Linux-gebaseerde servers gebruiken, maar u met HDInsight bent, Zie [tips Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een cluster van Linux-gebaseerde HDInsight (Hadoop op HDInsight)

* SSH-client. Linux, Unix en Mac-besturingssystemen moeten worden geleverd met een SSH-client. Windows-gebruikers moeten downloaden van een client, zoals [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met de FQDN-naam (Fully Qualified Domain Name) van het cluster HDInsight met behulp van de opdracht SSH. De FQDN-naam is de naam die u hebt gekregen van het cluster, gevolgd door **. azurehdinsight.net**. Bijvoorbeeld de volgende verbinding te maken met een cluster met de naam **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Als u een certificaatsleutel voor SSH-verificatie hebt opgegeven** bij het maken van het cluster HDInsight, wellicht moet u de locatie opgeven van de persoonlijke sleutel op de client-computer, bijvoorbeeld:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Als u een wachtwoord voor de SSH-verificatie hebt opgegeven** bij het maken van de HDInsight-cluster, moet u het wachtwoord wanneer daarom wordt gevraagd op te geven.

Zie voor meer informatie over het gebruik van SSH in HDInsight, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>PuTTY (Windows-clients)

Windows beschikt niet over een ingebouwde SSH-client. Beste **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Hadoop-opdrachten gebruiken

1. Nadat u met het cluster HDInsight verbonden bent, u het volgende **Hadoop** een MapReduce taak starten:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Hiermee start u de klasse **wordcount** , die is opgenomen in het bestand **hadoop-mapreduce-examples.jar** . Als invoer, wordt het document **wasbs://example/data/gutenberg/davinci.txt** gebruikt en de uitvoer wordt opgeslagen op de **wasbs: / / / voorbeeld/data/WordCountOutput**.

    > [AZURE.NOTE] Zie voor meer informatie over deze taak MapReduce en de voorbeeldgegevens [Gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

2. De taak genereert gegevens worden verwerkt en de gegevens worden geretourneerd met de volgende strekking wanneer de taak is voltooid:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wanneer de taak is voltooid, de output-bestanden die zijn opgeslagen op **wasbs://example/data/WordCountOutput**met de volgende opdracht:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Dit moet worden twee bestanden, **_SUCCESS** en **deel-r-00000**weergegeven. De **deel-r-00000** -bestand bevat de uitvoer voor deze taak.

    > [AZURE.NOTE] Sommige taken MapReduce kunnen de resultaten splitsen in meerdere bestanden van **deel-r-###** . Als dit het geval is, gebruikt u de ### achtervoegsel om aan te geven de volgorde van de bestanden.

4. Weergave van de uitvoer, gebruikt u de volgende opdracht:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Er verschijnt een lijst van de woorden die zijn opgenomen in het bestand **wasbs://example/data/gutenberg/davinci.txt** en het aantal keren dat elk woord is opgetreden. Hier volgt een voorbeeld van de gegevens die worden opgenomen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Samenvatting

Zoals u ziet, kunnen Hadoop opdrachten gemakkelijk MapReduce taken uitvoeren in een cluster HDInsight en bekijkt u de uitvoer van de taak.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce op HDInsight Hadoop gebruiken](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)
