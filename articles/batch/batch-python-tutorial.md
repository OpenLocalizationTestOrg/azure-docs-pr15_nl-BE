<properties
    pageTitle="Zelfstudie - aan de slag met de client Azure Batch Python | Microsoft Azure"
    description="Meer informatie over de basisbegrippen van Azure Batch en het ontwikkelen van de Batch-service met een eenvoudige scenario"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Aan de slag met de Azure Batch Python client

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Leer de grondbeginselen van [Azure Batch] [ azure_batch] en de [Batch Python] [ py_azure_sdk] client zoals we een kleine Batch toepassing geschreven in Python bespreken. We bekijken hoe twee sample scripts gebruik de Batch-service voor het verwerken van een parallelle werkbelasting op Linux virtuele machines in de cloud en hoe ze werken met [Azure opslag](./../storage/storage-introduction.md) voor bestand klaarzetten en ophalen. U zult een algemene workflow voor Batch toepassing leren en krijgen een basis kennis van de belangrijkste onderdelen van de Batch zoals projecten, taken, toepassingen en computerknooppunten.

![Workflow voor batch-oplossing (basic)][11]<br/>

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u enige basiskennis van Python en vertrouwd zijn met Linux. Ook wordt ervan uitgegaan dat u kunnen voldoen aan de account maken die bent onder zijn opgegeven voor Azure en de Batch- en services.

### <a name="accounts"></a>Rekeningen

- **Azure-account**: als u niet al een Azure-abonnement, [Maak een gratis account Azure][azure_free_account].
- **Batch-account**: zodra er een Azure-abonnement, [een Batch Azure-account maken](batch-account-create-portal.md).
- **Opslag account**: Zie [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md).

### <a name="code-sample"></a>Voorbeeld van code

De zelfstudie van Python [codevoorbeeld] [ github_article_samples] is een van de vele voorbeelden van Batch-code gevonden in de [voorbeelden van azure batch] [ github_samples] bibliotheek op GitHub. U kunt alle monsters downloaden door te klikken op **klonen of downloaden > downloaden ZIP-** op de introductiepagina van de opslagplaats, of door te klikken op de [azure batch-monsters master.zip] [ github_samples_zip] directe koppeling. Als u de inhoud van het ZIP-bestand hebt uitgepakt, de twee scripts voor deze zelfstudie worden gevonden in de `article_samples` map:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python-omgeving

Het voorbeeldscript *python_tutorial_client.py* op uw lokale werkstation worden uitgevoerd, moet u een **Python-interpreter** compatibel met versie **2.7** of **3.3 +**. Het script is getest op zowel Linux als Windows.

### <a name="cryptography-dependencies"></a>cryptografie met afhankelijkheden

Moet u de afhankelijkheden voor de [cryptografie] [ crypto] -bibliotheek vereist door de `azure-batch` en `azure-storage` Python pakketten. Voer een van de volgende bewerkingen die geschikt is voor uw platform of raadpleegt u de [installatie van cryptografie] [ crypto_install] details voor meer informatie:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Als voor Python 3.3 + op Linux installeert, gebruikt u de python3-equivalenten voor de Python afhankelijkheden. Bijvoorbeeld, voor Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure-pakketten

Vervolgens wordt de **Azure Batch** en **Azure opslag** Python pakketten installeert. U kunt dit doen met **pip** en de *requirements.txt* hier vinden:

`/azure-batch-samples/Python/Batch/requirements.txt`

Probleem na **pip** -opdracht naar de Batch- en pakketten te installeren:

`pip install -r requirements.txt`

Of u kunt de [azure batch] installeren[ pypi_batch] en [azure opslag] [ pypi_storage] Python pakketten handmatig:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Mogelijk moet u de opdrachten met het voorvoegsel `sudo` als u een account zonder machtigingen. Bijvoorbeeld `sudo pip install -r requirements.txt`. Zie voor meer informatie over het installeren van pakketten Python [Pakketten installeren] [ pypi_install] op readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Batch Python zelfstudie codevoorbeeld

