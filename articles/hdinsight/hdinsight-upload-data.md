<properties
    pageTitle="Uploaden van gegevens voor Hadoop projecten in HDInsight | Microsoft Azure"
    description="Informatie over het uploaden en toegang tot gegevens voor Hadoop projecten in HDInsight met behulp van de CLI Azure, Azure Opslagverkenner, Azure PowerShell, de opdrachtregel Hadoop of Sqoop."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Uploaden van gegevens voor Hadoop projecten in HDInsight

Azure HDInsight biedt een complete Hadoop gedistribueerd bestandssysteem (HDFS) via Azure Blob-opslag. Het is ontworpen als een extensie HDFS een naadloze oplossing om klanten te voorzien. U kunt de volledige set van componenten in het Hadoop-ecosysteem rechtstreeks voor het beheren van gegevens. Azure Blob-opslag en HDFS zijn afzonderlijke bestandssystemen die geoptimaliseerd zijn voor de opslag van gegevens en berekeningen op deze gegevens. Zie voor informatie over de voordelen van het gebruik van Azure Blob-opslag [Gebruik Azure Blob-opslag met HDInsight][hdinsight-storage].

**Vereisten**

Houd rekening met de volgende vereiste voordat u begint:

* Een cluster Azure HDInsight. Zie voor instructies [aan de slag met Azure HDInsight] [ hdinsight-get-started] of [clusters bepaling HDInsight][hdinsight-provision].

##<a name="why-blob-storage"></a>Waarom blob-opslag?

Azure HDInsight clusters meestal worden ingezet om uit te voeren taken MapReduce en clusters zijn verbroken nadat deze taken hebt voltooid. Houden van de gegevens in de HDFS is clusters nadat berekeningen voltooid zijn een dure manier om deze gegevens op te slaan. Azure Blob-opslag is een hoge beschikbaarheid, uiterst schaalbaar, hoge capaciteit, lage kosten en deelbaar opslagoptie voor gegevens die worden verwerkt met behulp van HDInsight. Gegevens opslaan in een blob kan de clusters HDInsight die worden gebruikt voor het berekenen van de veilig zonder gegevensverlies worden vrijgegeven.

###<a name="directories"></a>Mappen

Opslaan van gegevens als de sleutel/waarde-paren Azure Blob storage containers en er is geen directory-hiërarchie. Maar kan het teken '/' in de naam van de sleutel worden gebruikt zodat deze worden weergegeven als een bestand wordt opgeslagen in een mapstructuur. HDInsight ziet deze als feitelijke mappen.

De sleutel van een blob kan bijvoorbeeld *input/log1.txt*zijn. Er is geen werkelijke "input" directory bestaat, maar vanwege de aanwezigheid van het teken '/' in de naam van de sleutel, heeft het uiterlijk van een pad.

Daarom, als u extra Explorer Azure mogelijk enkele bestanden van 0 bytes. Deze bestanden hebben twee doelen:

- Als er lege mappen, markeer ze van het bestaan van de map. Azure Blob-opslag is doeltreffend te weten dat als een blob genoemd foo/bar aanwezig is, er een map met de naam **foo is**. Maar de enige manier om aan te duiden een lege map met de naam **foo** is door dit bestand met speciale 0 bytes in plaats.

- Ze bevatten speciale metagegevens die nodig is voor het bestandssysteem Hadoop, met name de machtigingen en de eigenaars van de mappen.

##<a name="command-line-utilities"></a>Opdrachtregelprogramma 's

Microsoft biedt de volgende functies werken met Azure Blob-opslag:

| Gereedschap | Linux | OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure Interface met opdrachtregel][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoop, opdracht](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Hoewel de CLI Azure, Azure PowerShell en AzCopy kunnen alle van buitenaf Azure, het Hadoop-opdracht is alleen beschikbaar op de cluster HDInsight en kan alleen gegevens laden vanaf het lokale bestandssysteem in Azure Blob-opslag worden gebruikt.

###<a id="xplatcli"></a>Azure CLI

De CLI Azure is een cross-platform-hulpprogramma waarmee u Azure services beheren. Gebruik de volgende stappen uit voor het uploaden van gegevens naar Azure Blob-opslag:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installeren en configureren van de CLI Azure voor Mac, Linux en Windows](../xplat-cli-install.md).

2. Open een opdrachtprompt, bash of andere shell en gebruikt u het volgende om uw abonnement op Azure te verifiëren.

        azure login

    Wanneer dat wordt gevraagd, voert u de gebruikersnaam en het wachtwoord voor uw abonnement.

3. Voer de volgende opdracht om de rekeningen van de opslag voor uw abonnement:

        azure storage account list

4. Selecteer de opslag rekening met de label die u wilt werken en de volgende opdracht gebruiken voor het ophalen van de sleutel voor deze account:

        azure storage account keys list <storage-account-name>

    Dit moet de **primaire** en **secundaire** sleutels retourneren. De **primaire** -sleutelwaarde niet kopiëren omdat het wordt gebruikt in de volgende stappen.

