<properties 
    pageTitle="Overzicht van de gebeurtenis Azure Hubs API's | Microsoft Azure"
    description="Een overzicht van enkele van de belangrijkste gebeurtenis Hubs .NET client-API."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Overzicht van de gebeurtenissen Hubs API

Dit artikel bevat een overzicht van enkele van de sleutel gebeurtenis Hubs .NET client-API's. Er zijn twee categorieën: beheer en de runtime-API's. Runtime-API's bestaan uit alle bewerkingen die nodig zijn voor het verzenden en ontvangen van een bericht. Beheer kunnen u de status van een gebeurtenis Hubs entiteit door maken, bijwerken en verwijderen van entiteiten beheren.

Controlescenario's omvatten het beheer en de uitvoering. Zie voor gedetailleerde documentatie over de API's van .NET [Service Bus.NET](https://msdn.microsoft.com/library/azure/mt419900.aspx) en de [EventProcessorHost-API](https://msdn.microsoft.com/library/azure/mt445521.aspx) verwijzingen.

## <a name="management-apis"></a>Beheer-API 's

Als u de volgende beheertaken uit te voeren, hebt u machtigingen **beheren** voor de gebeurtenis Hubs-naamruimte:

### <a name="create"></a>Maken

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Update

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Verwijderen

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>Runtime-API 's

### <a name="create-publisher"></a>Publicatie maken

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Bericht publiceren

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Consument maken

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Bericht verbruiken

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Gebeurtenis processor host API 's

Deze API's bieden tolerantie aan werkprocessen die mogelijk niet meer beschikbaar, door shards distribueren over beschikbare werknemers.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

De interface [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) wordt als volgt gedefinieerd:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de gebeurtenis Hubs scenario's, gaat u naar deze koppelingen:

- [Wat is Azure gebeurtenis Hubs?](event-hubs-what-is-event-hubs.md)
- [Overzicht van de gebeurtenissen Hubs](event-hubs-overview.md)
- [Gebeurtenis Hubs programming guide](event-hubs-programming-guide.md)
- [Gebeurtenis Hubs codevoorbeelden](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

De .NET API-referenties zijn hier:

- [Verwijzingen naar de Bus-service en gebeurtenis Hubs .NET API](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Gebeurtenis processor host API: naslag](https://msdn.microsoft.com/library/azure/mt445521.aspx)
