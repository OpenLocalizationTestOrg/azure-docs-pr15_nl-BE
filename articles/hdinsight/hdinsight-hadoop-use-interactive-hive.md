<properties
    pageTitle="Gebruik interactieve component in HDInsight | Microsoft Azure"
    description="Informatie over het gebruik van interactieve component (onderdeel van LLAP) in HDInsight."
    keywords=""
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Gebruik de interactieve component in HDInsight (voorbeeld)

Interactieve (ook component [Live lang en proces]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) is een nieuwe HDInsight [clustertype]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interactieve component kan in het cachegeheugen die component query's kunt u veel meer interactieve en sneller. Deze nieuwe functie kunt u HDInsight van's werelds meeste zodat, flexibele en open grote oplossing van de wolk met in het geheugen in de cache opgeslagen (met behulp van de component en Spark) en geavanceerde analytics via naadloze integratie met R-Services. 

Het cluster interactieve component verschilt van het cluster Hadoop. Het bevat alleen de service component. 

> [AZURE.NOTE] MapReduce, varkens, Sqoop, Oozie en andere services van dit clustertype binnenkort verwijderd.
De service component in het cluster interactieve component is alleen toegankelijk via de component Ambari weergave, Beeline en ODBC-component. Het is niet toegankelijk via de console, Templeton, Azure CLI en Azure PowerShell component. 


 


## <a name="create-an-interactive-hive-cluster"></a>Een interactieve component cluster maken

Interactieve component cluster wordt alleen ondersteund op Linux gebaseerde clusters. Zie voor meer informatie over het maken van clusters HDInsight [Hadoop maken Linux gebaseerde clusters in het HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Component van interactieve component uitvoeren

Er zijn verschillende opties voor het uitvoeren van query's component:

- Met de weergave van de component Ambari component uitvoeren

    Zie de informatie over het gebruik van de component-weergave, [Gebruik de component-weergave met Hadoop in HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Beeline met component uitvoeren

    Zie [Gebruik component met Hadoop in HDInsight met Beeline](hdinsight-hadoop-use-hive-beeline.md)voor de informatie over het gebruik van Beeline op HDInsight.

    U Beeline uit de headnode of een randknooppunt leeg.  Beeline gebruik van een randknooppunt leeg wordt aanbevolen.  Zie voor meer informatie over het maken van een cluster van HDInsight met een lege edgenode [Gebruik lege rand knooppunten in HDInsight](hdinsight-apps-use-edge-node.md).

- Met behulp van ODBC component component uitvoeren

    Zie de informatie over het gebruik van ODBC-component, [Hadoop met de component ODBC-stuurprogramma verbinding maken met Excel](hdinsight-connect-excel-hive-odbc-driver.md).

**Zoeken in de verbindingsreeks JDBC:**

1.  Aanmelden bij Ambari met de volgende URL: https://<ClusterName>. AzureHDInsight.net.
2.  Klik in het linkermenu op **component** .
3.  Klik op het gemarkeerde pictogram om de URL te kopiëren:

    ![HDInsight Hadoop interactieve component LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Zie ook
-   [Hadoop maken Linux gebaseerde clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): informatie over het maken van interactieve component clusters in HDInsight.
-   [Gebruik component met Hadoop in HDInsight met Beeline](hdinsight-hadoop-use-hive-beeline.md): informatie over het gebruik van Beeline component query's indienen.
-   [Hadoop met de component ODBC-stuurprogramma verbinding maken met Excel](hdinsight-connect-excel-hive-odbc-driver.md): informatie over het aansluiten van Excel naar component.
