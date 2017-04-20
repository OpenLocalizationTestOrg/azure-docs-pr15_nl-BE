## <a name="receive-messages-with-eventprocessorhost"></a>Ontvangen berichten met EventProcessorHost

[EventProcessorHost][] is een .NET-klasse die ontvangende gebeurtenissen van Hubs gebeurtenis door beheer permanente controlepunten vereenvoudigt en parallelle ontvangt van deze gebeurtenis Hubs. Met behulp van [EventProcessorHost][]kunt u splitsen gebeurtenissen via meerdere ontvangers, zelfs wanneer deze wordt gehost in verschillende knooppunten. In dit voorbeeld ziet hoe u [EventProcessorHost][] voor een enkele ontvanger. De [schaal van de gebeurtenis verwerken][] -voorbeeld ziet u hoe [EventProcessorHost][] gebruiken met meerdere ontvangers.

Voor het gebruik van [EventProcessorHost][], hebt u een [account Azure opslag][]:

1. Meld u aan bij de [Azure portal][]en klikt u op **Nieuw** op de bovenkant van het scherm links.

2. **Gegevens- en**Klik op **account voor opslag**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Typ een naam voor de account van de opslag in de blade **opslag-account maken** . Kies een abonnement Azure, resourcegroep en locatie in voor het maken van de resource. Klik vervolgens op **maken**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Klik in de lijst met accounts voor opslag, de gemaakte opslag account.

5. Klik op de **toegangstoetsen**in het blad opslag-account. Kopieer de waarde van **key1** verderop in deze zelfstudie gebruiken.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. Maak een nieuw Visual C# Desktop-App-project met behulp van de projectsjabloon **Consoletoepassing** in Visual Studio. Naam van de **ontvanger**van het project.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. Met de rechtermuisknop op de oplossing in de Solution Explorer en klik op **Beheren NuGet pakketten voor oplossing**.

6. Klik op het tabblad **Bladeren** en zoeken naar `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Zorg ervoor dat de naam van het project (**ontvanger**) is opgegeven in het vak **versie (s)** . Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio downloadt, installeert en een verwijzing toegevoegd naar de [Azure Service Bus gebeurtenis Hub - EventProcessorHost NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), met alle bijbehorende afhankelijkheden.

7. Klik met de rechtermuisknop op het project van de **ontvanger** , klikt u op **toevoegen**en klik op **klasse**. Naam van de nieuwe klasse **SimpleEventProcessor**en klik vervolgens op **toevoegen** om de klasse te maken.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. De volgende instructies toevoegen aan het begin van het bestand SimpleEventProcessor.cs:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Vervolgens vervangen door de volgende code voor de hoofdtekst van de klasse:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Deze klasse wordt aangeroepen door de **EventProcessorHost** om gebeurtenissen te verwerken van de gebeurtenis Hub wordt ontvangen. Houd er rekening mee dat de `SimpleEventProcessor` klasse een stopwatch gebruikt regelmatig het checkpoint-methode aanroepen van de context van de **EventProcessorHost** . Dit zorgt ervoor dat, als de ontvanger opnieuw wordt opgestart, gaan, niet meer dan vijf minuten verloren van het werk wordt verwerkt.

9. Voeg de volgende in de klasse **programma** `using` instructie aan het begin van het bestand:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Vervang vervolgens de `Main` -methode in de `Program` klasse met de volgende code wordt vervangen door de naam van de gebeurtenis Hub en de naamruimte niveau verbindingsreeks die u eerder hebt opgeslagen en de opslag-account en de sleutel die u hebt gekopieerd in de vorige gedeelten. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] In deze zelfstudie wordt één exemplaar van [EventProcessorHost][]. U verhoogt de doorvoer door verdient het uitvoeren van meerdere exemplaren van [EventProcessorHost][], zoals in het voorbeeld van de [schaal van de verwerking van de gebeurtenis][] . In dat geval coördineren de verschillende exemplaren automatisch onderling te verdelen de gebeurtenissen ontvangen. Als u meerdere ontvangers voor elk proces *alle* gebeurtenissen wilt, moet u het concept **ConsumerGroup** . Bij het ontvangen van gebeurtenissen uit verschillende machines, kan het zijn handig om te namen opgeven voor [EventProcessorHost][] exemplaren op basis van de machines (of rollen) in waarin ze worden geïmplementeerd. Zie de onderwerpen over [Hubs overzicht van de gebeurtenissen][] en [Gebeurtenis Hubs Programming Guide][] voor meer informatie over deze onderwerpen.

<!-- Links -->
[Overzicht van de gebeurtenissen Hubs]: ../articles/event-hubs/event-hubs-overview.md
[Gebeurtenis Hubs Programming Guide]: ../articles/event-hubs/event-hubs-programming-guide.md
[Verwerking van de gebeurtenis schaalvergroting]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure opslag account]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure portal]: https://portal.azure.com