<properties
    pageTitle="Het gebruik van opslag van Java tabel | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud opslag Azure-tabel, een gegevensarchief NoSQL."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Het gebruik van tabelopslag van Java

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's met behulp van de tabel Azure storage-service. De monsters zijn geschreven in Java en de [Azure opslag SDK for Java][]gebruiken. De scenario's die zijn **maken**, **weergeven**en **verwijderen van** tabellen, evenals **Invoegen** **opvragen**, **wijzigen**en **verwijderen** entiteiten in een tabel. Zie de sectie [volgende stappen](#Next-Steps) voor meer informatie over tabellen.

Opmerking: Er is een SDK beschikbaar voor ontwikkelaars die werken met Azure opslag op Android-apparaten. Zie de [SDK van Azure opslag voor Android][]voor meer informatie.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken

In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een Java-toepassing lokaal of in de code die wordt uitgevoerd binnen een webpagina rol of functie van de werknemer in Azure.

Hiertoe moet u de Java Development Kit (JDK) installeren en een account Azure opslag maken van je abonnement op Azure. Nadat u dit hebt gedaan, moet u controleren of uw ontwikkelingssysteem voldoet aan de minimumvereisten en de afhankelijkheden die zijn opgenomen in de bibliotheek [Azure opslag SDK voor Java][] op GitHub. Als uw systeem voldoet aan deze eisen wordt voldaan, kunt u de instructies voor downloaden en installeren van de bibliotheken Azure opslag voor Java op uw systeem uit die bibliotheek. Als u deze taken hebt voltooid, kunt u zijn kunnen maken van een Java-toepassing die wordt gebruikt in de voorbeelden in dit artikel.

## <a name="configure-your-application-to-access-table-storage"></a>Uw toepassing configureren voor toegang tot tabelopslag

De volgende importinstructies aan de bovenkant van het Java-bestand waar u Microsoft Azure opslag API's gebruiken om toegang te krijgen tot tabellen toevoegen:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag Setup

Een opslag Azure-client gebruikt een verbindingsreeks opslag eindpunten en referenties voor het openen van gegevens wilt opslaan. Wanneer een clienttoepassing wordt uitgevoerd, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw account voor de opslag en de van primaire toegangssleutel voor de opslag in [Azure Portal](https://portal.azure.com) voor de *accountnaam* en *AccountKey* -waarden weergegeven. In dit voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In een toepassing die wordt uitgevoerd binnen een rol in Microsoft Azure, wordt deze tekenreeks kan worden opgeslagen in het configuratiebestand service, *ServiceConfiguration.cscfg*, en is toegankelijk via een aanroep van de methode **RoleEnvironment.getConfigurationSettings** . Hier volgt een voorbeeld van de verbindingsreeks opvragen van een **instelling** -element met de naam *StorageConnectionString* in het configuratiebestand van de service:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om de verbindingsreeks voor opslag.

## <a name="how-to-create-a-table"></a>Procedure: een tabel maken

Een **CloudTableClient** -object kunt u de verwijzing naar objecten ophalen voor tabellen en entiteiten. De volgende code wordt een **CloudTableClient** -object gemaakt en gebruikt voor het maken van een nieuw **CloudTable** -object dat staat voor een tabel met de naam "mensen". (Opmerking: Er zijn meer manieren voor het maken van objecten **CloudStorageAccount** ; Zie **CloudStorageAccount** in de [Referentie voor Azure opslag Client SDK]voor meer informatie.)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Procedure: de tabellen

Als u een lijst met tabellen, roept u de methode **CloudTableClient.listTables()** om op te halen van een iterable lijst met namen van tabellen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Procedure: een entiteit aan een tabel toevoegen

Entiteiten toewijzen aan Java-objecten met behulp van een aangepaste klasse implementeren van **TableEntity**. Voor het gemak de klasse **TableServiceEntity** **TableEntity** wordt geïmplementeerd en maakt gebruik van reflectie eigenschappen toe te wijzen aan de methoden getter en setter met de naam van de eigenschappen. Als een entiteit toevoegen aan een tabel, maakt u eerst een klasse die de eigenschappen van de entiteit definieert. De volgende code definieert een klasse entiteit die de voornaam van de klant als de rijsleutel- en achternaam als de partitiesleutel gebruikt. Samen identificeren de partitie van een entiteit en rijsleutel de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel kunnen sneller dan die met andere partitiesleutels worden doorzocht.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Tabel-operaties met betrekking tot diensten vereisen een **TableOperation** -object. Dit object definieert de bewerking moet worden uitgevoerd op een entiteit die kan worden uitgevoerd met een **CloudTable** -object. De volgende code maakt een nieuw exemplaar van de klasse **CustomerEntity** met enkele klantgegevens kunnen worden opgeslagen. De code vervolgens roept **TableOperation.insertOrReplace** wilt maken van een **TableOperation** -object een entiteit in een tabel invoegen en de nieuwe **CustomerEntity** gekoppeld. Ten slotte roept de code de methode **execute** van het object **CloudTable** , waarin de tabel "gebruikers" en de nieuwe **TableOperation**, die vervolgens een verzoek naar de storage-service verzendt voor de nieuwe klantentiteit in de tabel 'gebruikers' invoegen of vervangen van de entiteit als deze al bestaat.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Procedure: een batch van entiteiten invoegen

U kunt een batch van entiteiten die aan de service tabel invoegen in een schrijfbewerking. De volgende code wordt een **TableBatchOperation** -object gemaakt en vervolgens voegt dat drie bewerkingen aan het invoegen. Elke invoegbewerking is door het maken van een nieuwe entiteitsobject, stelt u de waarden en vervolgens aanroepen van de methode **insert** voor het object **TableBatchOperation** op de entiteit koppelen aan een nieuwe invoegbewerking toegevoegd. Vervolgens roept de code **uitvoeren** op het **CloudTable** -object, waarbij u de tabel "gebruikers" en het **TableBatchOperation** -object dat de partij van de tabel naar de storage-service in een enkele aanvraag verzendt.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Sommige dingen om te weten op batch-bewerkingen:

- U kunt maximaal 100 invoegen uitvoeren, verwijderen, samenvoegen, vervangen, invoegen of samenvoegen, en invoegen of vervangbewerkingen in elke gewenste combinatie in één batch.
- Een batchbewerking kan een bewerking ophalen hebben als het is de enige bewerking in de batch.
- Alle entiteiten in een enkele batch-bewerking moeten dezelfde partitiesleutel hebben.
- Een batchbewerking is beperkt tot een nettolading van 4MB.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Procedure: alle entiteiten in een partitie ophalen

Als u wilt een querytabel voor entiteiten in een partitie kunt u een **TableQuery**. Bel **TableQuery.from** als u wilt een query maken voor een bepaalde tabel die het type van een opgegeven resultaat retourneert. De volgende code bevat een filter voor entiteiten waar 'Smith' de partitiesleutel is. **TableQuery.generateFilterCondition** is een hulpprogramma om filters voor query's te maken. Bel **waar** op de verwijzing die wordt geretourneerd door de methode **TableQuery.from** in het filter wilt toepassen op de query. Wanneer de query wordt uitgevoerd met een gesprek **voeren** op het **CloudTable** -object, wordt een **Iterator** met het opgegeven type **CustomerEntity** resultaat. U kunt de **Iterator** geretourneerd in een voor elke lus de resultaten in beslag neemt. Deze code wordt afgedrukt voor de velden van elke entiteit in de resultaten van de query naar de console.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Procedure: een bereik van de entiteiten in een partitie ophalen

Als u niet wilt opvragen van de entiteiten in een partitie, kunt u een bereik met behulp van vergelijkingsoperators in een filter. De volgende code een combinatie van twee filters als u alle entiteiten in de partitie 'Smith', waar de rijsleutel (voornaam) met een letter op 'E begint' in het alfabet. Vervolgens worden de resultaten van de query. Als u de entiteiten die zijn toegevoegd aan de tabel in de batch sectie van deze handleiding invoegen, worden slechts twee entiteiten geretourneerd deze keer (Ben en Denise Smith); Jeff Smith is niet opgenomen.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Procedure: één enkele entiteit ophalen

U kunt een query voor het ophalen van een enkele, specifieke entiteit schrijven. De volgende code roept **TableOperation.retrieve** met partitie-toets en de rij belangrijke parameters op te geven van de klant "Jeff Smith", in plaats van een **TableQuery** maken en gebruiken van filters met hetzelfde doen. Wanneer uitgevoerd, wordt de bewerking ophalen retourneert één entiteit, in plaats van een collectie. De methode **getResultAsType** wordt geworpen het resultaat met het type van het doel van de toewijzing, een **CustomerEntity** -object. Als dit type niet compatibel met het type dat is opgegeven voor de query is, wordt een uitzondering gegenereerd worden. Een null-waarde wordt geretourneerd als geen entiteit is een exacte partitie en rijsleutel overeen. Partitie-en opgeven in een query is de snelste manier om een enkele entiteit worden opgehaald uit de tabel service.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Procedure: een entiteit wijzigen

Als een entiteit wijzigt, ophalen uit de tabel service, wijzigingen aanbrengen in de entiteitsobject en de wijzigingen opgeslagen in de tabel service met een bewerking vervangen of samenvoegen. De volgende code wijzigt een bestaande klant telefoonnummer. In plaats van het aanroepen van **TableOperation.insert** zoals wij dat hebben gedaan als u wilt invoegen, roept deze code **TableOperation.replace**. De methode **CloudTable.execute** roept de service van de tabel en de entiteit wordt vervangen, tenzij u een andere toepassing gewijzigd in de tijd nadat u deze toepassing het opgevraagd. In dat geval een uitzondering is opgetreden en de entiteit moet worden opgehaald, gewijzigd en opnieuw worden opgeslagen. Dit patroon optimistische gelijktijdigheid opnieuw is gebruikelijk in een gedistribueerd systeem.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Procedure: een subset van de entiteitseigenschappen opvragen

Een query in een tabel kunt u een paar eigenschappen ophalen uit een entiteit. Deze techniek, projectie, genaamd minder bandbreedte en kan query's sneller, met name voor grote entiteiten. De query in de volgende code retourneert alleen de e-mailadressen van de entiteiten in de tabel de methode **select** . De resultaten worden geprojecteerd naar een verzameling van de **tekenreeks** met behulp van een **EntityResolver**, die het typeconversie op de entiteiten die zijn geretourneerd door de server. Voor meer informatie over de projectie in [tabellen Azure: Introductie van Upsert en projectie van de Query][]. Houd er rekening mee dat projectie wordt niet ondersteund op de lokale opslag-emulator, zodat deze code wordt alleen uitgevoerd als met een account op de service van de tabel.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Procedure: invoegen of vervangen van een entiteit

Vaak wilt u een entiteit toevoegen aan een tabel zonder te weten als het al in de tabel bestaat. Een insert-of vervangbewerking kunt u slechts één aanvraag die de entiteit wordt ingevoegd als het bestaat niet of bestaande vervangen als dit het geval is. Voortbouwend op eerdere voorbeelden, met de volgende code wordt ingevoegd of vervangen van de entiteit voor 'Walter Harp'. Na het maken van een nieuwe entiteit, roept deze code de methode **TableOperation.insertOrReplace** . Deze code vervolgens roept **uitvoeren** op het **CloudTable** -object met de tabel en het invoegen of vervangen van bewerking van de tabel als de parameters. Als u wilt slechts een gedeelte van een entiteit bijwerken, worden de methode **TableOperation.insertOrMerge** in plaats daarvan gebruikt. Houd er rekening mee dat invoegen of-vervangen wordt niet ondersteund op de lokale opslag-emulator, zodat deze code wordt alleen uitgevoerd als met een account op de service van de tabel. U kunt meer informatie over invoegen of vervangen en invoegen of-samenvoegen in dit [Azure tabellen: Introductie van Upsert en projectie van de Query][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Procedure: een entiteit verwijderen

Nadat u deze hebt opgehaald, kunt u eenvoudig een entiteit verwijderen. Als de entiteit is opgehaald, roept u **TableOperation.delete** met de entiteit te verwijderen. Roep vervolgens **uitvoeren** op het **CloudTable** -object. De volgende code haalt en de entiteit van een klant wordt verwijderd.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Procedure: een tabel verwijderen

De volgende code verwijdert ten slotte een tabel uit een opslag-account. Een tabel die is verwijderd niet beschikbaar worden gemaakt voor een periode na de verwijdering, meestal minder dan 40 seconden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Volgende stappen

U de basisbeginselen van tabelopslag hebt geleerd, klik op deze koppelingen als u wilt weten hoe meer complexe opslagtaken.

- [Azure opslag SDK voor Java][]
- [Verwijzing naar Azure opslag Client SDK][]
- [Azure opslag REST API][]
- [Azure opslag-teamblog][]

Zie ook de [Java Developer Center](/develop/java/)voor meer informatie.


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure opslag SDK voor Java]: https://github.com/azure/azure-storage-java
[Azure opslag SDK voor Android]: https://github.com/azure/azure-storage-android
[Verwijzing naar Azure opslag Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure opslag REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure tabellen: Introductie van Upsert en projectie van de Query]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
