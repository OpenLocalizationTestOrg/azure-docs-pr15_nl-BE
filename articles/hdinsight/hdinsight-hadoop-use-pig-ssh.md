<properties
   pageTitle="Hadoop varkens met SSH te gebruiken op een cluster HDInsight | Microsoft Azure"
   description="Meer informatie over hoe een verbinding met een Linux-gebaseerde Hadoop cluster met SSH en gebruik vervolgens de opdracht varkens varkens Latijns-instructies interactief uitvoeren of als een batchtaak."
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

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Varken taken uitvoeren op een Linux-gebaseerde cluster met de opdracht varken (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In dit document helpt u bij het proces van het verbinden met een Linux-gebaseerde Azure HDInsight cluster met behulp van Secure Shell (SSH), vervolgens met de opdracht varkens varkens Latijns-instructies interactief uitvoeren of als een batchtaak.

De varkens Latijn programmeertaal kunt u transformaties die worden toegepast op de gegevens zijn ingevoerd om de gewenste uitvoer te produceren beschrijven.

> [AZURE.NOTE] Als u al bekend bent met het gebruik van Hadoop op basis van Linux-servers, maar toegevoegd aan de HDInsight zijn, Zie [Tips voor Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, moet u het volgende.

* Een cluster van Linux-gebaseerde HDInsight (Hadoop op HDInsight).

* SSH-client. Linux, Unix en Mac OS moeten worden geleverd met een SSH-client. Windows-gebruikers moeten downloaden van een client, zoals [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met de FQDN-naam (Fully Qualified Domain Name) van het cluster HDInsight met behulp van de opdracht SSH. De FQDN-naam is de naam die u aan het cluster, vervolgens gegeven **. azurehdinsight.net**. Bijvoorbeeld de volgende verbinding te maken met een cluster met de naam **myhdinsight**.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Als u een certificaatsleutel voor SSH-verificatie hebt opgegeven** bij het maken van het cluster HDInsight, wellicht moet u de locatie opgeven van de persoonlijke sleutel op de client-computer.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Als u een wachtwoord voor de SSH-verificatie hebt opgegeven** bij het maken van de HDInsight-cluster, moet u het wachtwoord wanneer daarom wordt gevraagd op te geven.

Zie voor meer informatie over het gebruik van SSH in HDInsight, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (Windows-gebaseerde clients)

Windows beschikt niet over een ingebouwde SSH-client. Beste **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Gebruik de opdracht varken

2. Wanneer een verbinding, met de volgende opdracht start de varkens opdrachtregelinterface (CLI).

        pig

    Na korte tijd, wordt er een `grunt>` vragen.

3. Voer de volgende instructie.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Met deze opdracht laadt de inhoud van het bestand sample.log in LOGBOEKEN. U kunt de inhoud van het bestand weergeven met behulp van de volgende.

        DUMP LOGS;

4. Vervolgens moet u de gegevens transformeren door het toepassen van een reguliere expressie om uit te pakken van de niveau van de logboekregistratie uit elke record met behulp van de volgende.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Kunt u **DUMP** de gegevens weergeven na de transformatie. In dit geval gebruikt u `DUMP LEVELS;`.

5. Transformaties toepassen met behulp van de volgende instructies worden voortgezet. Gebruik `DUMP` om het resultaat van de transformatie na elke stap weer te geven.

    <table>
    <tr>
    <th>Instructie</th><th>Wat het doet</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER niveaus door LOGLEVEL niet null is.</td><td>Rijen met een null-waarde voor het logboek wordt verwijderd en worden de resultaten opgeslagen in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = groep FILTEREDLEVELS door LOGLEVEL;</td><td>Door het logboekniveau rijen worden gegroepeerd en worden de resultaten opgeslagen in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENTIES foreach = GROUPEDLEVELS groep genereren als LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL) als tellen;</td><td>Hiermee maakt u een nieuwe set van gegevens in elk logboek unieke waarde en hoe vaak deze optreedt. Dit bestand bevindt zich in de FREQUENTIES.</td>
    </tr>
    <tr>
    <td>RESULTAAT = order FREQUENTIES door TELLING desc;</td><td>De logboekniveaus orders per aantal (aflopend) en wordt opgeslagen in resultaat.</td>
    </tr>
    </table>

6. U kunt ook de resultaten van een transformatie opslaan met behulp van de `STORE` instructie. Bijvoorbeeld de volgende bespaart de `RESULT` naar de map **/example/data/pigout** op de standaard opslag container voor uw cluster.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] De gegevens worden opgeslagen in de opgegeven map in bestanden met de naam **onderdeel nnnnn**. Als de map al bestaat, wordt een foutbericht weergegeven.

7. Voer de volgende instructie de prompt heidens om af te sluiten.

        QUIT;

###<a name="pig-latin-batch-files"></a>Varken Latijn batch-bestanden

U kunt ook de opdracht varken varken Latijn opgenomen in een bestand wordt uitgevoerd.

3. Gebruik de volgende opdracht naar de pipe STDIN in een bestand met de naam **pigbatch.pig**na het afsluiten van de grunt-prompt. Dit bestand wordt gemaakt in de basismap voor de account die u bent aangemeld voor de SSH-sessie.

        cat > ~/pigbatch.pig

4. Typ of plak de volgende regels en gebruik vervolgens Ctrl + D als u klaar bent.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Gebruik de volgende uit te voeren van het bestand **pigbatch.pig** met de opdracht varken.

        pig ~/pigbatch.pig

    Nadat de batchverwerking is voltooid, ziet u de volgende uitvoer, hetzelfde zijn moet als wanneer u gebruikt `DUMP RESULT;` in de vorige stappen.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Samenvatting

Zoals u ziet, wordt de opdracht varken kunt u interactief MapReduce bewerkingen uitvoeren met Latijnse varkens, alsmede instructies die zijn opgeslagen in een batchbestand uitvoeren.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over varkens in HDInsight.

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight.

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
