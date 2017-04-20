<properties
    pageTitle="Gegevens verplaatsen en naar Azure Blob-opslag met behulp van AzCopy | Microsoft Azure"
    description="Gegevens verplaatsen en naar Azure Blob-opslag met behulp van AzCopy"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Gegevens verplaatsen en naar Azure Blob-opslag met behulp van AzCopy

AzCopy is een opdrachtregelprogramma dat is ontworpen voor het uploaden, downloaden en kopiëren van gegevens naar en van Microsoft Azure blob-bestand en tabelopslag.

Zie [Aan de slag met het hulpprogramma AzCopy voor opdrachtregel](../storage/storage-use-azcopy.md)voor instructies over het installeren van AzCopy en aanvullende informatie over het gebruik van het met de Azure-platform.

Advies over technologieën die worden gebruikt om gegevens te verplaatsen naar of uit de Azure Blob-opslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Als u van VM die is ingesteld met de scripts die [gegevens wetenschap virtuele machines in Azure gebruikmaakt](machine-learning-data-science-virtual-machines.md), is vervolgens AzCopy al geïnstalleerd op de VM.

> [AZURE.NOTE] Verwijzen naar [Azure Blob grondbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-service voor een volledige inleiding op Azure blob-opslag.


## <a name="prerequisites"></a>Vereisten

Dit document wordt ervan uitgegaan dat u een abonnement op Azure, een opslag-account en de bijbehorende sleutel opslag voor die account. Voor het uploaden/downloaden van gegevens, moet u uw account Azure opslag de sleutel de naam en account weten.

- Zie instellen van een Azure-abonnement, [gratis proefperiode van een maand](https://azure.microsoft.com/pricing/free-trial/).

- Zie voor instructies over het maken van een account voor opslag en voor rekening en belangrijke informatie ophalen [over Azure opslag rekeningen](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>AzCopy opdrachten uitvoeren

AzCopy als opdrachten wilt uitvoeren, opent u een opdrachtvenster en navigeer naar de map AzCopy installeren op uw computer, waar de uitvoerbare AzCopy.exe zich bevindt. 

De basissyntaxis voor opdrachten van AzCopy is:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] U kunt de locatie van de AzCopy-installatie toevoegen aan het systeempad en vervolgens de opdrachten uitvoeren vanuit een andere map. AzCopy wordt standaard geïnstalleerd in *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* of *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.

## <a name="upload-files-to-an-azure-blob"></a>Bestanden uploaden naar een Azure blob

Als u wilt een bestand uploadt, gebruik de volgende opdracht:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Bestanden downloaden van een Azure blob

Als u wilt een bestand downloaden van een Azure blob, gebruik de volgende opdracht:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>BLOB's overbrengen tussen Azure containers

Om BLOB's tussen Azure containers, gebruikt u de volgende opdracht:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tips voor het gebruik van AzCopy

> [AZURE.TIP]   
> 1. **Uploaden** van bestanden, */S* uploads wanneer bestanden recursief. Zonder deze parameter worden bestanden in de submappen niet geüpload.  
> 2. Wanneer de container recursief **downloaden** bestand */S* totdat alle bestanden in de opgegeven map en alle submappen of alle bestanden die overeenkomen met het opgegeven patroon in de opgegeven map en de bijbehorende submappen gezocht, worden gedownload.  
> 3.  U kunt een **specifieke blob-bestand** te downloaden met behulp van de parameter */Source* niet opgeven. Geef de naam blob-bestand te downloaden met behulp van de parameter */Pattern* een bepaald bestand te downloaden. De parameter **/S** kan hebben gezocht naar een bestand naam patroon recursief AzCopy worden gebruikt. Zonder de parameter patroon downloaden AzCopy van alle bestanden in die map.
