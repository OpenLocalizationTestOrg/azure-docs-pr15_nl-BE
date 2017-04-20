<properties
 pageTitle="MapReduce broeien Maven projecten ontwikkelen | Microsoft Azure"
 description="Informatie over het Maven gebruik maken van een taak MapReduce broeien en vervolgens implementeren en uitvoeren van de taak op een Hadoop op HDInsight cluster."
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>MapReduce broeien projecten met Apache Hadoop op HDInsight ontwikkelen

Broeien is een Scala-bibliotheek waarmee u gemakkelijk Hadoop MapReduce taken maken. Het biedt een beknopte syntaxis, alsook de nauwe integratie met Scala.

Informatie over het gebruik van Maven voor het maken van een eenvoudige word count MapReduce taak geschreven in Scalding in dit document. Vervolgens leert u hoe u deze taak uitvoeren op een cluster HDInsight implementeren.

## <a name="prerequisites"></a>Vereisten

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een Windows- of Linux op basis van Hadoop op HDInsight cluster**. Zie [voorziening Linux-gebaseerde Hadoop op HDInsight](hdinsight-hadoop-provision-linux-clusters.md) of [Hadoop voorziening Windows gebaseerd op HDInsight](hdinsight-provision-clusters.md) voor meer informatie.

* **[Maven](http://maven.apache.org/)**

* **[Platform voor Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 of hoger**

## <a name="create-and-build-the-project"></a>Maken en bouwt het project

1. Gebruik de volgende opdracht een nieuwe Maven-project te maken:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Met deze opdracht maakt een nieuwe map met de naam **scaldingwordcount**en de steiger voor Scala toepassingen maken.

2. Open het **pom.xml** bestand in de map **scaldingwordcount** en de inhoud vervangen door de volgende:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Dit bestand wordt beschreven voor het project, afhankelijkheden en Plug-ins. Hier vindt u de belangrijke posten:

    * **maven.compiler.Source** en **maven.compiler.target**: Hiermee stelt u de Java-versie van dit project

    * **bibliotheken**: de opslagplaatsen met afhankelijkheidsbestanden die worden gebruikt door dit project

    * **broeien core_2.11** en **hadoop-core**: dit project hangt af van zowel Scalding als Hadoop core pakketten

    * **scala-maven-plugin**: plugin scala toepassingen compileren

    * **grijs-maven-plugin**: plugin voor het maken van gearceerde potten (fat). Deze invoegtoepassing wordt toegepast, filters en transformaties; specificially:

        * **filters**: de filters zijn toegepast wijzigen de meta-informatie in het jar-bestand met opgenomen. Om te voorkomen dat ondertekening uitzonderingen tijdens runtime, verschillende bestanden kunnen worden opgenomen met afhankelijkheden uitsluit.

        * **uitvoeringen**: de configuratie van het uitvoeren van fase geeft de klasse **com.twitter.scalding.Tool** de hoofdklasse voor het pakket. Zonder deze moet u zowel com.twitter.scalding.Tool als de klasse die de toepassingslogica bevat wanneer de taak wordt uitgevoerd met de opdracht hadoop opgeven.

3. De map **src/test** verwijderen als u geen tests met dit voorbeeld maakt.

4. Open het bestand **src/main/scala/com/microsoft/example/App.scala** en de inhoud vervangen door de volgende:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Dit implementeert een aantal word basic taak.

5. Opslaan en sluiten van de bestanden.

6. Gebruik de volgende opdracht uit de map **scaldingwordcount** en de toepassing van pakket:

        mvn package

    Zodra deze taak is voltooid, kunt u het pakket met de WordCount toepassing vinden op **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## <a name="run-the-job-on-a-linux-based-cluster"></a>De taak uitvoeren op een op Linux gebaseerde cluster

> [AZURE.NOTE] De volgende stappen gebruiken SSH en de opdracht Hadoop. Zie voor andere methoden voor het uitvoeren van taken MapReduce, [Gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

1. Gebruik de volgende opdracht om het pakket te uploaden naar uw cluster HDInsight:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Dit kopieert de bestanden van het lokale systeem naar de hoofd-knooppunt.

    > [AZURE.NOTE] Als u een wachtwoord hebt gebruikt om uw SSH-account te beveiligen, wordt u gevraagd om het wachtwoord. Als u een SSH-sleutel gebruikt, kan er gebruik van de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld:`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Gebruik de volgende opdracht als u verbinding maakt met het hoofd clusterknooppunt:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Als u een wachtwoord hebt gebruikt om uw SSH-account te beveiligen, wordt u gevraagd om het wachtwoord. Als u een SSH-sleutel gebruikt, kan er gebruik van de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld:`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Wanneer een verbinding met het hoofdkantoor knooppunt, moet u de volgende opdracht kunt de word count

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Hiermee voert de WordCount klasse die u eerder hebt geïmplementeerd. `--hdfs`Hiermee geeft u de taak HDFS gebruiken. `--input`Hiermee geeft u het bestand invoertekst terwijl `--output` geeft de uitvoerlocatie.

4. Nadat de taak is voltooid, gebruikt u de volgende om de uitvoer weer te geven.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Hierdoor worden de volgende informatie weergegeven:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>De taak uitvoeren op een op Windows gebaseerde cluster

De volgende stappen gebruikt Windows PowerShell. Zie voor andere methoden voor het uitvoeren van taken MapReduce, [Gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Start Azure PowerShell en Log in op uw account Azure. Na het opgeven van uw referenties, retourneert de opdracht informatie over uw account.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Als er meerdere abonnementen, bieden de abonnement-id die u wilt gebruiken voor de implementatie.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] U kunt `Get-AzureRMSubscription` voor een lijst van alle abonnementen die zijn gekoppeld aan uw account, inclusief de abonnements-Id voor elk item.

4. het volgende script gebruiken voor het uploaden en uitvoeren van de taak WordCount. Vervangen `CLUSTERNAME` met de naam van uw HDInsight cluster en zorg ervoor dat `$fileToUpload` is het juiste pad naar het bestand __scaldingwordcount-1.0-SNAPSHOT.jar__ .

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Wanneer u het script uitvoert, wordt u gevraagd admin gebruikersnaam en wachtwoord voor de cluster HDInsight invoeren. Fouten die optreden tijdens het uitvoeren van de taak wordt geregistreerd aan de console.
     
6. Nadat de taak is voltooid, wordt de uitvoer naar de __uitvoer.txt__ in de huidige map worden gedownload. Gebruik de volgende opdracht om de resultaten weer te geven.

        cat output.txt

    Het bestand moet de volgende waarden bevatten:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe MapReduce taken maken voor een HDInsight met Scalding, via de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.

* [Gebruik component met HDInsight](hdinsight-use-hive.md)

* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
