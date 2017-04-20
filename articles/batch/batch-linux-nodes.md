<properties
    pageTitle="Knooppunten in Azure Batch toepassingen Linux | Microsoft Azure"
    description="Informatie over het verwerken van uw werkbelasting parallelle compute op pools van Linux virtuele machines in Azure Batch."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Inrichten van Linux-computerknooppunten in Azure Batch van toepassingen

U kunt Batch Azure parallelle compute werklasten worden uitgevoerd op zowel Linux als Windows virtuele machines. In dit artikel wordt gedetailleerd beschreven hoe u toepassingen van Linux-computerknooppunten in de Batch-service maken met behulp van zowel de [Batch Python] [ py_batch_package] en [Batch.NET] [ api_net] clientbibliotheken.

> [AZURE.NOTE] [Toepassingspakketten](batch-application-packages.md) worden momenteel niet ondersteund op Linux-computerknooppunten.

## <a name="virtual-machine-configuration"></a>Configuratie van de virtuele machine

Als u een groep met computerknooppunten in Batch maakt, hebt u twee opties te selecteren, de grootte van het knooppunt en het besturingssysteem: Cloud Services-configuratie en de configuratie van de virtuele Machine.

**Configuratie van cloud Services** biedt dat Windows knooppunten *alleen*berekenen. Beschikbare compute knooppunt formaten worden weergegeven in de [formaten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md)en beschikbare besturingssystemen worden weergegeven in het [Gastbesturingssysteem Azure releases en SDK compatibility matrix](../cloud-services/cloud-services-guestos-update-matrix.md). Als u een groep met Azure Cloud Services-knooppunten maakt, moet u geeft de grootte van het knooppunt en de "OS-familie' in de eerder genoemde artikelen worden gevonden. Voor groepen van Windows computerknooppunten, wordt meestal Cloud Services gebruikt.

