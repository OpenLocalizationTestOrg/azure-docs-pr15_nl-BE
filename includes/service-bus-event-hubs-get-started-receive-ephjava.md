## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Ontvangen berichten met EventProcessorHost in Java

EventProcessorHost is een Javaklasse die ontvangende gebeurtenissen van Hubs gebeurtenis door beheer permanente controlepunten vereenvoudigt en parallelle ontvangt van deze gebeurtenis Hubs. Met behulp van EventProcessorHost kunt u splitsen gebeurtenissen via meerdere ontvangers, zelfs wanneer deze wordt gehost in verschillende knooppunten. In dit voorbeeld ziet hoe u EventProcessorHost voor een enkele ontvanger.

###<a name="create-a-storage-account"></a>Maak een account voor opslag

EventProcessorHost gebruiken, hebt u een [account Azure opslag][]:

1. Meld u aan bij de [Azure klassieke portal][]en klik op **Nieuw** onder aan het scherm.

2. Op **Data Services**, **opslag**, vervolgens de **Snel maken**en typ vervolgens een naam voor de account van uw opslag. Selecteer de gewenste regio en klik vervolgens op **Opslag Account maken**.

    ![][11]

3. Klik op de zojuist gemaakte opslag-account en klik vervolgens op **Access-sleutels beheren**:

    ![][12]

    De primaire toegangstoets gebruiken verderop in deze zelfstudie kopiëren.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Maak een Java-project met behulp van de EventProcessor Host

De Java-client library voor gebeurtenis Hubs beschikbaar is voor gebruik in Maven projecten uit de [Centrale opslagplaats voor Maven][Maven Package], en met de volgende declaratie voor de afhankelijkheid in het projectbestand Maven kan worden verwezen:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Voor verschillende soorten build-omgeving, kunt u expliciet de laatste JAR bestanden ophalen uit de [Centrale opslagplaats voor Maven] [ Maven Package] of van [het distributiepunt release op GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Voor in het volgende voorbeeld moet u eerst een nieuwe Maven project voor een console/shell-toepassing maken in uw favoriete Java development environment. De klasse worden aangeroepen ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. De volgende code gebruiken voor het maken van een nieuwe klasse met de naam ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Maken van een definitieve klasse genoemd ```EventProcessorSample```, met de volgende code.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. De volgende velden vervangen door de waarden die worden gebruikt bij het maken van de gebeurtenis Hub en opslag.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] In deze zelfstudie wordt één exemplaar van EventProcessorHost. U verhoogt de doorvoer door verdient het uitvoeren van meerdere exemplaren van de EventProcessorHost. In dat geval coördineren de verschillende exemplaren automatisch met elkaar te verdelen de gebeurtenissen ontvangen. Als u meerdere ontvangers voor elk proces *alle* gebeurtenissen wilt, moet u het concept **ConsumerGroup** . Bij het ontvangen van gebeurtenissen uit verschillende machines, kan het zijn handig om te namen opgeven voor EventProcessorHost exemplaren op basis van de machines (of rollen) in waarin ze worden geïmplementeerd.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Azure opslag account]: ../articles/storage/storage-create-storage-account.md
[Azure klassieke portal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

