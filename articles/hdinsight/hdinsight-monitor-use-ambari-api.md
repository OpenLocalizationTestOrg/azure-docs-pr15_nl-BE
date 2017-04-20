<properties
    pageTitle="Hadoop clusters in HDInsight met de Ambari-API controleren | Microsoft Azure"
    description="Gebruik de Apache Ambari APIs voor het maken, beheren en controleren van clusters van Hadoop. Verbergt de complexiteit van Hadoop operator intuïtieve hulpmiddelen en API's."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Hadoop clusters in HDInsight met de Ambari-API controleren

Informatie over het controleren van HDInsight clusters in Ambari APIs.

> [AZURE.NOTE] De informatie in dit artikel is voornamelijk bedoeld voor HDInsight Windows-gebaseerde clusters een alleen-lezen versie van de API Ambari REST bieden. Voor Linux gebaseerde clusters Zie [clusters van Hadoop beheren met behulp van Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Wat is Ambari?

[Apache-Ambari] [ ambari-home] wordt gebruikt voor het inrichten, beheren en controleren van clusters van Apache Hadoop. Het bevat een intuïtieve verzameling hulpprogramma's de operator en een robuuste set van API's die de complexiteit van Hadoop, vereenvoudiging van de werking van clusters te verbergen. Zie voor meer informatie over de API [Ambari API Reference][ambari-api-reference]. 

HDInsight ondersteunt momenteel alleen de controlefunctie Ambari. Ambari API 1.0 wordt ondersteund door HDInsight versie 3.0 en 2.1 clusters. Dit artikel heeft betrekking op toegang tot Ambari APIs HDInsight versie 3.1 en 2.1 clusters. Het belangrijkste verschil tussen de twee is dat sommige onderdelen zijn veranderd met de introductie van nieuwe mogelijkheden (bijvoorbeeld de taakserver geschiedenis). 

**Vereisten**

Voordat u deze zelfstudie hebt u het volgende:

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Optioneel) [cURL] [curl]. Zie [cURL-Releases en Downloads]om deze te installeren,[curl-download].

    >[AZURE.NOTE] Wanneer de opdracht krul in Windows, gebruik dubbele aanhalingstekens in plaats van enkele aanhalingstekens voor de optiewaarden gebruiken.

- **Een Azure HDInsight cluster**. Zie voor meer informatie over het cluster beschikbaar [aan de slag met HDInsight] [ hdinsight-get-started] of [clusters bepaling HDInsight][hdinsight-provision]. U moet de volgende gegevens om de zelfstudie doorlopen:

    Cluster, eigenschap|Azure PowerShell variabelenaam|Waarde|Beschrijving
    ---|---|---|---
    HDInsight de naam van cluster|$clusterName||De naam van het cluster HDInsight.
    Cluster-gebruikersnaam|$clusterUsername||Gebruikersnaam cluster opgegeven waarop het cluster is gemaakt.
    Wachtwoord van de cluster|$clusterPassword||Cluster gebruikerswachtwoord.

    >[AZURE.NOTE] Invullen de waarden in de tabel. Dit is handig voor het doorlopen van deze zelfstudie.

## <a name="jump-start"></a>Jump start

Er zijn verschillende manieren om met Ambari voor het controleren van HDInsight clusters.

**Azure PowerShell gebruiken**

Het volgende is een MapReduce taak beheer informatie Azure PowerShell script *in een cluster HDInsight 3.1.*  Het belangrijkste verschil is dat we deze details van het GAREN service (in plaats van MapReduce) halen.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Dit is een Azure PowerShell-script voor het verkrijgen van de MapReduce taak vastleggen gegevens *in een cluster HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

De uitvoer is:

![Uitvoer van Jobtracker][img-jobtracker-output]

**Gebruiken van krul**

Hier volgt een voorbeeld van een cluster informatie ophalen met behulp van krul:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

De uitvoer is:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Vrijgeven voor 10/8/2014**:

Als u het eindpunt van de Ambari, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" retourneert het veld *hostnaam* de FQDN-naam (Fully Qualified Domain Name) van het knooppunt in plaats van de host-naam. In dit voorbeeld heeft vóór de release van 8-10/2014 gewoon '**headnode0**'. Na de release van 8-10/2014, krijgt u de FQDN-naam '**headnode0. { ClusterDNS} .azurehdinsight .net**", zoals in het vorige voorbeeld. Deze wijziging is vereist voor scenario's waarbij meerdere clustertypen (zoals HBase en Hadoop) kunnen worden geïmplementeerd in een virtueel netwerk (VNET) te vergemakkelijken. Dit gebeurt, bijvoorbeeld wanneer u HBase als een back-end-platform voor Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari API's controleren

De volgende tabel worden enkele van de meest voorkomende Ambari API-aanroepen te controleren. Zie voor meer informatie over de API [Ambari API Reference][ambari-api-reference].

Monitor API-aanroep|URI|Beschrijving
---|---|---
Ophalen van clusters|`/api/v1/clusters`|
Cluster-gegevens opvragen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|clusters, services, hosts
Services ophalen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Services omvatten: hdfs, mapreduce
Services-gegevens opvragen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Ophalen van service-onderdelen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode, datanode<br/>MapReduce: jobtracker; tasktracker
Component gegevens opvragen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo, host-onderdelen, metrics
Hosts ophalen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
Hostgegevens opvragen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
De onderdelen host ophalen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode, resourcemanager
Host onderdeel gegevens opvragen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles, component, host, metrics
Ophalen van configuraties|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Config-typen: core-site, hdfs-site, mapred site, component-site
Configuratie-informatie ophalen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Config-typen: core-site, hdfs-site, mapred site, component-site


##<a name="next-steps"></a>Volgende stappen

U hebt nu geleerd hoe te Ambari controle van API-aanroepen gebruiken. Voor meer informatie, Zie:

- [HDInsight clusters met de Azure Portal beheren][hdinsight-admin-portal]
- [HDInsight clusters met Azure PowerShell beheren][hdinsight-admin-powershell]
- [HDInsight clusters opdrachtregel beheren][hdinsight-admin-cli]
- [HDInsight documentatie][hdinsight-documentation]
- [Aan de slag met HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
