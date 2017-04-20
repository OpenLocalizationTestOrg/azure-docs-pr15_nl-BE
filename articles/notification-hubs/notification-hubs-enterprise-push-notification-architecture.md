<properties
    pageTitle="Push Notification Hubs - Enterprise-architectuur"
    description="Richtlijnen over het gebruik van Hubs van Azure kennisgeving in een bedrijfsomgeving"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>MSA Enterprise push

Ondernemingen worden vandaag geleidelijk verplaatsen tot de verwezenlijking van mobiele toepassingen voor eindgebruikers (extern) of voor de werknemers (intern). Bestaande back-end systemen mainframes of sommige LoB-toepassingen die moeten worden geïntegreerd in de mobiele toepassingsarchitectuur hebben. Deze handleiding zal praten over de beste manier om te doen deze mogelijke oplossing aanbeveelt voor algemene scenario's voor integratie.

Vaak vereist is voor het push-bericht verzenden naar de gebruikers via hun mobiele toepassing als een gebeurtenis van belang in de back-end systemen. Bv. een bank klant die van de bank bankieren app op haar iPhone heeft wil worden gewaarschuwd wanneer een debetbedrag is aangebracht boven een bepaald bedrag van haar rekening of een intranet-scenario waarbij een werknemer uit de afdeling Financiën met een budget goedkeuring app op zijn Windows Phone wil worden geïnformeerd wanneer hij een aanvraag tot goedkeuring ontvangt.

Bankrekening of goedkeuring verwerking kan worden uitgevoerd in sommige back endsysteem dat een push aan de gebruiker moet starten. Er zijn meerdere dergelijke back-end systemen die alle dezelfde soort logica push implementeren als een gebeurtenis een melding wordt gemaakt. De complexiteit hier ligt in de integratie van verschillende back-end-systemen met een enkele push-systeem waarbij de eindgebruikers kunnen verschillende berichten bent geabonneerd en er zelfs mogelijk meerdere mobiele toepassingen, bijvoorbeeld in het geval van intranet, mobiele toepassingen waarbij een mobiele toepassing kan meldingen wilt ontvangen van meerdere van dergelijke back-end systemen. De back-end systemen niet kent of wil weten van push-semantiek/technologie zodat hier een gemeenschappelijke oplossing traditioneel is in te voeren, een onderdeel dat controleert of de back-end systemen voor de gebeurtenissen van belang en is verantwoordelijk voor de push-berichten verzenden naar de client.
Hier zullen we praten over een nog betere oplossing met Azure Service Bus - onderwerp/abonnement model waarmee de complexiteit bij het schaalbaar maken van de oplossing wordt beperkt.

Hier wordt de algemene architectuur van de oplossing (gegeneraliseerde met meerdere mobiele toepassingen, maar eveneens van toepassing wanneer er slechts één mobiele app)

## <a name="architecture"></a>Architectuur

![][1]

De sleutel in dit diagram architectuur is Azure Service Bus biedt een onderwerpen/abonnementen programmeermodel (meer op het op de [Service Bus Pub/Sub programmeren]). De ontvanger, in dit geval is de mobiele back-end (meestal [Azure Mobile Service], die een push naar mobiele toepassingen starten zal) ontvangt geen berichten rechtstreeks vanuit de back-end systemen, maar in plaats daarvan hebben we een tussenliggende abstraction layer geleverd door [Azure Service Bus] waarmee mobiele back-end om berichten te ontvangen van een of meer back-end systemen. Een onderwerp van de Bus-Service moet worden gemaakt voor elk van de back-end systemen zoals Account, HR, financiën, die in principe 'onderwerpen' van belang dat berichten worden verzonden als een push-bericht wordt gestart. De back-end systemen sturen berichten naar deze onderwerpen. Een mobiele back-end kunnen zich abonneren op een of meer van dergelijke onderwerpen door het maken van een abonnement Service Bus. Dit recht geven de mobiele back-end een melding ontvangen van de corresponderende back endsysteem. Mobiele backend blijft luisteren naar berichten op hun abonnementen en zodra er een bericht binnenkomt, dit wordt weer en deze als kennisgeving aan de hub van de kennisgeving. Melding hubs uiteindelijk bezorgt het bericht vervolgens naar de mobiele app. Om samen te vatten de belangrijkste onderdelen, hebben we dus:

1. Back-end-systemen (LoB/Legacy-systemen)
    - Service Bus onderwerp maakt
    - Bericht verzonden
2. Mobiele back-end
    - Met deze methode maakt een serviceabonnement
    - Bericht ontvangen (van back-end-systeem)
    - Melding verzonden naar clients (via Azure melding Hub)
3. Mobiele toepassing
    - Ontvangt en melding weergeven

###<a name="benefits"></a>Voordelen:

1. De ontkoppeling tussen de ontvanger (mobiele app/service via kennisgeving Hub) en de afzender (back-end-systemen), kunt extra back-end-systemen worden geïntegreerd met minimale wijzigen.
2. Hierdoor wordt het scenario van meerdere mobiele apps kunnen ontvangen van gebeurtenissen van een of meer back-end systemen.  

