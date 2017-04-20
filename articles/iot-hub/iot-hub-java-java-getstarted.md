<properties
    pageTitle="Azure IoT Hub voor Java, aan de slag | Microsoft Azure"
    description="Azure IoT Hub met Java aan zelfstudie is gestart. Azure IoT Hub en Java gebruiken met de Microsoft Azure IoT SDK's voor het implementeren van een oplossing voor Internet van dingen."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Aan de slag met Azure IoT Hub voor Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het einde van deze zelfstudie hebt u drie toepassingen van Java console:

* **Maak apparaat identiteit**maakt een apparaat-id en de bijbehorende sleutel verbinding maken met het gesimuleerde apparaat.
* **lezen-d2c-berichten**, waarin de telemetrie verzonden door het gesimuleerde apparaat.
* **gesimuleerde apparaat**verbonden met de hub IoT met de identiteit van het apparaat eerder hebt gemaakt en een bericht telemetrie elke tweede met behulp van het protocol van AMQP.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de diverse SDK's waarmee u kunt beide toepassingen uit te voeren op apparaten en back-end van uw oplossing te bouwen.

Deze zelfstudie hebt u het volgende nodig:

+ Java SE 8. <br/> [Bereid uw ontwikkelomgeving] [ lnk-dev-setup] wordt beschreven hoe u Java voor deze zelfstudie te installeren op Windows of Linux.

+ 3 maven.  <br/> [Bereid uw ontwikkelomgeving] [ lnk-dev-setup] wordt beschreven hoe u Maven voor deze zelfstudie te installeren op Windows of Linux.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Een laatste stap Noteer de waarde van de **primaire sleutel** en klik vervolgens op **berichten**. Maak een notitie van de **naam van de gebeurtenis Hub-compatibel** en het **eindpunt van de gebeurtenis Hub-compatibele**op het blad **Messaging** . Deze drie waarden moet u bij het maken van uw toepassing **lezen-d2c-berichten** .

![Azure portal IoT Hub Messaging blade][6]

U hebt nu uw hub IoT gemaakt en u hebt de hostnaam IoT Hub, IoT Hub verbindingsreeks IoT Hub primaire sleutel, naam gebeurtenis Hub-compatibel en gebeurtenis Hub-compatibele eindpunt moet u deze zelfstudie.

## <a name="create-a-device-identity"></a>Een apparaat-id maken

In dit gedeelte maakt u een Java console app waarmee de identiteit van een nieuw apparaat in het register van de identiteit in de hub IoT. Een apparaat kan geen verbinding maken met IoT hub tenzij er een vermelding in het register van de identiteit apparaat. Raadpleeg de sectie **Apparaat identiteit register** van [IoT Hub Developer Guide] [ lnk-devguide-identity] voor meer informatie. Wanneer u deze consoletoepassing uitvoert, wordt een uniek apparaat-ID en de sleutel die het apparaat gebruiken kunt om zichzelf te identificeren bij het apparaat naar cloud-berichten met IoT Hub verzenden gegenereerd.

1. Maak een nieuwe lege map met de naam iot-java-get-gestart. Maak een nieuw Maven project **maken-apparaat-id** met de volgende opdracht bij de opdrachtprompt de naam in de map iot-java-get-gestart. Opmerking dat dit is een enkele, lange-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Ga bij de opdrachtprompt naar de nieuwe map in de maken-apparaat-id.

3. Met behulp van een teksteditor, open het pom.xml bestand in de map maken-apparaat-id en de volgende afhankelijkheid toevoegen aan het knooppunt **afhankelijkheden** . Hiermee kunt u het pakket iothub-service-sdk gebruiken in uw toepassing:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Opslaan en sluit het bestand pom.xml.

5. Open het bestand create-device-identity\src\main\java\com\mycompany\app\App.java in een teksteditor.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. De volgende klasse niveau variabelen toevoegen aan de klasse vervangen door **{yourhubconnectionstring}** de waarde vermeld uw eerder **App** :

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. De handtekening van de **belangrijkste** methode om op te nemen van de uitzonderingen als volgt wijzigen:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Voeg de volgende code op als de hoofdtekst van de **belangrijkste** methode. Deze code maakt u een zogeheten *javadevice* in het register van de identiteit IoT Hub als nog niet bestaat. Vervolgens wordt weergegeven de apparaat-id en de sleutel die u later nodig hebt:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Opslaan en sluiten van het bestand App.java.

11. Als u de toepassing **maken-apparaat-id** is met behulp van Maven, uitvoeren met de volgende opdracht bij de opdrachtprompt in de map maken-apparaat-id:

    ```
    mvn clean package -DskipTests
    ```

12. De **create-apparaat-id** als toepassing wilt uitvoeren met behulp van Maven, voeren de volgende opdracht bij de opdrachtprompt in de map maken-apparaat-id:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Maak een notitie van de **apparaat-id** en **sleutel apparaat**. U moet deze later wanneer u een toepassing die is verbonden met de IoT Hub als een apparaat maakt.

