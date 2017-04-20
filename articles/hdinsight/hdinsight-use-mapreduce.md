<properties
   pageTitle="MapReduce met Hadoop op HDInsight | Microsoft Azure"
   description="Informatie over het uit te voeren taken MapReduce op Hadoop in clusters van HDInsight. U zult een elementaire word count bewerking geïmplementeerd als een functie van Java MapReduce uitvoeren."
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

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>MapReduce gebruiken in Hadoop op HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u het uitvoeren van taken MapReduce op Hadoop in HDInsight clusters. Wij uitvoeren basic word count bewerking geïmplementeerd als een Java MapReduce taak.

##<a id="whatis"></a>Wat is MapReduce?

Hadoop MapReduce is een raamwerk van de software voor het schrijven van taken die grote hoeveelheden gegevens verwerken. Invoergegevens is opgesplitst in onafhankelijke stukken, die vervolgens parallel worden verwerkt op de knooppunten in het cluster. Een taak MapReduce bestaan uit twee functies:

* **Mapper**: invoergegevens verbruikt, analyseert het (meestal met filter- en sorteerbewerkingen) en zendt tuples (sleutel / waarde-paren)
* **Reducer**: tuples uitgestoten door de Mapper verbruikt en een samenvatting uitvoert die een kleinere, gecombineerde resultaat Mapper gemaakt

Voorbeeld van een eenvoudige word count MapReduce taak wordt in het volgende diagram geïllustreerd:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

De uitvoer van deze taak is een telling van het aantal keren dat elk woord is opgetreden in de tekst die is geanalyseerd.

* De toewijzing wordt elke regel van de ingevoerde tekst als invoer en splitst deze woorden. Zij zendt een sleutel/waarde paar telkens wanneer een woord het woord plaatsvindt wordt gevolgd door een 1. De uitvoer wordt gesorteerd voordat u deze verzendt naar reducer.

* De reducer de som van de individuele aantallen voor elk woord en genereert een enkele sleutel/waarde-paar met het woord gevolgd door de som van de exemplaren.

MapReduce kan worden geïmplementeerd in verschillende talen. Java is de meest voorkomende implementatie en ter illustratie in dit document wordt gebruikt.

### <a name="hadoop-streaming"></a>Hadoop-Streaming

Talen of raamwerken die zijn gebaseerd op Java en Java Virtual Machine (bijvoorbeeld Scalding of trapsgewijs,) kan direct als een taak MapReduce, vergelijkbaar met een Java-toepassing worden uitgevoerd. Anderen, moeten zoals C# of Python of zelfstandige uitvoerbare bestanden, gebruiken Hadoop streaming.

Hadoop streaming communiceert met de toewijzing en reducer via STDIN en STDOUT - de mapper reducer gegevens lezen een regel op een moment van STDIN en schrijven van de uitvoer naar STDOUT. Elke regel lezen of uitgestoten door de mapper en reducer moet zijn in de indeling van een sleutel/waarde-paar, gescheiden door een charaacter tab:

    [key]/t[value]

Zie [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html)voor meer informatie.

Zie de volgende onderwerpen voor voorbeelden van het gebruik van Hadoop streaming met HDInsight:

* [Python MapReduce projecten ontwikkelen](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Over de voorbeeldgegevens

In dit voorbeeld wordt voor de voorbeeldgegevens, gebruikt u de laptops van Leonardo Da Vinci, die worden geleverd als een tekstdocument in het cluster HDInsight.

De voorbeeldgegevens is in Azure Blob-opslag HDInsight wordt gebruikt als het standaardbestandssysteem voor clusters van Hadoop opgeslagen. HDInsight toegang tot bestanden die zijn opgeslagen in de Blob-opslag met behulp van het voorvoegsel **wasb** . Bijvoorbeeld, als u het bestand sample.log, gebruikt u de volgende syntaxis:

    wasbs:///example/data/gutenberg/davinci.txt

Aangezien Azure Blob-opslag de standaard opslaglocatie voor HDInsight is, kunt u ook toegang tot het bestand met behulp van **/example/data/gutenberg/davinci.txt**.

> [AZURE.NOTE] In de syntaxis van de vorige **wasbs: / / /** wordt gebruikt voor toegang tot bestanden die zijn opgeslagen in de opslag voor uw cluster HDInsight. Als u extra opslagruimte accounts hebt opgegeven bij het inrichten van het cluster en u toegang wilt tot bestanden die zijn opgeslagen in deze accounts, opent u de gegevens door te geven van de container en de opslag accountadres. Bijvoorbeeld **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Over het voorbeeld MapReduce

De MapReduce-taak die wordt gebruikt in dit voorbeeld bevindt zich op **wasbs://example/jars/hadoop-mapreduce-examples.jar**en is voorzien van het cluster HDInsight. Dit document bevat een voorbeeld van de word count die u op **davinci.txt uitvoeren zal**.

> [AZURE.NOTE] Op HDInsight 2.1 clusters is de locatie van het bestand **wasbs:///example/jars/hadoop-examples.jar**.

Ter referentie volgt de Java-code voor de word count MapReduce taak:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

Zie voor instructies voor het schrijven van uw eigen project MapReduce [MapReduce voor ontwikkeling van Java-programma's voor HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>De MapReduce uitvoeren

HDInsight HiveQL taken kan worden uitgevoerd met behulp van verschillende methoden. In de volgende tabel om te beslissen welke methode u gebruikt en volg de link voor een overzicht.

| **Gebruik dit**...                                                    | **.. .om hiervoor**                                       | .. .door dit **cluster besturingssysteem** | .. .from dit **besturingssysteem van client** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Gebruik de opdracht Hadoop via **SSH**                  | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [Krul](hdinsight-hadoop-use-mapreduce-curl.md)                     | De taak op afstand indienen met behulp van **REST**               | Linux of Windows                          | Linux, Unix, Mac OS X of Windows        |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | De taak op afstand indienen met behulp van **Windows PowerShell** | Linux of Windows                          | Windows                                  |
| [Extern bureaublad](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Gebruik de opdracht Hadoop via **Extern bureaublad**       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Volgende stappen

Maar MapReduce krachtige diagnostische mogelijkheden biedt, kan een uitdagende model bits zijn. Er zijn verschillende Java-frameworks waardoor het gemakkelijker voor het definiëren van MapReduce toepassingen zo technologieën, zoals varkens en component, die een eenvoudiger manier om te werken met gegevens in de HDInsight. Voor meer informatie, Zie de volgende artikelen:

* [MapReduce Java-programma's ontwikkelen voor HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Ontwikkelen van Python streaming MapReduce-programma's voor HDInsight](hdinsight-hadoop-streaming-python.md)

* [MapReduce broeien projecten met Apache Hadoop op HDInsight ontwikkelen](hdinsight-hadoop-mapreduce-scalding.md)

* [Gebruik component met HDInsight][hdinsight-use-hive]

* [Varken met HDInsight gebruiken][hdinsight-use-pig]

* [Voorbeelden van de HDInsight uitvoeren][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
