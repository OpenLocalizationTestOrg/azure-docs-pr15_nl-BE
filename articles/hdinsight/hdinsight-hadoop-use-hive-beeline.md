<properties
   pageTitle="Beeline gebruiken om te werken met het onderdeel op HDInsight (Hadoop) | Microsoft Azure"
   description="Leren met SSH kunt u verbinding maken met een cluster Hadoop in HDInsight en component query's interactief met behulp van Beeline te verzenden. Beeline is een hulpprogramma voor het werken met HiveServer2 via JDBC."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Component gebruiken in combinatie met Hadoop in HDInsight met Beeline

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe u met behulp van Secure Shell (SSH) verbinding maken met een cluster van Linux-gebaseerde HDInsight en interactief verzend component query's met behulp van het opdrachtregelprogramma [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) .

> [AZURE.NOTE] Beeline maakt JDBC verbinding met component. Zie [verbinding maken met de component in Azure HDInsight met het stuurprogramma voor JDBC component](hdinsight-connect-hive-jdbc-driver.md)voor meer informatie over het gebruik van JDBC met component.

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Linux-gebaseerde Hadoop op HDInsight cluster.

* SSH-client. Linux, Unix en Mac OS moeten worden geleverd met een SSH-client. Windows-gebruikers moeten downloaden van een client, zoals [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met de FQDN-naam (Fully Qualified Domain Name) van het cluster HDInsight met behulp van de opdracht SSH. De FQDN-naam is de naam die u aan het cluster, vervolgens gegeven **. azurehdinsight.net**. Bijvoorbeeld de volgende verbinding te maken met een cluster met de naam **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Als u een certificaatsleutel voor SSH-verificatie hebt opgegeven** bij het maken van het cluster HDInsight, wellicht moet u de locatie opgeven van de persoonlijke sleutel op de client-computer:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Als u een wachtwoord voor de SSH-verificatie hebt opgegeven** bij het maken van de HDInsight-cluster, moet u het wachtwoord wanneer daarom wordt gevraagd op te geven.

Zie voor meer informatie over het gebruik van SSH in HDInsight, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (Windows-gebaseerde clients)

Windows beschikt niet over een ingebouwde SSH-client. Beste **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Gebruik de opdracht Beeline

1. Zodra verbonden, gebruikt u de volgende Beeline starten:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Dit zal de Beeline-client starten en verbinding maken met de JDBC-url. Hier `localhost` wordt gebruikt, aangezien de HiveServer2 op beide head knooppunten in het cluster wordt uitgevoerd, en onze Beeline rechtstreeks op de primaire headnode.
    
    Nadat de opdracht is voltooid, ontvangt u bij een `jdbc:hive2://localhost:10001/>` vragen.

3. Beeline opdrachten begint meestal met een `!` teken, bijvoorbeeld `!help` wordt help weergegeven. Maar de `!` kan vaak worden weggelaten. Bijvoorbeeld, `help` , werkt ook.

    Als u help weergeeft, ziet u `!sql`, dat wordt gebruikt voor HiveQL-instructies uitvoeren. HiveQL wordt echter zo vaak gebruikt u kunt weglaten de voorafgaande `!sql`. De volgende twee instructies hebben precies dezelfde resultaten; de tabellen die momenteel beschikbaar zijn via de component weer te geven:
    
        !sql show tables;
        show tables;
    
    Op een nieuw cluster slechts één tabel moet worden weergegeven: __hivesampletable__.

4. De volgende gebruiken om het schema voor de hivesampletable weer te geven:

        describe hivesampletable;
        
    Hiermee herstelt u de volgende informatie:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Hiermee geeft de kolommen in de tabel. Terwijl we sommige query's op deze gegevens uitvoeren kan, gaan we in plaats daarvan maakt een nieuwe tabel laat zien hoe gegevens in de component laden en toepassen van een schema.
    
5. Voer de volgende instructies om een nieuwe tabel met voorbeeldgegevens die voorzien zijn van het cluster HDInsight de naam **log4jLogs** maken:

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
    * **INPUT__FILE__NAME zoals '%.log'** - die we alleen gegevens van bestanden die eindigen ophalen moeten vertelt component. log. Normaal gesproken zou hoeft u slechts gegevens met hetzelfde schema in dezelfde map als opvragen met de component, maar dit voorbeeldbestand is opgeslagen met andere gegevensindelingen.

    > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u verwacht de onderliggende gegevens worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens uploadproces of door een andere MapReduce bewerking, waarbij altijd component query's dat gebruiken de meest recente gegevens.
    >
    > Neerzetten van een externe tabel heeft **niet** verwijderen de gegevens, alleen de tabeldefinitie.
    
    De uitvoer van deze opdracht moet er ongeveer als volgt:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Gebruik Beeline om af te sluiten `!quit`.

##<a id="file"></a>Een HiveQL-bestand uitvoeren

Beeline kan ook worden gebruikt voor het uitvoeren van een bestand dat de instructies HiveQL bevat. Gebruik de volgende stappen voor het maken van een bestand en voer vervolgens met behulp van Beeline.

1. Gebruik de volgende opdracht maakt u een nieuw bestand met de naam __query.hql__:

        nano query.hql
        
2. Zodra de editor wordt geopend, gebruikt u de volgende als de inhoud van het bestand. Deze query maakt een nieuwe "interne" tabel met de naam **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies worden de volgende acties uitvoeren:

    * **Maken van tabel als niet bestaat** - maakt een tabel, als deze nog niet bestaat. Aangezien de **externe** sleutelwoord niet gebruikt wordt, is dit een interne tabel die is opgeslagen in het gegevenswarehouse component en wordt volledig beheerd door component.
    * **Opgeslagen als ORC** - worden de gegevens opgeslagen in de indeling voor geoptimaliseerde rij in kolomvorm (ORC). Dit is een zeer geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens van de component.
    * OVERSCHRIJVEN INSERT **... Selecteer** - rijen uit de tabel **log4jLogs** bevat **[fout]**selecteert en vervolgens de gegevens in de tabel **errorLogs** ingevoegd.
    
    > [AZURE.NOTE] In tegenstelling tot de externe tabellen wordt neerzetten van een interne tabel verwijderd en de onderliggende gegevens.
    
3. Om het bestand, __Ctrl__+__X___ gebruiken, voert u __Y__en ten slotte op __Enter__.

4. Gebruik de volgende uit te voeren van het bestand met Beeline. Vervang __hostnaam__ door de naam eerder verkregen voor de hoofd-knooppunt en het __wachtwoord__ door het wachtwoord voor de admin-account:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] De `-i` parameter Beeline wordt gestart, voert u de instructies in het bestand query.hql en blijft in de Beeline op de `jdbc:hive2://localhost:10001/>` vragen. U kunt ook uitvoeren voor een bestand met de `-f` parameter, die u terug naar Bash nadat het bestand is verwerkt.

