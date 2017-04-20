<properties
    pageTitle="Anonieme leestoegang tot containers en BLOB's beheren | Microsoft Azure"
    description="Informatie over hoe u containers en BLOB's beschikbaar voor anonieme toegang en toegang via programmacode tot."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Anonieme leestoegang tot containers en BLOB's beheren

## <a name="overview"></a>Overzicht

Standaard alleen de eigenaar van de opslag-account toegang krijgen tot opslagbronnen in die account. Voor blobopslag, kunt u machtigingen voor anonieme leestoegang tot de container en de BLOB's, van de container instellen zodat u toegang tot deze bronnen verlenen kunt zonder het delen van de sleutel van uw account.

Anonieme toegang is het meest geschikt voor scenario's waar u bepaalde BLOB's altijd beschikbaar moeten zijn voor anonieme toegang voor lezen. Voor een nauwkeurigere besturing mogelijk is, kunt u een gedeelde access-handtekening waarmee u via een opgegeven tijdsinterval en beperkte Gemachtigdentoegang met verschillende machtigingen. Zie voor meer informatie over het maken van handtekeningen voor gedeelde toegang [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Anonieme gebruikersmachtigingen toewijzen aan containers en BLOB 's

Standaard kunnen een container en alle BLOB's binnen dit alleen worden benaderd door de eigenaar van de account van de opslag. Om anonieme gebruikers leesmachtigingen voor een container en de BLOB's, kunt u de machtigingen van de container voor openbare toegang instellen. Anonieme gebruikers kunnen lezen van BLOB's in een openbaar toegankelijke container zonder verificatie van de aanvraag.

Containers bevatten de volgende opties voor het beheren van container:

- **Volledige openbare toegang voor lezen:** Container- en blob-gegevens kunnen worden gelezen via anonieme aanvragen. Clients BLOB's in de container via anonieme aanvragen kunnen opsommen, maar kunnen niet inventariseren containers in de opslag-account.

- **Openbare toegang voor BLOB's alleen lezen:** BLOB-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de container is niet beschikbaar. Clients kunnen niet opsommen BLOB's in de container via anonieme aanvragen.

- **Geen enkele openbare leestoegang:** Container- en blob-gegevens kunnen worden gelezen door alleen de eigenaar van de account.

U kunt machtigingen voor de container op de volgende manieren instellen:

- Vanaf de [Portal Azure](https://portal.azure.com).
- Programmacode, met behulp van client-bibliotheek van de opslag of de REST API.
- Met PowerShell. Zie voor meer informatie over het instellen van machtigingen voor de container van Azure PowerShell, [Azure PowerShell gebruiken met Azure opslag](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Container machtigingen instellen vanaf de Portal Azure

Container machtigingen instellen vanaf de [Portal Azure](https://portal.azure.com), volg deze stappen:

1. Ga naar het dashboard voor uw opslag-account.
2. Selecteer de containernaam van de in de lijst. Te klikken op de naam beschrijft de BLOB's in de gekozen container.
3. **-Beleid** op de werkbalk selecteren.
4. Selecteer in het veld **type toegang** uw gewenste niveau van machtigingen zoals in de screenshot hieronder.

    ![Dialoogvenster Container metagegevens bewerken](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Machtigingen voor de container instellen via programmacode met behulp van .NET

Als machtigingen wilt instellen voor een container met de .NET client-bibliotheek, de bestaande machtigingen van de container eerst worden opgehaald door het aanroepen van de **GetPermissions** , methode. Stel vervolgens de eigenschap **PublicAccess** voor het **BlobContainerPermissions** -object dat wordt geretourneerd door de methode **GetPermissions** . Tenslotte roept u de methode **SetPermissions** met de bijgewerkte machtigingen.

Het volgende voorbeeld wordt de machtigingen van de container naar volledige openbare leestoegang. U kunt machtigingen instellen voor stellen openbare leestoegang voor BLOB's, de **PublicAccess** **BlobContainerPublicAccessType.Blob**. Als u wilt dat alle machtigingen voor anonieme gebruikers, moet u de eigenschap instelt op **BlobContainerPublicAccessType.Off**.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Anoniem toegang krijgen tot containers en BLOB 's

Een client die toegang heeft tot containers en blobs anoniem kunt constructors waarvoor geen referenties gebruiken. De volgende voorbeelden worden een paar verschillende manieren om te verwijzen naar van Blob-servicebronnen anoniem.

### <a name="create-an-anonymous-client-object"></a>Een anonieme clientobject maken

Doordat de Blob-service-eindpunt voor de account kunt u een nieuw object van de client service voor anonieme toegang. U moet echter ook de naam van een container in die account voor anonieme toegang kennen.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Een container anoniem verwijst naar

Als u de URL van een container die anoniem beschikbaar is, kunt u deze verwijst rechtstreeks naar de container.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Een blob anoniem verwijst naar

Als u de URL van een blob die beschikbaar is voor anonieme toegang hebt, kunt u verwijzen naar de blob rechtstreeks met die URL:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Functies die beschikbaar zijn voor anonieme gebruikers

De volgende tabel ziet u welke bewerkingen kunnen worden aangeroepen door anonieme gebruikers als de ACL van de container is ingesteld dat de toegang van het publiek.

| REST-bewerking                                         | Toestemming met volledige openbare leestoegang | Toestemming met openbare leestoegang voor alleen BLOB 's |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Lijst Containers                                        | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Maken van Container                                       | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Ophalen van de containereigenschappen                               | Alle                                     | Alleen de eigenaar                                        |
| Container metagegevens ophalen                                 | Alle                                     | Alleen de eigenaar                                        |
| Metagegevens van Container instellen                                 | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Ophalen van de Container ACL                                      | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| ACL-Container instellen                                      | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Container verwijderen                                       | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Lijst BLOB 's                                             | Alle                                     | Alleen de eigenaar                                        |
| Blob plaatsen                                               | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob ophalen                                               | Alle                                     | Alle                                               |
| Blob-eigenschappen ophalen                                    | Alle                                     | Alle                                               |
| Blob-eigenschappen instellen                                    | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob metagegevens ophalen                                      | Alle                                     | Alle                                               |
| Blob metagegevens instellen                                      | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blok plaatsen                                              | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blok-lijst (alleen voor toegewezen blokken)                 | Alle                                     | Alle                                               |
| Ophalen van de lijst met geblokkeerde websites (alleen niet-doorgevoerde blokken of alle blokken) | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Block lijst                                         | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob verwijderen                                            | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob kopiëren                                              | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Momentopname Blob                                          | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Lease Blob                                             | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Pagina plaatsen                                               | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Get paginabereiken                                        | Alle                                     | Alle                                                  |
| Blob toevoegen                                            | Alleen de eigenaar                              | Alleen de eigenaar                                                  |


## <a name="see-also"></a>Zie ook

- [Verificatie voor de van Azure opslagservices](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Gebruik van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md)
- [Toegang met een handtekening gedeelde toegang delegeren](https://msdn.microsoft.com/library/azure/ee395415.aspx)
