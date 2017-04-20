<properties
    pageTitle="Analyseren met behulp van de component en Hadoop sensorgegevens | Microsoft Azure"
    description="Leren hoe de sensorgegevens analyseren met behulp van de Query component Console met HDInsight (Hadoop) en vervolgens de gegevens in Microsoft Excel met PowerView."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Met de Query component Console op Hadoop in HDInsight sensorgegevens analyseren

Leren hoe de sensorgegevens analyseren met behulp van de Query component Console met HDInsight (Hadoop) en vervolgens de gegevens in Microsoft Excel weergeven met behulp van Power View.

> [AZURE.NOTE] Werken de stappen in dit document alleen met HDInsight op basis van Windows-clusters.

In dit voorbeeld, kunt u component proces historische gegevens van verwarming, ventilatie en airconditioning (HVAC)-systemen om te bepalen welke systemen niet kunnen op betrouwbare wijze een set om temperatuur te houden. U leert hoe u:

- COMPONENT tabellen aan de querygegevens opgeslagen in een door komma's gescheiden waarden (CSV) bestanden maken.
- COMPONENT-query's voor het analyseren van de gegevens maken.
- Gebruik Microsoft Excel verbinding maakt met HDInsight (open database connectivity (ODBC) met de geanalyseerde gegevens op te halen.
- Power View gebruiken om de gegevens anders.

![Een diagram van de oplossingsarchitectuur](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Vereisten

* Een HDInsight (Hadoop)-cluster: [bepaling Hadoop clusters in het HDInsight](hdinsight-provision-clusters.md) Zie voor meer informatie over het maken van een cluster.

* Microsoft Excel 2013

    > [AZURE.NOTE] Microsoft Excel wordt gebruikt voor weergave van gegevens met [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [De component Microsoft ODBC-stuurprogramma](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Voor het uitvoeren van de steekproef

1. Ga naar de volgende URL in uw webbrowser. Vervangen `<clustername>` met de naam van het cluster HDInsight.

        https://<clustername>.azurehdinsight.net

    Wanneer dat wordt gevraagd, worden geverifieerd met behulp van de gebruikersnaam en het wachtwoord waarmee u tijdens het inrichten van dit cluster.

2. Op de webpagina klikt u op het tabblad **Getting Started galerie** en klik onder de categorie **oplossingen met voorbeeldgegevens** het monster **Sensor Data Analysis** .

    ![Gestarte afbeelding ophalen](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Volg de instructies op de pagina voltooien van de steekproef.