## <a name="sample"></a>Voorbeeld:

###<a name="prerequisites"></a>Vereisten
U moet de volgende zelfstudies vertrouwd met de concepten, alsmede gemeenschappelijke maken & configuratiestappen uitvoeren:

1. [Programmeren Service Bus Pub/Sub] - Dit verklaart de details van het werken met abonnementen/Service Bus onderwerpen, het maken van een naamruimte bevat onderwerpen/abonnementen, verzenden en ontvangen van berichten van deze procedure.
2. [Melding Hubs - zelfstudie Windows Universal] - dit wordt uitgelegd hoe u een Windows Store app instellen en gebruiken van kennisgeving Hubs te registreren en vervolgens berichten te ontvangen.

###<a name="sample-code"></a>Voorbeeld van code

De volledige voorbeeldcode is [Kennisgeving Hub monsters]beschikbaar. Het is opgedeeld in drie onderdelen:

1. **EnterprisePushBackendSystem**

    een. Dit project maakt gebruik van het pakket *WindowsAzure.ServiceBus* Nuget en is gebaseerd op de [Service Bus Pub/Sub programmeren].

    b. Dit is een eenvoudig C# console app voor het simuleren van een LoB-systeem waarmee het bericht is afgeleverd bij de mobiele app wordt geïnitieerd.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`wordt gebruikt voor het maken van de Service Bus onderwerp waar we berichten sturen.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`de berichten worden verzonden naar dit onderwerp Service Bus wordt gebruikt. Hier zijn we alleen het een reeks willekeurige berichten verzenden naar het onderwerp regelmatig voor de toepassing van het monster. Normaal gesproken wordt er een back endsysteem dat berichten verzenden zal wanneer een gebeurtenis plaatsvindt.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    een. Dit project maakt gebruik van de *WindowsAzure.ServiceBus* en *Microsoft.Web.WebJobs.Publish* Nuget pakketten en is gebaseerd op de [Service Bus Pub/Sub programmeren].

    b. Dit is een andere C# console app die we als een [Azure-WebJob] wordt uitgevoerd, omdat er voortdurend actief om te luisteren naar berichten van de LoB/back-end systemen. Dit is onderdeel van uw mobiele back-end.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`wordt gebruikt voor het maken van een abonnement Service Bus voor het onderwerp waar de back-endsysteem verzenden. Afhankelijk van het scenario maakt dit onderdeel een of meer abonnementen naar overeenkomstige onderwerpen (bv. Sommige kunnen worden ontvangen van HR-systeem, sommige van het financiële systeem, enzovoort)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification wordt gebruikt om het bericht te lezen van het onderwerp werken met het abonnement en als het lezen lukt vervolgens een melding (in het voorbeeldscenario een melding van Windows native toast) opgesteld om te worden verzonden naar de mobiele toepassing met Azure melding Hubs.

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Klik met de rechtermuisknop op de oplossing in Visual Studio voor het publiceren van dit als een **WebJob**, en selecteer **publiceren als WebJob**

    ![][2]

    f. Selecteer het gewenste profiel publiceren en maak een nieuwe Azure-WebSite als deze niet al die host is van dit WebJob en als u eenmaal en de WebSite vervolgens **Publiceren hebt**.

    ![][3]

    g. Configureren van de taak 'Doorlopend worden uitgevoerd' worden zodat wanneer u zich bij de [Klassieke Portal Azure aanmeldt] moet er ongeveer zo uit:

    ![][4]


3. **EnterprisePushMobileApp**

    een. Dit is een toepassing voor Windows Store toast meldingen ontvangen van de WebJob die wordt uitgevoerd als onderdeel van uw mobiele back-end en weer te geven. Dit is gebaseerd op de [Melding Hubs - universele Windows zelfstudie].  

    b. Zorg ervoor dat uw toepassing is ingeschakeld voor het ontvangen van meldingen van toast.

    c. Zorg ervoor dat de volgende melding Hubs registratiecode wordt aangeroepen op de App opstarten (na het vervangen van de *HubName* en *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Steekproef uitgevoerd:

1. Zorg ervoor dat uw WebJob met succes is uitgevoerd en gepland voor "Doorlopend worden uitgevoerd".
2. De **EnterprisePushMobileApp** die de Windows Store-app wordt uitgevoerd.
3. Voer de **EnterprisePushBackendSystem** console-toepassing die de LoB-end simuleren en zal beginnen met het verzenden van berichten en toast meldingen worden weergegeven als de volgende weergegeven:

    ![][5]

4. De berichten die werden oorspronkelijk verzonden naar was wordt bewaakt door Bus Service abonnementen in uw Project Web Service Bus-onderwerpen. Wanneer een bericht is ontvangen, is een melding gemaakt en verzonden naar de mobiele app. U kunt bekijken via de logboeken WebJob bevestigen de verwerking wanneer u naar de koppeling Logboeken in [Azure klassieke Portal] voor uw Web-functie gaat:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Melding Hub monsters]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobiele Service]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub-programmering]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Melding Hubs - universele Windows zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure klassieke Portal]: https://manage.windowsazure.com/