> [AZURE.NOTE] Het register van de identiteit IoT Hub slaat alleen apparaat-id's voor beveiligde toegang tot de hub. Toetsen wilt gebruiken als gebruikersnaam en een vlag ingeschakeld/uitgeschakeld, kunt u de toegang voor een bepaald apparaat uitschakelen en apparaat-id's worden opgeslagen. Als uw toepassing nodig heeft voor het andere apparaat-specifieke metagegevens opslaan, moet er een specifieke winkel gebruiken. Zie handleiding voor [ontwikkelaars] van IoT Hub[ lnk-devguide-identity] voor meer informatie.

## <a name="receive-device-to-cloud-messages"></a>Apparaat-wolk-berichten ontvangen

In dit gedeelte maakt u een Java console app die van IoT Hub apparaat-wolk-berichten. Een hub IoT beschrijft een [Gebeurtenis Hub][lnk-event-hubs-overview]-compatibel eindpunt zodat u kunt apparaat-wolk-berichten lezen. Om dingen eenvoudig te houden, maakt deze zelfstudie basic lezer is niet geschikt voor de implementatie van een hoge gegevensdoorvoer. Het [apparaat naar cloud-berichten verwerken] [ lnk-process-d2c-tutorial] handleiding hoe u apparaat-wolk-berichten op een schaal te verwerken. Het is [Aan de slag met Hubs gebeurtenis] [ lnk-eventhubs-tutorial] zelfstudie biedt meer informatie over het op berichten van gebeurtenis Hubs en is van toepassing op de eindpunten IoT Hub gebeurtenis Hub-compatibel.

> [AZURE.NOTE] De gebeurtenis Hub-compatibele eindpunt voor het lezen van apparaat-wolk-berichten altijd het AMQP-protocol wordt gebruikt.

1. In de map iot java-get-started die u hebt gemaakt in de sectie *identiteit van een apparaat maken* , maakt u een nieuw Maven project genoemd **gelezen-d2c-berichten** met de volgende opdracht bij de opdrachtprompt. Opmerking dat dit is een enkele, lange-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Ga naar de nieuwe map lezen-d2c-berichten bij de opdrachtprompt.

3. Met behulp van een teksteditor, open het pom.xml bestand in de map lezen-d2c-berichten en de volgende afhankelijkheid toevoegen aan het knooppunt **afhankelijkheden** . Hiermee kunt u het pakket eventhubs-client gebruiken in uw toepassing worden gelezen vanuit het eindpunt van de gebeurtenis Hub-compatibel:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Opslaan en sluit het bestand pom.xml.

5. Open het bestand read-d2c-messages\src\main\java\com\mycompany\app\App.java in een teksteditor.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. De volgende klasse niveau variabelen toevoegen aan de klasse **App** . **{Youriothubkey}**, **{youreventhubcompatibleendpoint}**en **{youreventhubcompatiblename}** vervangen door de waarden die u eerder hebt genoteerd:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. De volgende **receiveMessages** methode toevoegen aan de klasse **App** . Deze methode maakt een instantie van de **EventHubClient** verbinding maken met het eindpunt van de gebeurtenis Hub-compatibel en maakt u een exemplaar van de **PartitionReceiver** te lezen van een partitie gebeurtenis Hub asynchroon. Het voortdurend een lus en wordt de berichtgegevens afgedrukt totdat de toepassing wordt beëindigd.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Deze methode wordt een filter wanneer de ontvanger wordt gemaakt zodat de ontvanger wordt alleen berichten worden verzonden naar de IoT gelezen Hub nadat de ontvanger is gestart. Dit is handig in een testomgeving, zodat u de huidige set berichten kunt zien. In een productieomgeving uw code moet ervoor zorgen dat deze verwerkt alle berichten - Zie de [IoT Hub apparaat naar cloud berichten verwerken] [ lnk-process-d2c-tutorial] zelfstudie voor meer informatie.

9. De handtekening van de **belangrijkste** methode voor het opnemen van de uitzondering als volgt wijzigen:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. De volgende code toevoegen aan de **belangrijkste** methode in de klasse **App** . Deze code maakt de twee exemplaren van de **EventHubClient** en **PartitionReceiver** en kunt u de toepassing sluiten wanneer u klaar bent met het verwerken van berichten:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Deze code wordt ervan uitgegaan dat u uw hub IoT gemaakt in de F1 (gratis) laag. Een gratis IoT hub heeft twee partities met de naam '0' en '1'.

11. Opslaan en sluiten van het bestand App.java.

12. Wilt maken in de **lezen-d2c-berichten** -toepassing met behulp van Maven, voeren de volgende opdracht bij de opdrachtprompt in de map lezen-d2c-berichten:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaat app maken

