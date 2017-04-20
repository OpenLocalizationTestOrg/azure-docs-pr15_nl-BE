<properties
    pageTitle="Hadoop clusters met Azure CLI beheren | Microsoft Azure"
    description="Met behulp van de CLI Azure beheren Hadoop clusters in HDIsight"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Hadoop clusters in HDInsight met behulp van de CLI Azure beheren

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Informatie over het gebruik van de [opdrachtregelinterface Azure](../xplat-cli-install.md) Hadoop clusters in Azure HDInsight beheren. De CLI Azure is geïmplementeerd in Node.js. Het kan worden gebruikt op elk platform dat Node.js, waaronder Windows, Mac en Linux ondersteunt.

In dit artikel beschreven hoe u met alleen de CLI Azure met HDInsight. Zie voor algemene richtlijnen voor het gebruik van Azure CLI [installeren en configureren van Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI** - Zie [installeren en configureren van de CLI Azure](../xplat-cli-install.md) voor informatie over installatie en configuratie.
- **Verbinding maken met Azure**, met de volgende opdracht:

        azure login

    Zie voor meer informatie over verificatie via een account voor werk of school, [verbinding maken met een Azure-abonnement van de CLI Azure](xplat-cli-connect.md).
    
- **Overschakelen naar de modus Azure Resource Manager**met de volgende opdracht:

        azure config mode arm

Als u help wilt, gebruikt u de schakeloptie **-h** .  Bijvoorbeeld:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Clusters maken

Zie [maken van Linux gebaseerde clusters in HDInsight met behulp van de CLI Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Overzicht en details van het cluster weergeven
Gebruik de volgende opdrachten te kunnen aanbieden en de details van het cluster weergeven:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Verwijderen van clusters

Gebruik de volgende opdracht om een cluster te verwijderen:

    azure hdinsight cluster delete <Cluster Name>

U kunt ook een cluster verwijderen door de bronnengroep met het cluster. Let op: Hiermee verwijdert u alle bronnen in de groep met inbegrip van de standaardaccount voor opslag.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Schaal clusters

De clustergrootte Hadoop wijzigen:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>HTTP-toegang voor een cluster in-of uitschakelen

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>RDP-toegang voor een cluster in-of uitschakelen

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe verschillende HDInsight cluster beheertaken uitvoeren. Voor meer informatie, Zie de volgende artikelen:

* [HDInsight beheren via de Portal Azure] [hdinsight-admin-portal]
* [HDInsight met Azure PowerShell beheren] [hdinsight-admin-powershell]
* [Aan de slag met Azure HDInsight] [hdinsight-get-started]
* [Het gebruik van de CLI Azure] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Een lijst en clusters weergeven"
