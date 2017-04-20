<properties
    pageTitle="De monsters Hadoop uitgevoerd in HDInsight | Microsoft Azure"
    description="Aan de slag met de HDInsight van Azure-service met de voorbeelden. Gebruikt u PowerShell scripts die MapReduce programma's op clusters van gegevens uitvoeren."
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
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Voorbeelden van Hadoop MapReduce uitvoeren in Windows-gebaseerde HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Een aantal voorbeelden zijn bedoeld om u begonnen met MapReduce taken op Hadoop-clusters met Azure HDInsight. Deze monsters worden beschikbaar gesteld op elke beheerde HDInsight clusters die u maakt. Deze monsters wordt uitgevoerd maakt u vertrouwd met met Azure PowerShell-cmdlets voor het uitvoeren van taken op Hadoop-clusters.

- [**Aantal woorden**][hdinsight-sample-wordcount]: telt word exemplaren in een tekstbestand.
- [**C#-streaming woorden tellen**][hdinsight-sample-csharp-streaming]: telt word exemplaren in een tekstbestand met de streaming Hadoop-interface.
- [**PI estimator**][hdinsight-sample-pi-estimator]: maakt gebruik van een statistisch (quasi Monte Carlo) methode voor het schatten van de waarde van pi.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: GraySort voor algemene doeleinden worden uitgevoerd op een bestand met 10 GB met behulp van HDInsight. Er zijn drie taken uit te voeren: Teragen voor het genereren van de gegevens, de Terasort om de gegevens te sorteren en Teravalidate om te bevestigen dat de gegevens correct zijn gesorteerd.

>[AZURE.NOTE] De broncode kan worden gevonden in het aanhangsel. 

Er bestaat veel aanvullende documentatie op het web voor Hadoop-gerelateerde technologieën, zoals het programmeren met Java gebaseerde MapReduce en streaming en documentatie over de cmdlets die worden gebruikt in Windows PowerShell scripts. Zie voor meer informatie over deze bronnen:

- [MapReduce Java-programma's ontwikkelen voor Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Hadoop taken in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Inleiding tot Azure HDInsight][hdinsight-introduction]

Veel mensen kiezen tegenwoordig component en varken via MapReduce.  Zie voor meer informatie:

- [Gebruik de component in HDInsight](hdinsight-use-hive.md)
- [Varken in HDInsight gebruiken](hdinsight-use-pig.md)
 
**Vereisten**:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **een cluster van HDInsight**. Zie voor meer informatie over de verschillende manieren waarop dergelijke clusters kunnen worden gemaakt, [maakt Hadoop clusters in het HDInsight](hdinsight-provision-clusters.md).
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Word count - Java 

Om te dienen een MapReduce-project, maakt u eerst een taakdefinitie MapReduce. In de taakdefinitie geeft u het MapReduce jar bestand en de locatie van het jar-bestand **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, de naam van de klasse en de argumenten is.  Het programma wordcount MapReduce heeft twee argumenten: het bestand dat wordt gebruikt voor het tellen van woorden en de locatie voor uitvoer.

In de [bijlage A](#apendix-a---the-word-count-MapReduce-program-in-java)vindt u de broncode.

De regeling van de ontwikkeling van een programma van Java MapReduce, Zie - [MapReduce voor ontwikkeling van Java-programma's voor Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**Indienen van een project word count MapReduce**

1. Open **Windows PowerShell ISE**. Zie voor meer informatie [installeren en configureren van Azure PowerShell][powershell-install-configure].
2. Plak de volgende PowerShell script:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    De taak MapReduce maakt het bestand met de naam *deel-r-00000*, waarin de woorden en de tellingen. Het script maakt gebruik van de opdracht **findstr** om alle woorden met *"er"*.

3. De eerste 3 variabelen instellen en uitvoeren van het script.

## <a name="hdinsight-sample-csharp-streaming"></a>Word count - C#-streaming

Hadoop biedt een streaming API MapReduce, waarin u kunt schrijven met hyperlinks en functies in andere talen dan Java te verminderen.

> [AZURE.NOTE] De stappen in deze zelfstudie gelden alleen voor HDInsight op basis van Windows-clusters. Zie voor een voorbeeld van streaming voor HDInsight Linux gebaseerde clusters [ontwikkelt Python streaming programma's voor HDInsight](hdinsight-hadoop-streaming-python.md).

In het voorbeeld wordt de toewijzing en de reducer zijn uitvoerbare bestanden die de input van [stdin lezen] [ stdin-stdout-stderr] (regel voor regel) en de uitvoer naar [stdout]uitstoten[stdin-stdout-stderr]. Het programma omvat alle woorden in de tekst.

Wanneer u een uitvoerbaar bestand voor **mappers**is opgegeven, wordt elke taak mapper het uitvoerbare bestand als een afzonderlijk proces gestart wanneer de toewijzing wordt geïnitialiseerd. Als de taak van de toewijzing wordt uitgevoerd, converteert de invoer in regels en de regels voor de [stdin] feeds[ stdin-stdout-stderr] van het proces.

Ondertussen verzamelt de mapper de regelgeoriënteerde uitvoer van stdout van het proces. Elke regel wordt geconverteerd naar een sleutel/waarde-paar wordt verzameld als de uitvoer van de toewijzing. Standaard is de sleutel van het voorvoegsel van een regel tot de eerste Tab-teken en is de waarde van de rest van de regel (met uitzondering van het Tab-teken). Als er geen tabblad teken in de regel, hele regel beschouwd als de sleutel en de waarde null is.

Wanneer u een uitvoerbaar bestand voor **verkleiningstoestellen**is opgegeven, wordt elke taak reducer het uitvoerbare bestand als een afzonderlijk proces gestart wanneer de reducer wordt geïnitialiseerd. Als de taak reducer wordt uitgevoerd, de paren invoer sleutelwaarden in regels worden geconverteerd en deze regels aan de [stdin] feeds[ stdin-stdout-stderr] van het proces.

In de tussentijd de reducer berekent de output regelgeoriënteerde [stdout] [ stdin-stdout-stderr] van het proces. Elke regel wordt geconverteerd naar een sleutel/waarde-paar wordt verzameld als de uitvoer van de reducer. Standaard is de sleutel van het voorvoegsel van een regel tot de eerste Tab-teken en is de waarde van de rest van de regel (met uitzondering van het Tab-teken).

Zie [Hadoop Streaming] [hadoop streaming] voor meer informatie over de interface Hadoop Streaming.

**Indienen van een C#-streaming word count-functie**

- Volg de procedure in de [woordentelling - Java](#word-count-java)en de taakdefinitie wordt vervangen door het volgende:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Het uitvoerbestand zijn:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>PI estimator

De pi estimator maakt gebruik van een statistisch (quasi Monte Carlo) methode voor het schatten van de waarde van pi. Punten die in willekeurige volgorde worden geplaatst in een eenheid vallen ook vierkant binnen een cirkel met een kans gelijk is aan de oppervlakte van de cirkel zijn aangebracht binnen dat vierkant pi/4. De waarde van pi kan worden geraamd van de waarde van 4R, waarbij R de verhouding van het aantal punten die binnen de cirkel aan het totale aantal punten die binnen het vierkant is. Hoe groter de punten gebruikt, des te beter de raming is een voorbeeld.

Het script dat voor dit voorbeeld een jar Hadoop taak ingediend en wordt ingesteld tot uitvoeren met een waarde van 16 kaarten, die elk 10 miljoen monster punten berekenen door de parameterwaarden is vereist. De parameterwaarden voor deze kunnen worden gewijzigd ter verbetering van de geschatte waarde van pi. De eerste 10 decimalen van pi zijn voor een verwijzing naar 3.1415926535.

**Een pi estimator taak verzenden**

- Volg de procedure in de [woordentelling - Java](#word-count-java)en de taakdefinitie wordt vervangen door het volgende:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>Graysort 10 GB

In dit voorbeeld wordt een bescheiden 10GB aan gegevens zodat kunnen relatief snel worden uitgevoerd. De MapReduce toepassingen ontwikkeld door Owen O'Malley en Arun Murthy dat de jaarlijkse algemene ("daytona") terabyte sorteren benchmark in 2009 met een snelheid van 0.578 TB/min (100 TB in 173 minuten gewonnen) wordt gebruikt. Zie de website [Sortbenchmark](http://sortbenchmark.org/) voor meer informatie over deze en andere benchmarks sorteren.

In dit voorbeeld drie sets van MapReduce-programma's gebruikt:

1. **TeraGen** is een MapReduce-programma dat u gebruiken kunt voor het genereren van de rijen met gegevens wilt sorteren.
2. **TeraSort** monsters van de ingevoerde gegevens en de gegevens sorteren in een totale order met MapReduce. TeraSort is een standaard sorteervolgorde van MapReduce-functies, met uitzondering van een aangepaste partitioner die gebruikmaakt van een gesorteerde lijst van N-1 bemonsterd sleutels die de belangrijkste bereik voor elke verminderen definiëren. In het bijzonder alle sleutels die monster [i-1] < = sleutel < monster [i] ik verminderen worden verzonden. Deze garanties dat de uitvoer van ik beperken zijn alle kleiner is dan de uitvoer van i + 1 verminderen.
3. **TeraValidate** is een programma MapReduce valideert dat de uitvoer globaal is gesorteerd. Één kaart per bestand wordt gemaakt in de uitvoermap en elke toewijzing zorgt ervoor dat elke sleutel kleiner dan of gelijk aan de vorige is. De kaart-functie ook records van de eerste en laatste sleutels van elk bestand wordt gegenereerd en de functie verminderen die ervoor zorgt dat de eerste sleutel van i-bestand groter dan de laatste sleutel van het bestand i-1 is. Eventuele problemen worden gerapporteerd als een uitvoer van de verminderen met de sleutels die geplaatst zijn.

De indeling invoer en uitvoer, die door alle drie de toepassingen, leest en schrijft de tekstbestanden in de juiste indeling. De uitvoer van de verminderen heeft replicatie ingesteld op 1 in plaats van de standaard 3, omdat de wedstrijd benchmark niet vereist is dat de gegevens over de output op meerdere knooppunten worden gerepliceerd.

Drie taken zijn vereist voor het monster, die elk overeenkomen met een van de MapReduce-programma's die worden beschreven in de introductie:

1. De gegevens voor het sorteren door middel van de taak **TeraGen** MapReduce genereren.
2. De gegevens sorteren door de **TeraSort** MapReduce taak uit te voeren.
3. Zorg ervoor dat de gegevens correct zijn gesorteerd door het uitvoeren van de taak **TeraValidate** MapReduce.

**De taken**

- Volg de procedure in de [woordentelling - Java](#word-count-java)en de volgende taak-definities:

    $teragen = New AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - ClassName 'teragen' '-argumenten "-Dmapred.map.tasks=50", "100000000", "/ voorbeeld / / 10 GB-sorteren-invoer"
    
    $terasort = New AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - ClassName 'terasort' '-argumenten "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ voorbeeld / / 10 GB-sorteren-invoer", "/ voorbeeld / / 10 GB-sorteren-uitvoer"
    
    $teravalidate = New AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - ClassName 'teravalidate' '-argumenten "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ voorbeeld / / 10 GB-sorteren-uitvoer", "/ voorbeeld/data/10 GB-sorteren-valideren"


##<a name="next-steps"></a>Volgende stappen 

Van dit artikel en de artikelen in elk van de monsters, kunt u het uitvoeren van de monsters die met de HDInsight-clusters met Azure PowerShell geleerd. Voor een zelfstudie over het gebruik van varkens, component en MapReduce met HDInsight, Zie de volgende onderwerpen:

* [Aan de slag met Hadoop component in HDInsight voor het analyseren van mobiele telefoon gebruiken][hdinsight-get-started]
* [Varken gebruiken in combinatie met Hadoop op HDInsight][hdinsight-use-pig]
* [Component gebruiken in combinatie met Hadoop op HDInsight][hdinsight-use-hive]
* [Hadoop taken in HDInsight] [hdinsight-submit-jobs]
* [Azure HDInsight SDK-documentatie][hdinsight-sdk-documentation]
* [Debug Hadoop in HDInsight: foutberichten] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Bijlage A - de broncode Word count

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


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Bijlage B - de woordentelling streaming broncode

MapReduce wordt de toepassing van de cat.exe als de interface van een toewijzing voor het stroomsgewijs verzenden van de tekst in de console en de toepassing van de wc.exe als de interface verkleinen om het aantal woorden die afkomstig zijn uit een document. De toewijzing en de reducer standaard invoerstroom (stdin) tekens, regel voor regel lezen en schrijven naar de stream standaard uitvoer (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



De mapper-code in het bestand cat.cs wordt een [StreamReader] [ streamreader] object te lezen van de tekens van de inkomende stroom naar de console, die vervolgens de stroom naar de Standaarduitvoerstroom met de statische [Console.Writeline schrijft] [ console-writeline] methode.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


De reducer-code in het bestand wc.cs wordt een [StreamReader] [ streamreader] object om tekens te lezen uit de standaard invoer stroom die uitvoer door de cat.exe-toewijzing zijn. Als het lezen van de tekens met de [Console.Writeline] [ console-writeline] methode, telt deze woorden door te tellen, spaties en einde van regel-tekens aan het einde van elk woord. Vervolgens wordt de totale geschreven naar de Standaarduitvoerstroom met de [Console.Writeline] [ console-writeline] methode.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Bijlage C - de broncode Pi estimator

De pi estimator Java-code waarin de toewijzing en reducer functies is beschikbaar voor inspectie hieronder. Het programma mapper genereert een opgegeven aantal punten die in willekeurige volgorde worden geplaatst in een vierkant van eenheid en vervolgens telt het aantal van die punten die binnen de cirkel. Het programma reducer punten geteld door de mappers worden bij elkaar opgeteld en vervolgens maakt een schatting van de waarde van pi uit de formule 4R, waarbij R de verhouding van het aantal punten geteld binnen de cirkel aan het totale aantal punten die binnen het vierkant is.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Bijlage D - de broncode van 10gb graysort

De code voor het programma TeraSort MapReduce wordt voor inspectie in deze sectie weergegeven.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
