<properties
    pageTitle="Aan de slag met Azure tabel opslag met behulp van .NET | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud opslag Azure-tabel, een gegevensarchief NoSQL."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Aan de slag met Azure tabel opslag met behulp van .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Azure tabel storage is een service waarmee gestructureerde NoSQL gegevens worden opgeslagen in de cloud. Tabelopslag is een sleutel/kenmerk Archief met een schemaless ontwerp. Omdat tabelopslag schemaless is, is het eenvoudig om uw gegevens aan te passen als de behoeften van uw toepassing evolve. Toegang tot gegevens is snel en rendabel voor allerlei soorten toepassingen. Tabelopslag is meestal in de kosten aanzienlijk lager is dan traditionele SQL voor dergelijke volumes van gegevens.

U kunt opslag tabel opslaan flexibele datasets, zoals gegevens van de gebruiker voor webtoepassingen, adresboeken, gegevens van een apparaat en een ander type metagegevens die de service nodig heeft. U kunt een willekeurig aantal entiteiten opslaan in een tabel en een opslag-account kan een willekeurig aantal tabellen tot de capaciteit van de opslag account bevatten.

### <a name="about-this-tutorial"></a>Over deze handleiding

Deze zelfstudie laat zien hoe .NET code schrijven voor enkele algemene scenario's met Azure tabel opslag, met inbegrip van maken en verwijderen van een tabel invoegen, bijwerken, verwijderen en opvragen van gegevens in een tabel.

