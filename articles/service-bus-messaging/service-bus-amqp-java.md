<properties 
    pageTitle="Service Bus en Java met AMQP 1.0 | Microsoft Azure"
    description="Bus Service van Java gebruiken met AMQP"
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="use-service-bus-from-java-with-amqp-10"></a>Bus Service van Java gebruiken met AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

De Java Message Service (JMS) is een standaard-API voor het werken met message-oriented middleware ' op het Java platform. JMS client-bibliotheek ontwikkeld door de Apache Qpid project op basis van Microsoft die Azure Service Bus is getest met de AMQP 1.0. Deze bibliotheek ondersteunt de volledige JMS-1.1-API en kan worden gebruikt met elke willekeurige service voor AMQP 1.0-compatibel. In dit scenario wordt ook ondersteund in [Bus Service voor Windows Server](https://msdn.microsoft.com/library/dn282144.aspx) (op-premises Service Bus). Zie [AMQP in Bus Service voor Windows Server][]voor meer informatie.

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>De bibliotheek van Apache Qpid AMQP 1.0 JMS-client downloaden

Ga naar [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html)voor meer informatie over het downloaden van de nieuwste versie van de bibliotheek van Apache Qpid JMS AMQP 1.0-client.

U moet de volgende vier JAR-bestanden uit het archief van Apache Qpid JMS AMQP 1.0 distributie toevoegen aan het Java-KLASSENPAD bij het bouwen en uitvoeren van JMS toepassingen met Bus-Service:

-   geronimo-jms\_1.1\_JAR spec-[versie]

-   qpid-amqp-1-0-client-[Version].jar

-   qpid-amqp-1-0-client-jms-[Version].jar

-   qpid-amqp-1-0-Common-[Version].jar

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Werken met wachtrijen, onderwerpen en abonnementen van JMS Service Bus

### <a name="java-naming-and-directory-interface-jndi"></a>Java Naming en Directory-Interface (JNDI)

JMS gebruikt Java Naming en Directory-Interface (JNDI) voor het maken van een scheiding tussen de namen van de logische en fysieke. Twee typen JMS objecten worden opgelost met behulp van JNDI: **ConnectionFactory** en de **bestemming**. JNDI maakt gebruik van een providermodel waarin u verschillende directoryservices voor het afhandelen van naam resolutie rechten kunt aansluiten. De bibliotheek van Apache Qpid JMS AMQP 1.0 wordt geleverd met een eenvoudige eigenschappen op basis van een bestand JNDI-Provider die is geconfigureerd met een tekstbestand.

De Qpid eigenschappen bestand JNDI-Provider wordt geconfigureerd met een eigenschappenbestand van de volgende notatie:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>De fabriek verbinding configureren

De post die wordt gebruikt voor het definiëren van een **ConnectionFactory** in de Qpid eigenschappen bestand JNDI-Provider heeft de volgende notatie:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Waar `[jndi\_name]` en `[ConnectionURL]` hebben de volgende betekenis:

| Naam            | Betekenis                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | De logische naam van de verbinding-fabriek. Deze naam wordt omgezet in de Java-toepassing met behulp van de JNDI `IntialContext.lookup()` methode. |   |   |   |   |
| `[ConnectionURL]` | Een URL die de bibliotheek JMS met de vereiste gegevens aan de AMQP broker levert.                                                      |   |   |   |   |

De indeling van de verbinding URL is als volgt:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Waar `[namespace]`, `[username]`, en `[password]` hebben de volgende betekenis:

| Naam          | Betekenis                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | De naamruimte Service Bus is verkregen uit de [Azure portal][].                      |   |   |   |   |
| `[username]`  | De Service Bus SAS sleutelnaam verkregen uit de [Azure portal][].                    |   |   |   |   |
| `[password]`  | URL-gecodeerde vorm van de verkregen uit de [portal Azure][]Service Bus SAS-sleutel. |   |   |   |   |

