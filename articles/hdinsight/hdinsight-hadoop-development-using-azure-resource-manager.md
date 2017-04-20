<properties
    pageTitle="Migreren naar Azure Resource Manager ontwikkelprogramma's voor clusters HDInsight | Microsoft Azure"
    description="Migreren naar Azure Resource Manager ontwikkelprogramma's voor clusters van HDInsight"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Migreren naar Azure Resource Manager gebaseerde ontwikkelingsprogramma's voor clusters van HDInsight

HDInsight is Azure Service Manager ASM-hulpprogramma's voor HDInsight bestandstypen. Als u hebt gebruikt Azure PowerShell en Azure CLI HDInsight .NET SDK om te werken met clusters van HDInsight, wordt u aangeraden Azure Resource Manager ARM-versies van PowerShell CLI en .NET SDK gaan gebruiken. Dit artikel bevat aanwijzers op migreren naar de nieuwe aanpak op basis van een ARM. Waar van toepassing, wijst in dit artikel ook op de verschillen tussen de ARM en ASM benaderingen voor HDInsight.

>[AZURE.IMPORTANT] De ondersteuning voor ASM op basis van PowerShell, CLI, en .NET SDK op **1 januari 2017**af te breken.

##<a name="migrating-azure-cli-to-azure-resource-manager"></a>Azure CLI migreren naar Azure Resource Manager

De CLI Azure nu standaard Azure Resource Manager (ARM)-modus, tenzij u een upgrade van een vorige installatie uitvoert; u moet in dat geval gebruikt u de `azure config mode arm` opdracht overschakelen naar de ARM.

De basisopdrachten die de CLI Azure met HDInsight wilt werken met Azure Service Management (ASM) zijn hetzelfde bij het gebruik van de ARM; Sommige parameters en schakelopties kunnen wel nieuwe namen en er zijn veel nieuwe parameters beschikbaar bij het gebruik van de ARM. Bijvoorbeeld: u kunt nu gebruiken `azure hdinsight cluster create` de Azure virtueel netwerk die moeten worden gemaakt in een cluster of component en Oozie metastore informatie te geven.

Basisopdrachten voor het werken met HDInsight via Azure Resource Manager zijn:

* `azure hdinsight cluster create`-maakt een nieuw cluster van HDInsight
* `azure hdinsight cluster delete`-Hiermee verwijdert u een bestaand cluster van HDInsight
* `azure hdinsight cluster show`-informatie weergeven over een bestaand cluster
* `azure hdinsight cluster list`-Geeft een overzicht van HDInsight van clusters voor uw abonnement op Azure

Gebruik de `-h` switch om te controleren parameters en schakelopties die beschikbaar zijn voor elke opdracht.

###<a name="new-commands"></a>Nieuwe opdrachten

Nieuwe opdrachten met Azure Resource Manager beschikbaar zijn:

* `azure hdinsight cluster resize`-het aantal knooppunten in het cluster werknemer dynamisch wordt gewijzigd
* `azure hdinsight cluster enable-http-access`-HTTPs toegang biedt tot het cluster (op standaard)
* `azure hdinsight cluster disable-http-access`-toegang tot het cluster HTTPs wordt uitgeschakeld
* `azure hdinsight-enable-rdp-access`-Remote Desktop Protocol kunnen op een cluster met Windows-gebaseerde HDInsight
* `azure hdinsight-disable-rdp-access`-Remote Desktop Protocol uitgeschakeld op een cluster met Windows-gebaseerde HDInsight
* `azure hdinsight script-action`-bevat opdrachten voor het maken/beheren scriptacties op een cluster
* `azure hdinsight config`-bevat opdrachten voor het maken van een configuratiebestand dat kan worden gebruikt met de `hdinsight cluster create` de opdracht om configuratie-informatie te verstrekken.

###<a name="deprecated-commands"></a>Afgekeurde opdrachten

