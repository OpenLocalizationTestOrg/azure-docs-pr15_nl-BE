<properties
    pageTitle="Met Azure PowerShell met Azure opslag | Microsoft Azure"
    description="Informatie over het gebruik van de Azure PowerShell-cmdlets voor Azure opslag maken en beheren van accounts voor opslag; werken met BLOB's, tabellen, wachtrijen en bestanden. configureren en opslag analytics opvragen en gedeelde toegang handtekeningen maken."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Met Azure PowerShell en Azure opslag

## <a name="overview"></a>Overzicht

Azure PowerShell is een module waarmee u de cmdlets voor het beheren van Azure via Windows PowerShell. Het is een taak gebaseerde opdrachtregel shell en scripttaal speciaal ontworpen voor systeembeheer. Met PowerShell, kunt u gemakkelijk instellen en het beheer van uw Azure services en toepassingen. U kunt bijvoorbeeld de cmdlets voor het uitvoeren van de taken die u via de [Portal Azure uitvoeren kunt](https://portal.azure.com).

In deze handleiding, zullen we de [Azure opslag Cmdlets](https://msdn.microsoft.com/library/azure/mt269418.aspx) gebruiken voor het uitvoeren van allerlei ontwikkeling en het beheer van taken met Azure opslag verkennen.

Deze handleiding wordt ervan uitgegaan dat u ervaring met [Opslag Azure](https://azure.microsoft.com/documentation/services/storage/) en [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). De handleiding bevat een aantal voorbeelden van het gebruik van opslag Azure PowerShell scripts. Het scriptvariabelen op basis van uw configuratie voordat u elk script moet worden bijgewerkt.

De eerste sectie van deze handleiding biedt een snelle blik op Azure opslag en PowerShell. Start van de [vereisten voor het gebruik van Azure PowerShell met Azure opslag](#prerequisites-for-using-azure-powershell-with-azure-storage)voor uitgebreide informatie en instructies.


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Aan de slag met Azure opslag en PowerShell in 5 minuten

In deze sectie wordt beschreven hoe u toegang tot opslag via PowerShell Azure in 5 minuten.

**Met Azure:** Een abonnement op Microsoft Azure en een Microsoft-account is gekoppeld aan het abonnement ophalen. Zie voor informatie over opties voor aanschaf Azure, [Gratis](https://azure.microsoft.com/pricing/free-trial/) [Opties kopen](https://azure.microsoft.com/pricing/purchase-options/)en [Biedt lid](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, de Microsoft Partner Network en BizSpark en andere Microsoft-programma's).

Zie [rollen van beheerder toewijzen in Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) voor meer informatie over abonnementen Azure.

**Na het maken van een account en een abonnement op Microsoft Azure:**

1.  Download en installeer [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Start Windows PowerShell Integrated Scripting Environment (ISE): In de lokale computer, gaat u naar het menu **Start** . Typ **Systeembeheer** en klik op uit te voeren. Klik met de rechtermuisknop op **Windows PowerShell ISE**in het venster **Hulpprogramma's voor systeembeheer** , klik op **als Administrator uitvoeren**.
3.  Klik op **bestand**in **Windows PowerShell ISE**, > ,**Nieuw** om een nieuw scriptbestand te maken.
4.  Nu krijgt u een eenvoudig script waarin de elementaire PowerShell-opdrachten voor toegang tot opslag Azure. Het script wordt eerst gevraagd de Azure accountreferenties toevoegen uw Azure account in de lokale omgeving van PowerShell. Het script wordt vervolgens de standaard Azure abonnement en maak een nieuwe account voor opslag in Azure. Vervolgens wordt het script maakt een nieuwe container in deze nieuwe account voor opslag en (blob) van een bestaand afbeeldingsbestand uploaden naar die container. Nadat u het script geeft een overzicht van alle BLOB's in die container, worden een nieuwe doelmap in uw lokale computer maken en downloaden van het afbeeldingsbestand.
5.  Selecteer het script dat tussen de opmerkingen in de volgende sectie code **#beginnen met** en **#end**. Druk op CTRL + C om deze te kopiëren naar het Klembord.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  Druk op CTRL + V om te kopiëren van het script in **Windows PowerShell ISE**. Klik op **bestand** > **Opslaan**. Typ in het dialoogvenster **OpslaanAls** de naam van het scriptbestand bijvoorbeeld 'mystoragescript'. Klik op **Opslaan**.

6.  Nu moet u het scriptvariabelen op basis van de configuratie-instellingen bijwerken. U moet de variabele **$SubscriptionName** met uw eigen abonnement bijwerken. U kunt variabelen zoals opgegeven in het script behouden of als u wilt bijwerken.

    - **$SubscriptionName:** U moet deze variabele met de naam van uw eigen abonnement bijwerken. Voer een van de volgende drie manieren om te zoeken naar de naam van uw abonnement:

        een. Klik op **bestand**in **Windows PowerShell ISE**, > ,**Nieuw** om een nieuw scriptbestand te maken. Kopieer het volgende script naar het nieuwe scriptbestand en klik op **Foutopsporing** > **uitvoeren**. Het volgende script wordt eerst gevraagd de Azure accountreferenties uw Azure toevoegen naar de lokale omgeving van PowerShell en vervolgens alle abonnementen die zijn aangesloten op de lokale PowerShell-sessie weergeven. Noteer de naam van het abonnement dat u gebruiken wilt terwijl u deze zelfstudie:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Als u wilt zoeken en de abonnementsnaam van uw te kopiëren in de [Portal Azure](https://portal.azure.com), in de Hub menu aan de linkerkant, klikt u op **abonnementen**. Kopieer de naam van het abonnement dat u gebruiken wilt tijdens het uitvoeren van de scripts in deze handleiding.

        ![Azure Portal][Image2]

        c. Schuif naar beneden om te zoeken en de abonnementsnaam van uw in de [Klassieke Portal Azure](https://manage.windowsazure.com/)kopiëren, en klik op **Instellingen** op de linkerkant van de portal. Klik op **abonnementen** voor een overzicht van uw abonnementen. Kopieer de naam van het abonnement dat u gebruiken wilt tijdens het uitvoeren van de scripts die in deze handleiding.

        ![Azure klassieke Portal][Image1]

    - **$StorageAccountName:** De opgegeven naam gebruiken in het script of voer een nieuwe naam voor de account van uw opslag. **Belangrijk:** De naam van de account van de opslag moet uniek zijn in Azure. Het moet in kleine letters, te!

    - **$Location:** Bepaalde "West Nederland" in het script te gebruiken of kies andere Azure locaties, zoals de Verenigde Staten Oost-, Noord-Europa, enzovoort.

    - **$ContainerName:** De opgegeven naam gebruiken in het script of voer een nieuwe naam voor de container.

    - **$ImageToUpload:** Geef een pad naar een afbeelding op uw lokale computer, zoals: 'C:\Images\HelloWorld.png'.

    - **$DestinationFolder:** Geef een pad naar een lokale map voor het opslaan van bestanden die zijn gedownload van Azure opslag, zoals: 'C:\DownloadImages'.

7.  Na het bijwerken van de scriptvariabelen in het bestand 'mystoragescript.ps1', klikt u op het **bestand** > **Opslaan**. Klik vervolgens op **Foutopsporing** > **uitvoeren** of druk op **F5** om het script uitvoert.  

Nadat u het script wordt uitgevoerd, moet u een lokale map waarin u het gedownloade bestand hebt. De volgende schermafbeelding ziet u een voorbeeld van de uitvoer:

![BLOB's downloaden][Image3]


> [AZURE.NOTE] De sectie "Aan de slag met Azure opslag en PowerShell in 5 minuten" beschikbaar: een korte inleiding op Azure PowerShell gebruiken met Azure opslag. Voor uitgebreide informatie en instructies raden we u aan het lezen in de volgende secties.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Vereisten voor het gebruik van Azure PowerShell met Azure opslag
U moet een Azure abonnement en account die u wilt uitvoeren de PowerShell-cmdlets die in deze handleiding, zoals hierboven beschreven.

Azure PowerShell is een module waarmee u de cmdlets voor het beheren van Azure via Windows PowerShell. Zie voor meer informatie over het installeren en instellen van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Wij raden aan dat downloaden en installeren of naar de nieuwste Azure PowerShell module upgraden voordat u deze handleiding.

U kunt de cmdlets kunt uitvoeren in de standaard Windows PowerShell-console of de Windows PowerShell Integrated Scripting Environment (ISE). Bijvoorbeeld als u wilt openen in **Windows PowerShell ISE**, Ga naar het menu Start, typ Systeembeheer en op uit te voeren. Klik met de rechtermuisknop op Windows PowerShell ISE in het venster hulpprogramma's voor systeembeheer, klik op uitvoeren als beheerder.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Het beheren van accounts in Azure opslag

### <a name="how-to-set-a-default-azure-subscription"></a>Het instellen van een standaard Azure abonnement
Azure-opslag met Azure PowerShell beheren, moet u uw clientomgeving met Azure via Azure Active Directory-verificatie of verificatie op basis van certificaten verifiëren. Zie de handleiding voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor gedetailleerde informatie. Deze handleiding gebruikt de Azure Active Directory-verificatie.

1.  Typ de volgende opdracht om de Azure toevoegen in Windows PowerShell ISE account in de lokale omgeving van PowerShell:

    `Add-AzureAccount`

2.  Typ in het venster 'Sign in naar Microsoft Azure' e-mailadres en wachtwoord aan uw account gekoppeld. Azure verifieert de referentie-informatie wordt opgeslagen en vervolgens wordt het venster gesloten.

3.  Voer de volgende opdracht om de Azure accounts in uw lokale omgeving van PowerShell weergeven en controleer of uw account wordt vermeld:

    `Get-AzureAccount`

4.  Vervolgens de volgende cmdlet wilt weergeven van alle abonnementen die zijn aangesloten op de lokale PowerShell-sessie worden uitgevoerd en controleer of uw abonnement:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Standaard Azure abonnement stelt de cmdlet selecteren AzureSubscription worden uitgevoerd:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Controleer of de naam van het standaardabonnement door de cmdlet Get-AzureSubscription uit te voeren:

    `Get-AzureSubscription -Default`

7.  Overzicht van de beschikbare PowerShell cmdlets voor opslag in Azure, uitvoeren:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Het maken van een nieuwe account met Azure opslag
Azure opslag gebruikt, moet u een account voor opslag. Nadat u uw computer verbinding maakt met uw abonnement hebt geconfigureerd, kunt u een nieuwe account met Azure opslag maken.

1.  De cmdlet Get-AzureLocation om te zoeken naar alle beschikbare datacenter locaties uitvoeren:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Voer de cmdlet New-AzureStorageAccount om een nieuwe opslag-account te maken. In het volgende voorbeeld wordt een nieuwe account voor opslag in het datacenter 'US West' gemaakt.

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] De naam van de account van uw opslag moet uniek zijn binnen de Azure en moet in kleine letters. Zie [Azure opslag rekeningen](storage-create-storage-account.md) en [Naming en verwijzen naar Containers, BLOB's, en metagegevens](http://msdn.microsoft.com/library/azure/dd135715.aspx)voor naamgevingsregels en beperkingen.

### <a name="how-to-set-a-default-azure-storage-account"></a>Het instellen van een standaard Azure opslag account
U kunt meerdere accounts voor opslag in uw abonnement hebt. U kunt een van deze kiezen en instellen als de standaard opslag-account voor alle opdrachten van de opslag in dezelfde sessie PowerShell. Hiermee kunt u de opslag Azure PowerShell-opdrachten uitvoeren zonder de context opslag expliciet op te geven.

1.  Als u wilt dat een standaardaccount opslag voor uw abonnement, kunt u de cmdlet Set-AzureSubscription uitvoeren.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Voer de cmdlet Get-AzureSubscription om te controleren of de opslag-account is gekoppeld aan uw account standaard abonnement. Deze opdracht retourneert de eigenschappen van het abonnement op het huidige abonnement, met inbegrip van de huidige account voor opslag.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Hoe u alle accounts voor Azure opslag in een abonnement
Elk abonnement Azure kan maximaal 100 opslag accounts hebben. Zie voor de meest actuele gegevens over limieten, [Azure abonnement Service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md).

Voer de volgende cmdlet om erachter te komen met de naam en status van de rekeningen van de opslag in het huidige abonnement:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Het maken van een context Azure opslag
Azure opslag context is een object in PowerShell voor het inkapselen van de referenties van de opslag. Met behulp van een context opslag tijdens eventuele latere cmdlet wordt uitgevoerd, u kunt voor de verificatie van uw aanvraag zonder de account voor de opslag en de toegangssleutel expliciet. Op veel manieren, zoals het gebruik van opslag account naam en toegangssleutel, gedeelde toegangstoken van handtekening (SAS), de verbindingsreeks, kunt u een context opslag maken of anonieme. Zie [Nieuw AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx)voor meer informatie.  

Gebruik een van de volgende drie manieren voor het maken van een context voor opslag:

- Voer de cmdlet [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) en ontdek de toegangstoets primaire opslag voor uw account Azure opslag. Daarna roept de cmdlet [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) om de context van een opslag maken:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Genereren van een token gedeelde toegang handtekening voor een container Azure opslag en de context van een opslag maken:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Zie voor meer informatie, [Nieuwe AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) en het [Gebruik van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

- In sommige gevallen wilt u mogelijk de eindpunten opgeven bij het maken van een nieuwe context voor opslag. Dit kan nodig zijn wanneer u een aangepaste domeinnaam voor uw opslag-account hebt geregistreerd met de Blob-service of u een gedeelde access-handtekening gebruiken wilt voor toegang tot de opslagbronnen. De service-endpoints in een verbindingsreeks instellen en een nieuwe context voor opslag maken, zoals hieronder wordt weergegeven:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Zie voor meer informatie over het configureren van een verbindingsreeks opslag [Verbindingsreeksen configureren](storage-configure-connection-string.md).

Nu dat u hebt ingesteld op uw computer en hebt geleerd hoe u abonnementen en opslag rekeningen met Azure PowerShell beheren, gaat u naar de volgende sectie voor informatie over het beheren van Azure BLOB's en momentopnamen blob.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Het ophalen en opnieuw genereren van sleutels Azure opslag

Een account Azure opslag wordt geleverd met twee sleutels van de account. De volgende cmdlet kunt u uw sleutels ophalen.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Gebruik de volgende cmdlet een specifieke sleutel ophalen. Geldige waarden zijn de primaire en secundaire.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Als u uw sleutels opnieuw genereren wilt, gebruikt u de volgende cmdlet. Geldige waarden voor KeyType - zijn 'Primaire' en 'Secundaire'

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Azure BLOB's beheren
Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die kan worden geopend vanuit overal in de wereld via HTTP of HTTPS. In dit gedeelte wordt ervan uitgegaan dat u al bekend met de begrippen Azure Blob Storage-Service bent. Zie [aan de slag met Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md) en [Blob-Service-begrippen](http://msdn.microsoft.com/library/azure/dd179376.aspx)voor meer informatie.

### <a name="how-to-create-a-container"></a>Het maken van een container
Elke blob in Azure opslag moet in een container. U kunt een persoonlijke container met de cmdlet New-AzureStorageContainer maken:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Er zijn drie niveaus van anonieme leestoegang: **uit** **Blob**en **Container**. Om te voorkomen dat anonieme toegang tot de BLOB's, de machtiging-parameter in te stellen op **Off**. Standaard is de nieuwe container is persoonlijk en kan alleen worden benaderd door de eigenaar van de account. Anonieme openbare leestoegang tot de blob-bronnen, maar niet in de metagegevens van de container of het aantal BLOB's in de container, de machtiging parameter instellen op **Blob**toestaan. Stel de parameter machtiging **container**wilt toestaan volledige openbare leestoegang tot de blob-bronnen, container metagegevens en de lijst van BLOB's in de container. Zie [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)voor meer informatie.

### <a name="how-to-upload-a-blob-into-a-container"></a>Het uploaden van een blob in een fles
Azure Blob-opslag ondersteunt BLOB's blokkeren en BLOB's pagina. Zie [Wat blok BLOB's, BLOB's toevoegen en BLOB's pagina](http://msdn.microsoft.com/library/azure/ee691964.aspx)voor meer informatie.

Als u wilt uploaden BLOB's in een container, kunt u de cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) . Met deze opdracht kunnen standaard lokale bestanden uploaden naar een blob blokkeren. Als u wilt opgeven welk type voor de blob, kunt u de parameter - BlobType.

In het volgende voorbeeld wordt de cmdlet [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) als u de bestanden in de opgegeven map wordt uitgevoerd en vervolgens doorgegeven aan de volgende cmdlet met behulp van de exploitant van de pijpleiding. De cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) lokale bestanden kunnen uploaden naar de container:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Het BLOB's downloaden van een container
In het volgende voorbeeld laat zien hoe BLOB's downloaden van een container. Het voorbeeld maakt eerst een verbinding met Azure opslag met behulp van de context van de account opslag, inclusief de naam van de opslag en de van primaire toegangssleutel. Het voorbeeld wordt vervolgens een blob-verwijzing met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) opgehaald. Het voorbeeld wordt vervolgens de cmdlet [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) BLOB's downloaden naar de lokale map.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>BLOB's van een container voor opslag naar de andere kopiëren
U kunt BLOB's asynchroon kopiëren aan opslag rekeningen en regio's. In het volgende voorbeeld laat zien hoe BLOB's uit een opslag container kopiëren naar een andere in twee verschillende opslag rekeningen. In het voorbeeld wordt eerst variabelen voor de bron- en doeladressen opslag rekeningen en maakt vervolgens een context opslag voor elke rekening. Het voorbeeld wordt vervolgens BLOB's uit de Broncontainer naar de doelcontainer met de cmdlet [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) . In het voorbeeld wordt ervan uitgegaan dat de bron- en doeladressen opslag en containers al bestaan.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Houd er rekening mee dat in dit voorbeeld een kopie van de asynchrone wordt. U kunt de status van elke kopie controleren door de cmdlet [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) wordt uitgevoerd.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>BLOB's vanuit een secundaire locatie kopiëren
U kunt BLOB's kopiëren van de secundaire locatie van een RA-GRS-ingeschakelde account.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Het verwijderen van een blob
De eerste keer een blob-referentie wilt verwijderen een blob, en roept de cmdlet verwijderen AzureStorageBlob erop. In het volgende voorbeeld worden alle BLOB's in een bepaalde container verwijderd. In het voorbeeld eerst worden variabelen ingesteld voor een account voor opslag en maakt vervolgens een opslag-context. In het voorbeeld wordt vervolgens haalt een blob-verwijzing met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) en de cmdlet [Verwijderen AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) om BLOB's verwijderen uit een container in Azure opslag wordt uitgevoerd.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Het beheren van momentopnamen Azure blob
Azure kunt u een momentopname maken van een blob. Een momentopname is een alleen-lezen versie van een blob die op een punt in de tijd wordt genomen. Wanneer u een momentopname hebt gemaakt, kan deze worden gelezen, gekopieerd of verwijderd, maar niet gewijzigd. Snapshots bieden een manier om een back-up een blob zoals deze wordt weergegeven op een moment. Zie [een momentopname van een Blob maken](http://msdn.microsoft.com/library/azure/hh488361.aspx)voor meer informatie.

### <a name="how-to-create-a-blob-snapshot"></a>Het maken van een momentopname van een blob
De eerste keer een blob-referentie wilt maken van een snaphot van een blob, en roept u vervolgens de `ICloudBlob.CreateSnapshot` methode erop. In het volgende voorbeeld eerst worden variabelen ingesteld voor een account voor opslag en maakt vervolgens een opslag-context. In het voorbeeld wordt vervolgens haalt een blob-verwijzing met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) en voert de methode [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) als een momentopname wilt maken.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Hoe u momentopnamen van een blob
U kunt zoveel momentopnamen als u voor een blob wilt. U kunt de momentopnamen die is gekoppeld aan de blob voor het bijhouden van uw huidige momentopnamen weergeven. In het volgende voorbeeld wordt een vooraf gedefinieerde blob en roept de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) om de momentopnamen van die blob.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Het kopiëren van een momentopname van een blob
U kunt een momentopname van een blob te herstellen van de momentopname kopiëren. Zie [een momentopname van een Blob maken](http://msdn.microsoft.com/library/azure/hh488361.aspx)voor uitgebreide informatie en beperkingen. In het volgende voorbeeld eerst worden variabelen ingesteld voor een account voor opslag en maakt vervolgens een opslag-context. In het voorbeeld wordt vervolgens de naam container en blob variabelen. In het voorbeeld verwijst blob met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) opgehaald en wordt de methode [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) om een momentopname te maken. In het voorbeeld wordt vervolgens de cmdlet [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) om te kopiëren van de momentopname van een het ICloudBlob-object gebruiken voor de bron-blob blob uitgevoerd. Zorg ervoor dat de variabelen op basis van uw configuratie voordat u het voorbeeld uitvoert. Opmerking in het volgende voorbeeld wordt aangenomen dat de bron en bestemming, containers en de blob bron al aanwezig.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

U hebt geleerd hoe u blob-momentopnamen met Azure PowerShell en Azure BLOB's beheren, gaat u naar de volgende sectie voor meer informatie over het beheren van tabellen, wachtrijen en bestanden.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Azure tabellen en tabel entiteiten beheren
Azure tabel storage-service is een gegevensarchief NoSQL, die u gebruiken kunt voor het opslaan en opvragen van grote sets van gestructureerde en niet-relationele gegevens. De belangrijkste onderdelen van de service zijn tabellen, entiteiten en eigenschappen. Een tabel is een verzameling van entiteiten. Een entiteit is een set eigenschappen. Elke entiteit kan maximaal 252 eigenschappen die alle naam / waarde-paren hebben. In dit gedeelte wordt ervan uitgegaan dat u al bekend met de begrippen Azure tabel Storage-Service bent. Zie [de tabel Service Data Model begrijpen](http://msdn.microsoft.com/library/azure/dd179338.aspx) en [aan de slag met Azure tabel opslag met behulp van .NET](storage-dotnet-how-to-use-tables.md)voor meer informatie.

In de volgende subsecties leert u Azure tabel met Azure PowerShell storage-service beheren. De scenario's die zijn **maken**, **verwijderen**en **ophalen van** **tabellen**, alsmede **toe te voegen**, **bevragen**en **entiteiten tabel verwijderen**.

### <a name="how-to-create-a-table"></a>Een tabel maken
Elke tabel moet zich bevinden in een opslagruimte Azure-account. In het volgende voorbeeld wordt gedemonstreerd hoe u een tabel maakt in Azure opslag. Het voorbeeld maakt eerst een verbinding met Azure opslag met behulp van de context van de account opslag, inclusief de naam van de opslag en de toegangstoets. Vervolgens wordt de cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) voor het maken van een tabel in Azure opslag.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Hoe u een tabel opgehaald
U kunt opvragen en ophalen van een of meer tabellen in een opslag-account. In het volgende voorbeeld laat zien hoe een bepaalde tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) ophalen.

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Als u de cmdlet Get-AzureStorageTable zonder parameters, wordt alle tabellen van de opslag voor een account voor opslag.

### <a name="how-to-delete-a-table"></a>Een tabel verwijderen
U kunt een tabel uit een account voor opslag met behulp van de cmdlet [Verwijderen AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) verwijderen.  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Tabel entiteiten beheren
Azure PowerShell biedt momenteel geen cmdlets tabel entiteiten als direct wilt beheren. Om bewerkingen uitvoeren op entiteiten van de tabel, kunt u de klassen die in de [Clientbibliotheek van Azure opslag voor .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Tabel entiteiten toevoegen
Als een entiteit toevoegen aan een tabel, moet u eerst een object waarin u de entiteitseigenschappen voor de maken. Een entiteit kan maximaal 255 eigenschappen, waaronder Systeemeigenschappen 3 hebben: **PartitionKey**, **RowKey**en **tijdstempel**. U bent verantwoordelijk voor het invoegen en bijwerken van de waarden van **PartitionKey** en **RowKey**. De server beheert de waarde van het **tijdstempel**, die kan niet worden gewijzigd. Samen de **PartitionKey** en **RowKey** worden geïdentificeerd via elke entiteit binnen een tabel.

-   **PartitionKey**: Hiermee bepaalt u de partitie die de entiteit wordt opgeslagen in.
-   **RowKey**: een unieke aanduiding voor de entiteit in de partitie.

U mag maximaal 252 aangepaste eigenschappen definiëren voor een entiteit. Zie [Wat zijn de gegevens van tabel servicemodel](http://msdn.microsoft.com/library/azure/dd179338.aspx)voor meer informatie.

In het volgende voorbeeld laat zien hoe entiteiten toevoegen aan een tabel. Het voorbeeld laat zien hoe de werknemerstabel ophalen en verschillende entiteiten toevoegen aan het. Het maakt eerst een verbinding met Azure opslag met behulp van de context van de account opslag, inclusief de naam van de opslag en de toegangstoets. Vervolgens wordt de tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) opgehaald. Als de tabel niet bestaat nog, wordt de cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) wordt gebruikt voor het maken van een tabel in Azure opslag. In het voorbeeld wordt vervolgens een aangepaste functie toevoegen-entiteit entiteiten toevoegen aan de tabel door te geven van de partitie van de entiteit en de rijsleutel. De functie toevoegen entiteit roept de cmdlet [New-Object](http://technet.microsoft.com/library/hh849885.aspx) op de [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) -klasse een entiteitsobject te maken. Later, roept het voorbeeld de methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) op deze entiteitsobject toe te voegen aan een tabel.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Tabel entiteiten doorzoeken
Als u wilt een tabel een query uitvoert, gebruikt u de klasse [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) . In het volgende voorbeeld wordt ervan uitgegaan dat u al het gegeven in hoe u het script hebt uitgevoerd om toe te voegen diensten sectie van deze handleiding. Het voorbeeld maakt eerst een verbinding met Azure opslag met behulp van de context van opslag, inclusief de naam van de opslag en de toegangstoets. Vervolgens wordt geprobeerd de eerder gemaakte "Werknemers" tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) ophalen. De cmdlet [New-Object](http://technet.microsoft.com/library/hh849885.aspx) aanroepen voor de klasse Microsoft.WindowsAzure.Storage.Table.TableQuery maakt een nieuwe query. In het voorbeeld wordt gezocht naar de entiteiten die een 'ID' kolom waarvan de waarde 1 als omschreven in het filter van een tekenreeks is. Zie [tabellen opvragen en entiteiten](http://msdn.microsoft.com/library/azure/dd894031.aspx)voor gedetailleerde informatie. Wanneer u deze query uitvoert, wordt alle entiteiten die aan de filtercriteria voldoen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Tabel entiteiten verwijderen
U kunt een entiteit met de sleutels die partitie en rij verwijderen. In het volgende voorbeeld wordt ervan uitgegaan dat u al het gegeven in hoe u het script hebt uitgevoerd om toe te voegen diensten sectie van deze handleiding. Het voorbeeld maakt eerst een verbinding met Azure opslag met behulp van de context van opslag, inclusief de naam van de opslag en de toegangstoets. Vervolgens wordt geprobeerd de eerder gemaakte "Werknemers" tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) ophalen. Als de tabel aanwezig is, roept het voorbeeld de methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) om op te halen van een entiteit op basis van de waarden van partitie en rij. Vervolgens geeft de entiteit aan de methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) om te verwijderen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Azure wachtrijen en berichten beheren
Azure Queue storage is een service voor het opslaan van een groot aantal berichten dat kunnen worden geopend vanuit overal in de wereld via geverifieerde oproepen met behulp van HTTP of HTTPS. In dit gedeelte wordt ervan uitgegaan dat u al bekend met de begrippen Azure Queue Storage-Service bent. Zie voor gedetailleerde informatie [aan de slag met Azure wachtrij opslag met behulp van .NET](storage-dotnet-how-to-use-queues.md).

In deze sectie wordt beschreven hoe u Azure wachtrij met Azure PowerShell storage-service beheren. De scenario's die gedekt zijn **Invoegen** en **verwijderen van** berichten, en **maken**, **verwijderen**en **wachtrijen ophalen**.

### <a name="how-to-create-a-queue"></a>Een wachtrij maken
In het volgende voorbeeld wordt eerst een verbinding naar Azure opslag met behulp van de context van de account opslag, inclusief de naam van de opslag en de toegangstoets. Vervolgens roept deze cmdlet [New-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) om een wachtrij met de naam 'wachtrijnaam' te maken.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Zie voor informatie over naamgevingsconventies voor Azure Queue-Service, [naamgeving wachtrijen en metagegevens](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Hoe kunt u een wachtrij worden opgehaald
U kunt opvragen en ophalen van een bepaalde wachtrij of een lijst met alle wachtrijen op een rekening voor opslag. In het volgende voorbeeld laat zien hoe een opgegeven wachtrij met de cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) ophalen.

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Als u de cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) zonder parameters aanroept, wordt een lijst met alle wachtrijen.

### <a name="how-to-delete-a-queue"></a>Een wachtrij verwijderen
Voor het verwijderen van een wachtrij en alle berichten die dit, roept de cmdlet verwijderen AzureStorageQueue. In het volgende voorbeeld ziet u hoe een opgegeven wachtrij met de cmdlet verwijderen AzureStorageQueue verwijderen.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Het invoegen van een bericht in een wachtrij
U een bericht in een bestaande wachtrij invoegt, maakt u eerst een nieuwe instantie van de klasse [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Vervolgens wordt de methode [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) aanroepen. Een CloudQueueMessage kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes.

In het volgende voorbeeld ziet u het bericht naar een wachtrij toevoegen. Het voorbeeld maakt eerst een verbinding met Azure opslag met behulp van de context van de account opslag, inclusief de naam van de opslag en de toegangstoets. Vervolgens wordt de opgegeven wachtrij met de cmdlet [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) opgehaald. Als de wachtrij bestaat, wordt de cmdlet [New-Object](http://technet.microsoft.com/library/hh849885.aspx) wordt gebruikt voor het maken van een exemplaar van de klasse [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Later, roept het voorbeeld de methode [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) voor dit berichtobject toe te voegen aan een wachtrij. Dit is de code die in een wachtrij opgehaald en wordt het bericht 'MessageInfo':

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Hoe kunt u het volgende bericht uit de wachtrij
Uw code wachtrijen uit een bericht in een wachtrij in twee stappen. Als u de methode [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) aangeroepen, krijgt u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd uit **GetMessage** wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. Als u klaar bent met het bericht uit de wachtrij te verwijderen, moet u de methode [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) aanroepen. Dit proces van het verwijderen van een bericht bent u verzekerd dat als de code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. Nadat het bericht is verwerkt, roept de code **DeleteMessage** rechts.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Het Azure bestandsshares en bestanden beheren
Azure bestandsopslag biedt gedeelde opslag voor toepassingen die gebruikmaken van de standaard SMB-protocol. Microsoft Azure virtuele machines en cloud services gegevens in een bestand kunnen delen via via aandelen gekoppelde onderdelen en toepassingen in gebouwen toegang tot de bestandsgegevens via de API voor File-opslag of Azure PowerShell in een.

Zie voor meer informatie over de opslag van bestanden in Azure, [aan de slag met opslag op Windows Azure-bestand](storage-dotnet-how-to-use-files.md) en [Bestand Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Het instellen en opvragen van opslag analytics
[Azure opslag Analytics](storage-analytics.md) kunt u verzamelen metrics voor uw opslag Azure accounts en gegevens over aanvragen die worden verzonden naar uw account opslag. Maatstaven voor opslag kunt u de status van een rekening voor opslag en opslag registreren om het opsporen en oplossen van problemen met uw account opslag controleren.
Opslag metrics is standaard niet ingeschakeld voor uw opslagservices. U kunt met behulp van de Windows PowerShell of Azure Portal of via programmacode met behulp van de client-bibliotheek van opslag-controle inschakelen. Opslag gebeurt aan de serverzijde en kunt u details voor zowel geslaagde als mislukte aanvragen in uw account voor de opslag van de records. Deze logboeken kunt u de details van lezen, schrijven en verwijderen van bewerkingen op tabellen, wachtrijen en BLOB's, alsmede de redenen voor mislukte aanvragen.

Zie informatie over het inschakelen en weergeven van opslag metrische gegevens met PowerShell, [het inschakelen van opslag Metrics met PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Zie informatie over het inschakelen en de opslag registreren gegevens met PowerShell, [opslag registreren met PowerShell inschakelen](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) en [Zoeken naar de logboekgegevens vastleggen van opslag](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Zie voor gedetailleerde informatie over het gebruik van opslag statistieken en opslag registreren en problemen met opslag, [controle, diagnose, en problemen met Microsoft Azure opslag](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Het beheren van gedeelde toegang handtekening (SAS) en -beleid opgeslagen
Gedeelde toegang handtekeningen zijn een belangrijk onderdeel van het beveiligingsmodel voor alle toepassingen met Azure opslag. Ze zijn nuttig voor beperkte machtigingen aan uw account voor de opslag te verlenen aan clients die niet de sleutel rekening moeten hebben. Standaard kan alleen de eigenaar van de opslag-account toegang tot BLOB's, tabellen en wachtrijen in die account. Als uw service of toepassing moet deze bronnen beschikbaar maken voor andere clients zonder het delen van uw access-sleutel, hebt u drie opties:

- Een container machtigingen instellen op het toestaan van anonieme leestoegang tot de container en de BLOB's. Dit is niet toegestaan voor tabellen of wachtrijen.
- Gedeelde toegang dat subsidies toegangsrechten containers, BLOB's, wachtrijen en tabellen voor een bepaald tijdsinterval beperkte handtekening gebruiken.
- Met een opgeslagen-beleid kunt u een extra niveau van controle over gedeelde toegang handtekeningen voor een container of het aantal BLOB's, een wachtrij of een tabel. De opgeslagen-beleid kunt u de begintijd, verlooptijd of machtigingen voor een handtekening te wijzigen of om het intrekken nadat deze is uitgegeven.

Een handtekening voor gedeelde toegang zijn in een van de twee formulieren:

- **Ad-hoc SAS**: wanneer u een ad-hoc SAS, de begintijd, de verlooptijd, maken en machtigingen voor de SA's worden opgegeven in de URI SAS. Dit soort Beveiligingskoppelingen kan worden gemaakt in een container, blob, tabel of wachtrij en het is niet revokable.
- **SAS-opgeslagen beleid**: een opgeslagen-beleid is gedefinieerd op een resource container een blob-container, een tabel of een wachtrij - en kunt u beperkingen voor een of meer gedeelde toegang handtekeningen te beheren. Wanneer u een SAS aan een opgeslagen-beleid koppelt, neemt de SA's beperkingen - de begintijd, verlooptijd en machtigingen - definitie voor het opgeslagen-beleid. Dit type van SAS is revokable.

Zie [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md) en [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)voor meer informatie.

In de volgende gedeelten leert u hoe u een gedeelde access handtekening access token en opgeslagen beleid voor Azure tabellen maken. Azure PowerShell biedt vergelijkbare cmdlets voor containers, BLOB's en ook wachtrijen. Downloaden voor het uitvoeren van de scripts in deze sectie, de [Azure PowerShell versie 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) of hoger.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Het maken van een handtekening voor gedeelde toegang op basis van beleid token
Gebruik de cmdlet New-AzureStorageTableStoredAccessPolicy voor het maken van een opgeslagen-beleid. Vervolgens roept de cmdlet [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) om een nieuwe handtekening-token van gedeelde toegang op basis van beleid voor een tabel Azure opslag.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Het maken van een ad-hoc gedeelde toegang handtekening-token (niet revokable)
Gebruik de cmdlet [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) een nieuwe ad-hoc (niet revokable) gedeeld Access token handtekening voor een tabel Azure opslag maken:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Het maken van een opgeslagen-beleid
Gebruik de cmdlet New-AzureStorageTableStoredAccessPolicy voor het maken van een nieuwe opgeslagen-beleid voor een tabel Azure opslag:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Het bijwerken van een opgeslagen-beleid
Gebruik de cmdlet Set-AzureStorageTableStoredAccessPolicy voor het bijwerken van een bestaande opgeslagen-beleid voor een tabel Azure opslag:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Een opgeslagen-beleid verwijderen
Gebruik de cmdlet verwijderen AzureStorageTableStoredAccessPolicy een opgeslagen-beleid op de tabel Azure opslag verwijderen:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Het gebruik van Azure opslag voor de Amerikaanse overheid en Azure China
Een Azure-omgeving is een onafhankelijke implementatie van Microsoft Azure, zoals [Azure overheid voor de Amerikaanse overheid](https://azure.microsoft.com/features/gov/)en [AzureCloud voor global Azure](https://portal.azure.com), [AzureChinaCloud voor Azure beheerd door 21Vianet in China](http://www.windowsazure.cn/). U kunt nieuwe Azure omgevingen voor de Amerikaanse regering en Azure China implementeren.

Azure opslag met AzureChinaCloud, hebt u nodig voor het maken van de context van een opslag die is gekoppeld aan de AzureChinaCloud. Volg deze stappen om u aan de slag:

1.  Voer de cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) overzicht van de beschikbare Azure omgevingen:

    `Get-AzureEnvironment`

2.  Een Azure China-account toevoegen aan Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Een context opslag voor een AzureChinaCloud account maken:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Azure opslag met [Azure overheid](https://azure.microsoft.com/features/gov/)gebruikt, moet u definieert een nieuwe omgeving en maakt u een nieuwe context voor opslag met deze omgeving:

1.  Voer de cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) overzicht van de beschikbare Azure omgevingen:

    `Get-AzureEnvironment`

2.  Een Azure Amerikaanse regering-account toevoegen aan Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Een context opslag voor een AzureUSGovernment account maken:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Zie voor meer informatie:

- [Handleiding voor ontwikkelaars van Microsoft Azure regering](../azure-government-developer-guide.md).
- [Overzicht van de verschillen bij het maken van een toepassing op de China Service](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u geleerd hoe Azure opslag met Azure PowerShell beheren. Hier zijn enkele verwante artikelen en bronnen voor meer informatie over deze.

- [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
- [Opslag van Azure PowerShell-Cmdlets](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Windows PowerShell-verwijzing](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
