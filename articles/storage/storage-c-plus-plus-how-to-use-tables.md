<properties
    pageTitle="Het gebruik van tabelopslag (C++) | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud opslag Azure-tabel, een gegevensarchief NoSQL."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>Het gebruik van opslag van C++ tabel

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht  
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's uitvoeren met behulp van de tabel Azure storage-service. De monsters worden geschreven in C++ en gebruikt de [Clientbibliotheek van Azure opslag voor C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's die zijn **maken en verwijderen van een tabel** en het **werken met tabel-entiteiten**.

>[AZURE.NOTE] Deze handleiding is bedoeld voor de Azure opslag Client Library for C++ versie 1.0.0 en boven. De aanbevolen versie is opslag clientbibliotheek 2.2.0, beschikbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Maak een C++-toepassing  
In deze handleiding vindt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing gebruikt. Hiertoe moet u de Azure opslag Client-bibliotheek voor C++ installeren en een account Azure opslag maken van je abonnement op Azure.  

Als u wilt installeren de Azure opslag Client Library for C++, kunt u de volgende methoden:

-   **Linux:** Volg de instructies op de pagina [Azure opslag Client-bibliotheek voor C++ Leesmij-bestand](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** Klik in Visual Studio **Extra > NuGet Package Manager > Package Manager-Console**. Typ de volgende opdracht in de [console NuGet Package Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , en druk op Enter.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Uw toepassing configureren voor toegang tot opslag van tabel  
Voeg dat de volgende instructies aan de bovenkant van de C++-bestand waar u de Azure opslag API's gebruiken om toegang te krijgen tot tabellen opnemen:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen  
Een opslag Azure-client gebruikt een verbindingsreeks opslag eindpunten en referenties voor het openen van gegevens wilt opslaan. Wanneer een clienttoepassing wordt uitgevoerd, moet u de verbindingstekenreeks opslag in de volgende indeling opgeven. De naam van uw account voor opslag en opslag toegangstoets gebruiken voor de opslag in [Azure Portal](https://portal.azure.com) voor de *accountnaam* en *AccountKey* -waarden weergegeven. Zie voor informatie over opslag rekeningen en toegangstoetsen, [over Azure opslag rekeningen](storage-create-storage-account.md). In dit voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Test uw toepassing in uw lokale Windows-computer, kunt u Azure [opslag emulator](storage-use-emulator.md) die met de [Azure SDK](https://azure.microsoft.com/downloads/)is geïnstalleerd. De opslag-emulator is een hulpprogramma dat de Azure Blob, wachtrij en tabel services die beschikbaar zijn op uw computer voor plaatselijke ontwikkeling simuleert. In het volgende voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks aan uw lokale opslag emulator kunt declareren:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

De emulator Azure opslag starten, klikt u op de knop **Start** of de Windows-toets. Begin met typen **Azure opslag Emulator**en selecteer **Microsoft Azure opslag Emulator** uit de lijst met toepassingen.  

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om de verbindingsreeks voor opslag.  

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen  
De klasse **cloud_storage_account** kunt u uw accountgegevens opslag vertegenwoordigen. Voor het ophalen van gegevens van uw opslag uit de verbindingsreeks voor de opslag, kunt u de parseermethode.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Vervolgens krijgt een verwijzing naar een klasse **cloud_table_client** als deze, kunt u de verwijzing naar objecten ophalen voor tabellen en entiteiten die zijn opgeslagen in de tabel storage-service. De volgende code maakt een **cloud_table_client** -object met behulp van het accountobject van de opslag dat we boven opgehaald:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Een tabel maken
Een **cloud_table_client** -object kunt u de verwijzing naar objecten ophalen voor tabellen en entiteiten. De volgende code wordt een **cloud_table_client** -object gemaakt en gebruikt om een nieuwe tabel te maken.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Een entiteit aan een tabel toevoegen
Een entiteit toevoegen aan een tabel, een nieuw **table_entity** -object maken en doorgeven aan **table_operation::insert_entity**. De volgende code wordt de voornaam van de klant als de rijsleutel- en achternaam als de partitiesleutel. Samen identificeren de partitie van een entiteit en rijsleutel de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel sneller dan die met andere partitiesleutels kunnen worden opgezocht, maar met verschillende partitiesleutels kan grotere schaalbaarheid van parallelle bewerking. Zie voor meer informatie [Microsoft Azure opslag prestaties en schaalbaarheid, controlelijst](storage-performance-checklist.md).

De volgende code maakt een nieuw exemplaar van de **table_entity** met enkele klantgegevens kunnen worden opgeslagen. De code vervolgens roept **table_operation::insert_entity** wilt maken van een **table_operation** -object een entiteit in een tabel invoegen en de nieuwe Tabelentiteit gekoppeld. Ten slotte roept de code de methode execute van het object **cloud_table** . En de nieuwe **table_operation** verzendt een verzoek naar de service tabel invoegen de nieuwe klantentiteit in de tabel 'gebruikers'.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Invoegen van een batch van entiteiten
U kunt een batch van entiteiten die aan de service tabel invoegen in een schrijfbewerking. De volgende code wordt een **table_batch_operation** -object gemaakt en worden vervolgens toegevoegd dat drie bewerkingen aan het invoegen. Elke invoegbewerking is door het maken van een nieuwe entiteitsobject, stelt u de waarden en vervolgens aanroepen van de methode insert voor het object **table_batch_operation** op de entiteit koppelen aan een nieuwe invoegbewerking toegevoegd. Vervolgens **cloud_table.execute** aangeroepen om de bewerking niet uitvoeren.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Sommige dingen om te weten op batch-bewerkingen:  

-   U kunt maximaal 100 invoegen, verwijderen, samenvoegen, vervangen, samenvoegen of invoegen en invoegen of vervangen in een willekeurige combinatie in één batch uitvoeren.  
-   Een batchbewerking kan een bewerking ophalen hebben als het is de enige bewerking in de batch.  
-   Alle entiteiten in een enkele batch-bewerking moeten dezelfde partitiesleutel hebben.  
-   Een batchbewerking is beperkt tot een nettolading van 4 MB.  

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen
Gebruik een **table_query** -object om een querytabel voor alle entiteiten in een partitie. In het volgende voorbeeld bevat een filter voor entiteiten waar 'Smith' de partitiesleutel is. In dit voorbeeld wordt de velden van elke entiteit in de resultaten van de query naar de console.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

De query in dit voorbeeld wordt de entiteiten die aan de filtercriteria voldoen. Als u grote tabellen moet downloaden van de tabel diensten vaak, raden we opslaan u uw gegevens in opslag Azure BLOB's in plaats daarvan.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Een bereik van de entiteiten in een partitie ophalen
Als u niet wilt opvragen van de entiteiten in een partitie, kunt u een bereik door de combinatie van de belangrijkste filter partitie met een rij belangrijke filter. In het volgende voorbeeld worden twee filters gebruikt om alle entiteiten in partitie Smith waar de rij (voornaam) eerder dan "E" in het alfabet begint met een letter en vervolgens de resultaten van de query worden afgedrukt.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Ophalen van één enkele entiteit
U kunt een query voor het ophalen van een enkele, specifieke entiteit schrijven. **Table_operation::retrieve_entity** de volgende code gebruikt om op te geven van de klant 'Jeff Smith'. Deze methode retourneert één entiteit, in plaats van een verzameling en de geretourneerde waarde is in **table_result**. Partitie-en opgeven in een query is de snelste manier om een enkele entiteit worden opgehaald uit de tabel service.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Vervangen van een entiteit
Te vervangen door een andere entiteit, halen uit de tabel service, het entiteitsobject wijzigen en vervolgens de wijzigingen opgeslagen in de tabel service. De volgende code wijzigt een bestaande klant telefoonnummer en e-mailadres. Deze code gebruikt in plaats van het aanroepen van **table_operation::insert_entity**, **table_operation::replace_entity**. Hierdoor wordt de entiteit moet volledig worden vervangen op de server, tenzij de entiteit op de server is gewijzigd sinds deze is opgehaald, in welk geval mislukt de bewerking. Deze fout wordt voorkomen dat de toepassing van het per ongeluk overschrijven van een wijziging aangebracht tussen het ophalen en bijwerken door een ander onderdeel van uw toepassing. De juiste afhandeling van deze fout is de entiteit opnieuw ophalen, breng de gewenste wijzigingen (als dat nog steeds geldig is) en vervolgens een andere **table_operation::replace_entity** -bewerking uitvoeren. De volgende sectie wordt beschreven hoe u dit gedrag wijzigen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Invoegen of-vervangen een entiteit
**table_operation::replace_entity** -bewerkingen mislukken als de entiteit is gewijzigd sinds deze is opgehaald van de server. Bovendien moet u de entiteit ophalen van de server eerst in om **table_operation::replace_entity** te laten slagen. Soms, maar u niet weet of de entiteit op de server bestaat en de huidige waarden in deze niet relevant zijn, de update ze allemaal moet overschrijven. Om dit te bereiken, gebruikt u een **table_operation::insert_or_replace_entity** -bewerking. Deze bewerking wordt de entiteit ingevoegd als deze niet bestaat, of vervangen als dit het geval is, ongeacht wanneer de laatste update is gemaakt. In het volgende codevoorbeeld wordt nog steeds de klantentiteit voor Jeff Smith opgehaald, maar deze wordt vervolgens opgeslagen op de server via **table_operation::insert_or_replace_entity**. Wijzigingen in de entiteit tussen het ophalen en bijwerken worden, overschreven.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Een subset van de entiteitseigenschappen opvragen  
Een query in een tabel kunt u een paar eigenschappen ophalen uit een entiteit. De query in de volgende code wordt de methode **table_query::set_select_columns** retourneert alleen de e-mailadressen van de entiteiten in de tabel.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Bij het controleren van een paar eigenschappen van een entiteit is een meer efficiënte werking dan het ophalen van alle eigenschappen.

## <a name="delete-an-entity"></a>Een entiteit verwijderen
Nadat u deze hebt opgehaald, kunt u eenvoudig een entiteit verwijderen. Zodra de entiteit is opgehaald, roept u **table_operation::delete_entity** aan de entiteit die u wilt verwijderen. Vervolgens roept de methode **cloud_table.execute** . De volgende code haalt en een entiteit met een partitiesleutel van 'Smit' en een rijsleutel van de "Jan" verwijdert.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Een tabel verwijderen
In het volgende voorbeeld wordt een tabel ten slotte vanuit een account opslag verwijderd. Een tabel die is verwijderd niet opnieuw te worden gemaakt voor een periode na de verwijdering beschikbaar.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd dat de basisbeginselen van de tabelopslag, gaat u als volgt deze koppelingen voor meer informatie over Azure opslag:  

-   [Het gebruik van C++ Blob-opslag](storage-c-plus-plus-how-to-use-blobs.md)
-   [Het gebruik van opslag van C++ wachtrij](storage-c-plus-plus-how-to-use-queues.md)
-   [Lijst van Azure opslagbronnen in C++](storage-c-plus-plus-enumeration.md)
-   [Client-bibliotheek voor C++ verwijzing van opslag](http://azure.github.io/azure-storage-cpp)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
