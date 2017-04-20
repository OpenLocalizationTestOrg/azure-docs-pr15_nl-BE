<properties
   pageTitle="Python-componenten gebruiken in een topologie Storm op HDinsight | Microsoft Azure"
   description="Informatie over hoe u kunt onderdelen van Python met Apache Storm op Azure HDInsight. Leert u hoe u onderdelen van beide een Java gebaseerd Python en Clojure Storm topologie gebaseerd."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Ontwikkelen van Apache Storm topologieën met Python op HDInsight

Apache Storm ondersteunt meerdere talen, zelfs zodat u onderdelen van verschillende talen in één topologie te combineren. In dit document leert u hoe Python onderdelen in uw topologieën Java en Storm Clojure gebaseerd op HDInsight.

##<a name="prerequisites"></a>Vereisten

* Python 2.7 of hoger

* Java JDK 1.7 of hoger

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Ondersteuning van meerdere talen storm

Storm is ontworpen om te werken met componenten die zijn geschreven met elke programmeertaal, maar dit vereist dat de onderdelen hoe begrijpt te werken met de [definitie van de Thrift voor de Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Voor een Python, wordt een module geleverd als onderdeel van het project Storm Apache waarmee u gemakkelijk samenwerken met Storm. In deze module kunt u vinden op [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Aangezien Apache Storm is een Java-proces dat wordt uitgevoerd op de Java Virtual Machine (JVM), worden componenten die zijn geschreven in andere talen als subprocessen uitgevoerd. De Storm bits uitgevoerd in JVM communiceert met deze subprocessen met behulp van JSON berichten verzonden via stdin/stdout. Meer informatie over de communicatie tussen onderdelen vindt u in de documentatie van de [Multi-taal-Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>De Storm-module

De storm-module (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py), biedt de bits die nodig zijn voor de Python-onderdelen maken die met de Storm werken.

Dit zorgt voor zaken als `storm.emit` tuples uitzenden en `storm.logInfo` om naar de logboekbestanden te schrijven. Ik zou raden u aan dit bestand gelezen en begrepen wat het biedt.

##<a name="challenges"></a>Uitdagingen

De __storm.py__ -module kunt u Python spouts die verbruiken gegevens en bouten die gegevens, maar de algemene topologie definitie van Storm die communicatie tussen onderdelen van de kabels nog steeds wordt geschreven met behulp van Java of Clojure maken. Als u Java hebt gebruikt, moet u bovendien ook Java-onderdelen die als een interface op de Python componenten fungeren maken.

Ook, aangezien Storm clusters op een gedistribueerde manier worden uitgevoerd, moet u ervoor zorgen dat alle modules die zijn vereist voor de Python onderdelen beschikbaar op alle knooppunten van de werknemer in het cluster zijn. Storm biedt een eenvoudige manier om dit te doen voor resources multi lang - u hebt alle afhankelijkheden opnemen als onderdeel van het jar-bestand voor de topologie of afhankelijkheden handmatig installeren op elk knooppunt van de werknemer in het cluster.

###<a name="java-vs-clojure-topology-definition"></a>Definitie van Java versus Clojure topologie

Van de twee methoden om een topologie te definiëren, is Clojure veruit de gemakkelijkste/zuiverst kunt u rechtstreeks referenc python componenten in de definitie van de topologie. Definities van Java gebaseerde topologie, moet u ook Java-onderdelen die dingen, zoals het declareren van de velden in de tupels geretourneerd van de onderdelen Python verwerken definiëren.

Beide methoden worden beschreven in dit document, met voorbeeldprojecten.

##<a name="python-components-with-a-java-topology"></a>Python-onderdelen met een Java-topologie

> [AZURE.NOTE] In dit voorbeeld is beschikbaar op [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) in de map __JavaTopology__ . Dit is een project op basis van Maven. Als u niet bekend met Maven bent, Zie [ontwikkelen Java gebaseerde topologieën met Apache Storm op HDInsight](hdinsight-storm-develop-java-topology.md) voor meer informatie over het maken van een Maven project voor een Storm-topologie.

Een topologie op basis van Java die gebruikmaakt van Python (of andere onderdelen van de taal JVM) verschijnt in eerste instantie gebruik van Java-onderdelen; maar als u in elk van de Java-spouts/bouten kijkt, ziet u de volgende code:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Dit is waar Java, Python roept en voert het script dat de werkelijke bout logica bevat. De Java-spouts/bouten (bijvoorbeeld deze) declareert u alleen de velden in de tupel die door het onderliggende Python onderdeel zal worden uitgestoten.

De Python bestanden worden opgeslagen in de `/multilang/resources` directory in het volgende voorbeeld. De `/multilang` directory wordt verwezen in de __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir} / multilang</directory>
    </resource>
</resources>

Dit omvat alle bestanden in de `/multilang` map in het oppervlak dat in dit project wordt opgezet.

> [AZURE.IMPORTANT] Merk op dat dit alleen bepaalt de `/multilang` directory en niet `/multilang/resources`. Storm verwacht niet JVM bronnen in een `resources` directory, zodat deze al intern wordt gezocht. Onderdelen in deze map plaatst kunt u alleen verwijzen naar door de naam in de Java-code. Bijvoorbeeld `super("python", "countbolt.py");`. Een andere manier om na te denken van het is dat Storm ziet de `resources` directory als de hoofdmap (/) toegang tot bronnen van multi-taal.
>
> Voor dit voorbeeldproject, de `storm.py` module is opgenomen in de `/multilang/resources` directory.

###<a name="build-and-run-the-project"></a>Bouwen en uitvoeren van het project

Om dit project lokaal worden uitgevoerd, hebt u het volgende Maven bouwen en uitvoeren in de lokale modus:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Gebruik ctrl + c om het proces beëindigen.

Voor de implementatie van het project met een HDInsight cluster Apache Storm, gebruikt u de volgende stappen uit:

1. Een jar uber maken:

        mvn package

    Hiermee maakt u een bestand met de naam __WordCount--1.0-SNAPSHOT.jar__ in de `/target` directory voor dit project.

2. Het jar-bestand uploaden naar het Hadoop-cluster met behulp van een van de volgende methoden:

    * Voor __Linux-gebaseerde__ HDInsight clusters: Gebruik `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` de jar-bestand te kopiëren naar het cluster, vervangen door een gebruikersnaam met uw gebruikersnaam SSH en CLUSTERNAAM de naam van het cluster HDInsight.

        Nadat het bestand is voltooid, verbinding maken met het cluster via SSH en de topologie gebruiken`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * Voor __Windows-gebaseerde__ clusters met HDInsight: verbinding maken met het Dashboard Storm terecht bij de HTTPS://CLUSTERNAME.azurehdinsight.net/ in uw browser. CLUSTERNAAM vervangen door de naam van het cluster HDInsight en bieden de admin en wachtwoord wanneer daarom wordt gevraagd.

        Met behulp van het formulier de volgende acties uitvoeren:

        * __Jar-bestand__: Selecteer __Bladeren__en selecteer vervolgens het bestand __WordCount-1.0-SNAPSHOT.jar__
        * __Klassenaam__: invoeren`com.microsoft.example.WordCount`
        * __Extra maximumconcentratie__: Geef een beschrijvende naam, zoals `wordcount` voor het identificeren van de topologie

        Ten slotte selecteert u __indienen__ bij de topologie te starten.

> [AZURE.NOTE] Eenmaal gestart, een Storm topologie wordt uitgevoerd totdat deze wordt gestopt (gedood). Gebruiken als u wilt stoppen met de topologie, de `storm kill TOPOLOGYNAME` opdracht vanaf de opdrachtregel (SSH sessie naar een Linux cluster, bijvoorbeeld) of met behulp van de gebruikersinterface van de Storm, de topologie selecteren en selecteer vervolgens de knop __verwijderen__ .

##<a name="python-components-with-a-clojure-topology"></a>Python-onderdelen met een topologie voor Clojure

> [AZURE.NOTE] In dit voorbeeld is beschikbaar op [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) in de map __ClojureTopology__ .

Deze topologie is gemaakt door [een nieuw project voor Clojure maken](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project)met behulp van [Leiningen](http://leiningen.org) . Daarna worden de volgende wijzigingen aan het scaffolded project zijn aangebracht:

* __project.CLJ__: afhankelijkheden toegevoegd voor Storm en uitsluitingen voor artikelen die als geïmplementeerd op de server HDInsight problemen veroorzaken.
* __resources/resources__: Leiningen wordt gemaakt van een standaard `resources` map, maar de bestanden hier lijken toegevoegd aan de hoofdmap van het jar-bestand dat is gemaakt op basis van dit project en Storm verwacht dat de bestanden in een submap met de naam `resources`. Een onderliggende map is toegevoegd en de Python bestanden worden opgeslagen in `resources/resources`. Tijdens de uitvoering, wordt dit beschouwd als de hoofdmap (/) voor toegang tot de onderdelen van Python.
* __src/WordCount/Core.CLJ__: dit bestand bevat de definitie van de topologie, en in het bestand __project.clj__ wordt verwezen. Zie voor meer informatie over het definiëren van een topologie Storm met Clojure [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Bouwen en uitvoeren van het project

__Bouwen en uitvoeren van het project lokaal__, gebruikt u de volgende opdracht:

    lein clean, run

Gebruik __Ctrl + C__om te stoppen met de topologie.

__Een uberjar maken en implementeren van HDInsight__, gebruikt u de volgende stappen uit:

1. Maak een uberjar met de topologie en de vereiste afhankelijkheden:

        lein uberjar

    Hiermee maakt u een nieuw bestand met de naam `wordcount-1.0-SNAPSHOT.jar` in de `target\uberjar+uberjar` directory.
    
2. Gebruik een van de volgende methoden te implementeren en uitvoeren van de topologie aan een cluster van HDInsight:

    * __Linux-gebaseerde HDInsight__
    
        1. Kopieer het bestand naar de HDInsight cluster head knooppunt met behulp van `scp`. Bijvoorbeeld:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            GEBRUIKERSNAAM vervangen door een SSH gebruiker voor uw cluster en de CLUSTERNAAM met de naam van het cluster HDInsight.
            
        2. Nadat het bestand is gekopieerd naar het cluster, gebruik SSH verbinding maken met het cluster en de taak verstuurt. Zie voor meer informatie over het gebruik van SSH in HDInsight, een van de volgende opties:
        
            * [SSH gebruiken met Linux-gebaseerde HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [SSH met Linux-gebaseerde HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Zodra verbonden, gebruikt u de volgende de topologie te starten:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Windows-gebaseerde HDInsight__
    
        1. Verbinding maken met het Dashboard Storm terecht bij de HTTPS://CLUSTERNAME.azurehdinsight.net/ in uw browser. CLUSTERNAAM vervangen door de naam van het cluster HDInsight en bieden de admin en wachtwoord wanneer daarom wordt gevraagd.

        2. Met behulp van het formulier de volgende acties uitvoeren:

            * __Jar-bestand__: Selecteer __Bladeren__en selecteer vervolgens het bestand __wordcount-1.0-SNAPSHOT.jar__
            * __Klassenaam__: invoeren`wordcount.core`
            * __Extra maximumconcentratie__: Geef een beschrijvende naam, zoals `wordcount` voor het identificeren van de topologie

            Ten slotte selecteert u __indienen__ bij de topologie te starten.

> [AZURE.NOTE] Eenmaal gestart, een Storm topologie wordt uitgevoerd totdat deze wordt gestopt (gedood). Gebruiken als u wilt stoppen met de topologie, de `storm kill TOPOLOGYNAME` opdracht vanaf de opdrachtregel (SSH sessie naar een Linux-cluster) of met behulp van de gebruikersinterface van de Storm, de topologie selecteren en selecteer vervolgens de knop __verwijderen__ .

##<a name="next-steps"></a>Volgende stappen

In dit document, hebt u geleerd hoe Python componenten gebruiken uit de topologie van een Storm. Zie de volgende documenten naar andere manieren Python gebruiken met HDInsight:

* [Het gebruik van Python voor streaming MapReduce taken](hdinsight-hadoop-streaming-python.md)
* [Het gebruik van Python gebruiker gedefinieerde functies (UDF) van varkens en component](hdinsight-python.md)
