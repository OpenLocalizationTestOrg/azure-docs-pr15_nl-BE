<properties 
    pageTitle="Jupyter-notitieblok op uw computer installeren en verbinden met een cluster HDInsight Spark | Microsoft Azure" 
    description="Informatie over het lokaal Jupyter notitieblok op uw computer installeren en verbinden met een cluster Apache Spark op Azure HDInsight." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Jupyter-notitieblok op uw computer installeren en verbinding maken met het cluster op HDInsight Linux Apache Spark

In dit artikel leert u installeren Jupyter-laptop met de aangepaste PySpark (voor Python) en kernels met magic Spark Spark (voor Scala), en de laptop verbinding met een cluster HDInsight. Kan er een aantal redenen Jupyter installeren op uw lokale computer en kunnen er ook enkele uitdagingen. Zie de sectie [Waarom zou ik Jupyter op mijn computer installeren](#why-should-i-install-jupyter-on-my-computer) aan het einde van dit artikel voor een lijst van redenen voor- en nadelen.

Er zijn drie belangrijke stappen betrokken bij het Jupyter en de magie Spark op uw computer installeren.

* Jupyter laptop installeren
* De PySpark en Spark kernels installeren met de magische vonk
* Spark magic voor toegang tot een cluster op HDInsight configureren

Zie voor meer informatie over de aangepaste kernels en de magie Spark verkrijgbaar voor laptops met HDInsight cluster Jupyter [Kernels die beschikbaar zijn voor laptops met Apache Spark Linux clusters op HDInsight Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Vereisten

De hier vermelde voorwaarden zijn niet voor het installeren van Jupyter. Dit zijn voor het aansluiten van de laptop Jupyter aan een cluster HDInsight zodra de laptop is geïnstalleerd.

- Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster van Apache Spark op HDInsight Linux. Zie [clusters in Azure HDInsight Apache Spark maken](hdinsight-apache-spark-jupyter-spark-sql.md)voor instructies.

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter-notitieblok op uw computer installeren.

U moet Python installeren voordat u de Jupyter-laptops kunt installeren. Python en de Jupyter zijn beschikbaar als onderdeel van de [verdeling van de Ananconda](https://www.continuum.io/downloads). Tijdens de installatie van Anaconda installeren u daadwerkelijk een verdeling van de Python. Nadat Anaconda is geïnstalleerd, kunt u de installatie van de Jupyter toevoegen door een opdracht uit te voeren. Deze sectie bevat de instructies die u moet volgen.

1. Het [installatieprogramma van de Anaconda](https://www.continuum.io/downloads) voor uw platform downloaden en uitvoeren van de setup. Tijdens het uitvoeren van de wizard setup moet dat u de optie Anaconda toevoegen aan de variabele PATH.

2. Voer de volgende opdracht om de Jupyter te installeren.

        conda install jupyter

    Zie voor meer informatie over installting, Jupyter, [Installeren van Jupyter met behulp van Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>De magische vonk en kernels installeren

Voor instructies over het installeren van de vonk magic, Zie de PySpark en Spark kernels, de [sparkmagic-documentatie](https://github.com/jupyter-incubator/sparkmagic#installation) op GitHub.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Magisch om toegang tot het cluster HDInsight Spark Spark configureren

In deze sectie configureert u de Spark magie die u eerder hebt geïnstalleerd voor de verbinding met een cluster van Apache Spark die u al in Azure HDInsight moet hebben gemaakt.

1. Informatie over de configuratie van de Jupyter wordt meestal opgeslagen in de basismap van de gebruikers. Typ de volgende opdrachten voor de basismap op elk OS platform.

    Start de Python shell. In het venster met een opdrachtprompt, typ het volgende:

        python

    Voer de volgende opdracht om te bepalen van de basismap op de shell Python.

        import os
        print(os.path.expanduser('~'))

2. Ga naar de basismap en maak een map met de naam **.sparkmagic** als deze nog niet bestaat.

3. In de map een bestand met de naam **config.json** maken en toevoegen van het volgende fragment JSON erin.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. **{USERNAME}**, **{CLUSTERDNSNAME}**en **{BASE64ENCODEDPASSWORD}** vervangen met de juiste waarden. U kunt een aantal hulpprogramma's in uw favoriete programmeertaal of on line een base64-gecodeerde wachtwoord genereren voor uw wachtwoord actualy. Is een eenvoudig stukje Python uit te voeren vanaf de opdrachtprompt:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Jupyter starten. Gebruik de volgende opdracht bij de opdrachtprompt.

        jupyter notebook

6. Controleer of u verbinding kunt maken met het cluster via de laptop Jupyter en u kunt de Spark magie beschikbaar met de kernels te gebruiken. Voer de volgende stappen.

    1. Een nieuw notitieblok maken. Klik op **Nieuw**in de rechterhoek. Ziet u de standaard kernel **Python2** en de twee nieuwe kernels die u installeert, **PySpark** en **Spark**.

        ![Een nieuwe laptop voor Jupyter maken] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Een nieuwe laptop voor Jupyter maken")

    
        Klik op **PySpark**.


    2. Het volgende codefragment worden uitgevoerd.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Als u de uitvoer met succes ophalen kunt, wordt de verbinding met het cluster HDInsight getest.

    >[AZURE.TIP] Als u wilt dat de configuratie van de laptop verbinding maken met een ander cluster bij te werken, werken de config.json met de nieuwe set van waarden, zoals in stap 3 hierboven. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Waarom moet ik Jupyter op mijn computer installeren?

Er zijn een aantal redenen waarom u willen zou Jupyter op uw computer installeren en vervolgens verbinding maken met een cluster motoren op HDInsight.

* Hoewel Jupyter-laptops al beschikbaar op het cluster Spark in Azure, HDInsight zijn, biedt Jupyter op uw computer installeren u de optie voor het maken van uw notitieblokken lokaal, testen van de toepassing tegen een cluster uitgevoerd en vervolgens de laptops uploaden met het cluster. Uploaden van de laptops aan het cluster, kunt u uploaden met behulp van de laptop Jupyter die wordt uitgevoerd of het cluster of opslaan in de map /HdiNotebooks in de opslag-account die is gekoppeld aan het cluster. Zie voor meer informatie op hoe laptops op het cluster worden opgeslagen, [waar Jupyter-laptops worden opgeslagen](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Met de laptops beschikbaar lokaal, kunt u verschillende Spark clusters op basis van de vereisten van uw toepassing.
* U kunt de GitHub te implementeren, een bronbeheersysteem en versiebeheer voor de laptops. U kunt ook een samenwerkingsomgeving waarin meerdere gebruikers met hetzelfde notitieblok kunnen werken hebben.
* U kunt lokaal werken met laptops zonder zelfs een cluster van. U hoeft alleen een cluster voor het testen van uw laptops tegen, niet voor het handmatig beheren van uw laptops of een ontwikkelomgeving.
* Wellicht gemakkelijker uw eigen lokale ontwikkelomgeving configureren, dan is de installatie van de Jupyter op het cluster configureren.  U kunt profiteren van alle software die u lokaal hebt geïnstalleerd zonder een of meer externe clusters configureren.

>[AZURE.WARNING] Met Jupyter op de lokale computer is geïnstalleerd, kunnen meerdere gebruikers uitvoeren hetzelfde notitieblok op hetzelfde cluster Spark op hetzelfde moment. Meerdere sessies van Livius zijn gemaakt in een dergelijke situatie. Als u een probleem optreden dat foutopsporing wilt, dient een complexe taak om bij te houden welke sessie hier welke gebruiker behoort.




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

* [Kernels die beschikbaar zijn voor de laptop in een cluster voor HDInsight Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Externe-pakketten gebruiken met Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Bronnen beheren

* [Bronnen voor het cluster Apache Spark in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Spoor en foutopsporing taken die worden uitgevoerd op een cluster van Apache Spark in HDInsight](hdinsight-apache-spark-job-debugging.md)
