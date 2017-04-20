## <a name="send-messages-to-event-hubs"></a>Verzenden van berichten met gebeurtenis-Hubs

In deze sectie, zult u een Windows-console app waarmee gebeurtenissen worden verzonden naar uw Hub-gebeurtenis.

1. Maak een nieuw Visual C# Desktop-App-project met behulp van de projectsjabloon **Consoletoepassing** in Visual Studio. Naam van de **afzender**van het project.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. Met de rechtermuisknop op de oplossing in de Solution Explorer en klik op **Beheren NuGet pakketten voor oplossing**. 

3. Klik op het tabblad **Bladeren** en zoeken naar `Microsoft Azure Service Bus`. Zorg ervoor dat de naam van het project (**afzender**) is opgegeven in het vak **versie (s)** . Klik op **installeren**en accepteer de gebruiksvoorwaarden. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio downloadt, installeert en een verwijzing toegevoegd naar het [Azure Service Bus bibliotheek NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Voeg de volgende `using` instructies boven aan het bestand **Program.cs** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. De volgende velden toevoegen aan de klasse **wordt** vervangen door de waarden van de tijdelijke aanduiding door de naam van de gebeurtenis Hub die u in de vorige sectie hebt gemaakt en de naamruimte niveau verbindingsreeks die u eerder hebt opgeslagen.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. De volgende methode toevoegen aan de klasse van het **programma** :

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Deze methode stuurt ononderbroken gebeurtenissen op uw Hub gebeurtenis met een vertraging van 200 ms.

7. Ten slotte, voeg de volgende regels aan de **Main** -methode:

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
