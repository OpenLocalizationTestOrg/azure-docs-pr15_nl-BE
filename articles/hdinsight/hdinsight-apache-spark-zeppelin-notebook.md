<properties 
    pageTitle="Zeppelin notitieblokken gebruiken met vonkontsteking cluster op HDInsight Linux | Microsoft Azure" 
    description="Stapsgewijze instructies voor het gebruik van Zeppelin-laptops met vonkontsteking clusters op HDInsight Linux." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Zeppelin-laptops met Apache Spark cluster op HDInsight Linux gebruiken

HDInsight Spark clusters zijn Zeppelin-laptops kunt u Spark taken uitvoeren. In dit artikel informatie over het gebruik van de Zeppelin-laptop op een cluster van HDInsight.


**Voorwaarden:**

* Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Een cluster van Apache Spark. Zie [clusters in Azure HDInsight Apache Spark maken](hdinsight-apache-spark-jupyter-spark-sql.md)voor instructies.

## <a name="launch-a-zeppelin-notebook"></a>Starten van een Zeppelin-laptop

1. Uit het cluster Spark blade, klikt u op **Cluster Dashboard**en klik op **Zeppelin-laptop**. Geef desgevraagd de referenties van de beheerder voor het cluster.

    > [AZURE.NOTE] U kan ook de Zeppelin-laptop voor uw cluster bereiken via de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van het cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Een nieuw notitieblok maken. Vanuit het deelvenster kop op **notitieblok**en klik vervolgens op **Nieuwe notitie maken**.

    ![Een nieuw Zeppelin notitieblok maken] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Een nieuw Zeppelin notitieblok maken")

    Voer een naam voor de laptop en klik vervolgens op **Notitie maken**.

3. Controleer ook of dat de koptekst van de laptop toont de status van een verbonden. Dit wordt aangeduid met een groene stip in de rechterbovenhoek.

    ![Status van zeppelin-laptop] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Status van zeppelin-laptop")

4. Voorbeeldgegevens worden geladen in een tijdelijke tabel. Wanneer u een cluster Spark in HDInsight maakt, wordt het voorbeeldgegevensbestand, **hvac.csv**, aan de account gekoppeld opslag onder **\HdiSamples\SensorSampleData\hvac**gekopieerd.

    In de lege alinea die standaard in de nieuwe laptop is gemaakt, plak het volgende fragment.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Druk op **SHIFT + ENTER** of klik op de knop **afspelen** voor de alinea uit te voeren in het fragment. De status in de rechterbenedenhoek van de alinea moet de voortgang van READY, in AFWACHTING van actief in GEREEDGEMELD. De uitvoer wordt weergegeven onder aan dezelfde alinea. Het screenshot ziet er als volgt uit:

    ![Maak een tijdelijke tabel uit onbewerkte gegevens] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Maak een tijdelijke tabel uit onbewerkte gegevens")

    U kunt ook een titel aan elke alinea opgeven. Klik op **het pictogram** in de rechter bovenhoek en klik vervolgens op **Titel weergeven**.

5. U kunt nu Spark SQL-instructies uitvoeren op de tabel **voor installaties** . De volgende query plakken in een nieuwe alinea. De query haalt de ID van het gebouw en het verschil tussen het doel en de werkelijke temperatuur voor elke bouwen op een bepaalde datum. Druk op **SHIFT + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    **% Sql** -instructie aan het begin wordt het notitieblok gebruiken de interpreter hier Scala.

    De volgende schermafbeelding ziet u de uitvoer.

    ![Uitvoeren van een vonk SQL-instructie met behulp van de laptop] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Uitvoeren van een vonk SQL-instructie met behulp van de laptop")

     Klik op de weergave-opties (gemarkeerd in de rechthoek) om te schakelen tussen verschillende weergaven voor hetzelfde resultaat op. Klik op **Instellingen** als u wilt kiezen welke consitutes de sleutel en de waarden in de uitvoer. De schermopname hierboven gebruikt **buildingID** als de sleutel en het gemiddelde van de **temp_diff** als de waarde.

    
6. Ook kunt u een SQL-instructies die variabelen gebruiken in de query uitvoeren. In het volgende fragment laat zien hoe de **Temp**-variabele definiëren in de query met de mogelijke waarden die u wilt opvragen met. Wanneer u de query voor het eerst uitvoert, wordt een vervolgkeuzelijst wordt automatisch gevuld met de waarden die u hebt opgegeven voor de variabele.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    In dit fragment plakken in een nieuwe alinea en druk op **SHIFT + ENTER**. De volgende schermafbeelding ziet u de uitvoer.

    ![Uitvoeren van een vonk SQL-instructie met behulp van de laptop] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Uitvoeren van een vonk SQL-instructie met behulp van de laptop")

    Voor volgende aanvragen kunt u een nieuwe waarde selecteren in de vervolgkeuzelijst en voer de query opnieuw uit. Klik op **Instellingen** als u wilt kiezen welke consitutes de sleutel en de waarden in de uitvoer. De bovenstaande schermopname gebruikt **buildingID** als de sleutel, het gemiddelde van de **temp_diff** als de waarde en de **targettemp** als de groep.

