<properties 
    pageTitle="AMQP 1.0 gebruiken met de API van .NET Service Bus | Microsoft Azure" 
    description="Informatie over het gebruik van geavanceerde Message Queuing-Protodol (AMQP) 1.0 met Azure .NET Service Bus-API." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>AMQP 1.0 gebruiken met de Service Bus .NET API

De geavanceerde Message Queuing-Protocol (AMQP) 1.0 is een efficiënte, betrouwbare, draad niveau messaging protocol waarmee u kunt bouwen robuuste, platforms, toepassingen voor berichtafhandeling.

Ondersteuning voor AMQP 1.0 in Bus Service betekent dat het in de wachtrij te gebruiken en publiceren/abonneren brokered functies voor berichten van een aantal platforms met behulp van een efficiënte binaire protocol. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gebouwd met behulp van een combinatie van talen, kaders en besturingssystemen maken.

In dit artikel wordt uitgelegd hoe de Service Bus brokered messaging-functies (wachtrijen en onderwerpen publiceren/abonneren) van .NET-toepassingen met de Service Bus .NET API gebruiken. Er is een [companion artikel](service-bus-java-how-to-use-jms-api-amqp.md) waarin wordt uitgelegd hoe u de hetzelfde met de standaard Java Message Service (JMS)-API. Kunt u deze twee richtlijnen samen voor meer informatie over berichtafhandeling op meerdere platforms met behulp van AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Aan de slag met Service Bus

In dit artikel wordt ervan uitgegaan dat er al een Service Bus naamruimte met een wachtrij met de naam "Wachtrij1." Als u dit niet doet, kunt vervolgens u de naamruimte en de wachtrij met de [Azure portal][]. Zie [aan de slag met Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal)voor meer informatie over het maken van naamruimten Service Bus en wachtrijen.

## <a name="download-the-service-bus-sdk"></a>De Bus Service SDK downloaden

