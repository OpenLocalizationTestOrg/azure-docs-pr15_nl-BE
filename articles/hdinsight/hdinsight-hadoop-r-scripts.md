<properties
    pageTitle="Gebruik R in HDInsight clusters aanpassen | Microsoft Azure"
    description="Informatie over het installeren van de actie Script R en R op HDInsight clusters gebruiken."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van R op HDInsight Hadoop-clusters

Informatie over het aanpassen van Windows gebaseerd HDInsight cluster met actie Script R en R gebruiken op HDInsight clusters. [Premium-reeks](https://azure.microsoft.com/pricing/details/hdinsight/) bieden voor HDInsight bevat R-Server als onderdeel van het cluster HDInsight. Dit kunt u via scripts R MapReduce en Spark gebruiken om gedistribueerde berekeningen uitvoeren. Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)voor meer informatie. Zie voor meer informatie over het gebruik van R met een Linux-gebaseerde clusters [installeren en gebruiken R op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
R op elk type cluster (Hadoop, Storm, HBase, Spark) kunt op Azure HDInsight u installeren met behulp van *De actie Script*. Een voorbeeldscript R installeren op een cluster HDInsight is beschikbaar in een blob Azure opslag voor alleen-lezen op [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1). 

**Verwante artikelen**

- [Installeren en gebruiken van R op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Clusters maken Hadoop in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van clusters van HDInsight
- [Aanpassen HDInsight cluster met behulp van de actie Script][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met actie Script
- [De actie Script scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Wat is R?

Het <a href="http://www.r-project.org/" target="_blank">Project voor het berekenen van statistische R</a> is een open source-taal en omgeving voor statistische berekeningen. R biedt honderden build in statistische functies en een eigen programmeertaal die aspecten van functionele en object-georiënteerd programmeren combineert. Het biedt ook uitgebreide grafische mogelijkheden. R is de voorkeurs-programmeeromgeving voor de meeste professionele statistici en wetenschappers in een groot aantal velden.

R is compatibel met Azure Blob Storage (WASB), zodat gegevens die er zijn opgeslagen, worden verwerkt met R op HDInsight.  

## <a name="install-r"></a>R installeren

Een [voorbeeldscript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) R installeren op een cluster van HDInsight zijn beschikbaar vanaf een alleen-lezen blob in Azure opslag. Deze sectie bevat instructies over het gebruik van het voorbeeldscript tijdens het maken van het cluster via de Portal Azure.

> [AZURE.NOTE] Het voorbeeldscript is geïntroduceerd met HDInsight cluster versie 3.1. Zie voor meer informatie over HDInsight cluster versies [HDInsight cluster versies](hdinsight-component-versioning.md).

1. Wanneer u een cluster HDInsight vanaf de Portal maakt, klikt u op **Optionele configuratie**en klik op **Scriptacties**.
2. Voer de volgende waarden op de pagina **Scriptacties** :

    ![Gebruik scriptactie voor het aanpassen van een cluster] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Gebruik scriptactie voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie, bijvoorbeeld <b>R installeren</b>.</td></tr>
        <tr><td>URI-script</td>
            <td>Geef de URI voor het script dat wordt aangeroepen voor het aanpassen van het cluster, bijvoorbeeld <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Knooppunttype</td>
            <td>Geef de knooppunten waarop het aangepaste script wordt uitgevoerd. U kunt <b>Alle knooppunten</b>, <b>alleen knooppunten Head</b>of <b>knooppunten werknemer</b> alleen.
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script. Echter, het script installeren R hoeft niet parameters, zodat u kunt deze leeg laten.</td></tr>
    </table>

    U kunt meer dan één scriptactie om meerdere onderdelen op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het crating van het cluster.

U kunt het script ook R op HDInsight met Azure PowerShell of de HDInsight .NET SDK installeren. Verderop in dit artikel vindt u instructies voor deze procedures.

## <a name="run-r-scripts"></a>R-scripts uitvoeren
In deze sectie wordt beschreven hoe een R-script uitvoert op het cluster Hadoop met HDInsight.

1. **Een extern bureaublad-verbinding met het cluster maken**: vanuit de Portal voor extern bureaublad inschakelen voor het cluster dat u hebt gemaakt met R geïnstalleerd en vervolgens verbinding maken met het cluster. Zie [verbinding maken met clusters van HDInsight met RDP](hdinsight-administer-use-management-portal.md#rdp)voor instructies.

2. **Open de console R**: R de installatie plaatst een link naar de R console op het bureaublad van het knooppunt head. Klikt u op de R console openen.

3. **Voer het script R**: de R-script rechtstreeks vanuit de console R kan worden uitgevoerd door te plakken, te selecteren en op ENTER te drukken. Hier volgt een eenvoudig voorbeeldscript die vervolgens wordt vermenigvuldigd met 2 en genereert de getallen 1 tot 100.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

De eerste twee regels roept de RHadoop-bibliotheken die worden geïnstalleerd met R. De laatste regel worden de resultaten aan de console. De uitvoer ziet er zo uit:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>R met Aure PowerShell installeren

Zie [clusters van HDInsight aanpassen met behulp van de actie Script](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Het voorbeeld laat zien hoe Spark met Azure PowerShell te installeren. U moet het script voor het gebruik van [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)aanpassen.

## <a name="install-r-using-net-sdk"></a>R met .NET SDK installeren

Zie [clusters van HDInsight aanpassen met behulp van de actie Script](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Het voorbeeld laat zien hoe Spark met de .NET SDK installeren. U moet het script voor het gebruik van [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11)aanpassen.


## <a name="see-also"></a>Zie ook

- [Installeren en gebruiken van R op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Clusters maken Hadoop in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van clusters van HDInsight
- [Aanpassen HDInsight cluster met behulp van de actie Script][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met actie Script
- [De actie Script scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md)
- [Installeren en gebruiken van Spark op clusters HDInsight][hdinsight-install-spark]: actie Script voorbeeld over het installeren van Spark
- [Giraph op HDInsight-clusters installeren](hdinsight-hadoop-giraph-install.md): actie Script voorbeeld over het installeren van Giraph
- [Solr installeren op HDInsight clusters](hdinsight-hadoop-solr-install-linux.md): actie Script voorbeeld over het installeren van Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
