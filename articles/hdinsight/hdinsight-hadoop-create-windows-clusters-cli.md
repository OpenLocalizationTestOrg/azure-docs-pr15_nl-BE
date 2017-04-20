<properties
   pageTitle="Hadoop Windows-gebaseerde clusters maken in Azure CLI met HDInsight"
    description="Informatie over het maken van clusters voor Azure HDInsight met behulp van de CLI Azure."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Hadoop Windows-gebaseerde clusters maken in Azure CLI met HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Informatie over het maken van HDInsight clusters met Azure CLI. Maken van het cluster andere hulpprogramma's en functies klikt u op het tabblad selecteren boven aan deze pagina of voor [methoden voor het Cluster maken](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Voorwaarden:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Voordat u de instructies in dit artikel, hebt u het volgende:

- **Azure-abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Verbinding maken met Azure

Met de volgende opdracht om verbinding met Azure te gebruiken:

    azure login

Zie voor meer informatie over verificatie via een account voor werk of school, [verbinding maken met een Azure-abonnement van de CLI Azure](../xplat-cli-connect.md).

Gebruik de volgende opdracht om te schakelen naar de modus ARM:

    azure config mode arm

Als u help wilt, gebruikt u de schakeloptie **-h** .  Bijvoorbeeld:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Clusters maken

Hebt u een Azure Resource Management (ARM) en een Azure Blob storage-account voordat u een cluster HDInsight kunt maken. Een HDInsight als cluster wilt maken, moet u het volgende opgeven:

- **Resourcegroep Azure**: A Data Lake Analytics account moet worden gemaakt binnen een groep Azure. Azure Resource Manager kunt u werken met de resources in uw toepassing als een groep. U kunt implementeren, bijwerken of verwijderen alle bronnen voor uw toepassing in een enkele, gecoördineerde werking.

    Overzicht van de resourcegroepen in uw abonnement:

        azure group list

    Een nieuwe resourcegroep maken:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **HDInsight de naam van cluster**

- **Locatie**: een van de Azure datacenters die clusters van HDInsight ondersteunt. Zie voor een lijst met ondersteunde locaties, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Standaardaccount voor opslag**: HDInsight een Azure Blob opslag container gebruikt als het standaardbestandssysteem. Een account Azure opslag is vereist voordat u een cluster HDInsight kunt maken.

    Een nieuwe account te maken Azure opslag:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]De opslag-account moet worden collocated met HDInsight in het datacenter.
    > Het soort opslag kan ZRS, niet omdat ZRS tabel wordt niet ondersteund.

    Zie voor meer informatie over het maken van een account Azure opslag met behulp van de Portal Azure [maken, beheren of verwijderen van een account opslag] [azure-maken-storageaccount].

    Als u al een opslag-account hebt, maar niet de naam en de sleutel van de account weet, kunt u de volgende opdrachten om de informatie te halen:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Zie de sectie 'Uw account opslag beheren' van [Azure over opslag accounts](../storage/storage-create-storage-account#manage-your-storage-account)voor meer informatie over het gebruik van de informatie via de Portal Azure.

- **Container (optioneel) standaard Blob**: de opdracht **cluster azure hdinsight maken** maakt u de container als het niet bestaat. Als u de container vooraf te maken, kunt u de volgende opdracht:

    Azure opslag container maken--rekening met de naam "<Storage Account Name>"--account-sleutel <Storage Account Key> [ContainerName]

Als u de account Storage is bereid hebt, bent u klaar voor het maken van een cluster:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Clusters met behulp van configuratiebestanden maken
Meestal maken een cluster HDInsight, taken uitvoeren, en verwijdert het cluster de kosten terugdringen. De opdrachtregelinterface biedt u de optie voor het opslaan van de configuratie in een bestand, zodat u deze opnieuw gebruiken kunt telkens wanneer u een cluster maakt.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Voorbeeld: Maak een configuratiebestand met de scriptactie van een worden uitgevoerd wanneer u een cluster maakt.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Clusters met scriptactie maken

Maak een configuratiebestand met de scriptactie van een worden uitgevoerd wanneer u een cluster maakt.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Een cluster maken met een scriptactie

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Algemeen script actie informatie, Zie [aanpassen van HDInsight clusters met actie Script (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Clusters met ARM-sjablonen maken

CLI kunt u clusters maken door het aanroepen van ARM-sjablonen. Zie de [implementatie van CLI Azure](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Zie ook

- [Aan de slag met Azure-HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - informatie over het werken met het cluster HDInsight starten
- [Hadoop indienen projecten via programmacode](hdinsight-submit-hadoop-jobs-programmatically.md) - informatie over het programmatisch taken kunnen verzenden naar HDInsight
- [Hadoop clusters in HDInsight met behulp van de CLI Azure beheren](hdinsight-administer-use-command-line.md)
- [Met behulp van de CLI Azure voor Mac, Linux en Windows met Azure Service Management](../virtual-machines-command-line-tools.md)
