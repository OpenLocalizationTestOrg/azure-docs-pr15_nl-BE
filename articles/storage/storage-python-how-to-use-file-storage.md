<properties
    pageTitle="Het gebruik van Azure bestandsopslag vanuit Python | Microsoft Azure"
    description="Informatie over het gebruik van de Azure opslag van Python te uploaden, lijst, downloaden en bestanden verwijderen."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Het gebruik van Azure bestandsopslag vanuit Python

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's voor het opslaan van bestanden met. De monsters zijn geschreven in Python en de [SDK van Microsoft Azure opslag voor Python]gebruiken. De scenario's die gedekt zijn, aanbieding uploaden, downloaden en verwijderen van bestanden.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Een share maken

Het **FileService** -object kunt u werken met shares, mappen en bestanden. De volgende code maakt een **FileService** -object. De volgende boven aan een Python-bestand waarin u via programmering toegang tot opslag Azure wilt toevoegen.

    from azure.storage.file import FileService

De volgende code maakt een **FileService** -object met behulp van de sleutel opslag account naam en account.  'Mijnaccount' en 'mykey' vervangen door uw naam en sleutel.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

In het volgende codevoorbeeld kunt u een **FileService** -object om de share te maken als deze niet bestaat.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Upload een bestand naar een share

Een opslag van Azure bestandsshare bevat in ieder geval een hoofdmap waar bestanden kunnen zich bevinden. In dit gedeelte leert u het uploaden van een bestand van lokale opslag naar de hoofdmap van een share.

Gebruik wilt maken van een bestand en uploaden van gegevens, de **maken\_bestand\_van\_pad**, **maken\_bestand\_van\_stream**, **maken\_bestand\_van\_bytes** of **maken\_bestand\_van\_tekst** methoden. Ze zijn op hoog niveau methoden voor het uitvoeren van de noodzakelijke logische groepen te verdelen wanneer de grootte van de gegevens die langer is dan 64 MB.

**maken\_bestand\_van\_pad** de inhoud van het opgegeven pad van een bestand uploadt en **maken\_bestand\_van\_stroom** uploadt u de inhoud van een reeds geopende bestandsstroom. **maken\_bestand\_van\_bytes** uploadt een matrix van bytes en **maken\_bestand\_van\_tekst** uploadt u de waarde van de opgegeven tekst met de opgegeven codering (standaard wordt UTF-8).

In het volgende voorbeeld wordt de inhoud van het bestand **sunset.png** geüpload in het bestand **mijnbestand** .

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Procedure: een map maken

Ook kunt u de opslag indelen plaatst bestanden in onderliggende mappen in plaats van ze allemaal in de hoofdmap. De Azure bestand storage-service kunt u zoveel mappen maken als uw account toestaat. De volgende code maakt u een submap met de naam **sampledir** onder de hoofdmap.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Procedure: bestanden en mappen in een lijst

U kunt de bestanden en mappen in een share gebruiken de **lijst\_mappen\_en\_bestanden** methode. Deze methode retourneert een generator. De volgende code wordt de **naam** van elk bestand en elke map op een share op de console.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Bestanden downloaden

Gebruiken om gegevens uit een bestand te downloaden, **ophalen\_bestand\_te\_pad**, **krijgen\_bestand\_te\_stream**, **ophalen\_bestand\_te\_bytes**, of **krijgen\_bestand\_te\_tekst**. Ze zijn op hoog niveau methoden voor het uitvoeren van de noodzakelijke logische groepen te verdelen wanneer de grootte van de gegevens die langer is dan 64 MB.

Het volgende voorbeeld worden **krijgen\_bestand\_te\_pad** de inhoud van het bestand **mijnbestand** downloaden en opslaan naar het bestand **out sunset.png** .

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Een bestand verwijderen

Ten slotte, als u wilt een bestand verwijdert, bel **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van het opslaan van bestanden, klik op deze koppelingen voor meer informatie.

- [Python voor ontwikkelaars](/develop/python/)
- [Azure opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure opslag-teamblog]
- [Opslag van Azure Microsoft SDK for Python]

[Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Opslag van Azure Microsoft SDK for Python]: https://github.com/Azure/azure-storage-python