In het codevoorbeeld voor Batch Python zelfstudie bestaat uit twee Python scripts en een paar bestanden.

- **python_tutorial_client.PY**: interactie met de Batch- en services voor het uitvoeren van een parallelle werkbelasting op computerknooppunten (virtuele machines). Het *python_tutorial_client.py* -script wordt uitgevoerd op uw lokale werkstation.

- **python_tutorial_task.PY**: het script dat wordt uitgevoerd op knooppunten in Azure voor het uitvoeren van de werkelijke hoeveelheid werk berekend. *Python_tutorial_task.py* analyseert in het voorbeeld wordt de tekst in een bestand gedownload van Azure opslag (invoerbestand). Vervolgens is het resultaat een tekstbestand (de uitvoer) met een lijst van de bovenste drie woorden die worden weergegeven in het invoerbestand. Nadat het uitvoerbestand is gemaakt, *python_tutorial_task.py* het bestand geüpload naar de opslag van Azure. Hierdoor kunt downloaden naar de clientscript dat wordt uitgevoerd op uw werkstation. Het script *python_tutorial_task.py* parallel op meerdere computerknooppunten in de Batch-service wordt uitgevoerd.

- **./data/taskdata\*.txt**: deze drie bestanden bevatten de invoer voor de taken die worden uitgevoerd op de compute nodes.

In het volgende diagram ziet u de primaire bewerkingen die door de client- en scripts worden uitgevoerd. Deze eenvoudige werkstroom is typisch voor veel compute-oplossingen die zijn gemaakt met de batchverwerking. Terwijl hij alle functies die beschikbaar zijn in de Batch-service niet aantonen heeft, ook bijna elke Batch geldt voor gedeelten van deze workflow.

![Batch voorbeeldwerkstroom][8]<br/>

