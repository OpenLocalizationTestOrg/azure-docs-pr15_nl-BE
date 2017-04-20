<properties
    pageTitle="Azure IoT Hub voor C# aan de slag | Microsoft Azure"
    description="Zelfstudie Azure IoT Hub met C# aan de slag. Azure IoT Hub en C# gebruiken met de Microsoft Azure IoT SDK's voor het implementeren van een oplossing voor Internet van dingen."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Aan de slag met Azure IoT Hub voor .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het einde van deze zelfstudie hebt u drie console Windows-toepassingen:

* **CreateDeviceIdentity**, die een apparaat identiteit en de bijbehorende beveiligingssleutel het gesimuleerde apparaat verbinding maakt.
* **ReadDeviceToCloudMessages**, waarin de telemetrie verzonden door het gesimuleerde apparaat.
* **SimulatedDevice**, die verbinding maakt met de IoT-hub met de identiteit van het apparaat eerder hebt gemaakt en een bericht telemetrie elke seconde met behulp van het protocol van AMQP.

> [AZURE.NOTE] Zie voor informatie over de diverse SDK's waarmee u kunt beide toepassingen uit te voeren op apparaten en uw oplossing voor back-end [IoT Hub SDK's][lnk-hub-sdks].

Deze zelfstudie hebt u het volgende nodig:

+ Microsoft Visual Studio 2015.

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw hub IoT gemaakt en u hebt de hostnaam en de verbindingstekenreeks die u nodig hebt voor de rest van deze zelfstudie.

## <a name="create-a-device-identity"></a>Een apparaat-id maken

In dit gedeelte maakt u een console-app voor Windows waarmee de identiteit van een apparaat in het register van de identiteit in de hub IoT. Een apparaat kan geen verbinding maken met IoT hub tenzij er een vermelding in het register van de identiteit apparaat. Zie de sectie 'Register apparaat identiteit' van [IoT Hub Developer Guide]voor meer informatie[lnk-devguide-identity]. Wanneer u deze console-toepassing uitvoert, wordt een uniek apparaat-ID en de sleutel die het apparaat gebruiken kunt om zichzelf te identificeren bij het apparaat naar cloud-berichten met IoT Hub verzenden gegenereerd.

