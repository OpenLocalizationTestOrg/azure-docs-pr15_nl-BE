<properties
    pageTitle="Wolk naar apparaat verzenden met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie voor meer informatie over het cloud-apparaat-berichten verzenden met Azure IoT Hub met C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Zelfstudie: Het verzenden van berichten met IoT Hub en .net wolk naar apparaat

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige bi-directionele communicatie tussen miljoenen IoT-apparaten inschakelen en weer een toepassing te beëindigen. De zelfstudie [aan de slag met IoT Hub] laat zien hoe een hub IoT maken, de identiteit van een apparaat in het inrichten en code een gesimuleerde apparaat dat apparaat-wolk-berichten verzonden.

Deze zelfstudie is gebaseerd op het [aan de slag met IoT Hub]. U ziet hoe aan:

- Vanuit uw toepassing cloud back-end wolk naar apparaat berichten naar een enkel apparaat via de IoT Hub te verzenden.
- Cloud-to-device-berichten op een apparaat ontvangen.
- Back-end uit de cloud toepassing, bevestiging van levering (*feedback*) aanvragen voor berichten die worden verzonden naar een apparaat van IoT Hub.

U vindt meer informatie over berichten wolk naar apparaat in de [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

Aan het einde van deze zelfstudie kunt u twee Windows consoletoepassingen wilt uitvoeren:

* **SimulatedDevice**, een gewijzigde versie van de app in [aan de slag met IoT Hub], die verbinding maakt met uw hub IoT en ontvangen van berichten van de wolk naar apparaat gemaakt.
* **SendCloudToDevice**, die een wolk naar apparaat-bericht verzendt naar het gesimuleerde apparaat via IoT Hub en vervolgens de bevestiging van de levering ontvangt.

> [AZURE.NOTE] IoT Hub heeft ondersteuning voor veel apparaat platforms en talen (zoals C, Java en Javascript) via Azure IoT apparaat SDK's SDK. Zie voor stapsgewijze instructies voor het aansluiten van uw apparaat code van deze zelfstudie, en in het algemeen Azure IoT Hub, [Azure IoT Developer Center].

Deze zelfstudie hebt u het volgende nodig:

+ Microsoft Visual Studio 2015

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Ontvangen van berichten op het gesimuleerde apparaat.

In deze sectie, zult u de toepassing van het gesimuleerde apparaat wijzigt u in [aan de slag met de IoT Hub] cloud naar apparaat om berichten te ontvangen van de hub IoT gemaakt.

1. Toevoegen de volgende methode aan de klasse **wordt** in het **SimulatedDevice** -project in Visual Studio.

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    De `ReceiveAsync` methode asynchroon retourneert van het ontvangen bericht op het moment dat deze is ontvangen door het apparaat. Het resultaat *leeg* na een specifiable time-out-periode (in dit geval wordt de standaardwaarde van één minuut wordt gebruikt). Als dit gebeurt, moet de code wachten op nieuwe berichten. Dit is de reden voor de `if (receivedMessage == null) continue` lijn.

    De aanroep van `CompleteAsync()` IoT Hub wordt gewaarschuwd dat het bericht is verwerkt. Het bericht kan veilig worden verwijderd uit de apparaatwachtrij. Als er iets is gebeurd waardoor de app van het apparaat van de voltooiing van de verwerking van het bericht, bezorgd IoT Hub opnieuw. Het is vervolgens belangrijk dat bericht verwerking logica in het apparaat app *idempotency is ingeschakeld*, zodat hetzelfde bericht meerdere malen ontvangen hetzelfde resultaat geeft. Een toepassing kan ook tijdelijk verlaten van een bericht, wat leidt tot behoud van het bericht in de wachtrij voor toekomstige consumptie IoT-hub. Of de toepassing een bericht waarmee het bericht permanent verwijderd uit de wachtrij kunt weigeren. Zie voor meer informatie over de levenscyclus van de wolk naar apparaat bericht [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Als u HTTP gebruikt in plaats van MQTT of AMQP als een transport, de `ReceiveAsync` methode onmiddellijk geretourneerd. De ondersteunde patroon voor berichten met HTTP wolk naar apparaat is tijdelijk aangesloten apparaten controleren voor berichten niet vaak (minder dan elke 25 minuten). Afgifte van meer HTTP, ontvangt resultaten in IoT Hub de aanvragen te beperken. Zie voor meer informatie over de verschillen tussen MQTT, AMQP en HTTP-ondersteuning en het beperken van IoT Hub [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

2. De volgende methode in de methode **Main** toevoegen na de `Console.ReadLine()` regel:

        ReceiveC2dAsync();

> [AZURE.NOTE] Deze zelfstudie implementeert voor eenvoud van mogelijk te houden, niet elk beleid opnieuw. In de productiecode te opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]nemen.

## <a name="send-a-cloud-to-device-message"></a>Een wolk naar apparaat verzenden.

In deze sectie, zult u een Windows-console app die cloud-to-device-berichten worden verzonden naar de toepassing van het gesimuleerde apparaat.

1. Maak een nieuw project van Visual C# Desktop App met behulp van de **Console Application** projectsjabloon in de huidige Visual Studio-oplossing. Naam van het project **SendCloudToDevice**.

    ![Nieuw project in Visual Studio][20]

2. Klik met de rechtermuisknop op de oplossing in de Solution Explorer en klik op **Beheren NuGet pakketten voor oplossing...**. 

    Hiermee opent u het venster **NuGet pakketten beheren** .

3. Zoeken naar `Microsoft Azure Devices`en klik op **installeren**, accepteer de gebruiksvoorwaarden. 

    Dit downloadt, installeert en een verwijzing toegevoegd naar de [Azure IoT - Service SDK NuGet package].

4. Voeg de volgende `using` instructie aan het begin van het bestand **Program.cs** :

        using Microsoft.Azure.Devices;

5. De volgende velden toevoegen aan de klasse van het **programma** . Vervang de aanduidingswaarde van de tijdelijke met de verbindingsreeks IoT-hub uit [aan de slag met IoT Hub]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. De volgende methode toevoegen aan de klasse van het **programma** :

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Deze methode verzendt een nieuwe cloud-to-device-bericht naar het apparaat met de ID, `myFirstDevice`. Wijzig deze parameter derhalve in het geval deze dan in [aan de slag met IoT Hub]is gewijzigd.

7. Ten slotte, voeg de volgende regels aan de **Main** -methode:

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Binnen Visual Studio, met de rechtermuisknop op uw oplossing en **projecten opstarten instellen...**te selecteren. Selecteer **meerdere projecten voor opstarten**en vervolgens de actie **Start** voor **ProcessDeviceToCloudMessages**, **SimulatedDevice**en **SendCloudToDevice**.

9.  Druk op **F5**. Alle drie de toepassingen te starten. Selecteer de **SendCloudToDevice** van windows en druk op **Enter**. Hier ziet u het bericht dat wordt ontvangen door de gesimuleerde app.

    ![Ontvangen bericht App][21]

## <a name="receive-delivery-feedback"></a>Levering feedback ontvangen
Het is mogelijk op aanvraag levering (of verlopen) bevestigingen van IoT Hub voor elk bericht wolk naar apparaat. Hierdoor kunnen de cloud back-end in kennis te stellen gemakkelijk opnieuw of compensatie logica. Zie de [IoT Hub Developer Guide]voor meer informatie over feedback wolk naar apparaat[IoT Hub Developer Guide - C2D].

In dit gedeelte wijzigt u de **SendCloudToDevice** app ontvangen IoT Hub en vraag om feedback.

1. Toevoegen de volgende methode aan de klasse **wordt** in het **SendCloudToDevice** -project in Visual Studio.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Houd er rekening mee dat het patroon ontvangen dezelfde is als wolk naar apparaat om berichten te ontvangen van de app van het apparaat wordt gebruikt.

2. Voegt u de volgende methode in de methode **Main** direct na de `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` regel:

        ReceiveFeedbackAsync();

3. Als u feedback voor de levering van uw bericht wolk naar apparaat, moet u een eigenschap opgeeft in de **SendCloudToDeviceMessageAsync** -methode. Voeg de volgende regel, direct na de `var commandMessage = new Message(...);` regel:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  De toepassingen worden uitgevoerd door op **F5**te drukken. Hier ziet u alle drie de toepassingen starten. Selecteer de **SendCloudToDevice** van windows en druk op **Enter**. Hier ziet u het bericht wordt ontvangen door de gesimuleerde app en na een paar seconden het Feedbackbericht wordt ontvangen door de toepassing van de **SendCloudToDevice** .

    ![Ontvangen bericht App][22]

> [AZURE.NOTE] Deze zelfstudie implementeert voor eenvoud van mogelijk te houden, niet elk beleid opnieuw. In de productiecode te opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]nemen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe cloud-to-device-berichten verzenden en ontvangen. 

Voorbeelden van volledige end-to-end oplossingen met IoT Hub, Zie [Azure IoT Suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, [IoT Hub Developer Guide].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - Service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Fout met betrekking tot tijdelijke behandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[IoT Hub Developer Guide]: iot-hub-devguide.md
[Aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/