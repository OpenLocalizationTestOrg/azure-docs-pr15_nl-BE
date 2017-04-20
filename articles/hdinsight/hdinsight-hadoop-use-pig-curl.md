<properties
   pageTitle="Gebruik van Hadoop varkens met krul in HDInsight | Microsoft Azure"
   description="Leren werken met krul varken Latijn taken uitvoeren op een cluster Hadoop in Azure HDInsight."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Varken taken uitvoeren met Hadoop op HDInsight met behulp van krul

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In dit document leert u hoe varkens Latijn taken uitvoeren op een cluster Azure HDInsight met krul. De varkens Latijn programmeertaal kunt u transformaties die worden toegepast op de gegevens zijn ingevoerd om de gewenste uitvoer te produceren beschrijven.

Krul wordt gebruikt om aan te tonen hoe u kunt gebruiken met HDInsight onbewerkte HTTP-verzoeken uitvoeren, controleren en ophalen van de resultaten van de taken van de varkens. Dit werkt met de WebHCat REST API (voorheen bekend als Templeton) die wordt geleverd door het cluster HDInsight.

> [AZURE.NOTE] Als u al bekend bent met het gebruik van Hadoop op basis van Linux-servers, maar toegevoegd aan de HDInsight zijn, Zie [Tips voor Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een cluster Azure HDInsight (Hadoop op HDInsight) (op basis van Linux of Windows-programma)

* [Krul](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Varken taken uitvoeren met behulp van krul

> [AZURE.NOTE] Als krul of overige communicatie met WebHCat, moet u de aanvragen worden geverifieerd door middel van de gebruikersnaam en het wachtwoord voor de cluster HDInsight. U moet de naam van het cluster ook gebruiken als onderdeel van de URI Uniform Resource Identifier () die wordt gebruikt voor het verzenden van de aanvragen naar de server.
>
> Voor de opdrachten in deze sectie **gebruikersnaam** vervangen door de gebruiker te verifiëren met het cluster en vervang **wachtwoord** door het wachtwoord voor de gebruikersaccount. **CLUSTERNAAM** vervangen door de naam van het cluster.
>
> De REST API wordt via [standaard-verificatie](http://en.wikipedia.org/wiki/Basic_access_authentication)beveiligd. Zorg altijd aanvragen via de HTTP-Secure (HTTPS) om ervoor te zorgen dat uw gegevens veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht om te controleren of u verbinding kunt maken met uw cluster HDInsight vanaf een opdrachtregel:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    U ontvangt een reactie van de volgende strekking:

        {"status":"ok","version":"v1"}

    Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-u**: de gebruikersnaam en het wachtwoord dat wordt gebruikt voor de verificatie van de aanvraag
    * **-G**: geeft aan dat dit een GET-aanvraag

    Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **/status/status**, geeft aan dat het verzoek om terug te keren de status van WebHCat (ook bekend als Templeton) voor de server.

2. De volgende code gebruiken om een varken Latijns-taak aan het cluster:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-d**: omdat `-G` niet wordt gebruikt, wordt standaard het verzoek aan de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met het verzoek.

        * **User.name**: de gebruiker die de opdracht wordt uitgevoerd
        * **uitvoeren**: het varken Latijns-instructies uit te voeren
        * **statusdir**: de map die de status van deze taak worden beschreven

    > [AZURE.NOTE] U ziet dat de ruimten in varken Latijns-instructies worden vervangen door de `+` teken gebruikt in combinatie met krul.

    Deze opdracht moet geven als resultaat een taak-ID kan worden gebruikt voor het controleren van de status van de afdruktaak, bijvoorbeeld:

        {"id":"job_1415651640909_0026"}

3. Controleer de status van de taak, kunt u de volgende opdracht gebruiken. **Taak-id** vervangen door de waarde die in de vorige stap. Als de geretourneerde waarde is bijvoorbeeld `{"id":"job_1415651640909_0026"}`, en vervolgens de **taak-id** is `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Als de afdruktaak is voltooid, worden de status **geslaagd**.

    > [AZURE.NOTE] Deze aanvraag krul geeft als resultaat een JavaScript Object Notation (JSON) document met informatie over de taak en jq wordt gebruikt voor het ophalen van alleen de waarde staat.

##<a id="results"></a>Resultaten weergeven

Wanneer de status van de taak is gewijzigd in **is voltooid**, kunt u de resultaten van het project ophalen uit Azure Blob-opslag. De `statusdir` -parameter die wordt doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **wasbs: / / / voorbeeld/pigcurl**. Dit adres wordt de uitvoer van de taak opgeslagen in de map **voorbeeld/pigcurl** in de opslag die wordt gebruikt door het cluster HDInsight.

U kunt een lijst en deze bestanden downloaden met behulp van de [CLI Azure](../xplat-cli-install.md). Bijvoorbeeld om een lijst met bestanden in **voorbeeld/pigcurl**, gebruik de volgende opdracht:

    azure storage blob list <container-name> example/pigcurl

Een bestand te downloaden met de volgende programmacode:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Moet u de naam van de opslag met de blob met behulp van de `-a` en `-k` parameters of set de **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** omgevingsvariabelen.

##<a id="summary"></a>Samenvatting

Zoals u in dit document, kunt u een raw HTTP-aanvraag worden uitgevoerd, controleren en de resultaten van varkens taken weergeven op het cluster HDInsight.

Zie voor meer informatie over de overige interface die wordt gebruikt in dit artikel de [Verwijzing naar WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over de varkens op HDInsight:

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