1. Toevoegen een project van Visual C# klassiek Windows-bureaublad naar de huidige oplossing met behulp van de projectsjabloon **Consoletoepassing** in Visual Studio. Zorg ervoor dat de versie van .NET Framework 4.5.1 of hoger. Naam van het project **CreateDeviceIdentity**.

    ![Nieuw project van Visual C# Windows bureaublad klassieke stijl][10]

2. In de Solution Explorer met de rechtermuisknop op het **CreateDeviceIdentity** -project en klik vervolgens op **Beheren Nuget pakketten**.

3. In het venster **Nuget Package Manager** selecteert u **Bladeren**, zoeken naar **microsoft.azure.devices**Selecteer **installeren** voor de installatie van het pakket **Microsoft.Azure.Devices** en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en een verwijzing toegevoegd naar de [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget package en de bijbehorende afhankelijkheden.

    ![Venster Nuget Package Manager][11]

4. Voeg de volgende `using` instructies boven aan het bestand **Program.cs** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. De volgende velden toevoegen aan de klasse van het **programma** . Vervang de aanduidingswaarde van de tijdelijke door de verbindingsreeks voor de IoT-hub die u in de vorige sectie hebt gemaakt.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. De volgende methode toevoegen aan de klasse van het **programma** :

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Deze methode maakt de identiteit van een apparaat met ID **myFirstDevice**. (Als dat apparaat-ID al in het register bestaat, de code eenvoudig haalt de bestaande gegevens van een apparaat.) De app geeft de primaire sleutel voor die identiteit. U kunt deze sleutel in het gesimuleerde apparaat aansluit op uw hub IoT.

7. Ten slotte, voeg de volgende regels aan de **Main** -methode:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Deze toepassing wordt uitgevoerd en maak een notitie van de sleutel voor het apparaat.

    ![Apparaatsleutel gegenereerd door de toepassing][12]

> [AZURE.NOTE] Het register van de identiteit IoT Hub slaat alleen apparaat-id's voor beveiligde toegang tot de hub. Toetsen wilt gebruiken als gebruikersnaam en een vlag ingeschakeld/uitgeschakeld, kunt u de toegang voor een bepaald apparaat uitschakelen en apparaat-id's worden opgeslagen. Als uw toepassing nodig heeft voor het andere apparaat-specifieke metagegevens opslaan, moet er een specifieke winkel gebruiken. Zie [IoT Hub Developer Guide]voor meer informatie[lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Apparaat-wolk-berichten ontvangen

In dit gedeelte maakt u een console-app voor Windows dat apparaat naar cloud berichten van IoT Hub leest. Een hub IoT beschrijft een [Azure gebeurtenis Hubs][lnk-event-hubs-overview]-compatibel eindpunt zodat u kunt apparaat-wolk-berichten lezen. Om dingen eenvoudig te houden, maakt deze zelfstudie basic lezer is niet geschikt voor de implementatie van een hoge gegevensdoorvoer. Zie meer informatie over het apparaat naar cloud-berichten op een schaal, het [apparaat naar cloud-berichten verwerken] [ lnk-process-d2c-tutorial] zelfstudie. Zie voor meer informatie over hoe u berichten verwerken van de gebeurtenis Hubs, de [Aan de slag met Hubs gebeurtenis] [ lnk-eventhubs-tutorial] zelfstudie. (Deze zelfstudie is van toepassing op de eindpunten IoT Hub gebeurtenis Hub-compatibel)

> [AZURE.NOTE] De gebeurtenis Hub-compatibele eindpunt voor het lezen van apparaat-wolk-berichten altijd het AMQP-protocol wordt gebruikt.

1. Toevoegen een project van Visual C# klassiek Windows-bureaublad naar de huidige oplossing met behulp van de projectsjabloon **Consoletoepassing** in Visual Studio. Zorg ervoor dat de versie van .NET Framework 4.5.1 of hoger. Naam van het project **ReadDeviceToCloudMessages**.

    ![Nieuw project van Visual C# Windows bureaublad klassieke stijl][10]

2. In de Solution Explorer met de rechtermuisknop op het **ReadDeviceToCloudMessages** -project en klik vervolgens op **Beheren Nuget pakketten**.

3. Zoeken naar **WindowsAzure.ServiceBus**in het venster **Nuget Package Manager** Selecteer **installeren**en accepteer de gebruiksvoorwaarden. Deze procedure gedownload, geïnstalleerd en een verwijzing toegevoegd naar [Azure Service Bus][lnk-servicebus-nuget], met alle bijbehorende afhankelijkheden. Dit pakket kan de toepassing verbinding maken met de gebeurtenis Hub-compatibele eindpunt op uw hub-IoT.

4. Voeg de volgende `using` instructies boven aan het bestand **Program.cs** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. De volgende velden toevoegen aan de klasse van het **programma** . Vervang de aanduidingswaarde van de tijdelijke door de verbindingsreeks voor de IoT-hub die u hebt gemaakt in de sectie 'Een hub IoT maken'.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. De volgende methode toevoegen aan de klasse van het **programma** :

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Deze methode wordt een exemplaar van de **EventHubReceiver** voor het ontvangen van berichten van alle de IoT hub apparaat-naar-wolk partities ontvangen. U ziet hoe het doorgeven een `DateTime.Now` parameter bij het maken van het **EventHubReceiver** -object, zodat alleen berichten die worden verzonden na het starten worden ontvangen. Dit filter is handig in een testomgeving, zodat u de huidige set berichten kunt zien. Uw code Zorg ervoor dat alle berichten worden verwerkt in een productieomgeving. Zie voor meer informatie de [IoT Hub apparaat naar cloud berichten verwerken] [ lnk-process-d2c-tutorial] zelfstudie.

7. Ten slotte, voeg de volgende regels aan de **Main** -methode:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaat app maken

In dit gedeelte maakt u een console-app voor Windows die een apparaat dat apparaat naar cloud berichten naar een hub IoT verzendt simuleert.

1. Toevoegen een project van Visual C# klassiek Windows-bureaublad naar de huidige oplossing met behulp van de projectsjabloon **Consoletoepassing** in Visual Studio. Zorg ervoor dat de versie van .NET Framework 4.5.1 of hoger. Naam van het project **SimulatedDevice**.

    ![Nieuw project van Visual C# Windows bureaublad klassieke stijl][10]

2. In de Solution Explorer met de rechtermuisknop op het **SimulatedDevice** -project en klik vervolgens op **Beheren Nuget pakketten**.

3. In het venster **Nuget Package Manager** selecteert u **Bladeren**, zoeken naar **Microsoft.Azure.Devices.Client**Selecteer **installeren** voor de installatie van het pakket **Microsoft.Azure.Devices.Client** en accepteer de gebruiksvoorwaarden. Deze procedure downloaden, installeren en een verwijzing toegevoegd naar de [Azure IoT - apparaat SDK Nuget package] [ lnk-device-nuget] en de bijbehorende afhankelijkheden.

4. Voeg de volgende `using` instructie aan het begin van het bestand **Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. De volgende velden toevoegen aan de klasse van het **programma** . Vult u de waarden van de tijdelijke aanduiding met de IoT hub hostnaam die u in de sectie 'Een hub IoT maken' opgehaald en de sleutel van het apparaat in de sectie 'Een apparaat-identiteit maken' opgehaald.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. De volgende methode toevoegen aan de klasse van het **programma** :

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Deze methode verzendt een nieuw apparaat op wolk bericht elke seconde. Het bericht bevat een JSON-geserialiseerd object, met de apparaat-ID en een willekeurig gegenereerd nummer voor het simuleren van een wind snelheid sensor.

7. Ten slotte, voeg de volgende regels aan de **Main** -methode:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  De methode **Create** maakt standaard een **DeviceClient** exemplaar dat de AMQP-protocol wordt gebruikt om te communiceren met IoT Hub. Als u het HTTP-protocol, gebruiken de overschrijving van de methode **Create** waarmee u kunt opgeven van het protocol. Als u het HTTP-protocol gebruikt, moet u het pakket **Microsoft.AspNet.WebApi.Client** Nuget ook toevoegen aan uw project op te nemen van de naamruimte **System.Net.Http.Formatting** .

Deze zelfstudie doorloopt u de stappen voor het maken van een IoT Hub device-client. Ook kunt u de [Service voor Azure IoT Hub aangesloten] [ lnk-connected-service] Visual Studio-extensie voor de benodigde code toevoegen aan uw device-clienttoepassing.


> [AZURE.NOTE] Deze zelfstudie implementeert om dingen eenvoudig te houden, niet elk beleid opnieuw. In de productiecode opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]te nemen[lnk-transient-faults].

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1.  Klik met de rechtermuisknop op uw oplossing in Visual Studio Solution Explorer en klik op **projecten opstarten ingesteld**. Selecteer **meerdere projecten voor opstarten**en selecteer **starten** als de actie voor de **ReadDeviceToCloudMessages** en de **SimulatedDevice** -projecten.

    ![Eigenschappen voor opstarten-project][41]

2.  Druk op **F5** om te starten beide apps uitgevoerd. De console-uitvoer van de app **SimulatedDevice** ziet u de berichten die het gesimuleerde apparaat naar uw hub IoT verzendt. De console-uitvoer van de **ReadDeviceToCloudMessages** -app toont de berichten die u ontvangt van uw hub-IoT.

    ![Console-uitvoer van apps][42]

3. De tegel voor **Gebruik** in de [Azure portal] [ lnk-portal] geeft het aantal berichten die naar de hub:

    ![Azure portal gebruik tegel][43]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie een hub IoT geconfigureerd in Azure portal en de identiteit van een apparaat wordt gemaakt in het register van de hub van de identiteit. U gebruikt deze apparaat-id voor het gesimuleerde apparaat app apparaat-wolk-berichten verzenden naar de hub. U ook een app die de berichten die zijn ontvangen door de hub wordt gemaakt. 

Zie om door te gaan aan de slag met IoT-Hub en andere IoT scenario's verkennen:

- [Aansluiten van een apparaat][lnk-connect-device]
- [Aan de slag met Apparaatbeheer][lnk-device-management]
- [Aan de slag met de SDK IoT Gateway][lnk-gateway-SDK]

Zie informatie over het uitbreiden van uw oplossing IoT en apparaat naar cloud-berichten op een schaal, de [berichten verwerken-apparaat naar cloud] [ lnk-process-d2c-tutorial] zelfstudie.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