In dit gedeelte maakt u een Java console app die overeenkomt met een apparaat dat een hub IoT apparaat-wolk-berichten verstuurt.

1. In de map iot java-get-started die u hebt gemaakt in de sectie *identiteit van een apparaat maken* , maakt u een nieuwe Maven project met de naam van de **gesimuleerde apparaat** met de volgende opdracht bij de opdrachtprompt. Opmerking dat dit is een enkele, lange-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Ga bij de opdrachtprompt naar de nieuwe map met gesimuleerde apparaat.

3. Met behulp van een teksteditor, open het pom.xml bestand in de map van het gesimuleerde apparaat en de volgende afhankelijkheden toevoegen aan het knooppunt **afhankelijkheden** . Hiermee kunt u het pakket iothub-java-client in uw toepassing gebruiken om te communiceren met uw hub IoT en Java-objecten naar JSON serialisatie toepassen:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Opslaan en sluit het bestand pom.xml.

5. Open het bestand simulated-device\src\main\java\com\mycompany\app\App.java in een teksteditor.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. De volgende klasse niveau variabelen toevoegen aan de klasse **App** , **{youriothubname}** vervangen door uw IoT hub en **{yourdevicekey}** met de apparaat-sleutelwaarde die u hebt gegenereerd in de sectie *identiteit van een apparaat te maken* :

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Deze voorbeeldtoepassing wordt de variabele **protocol** gebruikt wanneer er een **DeviceClient** -object wordt. U kunt het HTTP- of AMQP protocol communiceren met IoT Hub.

8. Voeg dat de volgende geneste **TelemetryDataPoint** klasse binnen de klasse **App** uw apparaat naar uw hub IoT verzendt telemetriegegevens opgeven:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Voeg de volgende geneste **EventCallback** klasse binnen de **App** -klasse weergeven over de status van de bevestiging dat de IoT hub wordt geretourneerd wanneer een bericht van het gesimuleerde apparaat worden verwerkt. Deze methode ook de hoofdthread in de toepassing een melding wanneer het bericht is verwerkt:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. De volgende geneste **MessageSender** klasse binnen de klasse **App** toevoegen. De methode **run** in deze klasse telemetrie voorbeeldgegevens te verzenden naar uw hub IoT genereert en wacht op bevestiging voordat het volgende bericht verzonden:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Deze methode verzendt een nieuw apparaat op wolk bericht één seconde na de hub IoT het vorige bericht erkent. Het bericht bevat een JSON-geserialiseerd object met de deviceId en een willekeurig gegenereerd nummer voor het simuleren van een wind snelheid sensor.

11. De **belangrijkste** methode vervangen door de volgende code die u maakt een thread op apparaat naar cloud om berichten te verzenden naar uw IoT hub:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Opslaan en sluiten van het bestand App.java.

13. Voor het opbouwen van het **gesimuleerde apparaat** -toepassing met behulp van Maven voeren met de volgende opdracht bij de opdrachtprompt in de map van het gesimuleerde apparaat:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Deze zelfstudie implementeert om dingen eenvoudig te houden, niet elk beleid opnieuw. In de productiecode opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]te nemen[lnk-transient-faults].

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij een opdrachtprompt in de map lezen d2c, kunt u de volgende opdracht om te beginnen met het toezicht op de eerste partitie in de hub IoT uitvoeren:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub-clienttoepassing apparaat naar cloud berichten controleren][7]

2. Bij een opdrachtprompt in de map van het gesimuleerde apparaat, kunt u de volgende opdracht om te beginnen met telemetriegegevens worden verzonden naar uw hub IoT uitvoeren:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Hub voor Java IoT device-clienttoepassing apparaat-wolk-berichten verzenden][8]

3. De tegel voor **Gebruik** in de [Azure portal] [ lnk-portal] geeft het aantal berichten die naar de hub:

    ![Azure portal gebruik tegel met aantal berichten voor IoT Hub][43]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, een nieuwe IoT hub in de Azure portal worden geconfigureerd en wordt gemaakt van de identiteit van een apparaat in het register van de hub van de identiteit. U gebruikt deze apparaat-id voor het gesimuleerde apparaat app apparaat-wolk-berichten verzenden naar de hub. U ook een app die de berichten die zijn ontvangen door de hub wordt gemaakt. 

Om door te gaan aan de slag met IoT Hub en verkennen Zie andere IoT scenario's:

- [Aansluiten van een apparaat][lnk-connect-device]
- [Aan de slag met Apparaatbeheer][lnk-device-management]
- [Aan de slag met de SDK IoT Gateway][lnk-gateway-SDK]

Zie informatie over het uitbreiden van uw oplossing IoT en apparaat naar cloud-berichten op een schaal, de [berichten verwerken-apparaat naar cloud] [ lnk-process-d2c-tutorial] zelfstudie.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/