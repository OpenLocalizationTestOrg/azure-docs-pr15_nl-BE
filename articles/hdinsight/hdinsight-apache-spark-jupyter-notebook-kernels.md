<properties 
    pageTitle="Verkrijgbaar met Jupyter notitieblokken op HDInsight Spark kernels clusters op Linux | Microsoft Azure" 
    description="Meer informatie over het beschikbaar met vonkontsteking cluster op HDInsight Linux extra Jupyter laptop kernels." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Beschikbaar voor Jupyter-laptops met clusters van Apache Spark op HDInsight Linux kernels

Apache Spark cluster op HDInsight (Linux) bevat Jupyter-laptops die u gebruiken kunt voor het testen van uw toepassingen. Een kernel is een programma dat wordt uitgevoerd en uw code wordt geïnterpreteerd. HDInsight Spark clusters bieden twee kernels die u met de Jupyter-laptop gebruiken kunt. Dit zijn:

1. **PySpark** (voor toepassingen die zijn geschreven in Python)
2. **Spark** (voor toepassingen die zijn geschreven in Scala)

In dit artikel leert u over het gebruik van deze kernels en wat zijn de voordelen van het gebruik ervan.

**Voorwaarden:**

U hebt de volgende opties:

- Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster van Apache Spark op HDInsight Linux. Zie [clusters in Azure HDInsight Apache Spark maken](hdinsight-apache-spark-jupyter-spark-sql.md)voor instructies.

## <a name="how-do-i-use-the-kernels"></a>Hoe gebruik ik de kernels 

