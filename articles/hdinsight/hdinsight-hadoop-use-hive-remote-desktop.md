<properties
   pageTitle="Hadoop component en extern bureaublad gebruiken in HDInsight | Microsoft Azure"
   description="Informatie over het cluster Hadoop in HDInsight verbinding met extern bureaublad en vervolgens component query's uitvoeren met behulp van de opdrachtregelinterface component."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Component met Hadoop op HDInsight met extern bureaublad gebruiken

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel, u leert hoe u via Extern bureaublad verbinding maken met een cluster HDInsight, en vervolgens component query's uitvoeren met behulp van de component opdrachtregelinterface CLI ().

> [AZURE.NOTE] Dit document geeft een gedetailleerde beschrijving van het doen van de overzichten van de HiveQL die in de voorbeelden worden gebruikt. Zie voor informatie over de HiveQL die in dit voorbeeld wordt gebruikt, [Gebruik component met Hadoop op HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

* Een cluster van Windows-gebaseerde HDInsight (Hadoop op HDInsight)

* Een clientcomputer met Windows 10, 8 venster of Windows 7

##<a id="connect"></a>Verbinding met extern bureaublad

Extern bureaublad inschakelen voor het cluster HDInsight en vervolgens verbinding maken met het door de instructies op [verbinding maken met clusters van HDInsight met RDP](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hive"></a>Gebruik de opdracht component

Wanneer u verbinding hebt met het bureaublad voor het cluster HDInsight, gebruik de volgende stappen uit om te werken met component:

1. Starten vanaf het bureaublad HDInsight de **Hadoop-opdrachtregel**.

2. Voer de volgende opdracht om de component CLI te starten:

        %hive_home%\bin\hive

    Wanneer de CLI is gestart, ziet u de prompt CLI component: `hive>`.

3. Met behulp van de CLI, voert u de volgende instructies om een nieuwe tabel met de naam **log4jLogs** met voorbeeldgegevens maken:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies worden de volgende acties uitvoeren:

    * **DROP TABLE**: Hiermee verwijdert u de tabel en het bestand als de tabel al bestaat.

    * **Externe tabel maken**: een nieuwe 'externe'-tabel gemaakt in een component. Alleen de tabeldefinitie opslaan externe tabellen in component (de gegevens links op de oorspronkelijke locatie).

        > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens worden bijgewerkt door een externe bron (zoals een geautomatiseerde gegevens uploadproces) of door een andere bewerking van MapReduce verwacht, maar u wilt dat altijd component query's gebruiken de meest recente gegevens.
        >
        > Neerzetten van een externe tabel heeft **niet** verwijderen de gegevens, alleen de tabeldefinitie.

    * **Rij-indeling**: vertelt component hoe de gegevens zijn ingedeeld. In dit geval worden de velden in elk logboek gescheiden door een spatie.

    * **Opgeslagen als TEXTFILE locatie**: vertelt component waarop de gegevens zijn opgeslagen (het voorbeeld/data directory) en wordt als tekst opgeslagen.

    * **Selecteren**: Hiermee selecteert u een telling van alle rijen waarin kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren omdat er drie rijen die deze waarde bevatten.

    * **INPUT__FILE__NAME zoals '%.log'** - die we alleen gegevens van bestanden die eindigen ophalen moeten vertelt component. log. Hiermee beperkt u de zoekopdracht naar het sample.log bestand dat de gegevens bevat en blijft van het retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat is gedefinieerd.


4. Gebruik de volgende instructies voor het maken van een nieuwe "interne" tabel met de naam **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies worden de volgende acties uitvoeren:

    * **Maken van tabel als niet bestaat**: een tabel gemaakt als deze nog niet bestaat. Omdat de **externe** sleutelwoord niet gebruikt wordt, is dit een interne tabel die is opgeslagen in het gegevenswarehouse component en wordt volledig beheerd door component.

        > [AZURE.NOTE] In tegenstelling tot de **externe** tabellen verwijderd een interne tabel neer te zetten ook de onderliggende gegevens.

    * **Opgeslagen als ORC**: de gegevens zijn opgeslagen in geoptimaliseerde rij kolomindeling (ORC). Dit is een zeer geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens van de component.

    * OVERSCHRIJVEN INSERT **... Selecteer**: geselecteerd rijen uit de tabel **log4jLogs** **[fout]**, vervolgens de gegevens worden ingevoegd in de tabel **errorLogs** .

    Om te controleren dat alleen rijen met **[fout]** in de kolom t4 zijn opgeslagen in de tabel **errorLogs** , moet u de volgende instructie gebruiken om alle rijen uit de **errorLogs**:

        SELECT * from errorLogs;

    Drie rijen met gegevens moet worden geretourneerd, alle met **[fout]** in de kolom t4.

##<a id="summary"></a>Samenvatting

Zoals u ziet het de opdracht component biedt een eenvoudige manier interactief component query's uitvoeren op een cluster van HDInsight, de taakstatus controleren en ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u met Tez component, raadpleegt u de volgende documenten voor foutopsporing informatie:

* [Gebruik de gebruikersinterface Tez op Windows gebaseerde HDInsight](hdinsight-debug-tez-ui.md)

* [Gebruik de weergave Ambari Tez op Linux-gebaseerde HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

