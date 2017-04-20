<properties
    pageTitle="MapReduce Java-programma's ontwikkelen voor Linux-gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over het MapReduce Java-programma's ontwikkelen en implementeren op Linux-gebaseerde HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>MapReduce Java-programma's ontwikkelen voor Hadoop op HDInsight Linux

Deze documenten doorloopt u een MapReduce-toepassing maken en vervolgens te implementeren en uitvoeren op een op Linux gebaseerde Hadoop op HDInsight cluster met behulp van Apache Maven.

##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 of hoger (of een gelijkwaardige, zoals OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Een abonnement op Azure**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

De volgende omgevingsvariabelen ingesteld tijdens de installatie van Java en de JDK. Echter, moet u controleren dat ze bestaan en dat ze de juiste waarden voor uw systeem bevatten.

* **JAVA_HOME** - verwijzen naar de map waarin u de Java runtime environment (JRE) is geïnstalleerd. Bijvoorbeeld in een OS X, Unix- of Linux-systeem, moet er een waarde van ongeveer `/usr/lib/jvm/java-7-oracle`. In Windows, zou er een vergelijkbaar met waarde`c:\Program Files (x86)\Java\jre1.7`

* **Pad** - mogen de volgende paden:

    * **JAVA_HOME** (of het equivalente pad)

    * **JAVA_HOME\bin** (of het equivalente pad)

    * De map waarin de Maven is geïnstalleerd

##<a name="create-a-new-maven-project"></a>Maak een nieuw Maven project

1. Wijzigen van een terminalsessie of opdrachtregel in uw ontwikkelomgeving mappen naar de locatie die u wilt opslaan van dit project.

3. Gebruik de opdracht __mvn__ , die is geïnstalleerd met de Maven, voor het genereren van de steiger voor het project.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Hiermee maakt u een nieuwe map in de huidige map met de naam die is opgegeven met de parameter __artifactID__ (**wordcountjava** in dit voorbeeld.) Deze map bevat de volgende items:

    * __pom.xml__ - [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) met de gegevens en configuratie details voor het samenstellen van het project.

    * __src__ - de map met de map __java/main/org/hadoop/apache/voorbeelden__ , waarin de toepassing wordt te ontwerpen.

3. Verwijder het bestand __src/test/java/org/apache/hadoop/examples/apptest.java__ , als deze niet in dit voorbeeld worden gebruikt.

##<a name="add-dependencies"></a>Afhankelijkheden toevoegen

1. Bewerk het bestand __pom.xml__ en voeg de volgende in de `<dependencies>` sectie:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Dit Maven vertelt dat het project de bibliotheken moet (vermeld in &lt;artifactId\>) met een specifieke versie (vermeld in &lt;versie\>). Tijdens het compileren, dit gedownload uit de bibliotheek standaard Maven. U kunt de [Maven opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) meer.

    De `<scope>provided</scope>` vertelt Maven deze afhankelijkheden moeten niet worden verpakt met de toepassing, die zullen worden geleverd door het cluster HDInsight tijdens runtime.

2. Voeg het volgende toe aan het bestand __pom.xml__ . Dit moet binnen de `<project>...</project>` codes in het bestand. bijvoorbeeld tussen `</dependencies>` en `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    De eerste invoegtoepassing configureert de [Maven grijs-Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), die wordt gebruikt voor het bouwen van een uberjar (ook wel een fatjar genoemd), waarin de toepassing afhankelijkheden. Het voorkomt ook dat duplicatie van licenties binnen het pakket jar problemen op sommige systemen veroorzaken kan.

    De invoegtoepassing voor tweede configureert u de Maven-compiler wordt gebruikt voor het instellen van de versie van Java vereist voor deze toepassing naar de versie die op de cluster HDInsight gebruikt.

3. Sla het bestand __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>De MapReduce-toepassing maken

1. Ga naar de map __wordcountjava/src/main/java/org/apache/hadoop/voorbeelden__ en wijzig het bestand __App.java__ in __WordCount.java__.

2. Open het bestand __WordCount.java__ in een teksteditor en de inhoud vervangen door de volgende:

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

    U ziet de pakketnaam is **org.apache.hadoop.examples** en de naam van de klasse **WordCount**. Gebruikt u deze namen wanneer u de taak MapReduce indient.

3. Sla het bestand.

##<a name="build-the-application"></a>De toepassing bouwen

1. Wijzigen in de map __wordcountjava__ als u nog geen er.

2. Gebruik de volgende opdracht maakt u een JAR-bestand met de toepassing:

        mvn clean package

    Dit wordt de vorige build artefacten schoon, eventuele afhankelijkheden die niet al geïnstalleerd zijn, bouwen en verpakken van de toepassing downloaden.

3. Nadat de opdracht is voltooid, bevat de map __wordcountjava/doel__ een bestand met de naam __wordcountjava-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Het bestand __wordcountjava-1.0-SNAPSHOT.jar__ is een uberjar waarin niet alleen de WordCount taak, maar ook afhankelijkheden die de taak vereist tijdens runtime.


##<a id="upload"></a>De jar uploaden

Gebruik de volgende opdracht in het jar-bestand uploaden naar de headnode HDInsight:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Dit kopieert de bestanden van het lokale systeem naar de hoofd-knooppunt.

> [AZURE.NOTE] Als u een wachtwoord hebt gebruikt om uw SSH-account te beveiligen, wordt u gevraagd om het wachtwoord. Als u een SSH-sleutel gebruikt, kan er gebruik van de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Voer de MapReduce.

1. Verbinding maken met de HDInsight via SSH zoals beschreven in de volgende artikelen:

    - [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Gebruik de volgende opdracht uit te voeren van de toepassing MapReduce uit de SSH-sessie:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Dit zal de toepassing WordCount MapReduce gebruiken om te tellen woorden in het bestand davinci.txt en de resultaten op te slaan __wasbs: / / / voorbeeld/data/wordcountout__. Het invoerbestand en de uitvoer worden naar de standaard opslag voor het cluster opgeslagen.

3. Nadat de taak is voltooid, gebruikt u de volgende weergave van de resultaten:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Een lijst met woorden en telt, met de waarden van de volgende strekking wordt weergegeven:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Volgende stappen

In dit document, hebt u geleerd hoe een taak Java MapReduce ontwikkelen. Zie de volgende documenten naar andere manieren om te werken met HDInsight.

- [Gebruik component met HDInsight][hdinsight-use-hive]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

Zie ook de [Java Developer Center](https://azure.microsoft.com/develop/java/)voor meer informatie.

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

