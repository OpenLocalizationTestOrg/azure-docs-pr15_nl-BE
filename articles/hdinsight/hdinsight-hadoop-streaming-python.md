<properties
   pageTitle="Ontwikkelen van Python MapReduce projecten met HDInsight | Microsoft Azure"
   description="Informatie over het maken en uitvoeren van taken Python MapReduce op HDInsight op basis van Linux-clusters."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Ontwikkelen van Python streaming programma's voor HDInsight

Hadoop biedt een streaming API voor MapReduce waarmee u kunt schrijven met hyperlinks en functies in andere talen dan Java te verminderen. In dit artikel leert u hoe u Python MapReduce bewerkingen uit te voeren.

> [AZURE.NOTE] Terwijl de Python code in dit document kan worden gebruikt met een cluster met Windows-gebaseerde HDInsight, zijn de stappen in dit document specifiek voor Linux gebaseerde clusters.

Dit artikel is gebaseerd op informatie en voorbeelden die worden gepubliceerd door Michael Noll bij het [schrijven van een programma van Hadoop MapReduce in Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/).

##<a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een Linux-gebaseerde Hadoop op HDInsight cluster

* Een teksteditor

    > [AZURE.IMPORTANT] De editor moet LF gebruiken als het einde van de regel. Als deze CRLF gebruikt, hierdoor worden fouten bij het uitvoeren van de taak MapReduce op HDInsight op basis van Linux-clusters. Als u niet zeker weet, gebruikt u de optionele stap in de sectie [MapReduce uitvoeren](#run-mapreduce) omzetten elke CRLF in LF.

* Voor Windows-clients, stopverf en PSCP. Deze hulpprogramma's zijn beschikbaar vanaf de <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Downloadpagina stopverf</a>.

##<a name="word-count"></a>Woorden tellen

In dit voorbeeld voert u een aantal eenvoudige woorden met behulp van een toewijzing en reducer. De mapper zinnen opgesplitst in afzonderlijke woorden en de reducer de woorden worden samengevoegd en telt de uitvoer te produceren.

Het volgende stroomdiagram ziet u wat er gebeurt tijdens de kaart en fasen te verminderen.

![afbeelding van de kaart beperken](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Waarom Python?

Python is een algemene, op hoog niveau programmeertaal waarmee u expliciete concepten in minder coderegels dan veel andere talen. Onlangs heeft werd populair onder wetenschappers gegevens als de taal van een prototype maken omdat de geïnterpreteerde aard, typt u dynamische en elegante syntaxis maken het geschikt is voor snelle ontwikkeling van toepassingen.

Python is geïnstalleerd op alle clusters van HDInsight.

##<a name="streaming-mapreduce"></a>Streaming MapReduce

Hadoop kunt u een bestand opgeeft dat de toewijzing bevat en de logica die wordt gebruikt door een taak te verkleinen. De specifieke voorschriften voor de toewijzing en logica te verminderen zijn:

* **Input**: de kaart en minder onderdelen invoergegevens moeten lezen van STDIN.

* **Uitvoer**: de kaart en minder onderdelen uitvoergegevens moeten schrijven naar STDOUT.

* **Gegevensindeling**: de gegevens geproduceerd en geconsumeerd moeten een sleutel/waarde-paar, gescheiden door een tabteken.

Python kan deze eisen eenvoudig verwerken met behulp van de module **sys** lezen van STDIN en **afdrukken** gebruiken om af te drukken naar STDOUT. De resterende taak is eenvoudig opmaken van de gegevens met een tab (`\t`) teken tussen de sleutel en de waarde.

##<a name="create-the-mapper-and-reducer"></a>De toewijzing en reducer maken

De toewijzing en reducer zijn tekstbestanden, in dit geval **mapper.py** en **reducer.py** (om te zorgen dat doet wat duidelijk). U kunt deze met de editor van uw keuze te maken.

###<a name="mapperpy"></a>Mapper.PY

Maak een nieuw bestand met de naam **mapper.py** en de volgende code gebruiken als de inhoud:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Neem even de tijd om te lezen via de code, zodat u kunt begrijpen wat het doet.

###<a name="reducerpy"></a>Reducer.PY

Maak een nieuw bestand met de naam **reducer.py** en de volgende code gebruiken als de inhoud:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Bestanden uploaden

Zowel **mapper.py** als **reducer.py** moeten op de hoofd-knooppunt van het cluster voordat we kan worden uitgevoerd. De eenvoudigste manier om te uploaden is te gebruiken **scp** (**pscp** als u een Windows-client).

Gebruik de volgende opdracht op in dezelfde map als het **mapper.py** en **reducer.py**, in de client. **Gebruikersnaam** vervangen door een SSH gebruiker en **clusternaam** de naam van het cluster.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Dit kopieert de bestanden van het lokale systeem naar de hoofd-knooppunt.

> [AZURE.NOTE] Als u een wachtwoord hebt gebruikt om uw SSH-account te beveiligen, wordt u gevraagd om het wachtwoord. Als u een SSH-sleutel gebruikt, kan er gebruik van de `-i` parameter en het pad naar de persoonlijke sleutel, bijvoorbeeld, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>MapReduce uitvoeren

1. Verbinding maken met het cluster via SSH:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Als u een wachtwoord hebt gebruikt om uw SSH-account te beveiligen, wordt u gevraagd om het wachtwoord. Als u een SSH-sleutel gebruikt, kan er gebruik van de `-i` parameter en het pad naar de persoonlijke sleutel, bijvoorbeeld, `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Optioneel) Als u een teksteditor die CRLF gebruikt als een regel die eindigt bij het maken van de bestanden mapper.py en reducer.py, of niet weet wat uw editor lijn-uitgang wordt gebruikt, gebruikt u de volgende opdrachten waarin CRLF in mapper.py en reducer.py converteren naar LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Gebruik de volgende opdracht om de MapReduce taak te starten.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Deze opdracht heeft de volgende onderdelen:

    * **hadoop-streaming.jar**: bij het uitvoeren van bewerkingen voor streaming MapReduce gebruikt. Het Hadoop-interfaces met de externe MapReduce-code die u opgeeft.

    * **-bestanden**: Hadoop geeft aan dat de opgegeven bestanden nodig zijn voor deze taak MapReduce en ze moeten worden gekopieerd naar alle knooppunten van de werknemer.

    * **-mapper**: Hadoop aangeeft welk bestand moet worden gebruikt als de toewijzing.

    * **-reducer**: Hadoop aangeeft welk bestand moet worden gebruikt als de reducer.

    * **-invoer**: het invoerbestand dat wij moeten tellen van woorden.

    * **-uitvoer**: de map waarin de uitvoer wordt geschreven.

        > [AZURE.NOTE] Deze map wordt gemaakt door de taak.

U moet Zie een aantal overzichten **INFO** als de taak wordt gestart en ten slotte de **kaart** en **Verminder** bewerking weergegeven als percentages.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

U ontvangt statusinformatie weergegeven over de taak als voltooid is.

##<a name="view-the-output"></a>Weergave van de uitvoer

Wanneer de taak voltooid is, gebruikt u de volgende opdracht om de uitvoer weer te geven:

    hdfs dfs -text /example/wordcountout/part-00000

Dit moet een lijst weergeven van woorden en hoe vaak het woord is opgetreden. Hier volgt een voorbeeld van de uitvoergegevens:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd dat het gebruik van streaming MapRedcue taken met HDInsight, via de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.

* [Gebruik component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
