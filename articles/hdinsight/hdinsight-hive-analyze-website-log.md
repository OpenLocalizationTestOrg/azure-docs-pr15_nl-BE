<properties 
    pageTitle="Component gebruiken met Hadoop voor logboekanalyse website | Microsoft Azure" 
    description="Informatie over component gebruiken met HDInsight website logs analyseren. U een logboekbestand gebruiken als invoer in een tabel met HDInsight en HiveQL gebruiken de gegevens opvragen." 
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
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Component gebruiken met HDInsight voor het analyseren van Logboeken van websites

Informatie over het gebruik van HiveQL met HDInsight voor het analyseren van Logboeken van een website. Website logboekanalyse kan worden gebruikt segment van uw doelgroep op basis van soortgelijke activiteiten, demografische gegevens van sitebezoekers categoriseren en ontdek de inhoud deze weergave, het zij afkomstig zijn van websites, enzovoort.

In dit voorbeeld gebruikt u een HDInsight-cluster voor het analyseren van logbestanden website om inzicht te krijgen in de frequentie van de bezoeken aan de website van externe websites in een dag. Ook genereert u een overzicht van de website fouten die de gebruikers ondervinden. U leert hoe u:

- Verbinding maken met een Azure Blob-opslag met website-logboekbestanden.
- COMPONENT u tabellen maken om deze logboeken opvragen.
- COMPONENT-query's voor het analyseren van de gegevens maken.
- Met Microsoft Excel verbinding maken met HDInsight (met behulp van open database connectivity (ODBC) de geanalyseerde gegevens op te halen.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Vereisten

- U moet een cluster Hadoop op Azure HDInsight hebben ingericht. Zie voor instructies de [Voorziening HDInsight Clusters][hdinsight-provision]. 
- U moet Microsoft Excel 2013 of Excel 2010 is geïnstalleerd.
- U hebt [Microsoft Component ODBC-stuurprogramma](http://www.microsoft.com/download/details.aspx?id=40886) gegevens van de component importeren in Excel.


##<a name="to-run-the-sample"></a>Voor het uitvoeren van de steekproef

1. Vanuit de [Azure Portal](https://portal.azure.com/)van de Startboard (als u het cluster er vastgemaakt), klikt u op het cluster waarop de gewenste tegel uit te voeren van het monster.

2. Van de bladeserver cluster onder **Snelkoppelingen**op **Cluster Dashboard**en klik **HDInsight Cluster Dashboard**van de bladeserver **Cluster Dashboard** . U kunt het dashboard ook rechtstreeks openen via de volgende URL:

        https://<clustername>.azurehdinsight.net
    
    Wanneer dat wordt gevraagd, worden geverifieerd met behulp van de gebruikersnaam en het wachtwoord waarmee u tijdens het inrichten van het cluster.
  
2. Op de webpagina klikt u op het tabblad **Getting Started galerie** en klik vervolgens op het monster **Logboekanalyse Website** onder de categorie **oplossingen met voorbeeldgegevens** .

3. Volg de instructies op de pagina voltooien van de steekproef.

##<a name="next-steps"></a>Volgende stappen
Probeer het volgende voorbeeld: [sensorgegevens analyseren met behulp van de component met HDInsight](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
