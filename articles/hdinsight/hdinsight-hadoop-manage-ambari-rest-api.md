<properties
   pageTitle="Bewaken en beheren van clusters van HDInsight met de Apache Ambari REST API | Microsoft Azure"
   description="Informatie over het controleren en beheren van HDInsight Linux gebaseerde clusters met Ambari. In dit document leert u hoe u met de Ambari REST API opgenomen met clusters van HDInsight."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>HDInsight clusters beheren met behulp van de API Ambari REST

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereenvoudigt het beheer en de controle van een cluster Hadoop door middel van een eenvoudig te gebruiken web UI en REST API. Ambari op HDInsight op basis van Linux-clusters is opgenomen en wordt gebruikt voor het cluster controleren en configuratiewijzigingen aanbrengen. In dit document leert u de basisbeginselen van het werken met de API Ambari REST door het uitvoeren van veelvoorkomende taken met krul.

##<a name="prerequisites"></a>Vereisten

* [cURL](http://curl.haxx.se/): cURL is een cross-platform-hulpprogramma dat kan worden gebruikt om te werken met REST API's vanaf de opdrachtregel. In dit document, wordt deze gebruikt om te communiceren met de API Ambari REST.
* [jq](https://stedolan.github.io/jq/): jq is een opdrachtregelprogramma van cross-platform voor het werken met JSON-documenten. In dit document wordt gebruikt de geretourneerde van de API Ambari REST JSON-documenten te parseren.
* [Azure CLI](../xplat-cli-install.md): een cross-platform opdrachtregelprogramma voor het werken met Azure services.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Wat is Ambari?

[Ambari van Apache](http://ambari.apache.org) is Hadoop beheer eenvoudiger door middel van een eenvoudig te gebruiken web UI die kan worden gebruikt voor het inrichten, beheren en controleren van clusters van Hadoop. Ontwikkelaars van kunnen deze mogelijkheden integreren in hun toepassingen met behulp van de [Ambari REST API's](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari krijgt u standaard met HDInsight Linux gebaseerde clusters.

##<a name="rest-api"></a>REST-API

De basis-URI voor de API Ambari REST van HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, waarbij de __CLUSTERNAAM__ de naam van het cluster is.

> [AZURE.IMPORTANT] De naam van het cluster in de FQDN-naam (Fully Qualified Domain Name) van de URI (CLUSTERNAME.azurehdinsight.net) is niet hoofdlettergevoelig, zijn andere voorvallen in de URI hoofdlettergevoelig. Als het cluster met de naam mijncluster, zijn de volgende bijvoorbeeld geldige URI's:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> De volgende URI retourneert een fout omdat het tweede exemplaar van de naam is niet de juiste case.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Verbinding maken met de Ambari op HDInsight vereist HTTPS. Bij de verificatie van de verbinding, moet u de admin-accountnaam (standaard is __admin__) en wachtwoord die u hebt opgegeven bij het maken van het cluster.

Krul wordt het volgende voorbeeld een GET-verzoek ten opzichte van de REST API. __Wachtwoord__ vervangen door de admin-wachtwoord voor de cluster. __CLUSTERNAAM__ vervangen door de naam van het cluster:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Het antwoord is een JSON-document die met de volgende informatie begint:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Aangezien dit JSON, is het eenvoudiger een JSON parser gebruiken om te werken met de gegevens. Bijvoorbeeld: in het volgende voorbeeld jq gebruikt om weer te geven alleen de `health_report` element.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Voorbeeld: De FQDN van de knooppunten van het cluster ophalen

Bij het werken met HDInsight, wellicht moet u de FQDN-naam (Fully Qualified Domain Name) van een clusterknooppunt weet. U kunt de FQDN-naam voor de verschillende knooppunten in het cluster met behulp van de volgende terugvinden:

* __Hoofd knooppunten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Werknemer knooppunten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper knooppunten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Opmerking dat elk van deze voorbeelden hetzelfde patroon volgen:

1. Een onderdeel dat we weten dat query wordt uitgevoerd op die knooppunten.
2. Ophalen van de `host_name` -elementen, die de FQDN-naam voor deze knooppunten bevatten.

De `host_components` van het resulterende document-element bevat meerdere items. Met behulp van `.host_components[]`, en vervolgens een pad in het element op te geven wordt elk object doorlopen en de waarde van het specifieke pad uitlichten. Als u alleen een waarde, zoals de eerste vermelding van de FQDN-naam kunt u de items als een verzameling terug en selecteer een specifieke vermelding:

    jq '[.host_components[].HostRoles.host_name][0]'

Hiermee wordt de eerste FQDN-naam van de collectie.

##<a name="example-get-the-default-storage-account-and-container"></a>Voorbeeld: Het standaardaccount voor opslag en de container ophalen

Wanneer u een cluster HDInsight maakt, moet u een Account Azure opslag en een blob-container als de standaard opslag voor het cluster. Ambari kunt u deze informatie ophalen nadat u het cluster hebt gemaakt. Bijvoorbeeld, als u via programmacode wilt schrijven gegevens rechtstreeks naar de container.

De volgende haalt de WASB URI van de opslag van de standaard clusters:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Hiermee wordt de eerste configuratie is toegepast op de server (`service_config_version=1`,) die deze gegevens bevat. Als u een waarde die is gewijzigd na het maken van het cluster ophaalt, moet u de versies van de configuratie en de meest recente versie op te halen.

Dit geeft als resultaat een waarde vergelijkbaar met het volgende voorbeeld, waarbij __CONTAINER__ de standaardcontainer en __accountnaam__ de accountnaam Azure opslag:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Vervolgens kunt u deze gegevens met de [Azure CLI](../xplat-cli-install.md) te uploaden of downloaden van gegevens uit de container.

1. De resourcegroep voor de opslag krijgen. __Accountnaam__ vervangen door de opslag naam opgehaald van Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Hiermee wordt de naam van de resource voor de account.
    
    > [AZURE.NOTE] Als er niets door deze opdracht wordt geretourneerd, moet u de CLI Azure Azure Resource Manager modus wijzigen en voert u de opdracht opnieuw. Als u wilt overschakelen naar de bronnenbeheerder Azure, gebruik de volgende opdracht:
    >
    > `azure config mode arm`
    
2. De sleutel voor de opslag krijgen. __GROUPNAME__ vervangen door de resourcegroep van de vorige stap. __Accountnaam__ vervangen door de naam van de opslag:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    In het volgende voorbeeld wordt de primaire sleutel voor de account.
    
3. Gebruik de upload-opdracht voor het opslaan van een bestand in de container:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    __Accountnaam__ vervangen door de naam van de opslag. __ACCOUNTKEY__ vervangen door de sleutel eerder opgehaald. __BESTANDSPAD__ is het pad naar het bestand dat u uploaden wilt, terwijl __BLOBPATH__ het pad in de container is.

    Als u wilt dat het bestand weergegeven in HDInsight op wasbs://example/data/filename.txt, vervolgens __BLOBPATH__ zou bijvoorbeeld `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Voorbeeld: Ambari Update configuratie

1. Ophalen van de huidige configuratie, Ambari worden opgeslagen als de "gewenste"configuratie:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    In het volgende voorbeeld wordt een JSON-document met de huidige configuratie (aangeduid met een waarde van het _label_ ) voor de onderdelen die zijn geïnstalleerd op het cluster. In het volgende voorbeeld is een fragment uit de ontvangen gegevens van een vonk clustertype.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Uit deze lijst, moet u de naam van het onderdeel te kopiëren (bijvoorbeeld __spark\_thrift\_sparkconf__ en de waarde van het __label__ .
    
2. De configuratie voor het onderdeel en het label met de volgende opdracht ophalen. __Spark-thrift-sparkconf__ en __eerste__ vervangen door het onderdeel en de code die u wilt ophalen van de configuratie voor.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Krul de JSON-document opgehaald en vervolgens jq kunt u wijzigingen aanbrengen in de gegevens wilt maken van een sjabloon. De sjabloon wordt vervolgens gebruikt voor het toevoegen/wijzigen van waarden van de systeemconfiguratie. Specifiek wordt het volgende uitgevoerd:
    
    * Hiermee maakt u een unieke waarde die de tekenreeks 'versie' en de datum die is opgeslagen in __newtag__.
    * Maakt een document root voor de nieuwe gewenste configuratie.
    * Met deze eigenschap wordt de inhoud van de `.items[]` array en voegt u deze toe onder het element __desired_config__ .
    * De __href__, __versie__en __Config__ -elementen worden verwijderd als deze elementen niet nodig zijn voor het indienen van een nieuwe configuratie.
    * Voegt een nieuw __tag__ -element en wordt de waarde ingesteld op __versie nummer__. Het numerieke gedeelte is gebaseerd op de huidige datum. Elke configuratie moet een unieke code hebben.
    
    Ten slotte, de gegevens worden opgeslagen in het document __newconfig.json__ . De documentstructuur moet worden weergegeven zoals in het volgende voorbeeld:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Open de __newconfig.json__ document en wijzigen/toevoegen de waarden in het __properties__ -object. Het volgende voorbeeld wordt de waarde van __"spark.yarn.am.memory"__ van __"1 g"__ naar __"3 g"__ en, en voegt een nieuw element voor __'spark.kryoserializer.buffer.max'__ met een waarde van __"256 m"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Sla het bestand op als u klaar bent met alle wijzigingen.

4. Gebruik de volgende opdracht in te dienen de bijgewerkte configuratie van Ambari.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Met deze opdracht buizen de inhoud van het bestand __newconfig.json__ naar de krul aanvraag die naar het cluster als de gewenste configuratie voor nieuwe verstuurt. De aanvraag krul geeft als resultaat een JSON-document. Het element __versionTag__ in dit document moet overeenkomen met de versie die u hebt ingediend en het object __configuraties__ bevat wijzigingen in de configuratie die u hebt aangevraagd.

###<a name="example-restart-a-service-component"></a>Voorbeeld: Een onderdeel van de service opnieuw starten

Op dit moment als u naar het web Ambari UI kijkt, de vonk service geeft aan dat het moet opnieuw worden opgestart voordat de nieuwe configuratie van kracht. Gebruik de volgende stappen uit om de service opnieuw starten.

1. Gebruik de volgende onderhoudsmodus voor de Spark-service in te schakelen:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Met deze opdracht een JSON-document verzendt naar de server (die zich de `echo` -instructie) die in deze modus wordt ingeschakeld.
    U kunt controleren of de service is nu in de onderhoudsmodus met behulp van de volgende aanvraag:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Dit resulteert in een waarde van `"ON"`.

3. Vervolgens gebruikt het volgende om de service uit te schakelen:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Met deze opdracht geeft als resultaat een antwoord met de volgende strekking.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    De `href` waarde die wordt geretourneerd door deze URI met behulp van het interne IP-adres van het clusterknooppunt. Om dit te gebruiken van buiten het cluster, kunt u het gedeelte '10.0.0.18:8080' vervangen door de FQDN-naam van het cluster. Bijvoorbeeld haalt de volgende opdracht de status van de aanvraag.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Dit geeft een waarde van `"COMPLETED"` en vervolgens de aanvraag is voltooid.

4. Zodra een vorige aanvraag is voltooid, gebruikt u de volgende om de service te starten.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Zodra de service opnieuw wordt gestart, is de nieuwe configuratie-instellingen.

5. Ten slotte, gebruikt u de volgende onderhoudsmodus uit te schakelen.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Volgende stappen

Zie voor een volledig overzicht van de REST API [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Sommige Ambari-functionaliteit is uitgeschakeld, zoals het wordt beheerd door de HDInsight cloud service; bijvoorbeeld, toevoegen of verwijderen van hosts uit het cluster of nieuwe services toevoegen.
