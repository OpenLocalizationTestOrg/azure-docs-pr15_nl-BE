<properties
    pageTitle="R installeren op Linux-gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over het installeren en gebruiken van R Hadoop Linux gebaseerde clusters aanpassen."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van R op HDInsight Hadoop-clusters

R kunt op elk type van cluster in Hadoop op HDInsight u installeren met behulp van **De actie Script** cluster aanpassen. Hierdoor kunnen gegevens wetenschappers en analisten R gebruiken voor de implementatie van de krachtige MapReduce garens/programming framework voor het verwerken van grote hoeveelheden gegevens op Hadoop-clusters die zijn geïmplementeerd in HDInsight.

> [AZURE.IMPORTANT] [Premium-reeks](https://azure.microsoft.com/pricing/details/hdinsight/) bieden voor HDInsight bevat R-Server als onderdeel van het cluster HDInsight. Dit kunt u via scripts R MapReduce en Spark gebruiken om gedistribueerde berekeningen uitvoeren. Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)voor meer informatie. 


## <a name="what-is-r"></a>Wat is R?

Het <a href="http://www.r-project.org/" target="_blank">Project voor het berekenen van statistische R</a> is een open source-taal en omgeving voor statistische berekeningen. R biedt honderden build in statistische functies en een eigen programmeertaal die aspecten van functionele en object-georiënteerd programmeren combineert. Het biedt ook uitgebreide grafische mogelijkheden. R is de voorkeurs-programmeeromgeving voor de meeste professionele statistici en wetenschappers in een groot aantal velden.

R-scripts kunnen worden uitgevoerd op Hadoop clusters in HDInsight die zijn aangepast met de actie Script bij het installeren van de R-omgeving. R is compatibel met Azure Blob Storage (WASB), zodat gegevens die er zijn opgeslagen, worden verwerkt met R op HDInsight.

## <a name="what-the-script-does"></a>Wat het script doet

De scriptactie gebruikt voor de installatie van R op het cluster HDInsight installeert de volgende Ubuntu pakketten bieden een eenvoudige R-installatie:

* [r-base](http://packages.ubuntu.com/precise/r-base): Base GNU R package
* [r-base-dev](http://packages.ubuntu.com/precise/r-base-dev): Auxilliary GNU R pakketten

De volgende RHadoop-pakketten zijn geïnstalleerd, die zorgen voor integratie met MapReduce en HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): Hiermee kunnen ontwikkelaars gebruik van Hadoop MapReduce R
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): kunnen ontwikkelaars R Hadoop HDFS (WASB voor HDInsight)

Bovendien worden de volgende R-pakketten worden geïnstalleerd:

| R-pakket | Wat biedt |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Een laag niveau R Java-interface. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | Integratie van R- en C++. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Objecten R serialiseren/terugconverteren naar JSON |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Functies voor bitsgewijze bewerkingen op vectoren met integer. |
| [Verificatiesamenvatting](https://cran.r-project.org/web/packages/digest/index.html) | Cryptografische Hash geklaard R-objecten maken. |
| [functionele](https://cran.r-project.org/web/packages/functional/index.html) | Currypoeders, opstellen en andere functies van hogere orde |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Flexibel reorganiseren en statistische gegevens. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Eenvoudige en consistente Wrappers voor veelgebruikte tekenreeksbewerkingen. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Hulpprogramma's voor splitsen, toepassen en combineren van gegevens. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Hulpmiddelen voor het verplaatsen van venster Statistiek, GIF, Base64, ROC AUC, enz. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Overeenkomende tekenreeks benaderen en String functies afstand. |

## <a name="install-r-using-script-actions"></a>R acties Script installeren

Het volgende scriptactie wordt R installeren op een cluster van HDInsight gebruikt. https://hdiconfigactions.BLOB.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.sh
    
Deze sectie bevat instructies over het gebruik van het script bij het maken van een nieuw cluster met behulp van de portal Azure. 

> [AZURE.NOTE] Azure PowerShell, de CLI Azure, het HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt script acties worden uitgevoerd. U kunt ook scriptacties toepassen op clusters is gebeurd. Zie [clusters met scriptacties HDInsight aanpassen](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie.

1. Beginnen met het inrichten van een cluster met behulp van de stappen in de [HDInsight bepaling Linux gebaseerde clusters](hdinsight-hadoop-provision-linux-clusters.md#portal), maar Voer inrichten.

2. Op het blad **Optionele configuratie** **Scriptacties**selecteren en vul de onderstaande gegevens:

    * __Naam__: Geef een beschrijvende naam voor de scriptactie.
    * __SCRIPT-URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Kop__: Schakel dit selectievakje in
    * __Werknemer__: Schakel deze optie in
    * __ZOOKEEPER__: Schakel deze optie in op het knooppunt Zookeeper te installeren.
    * __PARAMETERS__: laat dit veld leeg

3. Gebruik de knop **Selecteer** de configuratie opslaan onderaan het **Scriptacties**. Tot slot de knop **selecteren** onderaan het blad **Optionele configuratie** gebruiken de optionele configuratie-informatie opslaan.

4. Het cluster wordt ingericht als beschreven in [HDInsight bepaling Linux gebaseerde clusters](hdinsight-hadoop-provision-linux-clusters.md#portal)worden voortgezet.

## <a name="run-r-scripts"></a>R-scripts uitvoeren

Nadat het cluster wordt ingericht, is voltooid, gebruik de volgende stappen R gebruiken om een bewerking MapReduce op het cluster.

1. Verbinding maken met het HDInsight-cluster met behulp van SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Zie de volgende onderwerpen voor meer informatie over het gebruik van SSH in HDInsight:

    * [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Van de `username@hn0-CLUSTERNAME:~$` wordt gevraagd, voert u de volgende opdracht om een interactieve R-sessie te starten:

        R

3. Voer de volgende R-programma. Hiermee genereert de getallen 1 tot 100 en vervolgens wordt vermenigvuldigd met 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    De eerste regel roept de RHadoop-bibliotheek rmr2, die wordt gebruikt voor het MapReduce.

    De tweede regel genereert waarden 1-100, en vervolgens worden deze opgeslagen op de Hadoop bestandssysteem via `to.dfs`.

    De derde regel maakt een MapReduce-proces met behulp van de functionaliteit die door rmr2 en verwerkt. Worden er meerdere regels schuift dan tijdens de verwerking wordt begonnen.

4. Gebruik vervolgens de volgende zodat het tijdelijke pad dat de uitvoer MapReduce is opgeslagen:

        print(calc())

    Dit moet een vergelijkbare `/tmp/file5f615d870ad2`. Als u wilt weergeven op de werkelijke output, gebruikt u de volgende:

        print(from.dfs(calc))

    De uitvoer ziet er zo uit:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Om af te sluiten, R, u het volgende:

        q()


## <a name="next-steps"></a>Volgende stappen

- [Installeren en gebruiken de tint van HDInsight clusters](hdinsight-hadoop-hue-linux.md). Tint is een web-UI waarmee u gemakkelijk te maken, uitvoeren en opslaan van varkens- en component-taken en bladeren de standaard opslag voor uw HDInsight cluster.

- [Giraph op HDInsight-clusters installeren](hdinsight-hadoop-giraph-install.md). Giraph op HDInsight Hadoop-clusters installeren via cluster aanpassen. Giraph, kunt u de grafiek verwerken met behulp van Hadoop en kan worden gebruikt met Azure HDInsight.

- [Solr op HDInsight clusters installeren](hdinsight-hadoop-solr-install.md). Solr op HDInsight Hadoop-clusters installeren via cluster aanpassen. Solr kunt u krachtige zoekfunctie bewerkingen uitvoeren op opgeslagen gegevens.

- [Kleurtoon op HDInsight clusters installeren](hdinsight-hadoop-hue-linux.md). Installeert met een cluster aanpassing Kleurtoon op HDInsight Hadoop-clusters. Tint is een verzameling van webtoepassingen die worden gebruikt voor interactie met een cluster Hadoop.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
