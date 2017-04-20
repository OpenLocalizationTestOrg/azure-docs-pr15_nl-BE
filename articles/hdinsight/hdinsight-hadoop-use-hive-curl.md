<properties
   pageTitle="Hadoop-component gebruiken met krul in HDInsight | Microsoft Azure"
   description="Informatie over het HDInsight met krul varken taken op afstand indienen."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Component-query's uitvoeren met Hadoop in HDInsight met krul

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit document leert u hoe met krul component query's uitvoeren op een Hadoop op Azure HDInsight cluster.

Krul wordt gebruikt om aan te tonen hoe u kunt gebruiken met HDInsight onbewerkte HTTP-verzoeken uitvoeren, controleren en de resultaten van query's component ophalen. Dit werkt met behulp van de WebHCat REST API (voorheen bekend als Templeton) die door de cluster HDInsight.

> [AZURE.NOTE] Als u al bekend bent met het gebruik van Hadoop op basis van Linux-servers, maar toegevoegd aan de HDInsight zijn, Zie [Wat u moet weten over Hadoop op Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een Hadoop op de cluster HDInsight (Linux of Windows-)

* [Krul](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Component-query's uitvoeren met behulp van krul

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

    Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **/status/status**, geeft aan dat de aanvraag de status van WebHCat (ook bekend als Templeton) voor de server. U kunt ook de versie van de component aanvragen met de volgende opdracht:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    Dit moet het resultaat van de volgende strekking:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Maak een nieuwe tabel met de naam **log4jLogs**gebruikt u het volgende:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-d** - sinds `-G` niet wordt gebruikt, wordt standaard het verzoek aan de POST-methode. `-d`Hiermee geeft u de waarden die worden verzonden met het verzoek.

        * **user.name** - de gebruiker die de opdracht wordt uitgevoerd.

        * **execute** - instructies voor de HiveQL uit te voeren.

        * **statusdir** - de map die de status van deze taak worden beschreven.

    Deze instructies worden de volgende acties uitvoeren:

    * **DROP TABLE** - Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.

    * **Externe tabel maken** - maakt een nieuwe 'externe' tabel in een component. Alleen de tabeldefinitie opslaan externe tabellen in de component. De gegevens blijft op de oorspronkelijke locatie.

        > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u verwacht de onderliggende gegevens worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens uploadproces of door een andere MapReduce bewerking, waarbij altijd component query's dat gebruiken de meest recente gegevens.
        >
        > Neerzetten van een externe tabel heeft **niet** verwijderen de gegevens, alleen de tabeldefinitie.

    * **Rij-indeling** : vertelt component hoe de gegevens zijn ingedeeld. In dit geval worden de velden in elk logboek gescheiden door een spatie.

    * **Locatie van opgeslagen als TEXTFILE** - vertelt component waarop de gegevens zijn opgeslagen (de map voorbeeld/gegevens) en dat deze wordt opgeslagen als tekst.

    * **Selecteer** - Hiermee selecteert u een telling van alle rijen waarin kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren als er drie rijen die deze waarde bevatten.

    > [AZURE.NOTE] U ziet dat de afstand tussen de HiveQL-instructies worden vervangen door de `+` teken gebruikt in combinatie met krul. Geciteerde waarden met een spatie, zoals het scheidingsteken, niet mag worden vervangen door `+`.

    * **INPUT__FILE__NAME LIKE '% 25.log'** - dit beperkt de zoekopdracht alleen bestanden die eindigen gebruik. log. Als dit niet aanwezig is, probeert component te zoeken in alle bestanden in deze map en alle submappen, inclusief bestanden die niet overeenkomen met het schema kolom is gedefinieerd voor deze tabel.

    > [AZURE.NOTE] Let erop dat % 25 is de URL gecodeerd formulier %, zodat de werkelijke conditie is `like '%.log'`. De % moet worden als URL zijn gecodeerd, wordt deze behandeld als een speciale tekens in URL's.

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

6. Gebruik de volgende instructies voor het maken van een nieuwe "interne" tabel met de naam **errorLogs**:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Deze instructies worden de volgende acties uitvoeren:

    * **Maken van tabel als niet bestaat** - maakt een tabel, als deze nog niet bestaat. Aangezien de **externe** sleutelwoord niet gebruikt wordt, is dit een interne tabel die is opgeslagen in het gegevenswarehouse component en wordt volledig beheerd door component.

        > [AZURE.NOTE] In tegenstelling tot de externe tabellen wordt neerzetten van een interne tabel verwijderd en de onderliggende gegevens.

    * **Opgeslagen als ORC** - worden de gegevens opgeslagen in de indeling voor geoptimaliseerde rij in kolomvorm (ORC). Dit is een zeer geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens van de component.
    * OVERSCHRIJVEN INSERT **... Selecteer** - rijen uit de tabel **log4jLogs** bevat **[fout]**selecteert en vervolgens de gegevens in de tabel **errorLogs** ingevoegd.
    * **Selecteren** - Hiermee selecteert u alle rijen uit de tabel met nieuwe **errorLogs** .

7. Gebruik de taak-ID teruggestuurd naar de status van de taak. Zodra deze is voltooid, gebruik Azure CLI voor Mac, Linux en Windows beschreven eerder te downloaden en de resultaten bekijken. De uitvoer bevat drie regels, die alle **[fout]**bevatten.


##<a id="summary"></a>Samenvatting

Zoals u in dit document, kunt u een raw HTTP-aanvraag worden uitgevoerd, controleren en de resultaten van de component taken weergeven op het cluster HDInsight.

Zie voor meer informatie over de overige interface die wordt gebruikt in dit artikel, de <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">Verwijzing naar WebHCat</a>.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component met HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u met Tez component, raadpleegt u de volgende documenten voor foutopsporing informatie:

* [Gebruik de gebruikersinterface Tez op Windows gebaseerde HDInsight](hdinsight-debug-tez-ui.md)

* [Gebruik de weergave Ambari Tez op Linux-gebaseerde HDInsight](hdinsight-debug-ambari-tez-view.md)

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


