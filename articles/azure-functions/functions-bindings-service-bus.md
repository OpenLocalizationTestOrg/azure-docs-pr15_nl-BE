<properties
    pageTitle="Azure Service Bus van functies, triggers en bindingen | Microsoft Azure"
    description="Werken met triggers Azure Service Bus en bindingen in Azure functies."
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
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure Service Bus van functies, triggers en bindingen voor wachtrijen en onderwerpen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe te configureren en code Azure Service Bus triggers en bindingen in Azure functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>De wachtrij of onderwerp trigger Service Bus

#### <a name="functionjson"></a>Function.JSON

Het bestand *function.json* bevat de volgende eigenschappen.

- `name`: De naam van variabele in de functiecode wordt gebruikt voor de wachtrij, onderwerp of het bericht wachtrij of onderwerp. 
- `queueName`: Activeren, worden alleen de naam van de wachtrij op te vragen voor de wachtrij.
- `topicName`: Trigger, worden alleen de naam van het onderwerp op te vragen voor het onderwerp.
- `subscriptionName`: Trigger, naam van het abonnement voor het onderwerp.
- `connection`: De naam van de instelling van een app waarin een verbindingsreeks Service Bus. De verbindingsreeks moet zijn voor een Service Bus-naamruimte niet beperkt tot een bepaalde wachtrij of onderwerp. Als u de verbindingsreeks hebt rechten niet beheren, de `accessRights` eigenschap. Als u niet `connection` leeg is, de trigger of de binding werkt met de tekenreeks standaard Service Bus verbinding voor de toepassing van de functie, die wordt opgegeven door de instelling van de AzureWebJobsServiceBus app.
- `accessRights`: Hiermee geeft u de toegangsrechten voor de verbindingsreeks. Dit is de `manage`. Ingesteld op `listen` als u een verbindingsreeks die geen machtigingen beheren. De functies runtime proberen en niet te doen bewerkingen waarvoor anders beheren rechten.
- `type`: Het moet worden ingesteld op *serviceBusTrigger*.
- `direction`: *In*moet worden ingesteld. 

Voorbeeld van de *function.json* voor een Service Bus wachtrij trigger:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C#-codevoorbeeld waarin een bericht Service Bus wachtrij verwerkt

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # voorbeeld van code waarmee een bericht Service Bus wachtrij verwerkt

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Voorbeeld van code node.js die een bericht Service Bus wachtrij verwerkt

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Ondersteunde typen

Het bericht Service Bus wachtrij kan worden gedeserialiseerd aan een van de volgende typen:

* Object (JSON)
* tekenreeks
* byte-matrix 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock gedrag

De uitvoering van de functies in een bericht ontvangt `PeekLock` modus en roept `Complete` in het bericht als de functie is voltooid of oproepen `Abandon` als de functie is mislukt. Als de functie wordt uitgevoerd langer dan de `PeekLock` -time-out van de vergrendeling automatisch wordt vernieuwd.

#### <a id="sbpoison"></a>Verwerking van gevaarlijke berichten

Bus service heeft een eigen poison berichtafhandeling die niet kan worden beheerd of geconfigureerd in Azure functies configuration of code. 

#### <a id="sbsinglethread"></a>Single-threading

De functies verwerkt runtime standaard meerdere berichten tegelijkertijd. Stelt de runtime verwerken alleen een enkele wachtrij of onderwerp bericht rechtstreeks, `serviceBus.maxConcurrrentCalls` op 1 in het bestand *host.json* . Zie voor meer informatie over het bestand *host.json* [Mapstructuur](functions-reference.md#folder-structure) in het artikel ontwikkelaar en [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in de bibliotheek WebJobs.Script wiki.

## <a id="sboutput"></a>Binding-uitvoer Service Bus wachtrij of onderwerp.

#### <a name="functionjson"></a>Function.JSON

Het bestand *function.json* bevat de volgende eigenschappen.

- `name`: De variabele naam gebruikt in de functiecode voor de wachtrij of wachtrij weergegeven. 
- `queueName`: Activeren, worden alleen de naam van de wachtrij op te vragen voor de wachtrij.
- `topicName`: Trigger, worden alleen de naam van het onderwerp op te vragen voor het onderwerp.
- `subscriptionName`: Trigger, naam van het abonnement voor het onderwerp.
- `connection`: Gelijk aan die voor Bus-Service starten.
- `accessRights`: Hiermee geeft u de toegangsrechten voor de verbindingsreeks. Dit is de `manage`. Ingesteld op `send` als u een verbindingsreeks die geen machtigingen beheren. Anders beheren de runtime proberen functies en niet te doen bewerkingen waarvoor rechten, zoals het maken van wachtrijen.
- `type`: Het moet worden ingesteld op *serviceBus*.
- `direction`: Moet zijn ingesteld op *uit*. 

Voorbeeld van de *function.json* voor het gebruik van een trigger timer Service Bus berichten schrijven:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Ondersteunde typen

Azure functies kunnen maken van een wachtrij Service Bus bericht van een van de volgende typen.

* Object (altijd een JSON-bericht wordt gemaakt, wordt het bericht gemaakt met een null-object als de waarde null is wanneer de functie eindigt)
* tekenreeks (een bericht wordt gemaakt als de waarde niet null is wanneer de functie eindigt)
* byte-matrix (werkt net als tekenreeks) 
* `BrokeredMessage`(C#, werkt net als tekenreeks)

Voor het maken van meerdere berichten in een C#-functie, kunt u `ICollector<T>` of `IAsyncCollector<T>`. Een bericht wordt gemaakt wanneer u contact opneemt met de `Add` methode.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# codevoorbeelden die Service Bus berichten maken

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # voorbeeld van code waarmee een bericht van de wachtrij Service Bus

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Voorbeeld van code node.js die een bericht Service Bus wachtrij maken

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
