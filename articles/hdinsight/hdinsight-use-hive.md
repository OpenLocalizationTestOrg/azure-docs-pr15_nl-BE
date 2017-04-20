<properties
    pageTitle="Informatie over component en het gebruik van HiveQL | Microsoft Azure"
    description="Informatie over Apache component en het gebruik in combinatie met Hadoop in HDInsight. Het uitvoeren van de taak van de component en HiveQL gebruikt voor het analyseren van een voorbeeldbestand van Apache log4j kiezen."
    keywords="hiveql, wat een onderdeel is"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Component- en HiveQL met Hadoop in HDInsight gebruiken voor het analyseren van een voorbeeldbestand van Apache log4j

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


In deze zelfstudie u informatie over het gebruik van Apache component in Hadoop op HDInsight, en geef op hoe u uw project component uit te voeren. Ook leert u over HiveQL en het analyseren van een voorbeeldbestand van Apache log4j.

##<a id="why"></a>Wat is component en waarom gebruiken?
[Apache-component](http://hive.apache.org/) is een systeem voor magazijn voor Hadoop waarmee samenvatting gegevens opvragen en analyseren van gegevens met behulp van HiveQL (een querytaal zoals SQL). Component kan worden gebruikt om de gegevens interactief te verkennen of herbruikbare batch taken verwerkt.

Component kunt u de projectstructuur van het op grotendeels ongestructureerde gegevens. Nadat u de structuur hebt gedefinieerd, kunt u component opvragen zonder kennis van Java of MapReduce. **HiveQL** (Component query language) kunt u query's met instructies die vergelijkbaar met T-SQL zijn te schrijven.

Component begrijpt hoe u werkt met gestructureerde en semi-gestructureerde gegevens, zoals tekstbestanden waarin de velden worden gescheiden door specifieke tekens. Component ondersteunt ook aangepaste **serializer/deserializers (SerDe)** voor complexe of onregelmatig gestructureerde gegevens. Zie voor meer informatie [hoe u een aangepaste JSON-SerDe met HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Door de gebruiker gedefinieerde functies (UDF)

Component kan ook worden uitgebreid door de **gebruiker gedefinieerde functies (UDF)**. Een UDF kunt u functionaliteit of logica die eenvoudig is niet gemodelleerd implementeren in HiveQL. Zie de volgende onderwerpen voor een voorbeeld van het gebruik van UDF's met de component:

* [Gebruik een Java-gebruiker gedefinieerde functie met component](hdinsight-hadoop-hive-java-udf.md)

* [Met behulp van Python en component en varken in HDInsight](hdinsight-python.md)

* [Gebruik C# met component en varken in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Een aangepaste component UDF toevoegen aan HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Voorbeeld van de aangepaste component UDF datum/tijd-notaties converteren naar component tijdstempel](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Interne tabellen versus externe tabellen component

Er zijn een paar dingen die u moet weten over de interne component-tabel en de externe tabel:

- De opdracht **CREATE TABLE** maakt een interne tabel. Het gegevensbestand moet zich bevinden in de standaardcontainer.
- De opdracht **CREATE TABLE** wordt het bestand verplaatst naar de /hive/magazijn/<TableName> map.
- De opdracht **Externe tabel maken** maakt u een externe tabel. Het bestand kan zich bevinden buiten de standaardcontainer.
- De opdracht **Externe tabel maken** verplaatst het bestand niet.
- De opdracht **Externe tabel maken** , kunnen geen mappen op de locatie. Dit is de reden waarom de zelfstudie een kopie van het bestand sample.log wordt.

Zie voor meer informatie [HDInsight: component interne en externe tabellen Intro][cindygross-hive-tables].


##<a id="data"></a>Over de voorbeeldgegevens, een bestand van Apache log4j

In dit voorbeeld wordt een voorbeeldbestand *log4j* , dat is opgeslagen op de **/example/data/sample.log** in de blob-opslag container. Elk logboek in het bestand bestaat uit een reeks velden met een `[LOG LEVEL]` om weer te geven van het type en de ernst, bijvoorbeeld:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

In het vorige voorbeeld is het logboekniveau fout.

> [AZURE.NOTE] U kunt ook een log4j-bestand genereren met het hulpprogramma [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) vastleggen en vervolgens dat bestand te uploaden naar de container blob. Zie [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor meer informatie. Zie voor meer informatie over het gebruik van Azure Blob-opslag met HDInsight [Gebruik Azure Blob-opslag met HDInsight](hdinsight-hadoop-use-blob-storage.md).

De voorbeeldgegevens is in Azure Blob-opslag HDInsight wordt gebruikt als het standaardbestandssysteem opgeslagen. HDInsight toegang tot bestanden die zijn opgeslagen in de BLOB's met het voorvoegsel **wasb** . Bijvoorbeeld, als u het bestand sample.log, gebruikt u de volgende syntaxis:

    wasbs:///example/data/sample.log

Omdat Azure Blob-opslag de standaard opslaglocatie voor HDInsight is, kunt u het bestand ook openen met behulp van **/example/data/sample.log** van HiveQL.

> [AZURE.NOTE] De syntaxis van de **wasbs: / / /**, wordt gebruikt voor toegang tot bestanden die zijn opgeslagen in de opslag voor uw cluster HDInsight. Als u extra opslagruimte accounts hebt opgegeven bij het inrichten van het cluster en u toegang wilt tot bestanden die zijn opgeslagen in deze accounts, opent u de gegevens door te geven van de container en de opslag accountadres, bijvoorbeeld **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Voorbeeld project: Project kolommen op gescheiden gegevens

De volgende HiveQL-instructies worden kolommen project naar gescheiden gegevens die zijn opgeslagen in de **wasbs: / / / / voorbeeldgegevens** directory:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

In het vorige voorbeeld wordt uitvoeren met de instructies van de HiveQL de volgende acties:

* __hive.execution.engine=tez; instellen__: stelt de execution engine Tez gebruikt. Tez gebruiken in plaats van MapReduce krijgt een stijging van de prestaties van query's. Zie voor meer informatie over Tez, het gedeelte [Met Apache Tez voor verbeterde prestaties](#usetez) .

    > [AZURE.NOTE] Deze verklaring is alleen vereist bij het gebruik van een cluster van Windows-gebaseerde HDInsight. Tez is de standaard uitvoering-engine voor Linux-gebaseerde HDInsight.

* **DROP TABLE**: Hiermee verwijdert u de tabel en het bestand als de tabel al bestaat.
* **Externe tabel maken**: Hiermee maakt u een nieuwe **externe** tabel in component. Externe tabellen alleen opslaan de tabeldefinitie in component; de gegevens in de oorspronkelijke locatie en in de oorspronkelijke indeling blijft.
* **Rij-indeling**: vertelt component hoe de gegevens zijn ingedeeld. In dit geval worden de velden in elk logboek gescheiden door een spatie.
* **Opgeslagen als TEXTFILE locatie**: vertelt component waarop de gegevens zijn opgeslagen (het voorbeeld/data directory) en wordt als tekst opgeslagen. De gegevens kunnen worden in één bestand of verspreid over meerdere bestanden in die map.
* **Selecteren**: Hiermee selecteert u een telling van alle rijen waarin de kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren omdat er drie rijen die deze waarde bevatten.
* **INPUT__FILE__NAME zoals '%.log'** - die we alleen gegevens van bestanden die eindigen ophalen moeten vertelt component. log. Hiermee beperkt u de zoekopdracht naar het sample.log bestand dat de gegevens bevat en blijft van het retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat is gedefinieerd.

> [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u verwacht dat de onderliggende gegevens worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens upload-proces of een andere bewerking van MapReduce en u wilt dat altijd component query's gebruiken de meest recente gegevens.
>
> Neerzetten van een externe tabel heeft **niet** de gegevens verwijderd, worden alleen de tabeldefinitie.

Na het maken van de externe tabel, worden de volgende instructies voor het maken van een **interne** tabel gebruikt.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Deze instructies worden de volgende acties uitvoeren:

* **Maken van tabel als niet bestaat**: een tabel, wordt gemaakt als deze nog niet bestaat. Omdat de **externe** sleutelwoord niet gebruikt wordt, is dit een interne tabel die is opgeslagen in het gegevenswarehouse component en wordt volledig beheerd door component.
* **Opgeslagen als ORC**: de gegevens zijn opgeslagen in de indeling voor geoptimaliseerde rij in kolomvorm (ORC). Dit is een zeer geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens van de component.
* OVERSCHRIJVEN INSERT **... Selecteer**: Hiermee selecteert u rijen uit de tabel **log4jLogs** bevat **[fout]**en voegt vervolgens de gegevens in de tabel **errorLogs** .

> [AZURE.NOTE] In tegenstelling tot de externe tabellen verwijderd een interne tabel neer te zetten ook de onderliggende gegevens.

##<a id="usetez"></a>Apache Tez gebruiken voor betere prestaties

[Apache Tez](http://tez.apache.org) is een raamwerk waarmee gegevensintensieve toepassingen, zoals component veel efficiënter uitvoeren op schaal. In de meest recente versie van de HDInsight component ondersteunt op Tez uitgevoerd. Tez is standaard ingeschakeld voor HDInsight op basis van Linux-clusters.

> [AZURE.NOTE] Tez is momenteel standaard uitgeschakeld voor Windows-gebaseerde HDInsight clusters en moet zijn ingeschakeld. Als u wilt profiteren van Tez, moet de volgende waarde worden ingesteld voor een query component:
>
> ```set hive.execution.engine=tez;```
>
>Dit kan op basis van per query worden ingediend door het te plaatsen aan het begin van de query. U kunt ook instellen als worden standaard op een cluster door de configuratiewaarde wanneer u het cluster maakt. Meer informatie kunt u vinden in [HDInsight Clusters inrichten](hdinsight-provision-clusters.md).

De [component Tez ontwerp documenten](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) bevatten een aantal details over de implementatie-opties en configuraties afstemmen.

Om u te helpen bij het opsporen van fouten in taken hebt uitgevoerd met behulp van Tez, HDInsight biedt de volgende web-UI waarmee u details van Tez taken bekijken:

* [Gebruik de gebruikersinterface Tez op Windows gebaseerde HDInsight](hdinsight-debug-tez-ui.md)

* [Gebruik de weergave Ambari Tez op Linux-gebaseerde HDInsight](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Kies het uitvoeren van het project HiveQL

HDInsight HiveQL taken met behulp van verschillende methoden kan worden uitgevoerd. In de volgende tabel om te beslissen welke methode u gebruikt en volg de link voor een overzicht.

| **Gebruik deze optie** als u wilt...                                                     | .. .an **interactieve** shell | ... **batch** -verwerking | .. .door dit **cluster besturingssysteem** | .. .from dit **besturingssysteem van client** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Component weergeven](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Alle (browser gebaseerd) |
| [Beeline-opdracht (van een SSH-sessie)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [De opdracht component (van een SSH-sessie)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [Krul](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux of Windows                          | Linux, Unix, Mac OS X of Windows        |
| [Query-console](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Alle (browser gebaseerd)                            |
| [Extra HDInsight voor Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |
| [Extern bureaublad](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Component taken op Azure HDInsight uitgevoerd op ruimten SQL Server Integration Services gebruiken

U kunt SQL Server Integration Services (SSIS) ook gebruiken voor het uitvoeren van een taak van de component. De Azure Feature Pack voor SSIS omvat de volgende onderdelen die met component taken op HDInsight werken.


- [Taak Azure HDInsight component][hivetask]
- [Azure abonnement Connection Manager][connectionmanager]


Meer weten over de Azure Feature Pack voor SSIS [hier][ssispack].


##<a id="nextsteps"></a>Volgende stappen

U hebt geleerd wat component is en hoe het met Hadoop in HDInsight, via de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.


- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [Gebruik Sqoop met HDInsight](hdinsight-use-sqoop.md)
- [Gebruik Oozie met HDInsight](hdinsight-use-oozie.md)
- [MapReduce taken gebruiken met HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