**Geschatte tijd:** 45 minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure opslag Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Een [Azure opslag account](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Meer voorbeelden

Zie voor meer voorbeelden met behulp van tabelopslag, [Aan de slag met Azure tabelopslag in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). De voorbeeldtoepassing te downloaden en uit te voeren, of de code op GitHub te bladeren.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

Voeg de volgende `using` instructies aan de bovenkant van de `program.cs` bestand:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>De tabel service-client maken

De klasse **CloudTableClient** kunt u tabellen en opgeslagen in de tabelopslag entiteiten ophalen. Hier is één manier om de client te maken:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

U bent nu gereed voor de code schrijven die de gegevens leest uit en schrijft gegevens naar de opslag van de tabel.

## <a name="create-a-table"></a>Een tabel maken

In dit voorbeeld ziet u hoe een tabel te maken als deze nog niet bestaat:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Een entiteit aan een tabel toevoegen

Entiteiten toewijzen aan C\# objecten met behulp van een aangepaste klasse die is afgeleid van **TableEntity**. Als een entiteit toevoegen aan een tabel, een klasse die de eigenschappen van de entiteit definieert te maken. De volgende code definieert een klasse entiteit die de voornaam van de klant als de rijsleutel- en achternaam als de partitiesleutel gebruikt. Samen identificeren de partitie van een entiteit en rijsleutel de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel sneller dan die met andere partitiesleutels kunnen worden opgezocht, maar met verschillende partitiesleutels kan grotere schaalbaarheid van parallelle bewerkingen.  De eigenschap moet voor elke eigenschap die moet worden opgeslagen in de tabel service, een openbare eigenschap van een ondersteund type dat toegang biedt tot beide `get` en `set`.
Ook uw entiteit type *moet* een constructor zonder worden blootgesteld.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Bewerkingen die betrekking hebben op diensten worden uitgevoerd via het **CloudTable** -object dat u eerder hebt gemaakt in de sectie 'Een tabel maken'. De bewerking wordt uitgevoerd wordt door een **TableOperation** -object vertegenwoordigd.  In het volgende voorbeeld ziet u het maken van het **CloudTable** -object en vervolgens een **CustomerEntity** -object.  Als voorbereiding op de bewerking, een **TableOperation** -object gemaakt om de dienst van de klant in de tabel invoegen.  Ten slotte wordt de bewerking uitgevoerd door het aanroepen van **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Invoegen van een batch van entiteiten

U kunt een reeks diensten invoegen in een tabel in een schrijfbewerking. Sommige andere notities op batchbewerkingen:

-  U kunt updates uitvoeren wordt verwijderd en ingevoegd in dezelfde bewerking van één partij.
-  Een bewerking van één partij kan maximaal 100 entiteiten bevatten.
-  Alle entiteiten in een enkele batch-bewerking moeten dezelfde partitiesleutel hebben.
-  Het is mogelijk om een query als een batchbewerking uitvoeren, moet de enige bewerking in de batch.

<!-- -->
In het volgende voorbeeld wordt gemaakt van twee entiteitsobjecten en elk **TableBatchOperation** wordt toegevoegd met behulp van de methode **Insert** . Vervolgens **CloudTable.Execute** aangeroepen om de bewerking niet uitvoeren.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen

Gebruik een **TableQuery** -object om een querytabel voor alle entiteiten in een partitie.
In het volgende voorbeeld bevat een filter voor entiteiten waar 'Smith' de partitiesleutel is. In dit voorbeeld wordt de velden van elke entiteit in de resultaten van de query naar de console.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Een bereik van de entiteiten in een partitie ophalen

Als u niet wilt opvragen van de entiteiten in een partitie, kunt u een bereik door de combinatie van de belangrijkste filter partitie met een rij belangrijke filter. In het volgende voorbeeld worden twee filters gebruikt om alle entiteiten in partitie Smith waar de rij (voornaam) eerder dan "E" in het alfabet begint met een letter en vervolgens de resultaten van de query worden afgedrukt.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Ophalen van één enkele entiteit

U kunt een query voor het ophalen van een enkele, specifieke entiteit schrijven. De volgende code wordt de **TableOperation** 'Ben Smith' van de klant opgeven.
Deze methode geeft als resultaat één entiteit in plaats van een verzameling en de geretourneerde waarde in een **TableResult.Result** een **CustomerEntity** -object.
Partitie-en opgeven in een query is de snelste manier om een enkele entiteit worden opgehaald uit de tabel service.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Vervangen van een entiteit

Een entiteit bijwerken, ophalen uit de tabel service, het entiteitsobject wijzigen en vervolgens de wijzigingen opgeslagen in de tabel service. De volgende code wijzigt een bestaande klant telefoonnummer. In plaats van **Invoegen**, wordt deze code **vervangen**. Hierdoor wordt de entiteit moet volledig worden vervangen op de server, tenzij de entiteit op de server is gewijzigd sinds deze is opgehaald, in welk geval mislukt de bewerking.  Deze fout wordt voorkomen dat de toepassing van het per ongeluk overschrijven van een wijziging aangebracht tussen het ophalen en bijwerken door een ander onderdeel van uw toepassing.  De juiste afhandeling van deze fout is de entiteit opnieuw ophalen, breng de gewenste wijzigingen (als dat nog steeds geldig is) en voert u een andere bewerking **vervangen** .  De volgende sectie wordt beschreven hoe u dit gedrag wijzigen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Invoegen of-vervangen een entiteit

Als de entiteit is gewijzigd sinds deze is opgehaald uit de server mislukken **vervangen** bewerkingen.  Bovendien moet u de entiteit ophalen van de server eerst in om de bewerking **vervangen** lukken.
Soms echter weet u niet of de entiteit op de server bestaat en de huidige waarden in deze niet relevant zijn. De update moet overschrijven alle.  Om dit te bereiken, gebruikt u een **InsertOrReplace** -bewerking.  Deze bewerking wordt de entiteit ingevoegd als deze niet bestaat, of vervangen als dit het geval is, ongeacht wanneer de laatste update is gemaakt.  In het volgende voorbeeld, de dienst van de klant voor Ben Smith nog opgehaald, maar deze wordt vervolgens opgeslagen op de server via **InsertOrReplace**.  Wijzigingen in de entiteit tussen de bewerkingen ophalen en bijwerken worden, overschreven.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Een subset van de entiteitseigenschappen opvragen

Een tabelmaakquery kunt u een paar eigenschappen ophalen uit een entiteit in plaats van alle entiteitseigenschappen. Deze techniek, projectie, genaamd minder bandbreedte en kan query's sneller, met name voor grote entiteiten. De query in de volgende code retourneert alleen de e-mailadressen van de entiteiten in de tabel. Dit gebeurt met behulp van een query van de **DynamicTableEntity** en ook **EntityResolver**. U kunt meer informatie over de projectie op de [invoering van Upsert en de post-URL van de projectie van de Query][]. Houd er rekening mee dat projectie wordt niet ondersteund op de lokale opslag-emulator, zodat deze code wordt alleen uitgevoerd als u een account op de service van de tabel.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Een entiteit verwijderen

U kunt eenvoudig een entiteit verwijderen nadat u deze hebt opgehaald met behulp van hetzelfde patroon voor het bijwerken van een entiteit worden weergegeven.  De volgende code haalt en de entiteit van een klant wordt verwijderd.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Een tabel verwijderen

In het volgende voorbeeld wordt een tabel ten slotte vanuit een account opslag verwijderd. Een tabel die is verwijderd niet opnieuw te worden gemaakt voor een periode na de verwijdering beschikbaar.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asynchroon ophalen van entiteiten in pagina 's

Als u een groot aantal entiteiten leest en proces gewenste/display entiteiten zoals ze worden opgehaald dan wanneer ze alle te retourneren, kunt u entiteiten met een gesegmenteerde query ophalen. In dit voorbeeld ziet u hoe om resultaten te retourneren in pagina's met het wachten op Async patroon zodat worden uitgevoerd terwijl u wacht voor een grote verzameling resultaten om terug te keren niet wordt geblokkeerd. Zie voor meer informatie over het gebruik van de asynchrone Await patroon in .NET, [asynchrone programmering met asynchrone en Await (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de opslag van de tabel, gaat u als volgt deze koppelingen naar meer informatie over complexere opslagtaken:

- Zie meer voorbeelden van tabel opslag in [Aan de slag met Azure tabelopslag in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- De tabel service-documentatie voor volledige informatie over beschikbare API's weergeven:
    - [Client-bibliotheek van opslag voor referentie .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API: naslag](http://msdn.microsoft.com/library/azure/dd179355)
- Meer informatie over het vereenvoudigen van de code die u schrijft om te werken met Azure opslag met behulp van de [SDK van Azure WebJobs](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- Bekijk meer functie gidsen voor meer informatie over aanvullende opties voor het opslaan van gegevens in Azure.
    - [Aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md) ongestructureerde gegevens op te slaan.
    - [Verbinding maken met een SQL-Database met behulp van .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) voor het opslaan van relationele gegevens.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Kennismaking Upsert en projectie Query blogbericht]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
