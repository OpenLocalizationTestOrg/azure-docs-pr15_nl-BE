<properties
    pageTitle="Met behulp van de CLI Azure met Azure opslag | Microsoft Azure"
    description="Informatie over het gebruik van de opdrachtregelinterface (CLI Azure) van Azure met Azure opslag maken en beheren van accounts voor opslag en werken met Azure BLOB's en bestanden. De CLI Azure is een cross-platform "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Met behulp van de CLI Azure met Azure opslag

## <a name="overview"></a>Overzicht

De CLI Azure biedt een set van open source platforms opdrachten voor het werken met de Azure-Platform. Het biedt vrijwel dezelfde functionaliteit gevonden in de [Azure portal](https://portal.azure.com) , alsmede de toegang tot de functionaliteit van gegevens.

In deze handleiding wordt besproken [Azure opdrachtregelinterface (CLI Azure)](../xplat-cli-install.md) gebruiken voor het uitvoeren van allerlei ontwikkeling en het beheer van taken met Azure opslag. Wij raden aan dat u downloaden en installeren of naar de nieuwste Azure CLI upgraden voordat u deze handleiding.

Deze handleiding wordt ervan uitgegaan dat u de basisconcepten van Azure opslag begrijpt. De handleiding bevat een aantal scripts om aan te tonen het gebruik van de CLI Azure met Azure opslag. Zorg ervoor dat de scriptvariabelen op basis van uw configuratie voor elk script wordt uitgevoerd.

> [AZURE.NOTE] De handleiding bevat de Azure CLI command en script voorbeelden voor klassieke opslag rekeningen. Zie [met behulp van de CLI Azure voor Mac, Linux en Windows met Azure Resource Management](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) voor Azure CLI opdrachten voor Resource Manager opslag rekeningen.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Aan de slag met Azure opslag en de CLI Azure in 5 minuten

Deze handleiding Ubuntu gebruikt voor voorbeelden, maar andere OS platforms op dezelfde manier moeten worden uitgevoerd.

**Met Azure:** Een abonnement op Microsoft Azure en een Microsoft-account is gekoppeld aan het abonnement ophalen. Zie voor informatie over opties voor aanschaf Azure, [Gratis](https://azure.microsoft.com/pricing/free-trial/) [Opties kopen](https://azure.microsoft.com/pricing/purchase-options/)en [Biedt lid](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, de Microsoft Partner Network en BizSpark en andere Microsoft-programma's).

Zie [rollen van beheerder toewijzen in Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) voor meer informatie over abonnementen Azure.

**Na het maken van een account en een abonnement op Microsoft Azure:**

1. Download en installeer de Azure CLI de aanwijzingen in [de Azure CLI installeren](../xplat-cli-install.md).
2. Zodra de CLI Azure is geïnstalleerd, kunt u zijn de azure opdracht vanaf de opdrachtregelinterface (Bash, Terminal, MS-DOS-prompt) gebruiken voor toegang tot de Azure CLI-opdrachten. Type `azure` opdracht en u ziet de volgende uitvoer.

    ![Azure opdrachtuitvoer][Image1]

3. Typ in de command line interface `azure storage` lijst met alle opdrachten azure opslag en een eerste indruk van de functies van de CLI Azure biedt. U kunt de naam van de opdracht met de parameter **-h** typen (bijvoorbeeld: `azure storage share create -h`) details van de syntaxis van deze opdracht.
4. Nu krijgt u een eenvoudig script waarin Azure CLI basisopdrachten voor toegang tot opslag Azure. Het script wordt eerst gevraagd twee variabelen voor de opslag en sleutel instellen. Vervolgens wordt het script maakt een nieuwe container in deze nieuwe account voor opslag en (blob) van een bestaand afbeeldingsbestand uploaden naar die container. Nadat u het script geeft een overzicht van alle BLOB's in die container, worden het afbeeldingsbestand naar de doelmap die op de lokale computer bestaat gedownload.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Open uw favoriete teksteditor (vim bijvoorbeeld) in uw lokale computer. Typ het bovenstaande script in een teksteditor.

6. Nu moet u het scriptvariabelen op basis van de configuratie-instellingen bijwerken.

    - **< storage_account_name >** De opgegeven naam gebruiken in het script of voer een nieuwe naam voor de account van uw opslag. **Belangrijk:** De naam van de account van de opslag moet uniek zijn in Azure. Het moet in kleine letters, te!

    - **< storage_account_key >** De toegangssleutel van uw account voor opslag.

    - **< container_name >** De opgegeven naam gebruiken in het script of voer een nieuwe naam voor de container.

    - **< image_to_upload >** Geef een pad naar een afbeelding op uw lokale computer, zoals: ' ~ / images/HelloWorld.png ".

    - **< destination_folder >** Geef een pad naar een lokale map voor het opslaan van bestanden die zijn gedownload van Azure opslag, zoals: '~/downloadImages'.

7. Nadat u de benodigde variabelen in vim hebt bijgewerkt, drukt u op toetscombinaties "Esc,:, wq!" het script opslaan.

8. Dit script wordt uitgevoerd, typt u de naam van het script in de console bash. Nadat dit script wordt uitgevoerd, moet u een lokale map waarin u het gedownloade bestand hebt. De volgende schermafbeelding ziet u een voorbeeld van de uitvoer:

Nadat u het script wordt uitgevoerd, moet u een lokale map waarin u het gedownloade bestand hebt.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Accounts met de CLI Azure opslag beheren

### <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw abonnement Azure

Terwijl de meeste van de opslag-opdrachten zonder een Azure-abonnement werken, raden we u verbinding maakt met uw abonnement via de CLI Azure. De stappen voor het configureren van de CLI Azure werken met uw abonnement in [verbinding maken met een Azure-abonnement van de CLI Azure](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Maak een nieuwe account voor opslag

Azure opslag gebruikt, moet u een account voor opslag. Nadat u uw computer verbinding maakt met uw abonnement hebt geconfigureerd, kunt u een nieuwe account met Azure opslag maken.

        azure storage account create <account_name>

De naam van de account van uw opslag moet tussen 3 en 24 tekens bevatten en gebruik alleen cijfers en kleine letters.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Een standaardaccount Azure opslag in omgevingsvariabelen instellen

U kunt meerdere accounts voor opslag in uw abonnement hebt. U kunt kiezen uit deze en stel deze in de omgevingsvariabelen voor alle opdrachten van de opslag in dezelfde sessie. Hiermee kunt u de Azure CLI opslag-opdrachten uitvoeren zonder de opslag account en sleutel expliciet.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Een andere manier voor het instellen van een standaardaccount voor opslag verbindingsreeks gebruikt. In de eerste plaats de verbindingsreeks opvragen door de opdracht:

        azure storage account connectionstring show <account_name>

Vervolgens kopieert u de verbindingsreeks voor de uitvoer en stel deze in op de omgevingsvariabele:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Maken en beheren van BLOB 's

Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die kan worden geopend vanuit overal in de wereld via HTTP of HTTPS. In dit gedeelte wordt ervan uitgegaan dat u al bekend met de begrippen Azure Blob-opslag bent. Zie [aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md) en [Blob-Service-begrippen](http://msdn.microsoft.com/library/azure/dd179376.aspx)voor meer informatie.

### <a name="create-a-container"></a>Maken van een container

Elke blob in Azure opslag moet in een container. Kunt u een privé-container met behulp van de `azure storage container create` opdracht:

        azure storage container create mycontainer

> [AZURE.NOTE] Er zijn drie niveaus van anonieme leestoegang: **uit** **Blob**en **Container**. Om te voorkomen dat anonieme toegang tot de BLOB's, de machtiging-parameter in te stellen op **Off**. Standaard is de nieuwe container is persoonlijk en kan alleen worden benaderd door de eigenaar van de account. Anonieme openbare leestoegang tot de blob-bronnen, maar niet in de metagegevens van de container of het aantal BLOB's in de container, de machtiging parameter instellen op **Blob**toestaan. Stel de parameter machtiging **container**wilt toestaan volledige openbare leestoegang tot de blob-bronnen, container metagegevens en de lijst van BLOB's in de container. Zie [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)voor meer informatie.

### <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Azure Blob-opslag ondersteunt BLOB's blokkeren en BLOB's pagina. Zie [Wat blok BLOB's, BLOB's toevoegen en BLOB's pagina](http://msdn.microsoft.com/library/azure/ee691964.aspx)voor meer informatie.

Als u wilt uploaden BLOB's in een container, kunt u de `azure storage blob upload`. Met deze opdracht kunnen standaard lokale bestanden uploaden naar een blob blokkeren. Als u wilt opgeven welk type voor de blob, kunt u de `--blobtype` parameter.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>BLOB's downloaden van een container

In het volgende voorbeeld laat zien hoe BLOB's downloaden van een container.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>BLOB's kopiëren

U kunt de BLOB's binnen of tussen verschillende accounts voor opslag en regio's asynchroon kopiëren.

In het volgende voorbeeld laat zien hoe BLOB's van de ene account voor opslag naar de andere kopiëren. In dit voorbeeld maken we een container waarin BLOB's openbaar zijn, anoniem toegankelijk.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

In dit voorbeeld wordt een kopie van de asynchrone uitgevoerd. U kunt de status van elke kopieerbewerking controleren door het uitvoeren van de `azure storage blob copy show` bewerking.

Opmerking die de bron-URL voor het kopiëren van bestanden zijn toegankelijk voor het publiek, of een token SAS (gedeelde toegang handtekening) opnemen.

### <a name="delete-a-blob"></a>Een blob verwijderen

U een blob verwijderen met de volgende opdracht:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Maken en beheren van gedeelde bestanden

Azure bestandsopslag biedt gedeelde opslag voor toepassingen die gebruikmaken van de standaard SMB-protocol. Microsoft Azure virtuele machines en cloud-diensten, alsmede toepassingen in gebouwen, kunnen delen via gekoppelde gedeelde gegevens in een bestand. U kunt gedeelde bestanden en gegevens in een bestand via de Azure CLI beheren. Zie voor meer informatie over de opslag van bestanden in Azure, [aan de slag met Azure bestandsopslag op Windows](storage-dotnet-how-to-use-files.md) of [het gebruik van Azure bestandsopslag met Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Een bestandsshare maken

Een bestandsshare Azure is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bestandsshare. Een account kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden tot een maximum van de capaciteit van de opslag-account worden opgeslagen. In het volgende voorbeeld wordt een gedeeld bestand met de naam **mijnshare**gemaakt.

        azure storage share create myshare

### <a name="create-a-directory"></a>Een map maken

Een directory biedt een optionele hiërarchische structuur voor een bestandsshare Azure. In het volgende voorbeeld wordt de map **mijnmap** op een bestandsshare gemaakt.

        azure storage directory create myshare myDir

Opmerking die directorypad kan bestaan uit meerdere niveaus, *bijvoorbeeld* **een / b**. Echter, moet u ervoor zorgen dat alle bovenliggende mappen bestaat. Bijvoorbeeld voor het pad **een / b**, moet u directory **een** eerst maken en vervolgens directory **b**maken.

### <a name="upload-a-local-file-to-directory"></a>Een lokaal bestand te uploaden naar map

In het volgende voorbeeld een bestand van **~/temp/samplefile.txt** naar de map **mijnmap** geüpload. Het pad naar het bestand bewerken zodat deze naar een geldig bestand op uw lokale computer verwijst:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Houd er rekening mee dat een bestand in de share tot 1 TB in grootte kan worden.

### <a name="list-the-files-in-the-share-root-or-directory"></a>De bestanden in de hoofdmap van de share of map weergeven

U kunt bestanden en submappen in de hoofdmap van een share of map met de volgende opdracht in de lijst:

        azure storage file list myshare myDir

Houd er rekening mee dat de naam van de map optioneel voor de bewerking van de aanbieding is. Als dit argument wordt weggelaten, worden de opdracht de inhoud van de hoofdmap van de share.

### <a name="copy-files"></a>Bestanden kopiëren

Vanaf versie 0.9.8 van Azure CLI, kunt u een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. Hieronder gedemonstreerd hoe u deze kopieerbewerkingen via CLI opdrachten uitvoeren. Een bestand kopiëren naar de nieuwe map:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Een blob naar een map kopiëren:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Volgende stappen

Hier zijn enkele verwante artikelen en bronnen voor meer informatie over de opslag van Azure.

- [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
- [Azure opslag REST API: naslag](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
