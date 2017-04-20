<properties
    pageTitle="HBase clusters maken in een virtueel netwerk | Microsoft Azure"
    description="Aan de slag met HBase in Azure HDInsight. Informatie over het HDInsight HBase clusters op Azure virtueel netwerk maken."
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>HBase clusters in Azure virtueel netwerk maken 

Informatie over het maken van clusters Azure HDInsight HBase in een [Virtueel netwerk Azure][1].

Dankzij virtuele kunnen clusters HBase worden toegepast op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen rechtstreeks met HBase communiceren kunnen. De voordelen zijn:

- Directe verbinding van de webtoepassing op de knooppunten van het cluster HBase, waarmee communicatie via HBase Java remote procedure call (RPC)-API's.
- Verbeterde prestaties met het niet hebben van het verkeer over meerdere gateways en netwerktaakverdeling Ga.
- De mogelijkheid om gevoelige informatie op een veiliger manier verwerken zonder dat een openbare eindpunt.

###<a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**. Zie [installeren en Azure PowerShell gebruiken](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>HBase cluster in een virtueel netwerk maken

In dit gedeelte maakt u een Linux-gebaseerde HBase cluster met de afhankelijke Azure opslag rekening in een Azure virtueel netwerk met behulp van een [sjabloon Azure Resource Manager](../resource-group-template-deploy.md). De instellingen, Zie [clusters maken HDInsight](hdinsight-hadoop-provision-linux-clusters.md)voor andere methoden voor het cluster maken en inzicht. Zie voor meer informatie over het gebruik van een sjabloon voor het maken van clusters van Hadoop in HDInsight [clusters in HDInsight met behulp van sjablonen Azure Resource Manager Hadoop maken](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Sommige eigenschappen zijn hard gecodeerd in de sjabloon. Bijvoorbeeld:
>
> * __Locatie__: Oost-VS 2
> * __Cluster versie: 3.4
> * __Aantal clusters werknemer-knooppunt__: 4
> * __Standaardaccount voor opslag__: &lt;Clusternaam > opslaan
> * __Virtuele-netwerknaam__: &lt;Clusternaam >-vnet
> * __Virtueel netwerkadresruimte__: 10.0.0.0/16
> * __Subnetnaam__: standaard
> * __Subnet-adresbereik__: 10.0.0.0/24
>
> &lt;Clusternaam > wordt vervangen door de cluster die u opgeeft wanneer u de sjabloon.

1. Klik op de onderstaande afbeelding om de sjabloon te openen in de portal voor Azure. De sjabloon bevindt zich in een openbare blob-container. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende gegevens van de bladeserver **aangepaste implementatie** :

    - **Abonnement**: Selecteer een Azure abonnement gebruikt voor het maken van het cluster HDInsight, de afhankelijke opslag-account en de Azure virtueel netwerk.
    - **Resourcegroep**: Geef de naam van een nieuwe resource en selecteer **nieuwe maken**.
    - **Locatie**: Selecteer een locatie voor de resourcegroep.
    - **Clusternaam**: Voer een naam voor het Hadoop-cluster dat u wilt maken.
    - **Cluster-aanmeldingsnaam en wachtwoord**: de standaard login-naam **admin**is.
    - **SSH-gebruikersnaam en wachtwoord**: de standaardgebruikersnaam is **sshuser**.  U kunt deze wijzigen. 
    - **Ik ga akkoord met de voorwaarden en de hierboven vermelde voorwaarden**: (Selecteer)
    

3. Klik op **Inkoop**. Duurt ongeveer 20 minuten over het maken van een cluster. Als het cluster is gemaakt, klikt u op het cluster blade in de portal te openen.

Nadat u de zelfstudie hebt voltooid, kunt u het cluster te verwijderen. Uw gegevens worden opgeslagen in Azure-opslag met HDInsight, zodat u een cluster veilig verwijderen kunt wanneer het niet in gebruik. Er zijn ook in rekening gebracht voor een cluster HDInsight, zelfs wanneer deze niet in gebruik. Aangezien de kosten voor het cluster vele malen meer dan de kosten voor opslag zijn, is het zinvol economische clusters verwijderen wanneer ze niet in gebruik zijn. Zie voor de instructies van het verwijderen van een cluster, [clusters beheren Hadoop in HDInsight met behulp van de portal Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

Als u wilt werken met de nieuwe HBase cluster, kunt u de procedures in [aan de slag met HBase met Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md)gevonden.

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Verbinding maken met de HBase cluster met HBase Java RPC API 's

1.  Een infrastructuur als een service (IaaS) virtuele machine maken in Azure hetzelfde virtuele netwerk en op hetzelfde subnet. Zie voor instructies over het maken van een nieuwe virtuele machine voor IaaS, [maken een Windows Server virtuele Machine wordt uitgevoerd](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Wanneer u de stappen in dit document, moet u het volgende voor de netwerkconfiguratie:

    - __Virtueel netwerk__: &lt;Clusternaam >-vnet
    - __Subnetmasker__: standaard

    > [AZURE.IMPORTANT] Vervangen &lt;Clusternaam > met de naam die u gebruikt bij het maken van het cluster HDInsight in de vorige stappen.

    Met deze waarden wordt de virtuele machine configureren voor het gebruik van hetzelfde virtuele netwerk en subnet als het cluster HDInsight. Hierdoor kunnen ze rechtstreeks met elkaar communiceren.

2.  Als u op afstand verbinding maken met HBase met een Java-toepassing, moet u de FQDN-naam (Fully Qualified Domain Name). Om dit te bepalen, moet u de verbindingsspecifieke DNS-achtervoegsel van het cluster HBase. Hiervoor kunt u een van de volgende methoden:

    * Een webbrowser gebruiken om een oproep voor een Ambari:
    
        Ga naar https://&lt;Clusternaam >.azurehdinsight.net/api/v1/clusters/&lt;Clusternaam > / hosts?minimal_response = true. Hiermee schakelt u een JSON-bestand met de DNS-achtervoegsels.

    * Gebruik de Ambari-website:

        1. Ga naar https://&lt;Clusternaam >. azurehdinsight.net.
        2. Klik op **Hosts** uit het bovenste menu.

    * Gebruik de krul REST gesprekken tot stand brengen:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        Zoek de vermelding "hostnaam" in de geretourneerde JavaScript Object Notation (JSON) gegevens. Dit bevat de FQDN van de knooppunten in het cluster. Bijvoorbeeld:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Het gedeelte van het domein naam die begint met de naam van het cluster is het DNS-achtervoegsel. Bijvoorbeeld mycluster.b1.cloudapp.net.

    * Azure PowerShell gebruiken
    
        Het volgende Azure PowerShell-script gebruiken voor het registreren van de functie **Get-ClusterDetail** , die kan worden gebruikt voor het retourneren van het DNS-achtervoegsel:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Nadat het Azure PowerShell-script uitgevoerd, gebruikt u de volgende opdracht te retourneren van het DNS-achtervoegsel met de functie **Get-ClusterDetail** . Uw HDInsight HBase clusternaam, admin en admin-wachtwoord opgeven wanneer u deze opdracht gebruikt.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Hiermee herstelt u het DNS-achtervoegsel. Bijvoorbeeld **yourclustername.b4.internal.cloudapp.net**.

    * RDP gebruiken
    
        U kunt ook extern bureaublad verbinding maken met de HBase cluster (u wordt verbonden met het hoofd knooppunt) en voer **ipconfig** vanaf de opdrachtprompt voor het DNS-achtervoegsel. Zie voor instructies over het inschakelen van Remote Desktop Protocol (RDP) en verbinding maken met het cluster via RDP, [clusters beheren Hadoop in HDInsight met de Azure portal][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Gebruik de opdracht om te controleren of de virtuele machine met het cluster HBase communiceren kan, `ping headnode0.<dns suffix>` van de virtuele machine. Bijvoorbeeld: ping headnode0.mycluster.b1.cloudapp.net.

Als u deze informatie in een Java-toepassing, kunt u de stappen in de [Maven Java-toepassingen die gebruikmaken van HBase met HDInsight (Hadoop) gebruiken](hdinsight-hbase-build-java-maven.md) om een toepassing te maken. Als u wilt dat de verbinding met een externe server met HBase toepassing, door het bestand **hbase site.xml** in dit voorbeeld gebruikt u de FQDN-naam voor Zookeeper te wijzigen. Bijvoorbeeld:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Voor meer informatie over naamomzetting in Azure Zie virtuele netwerken, inclusief het gebruik van uw eigen DNS-server [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe een cluster HBase maken. Voor meer informatie, Zie:

- [Aan de slag met HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [HBase replicatie configureren in HDInsight](hdinsight-hbase-geo-replication.md)
- [Hadoop clusters maken in HDInsight](hdinsight-provision-clusters.md)
- [Aan de slag met HBase met Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md)
- [Twitter sentiment met HBase in HDInsight analyseren](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Virtueel netwerk-overzicht][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Details van de voorziening voor het nieuwe cluster van HBase"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Scriptactie gebruiken voor het aanpassen van een cluster HBase"

[azure-preview-portal]: https://portal.azure.com
