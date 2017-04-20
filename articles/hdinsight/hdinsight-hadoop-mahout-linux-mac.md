<properties
    pageTitle="Aanbevelingen met Mahout en Linux-gebaseerde HDInsight genereren | Microsoft Azure"
    description="Informatie over het leren van de bibliotheek Apache Mahout computer gebruiken voor het genereren van film aanbevelingen op basis van Linux HDInsight (Hadoop)."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Film aanbevelingen genereren met behulp van Apache Mahout met Linux-gebaseerde Hadoop in HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Leren leren-bibliotheek met Azure HDInsight [Apache Mahout](http://mahout.apache.org) computer gebruiken voor het genereren van film aanbevelingen.

Mahout is een [machine learning] [ ml] library voor Apache Hadoop. Mahout bevat algoritmen voor het verwerken van gegevens, zoals filteren, indelen, en clusters. In dit artikel gebruikt u een aanbeveling engine voor het genereren van film aanbevelingen op basis van films die je vrienden hebt gezien.

> [AZURE.NOTE] De stappen in dit document moet een Linux-gebaseerde Hadoop op HDInsight cluster. Zie voor meer informatie over het gebruik van Mahout met een op Windows gebaseerde cluster [genereren film aanbevelingen met behulp van Apache Mahout met op Windows gebaseerde Hadoop in HDInsight](hdinsight-mahout.md)

##<a name="prerequisites"></a>Vereisten

* Linux-gebaseerde Hadoop op HDInsight cluster. Zie voor informatie over het maken van een [aan de slag met Linux-gebaseerde Hadoop in HDInsight][getstarted]

##<a name="mahout-versioning"></a>Mahout versiebeheer

Zie voor meer informatie over de versie van Mahout deel uitmaakt van het cluster HDInsight [versies HDInsight en Hadoop-onderdelen](hdinsight-component-versioning.md).

> [AZURE.WARNING] Het is mogelijk een andere versie van Mahout uploaden naar het cluster HDInsight, alleen-onderdelen van het cluster HDInsight worden volledig ondersteund en Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze componenten.
>
> Aangepaste onderdelen ontvangen commercieel redelijke ondersteuning waarmee u het probleem verder oplossen. Dit kan leiden tot het oplossen van het probleem of vraag aan een van de beschikbare kanalen voor de open-source technologieën waarbij diepe expertise voor de technologie die wordt gevonden. Er zijn bijvoorbeeld veel sites van de community die kunnen worden gebruikt, zoals: [MSDN forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projecten hebben ook projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

##<a name="recommendations"></a>Wat zijn aanbevelingen

Een van de functies die beschikbaar is in de Mahout is een aanbeveling engine. Deze engine accepteert de gegevens in de indeling van `userID`, `itemId`, en `prefValue` (gebruikers van de voorkeur voor het artikel). Mahout kunt collega exemplaar analyse bepaalt vervolgens uitvoeren: _gebruikers met een voorkeur voor een artikel ook een voorkeur voor deze andere artikelen hebben_. Mahout bepaalt vervolgens gebruikers met dergelijke artikel voorkeuren, die kunnen worden gebruikt om aanbevelingen te doen.

Het volgende is een zeer eenvoudig voorbeeld met films:

* __Collega exemplaar__: Jan, Alice en Bob beviel _Star Wars_ _Terug stakingen in het rijk_en _van de Jedi terug_. Mahout bepaalt dat gebruikers die een van deze films ook zoals de andere twee.

* __Collega exemplaar__: Bob en Els ook interessant vond _De Phantom Menace_en _een aanval van de klonen_ _Revenge van de Sith_. Mahout bepaalt dat gebruikers die de vorige drie films ook interessant vond zoals deze drie.

* __Aanbeveling van de overeenkomsten__: Joe omdat de eerste drie films beviel, Mahout kijkt films die door anderen met soortgelijke voorkeuren beviel, maar Joe niet heeft gevolgd (beviel/geclassificeerd). In dat geval raadt Mahout _The Phantom Menace_en _een aanval van de klonen_ _Revenge van de Sith_.

###<a name="understanding-the-data"></a>Wat zijn de gegevens?

Gemakkelijk [GroupLens Research] [ movielens] beoordeling van gegevens voor de films in een indeling die compatibel is met de Mahout bevat. Deze gegevens zijn beschikbaar op uw van standaard clusteropslag op `/HdiSamples/HdiSamples/MahoutMovieData`.

Er zijn twee bestanden `moviedb.txt` (informatie over de films,) en `user-ratings.txt`. De gebruiker-ratings.txt-bestand wordt gebruikt tijdens de analyse, terwijl moviedb.txt wordt gebruikt om beschrijvende tekst gegevens bij het weergeven van de resultaten van de analyse.

De gegevens in de gebruiker ratings.txt heeft een structuur van `userID`, `movieID`, `userRating`, en `timestamp`, die vertellen hoe sterk een film het prioriteitsniveau van elke gebruiker. Hier volgt een voorbeeld van de gegevens:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>De analyse uitvoeren

Gebruik de volgende opdracht als u de aanbeveling taak uit te voeren:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] De taak kan enige tijd duren om te voltooien, en meerdere MapReduce taken kan uitvoeren.

##<a name="view-the-output"></a>Weergave van de uitvoer

1. Nadat de taak is voltooid, gebruik de volgende opdracht om de gegenereerde uitvoer weer te geven:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    De uitvoer wordt als volgt weergegeven:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    De eerste kolom is de `userID`. De waarden in ' [' en ']' worden `movieId`:`recommendationScore`.

2. De uitvoer, samen met de moviedb.txt, kunt u meer beschrijvende gebruikersgegevens weergeven. Eerst moet de bestanden kopiëren lokaal met de volgende opdrachten:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Dit wordt de uitvoergegevens kopiëren naar een bestand genaamd **recommendations.txt** in de huidige map, samen met de gegevensbestanden voor de film.

3. Gebruik de volgende opdracht om een nieuwe Python script maken dat film namen opgeven voor de gegevens in de uitvoer van aanbevelingen eruit:

        nano show_recommendations.py

    Wanneer de editor wordt geopend, gebruikt u het volgende als de inhoud van het bestand:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Druk op **Ctrl-X**, **Y**, en ten slotte **Enter gegevens opslaan.**

3. Gebruik de volgende opdracht om het bestand uitvoerbaar:

        chmod +x show_recommendations.py

4. De Python script uitvoeren. Het volgende wordt ervan uitgegaan dat u in de map waar alle bestanden zijn gedownload:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Dit bekijkt de aanbevelingen voor de gebruiker-ID 4 gegenereerd.

    * De **gebruiker ratings.txt** -bestand wordt gebruikt voor het ophalen van films die de gebruiker heeft beoordeeld.
    * Het bestand **moviedb.txt** wordt gebruikt voor het ophalen van de namen van de films
    * De **recommendations.txt** wordt gebruikt voor het ophalen van de film aanbevelingen voor deze gebruiker

    De uitvoer van deze opdracht is vergelijkbaar met het volgende:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Tijdelijke bestanden verwijderen

Mahout taken verwijderd tijdelijk gegevens die zijn gemaakt tijdens het verwerken van de taak niet. De `--tempDir` parameter is opgegeven in het voorbeeld-project voor het isoleren van de tijdelijke bestanden naar een specifiek pad voor eenvoudig verwijderen. De tijdelijke bestanden verwijderen, gebruik de volgende opdracht:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Als u wilt dat de opdracht opnieuw uitvoeren, moet u ook de uitvoermap verwijderen. Gebruik het volgende om deze map te verwijderen:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Mahout, andere manieren van werken met gegevens op HDInsight ontdekken:

* [Component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight](hdinsight-use-pig.md)
* [MapReduce met HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
