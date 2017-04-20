<properties
   pageTitle="Query met Hadoop extra component voor Visual Studio | Microsoft Azure"
   description="Informatie over het onderdeel gebruiken in combinatie met Hadoop in HDInsight met Hadoop met Visual Studio tools."
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Component uitvoeren met behulp van de HDInsight's voor Visual Studio

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe u query's component aan een HDInsight-cluster met behulp van de HDInsight's voor Visual Studio.

> [AZURE.NOTE] Dit document geeft een gedetailleerde beschrijving van het doen van de HiveQL-instructies die in de voorbeelden worden gebruikt. Zie voor informatie over de HiveQL die in dit voorbeeld wordt gebruikt, [Gebruik component met Hadoop op HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, moet u het volgende.

* Een cluster Azure HDInsight (Hadoop op HDInsight) (Linux of Windows-)

* Visual Studio (een van de volgende versies):

    Visual Studio 2013 Gemeenschap/Professional/Premium/Ultimate met [Update 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (communautaire/onderneming)

- HDInsight-hulpprogramma's voor Visual studio. Zie [aan de slag met Hadoop met Visual Studio tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) voor informatie over het installeren en configureren van de hulpprogramma's.

##<a id="run"></a>Component uitvoeren met behulp van de HDInsight's voor Visual Studio

1. **Visual Studio** openen en selecteer **Nieuw** > **Project** > **HDInsight** > **Component van toepassing**. Geef een naam voor dit project.

2. Open het bestand **Script.hql** dat wordt gemaakt met dit project en plak in de volgende instructies voor HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies worden de volgende acties uitvoeren:

    * **DROP TABLE**: Hiermee verwijdert u de tabel en het bestand als de tabel al bestaat.
    * **Externe tabel maken**: een nieuwe 'externe'-tabel gemaakt in een component. Externe tabellen opslaan alleen de tabeldefinitie in component (de gegevens links op de oorspronkelijke locatie).

        > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens worden bijgewerkt door een externe bron (zoals een geautomatiseerde gegevens uploadproces) of door een andere bewerking van MapReduce verwacht, maar u wilt dat altijd component query's gebruiken de meest recente gegevens.
        >
        > Neerzetten van een externe tabel heeft **niet** verwijderen de gegevens, alleen de tabeldefinitie.

    * **Rij-indeling**: vertelt component hoe de gegevens zijn ingedeeld. In dit geval worden de velden in elk logboek gescheiden door een spatie.
    * **Opgeslagen als TEXTFILE locatie**: vertelt component waarop de gegevens zijn opgeslagen (het voorbeeld/data directory) en wordt als tekst opgeslagen.
    * **Selecteren**: Selecteer een telling van alle rijen waarin de kolom **t4** waarde **[fout]**bevatten. Dit moet een waarde van **3** retourneren omdat er drie rijen die deze waarde bevatten.
    * **INPUT__FILE__NAME zoals '%.log'** - die we alleen gegevens van bestanden die eindigen ophalen moeten vertelt component. log. Hiermee beperkt u de zoekopdracht naar het sample.log bestand dat de gegevens bevat en blijft van het retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat is gedefinieerd.

3. **HDInsight Cluster** die u wilt gebruiken voor deze query selecteren op de werkbalk en selecteer vervolgens **indienen bij WebHCat** de instructies uitvoeren als een taak met een component met behulp van WebHCat. Ook kunt u de taak met behulp van de knop __uitvoeren via HiveServer2__ als HiveServer2 beschikbaar op uw versie van het cluster is indienen. De **Component samenvatting** weergegeven en wordt informatie weergegeven over de taak uitgevoerd. Gebruik de koppeling **vernieuwen** vernieuwen de taakgegevens totdat de **Taakstatus** gewijzigd in **voltooid**.

4. Met de koppeling van de **Uitvoer van de taak** kunt u de uitvoer van deze taak. Moet worden weergegeven `[ERROR] 3`, is de waarde die wordt geretourneerd door de SELECT-instructie.

5. U kunt ook een component query's uitvoeren zonder het maken van een project. **Azure** **Server Explorer**, vouw > **HDInsight**, met de rechtermuisknop op de server HDInsight en selecteer vervolgens **een query component**.

6. In het document **temp.hql** toevoegen met de volgende instructies voor HiveQL:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies worden de volgende acties uitvoeren:

    * **Maken van tabel als niet bestaat**: een tabel gemaakt als deze nog niet bestaat. Omdat de **externe** sleutelwoord niet gebruikt wordt, is dit een interne tabel die is opgeslagen in het gegevenswarehouse component en wordt volledig beheerd door component.

        > [AZURE.NOTE] In tegenstelling tot de **externe** tabellen verwijderd een interne tabel neer te zetten ook de onderliggende gegevens.

    * **Opgeslagen als ORC**: de gegevens zijn opgeslagen in geoptimaliseerde rij kolomindeling (ORC). Dit is een zeer geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens van de component.
    * OVERSCHRIJVEN INSERT **... Selecteer**: geselecteerd rijen uit de tabel **log4jLogs** **[fout]**, vervolgens de gegevens worden ingevoegd in de tabel **errorLogs** .

7. Selecteer in de werkbalk **indienen** bij het uitvoeren van de taak. De **Status van de taak** gebruiken om te bepalen dat de taak is voltooid.

8. **Server Explorer** gebruiken om te controleren dat de taak voltooid en een nieuwe tabel gemaakt, en vouw **Azure** > **HDInsight** > HDInsight cluster > **Databases component** > en de **standaard**. U ziet de **errorLogs** -tabel en de tabel **log4jLogs** .

##<a id="summary"></a>Samenvatting

Zoals u ziet, de de HDInsight's voor Visual Studio bieden een eenvoudige manier naar component query's uitvoeren op een cluster van HDInsight, de taakstatus controleren en ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor meer informatie over de HDInsight's voor Visual Studio:

* [Aan de slag met de HDInsight's voor Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
