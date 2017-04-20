<properties
    pageTitle="Aan de slag met tabelopslag en Visual Studio verbonden services (ASP.NET) | Microsoft Azure"
    description="Hoe aan de slag met Azure tabel opslag in een ASP.NET-project in Visual Studio nadat services verbinding maken met een opslag-account met behulp van Visual Studio worden verbonden"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-table-storage-and-visual-studio-connected-services-aspnet"></a>Aan de slag met tabelopslag en Visual Studio verbonden services (ASP.NET)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe aan de slag met Azure tabel opslag in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een ASP.NET-project waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** . In dit artikel wordt beschreven hoe u veelvoorkomende taken uitvoeren in Azure tabellen, zoals het maken en verwijderen van een tabel, en werken met tabel-entiteiten. De monsters zijn geschreven in C\# code en de [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)gebruiken. Voor meer algemene informatie over het gebruik van Azure tabel opslag, kunt u [aan de slag met Azure tabel opslag met behulp van .NET](storage-dotnet-how-to-use-tables.md).

Azure tabelopslag kunt u grote hoeveelheden gegevens gestructureerd worden opgeslagen. De service is een gegevensarchief NoSQL die geverifieerde oproepen van binnen en buiten de Azure cloud accepteert. Azure tabellen zijn ideaal voor het opslaan van gestructureerde en niet-relationele gegevens.


## <a name="access-tables-in-code"></a>Access-tabellen in code

1. Zorg ervoor dat de naamruimtedeclaraties aan de bovenkant van de C#-bestand omvatten deze instructies **gebruiken** .

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag. De volgende code gebruiken om de de verbindingsreeks voor opslag en opslag-accountgegevens van de configuratie van Azure service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Opmerking** : alle van de bovenstaande code voor de code gebruiken in de volgende voorbeelden.

3. Een **CloudTableClient** -object te verwijzen naar de tabelobjecten in uw account voor de opslag krijgen.  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Krijg een **CloudTable** reference-object te verwijzen naar een bepaalde tabel en entiteiten.

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Een tabel maken in code

De Azure als tabel wilt maken, voegt u toe een aanroep van **CreateIfNotExistsAsync()** naar de vorige code.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Een entiteit aan een tabel toevoegen

U maakt een klasse die de eigenschappen van de entiteit definieert een entiteit toevoegen aan een tabel. De volgende code definieert een entiteit klasse met de naam **CustomerEntity** die gebruikmaakt van de voornaam van de klant als de rijsleutel- en achternaam als de partitiesleutel.

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

Tabel-operaties met betrekking tot diensten zijn gedaan met behulp van de **CloudTable** object dat u eerder gemaakt "Access-tabellen in code." Het **TableOperation** -object vertegenwoordigt de bewerking moet worden uitgevoerd. In het volgende voorbeeld ziet u hoe een **CloudTable** -object en een **CustomerEntity** -object te maken. Als voorbereiding op de bewerking, een **TableOperation** gemaakt om de dienst van de klant in de tabel invoegen. Ten slotte wordt de bewerking uitgevoerd door het aanroepen van CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Invoegen van een batch van entiteiten

U kunt meerdere entiteiten invoegen in een tabel in een enkele schrijfbewerking. In het volgende voorbeeld maakt twee entiteitsobjecten ("Jeff Smith" en "Ben Smith"), worden deze toegevoegd aan een **TableBatchOperation** -object met de methode Insert en vervolgens de bewerking gestart door het aanroepen van **CloudTable.ExecuteBatchAsync**.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen
Gebruik een **TableQuery** -object om een querytabel voor alle entiteiten in een partitie. In het volgende voorbeeld bevat een filter voor entiteiten waar 'Smith' de partitiesleutel is. In dit voorbeeld wordt de velden van elke entiteit in de resultaten van de query naar de console.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity>
        resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
        }
        } while (token != null);

        return View();


## <a name="get-a-single-entity"></a>Ophalen van één enkele entiteit
U kunt een query als u een enkele, specifieke entiteit schrijven. De volgende code wordt een **TableOperation** -object op te geven van een klant met de naam 'Ben Smith'. Deze methode retourneert één entiteit, in plaats van een verzameling en de geretourneerde waarde in een **TableResult.Result** is een **CustomerEntity** -object. Partitie-en opgeven in een query is de snelste manier om een enkele entiteit worden opgehaald uit de tabel service.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);
    
    // Print the phone number of the result.
    if (retrievedResult.Result != null)
        Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Een entiteit verwijderen
Nadat u het hebt gevonden, kunt u een entiteit verwijderen. De volgende code wordt gezocht naar een klantentiteit met de naam 'Ben Smith' en als deze wordt gevonden, wordt de snelkoppeling verwijderd.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
