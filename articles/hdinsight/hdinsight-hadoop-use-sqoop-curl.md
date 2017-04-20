<properties
   pageTitle="Gebruik van Hadoop Sqoop met krul in HDInsight | Microsoft Azure"
   description="Leren op afstand indienen Sqoop taken HDInsight met krul."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Sqoop taken uitvoeren met Hadoop in HDInsight met krul

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruiken van krul Sqoop taken uitvoeren op een cluster Hadoop in HDInsight.

Krul wordt gebruikt om aan te tonen hoe u kunt gebruiken met HDInsight onbewerkte HTTP-verzoeken uitvoeren, controleren en ophalen van de resultaten van de Sqoop taken. Dit werkt met behulp van de WebHCat REST API (voorheen bekend als Templeton) die door de cluster HDInsight.

> [AZURE.NOTE] Als u al bekend bent met het gebruik van Hadoop op basis van Linux-servers, maar toegevoegd aan de HDInsight zijn, Zie de [informatie over het gebruik van HDInsight op Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een Hadoop op de cluster HDInsight (Linux of Windows-)

* [Krul](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Sqoop taken verzenden met krul

> [AZURE.NOTE] Als krul of overige communicatie met WebHCat, moet u de aanvragen worden geverifieerd door middel van de gebruikersnaam en het wachtwoord voor de beheerder van de cluster HDInsight. U moet de naam van het cluster ook gebruiken als onderdeel van de URI Uniform Resource Identifier () gebruikt voor het verzenden van de aanvragen naar de server.
>
> Voor de opdrachten in deze sectie **gebruikersnaam** vervangen door de gebruiker te verifiëren met het cluster en vervang **wachtwoord** door het wachtwoord voor de gebruikersaccount. **CLUSTERNAAM** vervangen door de naam van het cluster.
>
> De REST API is via [Basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication)beveiligd. Zorg altijd aanvragen via de HTTP-Secure (HTTPS) om ervoor te zorgen dat uw gegevens veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht om te controleren of u verbinding kunt maken met uw cluster HDInsight vanaf een opdrachtregel:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    U ontvangt een reactie van de volgende strekking:

        {"status":"ok","version":"v1"}

    Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-u** - de gebruikersnaam en het wachtwoord dat wordt gebruikt voor de verificatie van de aanvraag.
    * **-G** - geeft aan dat dit een GET-aanvraag.

    Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **/status/status**, geeft aan dat de aanvraag de status van WebHCat (ook bekend als Templeton) voor de server. 

2. Een sqoop-taak gebruikt u het volgende:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-d** - sinds `-G` niet wordt gebruikt, wordt standaard het verzoek aan de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met het verzoek.

        * **user.name** - de gebruiker die de opdracht wordt uitgevoerd.

        * **opdracht** - Sqoop de opdracht wordt uitgevoerd.

        * **statusdir** - de map die de status van deze taak worden beschreven.

    Deze opdracht moet een taak-ID kan worden gebruikt voor het controleren van de status van de taak retourneren.

        {"id":"job_1415651640909_0026"}

3. Controleer de status van de taak, kunt u de volgende opdracht gebruiken. **Taak-id** vervangen door de waarde die in de vorige stap. Als de geretourneerde waarde is bijvoorbeeld `{"id":"job_1415651640909_0026"}`, en vervolgens de **taak-id** is `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Als de afdruktaak is voltooid, worden de status **geslaagd**.

    > [AZURE.NOTE] Deze aanvraag krul geeft als resultaat een JavaScript Object Notation (JSON) document met informatie over de taak. jq wordt gebruikt voor het ophalen van alleen de waarde staat.

4. Nadat u de status van de taak is gewijzigd in **is voltooid**, kunt u de resultaten van het project ophalen uit Azure Blob-opslag. De `statusdir` -parameter die wordt doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **wasbs: / / / voorbeeld/curl**. Dit adres wordt de uitvoer van de taak opgeslagen in de map **voorbeeld/krul** op de standaard opslag container die wordt gebruikt door het cluster HDInsight.

    U kunt een lijst en deze bestanden downloaden met behulp van de [CLI Azure](../xplat-cli-install.md). Voorbeeld weergeven van bestanden in **voorbeeld/krul**, gebruik de volgende opdracht:

        azure storage blob list <container-name> example/curl

    Een bestand te downloaden met de volgende programmacode:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] U moet opgeven de naam van de opslag met de blob met behulp van de `-a` en `-k` parameters of set de **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** omgevingsvariabelen. Zie < een href = "hdinsight uploaden data.md" target = "_blank" voor meer informatie.

##<a name="limitations"></a>Beperkingen

* Bulk export - met Linux-gebaseerde HDInsight, de Sqoop-connector gebruikt om gegevens te exporteren naar Microsoft SQL Server of SQL-Database Azure biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchen - met Linux-gebaseerde HDInsight bij het gebruik van de `-batch` Ga bij het uitvoeren van toevoegingen, Sqoop meerdere ingevoegd in plaats van de batchverwerking de insert-bewerkingen worden uitgevoerd.

##<a name="summary"></a>Samenvatting

Zoals u in dit document, kunt u een raw HTTP-aanvraag worden uitgevoerd, controleren en de resultaten van Sqoop taken weergeven op het cluster HDInsight.

Zie de <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">handleiding Sqoop REST API</a>voor meer informatie op de overige interface die wordt gebruikt in dit artikel.

##<a name="next-steps"></a>Volgende stappen

Voor algemene informatie over component met HDInsight:

* [Sqoop gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-sqoop.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


