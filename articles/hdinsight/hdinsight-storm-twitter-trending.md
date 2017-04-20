<properties
   pageTitle="Twitter volgen van trends onderwerpen met Apache Storm op HDInsight | Microsoft Azure"
   description="Leren werken met Trident voor het maken van een topologie Apache Storm die onderwerpen volgen van trends op Twitter op basis van hashtags bepaalt."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Twitter volgen van trends onderwerpen met Apache Storm op HDInsight bepalen

Leren werken met Trident voor het maken van een topologie Storm die volgen van trends onderwerpen (hash-tags) op Twitter bepaalt.

Trident is een high-level abstractie die hulpmiddelen voor joins, aggregaties groepering, functies en filters biedt. Bovendien voegt Trident primitieven voor stateful, incrementele verwerking doet. In dit voorbeeld ziet u hoe u een topologie met behulp van een aangepaste spout, functie en diverse ingebouwde functies die door Trident kunt samenstellen.

> [AZURE.NOTE] In dit voorbeeld is sterk gebaseerd op de [Trident Storm](https://github.com/jalonsoramos/trident-storm) voorbeeld door Juan Alonso.

##<a name="requirements"></a>Vereisten

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java en de JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">GIT</a>

* Een Twitter-account developer

##<a name="download-the-project"></a>Het project downloaden

Gebruik de volgende code in het project lokaal kopiëren.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topologie

De topologie die in dit voorbeeld is als volgt:

![topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Dit is een vereenvoudigde weergave van de topologie. Meerdere exemplaren van de componenten worden verdeeld over de knooppunten in het cluster.

De Trident-code waarmee de topologie is als volgt:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Deze code doet het volgende:

1. Maakt een nieuwe stroom van de spout. De spout tweets van Twitter opgehaald en deze filters voor specifieke trefwoorden (liefde, muziek en koffie in dit voorbeeld).

2. HashtagExtractor, een aangepaste functie wordt gebruikt voor het ophalen van hash-tags uit elke tweet. Deze worden naar de stroom uitgestoten.

3. De stroom is gegroepeerd op hash tag en doorgegeven aan een lezer. Deze lezer maakt een telling van het aantal keren dat elke hash-code is opgetreden. Deze gegevens blijft in het geheugen behouden. Ten slotte is een nieuwe stroom met de hash-code en het aantal uitgestoten.

4. Omdat we alleen geïnteresseerd in de meest populaire hash-tags voor een bepaalde reeks tweets bent, is de **FirstN** -assembly toegepast, zodat alleen de hoogste 10 waarden, op basis van het aantalveld.

> [AZURE.NOTE] We gebruiken ingebouwde functionaliteit van Trident dan het spout en HashtagExtractor.
>
> Zie voor meer informatie over de werking van de ingebouwde <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">storm.trident.operation.builtin pakket</a>.
>
> Implementaties van Trident staat dan MemoryMapState, Zie het volgende:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Storm Trident elastische zoeken</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident-bestand Vgx.</a>

###<a name="the-spout"></a>De spout

De spout, **TwitterSpout**, maakt gebruik van <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> tweets ophalen uit Twitter. Een filter wordt gemaakt (liefde, muziek en koffie in dit voorbeeld) en binnenkomende tweets (status) die overeenkomen met het filter worden opgeslagen in een gekoppelde wachtrij blokkeren. (Zie de <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Klasse LinkedBlockingQueue</a>voor meer informatie.) Ten slotte worden items uit de wachtrij opgehaald en uitgestoten aan de topologie.

###<a name="the-hashtagextractor"></a>De HashtagExtractor

Als hash-tags, wordt <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> gebruikt voor het ophalen van alle hash-tags die zijn opgenomen in de tweet. Deze worden vervolgens naar de stroom uitgestoten.

##<a name="enable-twitter"></a>Twitter inschakelen

Gebruik de volgende stappen uit om een nieuwe Twitter toepassing registreren en verkrijgen van de consumenten- en token-informatie die nodig is om te lezen van Twitter:

1. Ga naar <a href="https://apps.twitter.com" target="_blank">Twitter Apps</a> en klikt u op **nieuwe toepassing maken** . Bij het invullen van het formulier, laat u het veld **Callback-URL** leeg.

2. Wanneer de toepassing is gemaakt, klikt u op het tabblad **toetsen en Access Tokens** .

3. De **Sleutel van de consument** en de **Consumentgeheim** gegevens kopiëren.

4. Selecteer onderaan de pagina **Mijn toegangstoken maken** als er geen tokens bestaat. Wanneer de tokens zijn gemaakt, moet u de **Access Token** en **Access Token Secret** gegevens kopiëren.

5. Open het bestand **resources/twitter4j.properties** in het **TwitterSpoutTopology** project dat u eerder hebt gekopieerd, voegt u de informatie die u verzameld in de vorige stappen en sla het bestand.

##<a name="build-the-topology"></a>De topologie maken

Gebruik de volgende code in het project te maken:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Test de topologie

Gebruik de volgende opdracht als u de topologie lokaal te testen:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Nadat de topologie is gestart, ziet u gegevens voor foutopsporing met de hash codes en telt verzonden door de topologie. De uitvoer ziet er ongeveer als volgt:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Volgende stappen

Nu dat u de topologie lokaal hebt getest, het implementeren van de topologie ontdekken: [implementeren en beheren van Apache Storm topologieën op HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Ook mogelijk geïnteresseerd in de volgende onderwerpen voor de Storm:

* [Ontwikkelen van Java topologieën voor Storm op HDInsight met behulp van Maven](hdinsight-storm-develop-java-topology.md)

* [Ontwikkelen van C# topologieën voor Storm op HDInsight met behulp van Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Voor meer voorbeelden van de Storm voor HDinsight:

* [Voorbeeld van de topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md)