Als u de `azure hdinsight job` opdrachten taken kunnen verzenden naar uw cluster HDInsight, deze zijn niet beschikbaar via de ARM-opdrachten. Als u taken programmatisch verzenden naar HDInsight van scripts, moet u in plaats daarvan de overige API's die door HDInsight. Zie de volgende documenten voor meer informatie over het verzenden van taken met REST API's.

* [MapReduce-taken uitvoeren met Hadoop op HDInsight met krul](hdinsight-hadoop-use-mapreduce-curl.md)
* [Component-query's uitvoeren met Hadoop op HDInsight met krul](hdinsight-hadoop-use-hive-curl.md)
* [Varken taken uitvoeren met Hadoop op HDInsight met krul](hdinsight-hadoop-use-pig-curl.md)

Zie voor informatie over andere manieren om interactief MapReduce component en varkens worden uitgevoerd, [Gebruik MapReduce met Hadoop op HDInsight](hdinsight-use-mapreduce.md) [Gebruik component met Hadoop op HDInsight](hdinsight-use-hive.md)en [Gebruik varken met Hadoop op HDInsight](hdinsight-use-pig.md).

###<a name="examples"></a>Voorbeelden

__Een cluster maken__

* Oude opdracht (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Nieuwe opdracht (ARM)`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

__Een cluster verwijderen__

* Oude opdracht (ASM)-`azure hdinsight cluster delete myhdicluster`
* Nieuwe opdracht (ARM)`azure hdinsight cluster delete mycluster -g myresourcegroup`

__Lijst met clusters__

* Oude opdracht (ASM)-`azure hdinsight cluster list`
* Nieuwe opdracht (ARM)`azure hdinsight cluster list`

> [AZURE.NOTE] Opgeven voor de opdracht lijst resource groep met `-g` retourneert alleen de clusters in de betreffende resourcegroep.

__Cluster weergeven__

* Oude opdracht (ASM)-`azure hdinsight cluster show myhdicluster`
* Nieuwe opdracht (ARM)`azure hdinsight cluster show myhdicluster -g myresourcegroup`


##<a name="migrating-azure-powershell-to-azure-resource-manager"></a>Azure PowerShell migreren naar Azure Resource Manager

Algemene informatie over Azure PowerShell in de modus Azure Resource Manager (ARM) kan worden gevonden op [Azure PowerShell Azure Resource Manager gebruiken](../powershell-azure-resource-manager.md).

De ARM van Azure PowerShell-cmdlets kunnen worden geïnstalleerd side-by-side met de ASM-cmdlets. De cmdlets van de twee modi kan worden onderscheiden door hun namen.  De modus van de ARM heeft *AzureRmHDInsight* in de cmdlet namen vergelijken met de *AzureHDInsight* in de modus van het AANT.  Bijvoorbeeld *Nieuwe AzureRmHDInsightCluster* vs. *Nieuw-AzureHDInsightCluster*. Parameters en schakelopties nieuws namen kunnen hebben en er zijn veel nieuwe parameters beschikbaar bij het gebruik van de ARM.  Verschillende cmdlets vereist een nieuwe switch *- ResourceGroupName*genoemd. 

Voordat u de HDInsight-cmdlets gebruiken kunt, moet u verbinding maken met uw account Azure en een nieuwe resourcegroep maken:

- Login AzureRmAccount of [Selecteer AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Zie [verificatie van een service principal Azure Resource Manager](../resource-group-authenticate-service-principal.md)
- [Nieuwe AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

###<a name="renamed-cmdlets"></a>Hernoemde cmdlets

Overzicht van de cmdlets ASM HDInsight in Windows PowerShell-console:

    help *azurermhdinsight*

De volgende tabel worden de ASM-cmdlets en hun namen in de ARM-modus:

|ASM-cmdlets|ARM-cmdlets|
|------|------|
| Toevoegen-AzureHDInsightConfigValues | [Toevoegen-AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx)|
| Toevoegen-AzureHDInsightMetastore |[Toevoegen-AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx)|
| Toevoegen-AzureHDInsightScriptAction |[Toevoegen-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx)|
| Toevoegen-AzureHDInsightStorage |[Toevoegen-AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx)|
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx)|
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx)|
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx)|
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Subsidie AzureHDInsightHttpServicesAccess | [Subsidie AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx)|
| Subsidie AzureHdinsightRdpAccess |[Subsidie AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx)|
| Aanroepen van AzureHDInsightHiveJob |[Aanroepen van AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx)|
| Nieuwe AzureHDInsightCluster | [Nieuwe AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx)|
| Nieuwe AzureHDInsightClusterConfig |[Nieuwe AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx)|
| Nieuwe AzureHDInsightHiveJobDefinition |[Nieuwe AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx)|
| Nieuwe AzureHDInsightMapReduceJobDefinition |[Nieuwe AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Nieuwe AzureHDInsightPigJobDefinition |[Nieuwe AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx)|
| Nieuwe AzureHDInsightSqoopJobDefinition |[Nieuwe AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx)|
| Nieuwe AzureHDInsightStreamingMapReduceJobDefinition |[Nieuwe AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Verwijderen AzureHDInsightCluster |[Verwijderen AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx)|
| REVOKE-AzureHDInsightHttpServicesAccess |[REVOKE-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx)|
| REVOKE-AzureHdinsightRdpAccess |[REVOKE-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx)|
| Set AzureHDInsightClusterSize |[Set AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx)|
| Set AzureHDInsightDefaultStorage |[Set AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx)|
| Start AzureHDInsightJob |[Start AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx)|
| Stop AzureHDInsightJob |[Stop AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx)|
| Gebruik AzureHDInsightCluster |[Gebruik AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx)|
| Wachten op AzureHDInsightJob |[Wachten op AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx)|

###<a name="new-cmdlets"></a>Nieuwe cmdlets
Hieronder vindt u de nieuwe cmdlets alleen beschikbaar in de modus van de ARM zijn. 

**De scriptactie cmdlets gerelateerd:**
- **Get-AzureRmHDInsightPersistedScriptAction**: haalt de permanente scriptacties voor een cluster en worden ze weergegeven in chronologische volgorde of details voor een actie opgegeven permanente script als resultaat gegeven. 
- **Get-AzureRmHDInsightScriptActionHistory**: haalt de geschiedenis van de actie script voor een cluster en details van de scriptactie van een eerder uitgevoerde haalt, wordt weergegeven in omgekeerde chronologische volgorde. 
- **Verwijderen AzureRmHDInsightPersistedScriptAction**: een permanente scriptactie verwijdert uit een cluster van HDInsight.
- **Set AzureRmHDInsightPersistedScriptAction**: een eerder uitgevoerde script een vastgelegde scriptactie worden ingesteld.
- **Verzenden-AzureRmHDInsightScriptAction**: een nieuwe scriptactie aan een cluster Azure HDInsight indient. 

Zie voor informatie over het gebruik van extra, [HDInsight aanpassen Linux gebaseerde clusters met actie Script](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter identiteit cmdlets gerelateerd:**

- **Toevoegen-AzureRmHDInsightClusterIdentity**: de identiteit van een cluster toegevoegd aan een cluster-configuratie-object zodat het cluster HDInsight toegang Azure Lake gegevensarchieven tot. Zie [een cluster van HDInsight met Lake gegevensarchief maken met Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Voorbeelden

**Cluster maken**

Oude opdracht (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Nieuwe opdracht (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials

 
**Cluster verwijderen**

Oude opdracht (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Nieuwe opdracht (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 
                
**Lijst cluster**

Oude opdracht (ASM):

    Get-AzureHDInsightCluster
                
Nieuwe opdracht (ARM):

    Get-AzureRmHDInsightCluster 

**Cluster weergeven**

Oude opdracht (ASM):

    Get-AzureHDInsightCluster -Name $clusterName
                
Nieuwe opdracht (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


####<a name="other-samples"></a>Andere voorbeelden

- [HDInsight clusters maken](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
- [Taken van de component](hdinsight-hadoop-use-hive-powershell.md)
- [Varken taken verzenden](hdinsight-hadoop-use-pig-powershell.md)
- [Sqoop taken](hdinsight-hadoop-use-sqoop-powershell.md)



##<a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a>Migreren naar de ARM gebaseerde HDInsight .NET SDK

De op basis van Azure Service Management [(ASM) HDInsight.NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) is nu verouderd. U wordt aangeraden de op basis van Azure Resource Management [(ARM) HDInsight.NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx)gebruiken. De volgende HDInsight op basis van ASM-pakketten zijn worden afgeschaft.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`
 

Deze sectie bevat verwijzingen naar meer informatie over het uitvoeren van bepaalde taken met de ARM gebaseerde SDK.

| Hoe... met de ARM gebaseerde HDInsight SDK | Koppelingen |
| ------------------- | --------------- |
| HDInsight clusters met .NET SDK maken| Zie [HDInsight maken clusters met .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)|
| Een cluster met behulp van de actie Script met .NET SDK aanpassen | Zie [met behulp van de actie Script aanpassen HDInsight Linux-clusters](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Toepassingen met .NET SDK met Azure Active Directory te verifiëren| Zie [component uitvoeren van query's met .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md). Het codefragment in dit artikel wordt de aanpak van interactieve verificatie gebruikt.|
| Toepassingen die gebruikmaken van Azure Active Directory niet interactief met de SDK voor .NET verifiëren | Zie [niet-interactieve toepassingen voor HDInsight maken](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Een component-taak met behulp van .NET SDK| Zie [taken voor component indienen](hdinsight-hadoop-use-hive-dotnet-sdk.md) |
| Een varken-taak met behulp van .NET SDK | Zie [taken varken indienen](hdinsight-hadoop-use-pig-dotnet-sdk.md)|
| Een Sqoop-taak met behulp van .NET SDK | Zie [taken Sqoop indienen](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |
| HDInsight lijst met .NET SDK-clusters | Zie [lijst HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| HDInsight clusters met .NET SDK schalen | Zie [clusters schaal HDInsight](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Toegang verlenen/intrekken met clusters van HDInsight met behulp van .NET SDK | Zie [toegang verlenen/intrekken met clusters van HDInsight](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| HTTP-gebruikersreferenties voor HDInsight clusters met .NET SDK bijwerken | Zie [HTTP Update gebruikersreferenties voor clusters met HDInsight](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| De standaardaccount voor opslag vinden voor HDInsight clusters met .NET SDK | Zie [de standaardaccount opslag voor clusters HDInsight zoeken](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| HDInsight clusters met .NET SDK verwijderen | Zie [clusters HDInsight verwijderen](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Voorbeelden

Hieronder volgen enkele voorbeelden van hoe een bewerking is uitgevoerd met behulp van de SDK ASM gebaseerd en de equivalente codefragment voor ARM gebaseerde SDK.

**Maken van een cluster CRUD-client**

* Oude opdracht (ASM)

        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
     
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);

* Nieuwe opdracht (ARM) (Service principal vergunning)

        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);

* Nieuwe opdracht (ARM) (machtiging van gebruikers)

        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);


**Een cluster maken**

* Oude opdracht (ASM)

        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);


* Nieuwe opdracht (ARM)

        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Windows,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);


**HTTP-toegang inschakelen**

* Oude opdracht (ASM)

        client.EnableHttp(dnsName, "West US", "admin", "*******");

* Nieuwe opdracht (ARM)

        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Een cluster verwijderen**

* Oude opdracht (ASM)

        client.DeleteCluster(dnsName);

* Nieuwe opdracht (ARM)

        client.Clusters.Delete(resourceGroup, dnsname);




