<properties
    pageTitle="Tint met Hadoop op HDInsight Linux-clusters gebruiken | Microsoft Azure"
    description="Informatie over het installeren en gebruiken van tint met clusters van Hadoop op HDInsight Linux."
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
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van tint op HDInsight Hadoop-clusters

Informatie over het kleurtoon op HDInsight Linux-clusters installeren en tunneling gebruiken voor het routeren van de aanvragen voor kleurtoon.

## <a name="what-is-hue"></a>Wat is de tint?

Tint is een verzameling van webtoepassingen die worden gebruikt voor interactie met een cluster Hadoop. Tint kunt u de opslag die is gekoppeld aan een cluster Hadoop (WASB, in het geval van HDInsight clusters) bladeren, uitvoeren van taken voor component en scripts voor varkens, enz. De volgende onderdelen zijn beschikbaar bij installaties op een cluster HDInsight Hadoop tint.

* Bijenwas component Editor
* Varken
* Metastore manager
* Oozie
* FileBrowser (die gesprekken voert WASB standaardcontainer)
* De Browser taak

> [AZURE.WARNING] Onderdelen van het cluster HDInsight worden volledig ondersteund en Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze componenten.
>
> Aangepaste onderdelen ontvangen commercieel redelijke ondersteuning waarmee u het probleem verder oplossen. Dit kan leiden tot het oplossen van het probleem of vraag aan een van de beschikbare kanalen voor de open-source technologieën waarbij diepe expertise voor de technologie die wordt gevonden. Er zijn bijvoorbeeld veel sites van de community die kunnen worden gebruikt, zoals: [MSDN forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projecten hebben ook projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Tint met acties Script installeren

Het volgende scriptactie kan tint installeren op een Linux-gebaseerde HDInsight-cluster worden gebruikt.
https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-HUE-uber-v02.sh
    
Deze sectie bevat instructies over het gebruik van het script tijdens het inrichten van het cluster via de Portal Azure. 

> [AZURE.NOTE] Azure PowerShell, de CLI Azure, het HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt script acties worden uitgevoerd. U kunt ook scriptacties toepassen op clusters is gebeurd. Zie [clusters met scriptacties HDInsight aanpassen](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie.

1. Start een cluster wordt ingericht door de stappen in de [voorziening HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), maar Voer inrichten.

    > [AZURE.NOTE] Kleurtoon installeren op HDInsight clusters, het aanbevolen headnode is ten minste A4 (8 cores, 14 GB geheugen).

2. Op het blad **Optionele configuratie** **Scriptacties**selecteren en de gegevens verstrekken, zoals hieronder wordt weergegeven:

    ![Geef scriptparameters actie voor kleurtoon] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Geef scriptparameters actie voor kleurtoon")

    * __Naam__: Geef een beschrijvende naam voor de scriptactie.
    * __SCRIPT-URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Kop__: Schakel dit selectievakje in
    * __Werknemer__: laat dit vak leeg.
    * __ZOOKEEPER__: laat dit vak leeg.
    * __PARAMETERS__: laat dit vak leeg.

3. Gebruik de knop **Selecteer** de configuratie opslaan onderaan het **Scriptacties**. Tot slot de knop **selecteren** onderaan het blad **Optionele configuratie** gebruiken de optionele configuratie-informatie opslaan.

4. Doorgaan met het cluster wordt ingericht als omschreven in [bepaling HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>Tint met HDInsight clusters gebruiken

SSH Tunneling is de enige manier om de kleurtoon in het cluster toegang wanneer deze wordt uitgevoerd. Via SSH tunneling kan het verkeer en Ga rechtstreeks naar de headnode van het cluster waarop tint wordt uitgevoerd. Nadat het cluster wordt ingericht, is voltooid, gebruik de volgende stappen tint in een cluster HDInsight Linux gebruiken.

1. Gebruik de informatie in het [Gebruik SSH Tunneling Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web UI van toegang tot](hdinsight-linux-ambari-ssh-tunnel.md) een SSH-tunnel van uw clientsysteem in het cluster HDInsight maken en vervolgens uw webbrowser de SSH-tunnel gebruikt als een proxy te configureren.

2. Zodra u hebt gemaakt van een SSH-tunnel en de browser op het verkeer via deze proxy geconfigureerd, moet u de hostnaam van het primaire knooppunt hoofd vinden. U kunt dit doen door verbinding te maken met het cluster via SSH op poort 22. Bijvoorbeeld `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` waarbij __gebruikersnaam__ uw gebruikersnaam SSH en __CLUSTERNAAM__ de naam van het cluster is.

    Zie de volgende documenten voor meer informatie over het gebruik van SSH:

    * [SSH gebruiken met Linux-gebaseerde HDInsight van een client voor Linux, Unix, of Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH met Linux-gebaseerde HDInsight van een Windows-client gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Wanneer een verbinding, gebruik de volgende opdracht de FQDN-naam van de primaire headnode verkrijgen:

        hostname -f

    Het resultaat is een naam met de volgende strekking:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Dit is de hostnaam van de primaire headnode waar de kleurtoon website zich bevindt.

2. Gebruik de browser te openen de kleurtoon-portal op http://HOSTNAME:8888. Host-naam vervangen door de naam die u hebt verkregen in de vorige stap.

    > [AZURE.NOTE] Wanneer u zich voor de eerste keer aanmeldt, wordt u gevraagd voor het maken van een account aan te melden bij de portal tint. De referenties die u hier opgeeft, zal worden beperkt tot de portal en zijn niet gerelateerd aan de admin of SSH gebruikersreferenties opgegeven terwijl bepaling het cluster.

    ![Meld u aan bij de kleurtoon-portal] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Geef de referenties voor kleurtoon portal")

### <a name="run-a-hive-query"></a>Een component uitvoeren

1. Klik op **Query Editors**vanaf de portal tint en klik vervolgens op **component** om de component-editor te openen.

    ![Gebruik de component] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Gebruik de component")

2. Klik op het tabblad **staan** onder **Database**ziet u **hivesampletable**. Dit is een tabel die wordt geleverd bij alle Hadoop clusters op HDInsight. Voer een voorbeeldquery in het rechterdeelvenster en de uitvoer weergegeven op het tabblad **resultaten** in het deelvenster, zoals wordt weergegeven in het scherm vastleggen.

    ![Query uitvoeren component] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Query uitvoeren component")

    U kunt ook het tabblad **grafiek** een visuele weergave van het resultaat zien.

### <a name="browse-the-cluster-storage"></a>De clusteropslag bladeren

1. Klik vanuit de portal tint **Bestandsbrowser** in de rechterbovenhoek van de menubalk.

2. De bestandsbrowser wordt standaard geopend in de map **/user/myuser** . Klik op de schuine streep na de gebruikerslijst in het pad naar de hoofdmap van de Azure opslag container die is gekoppeld aan het cluster.

    ![De bestandsbrowser gebruiken] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "De bestandsbrowser gebruiken")

3. Klik met de rechtermuisknop op een bestand of map voor een overzicht van de beschikbare bewerkingen. Gebruik de knop **uploaden** in de rechterhoek om bestanden te uploaden naar de huidige map. De knop **Nieuw** gebruik te maken van nieuwe bestanden of mappen.

> [AZURE.NOTE] De bestandsbrowser tint kan alleen de inhoud van de standaardcontainer die is gekoppeld aan het cluster HDInsight weergeven. Geen extra opslagruimte accounts/containers die u mogelijk hebt gekoppeld aan het cluster meer niet toegankelijk via de browser. Echter wordt de extra containers die zijn gekoppeld aan het cluster altijd toegankelijk zijn voor de taken van de component. Als u de opdracht typt bijvoorbeeld `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` in de component-editor kunt u de inhoud van andere containers ook bekijken. In deze opdracht is **newcontainer** niet de standaardcontainer die is gekoppeld aan een cluster.

## <a name="important-considerations"></a>Belangrijke overwegingen

1. Het script gebruikt voor de installatie van de kleurtoon geïnstalleerd op de primaire headnode van het cluster.

2. Tijdens de installatie worden meerdere Hadoop services (HDFS, GAREN, MR2, Oozie) voor het bijwerken van de configuratie opnieuw gestart. Zodra het script is voltooid installeren kleurtoon, duurt het enige tijd voor de andere Hadoop-services worden gestart. Dit kan in eerste instantie tint van prestaties beïnvloeden. Nadat alle services worden gestart, wordt tint volledig functioneel zijn.

3.  Kleurtoon wordt ondersteund Tez taken, de huidige standaard voor de component. Als u wilt MapReduce gebruiken als de component execution engine update het script voor het gebruik van de volgende opdracht in het script:

        set hive.execution.engine=mr;

4.  Met clusters van Linux hebt u een scenario waar uw services worden uitgevoerd op de primaire headnode terwijl de bronnenbeheerder kan worden uitgevoerd op de secundaire server. Deze situatie kan leiden tot fouten (Zie hieronder) wanneer u tint om details te bekijken van taken uitvoeren op het cluster. U kunt echter de taakdetails bekijken wanneer de taak is voltooid.

    ![Kleurtoon portal fout] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Kleurtoon portal fout")

    Dit is een bekend probleem. Als tijdelijke oplossing Ambari zodanig wijzigen dat ook de actieve Resource Manager wordt uitgevoerd op de primaire headnode.

5.  Kleurtoon heeft inzicht in de WebHDFS terwijl HDInsight clusters met Azure opslag met behulp van `wasbs://`. Het aangepaste script gebruikt in combinatie met de scriptactie installeert, WebWasb, een WebHDFS-compatibele service voor het gesprek met de WASB. Dus zelfs als de kleurtoon portal zegt HDFS in openbare ruimten (zoals wanneer u de muis boven de **Bestandsbrowser verplaatst**), moet dit worden beschouwd als WASB.


## <a name="next-steps"></a>Volgende stappen

- [Giraph op HDInsight-clusters installeren](hdinsight-hadoop-giraph-install-linux.md). Giraph op HDInsight Hadoop-clusters installeren via cluster aanpassen. Giraph, kunt u de grafiek verwerken met behulp van Hadoop en kan worden gebruikt met Azure HDInsight.

- [Solr op HDInsight clusters installeren](hdinsight-hadoop-solr-install-linux.md). Solr op HDInsight Hadoop-clusters installeren via cluster aanpassen. Solr kunt u krachtige zoekfunctie bewerkingen uitvoeren op opgeslagen gegevens.

- [R op HDInsight clusters installeren](hdinsight-hadoop-r-scripts-linux.md). R op HDInsight Hadoop-clusters installeren via cluster aanpassen. R is een open source-taal en omgeving voor statistische berekeningen. Biedt honderden ingebouwde statistische functies en een eigen programmeertaal die aspecten van functionele en object-georiënteerd programmeren combineert. Het biedt ook uitgebreide grafische mogelijkheden.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