Ondersteuning van AMQP 1.0 is beschikbaar in Service Bus SDK versie 2.1 of hoger. U kunt de meest recente bits downloaden van NuGet op [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Code .NET-toepassingen

Standaard wordt de bibliotheek Service Bus .NET client communiceert met de Service Bus-service via een toegewezen op basis van SOAP-protocol. AMQP 1.0 gebruikt in plaats van de standaard vereist protocol expliciete configuratie op de Bus Service connection string zoals beschreven in de volgende sectie. Dan deze wijziging ongewijzigd toepassingscode blijft in principe bij gebruik van AMQP 1.0.

In de huidige versie zijn er een aantal API-functies die niet worden ondersteund bij gebruik van AMQP. Deze niet-ondersteunde functies worden later vermeld in de sectie [niet-ondersteunde functies en beperkingen](#unsupported-features-and-restrictions). Enkele van de geavanceerde configuratie-instellingen ook hebben een andere betekenis bij het gebruik van AMQP. Geen van deze instellingen worden gebruikt in dit artikel, maar meer details beschikbaar zijn in de [Service Bus AMQP overzicht](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Via App.config configureren

Het is aanbevolen dat toepassingen het configuratiebestand App.config gebruiken voor het opslaan van instellingen. Voor Service Bus toepassingen kunt u voor het opslaan van de Service Bus **ConnectionString**App.config. Deze voorbeeldtoepassing wordt App.config ook gebruikt voor het opslaan van de naam van de Service Bus berichten entiteit die wordt gebruikt.

Een voorbeeld van App.config bestand wordt hier weergegeven:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>De verbindingsreeks Bus-Service configureren

De waarde van de instelling van de **Microsoft.ServiceBus.ConnectionString** is de Service Bus-verbindingsreeks die wordt gebruikt voor het configureren van de verbinding met de Service Bus. De indeling is als volgt:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Waar `[namespace]` en `[SAS key]` zijn verkrijgbaar bij de [Azure portal][]. Zie voor meer informatie [het gebruik van wachtrijen Service Bus] [].

Bij het gebruik van AMQP, de verbindingsreeks wordt toegevoegd met `;TransportType=Amqp`, die vertelt de clientbibliotheek zijn om verbinding te maken met Bus Service met behulp van AMQP 1.0.

### <a name="configure-the-entity-name"></a>Configureer de entiteitsnaam van de

Deze voorbeeldtoepassing wordt de `EntityName` in de sectie **appSettings** van het bestand App.config instellen voor het configureren van de naam van de wachtrij waaraan de toepassing berichten uitwisselt.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Een eenvoudige .NET-toepassing met behulp van een wachtrij Service Bus

In het volgende voorbeeld verzenden en ontvangen van berichten en een wachtrij Service Bus.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>De toepassing wordt uitgevoerd

De toepassing geeft een resultaat van het formulier:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Cross-platform messaging tussen JMS en .NET

In dit onderwerp bleek hoe berichten verzenden naar een Service Bus met .NET en het ontvangen van deze berichten met behulp van .NET. Een van de belangrijkste voordelen van AMQP 1.0 is echter dat het mogelijk toepassingen worden opgebouwd uit onderdelen die zijn geschreven in verschillende talen met de berichten uitgewisseld maakt, betrouwbaar en volledig betrouwbaar.

Met de .NET hierboven beschreven en een vergelijkbare Java-toepassing genomen uit een handleiding, [het gebruik van Java Message Service (JMS) API met Bus Service & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md), is het mogelijk voor het uitwisselen van berichten tussen .NET en Java. 

Zie voor meer informatie over de details van de cross-platform messaging Service Bus en AMQP 1.0 [Service Bus AMQP 1.0-overzicht](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS naar .NET

Voorbeelden van JMS .NET Messaging:

* Start de toepassing .NET monster zonder opdrachtregelargumenten.
* De voorbeeldtoepassing Java starten met de opdrachtregeloptie 'sendonly'. In deze modus wordt de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
* Druk enkele malen op **Enter** in de console Java-toepassing, waardoor de berichten moeten worden verzonden.
* Deze berichten worden ontvangen met het .NET-toepassing.

### <a name="output-from-jms-application"></a>Uitvoer van JMS-toepassing

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET naar JMS

Voorbeelden van .NET JMS Messaging:

* De voorbeeldtoepassing .NET start met het argument 'sendonly'. In deze modus wordt de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
* De voorbeeldtoepassing Java zonder argumenten voor de opdrachtregel start.
* Druk enkele malen op **Enter** in de console van de toepassing .NET, waardoor de berichten moeten worden verzonden.
* Deze berichten worden ontvangen door de Java-toepassing.

#### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Uitvoer van JMS-toepassing

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Niet-ondersteunde functies en beperkingen

De volgende functies van de API van .NET Service Bus worden momenteel niet ondersteund bij gebruik van AMQP:

 * Transacties
 * Verzenden via overdracht bestemming

Zie voor meer informatie, [niet-ondersteunde functies, beperkingen en verschillen doorgevoerd](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Samenvatting

Dit artikel werd toegang tot de Service Bus brokered messaging-functies (wachtrijen en onderwerpen publiceren/abonneren) van .NET AMQP 1.0 en de Service Bus .NET API gebruiken.

U kunt ook Service Bus AMQP 1.0 gebruiken uit andere talen zoals Java, C, Python en PHP. Onderdelen die zijn gebouwd met behulp van deze talen kunnen berichten uitwisselen, betrouwbaar en volledig betrouwbaar met AMQP 1.0 in Service Bus. Zie [Service Bus AMQP-overzicht](service-bus-amqp-dotnet.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van Service Bus en AMQP met .NET hebt gelezen, Zie de volgende koppelingen voor meer informatie.

* [Ondersteuning van AMQP 1.0 in Azure Service Bus](service-bus-amqp-overview.md)
* [Het gebruik van Java Message Service (JMS) API met Bus Service & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Het gebruik van wachtrijen Service Bus](service-bus-dotnet-get-started-with-queues.md)
 
[Azure portal]: https://portal.azure.com