**Virtuele machineconfiguratie** biedt zowel Linux als Windows-installatiekopieën voor computerknooppunten. Beschikbare compute knooppunt formaten worden vermeld in de [formaten voor virtuele machines in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) en - [grootten voor virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Wanneer u een pool met virtuele machineconfiguratie knooppunten maakt, moet u de grootte van de knooppunten, verwijzing naar de virtuele machine-afbeelding en de Batch knooppunt agent SKU moet worden geïnstalleerd op de knooppunten.

### <a name="virtual-machine-image-reference"></a>Verwijzing naar de virtuele machine-afbeelding

De Batch-service gebruikt [virtuele machine schaal ingesteld](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor Linux-computerknooppunten. De installatiekopieën van besturingssystemen voor deze virtuele machines worden geleverd door de [Azure Marketplace][vm_marketplace]. Wanneer u een verwijzing naar een virtuele machine-afbeelding hebt geconfigureerd, kunt u de eigenschappen van een virtuele machine Marketplace afbeelding opgeven. De volgende eigenschappen zijn vereist bij het maken van een verwijzing naar een virtuele machine-afbeelding:

| **Verwijzen naar eigenschappen van afbeelding** | **Voorbeeld** |
| ----------------- | ------------------------ |
| Publisher         | Canonieke                |
| Aanbieding             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| Versie           | meest recente                   |

> [AZURE.TIP] U kunt meer informatie over deze eigenschappen en hoe u afbeeldingen op Marketplace in [navigeren en selecteer Linux virtuele machine afbeeldingen in Azure met CLI of PowerShell](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md). Houd er rekening mee dat niet alle afbeeldingen op Marketplace momenteel compatibel met Batch zijn. Voor meer informatie Zie [knooppunt agent SKU](#node-agent-sku).

### <a name="node-agent-sku"></a>Knooppunt agent SKU

De Batch knooppunt agent is een programma dat wordt uitgevoerd op elk knooppunt in de groep en de command en control interface vormt tussen het knooppunt en de Batch-service. Er zijn verschillende implementaties van het knooppunt agent, bekend als SKU's, voor verschillende besturingssystemen. In feite, wanneer u de configuratie van een virtuele Machine maakt, u eerst een verwijzing naar de virtuele machine afbeelding opgeven en geef vervolgens het knooppunt agent installeren op de afbeelding. Normaal gesproken elke agent knooppunt SKU is compatibel met meerdere afbeeldingen voor virtuele machine. Hier volgen enkele voorbeelden van knooppunt agent SKU's:

* batch.node.Ubuntu 14.04
* batch.node.centos 7
* batch.node.Windows-amd64

> [AZURE.IMPORTANT] Niet alle afbeeldingen voor virtuele machine die beschikbaar in de markt zijn zijn compatibel met de momenteel beschikbare Batch knooppunt agenten. De Batch-SDK's moet u de beschikbare knooppunt agent SKU's en de virtuele machine afbeeldingen waarmee ze compatibel zijn. Zie de [lijst van de virtuele Machine afbeeldingen](#list-of-virtual-machine-images) verderop in dit artikel voor meer informatie.

## <a name="create-a-linux-pool-batch-python"></a>Maken van een groep Linux: Batch Python

Het volgende codefragment ziet u een voorbeeld van het gebruik van de [Microsoft Azure Batch Client Library voor Python] [ py_batch_package] een groep van de Ubuntu Server computerknooppunten te maken. Documentatie voor de Batch Python module kunt u vinden op [azure.batch pakket] [py_batch_docs] op de documenten lezen.

Dit codefragment maakt een [ImageReference] [ py_imagereference] expliciet en geeft u elk van de eigenschappen (publisher, aanbieding, SKU, versie). In de productiecode, echter raadzaam gebruik te maken van de [list_node_agent_skus] [ py_list_skus] methode voor het bepalen en selecteren uit de beschikbare image en knooppunt agent SKU combinaties tijdens runtime.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Zoals eerder vermeld, raden wij in plaats van de [ImageReference] [ py_imagereference] expliciet, het gebruik van de [list_node_agent_skus] [ py_list_skus] methode dynamisch uit het ondersteunde knooppunt agent/Marketplace afbeelding combinaties selecteren. In het volgende fragment van Python toont het gebruik van deze methode.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Maken van een groep Linux: Batch .NET

Het volgende codefragment ziet u een voorbeeld van het gebruik van de [Batch.NET] [ nuget_batch_net] client-bibliotheek van een groep van de Ubuntu Server computerknooppunten te maken. U vindt de [batchverwerking .NET documentatie] [ api_net] op MSDN.

Het volgende codefragment wordt de [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] methode te selecteren uit de lijst met momenteel Marketplace afbeelding knooppunt agent SKU combinaties en ondersteund. Deze techniek is wenselijk omdat de lijst met ondersteunde combinaties van tijd tot tijd kan veranderen. In het algemeen worden ondersteunde combinaties toegevoegd.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Hoewel het voorgaande codefragment maakt gebruik van de [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] methode dynamisch lijst en kies afbeelding en knooppunt agent SKU combinaties (aanbevolen) ondersteund, kunt u ook een [ImageReference] [ net_imagereference] expliciet:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Lijst met afbeeldingen van de virtuele machine

De volgende tabel worden de afbeeldingen op Marketplace virtuele machine die compatibel met de beschikbare Batch knooppunt agenten zijn als in dit artikel voor het laatst is bijgewerkt. Het is belangrijk te weten dat deze lijst is niet definitief omdat afbeeldingen en knooppunt agenten kunnen worden toegevoegd of verwijderd op elk gewenst moment. Het is raadzaam dat de partij toepassingen en services gebruik altijd [list_node_agent_skus] [ py_list_skus] (Python) en [ListNodeAgentSkus] [ net_list_skus] (Batch .NET) om te bepalen en te selecteren uit de beschikbare SKU's.

> [AZURE.WARNING] De volgende lijst kan op elk gewenst moment wijzigen. Gebruik altijd de **lijst knooppunt agent SKU** methoden beschikbaar in de Batch-API's aan de lijst en selecteer vervolgens in de compatibele virtuele machine en het knooppunt agent SKU's wanneer u de batchverwerkingen uitvoert.

| **Publisher** | **Aanbieding** | **Afbeelding SKU** | **Versie** | **Knooppunt agent SKU-ID** |
| ------- | ------- | ------- | ------- | ------- |
| Canonieke | UbuntuServer | 14.04.0-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.1-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.2-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.3-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.4-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.5-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 16.04.0-LTS | meest recente | batch.node.Ubuntu 16.04 |
| Credativ | Debian | 8 | meest recente | batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | meest recente | batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | meest recente | batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | meest recente | 13.2 batch.node.opensuse |
| SUSE | openSUSE sprong | 42,1 | meest recente | batch.node.opensuse 42,1 |
| SUSE | SLES HPC | 12 | meest recente | batch.node.opensuse 42,1 |
| SUSE | SLES | 12-SP1 | meest recente | batch.node.opensuse 42,1 |
| Microsoft-advertenties | Standaard data-science vm | Standaard data-science vm | meest recente | batch.node.Windows-amd64 |
| Microsoft-advertenties | Linux-gegevens-wetenschap-vm | linuxdsvm | meest recente | batch.node.centos 7 |
| Legitieme Microsoft Windows Server | WindowsServer | 2008 R2 SP1 | meest recente | batch.node.Windows-amd64 |
| Legitieme Microsoft Windows Server | WindowsServer | 2012-Datacenter | meest recente | batch.node.Windows-amd64 |
| Legitieme Microsoft Windows Server | WindowsServer | 2012-R2-Datacenter | meest recente | batch.node.Windows-amd64 |
| Legitieme Microsoft Windows Server | WindowsServer | Windows Server-technische Preview | meest recente | batch.node.Windows-amd64 |

## <a name="connect-to-linux-nodes"></a>Verbinding maken met de Linux-knooppunten

Tijdens de ontwikkeling of bij het oplossen van problemen vindt u het kan nodig zijn om aan te melden bij de knooppunten in uw groep. In tegenstelling tot Windows compute nodes kan u Remote Desktop Protocol (RDP) niet gebruiken voor verbinding met de Linux-knooppunten. De Batch-service kunt in plaats daarvan SSH toegang op elk knooppunt voor externe verbindingen.

Het volgende Python codefragment maakt een gebruiker op elk knooppunt in een groep die vereist voor externe verbindingen is. Informatie van de secure shell (SSH) voor elk knooppunt wordt afgedrukt.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Hier volgt een voorbeeld van uitvoer voor de vorige code voor een groep met vier knooppunten met Linux:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Houd er rekening mee dat in plaats van een wachtwoord, kunt u een openbare SSH-sleutel bij het maken van een gebruiker op een knooppunt. In de SDK van Python, dit gebeurt met behulp van de parameter **ssh_public_key** op [ComputeNodeUser][py_computenodeuser]. In .NET, wordt dit gedaan met behulp van de [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] eigenschap.

## <a name="pricing"></a>Prijzen

Azure Batch is gebaseerd op technologie van Azure Cloud Services en Azure virtuele Machines. De Batch-service zelf wordt aangeboden gratis, wat betekent dat u in rekening worden gebracht voor de compute-bronnen alleen dat uw oplossingen Batch verbruiken. Wanneer u ervoor kiest de **Configuratie van Cloud Services**, moet u betalen op basis van de [prijzen van Cloud Services] [ cloud_services_pricing] structuur. Wanneer u een **Virtuele machineconfiguratie**kiest, moet u betalen op basis van de [prijzen van virtuele Machines] [ vm_pricing] structuur.

## <a name="next-steps"></a>Volgende stappen

### <a name="batch-python-tutorial"></a>Batch Python zelfstudie

Voor een uitgebreidere zelfstudie over het werken met Batch met behulp van Python uitchecken [aan de slag met de Azure Batch Python client](batch-python-tutorial.md). De companion [codevoorbeeld] [ github_samples_pyclient] bevat een Help-functie, `get_vm_config_for_distro`, geeft een andere methode om de configuratie van een virtuele machine te verkrijgen.

### <a name="batch-python-code-samples"></a>Voorbeelden van batch Python code

Bekijk de andere [Python code monsters] [ github_samples_py] in de [voorbeelden van azure batch] [ github_samples] bibliotheek op GitHub voor verschillende scripts die hoe u veelvoorkomende batchbewerkingen zoals toepassingen, taak en taak maken. Het [Leesmij-bestand] [ github_py_readme] die wordt geleverd bij de Python monsters heeft meer informatie over het installeren van de vereiste pakketten.

### <a name="batch-forum"></a>Batch-forum

Het [Forum van Azure Batch] [ forum] is een prima plek om te bespreken Batch en vragen stellen over de service op MSDN. 'Stickied' nuttige geposte berichten lezen en vragen dat ze zich voordoen terwijl u uw Batch oplossingen bouwen.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
