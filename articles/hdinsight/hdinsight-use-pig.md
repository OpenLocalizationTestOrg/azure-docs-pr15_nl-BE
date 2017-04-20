<properties
   pageTitle="Gebruik van Hadoop varkens in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van varkens met Hadoop op HDInsight."
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Varken gebruiken in combinatie met Hadoop op HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache varken](http://pig.apache.org/) is een platform voor het maken van programma's voor Hadoop met behulp van een procedurele taal *Latijn varken*genoemd. Varken is een alternatief voor Java voor het maken van oplossingen met *MapReduce* en is opgenomen in Azure HDInsight.

In dit artikel leert u hoe u kunt varken met HDInsight.

##<a id="why"></a>Waarom varkens gebruiken?

Een van de uitdagingen bij het verwerken van gegevens met behulp van MapReduce in Hadoop is het implementeren van uw computer met alleen een kaart en een functie verminderen. Voor complexe verwerking, vaak zijn er verwerking opsplitsen in meerdere MapReduce-bewerkingen die zijn samengevoegd tot het gewenste resultaat.

In plaats van dat u alleen hyperlinks gebruiken en functies te beperken, kunt varken u verwerking definiëren als een reeks transformaties die de gegevens voor de productie van de gewenste uitvoer door loopt.

De varkens Latijnse taal kunt u voor het beschrijven van de overdracht van onbewerkte invoer, door middel van een of meer transformaties, voor de productie van de gewenste uitvoer. Varken Latijn programma's volgen deze algemene patroon:

- **Laden**: gegevens worden bewerkt vanuit het bestandssysteem lezen
- **Transformeren**: werken met de gegevens
- **Dump of winkel**: gegevens op het scherm uitvoeren of opslaan voor verwerking

Varken Latijn ondersteunt ook door de gebruiker gedefinieerde functies (UDF), waarmee u externe componenten waarbij logica die moeilijk model in Latijns varken aanroepen.

Zie voor meer informatie over het varken Latijn [varken Latijn Reference Manual 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) en [varken Latijn referentie handleiding 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Zie voor een voorbeeld van het gebruik van UDF's met varkens, de volgende documenten:

* [DataFu voor gebruik bij varkens in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu is een verzameling van nuttige UDF's onderhouden door Apache

* [Python gebruiken met varkens en component in HDInsight](hdinsight-python.md)

* [Gebruik C# met component en varken in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Over de voorbeeldgegevens

In dit voorbeeld wordt een voorbeeldbestand *log4j* , dat is opgeslagen op de **/example/data/sample.log** in de blob-opslag container. Elk logboek in het bestand bestaat uit een reeks velden met een `[LOG LEVEL]` om weer te geven van het type en de ernst, bijvoorbeeld:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

In het vorige voorbeeld is het logboekniveau fout.

> [AZURE.NOTE] U kunt ook een log4j-bestand genereren met het hulpprogramma [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) vastleggen en vervolgens dat bestand te uploaden naar de blob. Zie [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor meer informatie. Zie voor meer informatie over het gebruik van BLOB's in Azure opslag met HDInsight [Gebruik Azure Blob-opslag met HDInsight](hdinsight-hadoop-use-blob-storage.md).

De voorbeeldgegevens is in Azure Blob-opslag HDInsight wordt gebruikt als het standaardbestandssysteem voor clusters van Hadoop opgeslagen. HDInsight toegang tot bestanden die zijn opgeslagen in de BLOB's met het voorvoegsel **wasb** . Bijvoorbeeld, als u het bestand sample.log, gebruikt u de volgende syntaxis:

    wasbs:///example/data/sample.log

Omdat WASB de standaard opslaglocatie voor HDInsight is, kunt u ook toegang tot het bestand met behulp van de **/example/data/sample.log** van varkens, Latijn.

> [AZURE.NOTE] De syntaxis van de **wasbs: / / /**, wordt gebruikt voor toegang tot bestanden die zijn opgeslagen in de opslag voor uw cluster HDInsight. Als u extra opslagruimte accounts hebt opgegeven bij het inrichten van het cluster en u toegang wilt tot bestanden die zijn opgeslagen in deze accounts, opent u de gegevens door te geven van de container en de opslag accountadres, bijvoorbeeld: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Over de taak van het monster

De volgende taak in de Latijnse varken laadt het bestand **sample.log** uit de standaard opslag voor uw cluster HDInsight. Het voert een reeks transformaties die resulteren in een telling van het aantal keren dat elk logboekniveau is opgetreden in de invoergegevens. De resultaten worden gedumpt in STDOUT.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

De volgende afbeelding ziet u de onderverdeling van elke transformatie tot de gegevens heeft.

![Grafische weergave van de transformaties][image-hdi-pig-data-transformation]

##<a id="run"></a>Voer de Latijnse varken.

HDInsight varken Latijn taken kan worden uitgevoerd met behulp van verschillende methoden. In de volgende tabel om te beslissen welke methode u gebruikt en volg de link voor een overzicht.

| **Gebruik deze optie** als u wilt...                                   | .. .an **interactieve** shell | ... **batch** -verwerking | .. .door dit **cluster besturingssysteem** | .. .from dit **besturingssysteem van client** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [Krul](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux of Windows                          | Linux, Unix, Mac OS X of Windows        |
| [.NET SDK voor Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux of Windows                          | Windows (voor nu)                        |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |
| [Extern bureaublad](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Varken taken op Azure HDInsight uitgevoerd op ruimten SQL Server Integration Services gebruiken

U kunt SQL Server Integration Services (SSIS) ook gebruiken voor het uitvoeren van de taak van een varken. De Azure Feature Pack voor SSIS bevat de volgende onderdelen die met varkens taken op HDInsight werken.


- [Azure HDInsight varken taak][pigtask]
- [Azure abonnement Connection Manager][connectionmanager]


Meer weten over de Azure Feature Pack voor SSIS [hier][ssispack].


##<a id="nextsteps"></a>Volgende stappen

U hebt geleerd hoe u varken met HDInsight, via de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.

* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Gebruik component met HDInsight][hdinsight-use-hive]
* [Gebruik Sqoop met HDInsight](hdinsight-use-sqoop.md)
* [Gebruik Oozie met HDInsight](hdinsight-use-oozie.md)
* [MapReduce taken gebruiken met HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
