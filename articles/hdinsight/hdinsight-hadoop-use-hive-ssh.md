<properties
   pageTitle="Gebruik de component-shell in HDInsight (Hadoop) | Microsoft Azure"
   description="Informatie over het gebruik van de shell component met een HDInsight op basis van Linux-cluster. U wordt informatie over het verbinden met het HDInsight-cluster via SSh en vervolgens gebruikt u de component Shell interactief uitvoeren van query's."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Component gebruiken in combinatie met Hadoop in HDInsight met SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe u met de Secure Shell (SSH) verbinding maken met een Hadoop op Azure HDInsight cluster en component query's interactief met behulp van de component opdrachtregelinterface (CLI) te verzenden.

> [AZURE.IMPORTANT] Terwijl de opdracht component op Linux-gebaseerde HDInsight clusters beschikbaar is, moet u overwegen Beeline. Beeline is een nieuwere client voor het werken met component en is opgenomen in het cluster HDInsight. Zie [Gebruik component met Hadoop in HDInsight met Beeline](hdinsight-hadoop-use-hive-beeline.md)voor meer informatie over het gebruik van dit.

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Linux-gebaseerde Hadoop op HDInsight cluster.

* SSH-client. Linux, Unix en Mac OS moeten worden geleverd met een SSH-client. Windows-gebruikers moeten downloaden van een client, zoals [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met de FQDN-naam (Fully Qualified Domain Name) van het cluster HDInsight met behulp van de opdracht SSH. De FQDN-naam is de naam die u aan het cluster, vervolgens gegeven **. azurehdinsight.net**. Bijvoorbeeld de volgende verbinding te maken met een cluster met de naam **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Als u een certificaatsleutel voor SSH-verificatie hebt opgegeven** bij het maken van het cluster HDInsight, wellicht moet u de locatie opgeven van de persoonlijke sleutel op de client-computer:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Als u een wachtwoord voor de SSH-verificatie hebt opgegeven** bij het maken van de HDInsight-cluster, moet u het wachtwoord wanneer daarom wordt gevraagd op te geven.

Zie voor meer informatie over het gebruik van SSH in HDInsight, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (Windows-gebaseerde clients)

Windows beschikt niet over een ingebouwde SSH-client. Beste **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Gebruik de opdracht component

2. Wanneer een verbinding, start u de CLI component met de volgende opdracht:

        hive

3. Met behulp van de CLI, voert u de volgende instructies om een nieuwe tabel met de naam **log4jLogs** met behulp van de voorbeeldgegevens maken:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies worden de volgende acties uitvoeren:

    * **DROP TABLE** - Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.
    * **Externe tabel maken** - maakt een nieuwe 'externe' tabel in een component. De tabeldefinitie opslaan externe tabellen alleen in de component. De gegevens blijft op de oorspronkelijke locatie.
    * **Rij-indeling** : vertelt component hoe de gegevens zijn ingedeeld. In dit geval worden de velden in elk logboek gescheiden door een spatie.
    * **Locatie van opgeslagen als TEXTFILE** - vertelt component waarop de gegevens zijn opgeslagen (de map voorbeeld/gegevens) en dat deze wordt opgeslagen als tekst.
    * **Selecteer** - Hiermee selecteert u een telling van alle rijen waarin kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren als er drie rijen die deze waarde bevatten.
    * **INPUT__FILE__NAME zoals '%.log'** - die we alleen gegevens van bestanden die eindigen ophalen moeten vertelt component. log. Hiermee beperkt u de zoekopdracht naar het sample.log bestand dat de gegevens bevat en blijft van het retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat is gedefinieerd.

    > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u verwacht de onderliggende gegevens worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens uploadproces of door een andere MapReduce bewerking, waarbij altijd component query's dat gebruiken de meest recente gegevens.
    >
    > Neerzetten van een externe tabel heeft **niet** verwijderen de gegevens, alleen de tabeldefinitie.

4. Gebruik de volgende instructies voor het maken van een nieuwe "interne" tabel met de naam **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies worden de volgende acties uitvoeren:

    * **Maken van tabel als niet bestaat** - maakt een tabel, als deze nog niet bestaat. Aangezien de **externe** sleutelwoord niet gebruikt wordt, is dit een interne tabel die is opgeslagen in het gegevenswarehouse component en wordt volledig beheerd door component.
    * **Opgeslagen als ORC** - worden de gegevens opgeslagen in de indeling voor geoptimaliseerde rij in kolomvorm (ORC). Dit is een zeer geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens van de component.
    * OVERSCHRIJVEN INSERT **... Selecteer** - rijen uit de tabel **log4jLogs** bevat **[fout]**selecteert en vervolgens de gegevens in de tabel **errorLogs** ingevoegd.

    Om te controleren dat alleen de rijen met **[fout]** in de kolom t4 naar de tabel **errorLogs** zijn opgeslagen, moet u de volgende instructie gebruiken om alle rijen uit de **errorLogs**:

        SELECT * from errorLogs;

    Drie rijen met gegevens moet worden geretourneerd, alle met **[fout]** in de kolom t4.

    > [AZURE.NOTE] In tegenstelling tot de externe tabellen wordt neerzetten van een interne tabel verwijderd en de onderliggende gegevens.

##<a id="summary"></a>Samenvatting

U kunt zien, zorgt de opdracht component voor een eenvoudige manier interactief component query's uitvoeren op een cluster van HDInsight, de taakstatus controleren en ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

