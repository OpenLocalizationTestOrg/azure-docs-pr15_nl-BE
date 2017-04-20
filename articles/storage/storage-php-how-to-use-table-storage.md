<properties
    pageTitle="Het gebruik van tabelopslag van PHP | Microsoft Azure"
    description="Informatie over het gebruik van de tabel service van PHP maken en verwijderen van een tabel, en invoegen, verwijderen en opvragen van de tabel."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Het gebruik van tabelopslag van PHP

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's voor het gebruik van de tabel Azure service uitvoeren. De voorbeelden zijn geschreven in PHP en gebruikt u de [SDK voor PHP Azure][download]. De scenario's die zijn **maken en verwijderen van een tabel, en invoegen, verwijderen, en bij het controleren van de entiteiten in een tabel**. Voor meer informatie over de tabel Azure service, Zie de sectie [volgende stappen](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure-tabel is de verwijzing naar klassen in de SDK Azure voor PHP van uw code. Kunt u alle ontwikkelprogramma's voor het maken van uw toepassing, zoals Kladblok.

In deze handleiding gebruikt u de tabel service-functies die kunnen worden aangeroepen vanuit binnen een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een webpagina Azure rol, rol van werknemer of website.

## <a name="get-the-azure-client-libraries"></a>De Azure clientbibliotheken ophalen

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Uw toepassing configureren voor toegang tot de tabel service

Als de tabel Azure service API's gebruikt, moet u:

1. Een verwijzing naar de autoloader-bestand met behulp van de [require_once] [ require_once] -instructie en
2. Verwijzen naar alle klassen die u kunt gebruiken.

In het volgende voorbeeld ziet u hoe de autoloader-bestand opnemen en verwijzen naar de klasse **ServicesBuilder** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de PHP Client Libraries voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig hebt geïnstalleerd, moet u verwijzen naar de <code>WindowsAzure.php</code> autoloader-bestand.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In de voorbeelden hieronder, de `require_once` instructie wordt altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="set-up-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

U moet een geldige verbindingsreeks hebben voor het concretiseren van een tabel Azure serviceclient. De indeling voor de tabel service connection string is:

Voor toegang tot een live-service:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Voor toegang tot de opslag van de emulator:

    UseDevelopmentStorage=true


Elke client Azure service maken, moet u de klasse **ServicesBuilder** . U kunt:

* door de verbindingsreeks door te geven aan deze of
* **CloudConfigurationManager (CCM)** gebruiken om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen
    * u kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uit te breiden

Voor de hier beschreven voorbeelden, de verbindingsreeks rechtstreeks doorgegeven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Een tabel maken

Een **TableRestProxy** -object kunt u een tabel maken met de methode **createTable** . Wanneer u een tabel maakt, kunt u de tabel service-out instellen. (Zie voor meer informatie over de tabel service-timeout, [Time-outs instellen voor de tabel Service-activiteiten][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Zie voor informatie over beperkingen bij de namen van tabellen, [inzicht in de tabel Service Data Model][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Een entiteit aan een tabel toevoegen

Als een entiteit toevoegen aan een tabel, een nieuwe **entiteit** -object maken en doorgegeven aan de **TableRestProxy -> insertEntity**. Houd er rekening mee dat wanneer u een entiteit maakt, moet u een `PartitionKey` en `RowKey`. Dit zijn de unieke id's voor een entiteit en waarden die veel sneller dan andere entiteitseigenschappen kunnen worden opgezocht. Het systeem gebruikt `PartitionKey` entiteiten van de tabel automatisch over veel opslagknooppunten verdelen. Entiteiten met dezelfde `PartitionKey` op hetzelfde knooppunt worden opgeslagen. (Uitvoeren van bewerkingen op meerdere entiteiten opgeslagen op hetzelfde knooppunt beter dan op de entiteiten die zijn opgeslagen op verschillende knooppunten.) De `RowKey` is de unieke ID van een entiteit binnen een partitie.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Zie voor informatie over eigenschappen van tabel en typen, [inzicht in de tabel Service Data Model][table-data-model].

De klasse **TableRestProxy** biedt twee alternatieve methoden voor het invoegen van entiteiten: **insertOrMergeEntity** en **insertOrReplaceEntity**. Een nieuwe **entiteit** maakt en als parameter doorgeven aan een van de methoden voor het gebruik van deze methoden. Elke methode kan de entiteit wordt ingevoegd als deze nog niet bestaat. Als de entiteit al bestaat, **insertOrMergeEntity** eigenschapswaarden worden bijgewerkt als de eigenschappen nog bestaat en nieuwe eigenschappen worden toegevoegd als deze nog niet bestaan, terwijl **insertOrReplaceEntity** een bestaande entiteit volledig vervangt. In het volgende voorbeeld ziet u hoe u **insertOrMergeEntity**. Als de entiteit met de `PartitionKey` "tasksSeattle" en `RowKey` '1' nog niet bestaat, wordt ingevoegd. Echter, als deze eerder is ingevoegd (zoals in het bovenstaande voorbeeld), de `DueDate` wordt die eigenschap bijgewerkt, en de `Status` eigenschap wordt toegevoegd. De `Description` en `Location` eigenschappen worden ook bijgewerkt, maar met de waarden die effectief blijven ongewijzigd. Als deze laatste twee eigenschappen zijn niet toegevoegd, zoals in het voorbeeld, maar op de doelentiteit bestond, zouden de bestaande waarden ongewijzigd blijven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Ophalen van één enkele entiteit

De **TableRestProxy -> getEntity** -methode kunt u een enkele entiteit ophalen via het opvragen van de `PartitionKey` en `RowKey`. In het volgende voorbeeld wordt de partitiesleutel `tasksSeattle` en rijsleutel `1` worden doorgegeven aan de methode **getEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen

Entiteit-query's worden gemaakt met behulp van filters (Zie [tabellen opvragen en diensten]voor meer informatie,[filters]). Voor het ophalen van alle entiteiten in een partitie, gebruikt u het filter "PartitionKey eq *partitienaam*". In het volgende voorbeeld ziet u hoe het ophalen van alle entiteiten in de `tasksSeattle` partitie door een filter aan de methode **queryEntities** worden doorgegeven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Een subset van de entiteiten in een partitie ophalen

Hetzelfde patroon gebruikt in het vorige voorbeeld kan worden gebruikt voor het ophalen van een subset van de entiteiten in een partitie. De subset van de entiteiten die u ophaalt, wordt bepaald door het filter dat u wilt gebruiken (Zie [tabellen opvragen en diensten]voor meer informatie,[filters]). In het volgende voorbeeld ziet u hoe een filter te gebruiken voor het ophalen van alle entiteiten met een specifiek `Location` en een `DueDate` kleiner is dan een opgegeven datum.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Een subset van de entiteitseigenschappen ophalen

Een query kan een subset van de entiteitseigenschappen ophalen. Deze techniek, *projectie*, genaamd minder bandbreedte en kan query's sneller, met name voor grote entiteiten. Als u een eigenschap die moet worden opgehaald, moet u de naam van de eigenschap aan de methode **Query -> addSelectField** . U kunt deze methode meerdere keren aanroepen om meer eigenschappen toevoegen. De geretourneerde entiteiten hebben na het uitvoeren van **TableRestProxy -> queryEntities**, alleen de geselecteerde eigenschappen. (Als u een subset van de entiteiten tabel retourneren wilt, een filter gebruiken zoals in de bovenstaande query's.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Een entiteit bijwerken

Een bestaande entiteit kan worden bijgewerkt met behulp van de methoden **entiteit -> setProperty** en **entiteit -> addProperty** op de entiteit en vervolgens roepen **TableRestProxy -> updateEntity**. In het volgende voorbeeld haalt een entiteit, een eigenschap wijzigt, verwijdert u een andere eigenschap en wordt een nieuwe eigenschap toegevoegd. Houd er rekening mee dat u een eigenschap verwijderen kunt door de waarde **null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Geven u een entiteit verwijdert, de naam van de tabel en van de entiteit `PartitionKey` en `RowKey` de **TableRestProxy -> deleteEntity** -methode.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Voor gelijktijdige controles, u kunt instellen: de Etag voor een entiteit worden verwijderd met behulp van de methode **DeleteEntityOptions -> setEtag** en het **DeleteEntityOptions** -object doorgegeven aan **deleteEntity** als een vierde parameter.

## <a name="batch-table-operations"></a>Tabel batchbewerkingen

De methode **TableRestProxy -> batch** kunt u meerdere bewerkingen uitvoeren in een enkele aanvraag. Het patroon hier worden bewerkingen toe te voegen aan **BatchRequest** -object en vervolgens het **BatchRequest** -object wordt doorgegeven aan de methode **TableRestProxy -> batch** . Als een bewerking toevoegen aan een **BatchRequest** -object, kunt u bellen een van de volgende manieren meerdere keren:

* **addInsertEntity** (een insertEntity bewerking toevoegen)
* **addUpdateEntity** (een updateEntity bewerking toevoegen)
* **addMergeEntity** (voegt een mergeEntity werking)
* **addInsertOrReplaceEntity** (een insertOrReplaceEntity bewerking toevoegen)
* **addInsertOrMergeEntity** (een insertOrMergeEntity bewerking toevoegen)
* **addDeleteEntity** (voegt een deleteEntity werking)

In het volgende voorbeeld ziet u hoe **insertEntity** en **deleteEntity** bewerkingen uitvoeren in een enkele aanvraag:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Zie voor meer informatie over bewerkingen batchen [Entiteitstransacties uitvoeren van][entity-group-transactions].

## <a name="delete-a-table"></a>Een tabel verwijderen

Ten slotte om een tabel te verwijderen, geeft u de naam van de tabel aan de methode **TableRestProxy -> deleteTable** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de tabel Azure service, volg deze koppelingen voor meer informatie over complexere opslagtaken.

- Ga naar de [opslag van Azure-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

Zie ook de [PHP Developer Center](/develop/php/)voor meer informatie.

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
