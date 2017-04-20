<properties
    pageTitle="Wolk naar apparaat verzenden met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie voor meer informatie over het cloud-apparaat-berichten verzenden met Azure IoT Hub met Java."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Zelfstudie: Het verzenden van berichten met IoT Hub en Java wolk naar apparaat

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige bi-directionele communicatie tussen miljoenen IoT-apparaten inschakelen en weer een toepassing te beëindigen. De zelfstudie [aan de slag met IoT Hub] laat zien hoe een hub IoT maken, de identiteit van een apparaat in het inrichten en code een gesimuleerde apparaat dat apparaat-wolk-berichten verzonden.

Deze zelfstudie is gebaseerd op het [aan de slag met IoT Hub]. U ziet hoe aan:

- Vanuit uw toepassing cloud back-end wolk naar apparaat berichten naar een enkel apparaat via de IoT Hub te verzenden.
- Cloud-to-device-berichten op een apparaat ontvangen.
- Back-end uit de cloud toepassing, bevestiging van levering (*feedback*) aanvragen voor berichten die worden verzonden naar een apparaat van IoT Hub.

U vindt meer informatie over berichten wolk naar apparaat in de [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

Aan het einde van deze zelfstudie kunt u twee toepassingen van Java console uitvoeren:

* **gesimuleerde apparaat**, een gewijzigde versie van de app in [aan de slag met IoT Hub], die verbinding maakt met uw hub IoT en ontvangen van berichten van de wolk naar apparaat gemaakt.
* **verzenden-c2d-berichten**, die een wolk naar apparaat-bericht verzendt naar het gesimuleerde apparaat via IoT Hub en vervolgens de bevestiging van de levering ontvangt.

> [AZURE.NOTE] IoT Hub heeft ondersteuning voor veel apparaat platforms en talen (zoals C, Java en Javascript) via Azure IoT apparaat SDK's SDK. Zie voor stapsgewijze instructies voor het aansluiten van uw apparaat code van deze zelfstudie, en in het algemeen Azure IoT Hub, [Azure IoT Developer Center].

Deze zelfstudie hebt u het volgende nodig:

+ Java SE 8. <br/> [Bereid uw ontwikkelomgeving] [ lnk-dev-setup] wordt beschreven hoe u Java voor deze zelfstudie te installeren op Windows of Linux.

+ 3 maven.  <br/> [Bereid uw ontwikkelomgeving] [ lnk-dev-setup] wordt beschreven hoe u Maven voor deze zelfstudie te installeren op Windows of Linux.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Ontvangen van berichten op het gesimuleerde apparaat.

In deze sectie kunt u de toepassing van het gesimuleerde apparaat wijzigt u in [aan de slag met de IoT Hub] cloud naar apparaat om berichten te ontvangen van de hub IoT gemaakt.

1. Open het bestand simulated-device\src\main\java\com\mycompany\app\App.java in een teksteditor.

2. De volgende **MessageCallback** -klasse als een geneste klasse binnen de klasse **App** toevoegen. De methode **execute** wordt aangeroepen wanneer het apparaat een bericht van IoT Hub ontvangt. In dit voorbeeld wordt het apparaat altijd wordt gewaarschuwd de hub dat het bericht is voltooid:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. De **belangrijkste** methode om een exemplaar van **MessageCallback** en de methode **setMessageCallback** aanroepen voordat het openen van de client als volgt wijzigen:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Als u HTTP in plaats van MQTT of AMQP als het transport gebruikt, de instantie **DeviceClient** berichten van IoT Hub zelden (minder dan 25 minuten elke) gecontroleerd. Zie voor meer informatie over de verschillen tussen MQTT, AMQP en HTTP-ondersteuning en het beperken van IoT Hub [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Een wolk naar apparaat verzenden.

In dit gedeelte maakt u een Java console app die cloud-to-device-berichten worden verzonden naar de toepassing van het gesimuleerde apparaat. U moet het apparaat-Id van het apparaat dat u hebt toegevoegd in de zelfstudie [aan de slag met IoT Hub] . U moet ook de verbindingsreeks voor de IoT-hub die u in de [portal Azure vinden kunt].

1. Maak een Maven project genaamd **c2d-berichten-verzenden** met de volgende opdracht bij de opdrachtprompt. Opmerking dat dit is een enkele, lange-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Ga naar de nieuwe map verzenden-c2d-berichten bij de opdrachtprompt.

3. Met behulp van een teksteditor, open het pom.xml bestand in de map verzenden-c2d-berichten en de volgende afhankelijkheid toevoegen aan het knooppunt **afhankelijkheden** . De afhankelijkheid toevoegt, kunt u het pakket **iothub-java-service-client** in uw toepassing gebruiken om te communiceren met uw service IoT hub:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Opslaan en sluit het bestand pom.xml.

5. Open het bestand send-c2d-messages\src\main\java\com\mycompany\app\App.java in een teksteditor.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. De volgende klasse niveau variabelen toevoegen aan de klasse vervangen door **{yourhubconnectionstring}** en **{yourdeviceid}** de waarden vermeld uw eerder **App** :

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. De **belangrijkste** methode vervangen door de volgende code die is verbonden met de hub IoT en wacht op een bevestiging dat het apparaat ontvangen en het bericht verwerkt verzendt een bericht naar uw apparaat:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Deze zelfstudie implementeert voor eenvoud van mogelijk te houden, niet elk beleid opnieuw. In de productiecode te opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]nemen.

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Bij een opdrachtprompt in de map van het gesimuleerde apparaat, moet u de volgende opdracht te beginnen met het verzenden van telemetrie op uw hub IoT en om te luisteren naar de cloud naar apparaat worden verzonden vanaf de hub uitvoeren:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![De toepassing van het gesimuleerde apparaat uitvoeren][img-simulated-device]

2. Bij een opdrachtprompt in de map verzenden-c2d-berichten, voer de volgende opdracht om een wolk naar apparaat verzenden en wachten op een bevestiging van de feedback:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Voer de opdracht voor het verzenden van de wolk naar apparaat][img-send-command]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe cloud-to-device-berichten verzenden en ontvangen. 

Voorbeelden van volledige end-to-end oplossingen met IoT Hub, Zie [Azure IoT Suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, [IoT Hub Developer Guide].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Aan de slag met IoT Hub]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT Hub Developer Guide]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Fout met betrekking tot tijdelijke behandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/