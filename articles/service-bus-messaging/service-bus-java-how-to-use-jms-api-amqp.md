<properties 
    pageTitle="AMQP 1.0 gebruiken met de API voor Java Service Bus | Microsoft Azure" 
    description="Het gebruik van de Java Message Service (JMS) met Azure Service Bus en geavanceerde Message Queuing-Protodol (AMQP) 1.0." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Het gebruik van Java Message Service (JMS) API met Service Bus en AMQP 1.0

De geavanceerde Message Queuing-Protocol (AMQP) 1.0 is een efficiënte, betrouwbare, draad niveau messaging protocol kunt u robuuste, cross-platform messaging-toepassingen te bouwen.

Ondersteuning voor AMQP 1.0 in Bus Service betekent dat het in de wachtrij te gebruiken en publiceren/abonneren brokered functies voor berichten van een aantal platforms met behulp van een efficiënte binaire protocol. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gebouwd met behulp van een combinatie van talen, kaders en besturingssystemen maken.

In dit artikel wordt uitgelegd hoe Service Bus messaging-functies (wachtrijen en onderwerpen publiceren/abonneren) gebruik van Java-toepassingen met behulp van de populaire Java Message Service (JMS) standaard API. Er is een [companion artikel](service-bus-dotnet-advanced-message-queuing.md) waarin wordt uitgelegd hoe u hiervoor de Service Bus .NET API gebruiken. Kunt u deze twee richtlijnen samen voor meer informatie over berichtafhandeling op meerdere platforms met behulp van AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Aan de slag met Service Bus

