<properties
    pageTitle="HDInsight Clusters met scriptacties aanpassen | Microsoft Azure"
    description="Informatie over het aanpassen van HDInsight clusters met actie Script."
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
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>HDInsight Windows-gebaseerde clusters met actie Script aanpassen

**Scriptactie** kan worden gebruikt om op te roepen van [aangepaste scripts](hdinsight-hadoop-script-actions.md) tijdens het proces voor het cluster maken voor het installeren van extra software op een cluster.

De informatie in dit artikel is specifiek voor de HDInsight op basis van Windows-clusters. Zie [HDInsight aanpassen Linux gebaseerde clusters met actie Script](hdinsight-hadoop-customize-cluster-linux.md)voor Linux gebaseerde clusters. 

HDInsight clusters op allerlei andere manieren, zoals het opnemen van extra opslag Azure accounts kunnen worden aangepast, de Hadoop wijzigen configuratiebestanden (core site.xml component site.xml, enz.) of bibliotheken (bijvoorbeeld component, Oozie) toe te voegen gedeeld in veelgebruikte locaties in het cluster. Deze aanpassingen kunnen worden uitgevoerd via Azure PowerShell, Azure HDInsight .NET SDK of de Azure-Portal. Zie voor meer informatie, [Maak Hadoop clusters in HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>De scriptactie in het ontwikkelingsproces van een cluster

De scriptactie wordt alleen gebruikt wanneer een clusters is nog moet worden gemaakt. In het volgende diagram ziet u wanneer de scriptactie wordt uitgevoerd tijdens het maken van:

![HDInsight cluster aanpassen en de fasen tijdens het maken van het cluster][img-hdi-cluster-states]

Wanneer het script wordt uitgevoerd, voert de **ClusterCustomization** fase in het cluster. In dit stadium, het script wordt uitgevoerd onder de account system admin parallel op de opgegeven knooppunten in het cluster, en biedt volledige beheerdersbevoegdheden op de knooppunten.

> [AZURE.NOTE] Omdat u beheerdersbevoegdheden op de clusterknooppunten in de **ClusterCustomization** fase hebt, kunt u het script uit te voeren bewerkingen, zoals diensten, met inbegrip van Hadoop-gerelateerde services starten en stoppen. Dus, als onderdeel van het script, moet u ervoor zorgen dat de Ambari en andere Hadoop-gerelateerde services zijn uitgevoerd en voordat het script wordt beëindigd. Deze services zijn vereist om na te gaan is de gezondheid en de status van het cluster terwijl deze wordt gemaakt. Als u de configuratie van het cluster die invloed heeft op deze diensten wijzigt, moet u de Help-functies die beschikbaar zijn. Zie voor meer informatie over Help-functies [ontwikkelen scriptactie scripts voor HDInsight][hdinsight-write-script].

De uitvoer en de foutenlogboeken voor het script worden opgeslagen in de standaard opslag-account voor het cluster opgegeven. De logboekbestanden worden opgeslagen in een tabel met de naam **u < \cluster-name-fragment >< \time-stamp > Setuplog.txt**. Dit zijn statistische Logboeken uit het script uitvoeren op alle knooppunten (hoofd-knooppunt en werknemer knooppunten) in het cluster.

Elk cluster meerdere scriptacties die worden aangeroepen in de volgorde waarin ze zijn geaccepteerd. Een script kan worden uitgevoerd op het knooppunt head of de knooppunten van de werknemer.

HDInsight biedt verschillende scripts op clusters van HDInsight om de volgende onderdelen:

Naam | Script
----- | -----
**Installatie van motoren** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zie [installeren en gebruiken Wek op clusters HDInsight][hdinsight-install-spark].
**R installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zie [installeren en gebruiken R op clusters HDInsight][hdinsight-install-r].
**Solr installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zie [installeren en gebruiken Solr op HDInsight](hdinsight-hadoop-solr-install.md).
- **Giraph installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zie [installeren en gebruiken Giraph op HDInsight](hdinsight-hadoop-giraph-install.md).
| **Component bibliotheken vooraf laden** | https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Zie [bibliotheken op clusters HDInsight component toevoegen](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Bellen met behulp van de Portal Azure scripts

**Vanaf de Portal Azure**

1. Start u een cluster maakt, zoals beschreven bij het [maken van Hadoop clusters in het HDInsight](hdinsight-provision-clusters.md#portal).
2. Klik onder optionele configuratie voor de bladeserver **Scriptacties** op **scriptactie toevoegen** voor meer informatie over de scriptactie zoals hieronder wordt weergegeven:

    ![Gebruik scriptactie voor het aanpassen van een cluster] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Gebruik scriptactie voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie.</td></tr>
        <tr><td>URI-script</td>
            <td>Geef de URI voor het script dat wordt aangeroepen voor het aanpassen van het cluster. s</td></tr>
        <tr><td>Hoofd/werknemer</td>
            <td>Geef de knooppunten (**Head** of **werknemer**) op het aangepaste script wordt uitgevoerd. </b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script.</td></tr>
    </table>

    Druk op ENTER om meer dan één scriptactie om meerdere onderdelen op het cluster toevoegen.

3. Klik op **selecteren** om de configuratie van de actie script opslaan en doorgaan met het maken van het cluster.

## <a name="call-scripts-using-azure-powershell"></a>Bel met Azure PowerShell scripts

Deze volgende PowerShell script laat zien hoe Spark op Windows gebaseerde HDInsight cluster installeert.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Als u andere software installeert, moet u het scriptbestand in het script vervangen:


Voer desgevraagd de referenties voor het cluster. Het kan enkele minuten duren voordat het cluster wordt gemaakt.

## <a name="call-scripts-using-net-sdk"></a>Oproep-scripts met behulp van .NET SDK 

In het volgende voorbeeld wordt gedemonstreerd hoe Spark op Windows gebaseerde HDInsight cluster installeert. Als u andere software installeert, moet u het scriptbestand in de code vervangen.

**Voor het maken van een cluster HDInsight met vonkontsteking** 

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht vanaf de Console Nuget Package Manager.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Gebruik de volgende instructies gebruiken in het bestand Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. De code in de klasse met de volgende plaatsen:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Druk op **F5** om de toepassing te starten.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open source software gebruikt op de clusters HDInsight
De service Microsoft Azure HDInsight is een flexibel platform waarmee u toepassingen te bouwen grote gegevens in de cloud met behulp van een ecosysteem van open source-technologieën rond Hadoop gevormd. Microsoft Azure biedt een algemeen niveau van ondersteuning voor open source-technologieën, zoals besproken in de sectie **Bereik ondersteuning** van de <a href="http://azure.microsoft.com/support/faq/" target="_blank">website Azure ondersteunen Veelgestelde vragen</a>. De HDInsight-service biedt extra ondersteuning voor een aantal van de componenten, zoals hieronder beschreven.

Er zijn twee typen van open source-componenten die beschikbaar in de HDInsight-service zijn:

- **Ingebouwde onderdelen** - deze onderdelen zijn vooraf geïnstalleerd op HDInsight clusters en bieden basisfunctionaliteit van het cluster. Bijvoorbeeld, behoren GAREN ResourceManager de querytaal component (HiveQL) en de bibliotheek Mahout tot deze categorie. Een volledige lijst met clusteronderdelen is beschikbaar in [nieuwe functies in de versies van Hadoop cluster geleverd door HDInsight?](hdinsight-component-versioning.md) </a>.
- **Aangepaste onderdelen** - u, als gebruiker van het cluster, kunt installeren of gebruiken in uw werkbelasting onderdelen beschikbaar zijn in de Gemeenschap of door u gemaakt.

Ingebouwde onderdelen worden volledig ondersteund en Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze componenten.

> [AZURE.WARNING] Onderdelen van het cluster HDInsight worden volledig ondersteund en Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze componenten.
>
> Aangepaste onderdelen ontvangen commercieel redelijke ondersteuning waarmee u het probleem verder oplossen. Dit kan leiden tot het oplossen van het probleem of vraag aan een van de beschikbare kanalen voor de open-source technologieën waarbij diepe expertise voor de technologie die wordt gevonden. Er zijn bijvoorbeeld veel sites van de community die kunnen worden gebruikt, zoals: [MSDN forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projecten hebben ook projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

De HDInsight-service biedt verschillende manieren gebruik van aangepaste onderdelen. Ongeacht hoe een onderdeel wordt gebruikt of op het cluster is geïnstalleerd, geldt hetzelfde niveau van ondersteuning. Hieronder ziet u een lijst met de meest gangbare manieren dat aangepaste onderdelen op HDInsight-clusters kunnen worden gebruikt:

1. Project indienen - Hadoop of andere typen taken uitvoeren of aangepaste onderdelen kan worden ingediend bij het cluster.
2. Aanpassing van de cluster - tijdens het maken van het cluster, kunt u extra instellingen en aangepaste onderdelen die worden geïnstalleerd op de clusterknooppunten.
3. Voorbeelden van hoe deze onderdelen kunnen worden gebruikt op de clusters HDInsight bepalen monsters - voor populaire aangepaste onderdelen, Microsoft en anderen. Deze monsters worden geleverd zonder ondersteuning.

## <a name="develop-script-action-scripts"></a>De actie Script scripts ontwikkelen

Zie [scripts ontwikkelen scriptactie voor HDInsight][hdinsight-write-script].


## <a name="see-also"></a>Zie ook

- [Hadoop clusters maken in HDInsight] [ hdinsight-provision-cluster] vindt u instructies voor het maken van een HDInsight-cluster met behulp van andere aangepaste opties.
- [De actie Script scripts ontwikkelen voor HDInsight][hdinsight-write-script]
- [Installeren en gebruiken van motoren op HDInsight clusters][hdinsight-install-spark]
- [Installeren en gebruiken van R op clusters HDInsight][hdinsight-install-r]
- [Installeren en gebruiken Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md).
- [Installatie en gebruik Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "De fasen tijdens het maken van het cluster"