> [AZURE.NOTE] Moet u de URL-codeert het wachtwoord handmatig. Nuttige hulpprogramma URL-codering is beschikbaar op [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

Bijvoorbeeld, als de informatie opgehaald van is de portal als volgt:

| Namespace:   | Test.servicebus.Windows.NET                  |
|--------------|----------------------------------------------|
| Naam uitgever: | RootManageSharedAccessKey                                        |
| Sleutel van de uitgevende instelling:  | abcdefg |

Klik om het definiëren van een **ConnectionFactory** -object met de naam `SBCONNECTIONFACTORY`, de configuratiereeks worden als volgt:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>-Bestemmingen configureren

De post die u een bestemming in de Qpid eigenschappen bestand JNDI-Provider definieert heeft de volgende notatie:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Waar `[jndi\_name]` en `[physical\_name]` hebben de volgende betekenis:

| Naam              | Betekenis                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | De logische naam van de bestemming. Dit is de naam wordt omgezet in de Java-toepassing met behulp van de JNDI `IntialContext.lookup()` methode. |
| `[physical\name]` | De naam van de Service Bus entiteit waaraan de toepassing berichten verzendt of ontvangt.                                                  |

Let op het volgende:

- De `[physical\name]` waarde kan zijn voor een Service Bus wachtrij of onderwerp.
- Bij ontvangst van een onderwerp Bus Service abonnement, is de fysieke naam die is opgegeven in JNDI moet de naam van het onderwerp. Naam van het abonnement wordt geleverd wanneer de duurzame abonnement wordt gemaakt in de toepassingscode JMS.
- Het kan ook een abonnement Service Bus onderwerp behandelen als een wachtrij JMS. Er zijn verschillende voordelen van deze aanpak: alle adresgegevens (de namen van onderwerp en abonnement) is externalized in het eigenschappenbestand en ontvanger dezelfde code kan worden gebruikt voor wachtrijen en onderwerp abonnementen.
- Een abonnement Service Bus onderwerp moet worden behandeld als een wachtrij JMS, moet de vermelding in het eigenschappenbestand van het formulier: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |

Als u een logische JMS bestemming met de naam 'Onderwerp' die is toegewezen aan een Service Bus onderwerp met de naam "Onderwerp1" definieert, zou de vermelding in het eigenschappenbestand zijn als volgt:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>Berichten verzenden met JMS

De volgende code ziet u hoe een bericht te verzenden naar een onderwerp Service Bus. Aangenomen wordt dat `SBCONNECTIONFACTORY` en `TOPIC` zijn gedefinieerd in een configuratiebestand **servicebus.properties** zoals beschreven in de vorige sectie.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Ontvangen berichten via JMS

De volgende code toont `how` een bericht ontvangen van een abonnement Service Bus onderwerp. Aangenomen wordt dat `SBCONNECTIONFACTORY` en het onderwerp in een configuratiebestand **servicebus.properties** zijn gedefinieerd, zoals beschreven in de vorige sectie. Ook wordt ervan uitgegaan dat de naam van het abonnement is `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Richtlijnen voor het bouwen van krachtige toepassingen

De JMS-specificatie definieert hoe het contract met uitzondering van de methoden van de API en de toepassingscode voor het verwerken van dergelijke uitzonderingen moet worden geschreven. Hier zijn een aantal andere punten rekening te houden met betrekking tot de afhandeling van uitzonderingen:

-   Schrijf een **ExceptionListener** met de JMS verbinding met **connection.setExceptionListener**. Hierdoor kan clients asynchroon worden gesteld van een probleem. Deze melding is vooral belangrijk voor verbindingen die alleen berichten verbruiken, als ze er geen andere manier om te leren dat de verbinding is mislukt. De **ExceptionListener** wordt aangeroepen als er een probleem met de onderliggende AMQP verbinding, sessie, of de koppeling. In dit geval moet de toepassing de **Verbinding JMS**, **sessie**, **MessageProducer** en **MessageConsumer** objecten helemaal opnieuw.

-   Om te controleren dat een bericht is verzonden vanaf een **MessageProducer** aan een entiteit Service Bus, ervoor te zorgen dat de toepassing is geconfigureerd met de **qpid.sync\_publiceren** eigenschap systeem is ingesteld. U kunt dit doen door het programma met de **-Dqpid.sync\_publiceren = true** Java VM-optie ingesteld op de opdrachtregel bij het starten van de toepassing. Als u deze optie instelt, configureert u de bibliotheek om niet uit de oproep verzenden totdat bevestiging is ontvangen dat het bericht is goedgekeurd door de Bus Service. Als er een probleem optreedt tijdens het verzenden, wordt een **JMSException** gegenereerd. Er zijn twee mogelijke oorzaken: 
    1. Als het probleem veroorzaakt door Service Bus weigert het specifieke bericht dat wordt verzonden wordt, wordt een uitzondering van de **MessageRejectedException** worden verhoogd. Deze fout is van tijdelijke aard, of vanwege een probleem met het bericht. De aanbevolen cursus van actie is om verschillende pogingen op probeer het opnieuw met een bepaalde logica terug uit. Als het probleem zich blijft voordoen, moet het bericht worden afgeschaft met een fout die lokaal is aangemeld. Er is niet nodig opnieuw **Verbinding JMS**, **Session**of **MessageProducer** objecten in deze situatie te maken. 
    2. Als het probleem veroorzaakt door Service Bus de koppeling AMQP wordt gesloten wordt, wordt een uitzondering van de **InvalidDestinationException** worden verhoogd. Dit is vanwege een probleem van voorbijgaande aard, of door de entiteit berichten worden verwijderd. In beide gevallen moeten opnieuw **Verbinding JMS**, **sessie**en **MessageProducer** -objecten worden gemaakt. Als de fout weer tijdelijk is, kan deze bewerking is uiteindelijk niet gelukt. Als de entiteit is verwijderd, wordt de fout is permanent.

## <a name="messaging-between-net-and-jms"></a>Berichten tussen .NET en JMS

### <a name="message-bodies"></a>De berichttekst

JMS definieert vijf verschillende berichttypen: **BytesMessage**, **MapMessage**, **ObjectMessage**, **StreamMessage**en **TextMessage**. De Service Bus .NET API heeft een enkel berichttype, [BrokeredMessage][].

#### <a name="jms-to-service-bus-net-api"></a>JMS Service bus .NET API

In de volgende secties wordt aangegeven hoe berichten van elk van de typen JMS berichten van .NET in beslag neemt. Een **ObjectMessage** -voorbeeld is niet opgenomen, zoals de hoofdtekst van een **ObjectMessage** bevat een geserialiseerd object in de programmeertaal Java, die niet door een toepassing .NET interpreteerbare.

##### <a name="bytesmessage"></a>BytesMessage

De volgende code ziet u hoe de hoofdtekst van een **BytesMessage** -object met behulp van de Service Bus .NET API's in beslag neemt.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

De volgende code ziet u hoe de hoofdtekst van een **MapMessage** -object met behulp van de Service Bus .NET API's in beslag neemt. Deze code doorloopt naar de elementen van de kaart, de naam en waarde van elk element weer te geven.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

De volgende code ziet u hoe de hoofdtekst van een **StreamMessage** -object met behulp van de Service Bus .NET API's in beslag neemt. Deze code worden alle items uit de stream, samen met hun type.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

De volgende code ziet u hoe de hoofdtekst van een **TextMessage** -object met behulp van de Service Bus .NET API's in beslag neemt. Deze code wordt de tekst weergegeven in de hoofdtekst van het bericht opgenomen.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Service Bus .NET API's JMS

De volgende secties wordt uitgelegd hoe een .NET-toepassing een bericht dat wordt ontvangen op JMS in elk van de verschillende typen van JMS bericht kunt maken. Een **ObjectMessage** -voorbeeld is niet opgenomen, zoals de hoofdtekst van een **ObjectMessage** bevat een geserialiseerd object in de programmeertaal Java, die niet door een toepassing .NET interpreteerbare.

##### <a name="bytesmessage"></a>BytesMessage

De volgende code ziet u hoe een [BrokeredMessage][] -object in .NET wordt ontvangen door een JMS-client als een **BytesMessage**maken.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

De volgende code ziet u hoe een [BrokeredMessage][] -object in .NET wordt ontvangen door een JMS-client als een **StreamMessage**maken.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

De volgende code ziet u hoe de hoofdtekst van een **TextMessage** in beslag neemt met de Service Bus .NET API. Deze code wordt de tekst weergegeven in de hoofdtekst van het bericht opgenomen.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Eigenschappen van toepassingen

####<a name="jms-to-service-bus-net-apis"></a>JMS op Bus-API's van .NET Service

JMS berichten toepassingseigenschappen van de volgende typen ondersteuning: **boolean**, **byte**, **short**, **int**, **lange**, **float**, **dubbele**en **tekenreeks**. De volgende Java-code ziet u hoe eigenschappen instellen voor een bericht met behulp van elk type eigenschap.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

Berichteigenschappen van toepassing zijn in de Service Bus .NET API's die in de **Properties** -collectie van [BrokeredMessage][]. De volgende code ziet u hoe de eigenschappen van een bericht ontvangen van een client JMS.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

De volgende tabel ziet u hoe de eigenschap JMS typen toewijzen aan de eigenschap .NET typen.

| JMS (Type eigenschap) | .NET (Type eigenschap) |
|-------------------|--------------------|
| Byte              | SByte              |
| Geheel getal           | int                |
| Float             | Float              |
| Getal met dubbele precisie            | dubbel             |
| Boole-waarde           | BOOL               |
| Tekenreeks            | tekenreeks             |

Het type [BrokeredMessage][] toepassingseigenschappen van de volgende typen ondersteund: **byte**, **sbyte**, **char**, **korte**, **ushort**, **int**, **uint**, **long**, **ulong**, **float**, **double**, **decimaal**, **bool**, **Guid**, **string**, **Uri**, **DateTime**, **DateTimeOffset**, en **TimeSpan**. De volgende .NET-code ziet u hoe eigenschappen op een [BrokeredMessage][] -object met behulp van deze eigenschap instellen.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

De volgende Java-code ziet u hoe de eigenschappen van een bericht ontvangen van een client Service Bus .NET.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

De volgende tabel ziet u hoe de eigenschap .NET typen toewijzen aan de eigenschap JMS typen.

| .NET (Type eigenschap) | JMS (Type eigenschap) | Notities                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | UnsignedByte      | -                                                                                                                                                                      |
| SByte              | Byte              | -                                                                                                                                                                     |
| CHAR               | Teken         | -                                                                                                                                                                     |
| korte              | Korte             | -                                                                                                                                                                     |
| USHORT             | UnsignedShort     | -                                                                                                                                                                     |
| int                | Geheel getal           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| lange               | Lange              | -                                                                                                                                                                     |
| ULONG              | UnsignedLong      | -                                                                                                                                                                     |
| Float              | Float             | -                                                                                                                                                                     |
| dubbel             | Getal met dubbele precisie            | -                                                                                                                                                                     |
| decimaal            | BigDecimal        | -                                                                                                                                                                     |
| BOOL               | Boole-waarde           | -                                                                                                                                                                     |
| GUID               | UUID              | -                                                                                                                                                                     |
| tekenreeks             | Tekenreeks            | -                                                                                                                                                                     |
| Datum/tijd           | Datum              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | Toegewezen aan AMQP type DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| TimeSpan           | DescribedType     | Toegewezen aan AMQP type Timespan.Ticks:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType     | Toegewezen aan AMQP type Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>Standaardkoppen voor

De volgende tabellen tonen hoe de Standaardkoppen voor JMS en de [BrokeredMessage][] standaard eigenschappen worden toegewezen met behulp van AMQP 1.0.

#### <a name="jms-to-service-bus-net-apis"></a>JMS op Bus-API's van .NET Service

| JMS              | Bus service .NET               | Notities                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Momenteel niet beschikbaar        | De Bus service biedt alleen ondersteuning voor duurzame berichten; bijvoorbeeld, DeliveryMode.PERSISTENT, ongeacht wat is opgegeven.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Bericht. TimeToLive            | Conversie                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | JMSMessageID is standaard gecodeerd in binaire vorm in het bericht van AMQP. Na ontvangst van de binaire bericht-id omgezet de .NET client library in een tekenreeks die is gebaseerd op de Unicode-waarden van de bytes. Toevoegen als u de bibliotheek JMS gebruik string bericht-id, de "binaire messageid = false ' tekenreeks voor de queryparameters van de JNDI-ConnectionURL. Bijvoorbeeld: “amqps://[username]:[password]@[namespace].servicebus.windows.net? binair messageid = false '. |
| JMSPriority      | Momenteel niet beschikbaar        | Bus-service biedt geen ondersteuning voor de berichtprioriteit.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Momenteel niet beschikbaar        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Bericht. ReplyTo               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Conversie                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms-type"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Service Bus .NET API's JMS

| Bus service .NET        | JMS              | Notities                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | Momenteel niet beschikbaar |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Conversie              |
| Label                   | n.v.t.              | Momenteel niet beschikbaar |
| MessageId               | JMSMessageID     | -                         |
| ReplyTo                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | n.v.t.              | Momenteel niet beschikbaar |
| ScheduledEnqueueTimeUtc | n.v.t.              | Momenteel niet beschikbaar |
| Sessie-id               | n.v.t.              | Momenteel niet beschikbaar |
| TimeToLive              | JMSExpiration    | Conversie              |
| Aan                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Niet-ondersteunde functies en beperkingen

De volgende beperkingen zijn bij het gebruik van JMS via AMQP 1.0 met Service Bus:

-   Alleen een **MessageProducer** of **MessageConsumer** is toegestaan per sessie. Als u meerdere objecten te maken **MessageProducer** of **MessageConsumer** in een toepassing wilt, exclusieve sessies voor elk van hen maken.

-   Vluchtige onderwerp abonnementen worden momenteel niet ondersteund.

-   **MessageSelector** -objecten worden niet ondersteund.

-   Tijdelijke bestemmingen; bijvoorbeeld **TemporaryQueue** of **TemporaryTopic**, worden niet ondersteund, samen met de **QueueRequestor** en de **TopicRequestor** API's die ze gebruiken.

-   Transacted-sessies worden niet ondersteund.

-   Gedistribueerde transacties worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

Wilt u meer informatie? Ga naar de volgende koppelingen:

- [Overzicht Service Bus AMQP]
- [AMQP in Bus Service voor Windows Server]

[AMQP in Bus Service voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Overzicht Service Bus AMQP]: service-bus-amqp-overview.md
[Azure portal]: https://portal.azure.com
