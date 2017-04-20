<properties
   pageTitle="Tips voor het gebruik van Hadoop op Linux-gebaseerde HDInsight | Microsoft Azure"
   description="Implementatie tips voor het gebruik van clusters van Linux-gebaseerde HDInsight (Hadoop) op een bekende Linux-omgeving uitgevoerd in de Azure cloud."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Informatie over het gebruik van HDInsight op Linux

Linux-gebaseerde HDInsight Azure clusters bieden Hadoop op een bekende omgeving voor Linux, uitgevoerd in de Azure cloud. Voor de meeste zaken, moet deze werken hetzelfde als elke andere Hadoop op Linux-installatie. Dit document roept uit rekening houden met moet specifieke verschillen.

##<a name="prerequisites"></a>Vereisten

Veel van de stappen in dit document gebruikt u de volgende hulpprogramma's, die mogelijk moeten worden geïnstalleerd op uw systeem.

* [cURL](https://curl.haxx.se/) - gebruikt om te communiceren met webservices
* [jq](https://stedolan.github.io/jq/) - gebruikt bij het parseren van JSON-documenten
* [Azure CLI](../xplat-cli-install.md) - Azure services extern beheren

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Domeinnamen

De volledig gekwalificeerde domeinnaam (FQDN) te gebruiken wanneer u verbinding maakt met het cluster via het internet is ** &lt;Clusternaam >. azurehdinsight.net** of (SSH) ** &lt;clusternaam-ssh >. azurehdinsight.net**.

Intern, heeft elk knooppunt in het cluster een naam die is toegewezen tijdens de configuratie van het cluster. Om de clusternamen, kunt u __Hosts__ bezoeken op het Web Ambari UI of gebruikt u het volgende om een lijst van hosts uit het Ambari REST API:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Vervang __wachtwoord__ door het wachtwoord van de beheerdersaccount en de __CLUSTERNAAM__ de naam van het cluster. Het resultaat is een JSON-document dat een lijst met de hosts in het cluster bevat en klik op jq wordt getrokken uit de `host_name` de waarde van het element voor elke host in het cluster.

Als u zoeken naar de naam van het knooppunt voor een bepaalde service wilt, kunt u Ambari voor het desbetreffende onderdeel opvragen. Gebruik het volgende om de hosts voor het knooppunt HDFS naam.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Hiermee wordt een JSON-document met een beschrijving van de service en vervolgens jq wordt getrokken om alleen de `host_name` de waarde voor de hosts.

## <a name="remote-access-to-services"></a>Externe toegang tot services

* **Ambari (web)** - https://&lt;Clusternaam >. azurehdinsight.net

    Verifiëren met behulp van de cluster-beheerder en het wachtwoord en vervolgens aanmelden bij Ambari. Dit wordt ook gebruikt de cluster-beheerder en het wachtwoord.

    Verificatie wordt als leesbare tekst - altijd HTTPS gebruiken om ervoor te zorgen dat de verbinding veilig is.

    > [AZURE.IMPORTANT] Terwijl de Ambari voor uw cluster rechtstreeks via Internet toegankelijk is, afhankelijk van bepaalde functionaliteit knooppunten benaderen door de interne domeinnaam die wordt gebruikt door het cluster. Aangezien dit een interne domeinnaam is en niet openbaar is, u fouten 'server niet gevonden' ontvangt wanneer u probeert toegang te krijgen tot bepaalde functies via het Internet.
    >
    > Als u de volledige functionaliteit van het web Ambari UI, gebruiken een SSH-tunnel naar proxy webverkeer naar het hoofd clusterknooppunt. Zie [Gebruik SSH Tunneling Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web UI van toegang tot](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://&lt;Clusternaam >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Verifiëren met behulp van de cluster-beheerder en het wachtwoord.
    >
    > Verificatie wordt als leesbare tekst - altijd HTTPS gebruiken om ervoor te zorgen dat de verbinding veilig is.

* **WebHCat (Templeton)** - https://&lt;Clusternaam >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Verifiëren met behulp van de cluster-beheerder en het wachtwoord.
    >
    > Verificatie wordt als leesbare tekst - altijd HTTPS gebruiken om ervoor te zorgen dat de verbinding veilig is.

* **SSH** - &lt;Clusternaam >-ssh.azurehdinsight.net op poort 22 of 23. Verbinding maken met de primaire headnode, terwijl 23 wordt gebruikt voor verbinding met de secundaire poort 22 gebruikt. Zie voor meer informatie over de hoofd-knooppunten, [beschikbaarheid en betrouwbaarheid van Hadoop clusters in het HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] U kunt alleen toegang tot de statutaire clusterknooppunten via SSH vanaf een clientcomputer. Wanneer een verbinding, kunt u met behulp van SSH in een headnode werknemer knooppunten benaderen.

## <a name="file-locations"></a>Bestandslocaties

Hadoop-bestanden kunnen u vinden op de clusterknooppunten op `/usr/hdp`. Deze map bevat de volgende submappen:

* __2.2.4.9-1__: deze map met de naam van de versie van het Platform voor Hortonworks gebruikt door HDInsight, zodat het nummer op het cluster andere dan de hier vermelde.
* __huidige__: deze map bevat koppelingen naar de mappen onder de map __2.2.4.9-1__ en ervoor te zorgen dat u niet hoeft te typen een versienummer (die kan veranderen), telkens wanneer u toegang wilt tot een bestand.

Van de voorbeeldgegevens en JAR-bestanden kunnen u vinden op Hadoop Distributed bestand System (HDFS) of Azure Blob storage op ' / voorbeeld ' of ' wasbs: / / / voorbeeld '.

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, Azure Blob-opslag en opslag-aanbevolen procedures

HDFS wordt in de meeste distributies Hadoop ondersteund door lokale opslag op de computers in het cluster. Dit is een efficiënte, kan het zijn kostbare voor een cloud-gebaseerde oplossing waar u elk uur of minuut voor compute resources zijn belast.

Azure Blob-opslag HDInsight gebruikt als de standaardarchief de volgende voordelen biedt:

* Goedkope opslag op lange termijn

* Toegankelijkheid van externe services, zoals websites, hulpprogramma's voor bestand uploaden/downloaden van verschillende taal-SDK's en webbrowsers

Omdat de standaardopslag voor HDInsight, hebt u gewoonlijk niet te ondernemen om het te gebruiken. De volgende opdracht wordt bijvoorbeeld een lijst met bestanden in de map **/example/data** , die is opgeslagen op Azure Blob-opslag:

    hdfs dfs -ls /example/data

Sommige opdrachten moet u mogelijk opgeven dat u Blob-opslag gebruikt. Voor deze, als voorvoegsel voor de opdracht en de **wasb: / /**, of **wasbs: / /**.

HDInsight kunt u meerdere Blob storage accounts koppelen aan een cluster. Voor toegang tot gegevens op een niet-standaard Blob storage-account, kunt u de indeling **wasbs: / /&lt;container-name>@&lt;-accountnaam >.blob.core.windows.net/**. Bijvoorbeeld, de volgende wordt een lijst met de inhoud van de map **/example/data** voor de opgegeven container en Blob storage account:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Welke Blob-opslag wordt gebruikt voor het cluster?

U hebt geselecteerd tijdens het maken van het cluster aan met behulp van een bestaande opslag Azure-account en een container of maak een nieuwe. Vervolgens, u waarschijnlijk vergeten over het. Vindt u de standaardaccount voor de opslag en de container met de API Ambari REST.

1. Gebruik de volgende opdracht op te halen configuratiegegevens HDFS met krul en filteren met de [jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Hiermee herstelt u de eerste configuratie is toegepast op de server (`service_config_version=1`,) die deze gegevens bevat. Als u een waarde die is gewijzigd na het maken van het cluster ophaalt, moet u de versies van de configuratie en de meest recente versie op te halen.

    Deze waarde als resultaat het volgende, waarbij __CONTAINER__ de standaardcontainer en __accountnaam__ de accountnaam Azure opslag:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Ophalen van de resourcegroep voor de opslag, de [Azure CLI](../xplat-cli-install.md)gebruiken. De opslag naam opgehaald van de Ambari vervangen __accountnaam__ in de volgende opdracht:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Hiermee herstelt u de naam van de resource voor de account.
    
    > [AZURE.NOTE] Als er niets door deze opdracht wordt geretourneerd, moet u de CLI Azure Azure Resource Manager modus wijzigen en voert u de opdracht opnieuw. Gebruik de volgende opdracht als u wilt overschakelen naar de modus Azure Resource Manager.
    >
    > `azure config mode arm`
    
2. De sleutel voor de opslag krijgen. __GROUPNAME__ vervangen door de resourcegroep van de vorige stap. __Accountnaam__ vervangen door de naam van de opslag:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Hiermee herstelt u de primaire sleutel voor de account.

U kunt ook de opslaggegevens met behulp van de Portal Azure vinden:

1. Selecteer het cluster HDInsight in [Azure Portal](https://portal.azure.com/).

2. Selecteer __alle instellingen__in de sectie __Essentials__ .

3. Selecteer __Instellingen__, __Azure opslag sleutels__.

4. __Azure opslag sleutels__, selecteer een van de opslag rekeningen. Hiermee wordt informatie over de account van de opslag weergegeven.

5. Selecteer het pictogram van de sleutel. Toetsen voor deze account opslag wordt weergegeven.

### <a name="how-do-i-access-blob-storage"></a>Hoe krijg ik toegang tot Blob-opslag?

Anders dan via de opdracht Hadoop uit het cluster zijn er tal van manieren toegang krijgen tot de BLOB's:

* [CLI voor Mac, Linux en Windows Azure](../xplat-cli-install.md): opdrachtregelinterface opdrachten voor het werken met Azure. Na het installeren, gebruiken de `azure storage` voor meer informatie over het gebruik van opslag, opdracht of `azure blob` voor blob-specifieke opdrachten.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): een python-script voor het werken met blobs in Azure opslag.

* De diverse SDK's:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Opslag REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Schaal van het cluster

Het cluster functie schalen kunt u het aantal gegevensknooppunten dat die worden gebruikt door een cluster wordt uitgevoerd in Azure HDInsight zonder te verwijderen en opnieuw maken van het cluster wijzigen.

U kunt bewerkingen schaal terwijl u andere taken of processen worden uitgevoerd op een servercluster.

De verschillende clustertypen worden beïnvloed door het schalen als volgt:

* __Hadoop__: bij het verkleinen van het aantal knooppunten in een cluster, sommige van de in het cluster-services opnieuw zijn gestart. Hierdoor kunnen taken actief of in behandeling aan het einde van de schaal bewerking mislukt. Zodra de bewerking voltooid is, kunt u de taken opnieuw indienen.

* __HBase__: regionale servers automatisch binnen enkele minuten na de voltooiing van de bewerking schalen zijn in evenwicht. Handmatig evenwichtige regionale servers, gebruikt u de volgende stappen uit:

    1. Verbinding maken met het HDInsight-cluster met behulp van SSH. Zie voor meer informatie over het gebruik van SSH in HDInsight, een van de volgende documenten:

        * [SSH gebruiken met HDInsight van Linux, Unix en Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [SSH met HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. De HBase shell te starten, gebruikt u het volgende:

            hbase shell

    2. Nadat de shell HBase is geladen, gebruikt u het volgende om de regionale servers handmatig in balans:

            balancer

* __Storm__: U moet een actieve Storm topologieën opnieuw nadat een schaal bewerking is uitgevoerd. Hierdoor wordt de topologie past parallellisme instellingen op basis van het nieuwe aantal knooppunten in het cluster. Om actieve topologieën vastleggen, gebruikt u een van de volgende opties:

    * __SSH__: verbinding maken met de server en opnieuw een topologie met de volgende opdracht:

            storm rebalance TOPOLOGYNAME

        Ook kunt u parameters negeren de parallellisme hints oorspronkelijk geleverd door de topologie. Bijvoorbeeld, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` wordt de topologie op 5 werkprocessen en 3 executors voor de component blauw spout 10 executors voor het onderdeel geel bout configureren.

    * __Storm UI__: de volgende stappen gebruiken om opnieuw een topologie met behulp van de gebruikersinterface van de Storm.

        1. Open __https://CLUSTERNAME.azurehdinsight.net/stormui__ in uw webbrowser, waarbij de CLUSTERNAAM de naam van het cluster Storm is. Typ desgevraagd de naam HDInsight cluster-beheerder (admin) en wachtwoord die u hebt opgegeven bij het maken van het cluster.

        3. De topologie die u wilt vastleggen selecteren en selecteer vervolgens de knop __opnieuw__ . Geef de vertraging voordat het rebalance-bewerking wordt uitgevoerd.

Zie voor specifieke informatie op uw cluster HDInsight schalen:

* [Hadoop clusters in het HDInsight beheren via de Portal Azure](hdinsight-administer-use-portal-linux.md#scaling)

* [Hadoop clusters in HDinsight met Azure PowerShell beheren](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hoe installeer ik kleurtoon (of een andere component Hadoop)

HDInsight is een beheerde service, wat betekent dat knooppunten in een cluster kunnen worden vernietigd en door automatisch de service door Azure als er een probleem is aangetroffen. Daarom is het niet aanbevolen dingen handmatig installeren op clusterknooppunten. In plaats daarvan gebruik [HDInsight scriptacties](hdinsight-hadoop-customize-cluster.md) moet u het volgende installeren:

* Een service of website zoals Spark of tint.
* Een component die worden aangebracht op meerdere knooppunten in het cluster moet. Bijvoorbeeld een variabele vereiste omgeving maken van een map voor logboekregistratie of het maken van een configuratiebestand.

Scriptacties zijn Bash scripts die worden uitgevoerd tijdens het inrichten van de cluster en kunnen worden gebruikt voor het installeren en configureren van extra onderdelen op het cluster. Voorbeeldscripts zijn voor het installeren van de volgende onderdelen beschikbaar:

* [Tint](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Zie voor meer informatie over het ontwikkelen van uw eigen acties Script [scriptactie ontwikkelen met HDInsight](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>JAR-bestanden

Sommige Hadoop-technologieën zijn beschikbaar in zelfstandige jar-bestanden die worden gebruikt als onderdeel van een project MapReduce, of van functies bevatten binnen varken of component. Terwijl deze kunnen worden geïnstalleerd met behulp van scriptacties, kunnen ze vaak is niet vereist voor de installatie en alleen worden geüpload naar het cluster na het inrichten en gebruikt rechtstreeks. Als u controleren of dat het onderdeel blijft van het cluster reimaging makle wilt, kunt u het jar-bestand opslaan in WASB.

Bijvoorbeeld, als u gebruiken de nieuwste versie van [DataFu wilt](http://datafu.incubator.apache.org/), kunt u downloaden een jar met het project en upload het naar het cluster HDInsight. Volg de documentatie van de DataFu op het gebruik van varkens of component.

> [AZURE.IMPORTANT] Sommige onderdelen zelfstandige jar-bestanden zijn worden geleverd met HDInsight, maar niet in het pad. Als u een bepaald onderdeel zoekt, kunt u het volgen om te zoeken op het cluster:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Hiermee herstelt u het pad naar een overeenkomende jar-bestanden.

Als het cluster al een versie van een onderdeel als een zelfstandige jar-bestand bevat, maar u wilt een andere versie te gebruiken, kunt u een nieuwe versie van het onderdeel uploaden naar het cluster en gebruik in uw projecten.

> [AZURE.WARNING] Onderdelen van het cluster HDInsight worden volledig ondersteund en Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze componenten.
>
> Aangepaste onderdelen ontvangen commercieel redelijke ondersteuning waarmee u het probleem verder oplossen. Dit kan leiden tot het oplossen van het probleem of vraag aan een van de beschikbare kanalen voor de open-source technologieën waarbij diepe expertise voor de technologie die wordt gevonden. Er zijn bijvoorbeeld veel sites van de community die kunnen worden gebruikt, zoals: [MSDN forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projecten hebben ook projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Volgende stappen

* [Migreren van HDInsight op basis van Windows naar Linux-gebaseerde](hdinsight-migrate-from-windows-to-linux.md)
* [Gebruik component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
