<properties
   pageTitle="Hadoop varkens gebruiken met extern bureaublad in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van de opdracht varken varken Latijns-instructies uitvoeren vanaf een verbinding met extern bureaublad met een Windows-gebaseerde Hadoop cluster in HDInsight."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Varken taken uitvoeren vanaf een extern bureaublad-verbinding

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat een stapsgewijze instructies voor het gebruik van de opdracht varkens varkens Latijns-instructies uitvoeren vanaf een verbinding met extern bureaublad naar een cluster met Windows-gebaseerde HDInsight. Varken Latijn kunt u MapReduce toepassingen door een beschrijving van de gegevenstransformaties te maken in plaats van toewijzen en functies te beperken.

In dit document wordt beschreven hoe u

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, moet u het volgende.

* Een cluster van Windows-gebaseerde HDInsight (Hadoop op HDInsight)

* Een clientcomputer met Windows 10, Windows 8 of Windows 7

##<a id="connect"></a>Verbinding met extern bureaublad

Extern bureaublad inschakelen voor het cluster HDInsight en vervolgens verbinding maken met het door de instructies op [verbinding maken met clusters van HDInsight met RDP](hdinsight-administer-use-management-portal.md#rdp).

##<a id="pig"></a>Gebruik de opdracht varken

2. Nadat u een verbinding met extern bureaublad hebt, start u de **opdrachtregel Hadoop** met behulp van het pictogram op het bureaublad.

2. De varkens de opdracht start gebruikt u het volgende:

        %pig_home%\bin\pig

    U krijgt een `grunt>` vragen.

3. Voer de volgende instructie:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Met deze opdracht laadt de inhoud van het bestand sample.log in het bestand LOGBOEKEN. U kunt de inhoud van het bestand weergeven door met de volgende opdracht:

        DUMP LOGS;

4. De gegevens transformeren door het toepassen van een reguliere expressie om uit te pakken van de niveau van de logboekregistratie uit elke record:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Kunt u **DUMP** de gegevens weergeven na de transformatie. In dit geval `DUMP LEVELS;`.

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
    <td>FREQUENTIES foreach = GROUPEDLEVELS groep genereren als LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL) als tellen;</td><td>Hiermee maakt u een nieuwe set van gegevens in elk logboek unieke waarde en hoe vaak deze optreedt. Dit bestand bevindt zich in de FREQUENTIES</td>
    </tr>
    <tr>
    <td>RESULTAAT = order FREQUENTIES door TELLING desc;</td><td>De logboekniveaus orders per aantal (aflopend) en wordt opgeslagen in resultaat</td>
    </tr>
    </table>

6. U kunt ook de resultaten van een transformatie opslaan met behulp van de `STORE` instructie. Bijvoorbeeld de volgende opdracht worden de `RESULT` naar de map **/example/data/pigout** in de opslag voor uw cluster:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] De gegevens worden opgeslagen in de opgegeven map in bestanden met de naam **onderdeel nnnnn**. Als de map al bestaat, ontvangt u een foutbericht.

7. Voer de volgende instructie de prompt heidens om af te sluiten.

        QUIT;

###<a name="pig-latin-batch-files"></a>Varken Latijn batch-bestanden

U kunt ook de opdracht varken Latijn varken dat is opgenomen in een bestand wordt uitgevoerd.

3. Open **Kladblok** en maak een nieuw bestand met de naam **pigbatch.pig** in de map **% PIG_HOME %** na het afsluiten van de grunt-prompt.

4. Typ of plak de volgende regels in het bestand **pigbatch.pig** en vervolgens op te slaan:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Gebruik de volgende uit te voeren van het **pigbatch.pig** -bestand met de opdracht varken.

        pig %PIG_HOME%\pigbatch.pig

    Wanneer de batchverwerking is voltooid, ziet u de volgende uitvoer, hetzelfde zijn moet als wanneer u gebruikt `DUMP RESULT;` in de vorige stappen:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Samenvatting

Zoals u ziet, wordt de opdracht varken kunt u interactief MapReduce bewerkingen uitvoeren of varken Latijns-projecten die zijn opgeslagen in een batch-bestand uitgevoerd.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over varkens in HDInsight:

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