5. Gebruik de volgende opdracht als u een lijst van blob-containers in de opslag-account op te halen:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. De volgende opdrachten gebruiken voor het uploaden en downloaden van bestanden naar de blob:

    * Een bestand te uploaden:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Een bestand te downloaden:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Als u altijd met dezelfde account opslag werkt, stelt u de volgende omgevingsvariabelen in plaats van de account en sleutel voor elke opdracht:
>
> * **AZURE\_opslag\_ACCOUNT**: de naam van de opslag
>
> * **AZURE\_opslag\_ACCESS\_sleutel**: de sleutel opslag account

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell is een scriptomgeving kunt u de implementatie en het beheer van uw werkbelasting in Azure te automatiseren. Zie voor meer informatie over het configureren van uw werkstation Azure PowerShell uitvoeren [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Een lokaal bestand uploaden naar Azure Blob-opslag**

1. Open de console Azure PowerShell volgens de instructies [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
2. De waarden van de eerste vijf variabelen instellen in het volgende script:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Plak het script in de console Azure PowerShell uit te voeren om het bestand te kopiëren.

PowerShell scripts gemaakt om te werken met HDInsight, Zie bijvoorbeeld [Extra HDInsight](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy is een opdrachtregelprogramma dat is ontworpen om de taak van het overbrengen van gegevens naar en van een account Azure opslag te vereenvoudigen. U kunt gebruiken als een zelfstandig hulpprogramma of dit programma in een bestaande toepassing opnemen. [Download AzCopy][azure-azcopy-download].

De syntaxis van de AzCopy is:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Zie voor meer informatie [AzCopy - uploaden/downloaden van bestanden voor Azure BLOB's][azure-azcopy].


###<a id="commandline"></a>Hadoop vanaf de opdrachtregel

De opdrachtregel Hadoop is alleen nuttig voor het opslaan van gegevens in een blob-opslag als de gegevens al aanwezig op het hoofd clusterknooppunt is.

Om de opdracht Hadoop gebruikt, moet u eerst verbinding maken met de headnode met behulp van een van de volgende methoden:

* **HDInsight op basis van Windows**: [verbinding maken via Extern bureaublad](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **HDInsight op basis van Linux**: verbinding maken via SSH ([de opdracht SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) of [stopverf](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Wanneer een verbinding, kunt u de volgende syntaxis om een bestand te uploaden naar de opslag.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Bijvoorbeeld:`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Omdat het standaardbestandssysteem voor HDInsight in Azure Blob-opslag, zich /example/data.txt in Azure Blob-opslag. U kunt ook verwijzen naar het bestand als:

    wasbs:///example/data/data.txt

of

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Zie [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html) voor een lijst van andere Hadoop-opdrachten die met bestanden werken.

> [AZURE.WARNING] Op HBase clusters, de standaard blokgrootte gebruikt bij het schrijven van gegevens is 256KB. Terwijl dit prima met HBase APIs of REST API's werkt, via de `hadoop` of `hdfs dfs` opdrachten te schrijven gegevens meer dan ~ 12GB resulteert in een fout. Zie het volgende gedeelte [voor het schrijven van blob-opslag uitzondering](#storageexception) voor meer informatie.

##<a name="graphical-clients"></a>Grafische-clients

Er zijn verschillende toepassingen die een grafische interface voor het werken met Azure opslag bieden. Hier volgt een lijst van enkele van deze toepassingen:

| Client | Linux | OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio Tools voor HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure Opslagverkenner](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Cloud opslag Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for HDInsight

Zie [Navigeren de gekoppelde bronnen](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources)voor meer informatie.

###<a id="storageexplorer"></a>Azure Opslagverkenner

*Opslagverkenner Azure* is een handig hulpprogramma voor het inspecteren en wijzigen van de gegevens in de BLOB's. Het is een gratis, open-source programma dat kan worden gedownload van [http://storageexplorer.com/](http://storageexplorer.com/). De broncode is beschikbaar via deze link ook.

Voordat u het hulpprogramma gebruikt, moet u uw account Azure opslag de sleutel de naam en account weten. Zie voor meer informatie over het verkrijgen van deze informatie de "hoe: toegangstoetsen weergeven, kopiëren en opnieuw genereren van opslag" sectie [maken, beheren, of verwijderen van een account voor de opslag]van de[azure-create-storage-account].  

1. Azure Opslagverkenner worden uitgevoerd. Als dit de eerste keer dat u de Opslagverkenner is uitgevoerd, wordt u gevraagd voor de ___accountnaam voor opslag__ en __opslag account sleutel__. Als u hebt uitgevoerd het vóór gebruik de knop __toevoegen__ om een nieuwe naam voor opslag en de sleutel toevoegen.

    Voer de naam en de sleutel voor de opslag die wordt gebruikt door het cluster HDinsight en selecteer vervolgens __Opslaan en openen__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Klik op de naam van de container die is gekoppeld aan het cluster HDInsight in de lijst van containers links van de interface. Dit is standaard de naam van het cluster HDInsight, maar mogelijk anders als u een specifieke naam ingevoerd bij het maken van het cluster.

6. Selecteer het pictogram uploaden van de werkbalk.

    ![Werkbalk met gemarkeerd upload-pictogram](./media/hdinsight-upload-data/toolbar.png)

7. Een bestand wilt uploaden en klik vervolgens op **openen**. Wanneer dat wordt gevraagd, selecteert u __uploaden van__ het bestand te uploaden naar de hoofdmap van de opslag container. Als u wilt dat het bestand te uploaden naar een specifiek pad, voer het pad in het veld __doel__ en selecteer vervolgens __uploaden__.

    ![Het dialoogvenster bestand uploaden](./media/hdinsight-upload-data/fileupload.png)
    
    Nadat het bestand is voltooid, kunt u deze uit projecten in het cluster HDInsight.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Azure Blob-opslag als een lokaal station koppelen

Zie [Mount Azure Blob-opslag als lokaal station](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Services

###<a name="azure-data-factory"></a>Azure Data Factory

De service Azure Data Factory is een volledig beheerde service voor het opstellen van opslag, verwerking en data verkeer van gegevens in een gestroomlijnde, schaalbare en betrouwbare gegevens productie pijpleidingen.

Azure Data Factory kan worden gebruikt om gegevens te verplaatsen naar Azure Blob-opslag of pijpleidingen voor gegevens die direct HDInsight functies zoals component en varken gebruiken maken.

Zie voor meer informatie de [documentatie van Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache-Sqoop

Sqoop is een hulpmiddel dat is ontworpen voor het overbrengen van gegevens tussen Hadoop en relationele databases. U kunt het importeren van gegevens uit een relationele database management system (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in het Hadoop, MapReduce of component transformeren en vervolgens de gegevens exporteren naar een RDBMS.

Zie voor meer informatie, [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Ontwikkeling SDK 's

Azure Blob-opslag kan ook worden geopend met behulp van een SDK Azure uit de volgende programmeertalen:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Zie voor meer informatie over het installeren van de SDK's van Azure [Azure downloads](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a id="storageexception"></a>Uitzondering voor het schrijven van blob Storage

__Symptomen__: bij het gebruik van de `hadoop` of `hdfs dfs` opdrachten voor het schrijven van bestanden die ~ 12 GB zijn of groter op een cluster HBase kunnen de volgende fout optreden: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Oorzaak__: standaard HBase op HDInsight clusters met een blokgrootte van 256 KB bij het schrijven naar Azure opslag. Terwijl dit voor HBase APIs of REST API's werkt, resulteert dit in een fout bij het gebruik van de `hadoop` of `hdfs dfs` opdrachtregelprogramma's.

__Resolutie__: Gebruik `fs.azure.write.request.size` een grotere blokgrootte opgeven. U kunt dit doen op basis van per gebruik met behulp van de `-D` parameter. Het volgende is een voorbeeld met behulp van deze parameter met de `hadoop` opdracht:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

U kunt ook vergroten met de waarde van `fs.azure.write.request.size` wereldwijd via Ambari. De volgende stappen kunnen worden gebruikt de waarde in de gebruikersinterface van de website Ambari te wijzigen:

1. Ga naar de Ambari voor het cluster on line gebruikersinterface in uw browser. Dit is https://CLUSTERNAME.azurehdinsight.net, waarbij de __CLUSTERNAAM__ de naam van het cluster is.

    Voer desgevraagd de naam van de beheerder en het wachtwoord voor de cluster.

2. Selecteer __HDFS__vanaf de linkerkant van het scherm en selecteer vervolgens het tabblad __configuraties__ .

3. Voer in het veld __Filter__ `fs.azure.write.request.size`. En de huidige waarde in het midden van de pagina wordt weergegeven.

4. Wijzig de waarde van 262144 (256KB) past de nieuwe waarde. Bijvoorbeeld 4194304 (4MB).

![Afbeelding van het wijzigen van de waarde door Ambari Web UI](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Zie voor meer informatie over het gebruik van Ambari, [clusters van HDInsight beheren met behulp van de gebruikersinterface van de website Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Volgende stappen

Nu dat u hoe gegevens worden opgehaald in de HDInsight begrijpt, lees de volgende artikelen voor meer informatie over het uitvoeren van analyse:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Hadoop taken programmatisch verzenden][hdinsight-submit-jobs]
* [Gebruik component met HDInsight][hdinsight-use-hive]
* [Varken met HDInsight gebruiken][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