7. Start de interpreter hier om de toepassing af te sluiten. Hiertoe interpreter instellingen openen door te klikken op de aangemelde gebruikersnaam in de rechterbovenhoek en klik vervolgens op **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Component output")

2. Schuif hier interpreter instellingen aan en klik op **opnieuw opstarten**.

    ![Start de intepreter hier] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Opnieuw opstarten van de Zeppelin-intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Hoe gebruik externe pakketten met de laptop?

In Apache Spark cluster op HDInsight (Linux) kunt u het notitieblok Zeppelin configureren voor het gebruik van externe, Gemeenschap bijgedragen pakketten die niet opgenomen out-of-the-box in het cluster zijn. U kunt de [Maven opslagplaats](http://search.maven.org/) voor de complete lijst van pakketten die beschikbaar zijn op te zoeken. U kunt ook een lijst van beschikbare pakketten ophalen uit andere bronnen. Een complete lijst van pakketten communautaire bijdrage is bijvoorbeeld beschikbaar op [Spark pakketten](http://spark-packages.org/).

In dit artikel ziet u hoe u het pakket [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) met de laptop Jupyter.

1. Open instellingen voor interpreter. Klik op de aangemelde gebruikersnaam in de rechterbovenhoek en klik op **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Component output")

2. Schuif hier interpreter instellingen aan en klik vervolgens op **bewerken**.

    ![Interpreter-instellingen wijzigen] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Interpreter-instellingen wijzigen")

3. Voeg een nieuwe sleutel, genaamd **livy.spark.jars.packages** en stelt u de waarde in de notatie `group:id:version`. Als u het [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -pakket gebruiken wilt, moet u dus de waarde van de sleutel tot instellen `com.databricks:spark-csv_2.10:1.4.0`.

    ![Interpreter-instellingen wijzigen] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Interpreter-instellingen wijzigen")

    Klik op **Opslaan** en start de interpreter hier.

4. **Tip**: als u wilt weten hoe om te komen tot de waarde van de sleutel hierboven, is hier hoe.

    een. Zoek het pakket in de opslagplaats van Maven. We gebruiken [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar)voor deze zelfstudie.
    
    b. Verzamelen van de opslagplaats, de waarden voor de **groeps-id**, **ArtifactId**en **versie**.

    ![Gebruik externe pakketten met Jupyter-laptop] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Gebruik externe pakketten met Jupyter-laptop")

    c. Samenvoegen van de drie waarden, gescheiden door een dubbele punt (**:**).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Waar worden de Zeppelin-laptops opgeslagen?

De Zeppelin-laptops worden opgeslagen in de headnodes van het cluster. Dus als u het cluster verwijdert, worden de laptops ook verwijderd. Als u wilt dat uw notitieblokken voor later gebruik op andere clusters te behouden, moet u ze exporteren nadat u klaar bent met het uitvoeren van de taken. Als u wilt exporteren een notitieblok, klikt u op het pictogram **exporteren** zoals in de onderstaande afbeelding.

![Laptop downloaden] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Downloaden van de laptop")

Dit bespaart het notitieblok als een JSON-bestand op uw downloadlocatie.

## <a name="livy-session-management"></a>Livius sessiebeheer

Wanneer u de eerste alinea in de code in uw notitieblok Zeppelin uitvoert, wordt een nieuwe sessie van Livius in het cluster HDInsight Spark gemaakt. Deze sessie wordt gedeeld door alle Zeppelin laptops die u daarna maakt. Als de sessie is hier gedood om een bepaalde reden (cluster opnieuw opstarten, enz.), is het niet mogelijk taken uitvoeren vanuit de Zeppelin-laptop.

In dat geval moet u de volgende stappen uitvoeren voordat u kunt taken uitvoeren vanaf een laptop Zeppelin. 

1. De interpreter hier uit het notitieblok Zeppelin opnieuw. Hiertoe interpreter instellingen openen door te klikken op de aangemelde gebruikersnaam in de rechterbovenhoek en klik vervolgens op **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Component output")

2. Schuif hier interpreter instellingen aan en klik op **opnieuw opstarten**.

    ![Start de intepreter hier] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Opnieuw opstarten van de Zeppelin-intepreter")

3. Een cel code uitvoeren vanuit een bestaand notitieblok van Zeppelin. Hiermee maakt u een nieuwe sessie van Livius in het cluster HDInsight.

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

* [Kernels die beschikbaar zijn voor de laptop in een cluster voor HDInsight Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Externe-pakketten gebruiken met Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter op uw computer installeren en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md 







