## <a name="send-messages-to-event-hubs"></a>Verzenden van berichten met gebeurtenis-Hubs

De Java client library voor gebeurtenis Hubs beschikbaar is voor gebruik in Maven projecten uit de [Centrale opslagplaats voor Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)en met behulp van de volgende declaratie voor de afhankelijkheid in het projectbestand Maven kan worden verwezen:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Voor verschillende soorten build-omgeving, kunt u de laatste JAR bestanden expliciet uit de [Centrale opslagplaats voor Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) of [het distributiepunt release op GitHub](https://github.com/Azure/azure-event-hubs/releases)verkrijgen.  

Importeren voor een eenvoudige gebeurtenis uitgever, het *com.microsoft.azure.eventhubs* -pakket voor de gebeurtenis Hubs client klassen en de *com.microsoft.azure.servicebus* klasse hulpprogramma algemene uitzonderingen die worden gedeeld met de client voor Azure Service Bus. 

Voor in het volgende voorbeeld moet u eerst een nieuwe Maven project voor een console/shell-toepassing maken in uw favoriete Java development environment. De klasse worden aangeroepen ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

De naamruimte en de gebeurtenis Hub namen vervangen door de waarden die worden gebruikt bij het maken van de gebeurtenis Hub.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Maak vervolgens een enkelvoud gebeurtenis door een tekenreeks kunt omtoveren in de codering UTF-8-byte. We vervolgens een nieuw exemplaar van de gebeurtenis Hubs-client uit de verbindingsreeks maken en verzenden van het bericht.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
