<properties
    pageTitle="Voorbeelden van Hadoop MapReduce uitvoeren op Linux-gebaseerde HDInsight | Microsoft Azure"
    description="Aan de slag met MapReduce monsters met Linux-gebaseerde HDInsight. Verbinding maken met het cluster via SSH en gebruik vervolgens de opdracht Hadoop monster taken uitvoeren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>De monsters Hadoop uitgevoerd in HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Linux-gebaseerde HDInsight clusters bieden een reeks MapReduce monsters die kan worden gebruikt voor de planningfase Hadoop MapReduce uitvoeren van taken. In dit document, informatie over de beschikbare monsters en helpt bij het uitvoeren van een aantal.

##<a name="prerequisites"></a>Vereisten

- **Abonnement op een Azure**: Zie [Azure krijg gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **HDInsight op basis van een Linux cluster**: Zie [aan de slag met Hadoop met de component in HDInsight op Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Een SSH-client**: Zie de volgende artikelen voor meer informatie over het gebruik van SSH in HDInsight:

    - [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>De monsters ##

**Locatie**: de monsters bevinden zich op het cluster HDInsight op **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Inhoud**: de volgende voorbeelden in dit archief zijn opgenomen:

- **aggregatewordcount**: een statistische basis kaart/verminderen programma dat de woorden in de invoerbestanden geteld
- **aggregatewordhist**: een samengestelde kaart/verminderen programma dat het histogram van de woorden in de invoerbestanden berekent op basis
- **bbp**: een kaart/verminderen programma dat gebruikmaakt van Bailey-Borwein-Plouffe voor het berekenen van de exacte cijfers van Pi
- **dbcount**: een voorbeeld van de taak die de pageview logboekbestanden opgeslagen telt in een een-database
- **distbbp**: een kaart/verminderen programma dat gebruikmaakt van een BBP-type formule voor het berekenen van exacte bits van Pi
- **GREP**: een kaart/verminderen programma waarmee de resultaten van een regex in de invoer wordt geteld
- **join**: een taak die een join effecten dan gesorteerd, even gepartitioneerd datasets
- **multifilewc**: een taak die woorden uit verschillende bestanden telt
- **pentomino**: kaart/verminderen legdatum programma om te zoeken naar oplossingen voor problemen pentomino tegel
- **pi**: programma kaart/verminderen ramingen Pi met een quasi-Monte Carlo-methode
- **randomtextwriter**: een kaart/verminderen programma dat 10 GB aan willekeurige tekstuele gegevens per knooppunt schrijft
- **randomwriter**: een kaart/verminderen programma dat 10 GB aan willekeurige gegevens per knooppunt schrijft
- **secondarysort**: een voorbeeld van een secundaire sorteervolgorde definiëren voor het verminderen
- **sorteren**: een kaart/verminderen programma dat de geschreven door de schrijver willekeurige gegevens gesorteerd
- **sudoku**: een sudoku Oplosser
- **teragen**: genereren van gegevens voor de terasort
- **terasort**: uitvoeren van de terasort
- **teravalidate**: controle van de resultaten van terasort
- **WordCount**: een kaart/verminderen programma dat de woorden in de invoerbestanden geteld
- **wordmean**: een kaart/verminderen programma dat de gemiddelde lengte van de woorden in de invoerbestanden telt
- **wordmedian**: een kaart/verminderen programma dat de gemiddelde lengte van de woorden in de invoerbestanden telt
- **wordstandarddeviation**: een kaart/verminderen programma dat de standaarddeviatie van de lengte van de woorden in de invoerbestanden telt

**Broncode**: de broncode voor deze voorbeelden is opgenomen op de cluster HDInsight bij **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] De `2.2.4.9-1` in het pad is de versie van het Platform Hortonworks gegevens voor het cluster HDInsight en kan worden gewijzigd als HDInsight wordt bijgewerkt.

## <a name="how-to-run-the-samples"></a>Het uitvoeren van de monsters ##

1. Verbinding maken met de HDInsight via SSH zoals beschreven in de volgende artikelen:

    - [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Van de `username@#######:~$` vragen, gebruikt u de volgende opdracht om de monsters:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Dit genereert de lijst van het monster uit de vorige sectie van dit document.

3. Gebruik de volgende opdracht voor help bij een specifiek voorbeeld. In dit geval het monster **wordcount** :

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    U moet het volgende bericht:

        Usage: wordcount <in> [<in>...] <out>

    Dit betekent dat u diverse paden van invoer voor de brondocumenten opgeven kunt. Het uiteindelijke pad is de locatie (het aantal woorden in de brondocumenten) van de uitvoer wordt opgeslagen.

4. Voor het tellen van alle woorden in de laptops van Leonardo Da Vinci, die met het cluster beschikbaar worden gesteld als u voorbeeldgegevens gebruikt u het volgende:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Invoer voor deze taak wordt gelezen uit de **wasbs:///example/data/gutenberg/davinci.txt**.

    Uitvoer voor dit voorbeeld is opgeslagen in **wasbs: / / / voorbeeld/data/davinciwordcount**.

    > [AZURE.NOTE] Zoals beschreven in de help voor het monster wordcount, kan u ook meerdere invoerbestanden opgeven. Bijvoorbeeld `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` woorden in zowel davinci.txt als ulysses.txt zou tellen.

5. Nadat de taak is voltooid, gebruikt u de volgende opdracht om de uitvoer weer te geven:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Hierdoor worden alle uitvoerbestanden die worden geproduceerd door de taak en weer te geven. Dit eenvoudige voorbeeld is slechts één bestand, maar als er meer met deze opdracht ze allemaal doorlopen zou.

    De uitvoer ziet er ongeveer als volgt:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Elke regel staat voor een woord en het aantal keren dat deze is opgetreden in de invoergegevens.

## <a name="sudoku"></a>Sudoku

Voorbeeld van de Sudoku is enigszins unhelpful gebruiksinstructies; 'Een puzzel op de opdrachtregel opnemen'.

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is een logische puzzel bestaat uit negen 3 x 3 rasters. Sommige cellen in het raster zijn getallen, terwijl andere leeg zijn en het doel is om op te lossen voor lege cellen. De bovenstaande link is meer informatie over de puzzel, maar het doel van dit voorbeeld is om op te lossen voor lege cellen. Onze input moet dus een bestand dat zich in de volgende notatie:

- Negen rijen van negen kolommen

- Elke kolom kan bevatten ofwel een getal of `?` (geeft een lege cel)

- Cellen worden gescheiden door een spatie

Er is een bepaalde manier te maken van Sudoku puzzels; een getal in een kolom of rij kan niet worden herhaald. Er is een voorbeeld op het cluster HDInsight die goed is opgebouwd. Deze bevindt zich op **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** en bevat het volgende:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] De `2.2.4.9-1` gedeelte van het pad kan veranderen als het cluster HDInsight wordt bijgewerkt.

Gebruik de volgende opdracht als u wilt uitvoeren via de Sudoku-voorbeeld:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

De resultaten moeten met de volgende strekking weergegeven:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>PI (π)

Pi wordt een statistisch (quasi Monte Carlo) methode voor het schatten van de waarde van pi. Punten die in willekeurige volgorde worden geplaatst in een eenheid vallen ook vierkant binnen een cirkel met een kans gelijk is aan de oppervlakte van de cirkel zijn aangebracht binnen dat vierkant pi/4. De waarde van pi kan worden geraamd van de waarde van 4R, waarbij R de verhouding van het aantal punten die binnen de cirkel aan het totale aantal punten die binnen het vierkant is. Hoe groter de punten gebruikt, des te beter de raming is een voorbeeld.

De toewijzing voor dit voorbeeld wordt een aantal punten in een vierkant eenheid willekeurig wordt gegenereerd en vervolgens telt het aantal van die punten die binnen de cirkel zijn.

De reducer vervolgens worden geteld door de mappers punten bij elkaar opgeteld en maakt een schatting van de waarde van pi uit de formule 4R, waarbij R de verhouding van het aantal punten geteld binnen de cirkel aan het totale aantal punten die binnen het vierkant is.

Gebruik de volgende opdracht uit te voeren in dit voorbeeld. Deze maakt gebruik van 16 kaarten met 10.000.000 voorbeelden voor het schatten van de waarde van pi:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

De waarde die wordt geretourneerd door deze moet er ongeveer als **3.14159155000000000000**. De eerste 10 decimalen van pi zijn voor verwijzingen naar 3.1415926535.

##<a name="10gb-greysort"></a>Greysort 10GB

GraySort is een benchmark sorteren waarvan metric het sorteren (TB/minuut) die wordt bereikt is bij het sorteren van zeer grote hoeveelheden gegevens, meestal een 100TB minimum.

In dit voorbeeld wordt een bescheiden 10GB aan gegevens zodat kunnen relatief snel worden uitgevoerd. De MapReduce toepassingen ontwikkeld door Owen O'Malley en Arun Murthy dat de jaarlijkse algemene ("daytona") terabyte sorteren benchmark in 2009 met een snelheid van 0.578 TB/min (100 TB in 173 minuten gewonnen) wordt gebruikt. Zie de website [Sortbenchmark](http://sortbenchmark.org/) voor meer informatie over deze en andere benchmarks sorteren.

In dit voorbeeld drie sets van MapReduce-programma's gebruikt:

- **TeraGen**: een MapReduce programma waarbij rijen met gegevens sorteren

- **TeraSort**: monsters van de ingevoerde gegevens en de gegevens sorteren in een totale order met MapReduce

    TeraSort is een standaard sorteervolgorde van MapReduce-functies, met uitzondering van een aangepaste partitioner die gebruikmaakt van een gesorteerde lijst van N-1 bemonsterd sleutels die de belangrijkste bereik voor elke verminderen definiëren. In het bijzonder alle sleutels die monster [i-1] < = sleutel < monster [i] ik verminderen worden verzonden. Deze garanties dat de uitvoer van ik beperken zijn alle kleiner is dan de uitvoer van i + 1 verminderen.

- **TeraValidate**: een programma MapReduce valideert dat de uitvoer globaal is gesorteerd

    Één kaart per bestand wordt gemaakt in de uitvoermap en elke toewijzing zorgt ervoor dat elke sleutel kleiner dan of gelijk aan de vorige is. De kaart-functie ook records van de eerste en laatste sleutels van elk bestand wordt gegenereerd en de functie verminderen die ervoor zorgt dat de eerste sleutel van i-bestand groter dan de laatste sleutel van het bestand i-1 is. Eventuele problemen worden gerapporteerd als een uitvoer van de verminderen met de sleutels die geplaatst zijn.

Gebruik de volgende stappen uit voor het genereren van gegevens, sorteren en de uitvoer vervolgens valideren:

1. Genereren van 10GB van gegevens, die worden opgeslagen op de HDInsight van standaard clusteropslag op **wasbs: / / / voorbeeld / / 10GB-sorteren-invoer**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    De `-Dmapred.map.tasks` Hadoop vertelt hoeveel map taken voor deze taak. De laatste twee parameters opdracht geven de taak 10GB gegevens maken en opslaan op **wasbs: / / / voorbeeld / / 10GB-sorteren-invoer**.

2. Gebruik de volgende opdracht om de gegevens te sorteren:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    De `-Dmapred.reduce.tasks` Hadoop geeft aan hoeveel minder taken voor de taak. De laatste twee parameters zijn alleen de invoer en uitvoer locaties voor gegevens.

3. Voor het valideren van de gegevens die zijn gegenereerd door het sorteren, gebruikt u het volgende:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Volgende stappen ##

In dit artikel hebt u geleerd hoe de monsters die met de HDInsight op basis van Linux-clusters worden uitgevoerd. Voor een zelfstudie over het gebruik van varkens, component en MapReduce met HDInsight, Zie de volgende onderwerpen:

* [Varken gebruiken in combinatie met Hadoop op HDInsight][hdinsight-use-pig]
* [Component gebruiken in combinatie met Hadoop op HDInsight][hdinsight-use-hive]
* [MapReduce gebruiken in combinatie met Hadoop op HDInsight] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
