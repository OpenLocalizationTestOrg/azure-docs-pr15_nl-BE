<properties 
    pageTitle="Bekende problemen van Apache Spark in HDInsight | Microsoft Azure" 
    description="Bekende problemen van Apache Spark in HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Bekende problemen bij het cluster op HDInsight Linux Apache Spark

Dit document houdt van de bekende problemen voor de public preview HDInsight Spark.  

##<a name="livy-leaks-interactive-session"></a>Hier lekt interactieve sessie
 
Wanneer hier opnieuw met een interactieve sessie (Ambari of door headnode 0 virtuele machine opnieuw opstarten) nog in leven is gestart, wordt een taak interactieve sessie wordt gelekt. Hierdoor nieuwe banen kunnen blijven in de status goedgekeurd en kunnen niet worden gestart.

**Beperkende factoren:**

Gebruik de volgende procedure om een tijdelijke oplossing van het probleem:

1. SSH in headnode. 
2. Voer de volgende opdracht om de aanvraag-id's van de interactieve projecten gestart via hier vinden. 

        yarn application –list

    De standaard taak namen hier worden als de taken zijn gestart met een interactieve sessie hier geen expliciete namen opgegeven voor Livius-sessie gestart door Jupyter, laptop, de taaknaam van de wordt gestart met remotesparkmagics_ *. 

3. Voer de volgende opdracht voor het doden van deze taken. 

        yarn application –kill <Application ID>

Nieuwe taken worden gestart. 

##<a name="spark-history-server-not-started"></a>Spark geschiedenis Server is niet gestart 

Spark geschiedenis Server wordt niet automatisch gestart nadat er een cluster wordt gemaakt.  

**Beperkende factoren:** 

De geschiedenis-server handmatig starten vanaf Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Machtiging probleem in de logboekmap Spark 

Wanneer hdiuser een project met een spark-submit indient, er is een fout java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (toestemming geweigerd) en het Stuurprogrammalogboek niet is geschreven. 

**Beperkende factoren:**
 
1. Hdiuser toevoegen aan de groep Hadoop. 
2. 777 machtigingen op /var/log/spark na het maken van het cluster opgeven. 
3. De locatie van het logboek vonk met Ambari wordt een map met 777 machtigingen bijwerken.  
4. Run spark-indienen als sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Problemen met Jupyter-laptops

Hier volgen enkele bekende problemen met betrekking tot Jupyter-laptops.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Laptops met niet-ASCII-tekens in bestandsnamen

Jupyter-laptops die kunnen worden gebruikt in clusters Spark HDInsight mogen geen niet-ASCII-tekens in bestandsnamen. Als u probeert te uploaden via de Jupyter UI een niet-ASCII-bestandsnaam is, zal mislukken zonder bericht (dat wil zeggen Jupyter kunt u het bestand uploaden, maar het won't een zichtbare fout of throw). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fout tijdens het laden van laptops van groter formaat

Ziet u een fout **`Error loading notebook`** bij het laden van laptops die groter zijn.  

**Beperkende factoren:**

Als u deze fout krijgt, betekent niet dat uw gegevens is beschadigd of verloren gaan.  Uw notitieblokken worden nog steeds op de schijf in `/var/lib/jupyter`, en u kunt SSH in het cluster te krijgen. U kunt uw notitieblokken uit het cluster als een back-up om te voorkomen dat het verlies van alle belangrijke gegevens in het notitieblok op uw lokale computer (met SCP of WinSCP) kopiëren. Vervolgens kunt u SSH tunnel in uw headnode in haven 8001 toegang te krijgen tot Jupyter zonder tussenkomst van de gateway.  U kunt daar de uitvoer van uw laptop te wissen en opnieuw opslaan om het minimaliseren van de grootte van het notitieblok.

Als u wilt voorkomen dat deze fout in de toekomst, moet u volgen enkele aanbevelingen:

* Het is belangrijk klein te houden de laptop-grootte. Uitvoer van uw projecten vonk die wordt verzonden naar de Jupyter is vastgelegd in het notitieblok.  Het is raadzaam met Jupyter in het algemeen kunt u voorkomen dat `.collect()` op grote RDD of dataframes; in plaats daarvan kunt uitvoeren als u wilt voor het bekijken van de inhoud van een RDD, `.take()` of `.sample()` , zodat de uitvoer niet te groot.
* Ook wanneer u een notitieblok opslaan, uitvoer wissen alle van cellen wilt verkleinen.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Laptop opstarten duurt langer dan verwacht 

Eerste code-instructie in een Jupyter laptop met een vonk magic kan meer dan een minuut duren.  

**Uitleg:**
 
Dit gebeurt omdat wanneer de eerste cel in de code wordt uitgevoerd. Op de achtergrond gestart dit configuratie sessie en Spark, SQL en component contexten worden ingesteld. Nadat deze contexten zijn ingesteld, wordt de eerste instructie uitgevoerd en dit de indruk dat geeft de instructie de duurt lang te voltooien.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter laptop time-out bij het maken van de sessie

Wanneer Spark cluster geen bronnen, zal de kernels vonk en Pyspark in het notitieblok Jupyter time-out bij het maken van de sessie. 

**Beperkende factoren:** 

1. Sommige bronnen in het cluster Spark door vrij:

    - Andere laptops Spark stoppen door het menu sluiten en stilstand of klikt u op Afsluiten in het notitieblok explorer.
    - Andere toepassingen voor motoren van GAREN wordt gestopt.

2. Start het notitieblok dat u wilt starten. Onvoldoende systeembronnen moeten beschikbaar zijn voor u nu een sessie te maken.

##<a name="see-also"></a>Zie ook

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

* [Kernels die beschikbaar zijn voor de laptop in een cluster voor HDInsight Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Externe-pakketten gebruiken met Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter op uw computer installeren en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)