Deze handleiding wordt ervan uitgegaan dat er al een Service Bus naamruimte die een wachtrij met de naam **Wachtrij1**bevat. Als u dit niet doet, vervolgens kunt u [de naamruimte en de wachtrij maken](service-bus-create-namespace-portal.md) met behulp van de [portal Azure](https://portal.azure.com). Zie voor meer informatie over het maken van naamruimten Service Bus en wachtrijen, [wachtrijen Service Bus gebruiken](service-bus-dotnet-get-started-with-queues.md).

> [AZURE.NOTE] AMQP ondersteunen gepartitioneerde wachtrijen en onderwerpen. Zie [Partitioned messaging entiteiten](service-bus-partitioning.md) en [AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen](service-bus-partitioned-queues-and-topics-amqp-overview.md)voor meer informatie.

## <a name="downloading-the-amqp-10-jms-client-library"></a>De bibliotheek JMS van AMQP 1.0-client downloaden

Ga naar [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html)voor meer informatie over het downloaden van de nieuwste versie van de bibliotheek van Apache Qpid JMS AMQP 1.0-client.

U moet de volgende vier JAR-bestanden uit het archief van Apache Qpid JMS AMQP 1.0 distributie toevoegen aan het Java-KLASSENPAD bij het bouwen en uitvoeren van JMS toepassingen met Bus-Service:

- geronimo-jms\_1.1\_spec-1.0.jar
- qpid-amqp-1-0-client-[Version].jar
- qpid-amqp-1-0-client-jms-[Version].jar
- qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Code van Java-toepassingen

### <a name="java-naming-and-directory-interface-jndi"></a>Java Naming en Directory-Interface (JNDI)

JMS gebruikt Java Naming en Directory-Interface (JNDI) voor het maken van een scheiding tussen de namen van de logische en fysieke. Twee typen JMS objecten worden opgelost met behulp van JNDI: ConnectionFactory en de bestemming. JNDI maakt gebruik van een providermodel waarin u verschillende directoryservices voor het afhandelen van naam resolutie rechten kunt aansluiten. De Apache Qpid JMS AMQP 1.0-bibliotheek wordt geleverd met een eenvoudige eigenschappen op basis van een bestand JNDI-Provider die is geconfigureerd met een eigenschappenbestand van de volgende notatie:

```
# servicebus.properties - sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>De ConnectionFactory configureren

De post die wordt gebruikt voor het definiëren van een **ConnectionFactory** in de Qpid eigenschappen bestand JNDI-Provider heeft de volgende notatie:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Waar **[jndi_name]** en **[ConnectionURL]** de volgende betekenis hebben:

- **[jndi_name]**: de logische naam van het ConnectionFactory. Dit is de naam die in de Java-toepassing met de methode JNDI-IntialContext.lookup() wordt opgelost.
- **[ConnectionURL]**: een URL die de bibliotheek JMS met de vereiste gegevens aan de AMQP broker levert.

De indeling van de **ConnectionURL** is als volgt:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Waarbij **[namespace]**, **[SASPolicyName]** en **[SASPolicyKey]** hebben de volgende betekenis:

- **[namespace]**: de Service Bus-naamruimte.
- **[SASPolicyName]**: de naam van de wachtrij gedeelde toegang handtekening beleid.
- **[SASPolicyKey]**: de wachtrij gedeelde toegang beleid handtekeningsleutel.

> [AZURE.NOTE] Moet u de URL-codeert het wachtwoord handmatig. Een handig hulpprogramma voor URL-codering is beschikbaar op [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>-Bestemmingen configureren

De post die wordt gebruikt voor het definiëren van een bestemming in de Qpid eigenschappen bestand JNDI-Provider heeft de volgende notatie:

```
queue.[jndi_name] = [physical_name]
```

of

```
topic.[jndi_name] = [physical_name]
```

Waar **[jndi\_naam]** en **[fysieke\_naam]** hebben de volgende betekenis:

- **[jndi_name]**: de logische naam van de bestemming. Dit is de naam die in de Java-toepassing met de methode JNDI-IntialContext.lookup() wordt opgelost.
- **[physical_name]**: de naam van de Service Bus entiteit waaraan de toepassing berichten verzendt of ontvangt.

> [AZURE.NOTE] Bij ontvangst van een onderwerp Bus Service abonnement, is de fysieke naam die is opgegeven in JNDI moet de naam van het onderwerp. Naam van het abonnement wordt geleverd wanneer de duurzame abonnement wordt gemaakt in de toepassingscode JMS. De [Service Bus AMQP 1.0-handleiding voor ontwikkelaars](service-bus-amqp-dotnet.md) bevat meer informatie over het werken met de Service Bus onderwerp abonnementen van JMS.

### <a name="write-the-jms-application"></a>De toepassing JMS schrijven

Er zijn geen speciale API's of opties die het gebruik van JMS met Service Bus. Er zijn echter enkele beperkingen die later aan bod. Net als bij elke toepassing JMS beginnen eerst de vereiste configuratie van het milieu JNDI, kunnen een **ConnectionFactory** en bestemmingen op te lossen.

#### <a name="configure-the-jndi-initialcontext"></a>De JNDI-InitialContext configureren

De JNDI-omgeving wordt geconfigureerd door de hash-tabel van de configuratie-informatie wordt doorgegeven aan de constructor van de klasse javax.naming.InitialContext. De twee vereiste elementen in de hash-tabel zijn de klassenaam van de oorspronkelijke Context fabriek en de URL van de Provider. De volgende code toont de JNDI-omgeving voor het gebruik van het bestand Qpid-eigenschappen configureren op basis van JNDI-Provider met een eigenschappenbestand met de naam **servicebus.properties**.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Een eenvoudige JMS-toepassing met behulp van een wachtrij Service Bus

Het volgende voorbeeld wordt TextMessages van JMS naar een wachtrij Service Bus met de logische JNDI-naam van de wachtrij, berichten verzonden en ontvangen het terug.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>De toepassing wordt uitgevoerd

De toepassing geeft een resultaat van het formulier:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Cross-platform messaging tussen JMS en .NET

Deze handleiding bleek het verzenden en ontvangen van berichten van en naar de Bus-Service met behulp van JMS. Een van de belangrijkste voordelen van AMQP 1.0 is echter dat het mogelijk toepassingen worden opgebouwd uit onderdelen die zijn geschreven in verschillende talen met de berichten uitgewisseld maakt, betrouwbaar en volledig betrouwbaar.

Met de voorbeeldtoepassing JMS hierboven beschreven en een vergelijkbare toepassing die .NET afkomstig uit een handleiding, [het gebruik van AMQP 1.0 met .NET Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md), kun je berichten tussen .NET en Java. 

Zie voor meer informatie over de details van de cross-platform messaging Service Bus en AMQP 1.0 [Service Bus AMQP 1.0-handleiding voor ontwikkelaars](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS naar .NET

Voorbeelden van JMS .NET Messaging:

* Start de toepassing .NET monster zonder opdrachtregelargumenten.
* De voorbeeldtoepassing Java starten met de opdrachtregeloptie 'sendonly'. In deze modus wordt de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
* Druk enkele malen op **Enter** in de console Java-toepassing, waardoor de berichten moeten worden verzonden.
* Deze berichten worden ontvangen met het .NET-toepassing.

#### <a name="output-from-jms-application"></a>Uitvoer van JMS-toepassing

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET naar JMS

Voorbeelden van .NET JMS Messaging:

* De voorbeeldtoepassing .NET start met de opdrachtregeloptie 'sendonly'. In deze modus wordt de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
* Start de voorbeeldtoepassing Java zonder opdrachtregelargumenten.
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

De volgende beperkingen zijn bij het gebruik van JMS via AMQP 1.0 met Service Bus, namelijk:

* Alleen een **MessageProducer** of **MessageConsumer** is toegestaan per **sessie**. Als u nodig hebt voor het maken van meerdere **MessageProducers** of **MessageConsumers** in een toepassing, maakt u een exclusieve **sessie** voor elk van hen.
* Vluchtige onderwerp abonnementen worden momenteel niet ondersteund.
* **MessageSelectors** worden momenteel niet ondersteund.
* Tijdelijke bestemmingen, dat wil zeggen, **TemporaryQueue**, **TemporaryTopic** worden momenteel niet ondersteund, samen met de **QueueRequestor** en de **TopicRequestor** API's die ze gebruiken.
* Transactionele sessies en gedistribueerde transacties worden niet ondersteund.

## <a name="summary"></a>Samenvatting

Deze Procedurebeschrijving bleek Service Bus brokered messaging-functies (wachtrijen en onderwerpen publiceren/abonneren) van Java gebruiken met de API voor populaire JMS en AMQP 1.0.

U kunt ook Service Bus AMQP 1.0 gebruiken uit andere talen, zoals .NET, C, Python en PHP. Onderdelen gebouwd met behulp van deze verschillende talen kunnen wisselen berichten op betrouwbare wijze en in samenwerking met de AMQP 1.0 in Bus Service ondersteunen. Zie [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* [Ondersteuning van AMQP 1.0 in Azure Service Bus](service-bus-amqp-overview.md)
* [AMQP 1.0 gebruiken met de Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)
* [Service Bus AMQP 1.0-handleiding voor ontwikkelaars](service-bus-amqp-dotnet.md)
* [Het gebruik van wachtrijen Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Java Developer Center](/develop/java/).



 