1. De [Azure Portal](https://portal.azure.com/)van de startboard, klik op de tegel voor uw cluster Spark (als u deze aan de startboard vastgemaakt). U kunt ook navigeren naar het cluster onder **Alle bladeren** > **HDInsight Clusters**.   

2. Uit het cluster Spark blade, klikt u op **Cluster Dashboard**en klik op **Jupyter laptop**. Geef desgevraagd de referenties van de beheerder voor het cluster.

    > [AZURE.NOTE] U kan ook de laptop Jupyter voor uw cluster bereiken via de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van het cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken met de nieuwe kernels. Klik op **Nieuw**en klik vervolgens op **Pyspark** of **vonk**. Moet u de kernel van de vonk voor Scala toepassingen en kernel-PySpark voor Python applications.

    ![Een nieuwe laptop voor Jupyter maken] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Een nieuwe laptop voor Jupyter maken") 

3. Dit moet een nieuw notitieblok openen met de kernel die u hebt geselecteerd.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Waarom gebruikt u de PySpark of Spark kernels?

Hier zijn enkele voordelen van het gebruik van de nieuwe kernels.

1. **Vooraf ingestelde contexten**. Met de **PySpark** of **Spark** kernels Jupyter-laptops zijn voorzien, hoeft niet de vonk of component contexten expliciet instellen voordat u kunt werken met de toepassing ontwikkelt; Dit zijn standaard beschikbaar voor u. Deze contexten zijn:

    * **sc** - voor Spark context
    * **sqlContext** - voor de context van de component


    U moet dus geen instructies in de volgende contexten instellen uitvoeren:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    U kunt de vooraf ingestelde contexten in plaats daarvan rechtstreeks in uw toepassing gebruiken.
    
2. **Cel magics**. De PySpark kernel biedt bepaalde voorgedefinieerde "magics", die speciale opdrachten die u met bellen kunt zijn `%%` (bv. `%%MAGIC` <args>). De magische opdracht moet het eerste woord in een cel code en kunnen voor meerdere regels van inhoud. Het magische woord moet het eerste woord in de cel. Toevoegen van vóór de magie, zelfs opmerkingen, treedt er een fout.   Zie voor meer informatie over magics [hier](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    De volgende tabel worden de verschillende magics beschikbaar via de kernels.

  	| Magic     | Voorbeeld                         | Beschrijving  |
  	|-----------|---------------------------------|--------------|
  	| Help      | `%%help`                            | Genereert een lijst met alle beschikbare magics met voorbeeld en beschrijving |
  	| Info      | `%%info`                          | Sessie-informatie voor het huidige Livius eindpunt uitgangen |
  	| configureren | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Hiermee configureert u de parameters voor het maken van een sessie. De vlag (-f) is verplicht als u een sessie is gemaakt en wordt de sessie worden verwijderd en opnieuw gemaakt. Bekijk [hier de advertentie /sessions instantie verzoeken](https://github.com/cloudera/livy#request-body) voor een lijst met geldige parameters. Parameters moeten worden doorgegeven als een JSON-string en moeten op de volgende regel na de magic, zoals weergegeven in de voorbeeldkolom. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Hiermee voert u een query component op de sqlContext. Als de `-o` parameter wordt doorgegeven, het resultaat van de query worden behouden in de %% lokale Python context als een dataframe [Pandas](http://pandas.pydata.org/) .   |
  	| lokale     |     `%%local`<br>`a=1`              | De code in de volgende regels wordt lokaal uitgevoerd. Code moet geldige Python code. |
  	| Logboeken      | `%%logs`                        | Hiermee kunt u de logboeken voor de huidige sessie van Livius.  |
  	| verwijderen    | `%%delete -f -s <session number>` | Hiermee verwijdert u een bepaalde sessie van de huidige Livius eindpunt. Houd er rekening mee dat u de sessie wordt gestart voor de kernel zelf niet verwijderen. |
  	| opruimen   | `%%cleanup -f`                    | Hiermee verwijdert u alle sessies voor het huidige eindpunt hier, met inbegrip van de sessie van deze laptop. De werking van de vlag -f is verplicht.  |

    >[AZURE.NOTE] Naast de magics toegevoegd door de kernel PySpark, ook kunt u de [ingebouwde IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), met inbegrip van `%%sh`. U kunt de `%%sh` magic blok code en scripts uitvoeren op de headnode van het cluster. 

3. **Visualisatie van de auto**. De kernel **Pyspark** visualiseert automatisch de uitvoer van de component en SQL-query's. U hebt de optie om te kiezen tussen verschillende soorten visualisaties, met inbegrip van de tabel, cirkel-, lijn, gebied, balk.

## <a name="parameters-supported-with-the-sql-magic"></a>Parameters die worden ondersteund met de %% sql magic

De %% magic sql ondersteunt verschillende parameters die u gebruiken kunt om te bepalen welk type uitvoer dat wordt weergegeven wanneer u query's uitvoeren. De volgende tabel bevat de uitvoer.

| Parameter     | Voorbeeld                         | Beschrijving  |
|-----------|---------------------------------|--------------|
| -o      | `-o <VARIABLE NAME>`                          | Gebruik deze parameter om het resultaat van de query, in de %% lokale Python context, als een dataframe [Pandas](http://pandas.pydata.org/) . De naam van de variabele dataframe is de naam van de variabele die u opgeeft. |
| -q      | `-q`                          | Gebruik deze optie uitschakelen visualisaties voor de cel. Als u niet wilt dat de inhoud van een cel automatisch visualiseren en wil een vastlegt als een dataframe en gebruik vervolgens `-q -o <VARIABLE>`. Als u visualisaties uitschakelen wilt zonder het vastleggen van de resultaten (bv. voor het uitvoeren van een SQL-query met neveneffecten, zoals een `CREATE TABLE` instructie), gebruikt u `-q` zonder een `-o` argument. |
| -m       |  `-m <METHOD>`    | Indien **methode** **nemen** of **monster** is (de standaardinstelling is **nemen**). Als de methode **nemen**, haalt de kernel elementen van de bovenkant van de resultaatset gegevens opgegeven met MAXROWS (Zie verderop in deze tabel). Als de methode **monster**, de kernel elementen van de gegevensset volgens willekeurig monster `-r` parameter, die hieronder wordt beschreven in deze tabel.   |
| -r     |     `-r <FRACTION>`            | **NOEMER** is hier een getal met drijvende komma tussen 0,0 en 1,0. Als de methode monster voor de SQL-query is `sample`, en vervolgens de kernel willekeurig monsters die het opgegeven deel van de elementen van de resultaatset. bijvoorbeeld als u een SQL-query met de argumenten uitvoert `-m sample -r 0.01`, en vervolgens 1% van het Resultatenrijen worden willekeurig bemonsterd. |
| -n      | `-n <MAXROWS>`                        | **MAXROWS** is een geheel getal. Het aantal rijen met **MAXROWS**uitvoer, de kernel wordt beperkt. Als **MAXROWS** een negatief getal zoals **-1 is**, vervolgens het aantal rijen in de resultaatset niet worden beperkt. |

**Voorbeeld:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

De instructie hiervoor doet het volgende:

* Selecteert alle records uit de **hivesampletable**.
* Omdat we - q gebruiken, schakelt automatische visualisatie.
* Omdat we gebruik `-m sample -r 0.1 -n 500` het willekeurig monsters van 10% van de rijen in de hivesampletable en beperkt de grootte van de resultaatset naar 500 rijen.
* Ten slotte, omdat we gebruikt `-o query2` ook wordt de uitvoer opgeslagen in een dataframe **query2**genoemd.
    

## <a name="considerations-while-using-the-new-kernels"></a>Overwegingen bij het gebruik van de nieuwe kernels

De kernel die u gebruikt (PySpark of motoren), waardoor de laptops met verbruiken de clusterbronnen.  Met deze kernels, omdat de contexten zijn vooraf ingesteld, gewoon verlaten de laptops worden niet beëindigen de context en dus de de clusterbronnen zal blijven worden gebruikt. Met de PySpark en Spark kernels verstandig zou gebruik van de optie **afsluiten en stilstand** **van het notitieblok berichtenbestand** zijn. Dit beëindigd de context en de laptop vervolgens afgesloten.    


## <a name="show-me-some-examples"></a>Sommige voorbeelden weergeven

Als u een laptop Jupyter opent, ziet u twee mappen die beschikbaar zijn op het hoogste niveau.

* De map **PySpark** heeft monster laptops die gebruikmaken van de nieuwe **Python** kernel.
* De map **Scala** heeft monster-laptops met de nieuwe **motoren** kernel.

De laptop **00 - [Lees mij eerst] Spark Magic Kernel functies** kunt u openen vanuit de map **PySpark** of **vonk** voor meer informatie over de verschillende beschikbare magics. U kunt ook het andere monster-laptops beschikbaar onder de twee mappen gebruiken voor informatie over het bereiken van verschillende scenario's met Jupyter-laptops met HDInsight Spark clusters.

## <a name="where-are-the-notebooks-stored"></a>Waar zijn de laptops opgeslagen?

Jupyter-laptops worden opgeslagen in de opslag-account die is gekoppeld aan het cluster onder de map **/HdiNotebooks** .  Laptops, bestanden en mappen die u vanuit Jupyter maakt zijn toegankelijk vanuit de WASB.  Bijvoorbeeld als u Jupyter gebruikt om een map **mijnmap** en een laptop **myfolder/mynotebook.ipynb**te maken, u kunt openen die laptop tegen een `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Het omgekeerde geldt ook, dat wil zeggen, als u een laptop rechtstreeks uploaden naar uw rekening opslag bij `/HdiNotebooks/mynotebook1.ipynb`, de laptop ook van de Jupyter zichtbaar worden.  Laptops blijft in de opslag, zelfs nadat de cluster is verwijderd.

De manier waarop laptops worden opgeslagen op de rekening van de opslag is compatibel met HDFS. Dus als u SSH in het cluster dat kunt u opdrachten voor het beheer als volgt:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Als er problemen met toegang tot de account van de opslag voor het cluster zijn, de laptops worden ook opgeslagen in de headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Ondersteunde browser
Jupyter-laptops tegen HDInsight Spark clusters met worden alleen ondersteund in Google Chrome.

## <a name="feedback"></a>Feedback

De nieuwe kernels in fase in ontwikkeling en zal na verloop van tijd vervallen. Dit kan ook betekenen dat API's zoals deze kernels vervallen kan wijzigen. We zouden waarderen alle feedback die je hebt tijdens het gebruik van deze nieuwe kernels. Dit is erg handig in het vormgeven van de definitieve versie van deze kernels. In de sectie **opmerkingen** onderaan dit artikel kunt u uw opmerkingen/feedback.


## <a name="seealso"></a>Zie ook


* [Overzicht: Apache Spark op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Motoren met BI: interactieve gegevensanalyse Spark in HDInsight met BI-hulpprogramma's uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Motoren met Machine Learning: Spark in HDInsight voor het analyseren van gebouw temperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Motoren met Machine Learning: gebruik Spark in HDInsight te voorspellen resultaten van levensmiddelen controle](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Gebruik Spark in HDInsight voor het bouwen van real-time streaming toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met vonkontsteking in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Maken en uitvoeren van toepassingen

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Taken op afstand uitvoeren op een vonk cluster met behulp van Livius](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Hulpprogramma's en -extensies

* [HDInsight extra Plugin voor IntelliJ IDEE maken en indienen van Spark Scala applicatons gebruiken](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Gebruik HDInsight extra Plugin voor IntelliJ IDEE Spark toepassingen op afstand fouten opsporen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Zeppelin notitieblokken gebruiken met een cluster motoren op HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Externe-pakketten gebruiken met Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter op uw computer installeren en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)