5. Als u wilt controleren of de **errorLogs** -tabel is gemaakt, moet u de volgende instructie gebruiken om alle rijen uit de **errorLogs**:

        SELECT * from errorLogs;

    Drie rijen met gegevens moeten worden geretourneerd, alle met **[fout]** in de kolom t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Meer informatie over Beeline connectiviteit

De stappen in dit document gebruikt `localhost` met HiveServer2 op de headnode van het cluster. Terwijl u de host-naam kunt of de FQDN-naam van de headnode die moeten extra stappen om het proces (stappen om te zoeken naar de hostnaam of FQDN). Met behulp van `localhost` is voldoende als u Beeline uit de headnode.

Als er een randknooppunt in uw cluster, met Beeline is geïnstalleerd, moet u de hostnaam of FQDN van de headnode gebruiken om verbinding te maken.

Als u geïnstalleerd op een client buiten het cluster Beeline hebt, kunt u de volgende opdracht. __CLUSTERNAAM__ vervangen door de naam van het cluster HDInsight. Vervang __wachtwoord__ door het wachtwoord voor de beheerdersaccount (http-aanmelding).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Houd er rekening mee dat de parameters/URI anders is dan bij het rechtstreeks op een headnode of van een randknooppunt binnen het cluster uitgevoerd. Dit is omdat een openbare-gateway gerouteerd verkeer via poort 443 via internet verbinding maakt met het cluster worden gebruikt. Ook worden diverse andere services via de openbare gateway op poort 443, zodat de URI anders is dan bij een directe verbinding. Wanneer u verbinding maakt via het internet moet u de sessie ook verifiëren door middel van het wachtwoord.

##<a id="summary"></a><a id="nextsteps"></a>Volgende stappen

Zoals u zien kunt, biedt de opdracht Beeline een eenvoudige manier interactief component query's uitvoeren op een cluster van HDInsight.

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