[**Stap 1.**](#step-1-create-storage-containers) **Containers** in Azure Blob-opslag maken.<br/>
[**Stap 2.**](#step-2-upload-task-script-and-data-files) Script en input bestanden uploaden naar containers.<br/>
[**Stap 3.**](#step-3-create-batch-pool) Een Batch- **toepassingen**maken.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** De groep **starttaak** downloads het taak-script (python_tutorial_task.py) voor knooppunten als zij deelnemen aan de pool.<br/>
[**Stap 4.**](#step-4-create-batch-job) Maak een Batch **job**.<br/>
[**Stap 5.**](#step-5-add-tasks-to-job) **Taken** toevoegen aan het project.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** De taken worden worden uitgevoerd op de knooppunten gepland.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Elke taak de invoergegevens van Azure opslag gedownload en vervolgens wordt uitgevoerd.<br/>
[**Stap 6.**](#step-6-monitor-tasks) Taken controleren.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Als de taken zijn voltooid, uploaden ze hun uitvoergegevens naar Azure opslag.<br/>
[**Stap 7.**](#step-7-download-task-output) Download de uitvoer van de taak van opslag.

Zoals vermeld, niet elke Batch oplossing deze exacte stappen uitvoert en eventueel ook ziet veel meer, maar deze u het gemeenschappelijke processen in een Batch-oplossing gevonden.

## <a name="prepare-client-script"></a>Clientscript voorbereiden

Voordat u het voorbeeld uitvoert, moet u uw referenties van Batch- en toevoegen aan *python_tutorial_client.py*. Als u dit nog niet hebt gedaan, open het bestand in uw favoriete editor en de volgende regels bijwerken met uw referenties.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Kunt u uw referenties van Batch- en in het blad van de account van elke service vinden in de [Azure portal][azure_portal]:

![Batch-referenties in de portal][9]
![opslag van referenties in de portal][10]<br/>

In de volgende secties analyseren we de stappen die door de scripts gebruikt voor het verwerken van een werkbelasting in de Batch-service. We raden u aan regelmatig verwijzen naar de scripts in uw editor terwijl u werkt op uw manier door de rest van het artikel.

Ga naar de volgende regel in **python_tutorial_client.py** om te beginnen met stap 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Stap 1: Maak Storage containers

![Containers in Azure opslag maken][1]
<br/>

Batch beschikt over ingebouwde ondersteuning voor interactie met de opslag van Azure. Containers in uw account opslag krijgt u de bestanden die nodig zijn voor de taken die worden uitgevoerd in uw account voor de Batch. De containers bevatten ook een plaats voor de worden uitvoergegevens opgeslagen die de taken produceren. Allereerst wordt het script *python_tutorial_client.py* is drie containers maken in [Azure Blob-opslag](../storage/storage-introduction.md#blob-storage):

- **toepassing**: deze container de Python script uitvoeren door de taken, de *python_tutorial_task.py*worden opgeslagen.
- **invoer**: taken de gegevensbestanden voor het verwerken van de *invoer* container wordt gedownload.
- **output**: als invoerbestand verwerking taken hebt voltooid, zal zij de resultaten uploaden naar de container voor de *uitvoer* .

We gebruiken om te communiceren met een account voor opslag en containers maken, de [azure opslag] [ pypi_storage] pakket voor het maken van een [BlockBlobService] [ py_blockblobservice] object--de "blob client'. Vervolgens maken we drie containers in de opslag met de blob-client.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Nadat de containers zijn gemaakt, kan de toepassing nu de bestanden die worden gebruikt door de taken uploaden.

> [AZURE.TIP] [Het gebruik van Azure Blob-opslag van Python](../storage/storage-python-how-to-use-blob-storage.md) biedt een goed overzicht van het werken met Azure opslagtanks en BLOB's. Moeten worden aan de bovenkant van de lijst lezen als u begint te werken met Batch.

## <a name="step-2-upload-task-script-and-data-files"></a>Stap 2: Script- en bestanden uploaden

![Toepassing van de taak en de invoer (gegevens) bestanden uploaden naar containers][2]
<br/>

In de bewerking voor het uploaden van bestanden definieert *python_tutorial_client.py* eerst u verzamelingen van **toepassing** en een **invoer** paden die op de lokale computer zijn. Deze vervolgens uploadt deze bestanden naar de containers die u in de vorige stap hebt gemaakt.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Begrip van de lijst, met de `upload_file_to_container` -functie wordt aangeroepen voor elk bestand in de collecties en twee [ResourceFile] [ py_resource_file] collecties worden gevuld. De `upload_file_to_container` functie hieronder wordt weergegeven:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

Een [ResourceFile] [ py_resource_file] vindt u de taken in een Batch met de URL naar een bestand in Azure opslag die is gedownload naar een compute node voordat de taak wordt uitgevoerd. De [ResourceFile][py_resource_file]. de eigenschap **blob_source** geeft de volledige URL van het bestand zoals aanwezig in Azure opslag. De URL kan ook een gedeelde access-handtekening (SAS) die veilige toegang tot het bestand biedt. De meeste taaktypen in Batch bevatten een eigenschap van de *ResourceFiles* , met inbegrip van:

- [CloudTask][py_task]
- [Starttaak][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

In dit voorbeeld maakt geen gebruik van de taaktypen JobPreparationTask of JobReleaseTask, maar u kunt meer lezen over hen in [computerknooppunten uitvoeren voorbereiding en voltooiing van taken op Azure Batch](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Gedeelde toegang handtekening (SAS)

Gedeelde toegang handtekeningen zijn tekenreeksen die beveiligde toegang tot containers en BLOB's in Azure opslag bieden. Het script *python_tutorial_client.py* wordt gebruikt voor zowel blob en container gedeelde toegang handtekeningen en laat zien hoe u deze gedeelde toegang handtekening tekenreeksen van de Storage-service verkrijgen.

- **BLOB gedeelde toegang handtekeningen**: starttaak maakt gebruik van de toepassingen van blob-handtekeningen voor gedeelde toegang wanneer het downloaden van het script en input data bestanden uit de opslag (Zie [stap #3](#step-3-create-batch-pool) hieronder). De `upload_file_to_container` functie in de *python_tutorial_client.py* bevat de code die een handtekening voor elke blob van gedeelde toegang verkrijgt. Dit gebeurt door het aanroepen van [BlockBlobService.make_blob_url] [ py_make_blob_url] in de opslagmodule.

- **Container gedeelde toegang handtekening**: zoals elke taak het werk op de compute node heeft voltooid, het het output-bestand uploadt naar de container *uitvoer* in Azure opslag. *Python_tutorial_task.py* maakt gebruik van een handtekening van container gedeelde toegang dat schrijven toegang tot de container biedt doen. De `get_container_sas_token` -functie in de *python_tutorial_client.py* van de container gedeelde toegang handtekening, vervolgens als een opdrachtregelargument aan de taken doorgegeven wordt verkrijgt. Stap #5, [taken toevoegen aan een taak](#step-5-add-tasks-to-job), wordt het gebruik van de container SAS besproken.

> [AZURE.TIP] De tweedelige serie over gedeelde toegang handtekeningen, uitchecken [deel 1: wat het model SAS](../storage/storage-dotnet-shared-access-signature-part-1.md) en [deel 2: maken en gebruiken van een SAS met de Blob-service](../storage/storage-dotnet-shared-access-signature-part-2.md), voor meer informatie over een beveiligde toegang tot de gegevens in uw account voor de opslag.

## <a name="step-3-create-batch-pool"></a>Stap 3: Maak een Batch van toepassingen

![Een Batch-toepassingen maken][3]
<br/>

Een Batch- **groep** is een verzameling van computerknooppunten (virtuele machines) op welke Batch de taken van een taak uitvoeren.

*Python_tutorial_client.py* gestart nadat deze het script en gegevens bestanden geüpload naar de rekening voor de opslag, de interactie met de Batch-service met behulp van de Batch Python module. Om dit te doen, een [BatchServiceClient] [ py_batchserviceclient] wordt gemaakt:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Vervolgens wordt een groep computerknooppunten gemaakt in de Batch-account met een aanroep van `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Wanneer u een groep maakt, definieert u een [PoolAddParameter] [ py_pooladdparam] die de diverse eigenschappen voor de groep aangeeft:

- **ID** van de groep (*id* - vereist)<p/>De nieuwe groep moet een unieke ID binnen uw account Batch hebben net als bij de meeste entiteiten in Batch. De code verwijst naar deze groep met behulp van de ID en hoe het identificeren van de toepassingen in de Azure [portal]is[azure_portal].

- **Aantal computerknooppunten** (*target_dedicated* - vereist)<p/>Deze eigenschap geeft aan hoeveel VMs in de groep moeten worden geïmplementeerd. Het is belangrijk te weten dat alle accounts van de Batch standaard **quota** die het aantal **cores** (en dus computerknooppunten) in een Batch-account beperkt hebben. Vindt u de standaard-quota's en instructies voor het [quotum verhogen](batch-quota-limit.md#increase-a-quota) (zoals het maximum aantal cores in uw account Batch) in [quota's en beperkingen voor de Batch Azure service](batch-quota-limit.md). Als u merkt dat u vragen "Waarom won't mijn groep bereiken meer dan X knooppunten?" Dit quotum core kan de oorzaak zijn.

- **Besturingssysteem** voor knooppunten (*virtual_machine_configuration* **of** *cloud_service_configuration* - vereist)<p/>*Python_tutorial_client.py*, maken we een groep van Linux-knooppunten met behulp van een [VirtualMachineConfiguration][py_vm_config]. De `select_latest_verified_vm_image_with_node_agent_sku` in werking `common.helpers` vereenvoudigt het werken met [Virtuele Machines Marketplace van Azure] [ vm_marketplace] afbeeldingen. Zie [Voorziening Linux computerknooppunten in Azure Batch van toepassingen](batch-linux-nodes.md) voor meer informatie over het gebruik van afbeeldingen op Marketplace.

- **Grootte van computerknooppunten** (*vm_size* - vereist)<p/>Aangezien we Linux knooppunten voor onze [VirtualMachineConfiguration geeft][py_vm_config], geeft u een VM-grootte (`STANDARD_A1` in dit voorbeeld) van [formaten voor virtuele machines in Azure](../virtual-machines/virtual-machines-linux-sizes.md). Nogmaals, Zie [Linux bepaling computerknooppunten in Azure Batch van toepassingen](batch-linux-nodes.md) voor meer informatie.

- **Taak starten** (*start_task* - niet verplicht)<p/>Samen met de bovenstaande eigenschappen van het fysieke knooppunt, kunt u ook opgeven een [starttaak] [ py_starttask] voor de toepassingen (dit is niet vereist). De starttaak wordt uitgevoerd op elk knooppunt dat knooppunt wordt toegevoegd aan de groep, en elke keer dat een knooppunt opnieuw is gestart. De starttaak is vooral handig voor het voorbereiden van computerknooppunten voor het uitvoeren van taken, zoals het installeren van de toepassingen die de taken uitgevoerd.<p/>In deze voorbeeldtoepassing kopieert de starttaak de bestanden die het downloaden naar de *gedeelde* map die alle taken op het knooppunt toegang heeft tot de starttaak *directory werkt* uit de opslag (die zijn opgegeven met de eigenschap **resource_files** van de starttaak). In feite wordt Hiermee kopieert u `python_tutorial_task.py` met de gedeelde directory op elk knooppunt als het knooppunt lid van de groep, zodat alle taken die worden uitgevoerd op het knooppunt toegang.

Is het mogelijk de aanroep van de `wrap_commands_in_shell` Help-functie. Deze functie heeft een verzameling afzonderlijke opdrachten en maakt één opdrachtregel voor opdrachtregel-eigenschap van een taak.

Let op in het bovenstaande codefragment is ook het gebruik van twee omgevingsvariabelen in de **command_line** -eigenschap van de starttaak: `AZ_BATCH_TASK_WORKING_DIR` en `AZ_BATCH_NODE_SHARED_DIR`. Elk computerknooppunt in een Batch-groep wordt automatisch geconfigureerd met verschillende omgevingsvariabelen die specifiek voor de Batch zijn. Een proces dat door een taak is uitgevoerd, heeft toegang tot deze omgevingsvariabelen.

> [AZURE.TIP] Meer informatie over de omgevingsvariabelen die beschikbaar op de computerknooppunten in een Batch van toepassingen, alsook informatie over de taak werken mappen zijn Zie **omgevingsinstellingen voor taken** en **bestanden en mappen** in het [overzicht van functies van Batch Azure](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Stap 4: Maak een batchverwerking

![Batchverwerking maken][4]<br/>

Een Batch- **taak** is een verzameling taken en is gekoppeld aan een groep met computerknooppunten. De taken in een project worden uitgevoerd op de bijbehorende toepassingen computerknooppunten.

U kunt een taak niet alleen voor het ordenen en bijhouden van taken in de bijbehorende werklast, maar ook voor het opleggen van bepaalde beperkingen--zoals de maximale runtime voor de taak (en door uitbreiding van haar taken) en taken prioriteit ten opzichte van andere taken in de Batch-account. In dit voorbeeld wordt is de taak echter alleen gekoppeld aan de groep die is gemaakt in stap #3. Er zijn geen aanvullende eigenschappen geconfigureerd.

Alle taken zijn gekoppeld aan een specifieke groep. Deze koppeling geeft aan welke taken van de taak uitvoeren op knooppunten. U geeft de groep met behulp van de [PoolInformation] [ py_poolinfo] eigenschap, zoals in het onderstaande stukje code.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Nu dat een taak is gemaakt, worden de taken toegevoegd voor het uitvoeren van het werk.

## <a name="step-5-add-tasks-to-job"></a>Stap 5: Taken toevoegen aan project

![Taken toevoegen aan project][5]<br/>
*(1) taken toegevoegd aan het project, (2) de taken uitvoeren op knooppunten worden gepland en (3) de taken voor het verwerken van bestanden downloaden*

Batch **taken** zijn de afzonderlijke eenheden van werk uitvoeren op de compute nodes. Een taak is een opdrachtregel en scripts of uitvoerbare bestanden die u in die regel opdracht uitgevoerd.

Werkelijk werk uitvoeren, moeten de taken worden toegevoegd aan een taak. Elke [CloudTask] [ py_task] is geconfigureerd met een opdrachtregel-eigenschap en [ResourceFiles] [ py_resource_file] (net als bij de groep van toepassingen starttaak) die de taak kan worden gedownload naar het knooppunt voordat u de opdrachtregel wordt automatisch uitgevoerd. Elke taak verwerkt in het voorbeeld heeft slechts één bestand. Dus bevat de ResourceFiles-collectie een enkel element.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Wanneer zij toegang krijgen tot omgevingsvariabelen, zoals `$AZ_BATCH_NODE_SHARED_DIR` of het uitvoeren van een toepassing niet gevonden in van het knooppunt `PATH`, opdrachtregels taak moet de shell aanroepen expliciet, zoals met `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Deze eis is niet nodig als uw taken uitvoert van een toepassing in van het knooppunt `PATH` en niet verwijzen naar omgevingsvariabelen.

In de `for` in het bovenstaande codefragment lus, kunt u zien dat de opdrachtregel voor de taak is opgebouwd uit vijf argumenten voor de opdrachtregel die worden doorgegeven aan *python_tutorial_task.py*:

1. **bestandspad**: dit is het lokale pad naar het bestand zoals dit op het knooppunt. Wanneer het ResourceFile-object `upload_file_to_container` is gemaakt in stap 2, de naam voor deze eigenschap is gebruikt (de `file_path` -parameter in de constructor ResourceFile). Hiermee wordt aangegeven dat het bestand in dezelfde map als het *python_tutorial_task.py*op het knooppunt kan worden gevonden.

2. **NumWords**: de top *N* woorden moeten worden opgenomen in het uitvoerbestand.

3. **storageaccount**: de naam van de opslag-account die eigenaar is van de container waarin de uitvoer van de taak moet worden geüpload.

4. **storagecontainer**: de naam van de opslag container waaraan de uitvoerbestanden moeten worden geüpload.

5. **sastoken**: de handtekening gedeelde toegang (SAS) die schrijftoegang tot de container voor **uitvoer** in Azure opslag biedt. Het script *python_tutorial_task.py* wordt gebruikt voor deze gedeelde toegang handtekening wanneer de verwijzing BlockBlobService wordt gemaakt. Dit biedt schrijftoegang tot de container zonder een toegangstoets voor de opslag.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Stap 6: Monitortaken

![Monitortaken][6]<br/>
*Het script (1) de taken van de voltooiingsstatus gecontroleerd en (2) de taken resultaatgegevens uploaden naar Azure opslag*

Wanneer taken worden toegevoegd aan een taak, worden ze automatisch in de wachtrij geplaatst en gepland voor uitvoering op computerknooppunten binnen de groep die aan de taak is gekoppeld. Op basis van de instellingen die u opgeeft, Batch worden verwerkt alle taak queuing, planning, opnieuw proberen en andere taak beheer taken voor u.

Er zijn veel methoden voor monitoring van de uitvoering van taken. De `wait_for_tasks_to_complete` -functie in de *python_tutorial_client.py* biedt een eenvoudig voorbeeld van het controleren van taken voor een bepaalde staat in dit geval de [voltooid] [ py_taskstate] staat.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Stap 7: De uitvoer van de taak downloaden

![Uitvoer van de taak van de opslag te downloaden][7]<br/>

Nu dat de taak is voltooid, kunt u de uitvoer van de taken van Azure opslag downloaden. Dit wordt gedaan met een aanroep van `download_blobs_from_container` in *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] De aanroep van `download_blobs_from_container` geeft aan dat de bestanden moeten worden gedownload naar de basismap in *python_tutorial_client.py* . Altijd deze output locatie wijzigen.

## <a name="step-8-delete-containers"></a>Stap 8: Delete containers

Omdat u in rekening worden gebracht voor de gegevens die zich in opslag in Azure, is het altijd een goed idee om een BLOB's die niet langer nodig zijn voor uw batchtaken verwijderen. In de *python_tutorial_client.py*, dit wordt gedaan met drie aanroepen van [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Stap 9: De taak en de groep verwijderen

In de laatste stap wordt u gevraagd de taak en de toepassingen die zijn gemaakt door het script *python_tutorial_client.py* te verwijderen. Hoewel u niet worden aangerekend voor projecten en taken zelf, u *worden* aangerekend voor computerknooppunten. Wij raden dus aan knooppunten te wijzen, alleen indien nodig. Het verwijderen van ongebruikte toepassingen kan deel uitmaken van uw proces voor onderhoud.

Van de BatchServiceClient [JobOperations] [ py_job] en [PoolOperations] [ py_pool] hebben beide overeenkomstige verwijdering methoden die worden genoemd als u bevestigen:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Houd er rekening mee dat u in rekening worden gebracht voor resources compute - kosten voor het verwijderen van ongebruikte toepassingen te minimaliseren. Let op dat een groep verwijdert alle computerknooppunten binnen die groep en dat alle gegevens op de knooppunten kunnen niet worden teruggezet nadat de groep is verwijderd.

## <a name="run-the-sample-script"></a>Het voorbeeldscript uitvoeren

Bij het uitvoeren van het script *python_tutorial_client.py* van de zelfstudie [codevoorbeeld][github_article_samples], de console-uitvoer is vergelijkbaar met het volgende. Is er een pauze op `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` terwijl computerknooppunten van de toepassingen worden gemaakt, is gestart, en de opdrachten in de groep van toepassingen starttaak worden uitgevoerd. Gebruik de [Azure portal] [ azure_portal] voor het controleren van uw groep computerknooppunten, taak, taken tijdens en na de uitvoering. Gebruik de [Azure portal] [ azure_portal] of de [Microsoft Azure Opslagverkenner] [ storage_explorer] om de opslagbronnen (containers en BLOB's) die zijn gemaakt door de toepassing weer te geven.

>[AZURE.TIP] Het script *python_tutorial_client.py* uitvoeren vanuit de `azure-batch-samples/Python/Batch/article_samples` directory. Wordt een relatief pad voor de `common.helpers` de import, zodat u kunt zien `ImportError: No module named 'common'` als u niet de het script in deze map.

Standaard uitvoeringstijd is **ongeveer 5-7 minuten** tijdens het uitvoeren van het monster in de standaardconfiguratie.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen

Altijd wijzigingen aanbrengen in *python_tutorial_client.py* en *python_tutorial_task.py* om te experimenteren met verschillende compute-scenario's. Probeer bijvoorbeeld een vertraging van de uitvoering toe te voegen aan *python_tutorial_task.py* voor het simuleren van langdurige taken en controleren ze in de portal. Probeer meer taken toevoegen of aanpassen van het aantal knooppunten berekenen. Toevoegen van logica om te controleren en het gebruik van een bestaande toepassingen sneller worden uitgevoerd.

Nu u bekend bent met de eenvoudige werkstroom van een Batch-oplossing, is het tijd om zich te verdiepen de extra functies van de Batch-service.

- Lees het artikel [Overzicht Azure Batch-functies](batch-api-basics.md) , die het beste als u nog geen ervaring met de service.
- Start op de andere partij ontwikkeling artikelen in **diepgaande ontwikkeling** in de [Batch leren pad][batch_learning_path].
- Kijk dan eens een andere implementatie van de verwerking van de werkbelasting 'top N woorden' met Batch in de [TopNWords] [ github_topnwords] monster.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Containers in Azure opslag maken"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Toepassing van de taak en de invoer (gegevens) bestanden uploaden naar containers"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Batch-toepassingen maken"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Batchverwerking maken"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Taken toevoegen aan project"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Monitortaken"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Uitvoer van de taak van de opslag te downloaden"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Batch-oplossing-workflow (volledige diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Batch-referenties in de Portal"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Opslag van referenties in de Portal"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Batch-oplossing-workflow (minimale diagram)"
