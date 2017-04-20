<properties
    pageTitle="Installeren en gebruiken Giraph op Linux-gebaseerde HDInsight (Hadoop) | Microsoft Azure"
    description="Informatie over het installeren van Giraph op HDInsight Linux gebaseerde clusters met scriptacties. Scriptacties kunt u het cluster tijdens het maken van aanpassen door het wijzigen van de configuratie van het cluster of het installeren van services en hulpprogramma's."
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
    ms.date="10/17/2016"
    ms.author="larryfr"/>

# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Giraph op HDInsight Hadoop-clusters installeren en gebruiken van Giraph voor het verwerken van grootschalige grafieken

U kunt Giraph op elk type van cluster in Hadoop op Azure HDInsight installeren met behulp van **De actie Script** voor het aanpassen van een cluster.

In dit onderwerp leert u Giraph met behulp van de actie Script installeren. Als u Giraph hebt geïnstalleerd, leert u ook hoe u Giraph voor de meest voorkomende toepassingen, die voor het verwerken van grootschalige grafieken is.

> [AZURE.NOTE] De informatie in dit artikel is specifiek voor de HDInsight op basis van Linux-clusters. Zie voor informatie over het werken met Windows-gebaseerde clusters [Installeren Giraph op HDInsight Hadoop-clusters (Windows)](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>Wat is Giraph?

[Apache-Giraph](http://giraph.apache.org/) kunt u uitvoeren grafiek verwerkt met behulp van Hadoop en kunnen worden gebruikt met Azure HDInsight. Grafieken model relaties tussen objecten, zoals de verbindingen tussen routers op een groot netwerk zoals het Internet, of de relaties tussen mensen op sociale netwerken (ook wel aangeduid als een sociale grafiek). Verwerking van de grafiek kunt u de reden over de relaties tussen objecten in een grafiek, zoals:

- Identificeren van potentiële vrienden op basis van uw huidige relaties.
- De kortste route tussen twee computers in een netwerk identificeren.
- Berekening van de pagina rang van webpagina's.

> [AZURE.WARNING] Onderdelen van het cluster HDInsight worden volledig ondersteund en Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze componenten.
>
> Aangepaste onderdelen, zoals Giraph, recht op commercieel redelijke ondersteuning waarmee u het probleem verder oplossen. Dit kan leiden tot het oplossen van het probleem of vraag aan een van de beschikbare kanalen voor de open-source technologieën waarbij diepe expertise voor de technologie die wordt gevonden. Er zijn bijvoorbeeld veel sites van de community die kunnen worden gebruikt, zoals: [MSDN forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projecten hebben ook projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).

##<a name="what-the-script-does"></a>Wat het script doet

Met dit script worden de volgende acties uitgevoerd:

* Giraph om te worden geïnstalleerd`/usr/hdp/current/giraph`
* Kopieën de `giraph-examples.jar` bestand naar de standaard opslag (WASB) voor uw cluster:`/example/jars/giraph-examples.jar`

## <a name="install"></a>Giraph met acties Script installeren

Een voorbeeld van een script Giraph installeren op een cluster HDInsight is beschikbaar op de volgende locatie.

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Deze sectie bevat instructies voor het gebruik van het voorbeeldscript tijdens het maken van het cluster met behulp van de Portal Azure. 

> [AZURE.NOTE] Azure PowerShell, de CLI Azure, het HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt script acties worden uitgevoerd. U kunt ook scriptacties toepassen op clusters is gebeurd. Zie [clusters met scriptacties HDInsight aanpassen](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie.

1. Beginnen met het maken van een cluster met behulp van de stappen in [HDInsight maken van Linux gebaseerde clusters](hdinsight-hadoop-create-linux-clusters-portal.md), maar maken niet voltooien.

2. Op het blad **Optionele configuratie** **Scriptacties**selecteren en vul de onderstaande gegevens:

    * __Naam__: Geef een beschrijvende naam voor de scriptactie.
    * __SCRIPT-URI__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    * __Kop__: Schakel dit selectievakje in
    * __Werknemer__: laat deze ongecontroleerde
    * __ZOOKEEPER__: laat deze ongecontroleerde
    * __PARAMETERS__: laat dit veld leeg

3. Gebruik de knop **Selecteer** de configuratie opslaan onderaan het **Scriptacties**. Tot slot de knop **selecteren** onderaan het blad **Optionele configuratie** gebruiken de optionele configuratie-informatie opslaan.

4. Doorgaan met het maken van het cluster, zoals beschreven in het [HDInsight maken van Linux gebaseerde clusters](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Hoe gebruik ik Giraph in HDInsight

Nadat u het cluster maakt, is voltooid, gebruik de volgende stappen uit te voeren in het SimpleShortestPathsComputation voorbeeld opgenomen met Giraph. Dit implementeert de elementaire <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> uitvoering voor het zoeken naar de kortste route tussen objecten in een grafiek.

1. Verbinding maken met het HDInsight-cluster met behulp van SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Zie de volgende onderwerpen voor meer informatie over het gebruik van SSH in HDInsight:

    * [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

1. Maak een nieuw bestand met de naam __tiny_graph.txt__gebruikt u het volgende:

        nano tiny_graph.txt

    Als de inhoud van dit bestand, gebruikt u het volgende:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Deze gegevens beschrijft de relatie tussen objecten in een gestuurde grafiek, in de indeling [bron\_-id, de bron\_waarde [[dest\_id], [rand\_waarde],...]]. Elke regel staat voor een relatie tussen een **bron\_id** object en een of meer **dest\_id** objecten. Het **rand\_waarde** (of gewicht) kan worden beschouwd als de sterkte of de afstand van de verbinding tussen **source_id** en **doel\_id**.

    Getekend, en de waarde (of gewicht) gebruikt als de afstand tussen objecten, de bovenstaande gegevens als volgt uitzien:

    ![tiny_graph.txt getekend als cirkels met verschillende afstand tussen regels](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. Gebruik __Ctrl + X__, dan __Y__en ten slotte op __Enter__ te accepteren van de bestandsnaam om het bestand.

3. De gegevens worden opgeslagen in de primaire opslag voor uw cluster HDInsight gebruikt u het volgende:

        hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt

4. Het SimpleShortestPathsComputation voorbeeld met de volgende opdracht uitvoeren.

         yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

    De parameters voor deze opdracht worden in de volgende tabel beschreven.

  	| Parameter | Wat het doet |
  	| --------- | ------------ |
  	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | Het jar-bestand met de voorbeelden. |
  	| `org.apache.giraph.GiraphRunner` | De klasse die wordt gebruikt voor het starten van de voorbeelden. |
  	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | In het voorbeeld dat wordt uitgevoerd. In dit geval wordt het berekenen het kortste pad tussen-ID 1 en alle andere-id's in de grafiek. |
  	| `-ca mapred.job.tracker=headnodehost:9010` | De headnode voor het cluster. |
  	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | De input-indeling te gebruiken voor de ingevoerde gegevens. |
  	| `-vip /example/data/tiny_graph.txt` | Het bestand van de ingevoerde gegevens. |
  	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | De indeling van de uitvoer. In dit geval-ID en waarde als tekst zonder opmaak. |
  	| `-op /example/output/shortestpaths` | De locatie van uitvoer. |
  	| `-w 2` | Het aantal werknemers moet worden gebruikt. In dit geval 2. |

    Zie voor meer informatie over deze en andere parameters voor de Giraph monsters de [quickstart Giraph](http://giraph.apache.org/quick_start.html).

5. Nadat de afdruktaak is voltooid, worden de resultaten opgeslagen in de __wasbs: / / / voorbeeld/out/shotestpaths__ directory. Begint de bestanden die zijn gemaakt met __deel-m -__ en eindigen met een getal dat de eerste, tweede, enz., bestand aangeeft. Gebruik de volgende om de uitvoer weer te geven:

        hdfs dfs -text /example/output/shortestpaths/*

    De uitvoer ziet er ongeveer als volgt:

        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    Het SimpleShortestPathComputation voorbeeld vaststaat dat beginnen met object-ID 1 en de kortste route op andere objecten. Zodat de uitvoer moet worden gelezen als `destination_id distance`, waarbij afstand is de waarde (of gewicht) van de randen gereisd tussen object-ID 1 en de doel-ID.

    Dit visualiseren, kunt u de resultaten controleren door de kortste paden reist tussen-ID 1 en alle andere objecten. Houd er rekening mee dat het kortste pad tussen-ID 1 en 4-ID 5 is. Dit is de totale afstand tussen <span style="color:orange">ID 1 en 3</span>, en vervolgens <span style="color:red">ID 3 en 4</span>.

    ![Tekenen van objecten als cirkels met de kortste paden tussen](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## <a name="next-steps"></a>Volgende stappen

- [Installeren en gebruiken de tint van HDInsight clusters](hdinsight-hadoop-hue-linux.md). Tint is een web-UI waarmee u gemakkelijk te maken, uitvoeren en opslaan van varkens- en component-taken en bladeren de standaard opslag voor uw HDInsight cluster.

- [R op HDInsight clusters installeren](hdinsight-hadoop-r-scripts-linux.md): instructies voor het gebruik van cluster aanpassen als u wilt installeren en gebruiken van R op HDInsight Hadoop clusters. R is een open source-taal en omgeving voor statistische berekeningen. Biedt honderden ingebouwde statistische functies en een eigen programmeertaal die aspecten van functionele en object-georiënteerd programmeren combineert. Het biedt ook uitgebreide grafische mogelijkheden.

- [Solr op HDInsight clusters installeren](hdinsight-hadoop-solr-install-linux.md). Solr op HDInsight Hadoop-clusters installeren via cluster aanpassen. Solr kunt u krachtige zoekfunctie bewerkingen uitvoeren op gegevens die zijn opgeslagen.
