<properties
    pageTitle="Scriptactie gebruiken voor het installeren van Solr op Hadoop cluster | Microsoft Azure"
    description="Informatie over het cluster HDInsight aanpassen met actie Script Solr."
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
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van Solr op HDInsight Hadoop-clusters

Informatie over het aanpassen van Windows-gebaseerde HDInsight cluster met actie Script Solr en Solr gebruiken om gegevens te zoeken. Zie voor meer informatie over het gebruik van Solr met een Linux-gebaseerde clusters [installeren en gebruiken van Solr op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Solr op elk type cluster (Hadoop, Storm, HBase, Spark) kunt op Azure HDInsight u installeren met behulp van *De actie Script*. Een voorbeeldscript Solr installeren op een cluster HDInsight is beschikbaar in een blob Azure opslag voor alleen-lezen op [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1). 

Het script werkt alleen met HDInsight cluster versie 3.1. Zie voor meer informatie over de versies van HDInsight cluster, [cluster versies van HDInsight](hdinsight-component-versioning.md).

Het voorbeeldscript in dit onderwerp wordt gebruikt, maakt een Solr Windows gebaseerde cluster met een specifieke configuratie. Als u wilt dat de Solr-cluster configureren met verschillende collecties, shards, schema's, replica's, enz., moet u het script en de binaire bestanden van Solr dienovereenkomstig wijzigen.

**Verwante artikelen**

- [Installeren en gebruiken van Solr op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Clusters maken Hadoop in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van clusters van HDInsight.
- [Aanpassen HDInsight cluster met behulp van de actie Script][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met actie Script.
- [Ontwikkelen scriptactie scripts voor HDInsight](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Wat is Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is een enterprise search-platform waarmee u krachtige zoeken in volledige tekst van de gegevens. Hadoop kunt opslaan en beheren van grote hoeveelheden gegevens, geeft Apache Solr de zoekmogelijkheden die snel de gegevens te herstellen. 

## <a name="install-solr-using-portal"></a>Installeren met behulp van portal Solr

1. Beginnen met het maken van een cluster met de optie **Aangepast maken** zoals beschreven bij het [maken van Hadoop clusters in het HDInsight](hdinsight-provision-clusters.md#portal).
2. Op de pagina **Scriptacties** van de wizard, klikt u op **scriptactie toevoegen** voor meer informatie over de scriptactie zoals hieronder wordt weergegeven:

    ![Gebruik scriptactie voor het aanpassen van een cluster] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Gebruik scriptactie voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie. Bijvoorbeeld <b>Solr installeren</b>.</td></tr>
        <tr><td>URI-script</td>
            <td>Geef de URI Uniform Resource Identifier () aan het script dat wordt aangeroepen voor het aanpassen van het cluster. Bijvoorbeeld: <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Knooppunttype</td>
            <td>Geef de knooppunten waarop het aangepaste script wordt uitgevoerd. U kunt <b>alle knooppunten</b>, <b>alleen knooppunten Head</b>of <b>alleen knooppunten werknemer</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script. Het script Solr installeren vereist geen parameters, zodat u kunt deze leeg laten.</td></tr>
    </table>

    U kunt meer dan één scriptactie om meerdere onderdelen op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het maken van het cluster.


## <a name="use-solr"></a>Solr gebruiken

U moet beginnen met Solr met de gegevensbestanden indexeren. U kunt vervolgens Solr zoekopdrachten uitvoeren op de geïndexeerde gegevens. Voer de volgende stappen uit voor het gebruik van Solr in een cluster van HDInsight:

1. **Gebruik Remote Desktop Protocol (RDP) op afstand in het cluster HDInsight met Solr geïnstalleerd**. Vanaf de portal Azure, moet u extern bureaublad inschakelen voor het cluster dat u hebt gemaakt met Solr geïnstalleerd en vervolgens extern in het cluster. Zie [verbinding maken met clusters van HDInsight met RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)voor instructies.

2. **Index Solr door het uploaden van bestanden**. Bij het indexeren van Solr, plaatst u de documenten in dat u wilt zoeken. Solr indexeert, RDP op afstand gebruiken in het cluster, navigeert u naar het bureaublad, opent u de opdrachtregel Hadoop en Ga naar **C:\apps\dist\solr-4.7.2\example\exampledocs**. Voer de volgende opdracht:

        java -jar post.jar solr.xml monitor.xml

    Ziet u de volgende uitvoer op de console:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Het hulpprogramma post.jar indexeert Solr met twee voorbeelddocumenten, **solr.xml** en **monitor.xml**. Het hulpprogramma post.jar en de voorbeelddocumenten zijn beschikbaar bij de installatie van Solr.

3. **Gebruik het dashboard Solr om te zoeken binnen de geïndexeerde documenten**. In de RDP-sessie aan het cluster HDInsight, open Internet Explorer en start het dashboard Solr op **solr-http://headnodehost:8983 / #/**. Selecteer **collection1**in het linkerdeelvenster, klik in de vervolgkeuzelijst **Selector Core** en binnen die, klikt u op **Query**. Als u bijvoorbeeld wilt selecteren en retourneren van alle documenten in Solr, bieden de volgende waarden:

    * Voer in het tekstvak **q** ** \*:**\*. Hiermee herstelt u de documenten die zijn geïndexeerd in Solr. Als u zoeken naar een bepaalde tekenreeks in de documenten wilt, kunt u een tekenreeks voor deze hier invoeren.
    
    * Selecteer in het tekstvak **wt** de uitvoerindeling. De standaardwaarde is **json**. Klik op **Query uitvoeren**.

    ![Gebruik scriptactie voor het aanpassen van een cluster] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Uitvoeren van een query op Solr dashboard")
    
    De uitvoer geeft als resultaat de twee documenten die we hebben gebruikt voor het indexeren van Solr. De uitvoer er ongeveer als volgt:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Aanbevolen: Maak een Back-up van de geïndexeerde gegevens van Solr met Azure Blob-opslag die is gekoppeld aan het cluster HDInsight**. Als een goede gewoonte back u-up de geïndexeerde gegevens van de clusterknooppunten Solr naar Azure Blob-opslag. Doe het volgende doen:

    1. De RDP-sessie, open Internet Explorer en verwijzen naar de volgende URL:

            http://localhost:8983/solr/replication?command=backup

        Hier ziet u een antwoord als volgt:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. Ga in de externe sessie naar {SOLR_HOME}\{collectie} \data. Dit moet voor het cluster met behulp van het voorbeeldscript gemaakt, **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Op deze locatie, wordt er een map met momentopnamen gemaakt met een naam die lijkt op * *momentopname.* tijdstempel***.

    3. De map snapshot ZIP en upload deze naar Azure Blob-opslag. Navigeer naar de locatie van de map snapshot vanaf de opdrachtregel Hadoop met de volgende opdracht:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Met deze opdracht kopieert de momentopname van het /example/data/onder de container in de standaard opslag-account die is gekoppeld aan het cluster.

## <a name="install-solr-using-aure-powershell"></a>Solr met Aure PowerShell installeren

Zie [clusters van HDInsight aanpassen met behulp van de actie Script](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Het voorbeeld laat zien hoe Spark met Azure PowerShell te installeren. U moet het script voor het gebruik van [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)aanpassen.

## <a name="install-solr-using-net-sdk"></a>Solr met .NET SDK installeren

Zie [clusters van HDInsight aanpassen met behulp van de actie Script](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Het voorbeeld laat zien hoe Spark met de .NET SDK installeren. U moet het script voor het gebruik van [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)aanpassen.



## <a name="see-also"></a>Zie ook

- [Installeren en gebruiken van Solr op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Clusters maken Hadoop in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van clusters van HDInsight.
- [Aanpassen HDInsight cluster met behulp van de actie Script][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met actie Script.
- [Ontwikkelen scriptactie scripts voor HDInsight](hdinsight-hadoop-script-actions.md).
- [Installeren en gebruiken van Spark op clusters HDInsight][hdinsight-install-spark]: actie Script voorbeeld over het installeren van Spark.
- [R installeren op clusters HDInsight][hdinsight-install-r]: de actie Script voorbeeld over het installeren van R.
- [Giraph op HDInsight-clusters installeren](hdinsight-hadoop-giraph-install.md): actie Script voorbeeld over het installeren van Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
