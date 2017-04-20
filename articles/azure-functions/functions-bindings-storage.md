<properties
    pageTitle="Azure functies, triggers en bindingen voor Azure opslag | Microsoft Azure"
    description="Werken met opslag Azure triggers en bindingen in Azure functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking van gebeurtenissen, dynamische compute, zonder server architecture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure functies, triggers en bindingen voor Azure opslag

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe te configureren en code Azure opslag triggers en bindingen in Azure functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure Storage queue trigger

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON voor opslag wachtrij trigger

Het bestand *function.json* bevat de volgende eigenschappen.

- `name`: De variabele naam gebruikt in de functiecode voor de wachtrij of de wachtrij weergegeven. 
- `queueName`: De naam van de wachtrij op te vragen. Zie voor naamgevingsregels wachtrij, [wachtrijen naamgeving en metagegevens](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: De naam van de instelling van een app waarin een verbindingsreeks opslag. Als u niet `connection` leeg is, de trigger werkt met de standaard opslag-verbindingsreeks voor de functie die is opgegeven door de app AzureWebJobsStorage app.
- `type`: Het moet worden ingesteld op *queueTrigger*.
- `direction`: *In*moet worden ingesteld. 

Voorbeeld van de *function.json* voor storage queue triggers:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Wachtrij trigger ondersteunde typen

Het bericht wachtrij kan worden gedeserialiseerd aan een van de volgende typen:

* Object (JSON)
* Tekenreeks
* Byte-matrix 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Wachtrij trigger metagegevens

Wachtrij-metagegevens kunt in uw functie u opvragen met behulp van deze variabele namen:

* expirationTime
* insertionTime
* nextVisibleTime
* ID
* popReceipt
* dequeueCount
* queueTrigger (een andere manier tekst van het bericht wachtrij als een tekenreeks ophalen)

In dit voorbeeld C#-code opgehaald en metagegevens in de wachtrij logs:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Verwerking van gevaarlijke berichten

*Gevaarlijke berichten*, worden berichten waarvan de inhoud zorgt ervoor een functie dat niet genoemd. Wanneer de functie is mislukt, wordt het bericht wachtrij wordt niet verwijderd en uiteindelijk is opgenomen, waardoor de cyclus worden herhaald. De SDK kan automatisch de cyclus onderbreken na een beperkt aantal iteraties, of u kunt dit handmatig doen.

De SDK wordt een functie maximaal 5 keer aanroept voor het verwerken van een wachtrij. Als de vijfde poging mislukt, wordt het bericht verplaatst naar een wachtrij poison.

De wachtrij verontreinigd is met de naam *{originalqueuename}*-poison. U kunt schrijven een functie op berichten uit de wachtrij verontreinigd door deze registratie of het verzenden van een melding die handmatige aandacht nodig is. 

Als u van gevaarlijke berichten handmatig te verwerken wilt, krijgt u het aantal keren dat een bericht trok voor verwerking door te controleren `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure Storage queue uitvoer binding

#### <a name="functionjson-for-storage-queue-output-binding"></a>Function.JSON voor opslag wachtrij uitvoer binding

Het bestand *function.json* bevat de volgende eigenschappen.

- `name`: De variabele naam gebruikt in de functiecode voor de wachtrij of de wachtrij weergegeven. 
- `queueName`: De naam van de wachtrij. Zie voor naamgevingsregels wachtrij, [wachtrijen naamgeving en metagegevens](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: De naam van de instelling van een app waarin een verbindingsreeks opslag. Als u niet `connection` leeg is, de trigger werkt met de standaard opslag-verbindingsreeks voor de functie die is opgegeven door de app AzureWebJobsStorage app.
- `type`: Het moet worden ingesteld op *wachtrij*.
- `direction`: Moet zijn ingesteld op *uit*. 

Voorbeeld van de *function.json* voor een wachtrij opslag uitvoer bindend die gebruikmaakt van de trigger voor een wachtrij en schrijft een bericht wachtrij:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Wachtrijtypen uitvoer binding ondersteund

De `queue` binding kan de volgende typen op een bericht wachtrij serialiseren:

* Object (`out T` in C#, wordt een bericht gemaakt met een null-object dat de parameter null wanneer de functie eindigt)
* Tekenreeks (`out string` in C#, maakt message queue als parameterwaarde niet null is wanneer de functie eindigt)
* Byte-matrix (`out byte[]` in C#, werkt als tekenreeks) 
* `out CloudQueueMessage`(C#, werkt net als tekenreeks) 

In C# kunt u ook binden aan `ICollector<T>` of `IAsyncCollector<T>` waarbij `T` is een van de ondersteunde typen.

#### <a name="queue-output-binding-code-examples"></a>Voorbeelden van uitvoer binding-code in een wachtrij

In dit voorbeeld C#-code wordt een enkele uitvoer wachtrij voor elk bericht invoerwachtrij.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

Meerdere berichten in dit voorbeeld C#-code schrijft met behulp van `ICollector<T>` (Gebruik `IAsyncCollector<T>` van een asynchrone-functie):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure opslag blob trigger

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON voor opslag blob trigger

Het bestand *function.json* bevat de volgende eigenschappen.

- `name`: De variabele naam gebruikt in de functiecode voor de blob. 
- `path`: Pad Hiermee geeft u de container te controleren en eventueel een naampatroon blob.
- `connection`: De naam van de instelling van een app waarin een verbindingsreeks opslag. Als u niet `connection` leeg is, de trigger werkt met de standaard opslag-verbindingsreeks voor de functie die is opgegeven door de app AzureWebJobsStorage app.
- `type`: Het moet worden ingesteld op *blobTrigger*.
- `direction`: *In*moet worden ingesteld.

Voorbeeld van de *function.json* voor triggers blob storage dat controleert of het aantal BLOB's die worden toegevoegd aan de container monsters workitems:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>BLOB trigger ondersteunde typen

De blob kan worden gedeserialiseerd aan een van de volgende typen in een knooppunt of C#-functies:

* Object (JSON)
* Tekenreeks

In C#-functies kunt u ook binden aan een van de volgende typen:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

#### <a name="blob-trigger-c-code-example"></a>BLOB trigger C#-codevoorbeeld

In dit voorbeeld C#-code registreert de inhoud van elke blob die wordt toegevoegd aan de container gecontroleerde.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>BLOB trigger naam patronen

Kunt u een patroon blob naam in de `path` eigenschap. Bijvoorbeeld:

```json
"path": "input/original-{name}",
```

Dit pad vinden een blob met de naam *Oorspronkelijke Blob1.txt* in de container van de *invoer* en de waarde van de `name` variabele in de functiecode `Blob1`.

Een ander voorbeeld:

```json
"path": "input/{blobname}.{blobextension}",
```

Dit pad zou ook zoeken naar een blob met de naam *Oorspronkelijke Blob1.txt*en de waarde van de `blobname` en `blobextension` variabelen in functie zou zijn *Oorspronkelijke Blob1* en *txt*.

U kunt de BLOB's die de functie activeren door te geven van een patroon met een vaste waarde voor de bestandsextensie typen beperken. Als u de `path` *samples / {naam} .png*alleen *.png* BLOB's in de *monsters* container de functie wordt geactiveerd.

Als u een naampatroon opgeven voor blob namen met accolades in de naam, dubbelklikt u de accolades. Bijvoorbeeld, als u wilt zoeken naar BLOB's in de container voor *afbeeldingen* met namen als volgt:

        {20140101}-soundfile.mp3

Gebruik deze optie voor de `path` eigenschap:

        images/{{20140101}}-{name}

In het voorbeeld wordt de `name` de waarde van variabele *soundfile.mp3*zou zijn. 

#### <a name="blob-receipts"></a>BLOB-ontvangsten

De runtime-functies Azure zorgt ervoor dat geen trigger blob, functie meer dan één keer wordt aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Dit gebeurt door het bijhouden van *blob ontvangsten* te bepalen als een bepaald blob-versie is verwerkt.

BLOB ontvangsten worden opgeslagen in een container met de naam *azure-webjobs-hosts* in de Azure opslag-account die is opgegeven door de tekenreeks AzureWebJobsStorage. Een blob ontvangst heeft de volgende informatie:

* De functie die is aangeroepen voor de blob ('*{naam functie app}*. Functies. *{naam}*', bijvoorbeeld: "functionsf74b96f7. Functions.CopyBlob")
* De containernaam van de
* Het type blob ("BlockBlob" of "PageBlob")
* De naam van de blob
* De ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Als u afdwingen dat opwerking van een blob wilt, kunt u de ontvangst voor dat blob blob handmatig verwijderen uit de container *azure-webjobs-hosts* .

#### <a name="handling-poison-blobs"></a>Poison BLOB's verwerken

Wanneer een trigger blob, functie mislukt, roept de SDK deze opnieuw, in het geval de fout is veroorzaakt door een tijdelijke fout. Als de fout wordt veroorzaakt door de inhoud van de blob, wordt de functie niet elke keer dat de blob wordt verwerkt. Standaard roept de SDK een functie maximaal 5 keer voor een bepaalde blob. Als de vijfde poging mislukt, wordt een bericht toegevoegd aan een wachtrij met de naam *webjobs-blobtrigger-poison*van de SDK.

Het bericht wachtrij voor poison BLOB's is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie *{functie app naam}*. Functies. *{naam}*)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>BLOB polling voor grote containers

Als de blob-container die het controleren van de trigger meer dan 10.000 BLOB's bevat, logboekbestanden de runtime functies scans voor nieuwe of gewijzigde BLOB's bekijken. Dit proces is geen real-time; een functie kan niet ophalen geactiveerd tot enkele minuten of meer nadat de blob is gemaakt. Daarnaast [opslag logboeken worden gemaakt van "goede bedoelingen"](https://msdn.microsoft.com/library/azure/hh343262.aspx) basis; Er is geen garantie dat alle gebeurtenissen worden vastgelegd. Onder bepaalde omstandigheden mogelijk Logboeken worden overgeslagen. Als de beperkingen voor snelheid en betrouwbaarheid van blob triggers voor grote containers niet toegestaan voor uw toepassing zijn, is de aanbevolen methode voor het maken van een wachtrij bericht wanneer u de blob maken en de trigger voor een wachtrij in plaats van een blob-trigger gebruiken voor het verwerken van de blob.
 
## <a id="storageblobbindings"></a>Azure blob voor opslag, invoer en uitvoer van bindingen

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON voor een blob storage in- of uitvoer van binding

Het bestand *function.json* bevat de volgende eigenschappen.

- `name`: De variabele naam gebruikt in de functiecode voor de blob. 
- `path`: Pad Hiermee geeft u de container voor de blob van lezen of schrijven van de blob te en eventueel een naampatroon blob.
- `connection`: De naam van de instelling van een app waarin een verbindingsreeks opslag. Als u niet `connection` leeg is, de binding werkt met de standaard opslag-verbindingsreeks voor de functie die is opgegeven door de app AzureWebJobsStorage app.
- `type`: Moet worden ingesteld op de *blob*.
- `direction`: Stel *in* of *uit*. 

Voorbeeld *function.json* voor een opslag blob-input of output binding, een blob kopiëren met behulp van de trigger voor een wachtrij:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>BLOB-invoer en uitvoer van de ondersteunde typen

De `blob` binding kan serialiseren of terugconverteren van de volgende typen in Node.js of C#-functies:

* Object (`out T` in C# voor uitvoer blob: maakt een blob als null-object als parameterwaarde null is wanneer de functie eindigt)
* Tekenreeks (`out string` in C# voor uitvoer blob: een blob wordt gemaakt als de parameter string niet null is, de functie als resultaat wordt gegeven)

In C#-functies, kunt u ook binden aan de volgende typen:

* `TextReader`(uitsluitend invoer)
* `TextWriter`(alleen voor uitvoer)
* `Stream`
* `CloudBlobStream`(alleen voor uitvoer)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>BLOB uitvoer C#-codevoorbeeld

In dit voorbeeld C#-code een blob waarvan de naam wordt ontvangen in een bericht wachtrij opgehaald.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Tabellen met Azure opslag, invoer en uitvoer van bindingen

#### <a name="functionjson-for-storage-tables"></a>Function.JSON voor opslag tabellen

De *function.json* bevat de volgende eigenschappen.

- `name`: De naam van variabele in de functiecode voor het binden van tabel gebruikt. 
- `tableName`: De naam van de tabel.
- `partitionKey`en `rowKey` : één enkele entiteit in een C# of knooppunt functie lezen of schrijven van een enkele entiteit in de functie van een knooppunt samen worden gebruikt.
- `take`: Het maximum aantal rijen dat moet worden gelezen voor tabel invoeren in de functie van een knooppunt.
- `filter`: OData-filterexpressie voor tabel invoeren in de functie van een knooppunt.
- `connection`: De naam van de instelling van een app waarin een verbindingsreeks opslag. Als u niet `connection` leeg is, de binding werkt met de standaard opslag-verbindingsreeks voor de functie die is opgegeven door de app AzureWebJobsStorage app.
- `type`: Moet worden ingesteld op de *tabel*.
- `direction`: Stel *in* of *uit*. 

Het volgende voorbeeld *function.json* wordt de trigger voor een wachtrij voor het lezen van een enkele tabelrij. De JSON biedt een partitie vastgelegde waarde en geeft aan dat de rijsleutel afkomstig van de wachtrij weergegeven is.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Opslag tabellen invoer- en uitvoerfilters ondersteunde typen

De `table` binding kan serialiseren of terugconverteren van objecten in Node.js of C#-functies. De objecten worden de eigenschappen RowKey en PartitionKey hebben. 

In C#-functies, kunt u ook binden aan de volgende typen:

* `T`waar `T` implementeert`ITableEntity`
* `IQueryable<T>`(uitsluitend invoer)
* `ICollector<T>`(alleen voor uitvoer)
* `IAsyncCollector<T>`(alleen voor uitvoer)

#### <a name="storage-tables-binding-scenarios"></a>Opslag tabellen binding scenario 's

De tabelbinding ondersteunt de volgende scenario's:

* Een enkele rij in een C# of knooppunt functie lezen.

    Stel `partitionKey` en `rowKey`. De `filter` en `take` eigenschappen worden niet gebruikt in dit scenario.

* Meerdere rijen in een C#-functie lezen.

    De runtime functies biedt een `IQueryable<T>` object is gekoppeld aan de tabel. Type `T` moet zijn afgeleid van `TableEntity` of implementeren van `ITableEntity`. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen worden niet gebruikt in dit scenario; u kunt de `IQueryable` -object om alle filters vereist. 

* Meerdere rijen in een knooppunt functie lezen.

    Stel de `filter` en `take` eigenschappen. Geen `partitionKey` of `rowKey`.

* Schrijf een of meer rijen in een C#-functie.

    De runtime functies biedt een `ICollector<T>` of `IAsyncCollector<T>` gekoppeld aan de tabel, waar `T` geeft het schema van de entiteiten die u wilt toevoegen. Normaal gesproken typt `T` is afgeleid van `TableEntity` of implementeert `ITableEntity`, maar deze niet hoeven te worden. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen worden niet gebruikt in dit scenario.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Voorbeeld van opslag tabellen: een met één Tabelentiteit in C# of knooppunt lezen

De volgende C#-voorbeeldcode werkt met het vorige *function.json* -bestand eerder is getoond voor het lezen van een entiteit één tabel. Het bericht wachtrij heeft de waarde van de rij en de Tabelentiteit wordt gelezen in een type dat is gedefinieerd in het bestand *run.csx* . Bevat het type `PartitionKey` en `RowKey` eigenschappen en wordt niet afgeleid van `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

De volgende F # voorbeeld werkt ook met het vorige *function.json* -bestand te lezen van een entiteit één tabel.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

In het volgende voorbeeld knooppunt werkt ook met het vorige *function.json* -bestand te lezen van een entiteit één tabel.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Voorbeeld van de tabellen opslag: lezen van meerdere entiteiten van tabel c# 

De volgende *function.json* en C#-code voorbeeld leest entiteiten voor een partitie die is opgegeven in de wachtrij weergegeven.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De C#-code voegt een verwijzing naar de opslag Azure SDK, zodat het entiteitstype kan worden afgeleid `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Voorbeeld van de opslag tabellen: tabel entiteiten maken in C# 

De volgende *function.json* en *run.csx* -voorbeeld ziet u hoe tabel entiteiten in C# schrijven.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Voorbeeld van de opslag tabellen: tabel entiteiten maken in F#

In het volgende voorbeeld van *function.json* en *run.fsx* ziet u hoe u tabel entiteiten in F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Voorbeeld van opslag tabellen: een Tabelentiteit in knooppunt maken

Het volgende *function.json* en *run.csx* -voorbeeld ziet hoe een Tabelentiteit van het knooppunt.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
