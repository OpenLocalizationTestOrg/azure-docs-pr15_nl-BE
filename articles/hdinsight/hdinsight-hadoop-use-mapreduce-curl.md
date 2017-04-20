<properties
   pageTitle="Gebruik MapReduce en krul met Hadoop in HDInsight | Microsoft Azure"
   description="Informatie over het op afstand uitvoeren van taken MapReduce met Hadoop op HDInsight met krul."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>MapReduce-taken uitvoeren met Hadoop op HDInsight met krul

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit document leert u hoe met krul MapReduce taken uitvoeren op een Hadoop op HDInsight cluster.

Krul wordt gebruikt om aan te tonen hoe u kunt gebruiken met HDInsight onbewerkte HTTP-verzoeken MapReduce taken uitvoeren. Dit werkt met behulp van de WebHCat REST API (voorheen bekend als Templeton) die door de cluster HDInsight.

> [AZURE.NOTE] Als u al bekend bent met Hadoop Linux-gebaseerde servers gebruiken, maar u met HDInsight bent, Zie [Wat u moet weten over Hadoop Linux gebaseerd op HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een Hadoop op de cluster HDInsight (Linux of Windows-)

* [Krul](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>MapReduce-projecten met krul uitgevoerd

> [AZURE.NOTE] Wanneer u krul of overige communicatie met WebHCat, moet u de aanvragen worden geverifieerd door middel van de HDInsight cluster administrator gebruikersnaam en wachtwoord. U moet de naam van het cluster ook gebruiken als onderdeel van de URI die wordt gebruikt voor het verzenden van de aanvragen naar de server.
>
> Vervang **gebruikersnaam** met de gebruiker te verifiëren op het cluster en het **wachtwoord** door het wachtwoord voor de gebruikersaccount voor de opdrachten in deze sectie. **CLUSTERNAAM** vervangen door de naam van het cluster.
>
> De REST API is beveiligd met behulp van [standaard-verificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). U moet altijd aanvragen via HTTPS om ervoor te zorgen dat uw gegevens veilig worden verzonden naar de server maken.

1. Gebruik de volgende opdracht om te controleren of u verbinding kunt maken met uw cluster HDInsight vanaf een opdrachtregel:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    U ontvangt een reactie van de volgende strekking:

        {"status":"ok","version":"v1"}

    Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-u**: geeft aan dat de gebruikersnaam en het wachtwoord dat wordt gebruikt voor de verificatie van de aanvraag
    * **-G**: geeft aan dat dit een GET-aanvraag

    Het begin van de URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen.

2. Als een taak MapReduce, gebruik de volgende opdracht:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Het einde van de URI (mapreduce/jar) wordt WebHCat dat dit verzoek een MapReduce taak van een klasse in een jar-bestand wordt gestart. Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-d**: `-G` niet wordt gebruikt, zodat het verzoek wordt standaard ingesteld op de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met het verzoek.

        * **User.name**: de gebruiker die de opdracht wordt uitgevoerd
        * **JAR**: de locatie van de jar-bestand met de klasse worden uitgevoerd
        * **klasse**: de klasse de logica MapReduce bevat
        * **arg**: de argumenten moeten worden doorgegeven aan de functie MapReduce; in dit geval, de ingevoerde tekst-bestand en de map die worden gebruikt voor de uitvoer

    Deze opdracht moet een taak-ID kan worden gebruikt voor het controleren van de status van de taak retourneren:

        {"id":"job_1415651640909_0026"}

3. Controleer de status van de taak, kunt u de volgende opdracht gebruiken. De **taak-id** vervangen door de waarde die in de vorige stap. Als de geretourneerde waarde is bijvoorbeeld `{"id":"job_1415651640909_0026"}`, en vervolgens de taak-id is `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Als de taak voltooid is, zal de status worden "geslaagd".

    > [AZURE.NOTE] Deze aanvraag krul geeft als resultaat een JSON-document met informatie over de taak. jq wordt gebruikt voor het ophalen van alleen de waarde staat.

4. Wanneer de status van de taak is gewijzigd in **is voltooid**, kunt u de resultaten van het project ophalen uit Azure Blob-opslag. De `statusdir` parameter die wordt doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **wasbs: / / / voorbeeld/curl**. Dit adres wordt de uitvoer van de taak opgeslagen in de map **voorbeeld/krul** in de opslag die wordt gebruikt door het cluster HDInsight.

U kunt een lijst en deze bestanden downloaden met behulp van de [CLI Azure](../xplat-cli-install.md). Gebruik de volgende opdracht, bijvoorbeeld in het **voorbeeld van de**krul/weergeven van bestanden:

    azure storage blob list <container-name> example/curl

Een bestand te downloaden met de volgende programmacode:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Moet u de naam van de opslag met de blob met behulp van de `-a` en `-k` parameters of set de **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** omgevingsvariabelen. Zie [het uploaden van gegevens naar HDInsight](hdinsight-upload-data.md) voor meer informatie.

##<a id="summary"></a>Samenvatting

Zoals u in dit document, kunt u onbewerkte HTTP-aanvraag worden uitgevoerd, controleren en de resultaten van de component taken weergeven in het cluster HDInsight.

Zie voor meer informatie over de overige interface die wordt gebruikt in dit artikel de [Verwijzing naar WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)
